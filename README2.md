

# 💡 PROJECT

## **Helios-Grid — Decentralized Multi-Agent Reinforcement Learning Platform for Peer-to-Peer Energy Balancing with Graph-Neural Coordination in Simulated Smart Grids**

---

### Global Problem Being Solved

The global energy grid is undergoing a fundamental structural transformation — from centralized generation (few large power plants) to **distributed generation** (millions of rooftop solar panels, residential batteries, small wind turbines). Yet the software infrastructure managing this grid remains stubbornly centralized, creating a catastrophic mismatch. The International Energy Agency reports that **~7% of all renewable energy generated globally is curtailed (wasted)** because the grid cannot absorb it at the point of production when local demand is low. Simultaneously, peak demand periods force utilities to activate expensive, carbon-intensive "peaker" gas plants.

The core problem is **local imbalance at scale**: one neighborhood has surplus solar at noon while the adjacent neighborhood runs air conditioners at full load, yet no mechanism exists for real-time, autonomous, peer-to-peer energy redistribution at the neighborhood level. Current smart grid solutions rely on **centralized optimization** — a utility control center collects all data, computes optimal dispatch, and sends commands. This approach fails at scale: it creates single points of failure, introduces latency that makes real-time response impossible, raises privacy concerns (households must share granular consumption data), and cannot adapt to the **non-stationary dynamics** where millions of households simultaneously change behavior.

The result: wasted renewable energy, unnecessary carbon emissions, grid instability during peak loads, and consumer electricity costs that don't reflect the true real-time value of energy. The U.S. Department of Energy estimates that intelligent local energy balancing could **reduce peak grid load by 15–25%** and **prevent $10–15 billion annually** in grid infrastructure costs.

---

### Innovative Concept

Helios-Grid constructs a **decentralized Virtual Power Plant (VPP)** — a software platform where each household (or building) operates as an **autonomous RL agent** that learns to produce, consume, store, and trade energy optimally, coordinating with neighboring agents through peer-to-peer communication without any central authority.

The platform introduces three key innovations:

1. **Cooperative Multi-Agent RL with Communication**: Each household agent runs a lightweight PPO policy that decides actions every 15 minutes: consume from solar, charge battery, discharge battery, buy energy from peers, sell energy to peers, or draw from the main grid. Critically, agents don't operate in isolation — they exchange **learned communication messages** via a Graph Neural Network that encodes the local grid topology, enabling emergent cooperative strategies without explicit programming of cooperation rules.

2. **Graph-Neural Coordination Protocol (GNCP)**: The neighborhood's electrical topology is modeled as a graph where nodes are households and edges are electrical connections (with capacity constraints, line losses, transformer limits). A GNN operates on this graph to produce **coordination embeddings** — compact vectors that each agent receives, encoding the aggregate state and intentions of its neighbors. This solves the fundamental MARL challenge of **non-stationarity**: each agent's environment changes because other agents act simultaneously, but the GNN provides a stable, informative signal about the collective state.

3. **Decentralized Energy Credit Market**: Agents trade energy through a software-simulated **peer-to-peer market** using a double-auction mechanism. Prices emerge from supply/demand dynamics rather than being set centrally. An optional blockchain layer (lightweight EVM sidechain) provides transparent, tamper-proof settlement of energy credits, demonstrating how such a system could operate in a trust-minimized real-world deployment.

The breakthrough is demonstrating that **decentralized RL agents with graph-neural communication can achieve near-optimal grid balancing** — matching or exceeding centralized optimization — while being inherently scalable, privacy-preserving (no central data collection), and robust to agent failures.

---

### Technical Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│                        FRONTEND LAYER                                │
│  Next.js 14 + React + Three.js + Recharts + TailwindCSS              │
│                                                                      │
│  ┌────────────────────┐ ┌───────────────┐ ┌────────────────────────┐ │
│  │ 3D Neighborhood    │ │ Energy Flow   │ │ Market & Agent         │ │
│  │ Digital Twin       │ │ Dashboard     │ │ Analytics Panel        │ │
│  │ (Three.js)         │ │ (Recharts)    │ │                        │ │
│  │                    │ │               │ │ - Per-agent reward     │ │
│  │ Houses w/ solar,   │ │ - Generation  │ │ - P2P trade volume     │ │
│  │ batteries, grid    │ │ - Consumption │ │ - Price dynamics       │ │
│  │ connections.       │ │ - Storage     │ │ - Policy convergence   │ │
│  │ Color = net energy │ │ - Grid import │ │ - Carbon savings       │ │
│  │ Flow animations on │ │ - P2P trades  │ │ - Cooperative vs.      │ │
│  │ edges.             │ │ - Curtailment │ │   selfish comparison   │ │
│  └────────────────────┘ └───────────────┘ └────────────────────────┘ │
│  ┌──────────────────────────────────────────────────────────────────┐ │
│  │  Simulation Control: Time speed, weather injection, blackout    │ │
│  │  stress test triggers, agent policy toggles (selfish/coop/mix)  │ │
│  └──────────────────────────────────────────────────────────────────┘ │
│  WebSocket (Socket.IO) for real-time simulation state streaming      │
└────────────────────────────┬─────────────────────────────────────────┘
                             │ REST + WebSocket
┌────────────────────────────▼─────────────────────────────────────────┐
│                      API GATEWAY (FastAPI)                            │
│              Auth, rate limiting, simulation management               │
└──┬──────────┬──────────┬──────────┬──────────┬──────────────────────┘
   │          │          │          │          │
   ▼          ▼          ▼          ▼          ▼
┌───────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌──────────────────┐
│Grid   │ │Agent   │ │GNN     │ │Market  │ │Analytics &       │
│Environ│ │Runtime │ │Coord.  │ │Engine  │ │Evaluation        │
│Service│ │Service │ │Service │ │Service │ │Service           │
│       │ │        │ │        │ │        │ │                  │
│Gym env│ │N PPO   │ │Graph   │ │Double  │ │Grid stability,   │
│Weather│ │agents, │ │Neural  │ │auction │ │carbon metrics,   │
│+ Load │ │action  │ │Network │ │P2P     │ │peak shaving,     │
│+ Solar│ │select, │ │coord.  │ │trading,│ │comparative       │
│simul. │ │battery │ │embed-  │ │price   │ │analysis          │
│       │ │mgmt    │ │dings   │ │form.   │ │                  │
└──┬────┘ └──┬─────┘ └──┬─────┘ └──┬─────┘ └──┬───────────────┘
   │         │          │          │           │
   └─────────┴──────────┴──────────┴───────────┘
                        │
            ┌───────────▼───────────┐
            │  EVENT BUS (RabbitMQ) │
            │  Queues:              │
            │  - sim.tick           │
            │  - agent.actions      │
            │  - agent.observations │
            │  - market.orders      │
            │  - market.settlements │
            │  - grid.state         │
            │  - weather.updates    │
            └───────────┬───────────┘
                        │
   ┌────────────────────┼────────────────────┐
   ▼                    ▼                    ▼
┌──────────┐     ┌──────────────┐     ┌──────────┐
│ PostGIS  │     │ TimescaleDB  │     │ Redis    │
│ (Grid    │     │ (Time-series │     │ (Agent   │
│ topology,│     │  energy data,│     │  state   │
│ spatial) │     │  market logs)│     │  cache,  │
│          │     │              │     │  pub/sub)│
└──────────┘     └──────────────┘     └──────────┘
                        │
            ┌───────────▼───────────┐
            │  OPTIONAL: Blockchain │
            │  Layer (Hardhat local │
            │  EVM / Polygon Edge)  │
            │  Smart contracts for  │
            │  energy credit settle.│
            └───────────────────────┘
```

**Frontend Highlights:**
- **Three.js / React Three Fiber**: 3D digital twin of a neighborhood — houses with animated solar panels (brightness ∝ generation), battery indicators (fill level), power flow animations on grid connection edges (green = selling, red = buying, thickness ∝ volume), weather overlays (sun position, cloud cover). Color-coded houses: blue = net exporter, orange = net importer, green = self-sufficient.
- **Recharts**: Real-time stacked area charts (generation/consumption/storage/grid/P2P breakdown per agent and aggregate), price curves from the P2P market, carbon emission timeline.
- **Simulation Control**: time acceleration (1x–1000x), manual weather event injection (sudden cloud cover, heat wave), "blackout stress test" button (disconnects main grid, forces P2P-only operation), toggle between agent policy modes for live A/B comparison.

**Backend Microservices (Docker Compose):**

1. **Grid Environment Service** (FastAPI + Gymnasium):
   - Implements a custom **multi-agent Gymnasium environment** representing the electrical grid
   - Manages physics simulation: solar generation (irradiance × panel area × efficiency), battery charge/discharge dynamics (SoC limits, round-trip efficiency losses, degradation), household load profiles, grid connection constraints (transformer capacity, line losses)
   - Weather engine: ingests historical data or generates synthetic weather scenarios
   - Computes grid-level physics: voltage stability approximations, power flow feasibility, line congestion
   - Publishes state updates to RabbitMQ `sim.tick`

2. **Agent Runtime Service** (FastAPI + PyTorch):
   - Manages N household agents (N = 50–500), each with an independent PPO policy network
   - Per-agent observation: own solar generation, battery SoC, current load, time-of-day, electricity price, **GNN coordination embedding** from neighbors
   - Per-agent action space (discrete): {consume solar, charge battery, discharge battery, sell to P2P market at price tier, buy from P2P market at price tier, import from main grid}
   - Supports three training modes: **Independent PPO** (selfish), **CTDE (Centralized Training Decentralized Execution)** with shared critic, **GNCP-enhanced** (full cooperative with graph communication)
   - Publishes actions to `agent.actions`, receives observations from `agent.observations`

3. **GNN Coordination Service** (FastAPI + PyTorch Geometric):
   - Maintains the grid topology graph (from PostGIS)
   - At each timestep, collects agent state embeddings and runs a **Message-Passing GNN** (GraphSAGE or GAT) to produce per-agent coordination embeddings
   - These embeddings encode: aggregate neighbor surplus/deficit, local congestion signals, price pressure from nearby market activity
   - Enables agents to make informed decisions based on neighborhood context without centralized data collection

4. **Market Engine Service** (FastAPI):
   - Implements a **continuous double auction** for P2P energy trading
   - Sellers post ask orders (energy amount, minimum price), buyers post bid orders (energy amount, maximum price)
   - Matching engine clears trades at intersection price, respecting grid topology constraints (can't trade across a congested transformer)
   - Optional blockchain settlement: energy credits recorded on a local Hardhat EVM or Polygon Edge sidechain via Solidity smart contracts
   - Publishes settlements to `market.settlements`

5. **Analytics & Evaluation Service** (FastAPI):
   - Computes real-time and aggregate metrics: grid stability index, carbon footprint, peak shaving efficiency, Gini coefficient of energy costs (equity), social welfare
   - Runs comparative analysis between policy modes
   - Generates evaluation reports and exports data for research paper figures

---

### Algorithms / Models Involved

| Component | Algorithm/Model | Purpose |
|-----------|----------------|---------|
| Agent Policy | **Proximal Policy Optimization (PPO)** | Learn per-agent energy management policies (produce/consume/store/trade decisions) |
| Multi-Agent Training | **CTDE with Shared Critic** (MAPPO variant) | Centralized value function during training enables cooperative learning while maintaining decentralized execution |
| Graph Coordination | **GraphSAGE / GAT** (PyTorch Geometric) | Produce coordination embeddings by aggregating neighbor states on the grid topology graph |
| Communication Learning | **CommNet / TarMAC** inspired learned messages | Agents learn *what* to communicate to neighbors, not just *how* to act — emergent communication protocols |
| Market Mechanism | **Continuous Double Auction** (CDA) | Decentralized price discovery for P2P energy trading with supply-demand equilibrium |
| Demand Forecasting | **LSTM / Temporal Fusion Transformer** | Per-agent short-term load and solar generation forecasting (1–4 hour horizon) for proactive decisions |
| Grid Physics | **DC Power Flow Approximation** | Computationally tractable grid physics (voltage, line flows) that constrains feasible energy trades |
| Baseline Optimization | **Mixed-Integer Linear Programming (MILP)** via PuLP/OR-Tools | Centralized optimal benchmark — computes globally optimal dispatch with perfect information for comparison |
| Fairness Analysis | **Shapley Value Approximation** | Attribute cooperative surplus to individual agents for fair reward distribution |
| Stability Analysis | **Lyapunov-based convergence metrics** | Analyze whether the multi-agent system converges to stable equilibrium |

---

### Data Simulation Strategy

All data is software-simulated using public datasets and statistical models — no hardware required:

1. **Solar Generation Simulation:**
   - Historical weather data from **NASA POWER API** (free, global, hourly irradiance, temperature, wind speed, cloud cover — 20+ years)
   - Solar panel model: `P = A × η × G × (1 - β(T - 25°C))` where A=panel area, η=efficiency (15–22%), G=irradiance, β=temperature coefficient
   - Per-household variation: random panel orientation (±30° azimuth), partial shading profiles, panel degradation age factors
   - Synthetic scenarios: sudden cloud cover events, seasonal variation, extreme heat reducing panel efficiency

2. **Household Load Simulation:**
   - **Pecan Street Dataport** (public academic dataset, 1-minute resolution energy consumption for 1,000+ homes in Austin, TX — multi-year)
   - Statistical load profile generator: cluster Pecan Street homes into archetypes (small apartment, family home, large home with EV, home office), fit mixture models, sample synthetic households
   - Temporal patterns: morning/evening peaks, weekend vs. weekday, seasonal HVAC variation
   - Stochastic events: EV charging (random arrival times), appliance spikes (oven, dryer), occupancy variation

3. **Battery Storage Simulation:**
   - Models based on Tesla Powerwall / BYD specs: 5–13.5 kWh capacity, 5 kW max charge/discharge rate, 90% round-trip efficiency, 0.05%/day self-discharge
   - State-of-Charge (SoC) constraints: 10% minimum, 90% maximum for longevity
   - Degradation model: cycle-count-based capacity fade (simplified)

4. **Grid Topology Simulation:**
   - Generates realistic neighborhood grid topologies: radial distribution networks with 1–3 transformers serving 50–200 households
   - Transformer capacity limits (100–500 kVA), line impedance values from IEEE test feeder models
   - Stored in PostGIS for spatial queries and graph extraction

5. **Electricity Price Simulation:**
   - Historical wholesale prices from **EIA (Energy Information Administration)** API — hourly, regional
   - Time-of-use tariff structures from real utility rate schedules (PG&E, ConEd)
   - Feed-in tariff rates for solar export

6. **Stress Test Scenarios:**
   - **Cloudy day**: sudden 80% irradiance drop across all agents
   - **Heat wave**: 40% load increase (HVAC) + solar efficiency reduction
   - **Grid disconnection**: main grid unavailable for 2–6 hours (islanding test)
   - **Demand surge**: 50% of households simultaneously start EV charging
   - **Asymmetric generation**: half the neighborhood has solar, half doesn't

---

### Research Gap This Project Addresses

1. **MARL non-stationarity in energy systems is unsolved**: In multi-agent energy trading, each agent's action changes the environment for all others (buying energy raises prices, selling lowers them). Standard independent RL fails because each agent perceives a non-stationary environment. Existing solutions either use fully centralized optimization (not scalable/private) or simplistic rule-based trading. Helios-Grid's GNN coordination protocol provides a principled middle ground — decentralized execution with learned coordination.

2. **No graph-aware communication for energy MARL**: While GNNs have been applied to power systems (load forecasting, fault detection), and MARL has been applied to energy trading, **no existing work combines GNN-based topology-aware communication with MARL for P2P energy balancing**. The grid's physical topology constrains which trades are feasible — the GNN naturally encodes these constraints.

3. **Cooperative vs. selfish behavior is not empirically characterized**: Game-theoretic analysis suggests cooperative strategies dominate, but no large-scale empirical study compares independent (selfish) RL agents vs. cooperatively trained agents vs. communication-enhanced agents in realistic energy environments with heterogeneous households.

**Key references:**
- Chen & Su, "Peer-to-Peer Energy Trading with RL" (Applied Energy, 2021) — independent agents only, no communication, small scale
- Qiu et al., "MARL for Microgrid Energy Management" (IEEE TSG, 2023) — centralized critic but no graph awareness
- Kody et al., "GNN for Optimal Power Flow" (IEEE TPWRS, 2022) — GNN for grid but no RL agents or trading

---

### Experimental Methodology

1. **Baselines** (5 methods):
   - **Centralized MILP** (globally optimal with perfect information — upper bound)
   - **Rule-based self-consumption** (maximize own solar use, sell surplus at fixed feed-in tariff — industry standard)
   - **Independent PPO** (each agent learns selfishly, no communication)
   - **MAPPO with shared critic** (CTDE, cooperative but no graph awareness)
   - **Helios-Grid full** (MAPPO + GNN coordination + P2P market)

2. **Ablation Studies** (4 configurations):
   - Full system without GNN (MAPPO + market, no coordination embeddings)
   - Full system without P2P market (GNN coordination but fixed pricing)
   - Full system without battery storage (solar + grid only)
   - Full Helios-Grid

3. **Scale Experiments**:
   - Grid sizes: 25, 50, 100, 200, 500 households
   - Measure: does performance scale? Does training converge? How does communication overhead grow?

4. **Heterogeneity Experiments**:
   - Uniform households (all identical) vs. heterogeneous (mix of archetypes, varying solar capacity 0–10 kW, battery 0–13.5 kWh)
   - Measure: does cooperation benefit heterogeneous communities more?

5. **Stress Test Campaign**: Run all 6 stress test scenarios, compare resilience across 5 methods

6. **Fairness Analysis**: Compute Gini coefficient of electricity costs and Shapley values — does cooperation create equitable outcomes or exploit some agents?

7. **Emergent Behavior Analysis**: Visualize learned communication messages — what information do agents learn to share? Do spatial trading patterns emerge (neighborhood energy clusters)?

---

### Evaluation Metrics

| Category | Metric | Target |
|----------|--------|--------|
| **Grid Efficiency** | Peak-Load Shaving Efficiency (% peak reduction vs. no-control) | ≥ 20% |
| | Renewable Energy Utilization Rate (% of generated solar consumed locally) | ≥ 85% |
| | Curtailment Rate (% of solar wasted) | ≤ 5% |
| **Environmental** | Carbon Footprint Reduction vs. rule-based baseline | ≥ 25% |
| | Grid import reduction (kWh saved from fossil-powered grid) | ≥ 30% |
| **Economic** | Average household electricity cost reduction | ≥ 15% |
| | Gini coefficient of costs (fairness, lower = more equal) | ≤ 0.25 |
| **Grid Stability** | Voltage deviation index (from nominal, lower = more stable) | ≤ 3% |
| | Transformer overload events (count per simulated month) | ≤ 2 |
| **Optimality Gap** | Performance gap vs. centralized MILP optimal | ≤ 12% |
| **MARL Quality** | Training convergence (episodes to stable reward) | ≤ 100K |
| | Policy stability (reward variance in last 10K episodes) | ≤ 5% |
| **Scalability** | Per-timestep computation time at 500 agents | ≤ 2 sec |
| **Resilience** | Performance during grid disconnection (% deliveries maintained via P2P) | ≥ 70% |
| | Recovery time after stress event | ≤ 30 min (sim) |

---

### Prototype Development Plan

**Phase 1 — Simulation Foundation (Weeks 1–7): "The Grid"**

- Build Grid Environment Service: custom Gymnasium multi-agent environment with solar, battery, load, and grid physics simulation
- Data pipeline: NASA POWER API integration for weather, Pecan Street dataset processing for load profiles, grid topology generator (IEEE test feeders)
- Implement PostGIS grid topology storage and TimescaleDB for time-series energy data
- Set up RabbitMQ event bus with simulation tick orchestration
- Build basic Agent Runtime: simple rule-based agents (self-consumption maximization) as functional baseline
- Create Next.js dashboard: Three.js 3D neighborhood (static houses with solar panels, battery indicators), basic Recharts energy flow charts, simulation time controls
- **Deliverable:** Functional grid simulation with rule-based agents, real-time 3D visualization of energy flows

**Phase 2 — Multi-Agent Intelligence (Weeks 8–15): "The Swarm"**

- Implement PPO agent training pipeline (PyTorch + Stable-Baselines3 / RLlib)
- Train Independent PPO agents (selfish baseline)
- Implement GNN Coordination Service: GraphSAGE/GAT on grid topology (PyTorch Geometric), coordination embedding pipeline
- Train MAPPO with shared critic + GNN coordination (full cooperative model)
- Build Market Engine: continuous double auction, order matching, settlement logging
- Optional: Hardhat local blockchain with Solidity energy credit smart contract
- Implement all 5 baselines including centralized MILP (PuLP/OR-Tools)
- Upgrade dashboard: animated energy flow on graph edges, P2P trade visualization, market price charts, agent policy comparison mode
- **Deliverable:** Trained cooperative MARL agents trading energy P2P with GNN coordination, all visible in real-time 3D digital twin

**Phase 3 — Evaluation & Analysis (Weeks 16–22): "The Proof"**

- Run full experimental campaign: 5 methods × 5 grid sizes × 2 heterogeneity levels × 6 stress tests
- Ablation studies (4 configurations)
- Fairness analysis: Gini coefficients, Shapley value computation
- Emergent behavior analysis: communication message visualization, spatial trading pattern identification
- Scalability benchmarking
- Performance optimization: agent inference batching, Redis state caching, RabbitMQ consumer tuning
- Comprehensive metrics evaluation with statistical significance testing
- Research paper draft with figures from the visualization dashboard
- **Deliverable:** Complete platform with comparative evaluation, emergent behavior analysis, research paper draft, demo video

---

### Possible Research Paper Title

**"Helios-Grid: Decentralized Energy Equilibrium via Graph-Neural Coordinated Multi-Agent Reinforcement Learning for Peer-to-Peer Grid Balancing in Heterogeneous Prosumer Networks"**

*Target venues:*
1. **NeurIPS** (ML + multi-agent systems)
2. **AAMAS** (Autonomous Agents and Multi-Agent Systems)
3. **IEEE Transactions on Smart Grid** (domain-specific, high impact)
4. **Applied Energy** (Elsevier, interdisciplinary energy + AI)

---

### Future Scope (PhD / Startup Potential)

**PhD Directions:**

1. **Mean-Field Game Theory for Large-Scale Grids**: Replace individual agent modeling with mean-field approximations — scale from hundreds to millions of households with theoretical convergence guarantees
2. **Multi-Energy Carrier Coordination**: Extend beyond electricity to coupled electricity-heat-gas networks with cross-carrier optimization (sector coupling)
3. **Adversarial Robustness**: Study how malicious agents (price manipulators, false reporting) affect the cooperative equilibrium; design incentive-compatible mechanisms that are robust to strategic behavior
4. **Transfer Learning Across Grid Topologies**: Train on one neighborhood, deploy on another — study generalization of learned coordination policies across different grid structures, climates, and household demographics
5. **Cyber-Physical Security**: Model cyberattacks on the P2P trading protocol (false data injection, market manipulation) and design resilient MARL policies
6. **Real Hardware Integration**: Bridge to actual smart meter APIs and home energy management systems for pilot deployment

**Startup Potential:**

- **Product**: "Helios-Grid: The AI-native Virtual Power Plant. Deploy across a neighborhood or campus. Each building learns to optimize energy autonomously. P2P trading reduces costs and carbon. No central control required."
- **Market**: $45B+ Virtual Power Plant market (projected by 2030, 25% CAGR), driven by distributed solar growth and grid modernization mandates
- **Differentiation**: vs. **AutoGrid/Enbala** (centralized optimization — doesn't scale, privacy issues); vs. **Sonnen Community** (rule-based sharing — no intelligence); Helios-Grid is the first **decentralized AI-native** VPP with learned cooperative behavior
- **Revenue**: SaaS per-node fee to utilities/community energy programs + energy savings share model + carbon credit monetization
- **Patents** (2): (1) "Decentralized energy coordination via graph-neural multi-agent reinforcement learning on distribution network topologies," (2) "Learned peer-to-peer energy trading protocols with emergent cooperative pricing dynamics"
