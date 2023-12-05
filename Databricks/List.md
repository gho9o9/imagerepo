Paste Image を利用。
Ctrl + Alt + v でクリップボードの画像をペースト。
https://raw.githubusercontent.com/gho9o9/imagerepo/main/<カテゴリ>/images/*.png
![](https://raw.githubusercontent.com/gho9o9/imagerepo/main/Databricks/images/<hoge>.png)

<p  style="position:relative;width: 1000px;height: 450px;overflow: hidden;">
<img style="margin-top:25px;" src="https://sajpstorage.blob.core.windows.net/itagaki/Optimize.png" width="1000">  
</br>
<img style="margin-top:25px;" src="https://raw.githubusercontent.com/gho9o9/imagerepo/main/Databricks/images/o9o9_2023-08-17-08-40-58.png" width="1000">  
</p>
</div>

![](images/SynapseTechBook_2023-02-02-15-17-41.png)
![](images/SynapseTechBook_2023-02-02-15-30-12.png)
![](images/o9o9_2023-03-05-10-32-40.png)
![](images/o9o9_2023-03-05-10-41-13.png)
![](images/o9o9_2023-03-05-10-40-52.png)    
![](images/o9o9_2023-03-06-10-55-42.png)
![](images/o9o9_2023-03-06-10-59-46.png)
![](images/o9o9_2023-03-07-09-27-50.png)
![](images/o9o9_2023-04-10-11-20-07.png)
![](images/o9o9_2023-04-10-11-20-50.png)  
![](images/o9o9_2023-07-10-16-22-20.png)  
![](images/o9o9_2023-07-10-16-23-42.png)  
![](images/o9o9_2023-07-10-16-24-44.png)  
![](images/o9o9_2023-07-10-16-28-16.png)  
![](images/o9o9_2023-07-10-16-28-37.png)  
![](images/o9o9_2023-08-17-15-48-53.png)  

# Delta Sharing
## TL;DR
- 組織内外にセキュアにデータ共有する機能（インプレース ＆ R/O 共有）。  
- Databricks間共有とオープン共有があり、共有先の環境を限定しない。  
![](images/o9o9_2023-11-29-13-28-46.png)  
![](images/o9o9_2023-12-04-10-07-55.png)  
![](images/o9o9_2023-11-29-13-29-54.png)  
![](images/o9o9_2023-11-29-13-30-04.png)  
![](images/o9o9_2023-11-29-13-31-27.png)  
![](images/o9o9_2023-11-29-13-32-59.png)

## ハンズオン

### 1. 事前設定
#### 1-1. [メタストアで Delta Sharing を有効化する](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/set-up)  
[アカウントコンソール]->[データ] から対象のメタストアを選択し [Delta Sharing] のチェックボックスをクリック
![](images/o9o9_2023-12-05-10-53-48.png)  
[トークンの既定有効期限を設定し有効化](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-recipient#--manage-recipient-tokens-open-sharing)  
![](images/o9o9_2023-12-05-11-17-29.png)  
- トークンはオープン共有の受信側が共有データにアクセスする際に利用  
- Databricks 間共有にはトークンは利用されない（＝共有設定が存在する限り無期限）  
- 既定の有効期限はいつでも変更可能（すでに発行済みのトークンには反映されない）
- 発行済みトークンの有効期限は即時失効や前倒し可能（後ろ倒しは不可）
- 失効した場合はあらためてアクティベーションリンク（後述）の共有が必要  

![](images/o9o9_2023-12-05-11-27-20.png)  

#### 1-2. [監査ログ有効化(オプション)](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/audit-logging-provider)

##### [一覧確認](https://learn.microsoft.com/ja-jp/azure/databricks/administration-guide/system-tables/#list-available-system-schemas)
```bash
curl -v -X GET -H "Authorization: Bearer <PAT Token>" "https://adb-<xxx>.azuredatabricks.net/api/2.0/unity-catalog/metastores/<metastore-id>/systemschemas"
```  
![](images/o9o9_2023-11-29-13-21-00.png)  

##### [有効化](https://learn.microsoft.com/ja-jp/azure/databricks/administration-guide/system-tables/#enable-a-system-schema)
```bash
curl -v -X PUT -H "Authorization: Bearer <PAT Token>" "https://adb-<xxx>.azuredatabricks.net/api/2.0/unity-catalog/metastores/<metastore-id>/systemschemas/<SCHEMA_NAME>"
```
![](images/o9o9_2023-11-29-13-21-25.png)  
![](images/o9o9_2023-11-29-13-21-37.png)  

### 2. [Databricks 間共有](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/share-data-databricks#databricks-to-databricks-delta-sharing-workflow)

#### 2-1. [送信側による共有設定](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/share-data-databricks#databricks-to-databricks-delta-sharing-workflow)

##### 2-1-1. [共有の作成](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-share)
[カタログエクスプローラー]->[Delta Sharing]->[自分が共有]->[データの共有]  
![](images/o9o9_2023-11-29-13-21-52.png)  
[アセットを追加]  
![](images/o9o9_2023-11-29-13-22-00.png)  
![](images/o9o9_2023-11-29-13-22-09.png)  
※.オプションでタイムトラベルやCDFアクセスを許可可能  
![](images/o9o9_2023-11-29-13-22-15.png)  
![](images/o9o9_2023-11-29-13-22-21.png)  

##### 2-1-2. [受信者の共有識別子の取得](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-recipient#request-uuid)  
[カタログエクスプローラー]->[Delta Sharing]->[自分と共有]  
※. 受信者の Databricks アカウント側の操作
![](images/o9o9_2023-11-29-13-21-44.png)  

##### 2-1-3. [受信者の作成](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-recipient#create-recipient-db-to-db)
[カタログエクスプローラー]->[Delta Sharing]->[自分が共有]->[データの共有]->[新たな受信者] で受信者の共有識別子を入力  
![](images/o9o9_2023-11-29-13-22-29.png)  
![](images/o9o9_2023-11-29-13-22-36.png)  

##### 2-1-4. [受信者へのアクセス許可](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/grant-access)  
共有対象を選択  
![](images/o9o9_2023-11-29-13-22-44.png)  
![](images/o9o9_2023-11-29-13-22-51.png)  

#### 2-2. [受信側での共有の利用](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/read-data-databricks)  
[カタログエクスプローラー]->[Delta Sharing]->[自分と共有]  
![](images/o9o9_2023-11-29-13-22-57.png)  
[カタログを作成]  
![](images/o9o9_2023-11-29-13-23-05.png)  
カタログ名を設定  
![](images/o9o9_2023-11-29-13-23-10.png)  
![](images/o9o9_2023-11-29-13-23-22.png)  
※ ストレージへのアクセス権が無い場合はNG（組織外だと、これは使いにくい。。。）
![](images/o9o9_2023-11-29-13-23-29.png)  

### 3. [オープン共有](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/share-data-open)  

#### 3-1. [送信側による共有設定](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/share-data-open#delta-sharing-open-sharing-workflow)

##### 3-1-1. [共有の作成](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-share)
[カタログエクスプローラー]->[Delta Sharing]->[自分が共有]->[データの共有]  
![](images/o9o9_2023-11-29-13-21-52.png)  
[アセットを追加]  
![](images/o9o9_2023-11-29-13-22-00.png)  
![](images/o9o9_2023-11-29-13-22-09.png)  
※.オプションでタイムトラベルやCDFアクセスを許可可能  
![](images/o9o9_2023-11-29-13-22-15.png)  
![](images/o9o9_2023-11-29-13-22-21.png)  

##### 3-1-2. [受信者の作成](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-recipient#create-recipient-db-to-db)
[カタログエクスプローラー]->[Delta Sharing]->[自分が共有]->[データの共有]->[新たな受信者]  
※.オープン共有の場合は受信者の共有識別子の入力は不要
![](images/o9o9_2023-11-29-13-23-38.png)  
アクティベーションリンクを受信者に共有  
![](images/o9o9_2023-11-29-13-23-45.png)  
![](images/o9o9_2023-11-29-13-23-50.png)  

##### 3-1-3. [受信者へのアクセス許可](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/grant-access)  
共有対象を選択  
![](images/o9o9_2023-11-29-13-22-44.png)  
![](images/o9o9_2023-11-29-13-22-51.png)  

#### 3-2. [受信側での共有の利用](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/read-data-open)  

##### 3-2-1. [アクティベーションリンクを受信者から入手](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-recipient#get-activation-link)  
※. 送信者の Databricks アカウント側の操作
![](images/o9o9_2023-11-29-13-24-49.png)  

##### 3-2-2. [資格情報ファイルの入手](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/recipient#get-access-open)  
アクティベーションリンクにアクセスし資格情報ファイルをダウンロードする
![](images/o9o9_2023-11-29-13-24-53.png)  
![](images/o9o9_2023-11-29-13-25-00.png)  
![](images/o9o9_2023-11-29-13-25-06.png)  

##### 3-2-3. 例：Power BI から共有にアクセス  
資格情報ファイルからエンドポイントとベアラートークンをメモ  
![](images/o9o9_2023-11-29-13-25-12.png)  
[データを取得]から[Delta Sharing]を選択 
![](images/o9o9_2023-11-29-13-25-18.png)  
メモしたエンドポイントとベアラートークンを入力  
![](images/o9o9_2023-12-05-11-42-09.png)  
![](images/o9o9_2023-12-05-11-42-48.png)  
ここから先は他のデータソースと同様の操作
![](images/o9o9_2023-11-29-13-25-29.png)  
![](images/o9o9_2023-11-29-13-25-34.png)  
※. [Delta Sharing Ecosystem](https://delta.io/sharing/)
![](images/o9o9_2023-12-04-10-07-55.png)  

#### 3-3. [補足：トークンの管理](https://learn.microsoft.com/ja-jp/azure/databricks/data-sharing/create-recipient#--manage-recipient-tokens-open-sharing)  
- トークンはオープン共有の受信側が共有データにアクセスする際に利用  
- Databricks 間共有にはトークンは利用されない（＝共有設定が存在する限り無期限）  
- 既定の有効期限はいつでも変更可能（すでに構成済みの共有には反映されない＝失効後にトークンを再発行しても構成時の有効期限が利用される）
- 発行済みトークンの有効期限は即時失効や前倒し可能（後ろ倒しは不可）
- 失効した場合はあらためてアクティベーションリンク（後述）の共有が必要  

発行済みトークンの有効期限は即時失効や前倒し可能（後ろ倒しは不可）  
![](images/o9o9_2023-12-05-12-24-48.png)  
![](images/o9o9_2023-12-05-12-25-27.png)  
トークンの有効期限が失効すると Status が 保留中 となりアクセスは拒否される。
![](images/o9o9_2023-12-05-11-33-58.png)  
共有を再開する場合はアクティベーションリンクを再共有する。
![](images/o9o9_2023-12-05-11-38-25.png)  
資格情報ファイルをダウンロードされると Status が 有効化済み となる。
![](images/o9o9_2023-12-05-11-45-35.png)  

# DBSQL
![](images/o9o9_2023-08-17-15-45-28.png)

# Workflows
![](images/o9o9_2023-07-10-23-18-15.png)  
![](images/o9o9_2023-07-10-23-18-33.png)  
![](images/o9o9_2023-07-10-23-18-57.png)  
![](images/o9o9_2023-07-10-23-19-33.png)  
![](images/o9o9_2023-07-10-23-20-07.png)  
![](images/o9o9_2023-07-10-23-20-21.png)  
![](images/o9o9_2023-07-10-23-21-36.png)  
![](images/o9o9_2023-07-10-23-22-01.png)  

# UC
![](images/o9o9_2023-07-10-23-40-03.png)  
![](images/o9o9_2023-07-10-23-41-43.png)  
![](images/o9o9_2023-07-10-23-42-04.png)  
![](images/o9o9_2023-07-10-23-42-37.png)  
![](images/o9o9_2023-07-10-23-43-13.png)  
![](images/o9o9_2023-07-10-23-44-34.png)  
![](images/o9o9_2023-07-10-23-45-24.png)  
![](images/o9o9_2023-07-10-23-47-48.png)  
![](images/o9o9_2023-07-10-23-48-30.png)  
![](images/o9o9_2023-07-10-23-49-36.png)  
![](images/o9o9_2023-07-10-23-49-49.png)  
![](images/o9o9_2023-07-10-23-50-00.png)  
![](images/o9o9_2023-07-10-23-51-52.png)  
![](images/o9o9_2023-07-10-23-55-54.png)  
![](images/o9o9_2023-07-10-23-56-18.png)  
![](images/o9o9_2023-06-26-14-36-25.png)  
![](images/o9o9_2023-06-26-14-42-23.png)  
![](images/o9o9_2023-06-26-14-56-31.png)  
![](images/o9o9_2023-06-26-14-51-11.png)
![](images/o9o9_2023-06-26-14-57-45.png)  
![](images/o9o9_2023-06-26-14-58-53.png)  
![](images/o9o9_2023-06-26-14-59-32.png)  
![](images/o9o9_2023-06-26-16-15-52.png)  
![](images/o9o9_2023-06-26-15-52-35.png)  
![](images/o9o9_2023-06-26-16-04-27.png)  
![](images/o9o9_2023-06-26-16-09-49.png)  
![](images/o9o9_2023-06-26-16-11-42.png)
![](images/o9o9_2023-06-26-16-12-56.png)  
![](images/o9o9_2023-06-26-16-17-03.png)  
![](images/o9o9_2023-06-26-16-21-06.png)  
![](images/o9o9_2023-06-26-16-22-36.png)  
![](images/o9o9_2023-06-26-16-25-35.png)  
![](images/o9o9_2023-06-26-16-28-01.png)  
![](images/o9o9_2023-06-26-16-31-49.png)  
![](images/o9o9_2023-06-26-16-39-20.png)  
![](images/o9o9_2023-06-26-16-40-54.png)  
![](images/o9o9_2023-06-26-16-50-44.png)  
![](images/o9o9_2023-06-26-16-52-12.png)  
data / __unitystorage / catalogs / e94e40e2-b480-4e6d-97da-367d14b6ae79 / tables / 6a2f9cef-e362-4891-b442-8c861d6bcc50  
![](images/o9o9_2023-06-26-16-57-34.png)  
![](images/o9o9_2023-06-26-16-59-22.png)  
![](images/o9o9_2023-06-26-17-02-44.png)  
![](images/o9o9_2023-06-26-17-03-50.png)  
![](images/o9o9_2023-06-26-17-04-12.png)  
unitycatalog / metastore / b0aa6069-2eb1-4c2e-b0d1-22e19233b689 / tables / 35e08fd8-19c6-47eb-b62f-e57f4a915896  

# Storage
![](images/o9o9_2023-06-26-18-57-30.png)

# Optimize  
![](images/o9o9_2023-08-17-08-41-21.png)  
![](images/o9o9_2023-08-17-08-40-58.png)  
![](images/o9o9_2023-08-17-08-41-41.png)  
![](images/o9o9_2023-08-17-13-50-57.png)

# Purview UC
![](images/o9o9_2023-08-29-09-47-28.png)  
![](images/o9o9_2023-08-29-09-47-36.png)  
![](images/o9o9_2023-08-29-09-46-39.png)  
![](images/o9o9_2023-08-29-09-46-53.png)  
![](images/o9o9_2023-08-29-09-46-04.png)  
![](images/o9o9_2023-08-29-09-43-05.png)  
![](images/o9o9_2023-08-29-09-43-43.png)  
![](images/o9o9_2023-08-29-09-44-33.png)  
![](images/o9o9_2023-08-29-09-44-52.png)  

# Purview Lineage
![](images/o9o9_2023-09-11-17-54-59.png)  
![](images/o9o9_2023-09-11-18-40-29.png)  
![](images/o9o9_2023-09-11-18-42-35.png)  

# w/ADF
① コピーアクティビティ  
コピーアクティビティは Hive Metastore のみサポート
![](images/o9o9_2023-09-07-09-33-01.png)  
![](images/o9o9_2023-09-07-09-33-43.png)  
既存テーブルへのSyncのみ（テーブル新規作成は不可）
![](images/o9o9_2023-09-07-10-03-05.png)
パイプライン実行後にDeltaTableへの出力を確認  
![](images/o9o9_2023-09-07-10-22-48.png)  

② データフロー  
データフローはストレージへのDelta出力をサポート
![](images/o9o9_2023-09-07-10-04-20.png)
![](images/o9o9_2023-09-07-10-05-45.png)  
パイプライン実行後に UC へのカタログ登録とDeltaTableとしての出力を確認  
![](images/o9o9_2023-09-07-10-46-50.png)  

＜参考＞
- https://learn.microsoft.com/ja-jp/azure/data-factory/connector-azure-databricks-delta-lake?tabs=data-factory
- https://learn.microsoft.com/ja-jp/azure/data-factory/format-delta  


# Security
![](images/o9o9_2023-08-17-10-27-16.png)  
![](images/o9o9_2023-08-17-10-27-29.png)  
![](images/o9o9_2023-08-17-10-27-44.png)  
![](images/o9o9_2023-08-17-10-35-45.png)  
![](images/o9o9_2023-08-17-10-35-58.png)  
![](images/o9o9_2023-08-17-10-36-14.png)  
![](images/o9o9_2023-08-17-10-37-49.png)  
![](images/o9o9_2023-08-17-10-38-07.png)  
![](images/o9o9_2023-08-17-10-38-30.png)  
![](images/o9o9_2023-08-17-10-38-45.png)  
![](images/o9o9_2023-08-17-10-39-06.png)  
![](images/o9o9_2023-08-17-10-39-24.png)  
![](images/o9o9_2023-08-17-10-39-42.png)  
![](images/o9o9_2023-08-17-10-39-53.png)  
![](images/o9o9_2023-08-17-10-40-06.png)  
![](images/o9o9_2023-08-17-10-40-25.png)  