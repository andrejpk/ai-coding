# Azure IoT Operations Architecture Design Document

## 1. Overview

This document describes the architecture for an Azure IoT Operations solution spanning from factory assets to cloud services. The architecture consists of three primary domains: Factory, Edge, and Azure cloud, with distinct IT and OT management roles.

```mermaid
graph TB
    subgraph "Management Roles"
        ITRole["Information technology (IT) role"]
        OTRole["Operational technology (OT) role"]
    end

    subgraph "High-Level Architecture"
        Factory["Factory"]
        Edge["Edge"]
        Azure["Azure"]
    end

    ITRole -.-> Edge
    ITRole -.-> Azure
    OTRole -.-> Edge

    Factory --- Edge
    Edge --- Azure

    classDef azure fill:#CCE5FF,stroke:#0072C6;
    classDef edge fill:#E6F2FF,stroke:#0072C6;
    classDef factory fill:#E6E6E6,stroke:#666;
    classDef role fill:#F4DFFF,stroke:#6E4A9E;

    class Factory factory;
    class Edge edge;
    class Azure azure;
    class ITRole,OTRole role;
```

## 2. Factory Layer

The factory layer contains physical assets and industrial control systems that produce operational data.

```mermaid
graph LR
    subgraph "Factory Assets"
        ONVIFcams["ONVIF cameras"]
        IPcams["IP cameras"]
        OPC_UA_server1["OPC UA server-01"]
        OPC_UA_server2["OPC UA server-02"]
        Asset1["Asset-01"]
        Asset2["Asset-02"]
        Asset3["Asset-03"]
        Asset4["Asset-04"]
    end

    Asset1 --- OPC_UA_server1
    Asset2 --- OPC_UA_server2
    Asset3 --- OPC_UA_server2
    Asset4 --- OPC_UA_server2

    classDef asset fill:#E6E6E6,stroke:#666;
    class ONVIFcams,IPcams,OPC_UA_server1,OPC_UA_server2,Asset1,Asset2,Asset3,Asset4 asset;
```

### 2.1 Factory Components

- **ONVIF cameras**: Standard IP cameras using the ONVIF protocol
- **IP cameras**: Generic IP-based cameras
- **OPC UA servers**:
  - OPC UA server-01: Connected to Asset-01
  - OPC UA server-02: Connected to Asset-02, Asset-03, and Asset-04
- **Assets**:
  - Asset-01: Factory equipment (appears to be a computer/server)
  - Asset-02: Factory equipment (appears to be industrial equipment)
  - Asset-03: Factory equipment (appears to be a mobile device)
  - Asset-04: Factory equipment (appears to be a networking device)

## 3. Edge Layer

The edge layer processes factory data locally before sending it to the cloud, using Azure IoT Operations enabled by Azure Arc.

```mermaid
graph TB
    subgraph "Edge Layer"
        direction TB
        AzureIoTOps["Azure IoT Operations - enabled by Azure Arc"]
        
        subgraph "Connectors"
            ONVIFconnector["Connector for ONVIF (preview)"]
            MediaConnector["Media connector (preview)"]
            OPCconnector["Connector for OPC UA"]
        end
        
        MQTTBroker["MQTT broker"]
        Dataflows["Dataflows"]
        AzureIoTLNM["Azure IoT Layered Network Management"]
        KubeWorkloads["User Kubernetes workloads"]
        
        ArcKube["Azure Arc-enabled Kubernetes cluster deployed at the edge"]
        
        subgraph "Arc Services"
            ArcServices["Azure Arc-enabled services
(Data services, App services, Machine Learning)"]
            ArcInfra["Azure Arc-enabled infrastructure services
(Azure Monitor, Microsoft Defender for Cloud, Azure Policy, etc)"]
        end
    end
    
    ONVIFconnector <--> MQTTBroker
    MediaConnector <--> MQTTBroker
    OPCconnector <--> MQTTBroker
    MQTTBroker <--> Dataflows
    MQTTBroker <--> AzureIoTLNM
    
    ArcKube --- ArcServices
    ArcKube --- ArcInfra
    
    classDef edge fill:#E6F2FF,stroke:#0072C6;
    classDef connector fill:#99CCFF,stroke:#0072C6;
    classDef broker fill:#FFE6CC,stroke:#D79B00;
    classDef arc fill:#E6F2FF,stroke:#0072C6;
    
    class AzureIoTOps,KubeWorkloads,AzureIoTLNM edge;
    class ONVIFconnector,MediaConnector,OPCconnector connector;
    class MQTTBroker broker;
    class ArcKube,ArcServices,ArcInfra arc;
```

### 3.1 Edge Components

- **Azure IoT Operations - enabled by Azure Arc**: Core edge processing platform
  - **Connectors**:
    - Connector for ONVIF (preview): Interfaces with ONVIF cameras
    - Media connector (preview): Processes media streams
    - Connector for OPC UA: Interfaces with OPC UA servers via OPC UA communication
  - **MQTT broker**: Central message broker handling data from connectors
  - **Dataflows**: Processes data transformations
  - **Azure IoT Layered Network Management**: Manages network connectivity for IoT devices
  
- **User Kubernetes workloads**: Custom containerized applications running at the edge

- **Azure Arc-enabled Kubernetes cluster deployed at the edge**: Kubernetes infrastructure managed by Azure Arc

- **Azure Arc-enabled services**:
  - Data services, App services, Machine Learning
  - Infrastructure services: Azure Monitor, Microsoft Defender for Cloud, Azure Policy, etc.

## 4. Azure Cloud Layer

The Azure cloud layer provides management, analytics, and storage capabilities for IoT data.

```mermaid
graph TB
    subgraph "Azure Cloud"
        AzureIoTOpsPortal["Azure IoT Operations Experience portal"]
        DeviceRegistry["Azure Device Registry management"]
        AssetsPipelines["Assets and pipelines management"]
        
        PowerBI["Power BI"]
        
        subgraph "Azure Services"
            MicrosoftFabric["Microsoft Fabric"]
            EventGrid["Azure Event Grid"]
            EventHubs["Azure Event Hubs"]
            AzureStorage["Azure Storage"]
            DataExplorer["Azure Data Explorer"]
        end
        
        CloudApps["Cloud-based applications"]
    end
    
    AzureIoTOpsPortal --- DeviceRegistry
    AzureIoTOpsPortal --- AssetsPipelines
    
    AssetsPipelines --- MicrosoftFabric
    
    MicrosoftFabric <-- "Data pipeline" --> CloudApps
    EventGrid <-- "Data pipeline" --> CloudApps
    EventHubs <-- "Data pipeline" --> CloudApps
    AzureStorage <-- "Data pipeline" --> CloudApps
    DataExplorer <-- "Data pipeline" --> CloudApps
    
    PowerBI <-- "Data visualization" --> CloudApps
    
    classDef azure fill:#CCE5FF,stroke:#0072C6;
    classDef portal fill:#FFFFCC,stroke:#999900;
    classDef service fill:#CCE5FF,stroke:#0072C6;
    classDef apps fill:#E6F2FF,stroke:#0072C6;
    
    class AzureIoTOpsPortal,DeviceRegistry,AssetsPipelines portal;
    class MicrosoftFabric,EventGrid,EventHubs,AzureStorage,DataExplorer service;
    class PowerBI,CloudApps apps;
```

### 4.1 Azure Components

- **Azure IoT Operations Experience portal**: Management interface for IoT operations
  - **Azure Device Registry management**: Device identity and management
  - **Assets and pipelines management**: Manages industrial assets and data flows

- **Cloud Processing Services**:
  - **Microsoft Fabric**: Unified analytics platform
  - **Azure Event Grid**: Event routing service
  - **Azure Event Hubs**: Big data streaming platform
  - **Azure Storage**: Scalable cloud storage
  - **Azure Data Explorer**: Data exploration service
  - **Power BI**: Business intelligence and visualization tools

- **Cloud-based applications**: Custom applications consuming IoT data

## 5. Cross-Layer Relationships

The architecture shows key integration patterns between layers:

1. **Factory to Edge**:
   - ONVIF cameras connect to ONVIF connector
   - IP cameras connect to Media connector
   - OPC UA servers connect to OPC UA connector via OPC UA communication

2. **Edge Processing**:
   - Connectors feed data to MQTT broker
   - MQTT broker connects to Dataflows for transformation
   - Dataflows handle messaging and data preparation

3. **Edge to Cloud**:
   - Processed data flows from Edge to Azure services
   - Management is bidirectional:
     - IT management spans both Edge and Azure environments
     - OT management focuses on Edge operations

4. **Data Pipeline**:
   - Edge data transforms flow to cloud services
   - Azure services provide data pipeline capabilities
   - Power BI provides visualization of processed data
   - Cloud applications consume the processed data

The architecture provides a complete IoT solution from industrial assets to cloud analytics, with edge processing to optimize data transmission and enable local operations.