# Autonomous Supply Network and Inventory Design: A Cyber-Physical Work System

Course project for **IE 588: e-Work and e-Service**, Purdue University (May 2026). Team of two.

## Question

Can a network of AI agents run inventory replenishment across warehouses and suppliers on its own, and keep working when suppliers fail or agents deadlock?

## System

A simulated network of 3 warehouses, 5 suppliers and 847 SKUs, coordinated by an AI core built in MATLAB. Data comes from RFID sensors, weather APIs and ERP feeds, and the output is autonomous purchase orders and route dispatches.

The design follows a four-layer cyber-physical architecture:

| Layer | Contents |
|---|---|
| Physical | Warehouses, AGVs, conveyors, RFID sensors |
| Network | ERP integration, API feeds, IoT gateway, MQTT bus |
| Data | Real-time demand and inventory state |
| Application | AI agents: TRAP, SRAP, STOP, FTCP, ADAM, DDAM |

## Design

The project applies 15 design models from collaborative control theory and e-Work. The key pieces:

- **Task administration protocols (TRAP, SRAP, STOP):** TRAP generates replenishment tasks when stock runs low, SRAP runs parallel supplier bid cycles, and STOP sets a time window for each task.
- **FTCP (fault-tolerant coordination protocol):** deadlock detection, supplier failover and time-bounded execution, built on the protocol in Park, Gupta and Fernandez (2024), the course reference article.
- **ADAM (anticipatory disruption absorption):** raises buffer stock before a disruption hits, when supplier lead-time variance exceeds 1.5 times its baseline.
- **DDAM (delegated decision authority):** three authority levels from automated agents up to a human manager.
- **PNAM (progressive network autonomy):** five autonomy levels. The design runs at Level 3, supervised autonomy.
- **VCMM (value conflict mediation):** a weighted objective that balances holding cost (0.40), fill rate (0.35) and risk exposure (0.25).

## Results

30-day MATLAB Simulink simulation with 847 SKUs, 3 warehouses, 5 suppliers and 500 injected disruption events, compared with a traditional static reorder policy:

| Metric | Traditional | This design |
|---|---|---|
| Order lead time | about 4.1 hrs | about 0.8 hrs |
| Fill rate | 89.2% | 97.3% |
| Stockout events per month | 67 | 11 |
| Holding cost index | 1.00 | 0.77 |
| Human decisions per day | 847 | 19 |
| Disruption recovery time | 2.8 hrs | 47 min |

Other results from the deck:
- LSTM demand forecast: 6.2% MAPE against 18.4% for a moving average baseline.
- FTCP failover averaged 23.4 minutes against 44.3 minutes without the protocol.

## MATLAB tools used

Deep Learning Toolbox (LSTM forecasting), Optimization Toolbox (fmincon for safety stock and VCMM), Statistics Toolbox, Parallel Computing Toolbox, Simulink Stateflow (FTCP state machine).

## Limitations

- Validated in simulation only. A real warehouse deployment would be needed to confirm AGV integration.
- The move from autonomy Level 3 to Level 4 has not been validated with real-world data.
- VCMM weights are specific to this setup and would need recalibration for other industries.
- The cyber-security layer is not modeled.

## Team

Nikhitha Kalakuntla, Adharsh Rajasekar
