---
title: "[Git] リポジトリで改行コードを正規化する"
emoji: "📄"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["git"]
published: true
published_at: 2022-08-11 20:00
---

# 方法

1. ワーキングツリー上のルートディレクトリでテキストファイルを生成します。ファイル名は _.gitattributes_ です。
2. _pattern_ を使用してファイルを指定します。改行コードは _LF_, _CRLF_ から選択します。
```:.gitattributes
# チェックイン時、LFに正規化します。
# チェックアウトの際、常に改行コードをLFにします
*.json text eol=lf

# チェックインの際、LFに正規化します
# チェックアウトの際、常に改行コードをCRLFにします
*.bat text eol=crlf
```
3. 作成した _.gitattributes_ ファイルをコミットします
```bash
git add .gitattributes
git commit
```

4. 過去にコミットしたファイルも正規化するため、過去ファイルも再度追加、コミットします
```bash
git add --renormalize .
git commit
```


# 説明
Gitでは`git switch`、`git checkout`のような、リポジトリからワーキングツリーにコピーする時と`git add`、`git commit`のように、ワーキングツリーからリポジトリに保存する時、テキストファイルの改行コード (EOL: end-of-line)を正規化する挙動があります。

_.gitattributes_ ファイルで指定しない場合、通常はワーキングツリーの環境にあるGitの設定、_git-config_ に依存します。_git-config_ についての詳細はここでは省きますが、環境によってテキストファイルの改行コードがそろわなくなります。リポジトリのクローンを行った時点で、ある環境ではLF、ある環境ではCRLFになります。

_.gitattributes_ ファイルはテキストファイルです。このファイルは通常、リポジトリのルートディレクトリに配置され、リポジトリ内のファイルについての設定が記入されます。テキストファイルの改行コードも、その設定の1つです。

ファイル指定には _pattern_ を使用します。 _pattern_ のフォーマットは否定(`!`)と末尾のスラッシュ以外は _.gitignore_ ファイルと同じです。以下は[公式ドキュメントの.gitignoreの情報](https://git-scm.com/docs/gitignore#_pattern_format)から抜粋したものです。
- `#` で始まる行はコメントになります
- 先頭で `#` を使用したパスを使用する場合、バックスラッシュを置く必要があります (`\#`)
- 末尾の空白はバックスラッシュ `\` を前に置かない限り無視されます
- スラッシュ `/` はディレクトリの境界として使用されます。
- アスタリスク `*` はスラッシュ以外のすべてに一致します。 
- クエスチョンマーク `?` はスラッシュ以外の任意の1文字に一致します
- 範囲表記、たとえば`[a-zA-Z]`を使用して、範囲内の文字の中からある1文字に一致させることができます。
- アスタリスク2つ `**` は特別な場合があります
  - 先頭に `**/` を書くと、すべてのディレクトリで一致することを意味します
  - 末尾に `/**` と書くと、内部のすべてに一致します
  - スラッシュで挟んだ場合 `/**/` 、スラッシュ前後のパターンに一致し、間はさまざまになります

この設定は _.gitattributes_ が作成されたときに、新規追加されたテキストファイルにのみ適用されます。すでに追加されているテキストファイルには効果がありません。

過去に追加したテキストファイルにも改行コードの正規化を行う場合、過去に追加したテキストファイルを再度コミットする必要があります (`git add --renormalize .`)

# 参考
- [Git - gitattributes Documentation](https://git-scm.com/docs/gitattributes#_eol)
- [行終端を処理するようGitを設定する - GitHub Docs](https://docs.github.com/ja/get-started/getting-started-with-git/configuring-git-to-handle-line-endings)
- [.gitattributesで改行コードの扱いを制御する - Qiita](https://qiita.com/nacam403/items/23511637335fc221bba2)
- [もう迷わない .gitattributes で改行コードを統一 - Qiita](https://qiita.com/Yossy_Hal/items/6fe2d14cddd6e16796d7)
