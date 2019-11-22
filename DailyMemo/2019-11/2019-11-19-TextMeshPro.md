<h4>・11/19</h4>

TextMeshPro の扱いについて検証・方針決め

Assets フォルダ直下に各種 3rd party が自由にフォルダ切ることは許容する方針

なので「TextMesh Pro」フォルダもそのままのパスで残す

ただし中 (特に Resources 以下は) の整理はしたい

http://westhillapps.blog.jp/archives/52523951.html
を参考にデフォルトフォントアセットを非実用的でいいので最軽量なものに変更、

「Fonts & Materials」内は上記フォントアセットのみに<br/>
「Shaders」内も使用するもの以外は削除、 (いったん全部コミット→削除コミット、としておけばいつでも復元できる…ハズ。。)<br/>
「Sprite Assets」と「Style Sheets」もとりあえず削除、<br/>

この方針に合わせて TMP Settings を変更してみよう
エラー発生したりしたら再考すればいいでしょう。。

http://gutenberg.osdn.jp/font/ja.html

フォントはとりあえず「GL-アンチックPlus」を使用させていただくことに

https://qiita.com/thorikawa/items/03b65b75fa9461b53efd

上記を参考に Font Asset を作成したら文字が入り切らなかった。。

スプライトを大きくするかフォントを小さくするか、、いろいろ検証中