graph TD;
    classDef commonStyle fill:#f9f,stroke:#333,stroke-width:4px;
    classDef processStyle fill:#ccf,stroke:#333,stroke-width:2px;
    classDef dataStyle fill:#fff,stroke:#f66,stroke-width:2px,stroke-dasharray: 5, 5;
    classDef boldText font-weight:bold;

    subgraph AWS["AWS (bold title not supported natively)"]
        direction LR;
        S3[Amazon S3]:::commonStyle;
        bucket[(bucket)]:::dataStyle;
        Lambda[Lambda]:::processStyle;
        EventBridge[Event Bridge]:::processStyle;
        S3 -->|転送済みオブジェクトは削除| bucket;
        Lambda -->|手動で加工整形| bucket;
        EventBridge -->|毎時00分実行| Lambda;
    end

    subgraph GCP_東京リージョン["GCP 東京リージョン (bold title not supported natively)"]
        direction LR;
        gsutil[データ転送<br>gsutil]:::processStyle;
        Standard[Standardクラス<br>Cloud Storage]:::dataStyle;

        subgraph BigQuery_Transfer["BigQuery Transfer (bold title not supported natively)"]
            direction TB;
            BigQueryTransfer[データ転送<br>BigQuery Data Transfer]:::processStyle;
            temporaly_data[temporal_data<br>id created_at author_id]:::dataStyle;
            BigQueryTransfer --> temporaly_data;
        end
        
        BigQuery1[データ整形<br>BigQuery]:::processStyle;
        StorageTransfer[データ転送<br>Storage Transfer]:::processStyle;

        subgraph Coldline_Class["Coldline Class (bold title not supported natively)"]
            direction TB;
            Codline[Coldlineクラス<br>Cloud Storage]:::dataStyle;
            BigQuery2[データ整形<br>BigQuery]:::processStyle;
            Codline --> BigQuery2;
        end

        gsutil -->|cronでgsutilを毎時20分に実行| Standard;
        Standard -->|毎時30分実行| BigQueryTransfer;
        temporaly_data --> BigQuery1;
        BigQuery1 -->|スケジュールクエリでauthor_idを削除| StorageTransfer;
        StorageTransfer --> Codline;
    end

    subgraph GCP_大阪リージョン["GCP 大阪リージョン (bold title not supported natively)"]
        direction LR;
        Codline:::dataStyle;
    end

    TwitterAPI[Twitter API]:::commonStyle -->|2, request| AWS;
    AWS -->|3, result| GCP_東京リージョン;
    GCP_東京リージョン --> GCP_大阪リージョン;
    BigQuery2 -->|毎時40分実行| TwitterAPI_GCP[TwitterAPI_GCP<br>id created_at]:::commonStyle;