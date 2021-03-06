

# サービスの概要

2000アクセス/dayを記録したwebアプリケーションを作成しました。
-

twitterの会話の履歴をもとに人脈相関図を作成するツールです。
(url:https://fanstatic.short-tips.info/twitter_network/@nominz)

ユーザーはsns上の人間関係を見える化することで、これまで見えなかった人脈ネットワークの近隣のアカウントとコンタクトを図れるようにアカウントの運営を改善できます。


## アプリの特徴

アプリを構成するのは「アカウント:ノード」と「繋がり:リンク」の二種類のみです。
- ノードの端からさらに新しいリンクを作ることもできます。
- 会話を検知するとアカウントがビーコンのように発火します。
- 会話の内容は右下に出てくるタイムチャートから目視できます。


## どんな社会を目指しているのか

このアプリケーションが目指すのは「孤独感/虚無感の撲滅」です。
コロナが蔓延した現在、リモートワークや自粛が増えおうち時間が長くなりました。
それにより人と人とのリアルな繋がりが減り、どうしようもない閉塞感が増してるように感じます。

そこで私は、せめてもSNS上の無機質な孤独感を無くしたいと考えてます。
私が思うにSNS上での虚無感は1on1のコミュニケーションが連鎖している方式にあると思います。
1on1ではなく多数on多数のコミュニケーションを作り、一つのクラスターを作り、アットホームな環境を作りたいのです。





# 技術の概要

1. AWS,EC2 Ubuntu上に0から環境を構築。
1. 上記二つを繋ぐためにGit,Githubを使用。
1. その後、改修、拡張、本番環境と開発環境の移行のしやすさの観点からdockerを導入
1. google analytics,google seach consoleを用いて数値分析,seo対策を行い、検索順位を60位から5位以内に改善。


# サービスの運営上での課題と工夫



## Dockerによる開発環境構築

・課題
自前のPCはMacであるがEC2上にはUbuntuで管理しているため、開発環境と本番環境に環境面で差異が出てしまう。

・使用技術
dockerとdocker-composeを使用。独学で書籍からdockerの知識を会得し,ymlファイル,dockerfileを0から作成した。

・工夫
副次的効果として1プロセス1コンテナの原則に基づいた設計をすることでサービスが肥大化しても対応できる構築にできた。

・成果
開発環境と本番環境のPC環境の面での差異をなくし、安定した品質に繋げることができた。


## Git,Githubによる安全な管理と素早いリリース

・課題
高速な開発サイクルにより需要や運営の状態を見ながらサービスを柔軟に変更できるのが個人開発の強みだが、EC2上にあるサービスを直接操作するとソースの保守のリスクやリリースまでのサイクルに影響が出る。

・使用技術
Git,Githubを利用したソースの管理へ切り替える。

・工夫
「git add」から「git push~」までを入力するのが手間なのでそれらを一括にしたシェルを作成し
合わせて本番にリリースする際のコマンドもまとめ、こまめなコミット,プル,リリースができる体制をとった。

・成果
改修からリリースまでのスパンを短く(平均15分の短縮)できた。
また、開発期間中にPCが故障し開発環境へアクセスできなくなるアクシデントが発生したが、
Githubへ保存していたためソースを失わずに済んだ。


## 開発の全体像

利用言語：Javascript,Python3,Nginx
ライブラリ：D3.js 
フレームワーク：Django
その他：Lets'Encrypt, Docker, Docker-compose, Git, Github, EC2


＜内容＞
・twitterの会話の履歴をもとに人脈相関図を作成するwebアプリケーション
・ローカルに開発環境を、AWS,EC2 ubuntu上に0から環境を構築
・上記二つを繋ぐためにGit,Githubを導入
・改修、拡張、本番環境と開発環境の移行のしやすさの観点からdocker-composeを選択
・Let's Encryptを用いたhttps対応
・twitterのデータはtwitter apiを利用して取得する
・データベースはgithub上で管理


## これまでのプロジェクトの流れ

1,AWS,EC2を購入
2,docker-composeを導入し、仮のwebアプリケーションで実装が可能か検証
3,ローカルでの開発環境にdocker-composeを使ったdjangoアプリケーションを作成
3,visual studioのremote機能を用いてEC2上でも動くかどうかを確認
4,ローカル開発環境とEC2開発環境を繋ぐGithub,Githubを導入
5,Route53を用いてドメイン取得

上記の手順で本格的な開発サイクルがスタート

  1,ローカルの開発環境で開発を行う
  2,gitを用いてadd commit pushを行う
  3,EC2からpullを行う
  4.本番環境の動作が問題ないかを確認

上記のサイクルを用いて、以下の4点を実装
・D3.jsを用いた人脈図の可視化
・Let's Encryptを用いたhttps対応
・github上へのデータ保存
・html css改良によるUIUXの改善
・レスポンシブ化




title:2000アクセス/dayを記録したwebアプリケーションを0から構築した話【履歴書】

description:ローカルに開発環境を、AWS,EC2 ubuntu上に0から環境を構築し、上記二つを繋ぐためにGit,Githubを導入。改修、拡張、本番環境と開発環境の移行のしやすさの観点からdocker-composeを導入し、

img:https://fanstatic.short-tips.info/static/fanstatic/sample.png
