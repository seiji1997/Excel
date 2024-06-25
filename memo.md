graph TD;
    classDef subgraphNode fill:#f5f5f5,stroke:#999999,stroke-width:2px;

    subgraph AWS["AWS"]
        direction LR;
        S3["Amazon S3"]:::subgraphNode;
        bucket["Bucket"]:::subgraphNode;
        Lambda["Lambda"]:::subgraphNode;
        EventBridge["Event Bridge"]:::subgraphNode;
        S3 -->|Transferring objects and deleting| bucket;
        Lambda -->|Manual processing and shaping| bucket;
        EventBridge -->|Executed every hour| Lambda;
    end

    subgraph GCP["GCP"]
        direction LR;
        gsutil["gsutil"]:::subgraphNode;
        BigQuery["BigQuery"]:::subgraphNode;
        gsutil --> BigQuery;
    end