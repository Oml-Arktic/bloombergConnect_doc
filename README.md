# Bloomberg-connect, CFit enviroment

## Index
1. [Overview](#overview)
2. [Features](#features)
3. [Instalation](#instalation)
4. [Usage](#usage)
5. [Dependencies](#dependencies)
6. [Structure](#structure)
7. [Contributing](#contributing)

## Overview
Bloomberg connector `bloomberg-connect` is a microservice running in the BBVA C-Fit environment. Its main task is retrieve realtime market data from Bloomberg B-Pipe servers and provide snapshots of this information to other microservices in the BBVA C-Fit environment through kafka.

---

## Features
1. Real-Time Market Data Retrieval
   - Connects to Bloomberg B-Pipe servers to subscribe to market data
   - Supports high-frequency updates, handling up to 200,000 market data updates/sec when markets are open
2. Scalable Architecture
   - Vertical scalability:
      - Can increase the number of open sessions with Bloomberg
      - Each session runs in its own thread, utilizing multi-core CPUs efficiently
   - Horizontal scalability:
      - Deploys multiple instances in separate machines
      - Uses Kafka topic partitions to distribute market data requests across instances
      - Each instance handles a subset of the total securities universe
3. Bloomberg API Integration
   - Utilizes Bloomberg's BLPAPI for data requests & subscriptions
   - Supports multiple Bloomberg services, including:
      - `//blp/mktdata` → Market data service for real-time updates
      - `//blp/mktlist` → Market list service for retrieving futures/options chains
      - `//blp/refdata` → Reference data service for security metadata
      - `//blp/apiauth` → Authentication and user entitlement service
4. Kafka Integration for Market Data Distribution
   - Publishes market data snapshots to Kafka topics
   - Uses:
      - `XEDP_KAFKA_CALIBRATION_REQUEST_TOPIC` → Receives snapshot requests
      - `XEDP_KAFKA_CALIBRATION_RESPONSE_TOPIC` → Sends snapshot responses
5. Session & Subscription Management
   - Maintains multiple sessions to Bloomberg, each connected to a primary and a secondary B-Pipe server
   - Uses a failover mechanism. If the primary server fails, Bloomberg automatically switches to the secondary server
   - Supports multiple subscriptions per session
6. Metrics & Monitoring with Prometheus & Grafana
   - Exposes real-time performance metrics:
      - CPU & Memory usage
      - Active Bloomberg sessions & subscriptions
      - Market data update rates
   - Data is exported in Prometheus format, consumed by Grafana dashboards
7. Security & Authentication
   - Uses Bloomberg's token-based authentication
   - Supports per-user entitlements via EID (Entitlement ID) checks
8. Configurable via Environment Variables & JSON Files
   - Uses environment variables or a JSON configuration file for settings:
      - `XEDP_APP_CONFIG_FILENAME` → Configuration file path
      - `XEDP_BPIPE_SESSION_PRIMARY_SERVERS` → List of primary B-Pipe servers
      - `XEDP_BPIPE_SESSION_SECONDARY_SERVERS` → List of backup B-Pipe servers
9. Development & Testing Support (Mock Mode)
   - Mock mode can be enabled to test without connecting to Bloomberg
   - Allows:
      - Using pre-recorded snapshots instead of real-time data
      - Simulating API responses
1. DevOps & Deployment
   - Runs as a Dockerized microservice in AWS Kubernetes
   - Supports debugging & profiling with:
      - Perf (Linux performance tool)
      - CLion for analyzing performance data


---

## Instalation

### Prerequisites
To get the development environment running you need:
- **Visual Studio Code with Dev Container extension**
- **WSL2 (or linux environment)**
- **Git**

### Steps
1. Get Docker:
   ```bash
   sudo apt install docker.io
   ```
   For Docker with WSL2 must enable `Use the WSL 2 based engine`
   
2. Submit credentials:
   ```bash
   docker login artifactory.globaldevtools.bbva.com:443
   ```
   - USER is name from your email account name@bbva.com (basically without @bbva.com)
   - PASS is the 'Reference Token' from Artifactory (https://artifactory.globaldevtools.bbva.com/ui/user_profile)
  
3. Create a file called `.env` under .devcontainer/, with two env variables:
   ```
   ARTIFACTORY_USER=<name_without_@bbva.com>
   ARTIFACTORY_PASS=<artifactory_reference_token>
   ```
   
4. Modify your `~/.gitconfig` file by adding these two lines under `[user]` section (your git user by default):
   ```
   [includeIf "gitdir:/code/bbva/"]
   path = .gitconfig-bbva
   ```

5. Create `~/.gitconfig-bbva` with this content:
   ```
   [user]
        name = `<your_name>` (e.g. John Doe)
        email = `<bbva_email>`
   ```
   This will basically overwrite your default git user with the one from BBVA when in dir /code/bbva, essentially when in the dev container (or in the same dir from your host WSL machine).
   
5. Run:
   ```bash
     chmod 600 ~/.gitconfig* && chmod -x ~/.gitconfig*
   ```

### Mock enviroment
---


## Usage


---

## Dependencies
- Core dependencies
   - Bloomberg API (BLPAPI)
   - Kafka
   - Prometheus & Grafana (Monitoring)
- System dependencies
   - Docker & Kubernetes
   - Linux Tools
   - AWS PrivateLink & TLS/SSL (Networking & Security)
- Development & Build dependencies
   - Conan
   - CMake
   - Clang/LLVM
   - Python
- Configuration Files
   - `configuration.md` → Defines environment variables
   - `docker-compose.yml` → Defines Docker services
   - `.devcontainer.json` → Sets up a VS Code development container

To install dependencies:
```bash

```

---

## Structure

```plaintext
microservicio-autenticacion/
│── .devcontainer/
│   ├── env_files/
│   ├──   ├── conan.env             # Configuration settings for Conan
│   ├── .env                     # Artifactory env variables
│   ├── add_conan_remotes.sh     # Configuration of Conan
│   ├── devcontainer.json        # Enviroment configuration using Dev Containers
│   ├── docker-compose.yml       #
│   ├── Dockerfile               #
│── .gradle/
│── .vscode/
│── build/
│── charts/
│── cmake/
│── config/
│── docker/
│── docs/
│── docs_api/
│── docs_dev/
│── env/
│── gradle/
│── include/
│── resources/
│── scripts/
│── src/                       # C++ classes
│   ├── bbg/
│   ├──   ├── connector
│   ├──   ├── eid
│   ├──   ├── fsm
│   ├──   ├── metrics
│   ├──   ├── mkt
│   ├──   ├── refdata
│   ├──   ├── schema
│   ├──   ├── server
│   ├──   ├── session
│   ├──   ├── subscription
│   ├── bmg_connect
│   ├── config
│   ├── date
│   ├── grpc
│   ├── http
│   ├── kafka
│   ├── log
│   ├── main
│   ├── market_data
│   ├── metrics
│   ├── mock
│── testing/
│── Dockerfile
│── Jenkisfle
│── README.md

```

---

## Contributing


---

## License


---
