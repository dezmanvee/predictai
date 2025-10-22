# Your Project Title

## Architecture Overview
```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'primaryColor':'#FF6B6B','primaryTextColor':'#fff','primaryBorderColor':'#C92A2A','lineColor':'#495057','secondaryColor':'#4ECDC4','tertiaryColor':'#FFE66D','noteBkgColor':'#F8F9FA','noteTextColor':'#212529'}}}%%

graph LR
    START([🚀 START]):::startStyle
    
    START --> INGEST[📥 Data Ingestion<br/>Caching]:::ingestStyle
    
    INGEST --> STAGE[(Staging Tables<br/>Raw API Data)]:::dbStyle
    
    STAGE --> CLEAN[🧹 Data Storage<br/>& Cleaning]:::cleanStyle
    
    CLEAN --> CORE[(Core Tables<br/>Leagues | Teams<br/>Games | Statistics)]:::coreDbStyle
    
    CORE --> FEATURE[⚙️ Feature<br/>Engineering]:::featureStyle
    
    FEATURE --> VIEWS[(Materialized Views<br/>Feature Cache)]:::viewStyle
    
    VIEWS --> TRAIN[🤖 Model Training<br/>& Persistence]:::trainStyle
    
    TRAIN --> MODEL[(Model Metadata<br/>& Artifacts)]:::modelStyle
    
    MODEL --> PREDICT[🔮 Prediction<br/>Generation]:::predictStyle
    
    VIEWS -.->|Features| PREDICT
    
    PREDICT --> PRED_TABLE[(Predictions Table<br/>Game Scores)]:::predDbStyle
    
    PRED_TABLE --> API[🌐 REST API<br/>Express.js]:::apiStyle
    
    CORE -.->|Game Info| API
    
    API --> FRONTEND[💻 React Frontend<br/>Dashboard]:::frontendStyle
    
    FRONTEND --> END([✨ END]):::endStyle
    
    %% Technology Labels
    INGEST -.->|Node.js<br/>Axios<br/>Cron| TECH1[ ]:::techLabel
    CLEAN -.->|Node.js<br/>PL/pgSQL| TECH2[ ]:::techLabel
    FEATURE -.->|PostgreSQL<br/>Stored Procs| TECH3[ ]:::techLabel
    TRAIN -.->|TensorFlow.js<br/>XGBoost| TECH4[ ]:::techLabel
    PREDICT -.->|Node.js<br/>Express| TECH5[ ]:::techLabel
    API -.->|Express<br/>JSON REST| TECH6[ ]:::techLabel
    
    classDef startStyle fill:#e74c3c,stroke:#c0392b,stroke-width:3px,color:#fff,font-weight:bold,font-size:16px
    classDef endStyle fill:#27ae60,stroke:#229954,stroke-width:3px,color:#fff,font-weight:bold,font-size:16px
    classDef ingestStyle fill:#3498db,stroke:#2980b9,stroke-width:2px,color:#fff,font-weight:bold
    classDef cleanStyle fill:#9b59b6,stroke:#8e44ad,stroke-width:2px,color:#fff,font-weight:bold
    classDef featureStyle fill:#e67e22,stroke:#d35400,stroke-width:2px,color:#fff,font-weight:bold
    classDef trainStyle fill:#1abc9c,stroke:#16a085,stroke-width:2px,color:#fff,font-weight:bold
    classDef predictStyle fill:#f39c12,stroke:#e67e22,stroke-width:2px,color:#fff,font-weight:bold
    classDef apiStyle fill:#34495e,stroke:#2c3e50,stroke-width:2px,color:#fff,font-weight:bold
    classDef frontendStyle fill:#e91e63,stroke:#c2185b,stroke-width:2px,color:#fff,font-weight:bold
    classDef dbStyle fill:#5dade2,stroke:#3498db,stroke-width:2px,color:#fff,font-style:italic
    classDef coreDbStyle fill:#48c9b0,stroke:#1abc9c,stroke-width:2px,color:#fff,font-style:italic
    classDef viewStyle fill:#f8b739,stroke:#f39c12,stroke-width:2px,color:#fff,font-style:italic
    classDef modelStyle fill:#bb8fce,stroke:#9b59b6,stroke-width:2px,color:#fff,font-style:italic
    classDef predDbStyle fill:#ec7063,stroke:#e74c3c,stroke-width:2px,color:#fff,font-style:italic
    classDef techLabel fill:none,stroke:none,color:#7f8c8d,font-size:10px
```
