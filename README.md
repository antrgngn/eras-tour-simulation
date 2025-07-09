
# Surviving “The Great War”  
### A Complex-Systems Simulation of *The Eras Tour* Secondary-Ticket Market in Buenos Aires

> **Course:** CS 166 – Modeling & Analysis of Complex Systems  
> **Author:** An Nguyen (Minerva University)  
> **Semester:** Spring 2025

---

## 1️⃣  Project Overview
Swiftmania didn’t stop at sold-out stadiums—the informal resale market in Argentina ballooned despite an official ban on ticket transfers.  
This project builds an **agent-based / network simulation** to explore how tickets changed hands among Buenos Aires Swifties, starting right after official sales (6 June 2023) until the concerts concluded.

Two progressively richer models are implemented:

| Model | Key Mechanics | Extra Features |
|-------|---------------|----------------|
| **Basic** | • Nodes = buyers/sellers (1 ticket max)  <br>• Watts-Strogatz small-world graph <br>• Fixed demand ( *pₙ* = *p_d*) and fixed transaction probability ( *pₜ*) | *None* |
| **Buenos Aires+** | • Nodes may hold 0–4 tickets  <br>• Demand & transaction probability rise with a *popularity index* *eₙ*  <br>• Price-sensitive buying (normal \$200 ± \$20) | • Money balances tracked  <br>• Emergent correlation analysis between connectivity, cash, and ticket hoarding |

The notebook visualises network evolution, ticket-ownership heat-maps, and summary stats such as average degree vs. *p_d*:contentReference[oaicite:0]{index=0}.

---

## 2️⃣  Repository Layout
```

.
├── CS166\_FINAL\_PROJECT\_CODE.ipynb     # Jupyter notebook (main simulation)
├── cs166\_model\_taylor.pdf             # Full write-up / analysis
├── README.md                          # ← you are here
└── requirements.txt                   # Python dependencies (auto-generated)

````

---

## 3️⃣  Quick Start

### 📦 Set up environment
```bash
# 1. Clone
git clone https://github.com/<your-handle>/eras-tour-resale.git
cd eras-tour-resale

# 2. Create virtual env (conda or venv)
conda create -n eras python=3.10 -y
conda activate eras

# 3. Install deps
pip install -r requirements.txt
````

> `requirements.txt` was exported from the working notebook and typically includes
> `networkx`, `numpy`, `matplotlib`, `pandas`, `tqdm`, and `ipympl`.

### 🚀 Run the notebook

```bash
jupyter lab CS166_FINAL_PROJECT_CODE.ipynb
```

Follow the top-level **🔧 Parameters** cell to tweak:

```python
N            = 130        # number of agents
pd_initial   = 0.10       # baseline demand
pt_initial   = 0.20       # baseline transaction prob
timesteps    = 50
max_tickets  = 4          # per-agent ticket cap (extended model)
price_mu     = 200        # USD
price_sigma  = 20
```

Run all cells to reproduce every figure in the PDF.

---

## 4️⃣  Results Snapshot

* Average degree grows rapidly with higher demand probability, confirming the strong clustering expected in a Watts-Strogatz small-world network.
* Contrary to “You’re On Your Own, Kid,” low *p\_d* regimes still converge to fully connected ticket-sharing communities—just more slowly.
* Money-rich, highly connected nodes often retain the most tickets, hinting at “super-scalpers,” but confidence intervals remain wide, making firm conclusions elusive.

Detailed discussion, caveats, and future extensions (e.g., scale-free graphs, richer socio-economic traits) are in the accompanying PDF (§ “Is It Over Now?”).

---

## 5️⃣  How it Works (High-level)

1. **Network Generation** – `networkx.watts_strogatz_graph(N, k, beta)`
2. **Role Assignment** – Randomly label each node `BUYER` or `SELLER`; assign ticket counts & cash.
3. **Iteration Loop** (`update()`):

   * Rewire edges with probability *β*.
   * For every buyer–seller edge: attempt a transaction with prob *pₜ(n)* = *pt\_initial + eₙ − 0.001·price*.
   * Transfer tickets/cash; swap roles when a seller sells out.
4. **Observation** (`observe()`): render current graph, histograms, and write logs for post-hoc analysis.
5. **Repeat** for the chosen number of timesteps; optionally average over many Monte-Carlo trials.

---

## 6️⃣  Extending / Re-using

* **Different network topologies** – swap in `nx.barabasi_albert_graph` to test preferential attachment.
* **Policy experiments** – cap max-tickets to 2 or introduce transaction fees.
* **Parallel runs** – wrap the simulation loop in `joblib` or `dask` to explore parameter sweeps.

---

## 7️⃣  License & Attribution

This repository is released under the MIT License.
Portions of the analysis draw on code ideas from *Introduction to the Modeling and Analysis of Complex Systems* by Sayama (2015) and publicly shared network-science notebooks.
All Eras Tour references remain property of Taylor Swift and relevant right-holders.

---

## 8️⃣  Acknowledgements

*Prof. Lucas Tambasco* for guidance in CS166.
Swifties worldwide for data inspiration.
ChatGPT 4 assisted with matplotlib animation boilerplate and debugging loops—see AI-statement in the PDF for full disclosure.

---

*“So make the friendship bracelets, take the moment and simulate it* 🌟


