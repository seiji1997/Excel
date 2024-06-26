graph TD;
    classDef subgraphNode fill:#e0e0e0,stroke:#808080,stroke-width:2px;
    classDef noteStyle fill:#ffffcc,stroke:#808080,stroke-width:1px,font-style:italic;

    subgraph AWS["AWS"]
        direction LR;
        S3["Amazon S3"]:::subgraphNode;
        bucket["Bucket"]:::subgraphNode;
        Lambda["Lambda"]:::subgraphNode;
        EventBridge["Event Bridge"]:::subgraphNode;
        note1["Note: This is a memo inside the subgraph."]:::noteStyle;
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

    %% 矢印を点線にする
    linkStyle 0 stroke-dasharray: 5, 5;
    linkStyle 1 stroke-dasharray: 5, 5;
    linkStyle 2 stroke-dasharray: 5, 5;