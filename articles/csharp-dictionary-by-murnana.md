---
title: "[C#] 辞書には色んなクラスがある"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["csharp"]
published: true
---

C#には、何かをキー値として保持するための便利なクラス `Dictionary<TKey, TValue>` があります。

これはキーと値のペアを管理するコレクションで、配列や `List` のようにインデックス番号でなく、任意のキーを使って素早く値を取り出せるのが特徴です。

例えば大量のアイテムをIdで素早く検索したい、というときに使われます。
```cs
using System.Collections.Generic;

//...

Dictionary<long, ItemData> m_Indexes = new();

//...

public ItemData? GetById(long id) {
    if(m_Indexes.TryGetValue(id, out var masterData)) {
        return masterData;
    }

    return null;
}

```

実はC#には、このクラス以外にも2つ辞書があります。


## 並列処理でも整合性を保つ `ConcurrentDictionary<TKey,TValue>`

`Dictionary<TKey, TValue>` はスレッドセーフ（複数のスレッドから同時にアクセスしても正しく動作すること）ではありません。
これは、同時に書き込まれると何かが起きても仕方ないよ、ということでもあります。

例えば複数のスレッドが同時に `Dictionary` に書き込むと、データが壊れたり、例外が発生したりする可能性があります。

そこで、代わりに `ConcurrentDictionary<TKey,TValue>` を使います。

```cs
using System.Collections.Concurrent;

//...

ConcurrentDictionary<long, ItemData> m_Indexes = new();

//...

// スレッドセーフに追加
m_Indexes.TryAdd(item.Id, item);

// スレッドセーフに取得
if(m_Indexes.TryGetValue(id, out var itemData)) {
    return itemData;
}

// 存在すれば更新、なければ追加
m_Indexes.AddOrUpdate(item.Id, item, (key, old) => item);
```



## キー値が弱参照になる `ConditionalWeakTable<TKey,TValue>`

キー値を弱参照（GC＝ガベージコレクター（不要なメモリを自動解放する仕組み）による回収を妨げない参照）として保持します。

弱参照ということは、キー値の参照がGCによりメモリから削除されると、その値も消えるということです。

### キー値の比較について

キー値の比較は通常の辞書とは異なり、 **参照の同一性のみを比較** します（`ReferenceEquals` に相当します）。値が等しいかどうかでは比較しません。

別のインスタンスであれば、例え同じ値が入っていても別物として扱われます。

具体的には、 `Object.GetHashCode` を使ったハッシュ値による比較は行わず、参照が同じオブジェクトかどうかだけで判断します。

### 使い方

クラスを改変せずに、外部からインスタンスに追加情報を付与したいときに使います。

例えばデバッグ用として、識別子となる文字列を登録しておいて、後でログ出力する際はこの文字列を使う、といった使い方になります。

```cs
using System.Runtime.CompilerServices;

//...

ConditionalWeakTable<ItemUserData,string> m_DebugItemAddReason;

//...

// アイテム生成
var item = new ItemUserData();

// デバッグ用: どこでアイテムを作ったかいれる
m_DebugItemAddReason.Add(item, "In craft table");

//...

// ログ出力したい
if(m_DebugItemAddReason.TryGetValue(item, out var reason)) {
    Console.WriteLine($"Reason: {reason}");
}
```


## 参考
- [Dictionary<TKey,TValue> Class (System.Collections.Generic) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=netstandard-2.1)
- [ConcurrentDictionary<TKey,TValue> Class (System.Collections.Concurrent) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentdictionary-2?view=netstandard-2.1)
- [Add and Remove Items from a ConcurrentDictionary - .NET | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/standard/collections/thread-safe/how-to-add-and-remove-items)
- [ConditionalWeakTable<TKey,TValue> Class (System.Runtime.CompilerServices) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.conditionalweaktable-2?view=netstandard-2.1)
