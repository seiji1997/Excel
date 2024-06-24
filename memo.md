graph TD;
    classDef commonStyle fill:#f9f,stroke:#333,stroke-width:4px;
    classDef processStyle fill:#ccf,stroke:#333,stroke-width:2px;
    classDef dataStyle fill:#fff,stroke:#f66,stroke-width:2px,stroke-dasharray: 5, 5;

    subgraph AWS
        direction LR;
        S3[Amazon S3];
        bucket[(bucket)];
        Lambda[Lambda];
        EventBridge[Event Bridge];
        S3 -->|転送済みオブジェクトは削除| bucket;
        Lambda -->|手動で加工整形| bucket;
        EventBridge -->|毎時00分実行| Lambda;
        class S3,Lambda,EventBridge processStyle;
        class bucket dataStyle;
    end

    subgraph GCP_東京リージョン
        direction LR;
        gsutil[データ転送<br>gsutil];
        Standard[Standardクラス<br>Cloud Storage];

        subgraph BigQuery_Transfer
            direction TB;
            BigQueryTransfer[データ転送<br>BigQuery Data Transfer];
            temporaly_data[temporal_data<br>id created_at author_id];
            BigQueryTransfer --> temporaly_data;
            class BigQueryTransfer processStyle;
            class temporaly_data dataStyle;
        end
        
        BigQuery1[データ整形<br>BigQuery];
        StorageTransfer[データ転送<br>Storage Transfer];

        subgraph Coldline_Class
            direction TB;
            Codline[Coldlineクラス<br>Cloud Storage];
            BigQuery2[データ整形<br>BigQuery];
            Codline --> BigQuery2;
            class Codline dataStyle;
            class BigQuery2 processStyle;
        end

        gsutil -->|cronでgsutilを毎時20分に実行| Standard;
        Standard -->|毎時30分実行| BigQueryTransfer;
        temporaly_data --> BigQuery1;
        BigQuery1 -->|スケジュールクエリでauthor_idを削除| StorageTransfer;
        StorageTransfer --> Codline;
        class gsutil,BigQuery1,StorageTransfer processStyle;
        class Standard dataStyle;
    end

    subgraph GCP_大阪リージョン
        direction LR;
        Codline;
        class Codline dataStyle;
    end

    TwitterAPI[Twitter API]:::commonStyle -->|2, request| AWS;
    AWS -->|3, result| GCP_東京リージョン;
    GCP_東京リージョン --> GCP_大阪リージョン;
    BigQuery2 -->|毎時40分実行| TwitterAPI_GCP[TwitterAPI_GCP<br>id created_at]:::commonStyle;

    %% ノード間のスペーシング調整のための見えないノード
    invisibleNode[ ]:::commonStyle;
    AWS --> invisibleNode;
    invisibleNode --> GCP_東京リージョン;