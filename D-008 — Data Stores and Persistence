flowchart TB
    subgraph Applications["Applications and Services"]
        OpenWebUI["Open WebUI"]
        Nexus["Nexus / IMS"]
        Outline["Outline"]
        TagCommander["TagCommander"]
        GridCommander["GridCommander"]
        PID["PID Extractor"]
        LibreSpeed["LibreSpeed"]
        Timesheets["Timesheets"]
        NightVision["NightVision"]
        Airlock["Airlock Digital"]
        M365["Microsoft 365"]
    end

    subgraph DataStores["Data Stores"]
        CentralPG["PostgreSQL 17 + pgvector\nLXC 120"]
        OutlinePG["postgres:16\nlocal to debian3"]
        RedisOutline["redis\nlocal to debian3"]
        Valkey["Valkey/Redis\ndebian4"]
        Zot["Zot OCI Registry\nLXC 106"]
        SQLite["SQLite telemetry\nLibreSpeed"]
        AzureSQLTimesheets["Azure SQL\nTimesheets"]
        AzureSQLNightVision["Azure SQL\nNightVision"]
        AzureSQLERP["Azure SQL\nERPTesting pbg-erp-dev/prod"]
        SharePoint["SharePoint Online"]
        NAS["Synology NAS\nbne1-nas-01"]
        GitRepo["Git repository\nAirlock config"]
    end

    OpenWebUI --> CentralPG
    Nexus --> CentralPG
    Outline --> OutlinePG
    Outline --> RedisOutline
    TagCommander --> Zot
    GridCommander --> Zot
    PID --> Zot
    LibreSpeed --> SQLite
    Timesheets --> AzureSQLTimesheets
    NightVision --> AzureSQLNightVision
    Airlock --> GitRepo
    M365 --> SharePoint
    SharePoint --> NAS
    AzureSQLERP:::gap

    classDef gap fill:#fff3cd,stroke:#664d03,stroke-width:2px,color:#000;
