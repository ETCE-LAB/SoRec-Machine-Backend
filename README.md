
# SoRec – OPC-UA FastAPI Integration

This repository is part of the **SoRec project** developed by the **ETCE Lab**.

The SoRec machine is designed to process shredded electronic waste and separate it into **metallic and non-metallic components** for further recycling. This application provides the software layer for real-time machine control and monitoring, combining **FastAPI** with **OPC-UA** communication.

- The **main branch** contains stable code for controlling and monitoring machine speeds.
- The **dev branch** contains an alternative experimental implementation that is still under development.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture & Code Structure](#architecture--code-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Endpoints](#api-endpoints)
- [Logging & Monitoring](#logging--monitoring)
- [Contributing](#contributing)

---

## Overview

**SoRec** is a FastAPI-based software integration for an industrial sorting machine that separates shredded electronic waste (e-waste) into metallic and non-metallic materials.

This application allows:

- Connecting to an OPC-UA server to read and control machine parameters
- Monitoring conveyor belt speed via background tasks
- Controlling core machine components (e.g., feeder, belt, magnetic drum) through a REST API
- Easy extension and maintenance through a modular code structure

---

## Features

- **OPC-UA Integration:** Communicates with an OPC-UA server to read/write values such as speed.
- **Asynchronous Monitoring:** Background task monitors the conveyor speed and logs changes.
- **RESTful API:** Offers simple, well-structured endpoints for machine control.
- **Modular Codebase:** Clear separation between models, routes, utilities, and main logic.
- **Error Handling:** Consistent HTTP status codes and structured error responses.

---

## Architecture & Code Structure

```
SoRec/
├── app/
│   ├── __init__.py         # Package initialization
│   ├── main.py             # App setup, OPC-UA client, lifecycle hooks
│   ├── models.py           # Pydantic models for request validation
│   ├── routes.py           # API route definitions
│   └── utils.py            # Helper functions for OPC-UA interaction
├── requirements.txt        # Project dependencies
└── README.md               # Project documentation
```

- `main.py`: Starts the FastAPI app, sets up the OPC-UA client, and handles startup/shutdown events.
- `models.py`: Contains data validation models like `SpeedInput`.
- `routes.py`: Defines the REST API endpoints for machine control and speed adjustments.
- `utils.py`: Includes utility functions such as `update_opc_node` and `monitor_speed_belt`.

---

## Installation

### Prerequisites

- Python 3.8 or later
- Access to an OPC-UA server (server address required)

### Steps

1. **Clone the repository**

   ```bash
   git clone https://github.com/ETCE-LAB/SoRec-Machine-Backend.git
   cd SoRec-Machine-Backend
   ```

2. **Create a virtual environment**

   ```bash
   python -m venv venv
   ```

3. **Activate the environment**

   - On **Windows**:
     ```bash
     venv\Scripts\activate
     ```
   - On **macOS/Linux**:
     ```bash
     source venv/bin/activate
     ```

4. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

---

## Configuration

Update the OPC-UA server address directly in `app/main.py`:

```python
OPC_ADDRESS = "opc.tcp://139.174.27.2:4840"
```

You may also configure logging or default speed values here as needed.

---

## Running the Application

To run the app with automatic reloading for development:

```bash
uvicorn app.main:app --reload
```

The API will be available at: [http://127.0.0.1:8000](http://127.0.0.1:8000)  
Interactive documentation is available at: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

---

## API Endpoints

### 🔄 Belt Speed Monitoring

- **PATCH `/startBeltSpeedMonitor`**  
  Starts background speed monitoring.  
  Responses:
  - `{ "message": "Monitor started" }`
  - `{ "message": "Monitor is already running" }`

- **PATCH `/stopBeltSpeedMonitor`**  
  Stops the active monitoring task.  
  Responses:
  - `{ "message": "Monitor stopped" }`
  - `{ "message": "Monitor is not running" }`

---

### ⚙️ Machine Control

- **PATCH `/machineOn`**  
  Turns the machine on. *(Functionality placeholder)*

- **PATCH `/machineOff`**  
  Turns the machine off. *(Functionality placeholder)*

- **PATCH `/stop`**  
  Stops all machine components by setting speeds to zero.  
  Response includes result details for belt, drum, and feeder.

---

### 📊 Speed Monitoring

- **GET `/speedOfBelt`** → `{ "speedOfBelt": <value> }`  
- **GET `/speedOfDrum`** → `{ "speedOfDrum": <value> }`  
- **GET `/speedOfFeeder`** → `{ "speedOfFeeder": <value> }`  

---

### 🎚️ Speed Control

Each of the following endpoints accepts a JSON payload:

```json
{ "speed": 75 }
```

- **PATCH `/speedOfBelt`** – Set conveyor belt speed  
- **PATCH `/speedOfDrum`** – Set magnetic drum speed  
- **PATCH `/speedOfFeeder`** – Set feeder speed  

---

## Logging & Monitoring

- **Logging:** All major actions, client responses, and errors are logged with timestamp and severity level.
- **Monitoring Task:** A background task continuously checks the belt speed and logs deviations or changes.

---

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes and push the branch.
4. Open a pull request with a description of your update.

Please ensure your contributions follow the existing code structure and include helpful comments or documentation where needed.

---
