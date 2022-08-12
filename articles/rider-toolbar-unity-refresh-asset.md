---
title: "[Unity] Riderのツールバーに「Refresh Unity Assets」を追加する"
emoji: "🔄"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "rider"]
published: true
published_at: 2022-08-12 20:00
---

# 手順

1. ツールバーにある「Unity:」と書かれている辺りを右クリックします
2. ポップアップで出現する「Customize Toolbar...」をクリックします<br/>![ツールバーの「Unity」を右クリックした後](/images/rider-toolbar-unity-refresh-asset/2-show-customize-toolbar.gif)*ツールバーの「Unity」を右クリックした後に表示される「Customize Toolbar...」*
3. 挿入したい階層をクリックします<br />![Customize Navigation Bar Toolbar ウィンドウ](/images/rider-toolbar-unity-refresh-asset/3-select-hierarchy.png)*Customize Navigation Bar Toolbar ウィンドウ*
4. ウィンドウの「＋」マークをクリックします
5. 「Add Action...」を選択します<br />![「+」をクリックした後、「Add Action...」が表示される](/images/rider-toolbar-unity-refresh-asset/5-add-action.png)*「+」をクリックした後、「Add Action...」が表示される*
6. 検索窓に「Refresh Unity Assets」と入力します
7. 「Refresh Unity Assets」をクリックし、「OK」を押します <br />![「Refresh Unity Assets」を見つける](/images/rider-toolbar-unity-refresh-asset/7-find-refresh-unity-assets.png)*「Refresh Unity Assets」を見つける*
8. Customize Navigation Bar Toolbarウィンドウに戻るので、「Apply」をクリックします
9. ツールバーに、Unityアセットのリフレッシュアイコンが追加されます<br />![非アクティブ状態の「Refresh Unity Assets」](/images/rider-toolbar-unity-refresh-asset/9-show-icon.gif)*非アクティブ状態の「Refresh Unity Assets」*


# 説明
Riderで編集したC#ソースコードは基本、自動的にリロードされますが、この設定は切ることができます。切ってしまうと手動でリフレッシュをかけない限り、Unityがスクリプトを使うことはありません。

そこで便利なのが「Refresh Unity Assets」をRiderから1ボタンで実行することです。これによりUnityにリロードさせ、コンパイルエラー等を拾いやすくなります。

しかしRider 2022.2から、デフォルトで表示されていた「Refresh Unity Assets」のボタンがなくなっていました。そのため必要な場合は自分でカスタマイズする必要があります。


# 参考
- [Rider 2022.2 - "Refresh Unity Assets" button is missing from the toolbar : RIDER-80725](https://youtrack.jetbrains.com/issue/RIDER-80725)
- [Refresh Unity Assets | JetBrains Rider](https://www.jetbrains.com/help/rider/Refreshing_Unity_Assets.html)