# Your Project Title

## Architecture Overview
```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'primaryColor':'#FF6B6B','primaryTextColor':'#fff','primaryBorderColor':'#C92A2A','lineColor':'#495057','secondaryColor':'#4ECDC4','tertiaryColor':'#FFE66D'}}}%%

graph TB
    START([🚀 PROJECT START]):::startStyle
    
    %% Phase 1: Data Ingestion
    START --> INGEST[📥 PHASE 1: DATA INGESTION]:::phase1
    INGEST --> INGEST_PURPOSE["<b>Purpose:</b> Efficiently fetch and cache<br/>raw time-sensitive data from<br/>rate-limited free APIs"]:::purposeStyle
    INGEST_PURPOSE --> INGEST_ACTION["<b>Actions:</b> Scheduled Node.js service<br/>via cron makes API calls using Axios<br/>Fetches fixtures, historical results, odds"]:::actionStyle
    INGEST_ACTION --> INGEST_TECH["<b>Tech Stack:</b><br/>Node.js, Express.js<br/>Axios, pg package"]:::techStyle
    INGEST_TECH --> STAGE_DB[(STAGING TABLES<br/>Raw uncleaned API JSON<br/>Actively flushed after cleaning)]:::dbStyle
    
    %% Phase 2: Data Cleaning
    STAGE_DB --> CLEAN[🧹 PHASE 2: DATA STORAGE & CLEANING]:::phase2
    CLEAN --> CLEAN_PURPOSE["<b>Purpose:</b> Transform raw nested API data<br/>into normalized relational format<br/>suitable for complex analysis"]:::purposeStyle
    CLEAN_PURPOSE --> CLEAN_ACTION["<b>Actions:</b> Node.js reads staging tables<br/>Performs data type conversion<br/>Team name mapping and upserts"]:::actionStyle
    CLEAN_ACTION --> CLEAN_TECH["<b>Tech Stack:</b><br/>Node.js cleaning logic<br/>PL/pgSQL validation"]:::techStyle
    CLEAN_TECH --> CORE_DB[(CORE NORMALIZED TABLES<br/>Leagues, Teams<br/>Games, Game_Statistics<br/>Single Source of Truth)]:::coreDbStyle
    
    %% Phase 3: Feature Engineering
    CORE_DB --> FEATURE[⚙️ PHASE 3: FEATURE ENGINEERING]:::phase3
    FEATURE --> FEATURE_PURPOSE["<b>Purpose:</b> Calculate complex predictive<br/>variables capturing team form,<br/>strength and situation"]:::purposeStyle
    FEATURE_PURPOSE --> FEATURE_ACTION["<b>Actions:</b> Scheduled stored procedures<br/>Calculate rolling averages: ORtg,<br/>Last 5 Points, Opponent Strength<br/>Join data across time windows"]:::actionStyle
    FEATURE_ACTION --> FEATURE_TECH["<b>Tech Stack:</b><br/>PostgreSQL DBMS<br/>PL/pgSQL procedures"]:::techStyle
    FEATURE_TECH --> VIEWS_DB[(MATERIALIZED VIEWS<br/>Pre-calculated feature vectors<br/>Fast retrieval for training<br/>and prediction)]:::viewStyle
    
    %% Phase 4: Model Training - Horizontal Split
    VIEWS_DB --> TRAIN[🤖 PHASE 4: MODEL TRAINING]:::phase4
    TRAIN --> TRAIN_PURPOSE["<b>Purpose:</b> Use engineered features<br/>to train robust score prediction<br/>algorithm and save model artifact"]:::purposeStyle
    
    %% Horizontal branch for training details
    TRAIN_PURPOSE --> TRAIN_ACTION["<b>Actions:</b> Node.js queries materialized views<br/>Trains model: Poisson Regression or XGBoost<br/>Serializes resulting model object"]:::actionStyle
    TRAIN_ACTION --> TRAIN_TECH["<b>Tech Stack:</b><br/>Node.js training environment<br/>TensorFlow.js or ML libraries"]:::techStyle
    TRAIN_TECH --> MODEL_DB[(MODEL METADATA TABLE<br/>Version, training dates<br/>Performance metrics<br/>Model Artifact saved)]:::modelStyle
    
    %% Phase 5: Prediction Generation
    MODEL_DB --> PREDICT[🔮 PHASE 5: PREDICTION GENERATION]:::phase5
    VIEWS_DB -.->|Feature retrieval| PREDICT
    PREDICT --> PREDICT_PURPOSE["<b>Purpose:</b> Apply production-ready model<br/>to all future games and store<br/>predicted outcomes"]:::purposeStyle
    PREDICT_PURPOSE --> PREDICT_ACTION["<b>Actions:</b> Node.js prediction script<br/>runs on demand or scheduled<br/>Retrieves features and generates scores"]:::actionStyle
    PREDICT_ACTION --> PREDICT_TECH["<b>Tech Stack:</b><br/>Node.js execution<br/>Express.js prediction service"]:::techStyle
    PREDICT_TECH --> PRED_DB[(PREDICTIONS TABLE<br/>GameID, PredictedHomeScore<br/>PredictedAwayScore<br/>ConfidenceScore)]:::predDbStyle
    
    %% Phase 6: API & Frontend - Horizontal arrangement
    PRED_DB --> API[🌐 PHASE 6: MODEL SERVING]:::phase6
    CORE_DB -.->|Game metadata| API
    API --> API_PURPOSE["<b>Purpose:</b> Make validated predictions<br/>accessible via standard API<br/>for MERN frontend consumption"]:::purposeStyle
    API_PURPOSE --> API_ACTION["<b>Actions:</b> Express REST endpoint<br/>/api/v1/predictions/:gameId<br/>Queries Predictions and Games tables"]:::actionStyle
    API_ACTION --> API_TECH["<b>Tech Stack:</b><br/>Express.js REST API<br/>React.js frontend<br/>JSON data format"]:::techStyle
    
    API_TECH --> FRONTEND[💻 REACT FRONTEND DASHBOARD]:::frontendStyle
    FRONTEND --> API_RESPONSE["<b>API Response:</b><br/>JSON from Predictions table<br/>Ready for portfolio visualization"]:::responseStyle
    
    API_RESPONSE --> END([✨ PROJECT END<br/>Live Predictions Available]):::endStyle
    
    %% Styling
    classDef startStyle fill:#e74c3c,stroke:#c0392b,stroke-width:4px,color:#fff,font-weight:bold,font-size:18px
    classDef endStyle fill:#27ae60,stroke:#229954,stroke-width:4px,color:#fff,font-weight:bold,font-size:16px
    classDef phase1 fill:#3498db,stroke:#2980b9,stroke-width:3px,color:#fff,font-weight:bold,font-size:14px
    classDef phase2 fill:#9b59b6,stroke:#8e44ad,stroke-width:3px,color:#fff,font-weight:bold,font-size:14px
    classDef phase3 fill:#e67e22,stroke:#d35400,stroke-width:3px,color:#fff,font-weight:bold,font-size:14px
    classDef phase4 fill:#1abc9c,stroke:#16a085,stroke-width:3px,color:#fff,font-weight:bold,font-size:14px
    classDef phase5 fill:#f39c12,stroke:#e67e22,stroke-width:3px,color:#fff,font-weight:bold,font-size:14px
    classDef phase6 fill:#34495e,stroke:#2c3e50,stroke-width:3px,color:#fff,font-weight:bold,font-size:14px
    classDef purposeStyle fill:#ecf0f1,stroke:#bdc3c7,stroke-width:2px,color:#2c3e50,text-align:left
    classDef actionStyle fill:#d5f4e6,stroke:#27ae60,stroke-width:2px,color:#145a32,text-align:left
    classDef techStyle fill:#fdebd0,stroke:#f39c12,stroke-width:2px,color:#7d4e00,text-align:left
    classDef responseStyle fill:#fadbd8,stroke:#e74c3c,stroke-width:2px,color:#78281f,text-align:left
    classDef dbStyle fill:#5dade2,stroke:#3498db,stroke-width:3px,color:#fff,font-style:italic,font-weight:bold
    classDef coreDbStyle fill:#48c9b0,stroke:#1abc9c,stroke-width:3px,color:#fff,font-style:italic,font-weight:bold
    classDef viewStyle fill:#f8b739,stroke:#f39c12,stroke-width:3px,color:#fff,font-style:italic,font-weight:bold
    classDef modelStyle fill:#bb8fce,stroke:#9b59b6,stroke-width:3px,color:#fff,font-style:italic,font-weight:bold
    classDef predDbStyle fill:#ec7063,stroke:#e74c3c,stroke-width:3px,color:#fff,font-style:italic,font-weight:bold
    classDef frontendStyle fill:#e91e63,stroke:#c2185b,stroke-width:3px,color:#fff,font-weight:bold,font-size:14px
