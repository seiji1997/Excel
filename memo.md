graph LR
    subgraph AWS
        direction TB
        S3[Amazon S3]
        bucket[(bucket)]
        Lambda[Lambda]
        EventBridge[Event Bridge]
        S3 -->|転送済みオブジェクトは削除| bucket
        Lambda -->|手動で加工整形| bucket
        EventBridge -->|毎時00分実行| Lambda
    end

    subgraph GCP_東京リージョン
        direction TB
        gsutil[データ転送<br>gsutil]
        Standard[Standardクラス<br>Cloud Storage]
        
        subgraph BigQuery_Transfer
            BigQueryTransfer[データ転送<br>BigQuery Data Transfer]
            temporaly_data[temporal_data<br>id created_at author_id]
            BigQueryTransfer --> temporaly_data
        end
        
        BigQuery1[データ整形<br>BigQuery]
        StorageTransfer[データ転送<br>Storage Transfer]
        
        subgraph Coldline_Class
            Codline[Coldlineクラス<br>Cloud Storage]
            BigQuery2[データ整形<br>BigQuery]
            Codline --> BigQuery2
        end

        gsutil -->|cronでgsutilを毎時20分に実行| Standard
        Standard -->|毎時30分実行| BigQueryTransfer
        temporaly_data --> BigQuery1
        BigQuery1 -->|スケジュールクエリでauthor_idを削除| StorageTransfer
        StorageTransfer --> Codline
    end

    subgraph GCP_大阪リージョン
        direction TB
        Codline
    end

    TwitterAPI[Twitter API] -->|2, request| AWS
    AWS -->|3, result| GCP_東京リージョン
    GCP_東京リージョン --> GCP_大阪リージョン
    BigQuery2 -->|毎時40分実行| TwitterAPI_GCP[TwitterAPI_GCP<br>id created_at]