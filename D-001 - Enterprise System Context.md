flowchart TB
    Users["PBG Users<br/>Staff, leadership, engineers, admins"]
    IT["PBG IT / Software Team"]

    subgraph SaaS["SaaS / Cloud Services"]
        M365["Microsoft 365<br/>SharePoint, Exchange, Teams"]
        Entra["Entra ID<br/>Authentication"]
        Autodesk["Autodesk<br/>ACC, accounts, status"]
        Bentley["Bentley<br/>ProjectWise, CONNECTION"]
        Bluebeam["Bluebeam Studio"]
        Xero["Xero"]
        AirlockSaaS["Airlock Digital"]
        GitHub["GitHub"]
        TailscaleCloud["Tailscale Coordination"]
    end

    subgraph Azure["Microsoft Azure"]
        Timesheets["Timesheets<br/>Azure App Service"]
        TimesheetsSQL["Timesheets SQL<br/>Azure SQL"]
        NightVision["NightVision<br/>Azure Web App"]
        NightVisionSQL["NightVision SQL<br/>Azure SQL"]
        UptimeKuma["Uptime Kuma<br/>Azure VM, Tailscale-only"]
        AzureDNS["Azure DNS<br/>DNS-01 TLS challenge"]
        AzureStorage["Azure Blob Storage<br/>Proposed backup/archive"]
        AzureBackup["Azure Backup<br/>Airlock VM"]
    end

    subgraph OnPrem["Brisbane / Equinix On-Prem Environment"]
        Proxmox["Proxmox Cluster<br/>bne1.pitchblackeng.com"]
        NAS["bne1-nas-01<br/>Synology NAS"]
        Caddy["Caddy Reverse Proxy<br/>Wildcard TLS"]
        TailscaleRouter["Tailscale Subnet Router<br/>debian3"]
        Apps["Internal Apps<br/>Open WebUI, IMS/Nexus, Outline, GridCommander, PID Extractor"]
        Monitoring["PLG Stack<br/>Prometheus, Loki, Grafana, Alloy"]
        ProtegeWX["ProtegeWX<br/>Facilities access control"]
        Veeam["Veeam Backup Server<br/>Microsoft 365 backup"]
    end

    %% User access
    Users --> M365
    Users --> Autodesk
    Users --> Bentley
    Users --> Bluebeam
    Users --> Xero
    Users --> Timesheets
    Users --> NightVision
    Users --> Caddy

    %% IT operations
    IT --> GitHub
    IT --> TailscaleCloud
    IT --> Proxmox
    IT --> UptimeKuma

    %% Identity integration
    M365 --> Entra
    Autodesk --> Entra
    Bentley --> Entra
    AirlockSaaS --> Entra

    %% Application data flow
    Timesheets --> TimesheetsSQL
    NightVision --> NightVisionSQL

    %% Networking
    TailscaleCloud --> TailscaleRouter
    UptimeKuma --> TailscaleCloud
    TailscaleRouter --> Proxmox

    %% On-prem workloads
    Proxmox --> Apps
    Proxmox --> Monitoring
    Proxmox --> NAS
    Proxmox --> ProtegeWX

    %% Reverse proxy + DNS
    Caddy --> AzureDNS
    Caddy --> Apps

    %% Backup & storage
    Veeam --> NAS
    NAS --> AzureStorage
    M365 --> Veeam
    AzureBackup --> AzureStorage
