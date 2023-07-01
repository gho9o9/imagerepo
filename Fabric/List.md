Paste Image を利用。
Ctrl + Alt + v でクリップボードの画像をペースト。
https://raw.githubusercontent.com/gho9o9/imagerepo/main/<カテゴリ>/images/*.png

![](images/o9o9_2023-06-12-10-50-06.png)  
![](images/o9o9_2023-06-12-10-50-15.png)  
![](images/o9o9_2023-06-12-10-57-20.png)  
![](images/o9o9_2023-07-01-19-01-38.png)  
![](images/o9o9_2023-07-01-19-03-13.png)  

# Databricks Migration
## まとめ
![](images/o9o9_2023-06-16-16-35-10.png)

## Step by Step 
### Databricks Hive DeltaTable を定義
![](images/o9o9_2023-06-14-17-07-06.png)
![](images/o9o9_2023-06-14-17-16-37.png)

### Lakehouse ショートカットで Hive DeltaTable をアタッチ
![](images/o9o9_2023-06-14-16-55-09.png)
![](images/o9o9_2023-06-14-16-55-38.png)

### Lakehouse で Read/Write & TimeTravel
![](images/o9o9_2023-06-14-17-25-25.png)

### Databricks と相互運用可能
![](images/o9o9_2023-06-14-17-28-42.png)

### Lakehouse SQL Endpoint からも参照が可能（更新は不可）
![](images/o9o9_2023-06-14-17-39-19.png)
![](images/o9o9_2023-06-14-17-39-35.png)
![](images/o9o9_2023-06-14-17-51-06.png)

### Lakehouse 経由で Warehouse に DeltaTable をコピー（現時点で DeltaLake コネクターが未対応のため Lakehouse 経由でコピー）
![](images/o9o9_2023-06-14-19-50-53.png)
![](images/o9o9_2023-06-14-19-54-02.png)

### Warehouse で Read/Write
![](images/o9o9_2023-06-14-19-52-43.png)
![](images/o9o9_2023-06-14-19-53-32.png)

### Lakehouse SQL Endpoint に Warehouse をアタッチすると Lakehouse SQL Endpoin から Wahourese の Write が可能
![](images/o9o9_2023-06-14-19-56-10.png)
![](images/o9o9_2023-06-14-19-57-16.png)
![](images/o9o9_2023-06-14-19-57-44.png)
![](images/o9o9_2023-06-14-19-59-57.png)

### Lakehouse ショートカットで Warehouse をアタッチすると Write 不可
![](images/o9o9_2023-06-14-20-03-46.png)
![](images/o9o9_2023-06-14-20-12-49.png)
![](images/o9o9_2023-06-14-20-11-22.png)

### Databricks UC DeltaTable を定義
![](images/o9o9_2023-06-26-18-35-23.png)  

### UCのテーブルはフォルダパスがランダムな文字列になってしまうのでショートカット作成時のパス指定がつらい。
![](images/o9o9_2023-06-26-18-32-53.png)  
![](images/o9o9_2023-06-26-18-38-08.png)  

### Fabric Lakehouse ショートカットで UC DeltaTable をアタッチ
![](images/o9o9_2023-06-26-18-47-22.png)  
![](images/o9o9_2023-06-26-18-48-09.png)

### Fabric Lakehouse から UC DeltaTable を更新
![](images/o9o9_2023-06-16-15-58-16.png)

### Databricks と相互運用可能
![](images/o9o9_2023-06-16-16-00-47.png)