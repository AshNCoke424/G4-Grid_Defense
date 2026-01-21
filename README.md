# G4-Grid_Defense
Python simulation for G4 Solar Storm Defense
# ⚡ G4 Solar Storm Grid Defense System

### A Cyber-Physical System (CPS) Simulation for Critical Infrastructure Protection

**Author:** [Your Name]  
**Tech Stack:** Python, Modbus TCP, Streamlit, NumPy, IoT (LoRa)

---

## 📖 Project Overview
This project simulates a **resilient 110kV Substation Control System** designed to withstand two major threats:
1.  **G4 Geoelectric Storms:** Extreme solar weather capable of inducing Geomagnetically Induced Currents (GIC) in power transformers.
2.  **Cyber-Kinetic Attacks:** Attempts by malicious actors to force unsafe equipment states (e.g., Arc Flash incidents).

The system uses a "Secure-by-Design" architecture, integrating physics modeling, industrial safety protocols, and emergency off-grid communications.

---

## 🛡️ Key Features

### 1. Physics-Driven Defense Engine (`gic_physics.py`)
* **Real-time Modeling:** Uses `NumPy` to calculate induced Geoelectric Fields ($V/km$) based on USGS soil resistivity data (2000 $\Omega \cdot m$ for Georgia Piedmont).
* **Thermal Prediction:** Predicts transformer core temperature rise based on storm intensity.
* **Automated Load Shedding:** Automatically engages Neutral Blocking Devices (NBD) when core temps exceed safe limits ($120^\circ C$).

### 2. Safety-Critical PLC Logic (`mock_plc_safe.py`)
* **Arc Flash Prevention:** Implements a strict "State-Aware" safety interlock.
* **Panic Mitigation:** The PLC automatically rejects re-close commands if attempted within 5 seconds of a fault trip.
* **Audit Proven:** Verified via `safety_audit.py` to ensure the logic overrides human error during high-stress scenarios.

### 3. Emergency Mesh Network (`lora_mesh_chat.py`)
* **Grid-Down Comms:** Simulates a decentralized LoRa (Long Range) radio mesh using UDP Multicast.
* **Resilience:** Allows operators to communicate text commands even if cellular and internet infrastructure fails.

---

## ⚙️ Architecture

| Component | File | Description |
| :--- | :--- | :--- |
| **HMI / SCADA** | `dashboard.py` | Streamlit dashboard for monitoring grid health and storm intensity. |
| **Controller** | `mock_plc_safe.py` | Modbus TCP Server acting as the substation safety controller. |
| **Physics** | `gic_physics.py` | Mathematical model of the solar storm environment. |
| **Auditor** | `safety_audit.py` | Negative testing script to verify safety interlocks. |

---

## 🚀 How to Run

**1. Start the Secure PLC (Hardware)**
This acts as the server. It must be running first.
```bash
python mock_plc_safe.py
streamlit run dashboard.py
python safety_audit.py
⚠️ BREAKER TRIPPED!
🛑 SAFETY LOCKOUT: Cannot reset breaker yet. Cooling down...