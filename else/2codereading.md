

## 結論

- 記事を書く理由
- コードは賢く読みたい
- コードに対する信頼と前提
- 


## 記事を書いている理由

最近こんなプロジェクトに参加している

<a href="/career/3001oo4o.html">
プログラミング
</a>

社内の全ての業務ソフトウェアのDB接続方式をOO4O形式からODP.NET形式に変更するプロジェクトに障害・品質担当として参加

手順としては以下の通り

<pre><code>
各ソフトの刷新前に仕様を確認。同時に画面のキャプチャーを取得し、変更前の状態を保存。テスト項目についても作成。
その後、参照設定とコードを変更しDB接続方式をOO4Oミドルウェアを利用する方式からODP.NETを利用する方式に変更。
刷新前後でソフトウェアの品質に変更がないかどうかを確認し、問題がなければテストを通過。
以上の流れを5人で1200ソフトウェア分行う。
</code></pre>

もっとも大変な部分はここ


### 「各ソフトの刷新前に仕様を確認」

各ソフトの仕様はバラバラであるため、一から動かし方を調べなければいけない。

特にテスト環境のDBが存在しないこともあり、その場合は環境を一から用意しなければならない。

これがどれだけ骨の折れる作業であるか。

懸命な読者ならお分かりだろう。

おかげで私は3ヶ月で視力が半分に落ちた...


## コードを賢く読むには

## 目的を持つこと

目的から逆算してコードを読むことが第一。

これがない状態で漠然と読むと大変なことになる。

## 時間を決めて読むこと

1時間,2時間など時間を決めて読まなければ永遠に費やしてしまう。

ある程度努力してできなければ諦めて別の手段を考えることも大事。













title:コードの読み方

description:

img:https://wid.jp/wp-content/uploads/2017/05/%E3%82%BD%E3%83%BC%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%88%E3%82%99.png

category_script:page_name.startswith("100")























