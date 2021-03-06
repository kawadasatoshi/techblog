
## バックアップ

データを構築するために使用できるデータベースのデータのコピー


## 物理バックアップ

データファイル、制御ファイルなどの、データを構成する物理ファイルぐんのコピーを作ること

## 論理バックアップ

Oracle Data Pumpを使用して表やストアどプロシージャの定義、で＾たなどの論理データをバイナリファイルにエクスポートすること

後にそのバイナリファイルをデータベースにインポートしてリストアする


## Recovery Managerによる物理バックアップ

RMANと呼ばれる機能を使用してディスクまたはテープにコピーする

- 制御ファイル

- アーカイブREDOログファイル

- データファイル

- サーバーパラメータファイル

## データベースの「リストア」

ファイルが破損、または削除された場合に、バックアップ媒体を破損してしまったファイルの代わりに元の場所または新しい場所にコピーする作業。


## データベースの「リカバリ」

リストアしたデータファイルにREDOログファイルを適用することで、データファイルに反映されていない変更情報を反映していく作業。

REDOログファイルにはバックアップ以降、現在までの「トランザクション変更情報」が記録されている。

バックfアップからデータファイルをリストアすると、
その「バックアップファイル取得時点」までのデータが格納される。

リカバリ処理ではデータファイルに反映されていないトランザクションを、
「オンラインREDOログファイル」から適用し、
それが存在しない場合は「アーカイブREDOログファイル」から取得する


## バックアップのタイプ

「一貫性バックアップ」「非一貫性バックアップ」

## 一貫性バックアップ(オフラインバックアップ)

コミットされた全ての変更内容が
データファイルに書き込まれている状態で取得したバックアップ

「トランザクションの一貫性が保たれている状態」という意味。

一貫性バックアップはデータベースを正常にクローズしてインスタンスを停止してから行う。

これはデータベースを正常に停止する際に発生するチェックポイントによって
「コミット済みのトランザクションの変更内容」が全て、データファイルに書き込まれいているため。


## 非一貫性バックアップ(オンラインバックアップ）

データファイルに適用されていない変更がある状態で取得したバックアップ。

データベースをオープンした状態でも取得できる。

トランザクションの一貫性がデータファイルだけでは確保できない、
そのため、データファイルをリストアした後で、「REDOログファイル」を使用して
リカバリを実行する必要がある。


## リカバリ機能

「インスタンスリカバリ」「メディアリカバリ」


## インスタンスリカバリ

インスタンスリカバリは、インスタンス障害発生時点までにコミットされていたトランザクションの変更内容を適用するように、データベースをリカバリする。

インスタンスリカバリはインスタンス障害の発生後、
データベースを再起動した際にSMONによって自動的に実行される

ディスク障害によるファイルの損失やなどによって発生する。


1. 破損したファイルをバックアップからリストアする

1. オンラインREDOログファイルとアーカイブREDOログファイルを使用して
   「リスト明日データファイルに適応されていないトランザクションの変更情報」
   を適用する(「ロールフォワード」)

1. この時点のデータファイルには

1. コミット済みのトランザクションと

1. コミットされていない(未コミットの)トランザクションのデータが混在している(未コミットのトランザクションデータがある場合はUNDOデータも生成されている)

1. UNDOデータを使用して、未コミットのトランザクションが「ロールバック」される

1. データファイルがリカバリされた状態になる


## メディアリカバリの二種類

メディア障害からのリカバリ

## 完全リカバリ

バックアップ時点からREDOログファイルの全ての情報をデータファイルに反映することで
データベースを「障害発生直前のコミットの状態」にまでもどす。

## Point-inTimeリカバリ

発生時よりも前に作成された全てのデータファイルを含む、データベース全体のバックアップ(ターゲット時刻時点の)をリストアする。

これをターゲット時刻までの全てのアーカイブREDOログファイルに格納されている変更をデータファイルに適応することで、
「ターゲットの時刻の状態に戻す」

過去の任意の時点に戻すことができるが、ターゲット時刻から現時点までのデータは反映されない

## フラッシュバック機能

バックアップのリストやPoint-inTimeを実行することなく巻き戻しができる。

ユーザーエラーにも対応可能



## バックアップ及びリカバリを自動管理するためのデータベース構成

「高速リカバリ領域」が用意されている。

バックアップファイルがOracleデータベースによって自動的に管理される。

1. バックアップように高速リカバリ領域を構成する

1. 高速リカバリ領域をアーカイブREDOログの保存先にする

1. オンラインバックアップを実行できるように、データベースをARCHIVELOGモードで運用する


## 高速リカバリ領域の構成

データファイル郡が格納されているディスクとは別のディスクに
準備する(同時に壊れるリスクを防ぐ)

高速リカバリ領域ないのファイルは、Oracleデータベースによって自動的に管理される。

保存方針に基づいて不要になったファイルは領域が足りなくなると自動的に削除される

以下の全ファイルが格納できるサイズを検討する

- データファイルの完全バックアップを二つ

- リカバリしたい期間内の任意の時点のデータベースをリストアするために必要な増分バックアップ

- アーカイブREDOログファイル


## 高速リカバリ領域の場所

高速リカバリ領域の場所は「DB_RECOVERY_FILE_DEST」

高速リカバリ領域のサイズは「DB_RECOVERY_FILE_DEST_SIZE」


## データベースの稼働モード

## NOARCHIVELOGモード

- 満杯になったREDOロググループを「アーカイブしない」
  書き込みが蓄積してきたら再利用(上書き)される

- インスタンス障害からは保護される
  メディア障害からは保護されない

- 実行可能なバックアップは一貫性バックアップのみ

- 実行可能なリカバリは「インスタンスリカバリ」「一貫性バックアップをリストアした時のリカバリ」


## ARCHIVELOGモード

- 満杯になったREDOロググループをアーカイブして、
  「アーカイブREDOログぐファイルとして保存する」
  REDOログファイルが循環してきたら再利用する

- インスタンス障害とメディア障害の両方から守られる


- 一貫性バックアップ及び非一貫性バックアップが可能

- インスタンスリカバリ、メディアリカバリが可能


## デフォルトからのモード設定変更

デフォルトは「nOARCHIVELOGモード」である。

高速リカバリ領域を構成してデータベースをARCHIVELOGモードに変更するには次の手順が必要。


1. クライアントから接続する(SYSBACKUP権限が必要)

<pre><code>
rman target "'ユーザー名/パスワード as sysbackup'"
</code></pre>

例)

<pre><code>
> . oraenv 
> rman target "' / as sysbackup'" --OS認証で接続

> > SHOW PARAMETER db_recovery --高速リカバリ確認
</code></pre>


2. 次のコマンドでリカバリ

<pre><code>
ALTER SYSTEM SET DB_RECOVERY_FILE_DEST = 'ディレクトリ名'
ALTER SYSTEM SET DB_RECOVERY_FILE_DEST = 'サイズ'
</code></pre>


3. 次の一連の操作でAECHIVELOGモードにする

<pre><code>
1. SHOUTDOWN IMMEDIATEコマンドでデータベースを停止
2. データベースをバックアップ
3. STARTUP MOUNT コマンドを使用してマウントモード
4. データベースをARCHIVELOGモードに変更
5. データベースのオープン
</code></pre>




## バックアップの作成と管理

バックアップには二種類ある

- イメージコピー

- バックアップセット

## イメージコピー

「OSレベル」でのデータファイル、制御ファイル、アーカイブREDOログファイルなどのコピーのこと

OSコマンドでも作成できるが,RMANを使用して作成するとイメージコピーが「RMANリポジトリ」に記録される

データファイルに対してそのコピーが作られる


## バックアップセット

RMANの「BACKUPコマンド」によって作成されるRMAN固有のバックアップ

「バックアップピース」という物理ファイルが一つ以上含まれ、
一つのバックアップピースにつき、一つ以上のデータベースファイルのバックアップがRMAN固有のコンパクトな形式で格納される。

未使用のブロックが圧縮されるためデータファイルのバックアップに使用される領域を節約できる。




## バックアップ対象による分類

- データファイルの全体バックアップ

- データファイルの増分バックアップ

- 全体バックアップ

## データファイルの全体バックアップ

データファイルの全ての使用ずみブロックが含まれるバックアップ

イメージコピーorバックアップセットどちらでも可能


## データファイルの増分バックアップ

最初に「レベル0の増分バックアップ」
を取得し、「レベル1のバックアップ」を取得する。
レベル1はレベル0以降の増分バックアップを取得する。


## 全体バックアップ

全てのデータファイルだけでなく、制御ファイルやアーカイブREDOファイル、サーバーパラメータファイルが含まれる。
イメージコピーorバックアップセットどちらでも可能




## バックアップ設定の構成

RMANプロンプトで「SHOW ALL」コマンドの実行で確認可能

## ディスク設定

バックアップをディスクに格納する設定は次のコマンド

<pre><code>
CONFIGURE DEFAULT DEVICE TYPE TO DISK
</code></pre>

## 自動バックアップの設定

データファイルのバックアップ時に同時にそのほかのファイルもバックアップするコマンド

<pre><code>
CONFIGURE CONTROLFILE AUTOBACKUP ON;
</code></pre>


## 保存方針の設定

RMANの使用でバックアップの必要/不要を識別するための「保存方針」を設定できる。

保存方針は「リカバリ期間」または「冗長性」のいづれかを基準に設置できる

<pre><code>
CONFIGURE RETENTION POLICY TO RECOVERY WINDOW OF 7 DAYS
</code></pre>

全体バックアップか増分バックアップ及び制御ファイルのバックアップ数を指定できる

二世代分のバックアップを保存する場合は以下のコマンド

<pre><code>
CONFIGURE RETENTION POLICY TO REDUNDANCY 2
</code></pre>


## データベース全体のバックアップ

RMANを使用して全体のバックアップを作成できる


- rmanを使用してターゲットデータベースに接続

<pre><code>
rman target "' / as sysbackup'"
</code></pre>

- データベース全体のバックアップを作成

<pre><code>
backup database plus archivelog;
</code></pre>


## 推奨バックアップ計画の使用

Oracleの「推奨バックアップ計画」を使用すると、
「増分更新バックアップ機能」を利用した自動バックアップ計画を実装する、効率的なバックアップとリカバリが可能。


データファイルのイメージコピーを「レベル1の増分バックアップ」でロールフォワードして、新たなイメージコピーを作成する機能。

イメージコピーの内容は「最後のレベル１の増分バックアップ時点で取得したデータファイルの全体バックアップ」

毎日増分バックアップを取得している場合は、メディアリカバリのために一日を超えるREDOログを適用する必要はなくなる。



## 推奨バックアップ計画を使用したバックアップの作成

- 計画の一日目

データファイルの完全なイメージコピーバックアップを作成する

- 計画の二日目

データファイルのレベル1の増分バックアップを作成する


- 計画の三日目

二日目に作成したレベル1の増分バックアップを完全なイメージコピーバックアップに適応する

これによって二日目のレベル1増分バックアップが作成あsれた時点にロールフォワードされる

このイメージコピーをリストアすると
最後に適用されたレベル1の増分バックアップの
SCNで作成されたデータファイルのバックアップをリストアした場合と同じ結果になる。



## バックアップの管理

バックアップのステータス

RMANリポジトリはデータベースの「制御ファイル内部」に保存されている。

- AVAILABLE

使用可能。RMANリポジトリに記録されているバックアップが、ディスクまたはテープに存在し、使用できることを意味する。

- EXPIRED

期限ぎれ。RMANリポジトリに記録されているバックアップだが、ディスクやテープに存在しない


- UNAVAILABLE

使用不可能。外部のディスクにバックアップが存在するので、一時的に使用できないことを意味する。


## RMANリポジトリに格納されている情報の確認

RMANリポジトリに格納されている情報の確認には、
LIST BACKUPコマンド、LIST COPYコマンドを実行してバックアップを指定する


## バックアップのメンテナンスタスクの実行

- クロスチェック

「物理的なバックアップ」「RMANリポジトリないにあるバックアップレコード」が一致するかを確認すること。

「CROSSCHECK BACKUP」コマンドを実行すると全てのバックアップセットがクロスチェックされる


破損がない場合は「AVAILABLE」指定した位置にない場合は「EXPIRED」


- 期限ぎれバックアップの削除

「DELETE EXPIRED BACKUP」コマンドで「EXPIRED」になったバックアップは全て削除される

- 不要なバックアップの削除

「DELETE OBSOLETE」コマンドで全ての不要バックアップが削除される。
構成済みの保存方針にはしたがっている。

- ステータスを「UNAVAILABLE」にする

RMANで「CHANGE」から「UNAVAILABLE」コマンドを実行すると
ステータスが「UNAVAILABLE」に変更される。
この状態では外部のサイトにバックアップを移動した場合でもバックアップの情報をRMANリポジトリに保持することができる。

再度使用可能になった時は再度ステータスを「AVAILABLE」に帰れば良い。



- OSコマンドで取得したバックアップをカタログ化する

リポジトリに黒くされていないバックアップや、OSコマンドによって取得されたバックアップは、
その状態のままではRMANによるリカバリ操作では利用できない。

これらのバックアップを利用するにはRMANのCATALOGコマンドで
RMANリポジトリにカタログ化(登録)する必要がある。



# リカバリの実行

## Oracle社推奨のリカバリ

「データリカバリアドバイザ」というツールを使用する方法が推奨されている

これはデータ障害を自動的に診断した上で適切な修復オプションを表示する機能。

破損したデータに対するデータベース操作でエラーが発生すると、「データ整合性チェック」が自動的に開始され、
エラーに関する障害が調査される。

これが発生するとADR(自動診断リポジトリ)に記録される。



## 実際の動き


「LIST FAILURE」コマンドを実行して、データリカバリアドバイザが認識できる全ての障害の説明を表示して確認。

「ADVISE FAILURE」コマンドで自動及び手動の修復オプションを表示する

「REPAIR FAILURE」コマンドを実行して、直近のADVISE FAILUREコマンドによってリストされた障害の自動オプションを実行し、
自動的に修復します。



## フラッシュバック表

人為的なエラーについての対応は「フラッシュバック機能」を使用することで短時間でリカバリできる


## フラッシュバック表

フラッシュバック表は、バックアップをリストアすることなく、表を過去のある時点までリカバリすることができる機能。

論理的なデータ破損からリカバリが可能。

- 「バックアップをリストアしない」のでほかのデータベースオブジェクトにはなんの影響も与えない

- データベースを「オープン状態のまま」で処理を実行できる

- フラッシュバック表に使用するデータは「UNDO表領域」から取り出される

- データベース管理者に依存することはなく、ユーザー自身が自分でフラッシュバック表の機能を実行できる


## 必要な権限

- 種類:システム権限

FLASHBACK TABLE or FLASHBACK ANY TABLE

- オブジェクト権限

SELECT, INSERT, DELETE, ALTER

## 事前準備

事前に次のSQLを実行して表に対する行移動を有効にする必要がある。

<pre><code>
ALTER TABLE スキーマ名.表名 ENABLE ROW MOVEMENT;
</code></pre>


## 具体的な実行例

> SHAIN表の3000版のYoshino
> さんをDELETE,COMMMITしてしまった。

<pre><code>
> SELECT * FROM SHAIN
6行が選択されました

> SELECT systimstamp FROM dual;
> 14-11-06 15:40:22

> DELETE FROM SHAIN WHERE shain_no = 3000;
> COMMIT;

> SELECT * FROM SHAIN
5行が選択されました

</code></pre>

## フラッシュバックの実行例


<pre><code>
> ALTER TABLE SHAIN ENABLE ROW MOVEMENT

> FLASHBACK TABLE SHAIN
> TO TIMESTAMP TO_TIMESTAMP('14-11-06 15:40:22', 'YYY-MM-DD HH24:MI:SS')

> SELECT * FROM SHAIN
6行が選択されました
</code></pre>


## フラッシュバックドロップ

表の削除処理を向こうにする機能

> shain表をDROP TABLEしてしまった


## フラッシュドロップの実行

<pre><code>
> SHOW RECYCLEBIN
> FLASHBACK TABLE SHAIN TO BEFORE DROP
</code></pre>


## 注意点

- 索引や制約について

表に定義されている「制約」や「索引、トリガー」は表の依存オブジェクトである。
フラッシュバックドロップでゴミ箱から取り出せれるが、元の名前には戻らない。


- PURGE

PURGE句を指定すると完全に削除されてしまうのでリカバリできない。




title:Oracleが提供してくれるバックアップ/リカバリ/リストア機能一覧

description:物理的な破損からの回復だけでなく、ユーザーの誤操作からの復旧の方法をまとめました。

category_script:True














