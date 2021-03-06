

## サービスベースのアーキテクチャスタイル

サービスベースのアーキテクチャの基本的なトポロジは、

- 個別に展開されたユーザーインターフェイス、

- 個別に展開されたリモートの粗粒度サービス、

- およびモノリシックデータベース

で構成される分散マクロ階層構造に従います。

ちなみに、「粗粒度サービス」は大抵の場合、レイヤードアーキテクチャによって構成される。

<img src="https://camo.githubusercontent.com/6a3bb5e7744ff17abe176ac6b7f4e9ea5727635e155a759d7be7faa97ef6dc0f/68747470733a2f2f6c6561726e696e672e6f7265696c6c792e636f6d2f6c6962726172792f766965772f66756e64616d656e74616c732d6f662d736f6674776172652f393738313439323034333434372f6173736574732f666f73615f313330312e706e67">


このアーキテクチャスタイル内のサービスは、独立して個別に展開される「アプリケーションの一部」です。

通常、RESTがユーザーインターフェイスからサービスにアクセスするために使用されます


## サンプル例

ユーザーインターフェースの数、データベースの数、サービスの数は簡単に変化する

### ユーザーインターフェースの数が変化するパターン

<img src="https://camo.githubusercontent.com/d453f04a35c4096e651f5e52891c1f71c92ca8d59b916504986a871fff9fa2bb/68747470733a2f2f6c6561726e696e672e6f7265696c6c792e636f6d2f6c6962726172792f766965772f66756e64616d656e74616c732d6f662d736f6674776172652f393738313439323034333434372f6173736574732f666f73615f313330322e706e67">




### データベースが複数存在するパターン

<img src="https://camo.githubusercontent.com/9488fa07f6f2a3c2909b603fa575051d9f8b4cd55776dafa776bf0005a3562d2/68747470733a2f2f6c6561726e696e672e6f7265696c6c792e636f6d2f6c6962726172792f766965772f66756e64616d656e74616c732d6f662d736f6674776172652f393738313439323034333434372f6173736574732f666f73615f313330332e706e67">

単一のモノリシックデータベースを個別のデータベースに分割する機会が存在する可能性があります。各ドメインサービスに一致するドメインスコープのデータベース（マイクロサービスと同様）にまで及ぶ場合もあります。


### リバースプロキシ

次のように、ユーザーインターフェイスとサービスの間にリバースプロキシまたはゲートウェイで構成される構成を追加することもできます。

これは

- ドメインサービス機能を外部システムに公開する場合、

- 共有の横断的関心事を統合してユーザーインターフェイスの外部に移動する場合

に適応する

<img src="https://camo.githubusercontent.com/bb0e5ed68554c83a4c152d2c9d640e9150931170907bbe4a0ff6850d446325bc/68747470733a2f2f6c6561726e696e672e6f7265696c6c792e636f6d2f6c6962726172792f766965772f66756e64616d656e74616c732d6f662d736f6674776172652f393738313439323034333434372f6173736574732f666f73615f313330342e706e67">

ユーザーインターフェイスとドメインサービスの間にAPIレイヤーを追加する



## データベースの変更

ここまで紹介した通り、ほとんどの場合でデータベースの数は一つか、多くて二つ程度である。

少ないデータベースは、コミットとロールバックを含む通常の（アトミック性、一貫性、分離、耐久性）データベーストランザクションを使用して、
単一のドメインサービス内のデータベースの整合性を確保します。

一貫性を保つことができるのが少ないデータベースのメリットである。

しかしこの場合、**データベースの変更は全てのサービスへの影響を意味する**


このデータベース変更の影響とリスクを軽減する1つの方法は、データベースを論理的にパーティション化し、フェデレーション共有ライブラリを介して論理的なパーティション化を明示することです。

<img src="https://camo.githubusercontent.com/082f8e6a779f6fa2ea8175ad94a6fbc4000b7a021b9f851fb58f5417db9866f9/68747470733a2f2f6c6561726e696e672e6f7265696c6c792e636f6d2f6c6962726172792f766965772f66756e64616d656e74616c732d6f662d736f6674776172652f393738313439323034333434372f6173736574732f666f73615f313330372e706e67">



## メリットとデメリット

このアーキテクチャは「ドメイン管理方式」である。（技術管理方式ではない）

したがってあるサービスの改修が入った場合、その他の改修を行う必要はなく、関係のないコンポーネントのビルドも防ぐことができる。

サービス単位でテストを組むこともできるため、ソフトウェアの品質を高く保つことができる。

まとめると以下の通り

- サービスごとに分割されているため、迅速な変更が可能

- サービスには範囲が区切られているため、テストも対象のサービス内部に抑えられる。

- より少ないリスクでデプロイできる。

デメリットは「コストが高くなる可能性がある」こと

小規模なアプリケーションでは費用対効果が出ないこともある。


## まとめ

このアーキテクチャはほとんどが星4で評価されている。



<img src="https://camo.githubusercontent.com/6428bd5a21cb28c9f5c6b14beee4a43dc10b7fa5b7e561e29ee656417e9ef752/68747470733a2f2f6c6561726e696e672e6f7265696c6c792e636f6d2f6c6962726172792f766965772f66756e64616d656e74616c732d6f662d736f6674776172652f393738313439323034333434372f6173736574732f666f73615f313330392e706e67">



## 備考

title:サービスベースアーキテクチャのメリットとデメリット

description:このアーキテクチャは「ドメイン管理方式」である。（技術管理方式ではない）したがってあるサービスの改修が入った場合、その他の改修を行う必要はなく、関係のないコンポーネントのビルドも防ぐことができる。サービス単位でテストを組むこともできるため、ソフトウェアの品質を高く保つことができる。

img:https://camo.githubusercontent.com/6a3bb5e7744ff17abe176ac6b7f4e9ea5727635e155a759d7be7faa97ef6dc0f/68747470733a2f2f6c6561726e696e672e6f7265696c6c792e636f6d2f6c6962726172792f766965772f66756e64616d656e74616c732d6f662d736f6674776172652f393738313439323034333434372f6173736574732f666f73615f313330312e706e67

category_script:page_name.startswith("2")




