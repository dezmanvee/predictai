# PredictAI - Football Match Prediction Platform

```mermaid
flowchart TD
    %% === PHASE 1 ===
    A["
    **1️⃣ Data Ingestion (Extraction & Caching)**  
    🧭 *Purpose:* Efficiently fetch and temporarily store raw, time-sensitive data from rate-limited APIs.  
    ⚙️ *Logic:* Scheduled Node.js service (cron job) calls APIs via Axios to fetch fixtures, results, and odds.  
    🧰 *Tech:* Node.js, Express.js, Axios, pg (DB Connector).  
    🗄️ *PostgreSQL:* Staging Tables – store raw, uncleaned API JSON rows, flushed after cleaning.
    "] --> B

    %% === PHASE 2 ===
    B["
    **2️⃣ Data Storage & Cleaning**  
    🧭 *Purpose:* Transform raw API data into normalized relational form for analysis.  
    ⚙️ *Logic:* Node.js script reads staging tables, standardizes data (team mapping), and upserts into normalized tables.  
    🧰 *Tech:* Node.js, PL/pgSQL (validation & insertion).  
    🗄️ *PostgreSQL:* Core Tables – Leagues, Teams, Games, Game_Statistics.
    "] --> C

    %% === PHASE 3 ===
    C["
    **3️⃣ Feature Engineering**  
    🧭 *Purpose:* Generate predictive variables (features) describing team form, strength, and context.  
    ⚙️ *Logic:* Stored Procedures calculate rolling averages (e.g., ORtg, last 5 points, opponent strength) using joins & time windows.  
    🧰 *Tech:* PostgreSQL, PL/pgSQL.  
    🗄️ *PostgreSQL:* Materialized Views – store precomputed, flattened feature vectors.
    "] --> D

    %% === PHASE 4 ===
    D["
    **4️⃣ Model Training & Persistence**  
    🧭 *Purpose:* Train score prediction model and save the resulting artifact.  
    ⚙️ *Logic:* Node.js training script queries Materialized Views, fits ML model (e.g., Poisson Regression / XGBoost), and serializes it.  
    🧰 *Tech:* Node.js, TensorFlow.js / ML libraries.  
    🗄️ *PostgreSQL:* Model_Metadata table – version, training date, performance.  
    💾 *Artifact:* Serialized model stored in disk or table.
    "] --> E

    %% === PHASE 5 ===
    E["
    **5️⃣ Prediction Generation**  
    🧭 *Purpose:* Apply trained model to predict future (score-null) games.  
    ⚙️ *Logic:* Node.js script retrieves features for upcoming games from Materialized Views, loads model, and predicts outcomes.  
    🧰 *Tech:* Node.js, Express.js, PostgreSQL.  
    🗄️ *PostgreSQL:* Predictions Table – GameID, PredictedHomeScore, PredictedAwayScore, ConfidenceScore.
    "] --> F

    %% === PHASE 6 ===
    F["
    **6️⃣ Model Serving (API & Frontend)**  
    🧭 *Purpose:* Serve predictions via REST API and visualize results on frontend.  
    ⚙️ *Logic:* Express defines /api/v1/predictions/:gameId endpoint → React frontend consumes API → renders dashboard.  
    🧰 *Tech:* Express.js (API), React.js (Frontend), JSON (Data Format).  
    🗄️ *PostgreSQL:* Serves JSON data directly from Predictions Table.
    "]

    %% === STYLING ===
    classDef phase fill:#eef6ff,stroke:#0366d6,stroke-width:1.5px,color:#0d1117,font-size:13px;
    class A,B,C,D,E,F phase;
