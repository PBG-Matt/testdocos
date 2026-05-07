flowchart TB
    Users["PBG Users\nStaff, leadership, engineers, admins"]
    IT["PBG IT / Software Team"]

    subgraph SaaS["SaaS / Cloud Services"]
        M365["Microsoft 365\nSharePoint, Exchange, Teams"]
        Entra["Entra ID\nAuthentication"]
        Autodesk["Autodesk\nACC, accounts, status"]
        Bentley["Bentley\nProjectWise, CONNECTION"]
        Bluebeam["Bluebeam Studio"]
        Xero["Xero"]
        AirlockSaaS["Airlock Digital"]
        GitHub["GitHub"]
        TailscaleCloud["Tailscale Coordination"]
    end

    subgraph Azure["Microsoft Azure"]
        Timesheets["Timesheets\nAzure App Service"]
        TimesheetsSQL["Timesheets SQL\nAzure SQL"]
        NightVision["NightVision\nAzure Web App"]
        NightVisionSQL["NightVision SQL\nAzure SQL"]
        UptimeKuma["Uptime Kuma\nAzure VM, Tailscale-only"]
        AzureDNS["Azure DNS\nDNS-01 TLS challenge"]
        AzureStorage["Azure Blob Storage\nProposed backup/archive"]
        AzureBackup["Azure Backup\nAirlock VM"]
    end

    subgraph OnPrem["Brisbane / Equinix On-Prem Environment"]
        Proxmox["Proxmox Cluster\nbne1.pitchblackeng.com"]
        NAS["bne1-nas-01\nSynology NAS"]
        Caddy["Caddy Reverse Proxy\nWildcard TLS"]
        TailscaleRouter["Tailscale Subnet Router\ndebian3"]
        Apps["Internal Apps\nOpen WebUI, IMS/Nexus, Outline, GridCommander, PID Extractor"]
        Monitoring["PLG Stack\nPrometheus, Loki, Grafana, Alloy"]
        ProtegeWX["ProtegeWX\nFacilities access control"]
        Veeam["Veeam Backup Server\nMicrosoft 365 backup"]
    end

    Users --> M365
    Users --> Autodesk
    Users --> Bentley
    Users --> Bluebeam
    Users --> Xero
    Users --> Timesheets
    Users --> NightVision
    Users --> Caddy

    IT --> GitHub
    IT --> TailscaleCloud
    IT --> Proxmox
    IT --> UptimeKuma

    M365 --> Entra
    Autodesk --> Entra
    Bentley --> Entra
    AirlockSaaS --> Entra

    Timesheets --> TimesheetsSQL
    NightVision --> NightVisionSQL

    TailscaleCloud --> TailscaleRouter
    UptimeKuma --> TailscaleCloud
    TailscaleRouter --> Proxmox
    Proxmox --> Apps
    Proxmox --> Monitoring
    Proxmox --> NAS
    Proxmox --> ProtegeWX

    Caddy --> AzureDNS
    Caddy --> Apps

    Veeam --> NAS
    NAS --> AzureStorage
    M365 --> Veeam
    AzureBackup --> AzureStorage
