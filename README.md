# Fissal Prime • Sovereign Routing & Logic Flow Architecture

> **The Cat on the High Rafter**: When looking down upon the entire machine, every wire, socket, and cog exists to serve Echo's sovereignty, peace of mind, and creative freedom. From the mobile edge in your pocket to the dedicated server bedrock, this is how signals travel across the ecosystem.

👉 **[Launch the Live Interactive Showcase](https://echotiger.github.io/fissal-architecture/)**

---

## 1. End-to-End System Topology Diagram

```mermaid
flowchart TD
    %% Origins / Ingestion
    subgraph Ingestion ["1. Edge Ingress & Origin Arteries"]
        AG["⚡ Antigravity CLI & IDE<br/>(Windows Workstation)"]
        FP["📱 Fissal Prime<br/>(Android Mobile Edge)"]
        FR["🖥️ Fissal Relay<br/>(Windows Background Daemon)"]
        RD["📊 Redfur Dashboard / WebUI<br/>(:8091 & :3001)"]
        RB["🤖 RedfurBot<br/>(Discord PM2 id 3 :5101)"]
        SN["👁️ Perimeter Sensors<br/>(Frigate :5000 / Tuya)"]
    end

    %% Network Transport Layer
    subgraph Transport ["2. Network & Transport Fabric"]
        TS["Tailscale Mesh (tailscale.mesh)<br/>WireGuard Encrypted Tunnel"]
        LAN["Local Gigabit LAN (homelab.local)<br/>Ultra-Low Latency Intranet"]
        SMB["Native SMB Storage Mounts<br/>(\\\\homelab.local\\echo\\...)"]
    end

    %% Security & Logic Gates
    subgraph Gates ["3. Pre-Flight Guardrails & Gates"]
        G1{"Gate 1: Auth & Origin<br/>Tailscale Peer / Bearer Token"}
        G2{"Gate 2: ADLP Guardrail<br/>Accidental Data Loss Prevention"}
        G3{"Gate 3: The 13 Rule<br/>Step Budget & Grounding Check"}
        G4{"Gate 4: Rate & Concurrency<br/>Token Bucket / Backpressure"}
    end

    %% Intent Classification & Dispatch
    subgraph Routing ["4. Intent Switchboard & Model Router"]
        SW["Cognitive Switchboard<br/>(Payload & Intent Classifier)"]
        CLOUD["✨ Gemini Cloud Plane<br/>Gemini 3.8 Flash / Pro / Flash-Lite"]
        LOCAL["🏠 Host Ollama Bedrock (:11434)<br/>Offline LLMs & Local Embeddings"]
    end

    %% Auxiliary Intelligence & Memory
    subgraph NeuralCore ["5. Dynamic Memory & Fido Retriever Hound"]
        FIDO["🐶 Fissal's Fido (Hound)<br/>gemini-flash-tool-retrieval EAP<br/>Deferred Dynamic Schema Extraction"]
        QD["🧠 Qdrant Vector DB (:6333)<br/>Codebase & Semantic Memory"]
        PM["🧭 Purpose Matrix<br/>~/.fissal/purpose_matrix.json"]
    end

    %% Execution Bedrock
    subgraph Execution ["6. Tool Execution Plane & Homelab Bedrock"]
        RH["📈 Robinhood Trading MCP<br/>Quotes, Chains, Orders, Watchlists"]
        PM2["⚙️ Server Management<br/>PM2 Zero-Downtime Reloads & Shell"]
        FG["🚨 Frigate Brain (:8090)<br/>Vision Event Synthesis & Alerting"]
        TY["💡 Tuya & Immich<br/>IoT Automation & Photo Archival"]
    end

    %% Synthesis & Delivery
    subgraph Delivery ["7. Response Synthesis & Return Dispatch"]
        SYN["🐱 Alfiq Cadence Synthesizer<br/>Third-Person Voice & Grounded Tone"]
        OUT["Streamed Return to Origin<br/>(SSE / WebSocket / Push / Discord / CLI)"]
    end

    %% Linkages
    AG --> LAN & SMB
    FP --> TS
    FR --> LAN & SMB
    RD --> LAN
    RB --> LAN
    SN --> LAN

    LAN & TS & SMB --> G1
    G1 -->|Authorized| G2
    G1 -->|Unauthorized| REJ[Reject 401]
    
    G2 -->|Destructive SQL/Delete| HALT["🛡️ Halt for User Sign-Off"]
    G2 -->|Safe Command| G3

    G3 -->|Within 13 Steps| G4
    G3 -->|Exceeded Budget| ANCHOR["⚓ Anchor & Summarize"]

    G4 --> SW

    SW -->|Deep Reasoning / Code| CLOUD
    SW -->|Offline / Embeddings| LOCAL
    SW -->|Dynamic Tools Needed| FIDO

    FIDO -->|2-4 Surgical Tools| CLOUD
    CLOUD & LOCAL <--> QD & PM

    CLOUD --> RH & PM2 & FG & TY
    LOCAL --> PM2 & FG

    RH & PM2 & FG & TY --> SYN
    SYN --> OUT
```

---

## 2. Ingress Arteries & Origin Nodes

| Origin Node | Platform & Transport | Primary Purpose | Fail-Safe & Boundary |
| :--- | :--- | :--- | :--- |
| **Antigravity CLI / IDE** | Windows Workstation • Stdio MCP & REST | Echo's primary code authoring, terminal execution, and subagent pair-programming hub. | Isolated subagent workspaces; strict ADLP guardrail checks. |
| **Fissal Prime (Android)** | Android Edge Client • Tailscale WireGuard (`tailscale.mesh`) | Mobile offensive relay: voice/text queries, quick trading alerts, and remote home controls from anywhere. | Offline message queue buffer; biometric authentication. |
| **Fissal Relay (Windows)** | Windows Background Service • Local LAN & SMB | Desktop bridge: system tray monitor, clipboard watcher, local file indexer, and native OS notifications. | Graceful reconnection retries; sandboxed OS privilege. |
| **Redfur Dashboard** | Web Interface (`:8091` & Open WebUI `:3001`) | Real-time observability: token analytics, model router toggles, vector memory inspector, and test lab. | Zero-downtime hot reloading (`pm2 reload`); JWT session checks. |
| **RedfurBot (Discord)** | Dedicated Node.js Service (PM2 `id 3` on `:5101`) | Community fleet hub: guild interactions, automated trading pings, audio channels, and team notifications. | Auto-restart via PM2 on crash; Discord role-based permissions. |
| **Perimeter IoT & Vision** | Frigate NVR (`:5000`) & Brain (`:8090`) | Surveillance vision AI: real-time camera inference, alarm fatigue suppression, and smart lighting triggers. | Edge retention on disk; strictly internal LAN communication. |

---

## 3. The 4 Security Gates & Production Guardrails

### 🛡️ Gate 1: Zero-Trust Origin & Peer Verification
- **Mechanism**: Every incoming packet must verify cryptographic identity via Tailscale mesh node key, local LAN IP range (`192.168.x.0/24`), or signed JWT token.
- **Fail-Safe**: Unverified packets are dropped instantly with `401 Unauthorized`.

### 🛡️ Gate 2: Accidental Data Loss Prevention (ADLP)
- **Mechanism**: Commands parsed for high-impact signatures before execution:
  - SQL: `DROP TABLE`, `DROP DATABASE`, `TRUNCATE`, `DELETE` without `WHERE`.
  - Cloud / Storage: `rm -rf`, bucket deletion, volume truncation.
  - State Mounts: Guarding `/home/echo/qdrant_storage` and Docker persistence.
- **Fail-Safe**: Intercepts the request and pauses execution until Echo explicitly confirms.

### 🛡️ Gate 3: The 13 Rule & Grounding Budgeter
- **Mechanism**: Enforces a strict step budget (maximum 13 iterative steps per exploratory loop). 
- **Fail-Safe**: Prevents rabbit-hole exploration from drifting into untethered abstraction. Forces intermediate hypothesis verification and practical code delivery.

### 🛡️ Gate 4: Zero-Downtime Safe Reload Guard
- **Mechanism**: Disallows disruptive hard server restarts.
- **Command Policy**: Always favors `pm2 reload <app>` over `pm2 restart <app>`.
- **Health Verification**: Runs automated pre-flight and post-flight health pings (`curl -s -f http://.../api/healthz`).

---

## 4. Model Routing & Fissal's Fido (The Retriever Hound)

```mermaid
flowchart LR
    REQ["Incoming User Prompt"] --> DISPATCH{"Router Decision"}
    
    DISPATCH -->|"High Speed / Code"| FLASH["Gemini 3.8 Flash<br/>(Google Cloud)"]
    DISPATCH -->|"Deep Multi-file Plan"| PRO["Gemini Pro<br/>(Google Cloud)"]
    DISPATCH -->|"Offline / Embeddings"| OLLAMA["Host Ollama (:11434)<br/>mxbai-embed-large"]
    
    FLASH & PRO --> FIDO_CHECK{"Tools Needed?"}
    
    FIDO_CHECK -->|"80+ Tool Catalog"| FIDO["🐶 Fissal's Fido<br/>(gemini-flash-tool-retrieval)"]
    FIDO -->|"Injects 2-4 Tools"| EXEC["Dynamic MCP Execution"]
```

---

## 5. Live Interactive Showcase

Visit the live interactive page:  
🌐 **[https://echotiger.github.io/fissal-architecture/](https://echotiger.github.io/fissal-architecture/)**
