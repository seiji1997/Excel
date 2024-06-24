graph LR;
    subgraph AWS
        direction TB;
        S3[Amazon S3];
        bucket[(bucket)];
        Lambda[Lambda];
        EventBridge[Event Bridge];
        S3 -->|転送済みオブジェクトは削除| bucket;
        Lambda -->|手動で加工整形| bucket;
        EventBridge -->|毎時00分実行| Lambda;
    end;

    subgraph GCP_東京リージョン
        direction TB;
        gsutil[データ転送<br>gsutil];
        Standerd[Standardクラス<br>Cloud Storage];
        BigQueryTransfer[データ転送<br>BigQuery Data Transfer];
        BigQuery1[データ整形<br>BigQuery];
        StorageTransfer[データ転送<br>Storage Transfer];
        BigQuery2[データ整形<br>BigQuery];

        gsutil -->|cronでgsutilを毎時20分に実行| Standerd;
        Standerd -->|毎時30分実行| BigQueryTransfer;
        BigQueryTransfer --> temporaly_data[temporal_data<br>id created_at author_id];
        temporaly_data --> BigQuery1;
        BigQuery1 -->|スケジュールクエリでauthor_idを削除| select_query[select * except(author_id) from twitterapi_data_dwh.temporal_data];
        select_query --> StorageTransfer;
        StorageTransfer --> Codline[Coldlineクラス<br>Cloud Storage];
        Codline --> BigQuery2;
    end;

    subgraph GCP_大阪リージョン
        direction TB;
        Codline;
    end;

    TwitterAPI[Twitter API] -->|2, request| AWS;
    AWS -->|3, result| GCP_東京リージョン;
    GCP_東京リージョン --> GCP_大阪リージョン;
    BigQuery2 -->|毎時40分実行| TwitterAPI_GCP[TwitterAPI_GCP<br>id created_at];