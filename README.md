Quantum-Enhanced AI Logistics Engine
D3CODE 2025 Hackathon Project
A route optimization tool that uses AI traffic forecasting and quantum-inspired algorithms to help get critical supplies — medicines, vaccines, emergency aid — where they need to go, faster.

The Problem
Last-mile delivery in India is genuinely hard. It's not just traffic — it's traffic plus a delivery window plus 15 stops plus a driver who doesn't know about the accident that happened 10 minutes ago.
Existing routing tools treat these as separate problems. We tried to solve them together:

Traffic changes dynamically — routes should too
Some deliveries have hard time windows (a vaccine that needs to arrive by 2pm can't arrive at 4pm)
Optimizing across many destinations at once is a computationally brutal problem
Every extra kilometer is more fuel, more emissions, more cost


What We Built
At its core, this is a VRP (Vehicle Routing Problem) solver — but one that knows about traffic. It has two main pieces:
The AI layer predicts travel times based on historical patterns and live conditions. It knows that the same road at 9am and 2pm are very different roads.
The optimization layer takes those travel times and solves the routing problem using a QUBO formulation — the same mathematical structure used in quantum annealing. We run it on a classical hybrid solver (with D-Wave support if available), then clean up the result with local search.
The result: routes that are actually realistic, not just geometrically short.

Getting It Running
Backend
bashpython -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate
pip install -r requirements.txt
python solver.py
Frontend
bashnpm install
npm start
Frontend at localhost:3000, API at localhost:5000.

How It's Structured
quantum/
├── solver.py              # The main optimization engine
├── traffic_simulator.py   # AI traffic prediction
├── qubo_builder.py        # Turns the routing problem into a QUBO
├── server.js              # Express API
├── public/
│   ├── index.html
│   ├── style.css
│   └── app.js
└── requirements.txt
The pipeline looks like this:
Traffic prediction → QUBO formulation → Solver → 2-opt cleanup → Route
Each stage feeds into the next. The traffic model informs the cost matrix, which shapes the QUBO, which the solver optimizes, which gets polished by local search.

Demo Scenarios
We set up three scenarios you can run live:

Normal day — baseline routing vs. our optimized version
Peak traffic — watch the routes adapt as congestion builds
Incident — simulate an accident mid-route and see dynamic rerouting


Stack

Python + Flask — backend and solver
XGBoost + scikit-learn — traffic forecasting
D-Wave Ocean SDK — quantum/hybrid optimization
Node.js + Express — API server
Leaflet.js + OpenStreetMap — map rendering


Results
On our test scenarios, the optimized routes showed a 16.7% reduction in delivery time compared to naive routing. More importantly, on-time delivery rate improved — which matters a lot when "on time" means a clinic has vaccines before patients arrive.

What We Were Going For
This started from a pretty simple frustration: why do delivery apps still give you a route that ignores the traffic jam everyone on the road can see? We wanted to build something that treats routing as a live, dynamic problem — not a one-time calculation at the start of the day.
Whether this ends up being useful for medical supply chains, food delivery, or just regular logistics, the core idea is the same: better information + smarter optimization = fewer wasted hours on the road.

Built for D3CODE 2025 — AI + Quantum + Data Ecosystems track
