```mermaid
flowchart TD
    %% === DATA INGESTION PHASE ===
    A1(["🏗️ **Data Ingestion (Extraction & Caching)**"])
    A2["🧭 *Purpose:* Fetch & temporarily store raw, time-sensitive data from APIs.  
    ⚙️ *Logic:* Scheduled Node.js cron job fetches data (fixtures, odds) via Axios.  
    🧰 *Tech:* Node.js, Express.js, Axios, pg (DB Connector)."]
    A3["🗄️ *PostgreSQL Object:* Staging Tables → store raw, uncleaned JSON rows."]

    %% === DATA CLEANING PHASE ===
    B1(["🧹 **Data Storage & Cleaning**"])
    B2["🧭 *Purpose:* Normalize API data into relational format.  
    ⚙️ *Logic:* Node.js script standardizes data, performs upserts to normalized tables.  
    🧰 *Tech:* Node.js, PL/pgSQL. "]
    B3["🗄️ *PostgreSQL Object:* Core Tables → Leagues, Teams, Games, Game_Statistics."]

    %% === FEATURE ENGINEERING ===
    C1(["🧮 **Feature Engineering**"])
    C2["🧭 *Purpose:* Generate predictive variables capturing team form & performance.  
    ⚙️ *Logic:* Stored procedures compute rolling averages, opponent strength, etc.  
    🧰 *Tech:* PostgreSQL, PL/pgSQL."]
    C3["🗄️ *PostgreSQL Object:* Materialized Views → precomputed feature vectors."]

    %% === MODEL TRAINING ===
    D1(["🤖 **Model Training & Persistence**"])
    D2["🧭 *Purpose:* Train and save score prediction model.  
    ⚙️ *Logic:* Node.js script queries feature views, trains Poisson/XGBoost model, serializes artifact.  
    🧰 *Tech:* Node.js, TensorFlow.js, ML libraries."]
    D3["🗄️ *PostgreSQL Object:* Model_Metadata Table → version, date, metrics.  
    💾 *Artifact:* Saved model file or DB blob."]

    %% === PREDICTION GENERATION ===
    E1(["📈 **Prediction Generation**"])
    E2["🧭 *Purpose:* Apply trained model to predict scores for future games.  
    ⚙️ *Logic:* Node.js script loads model, retrieves features, generates predictions.  
    🧰 *Tech:* Node.js, Express.js, PostgreSQL."]
    E3["🗄️ *PostgreSQL Object:* Predictions Table → GameID, Scores, Confidence."]

    %% === MODEL SERVING ===
    F1(["🌐 **Model Serving (API & Frontend)**"])
    F2["🧭 *Purpose:* Expose predictions via REST API and visualize on frontend.  
    ⚙️ *Logic:* Express.js defines /api/v1/predictions/:gameId → React consumes → Dashboard.  
    🧰 *Tech:* Express.js (API), React.js (Frontend), JSON."]
    F3["🗄️ *PostgreSQL Object:* API Response → JSON served from Predictions Table."]

    %% === FLOW CONNECTIONS (CURVY) ===
    A1-.->A2-->A3===>B1-.->B2-->B3===>C1-.->C2-->C3===>D1-.->D2-->D3===>E1-.->E2-->E3===>F1-.->F2-->F3

    %% === STYLING ===
    classDef ingestion fill:#e6f0ff,stroke:#1b6ec2,stroke-width:2px,color:#0a1c2f,font-weight:bold;
    classDef cleaning fill:#e8f8f0,stroke:#1b9e77,stroke-width:2px,color:#073b24,font-weight:bold;
    classDef features fill:#fff6e6,stroke:#ffb347,stroke-width:2px,color:#5a3e00,font-weight:bold;
    classDef training fill:#fde7eb,stroke:#c41e3a,stroke-width:2px,color:#4a0d14,font-weight:bold;
    classDef prediction fill:#e8f6fa,stroke:#007f9f,stroke-width:2px,color:#003844,font-weight:bold;
    classDef serving fill:#f4f4f6,stroke:#444c56,stroke-width:2px,color:#1b1f23,font-weight:bold;

    class A1,A2,A3 ingestion;
    class B1,B2,B3 cleaning;
    class C1,C2,C3 features;
    class D1,D2,D3 training;
    class E1,E2,E3 prediction;
    class F1,F2,F3 serving;

    %% === TITLE ===
    title "⚙️ Full Software Project Lifecycle — From Data Ingestion to Frontend Delivery"
