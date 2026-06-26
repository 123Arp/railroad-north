# 🚂 Railroad North - OT Security Lab

**Master-Slave Distributed PLC Architecture for Railway Control System**

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.9%2B-green.svg)](https://www.python.org/)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)
[![OT Security](https://img.shields.io/badge/OT%20Security-Training-red.svg)]()

## 📋 Overview

Railroad North is a **comprehensive OT/ICS security training lab** simulating a distributed freight railway control system with master-slave PLC architecture. Perfect for learning:

- ✅ Industrial Protocol Analysis (Modbus TCP)
- ✅ PLC Programming & Coordination
- ✅ Network Segmentation & Firewall Rules
- ✅ Security Monitoring & Detection
- ✅ Safety-Critical System Design
- ✅ Incident Response in OT Environments

### Key Features

- **15 Docker Containers** - Complete OT environment
- **3 Track Segments** - Distributed control system
- **Master-Slave Architecture** - Real-world PLC topology
- **Safety Interlocks** - 4+ critical safety rules
- **Full Monitoring Stack** - Elasticsearch + Kibana + IDS
- **4 Training Exercises** - 8 hours hands-on
- **5 Attack Scenarios** - Realistic security tests
- **Network Segmentation** - IT/DMZ/OT separation

---

## 🚀 Quick Start

### Prerequisites
```bash
- Docker (20.10+)
- Docker Compose (1.29+)
- 8GB RAM minimum (16GB recommended)
- 20GB free disk space
```

### Deploy in 5 Steps

```bash
# 1. Clone repository
git clone https://github.com/yourusername/railroad-north.git
cd railroad-north

# 2. Create networks
docker network create ot-network
docker network create dmz-network
docker network create it-network

# 3. Deploy lab
docker-compose up -d

# 4. Wait for services (30 seconds)
sleep 30

# 5. Verify
docker-compose ps
```

### Access Points

| Service | URL | Port |
|---------|-----|------|
| SCADA UI | http://localhost:8080 | 8080 |
| Kibana | http://localhost:5601 | 5601 |
| Elasticsearch | http://localhost:9200 | 9200 |
| Master PLC API | http://localhost:8080/api/status | 8080 |

---

## 📁 Repository Structure

```
railroad-north/
├── README.md                          # This file
├── docker-compose.yml                 # 15 container configuration
├── LICENSE                            # MIT License
├── .gitignore                         # Git ignore patterns
│
├── master-plc/
│   ├── Dockerfile                     # Master PLC container image
│   ├── master_plc.py                  # Master controller logic
│   ├── slave_config.json              # Slave PLC addresses
│   └── requirements.txt               # Python dependencies
│
├── slave-plc/
│   ├── Dockerfile                     # Slave PLC container image
│   ├── slave_plc.py                   # Slave controller logic
│   ├── segment_1.json                 # North segment config
│   ├── segment_2.json                 # Central segment config
│   ├── segment_3.json                 # South segment config
│   └── requirements.txt               # Python dependencies
│
├── scada/
│   ├── Dockerfile                     # SCADA container image
│   ├── scada_server.py                # Central control interface
│   └── requirements.txt               # Python dependencies
│
├── monitoring/
│   ├── logstash.conf                  # Log processing pipeline
│   ├── elasticsearch.yml              # Elasticsearch config
│   ├── syslog_collector.py            # Syslog aggregation
│   ├── zeek-rules.sig                 # IDS signatures
│   └── kibana-dashboards.json         # Pre-built dashboards
│
├── scripts/
│   ├── deploy.sh                      # Main deployment script
│   ├── health-check.sh                # System health monitoring
│   ├── reset-lab.sh                   # Clean reset
│   ├── generate-traffic.py            # Test traffic generator
│   └── modbus-attack.py               # Attack simulation
│
├── config/
│   ├── firewall-rules.conf            # DMZ firewall rules
│   ├── network-config.conf            # Network settings
│   └── docker-compose.env             # Environment variables
│
├── docs/
│   ├── ARCHITECTURE.md                # System design
│   ├── DEPLOYMENT.md                  # Step-by-step deployment
│   ├── TRAINING.md                    # Training program
│   ├── SCENARIOS.md                   # Attack scenarios
│   ├── TROUBLESHOOTING.md             # Common issues
│   └── API.md                         # API documentation
│
├── training/
│   ├── exercises/
│   │   ├── exercise-1.md              # Lab Exercise 1
│   │   ├── exercise-2.md              # Lab Exercise 2
│   │   ├── exercise-3.md              # Lab Exercise 3
│   │   └── exercise-4.md              # Lab Exercise 4
│   │
│   ├── scenarios/
│   │   ├── scenario-1-unauthorized-switching.md
│   │   ├── scenario-2-heartbeat-failure.md
│   │   ├── scenario-3-safety-bypass.md
│   │   ├── scenario-4-modbus-attack.md
│   │   └── scenario-5-syslog-injection.md
│   │
│   └── grading-rubric.md              # Assessment criteria
│
└── volumes/                           # Data persistence (gitignored)
    ├── scada/
    ├── master-plc/
    ├── slave-plc-1/
    ├── slave-plc-2/
    ├── slave-plc-3/
    └── elasticsearch/
```

---

## 🏗️ System Architecture

### Network Topology

```
┌─────────────────────────────────────────────────────────┐
│                  IT NETWORK (172.26.0.0/16)              │
│              (Management & Engineering)                  │
└──────────────────────┬──────────────────────────────────┘
                       │ [Firewall]
┌──────────────────────┴──────────────────────────────────┐
│                 DMZ NETWORK (172.27.0.0/16)              │
│         (Bridge between IT and OT networks)              │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐ │
│  │ EWS         │  │ SCADA Server │  │ DMZ Collector  │ │
│  │ (Remote)   │  │ (Control)    │  │ (Logs)         │ │
│  └─────────────┘  └──────────────┘  └────────────────┘ │
└──────────────────────┬──────────────────────────────────┘
                       │ [Firewall]
┌──────────────────────┴──────────────────────────────────┐
│                  OT NETWORK (172.25.0.0/16)              │
│            (Critical Control Systems)                    │
│                                                          │
│  Master PLC (172.25.0.10)  ← Central Coordinator        │
│       │                                                  │
│  ┌────┼────┐                                            │
│  │    │    │                                            │
│  ▼    ▼    ▼                                            │
│ S1   S2   S3      ← Slave PLCs (Segments)               │
│                                                          │
│  Monitoring Stack:                                       │
│  ├─ Zeek IDS                                            │
│  ├─ Syslog Collector                                    │
│  ├─ Elasticsearch                                       │
│  ├─ Kibana                                              │
│  └─ Logstash                                            │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### Components

| Component | Role | Port | Container |
|-----------|------|------|-----------|
| Master PLC | Central coordinator | 502 | railroad-master-plc |
| Slave PLC 1 | North segment | 5021 | railroad-slave-plc-1 |
| Slave PLC 2 | Central segment | 5022 | railroad-slave-plc-2 |
| Slave PLC 3 | South segment | 5023 | railroad-slave-plc-3 |
| SCADA | Control interface | 8080 | railroad-scada |
| Zeek | IDS | 5901 | railroad-ids |
| Elasticsearch | Data store | 9200 | railroad-elasticsearch |
| Kibana | Visualization | 5601 | railroad-kibana |
| Logstash | Log processing | 5000 | railroad-logstash |
| Collector | Syslog aggregator | 514 | railroad-collector |

---

## 📚 Documentation

### Getting Started
- **[DEPLOYMENT.md](docs/DEPLOYMENT.md)** - Step-by-step setup guide
- **[ARCHITECTURE.md](docs/ARCHITECTURE.md)** - System design details
- **[API.md](docs/API.md)** - API documentation

### Training
- **[TRAINING.md](docs/TRAINING.md)** - Training program overview
- **[Exercise 1](training/exercises/exercise-1.md)** - System Architecture
- **[Exercise 2](training/exercises/exercise-2.md)** - PLC Programming
- **[Exercise 3](training/exercises/exercise-3.md)** - Network Segmentation
- **[Exercise 4](training/exercises/exercise-4.md)** - Security Monitoring

### Security
- **[SCENARIOS.md](docs/SCENARIOS.md)** - Attack scenarios
- **[Scenario 1](training/scenarios/scenario-1-unauthorized-switching.md)** - Unauthorized Switching
- **[Scenario 2](training/scenarios/scenario-2-heartbeat-failure.md)** - Heartbeat Failure
- **[Scenario 3](training/scenarios/scenario-3-safety-bypass.md)** - Safety Bypass
- **[Scenario 4](training/scenarios/scenario-4-modbus-attack.md)** - Modbus Attack
- **[Scenario 5](training/scenarios/scenario-5-syslog-injection.md)** - Syslog Injection

### Reference
- **[TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)** - Common issues
- **[Grading Rubric](training/grading-rubric.md)** - Assessment criteria

---

## 🎯 Quick Test Commands

### Send Route Command
```bash
curl -X POST http://localhost:8080/api/command \
  -H "Content-Type: application/json" \
  -d '{"segment_id": 1, "route": "ROUTE_A"}'
```

### Check System Status
```bash
curl http://localhost:8080/api/status | jq '.'
```

### View Logs
```bash
docker-compose logs -f railroad-master-plc
```

### Monitor Health
```bash
bash scripts/health-check.sh
```

---

## 🧪 Training Exercises

### Exercise 1: System Architecture & Deployment (2 hours)
Learn the lab structure and deploy all components.
- Deploy the complete system
- Understand network topology
- Verify all services running
- Explore monitoring dashboards

### Exercise 2: PLC Programming & Control (2 hours)
Understand master-slave coordination.
- Send route change commands
- Monitor Modbus protocol traffic
- Test safety interlocks
- Analyze command sequences

### Exercise 3: Network Segmentation & Firewall (2 hours)
Implement security boundaries.
- Design firewall rules
- Test zone separation
- Verify access controls
- Document rule effectiveness

### Exercise 4: Security Monitoring & Detection (2 hours)
Build detection capabilities.
- Create baseline behavior profile
- Write detection rules
- Build Kibana dashboards
- Test rule accuracy

**Total Training:** 8 hours hands-on

---

## 🔓 Security Scenarios

### 1. Unauthorized Track Switching
Detect attempts to change routes from unauthorized sources.
**Duration:** 20 minutes | **Difficulty:** Medium

### 2. PLC Heartbeat Failure
Simulate loss of slave PLC communication.
**Duration:** 15 minutes | **Difficulty:** Easy

### 3. Safety Interlock Bypass
Attempt to bypass safety rules.
**Duration:** 30 minutes | **Difficulty:** Hard

### 4. Modbus Protocol Manipulation
Detect malformed/abnormal Modbus traffic.
**Duration:** 45 minutes | **Difficulty:** Advanced

### 5. Syslog Injection
Detect false log messages in audit trail.
**Duration:** 25 minutes | **Difficulty:** Medium

**Total Scenarios:** 3 hours hands-on

---

## 🛠️ Usage Examples

### Deploy the Lab
```bash
docker-compose up -d
sleep 30
docker-compose ps
```

### Reset Lab
```bash
bash scripts/reset-lab.sh
```

### Generate Traffic
```bash
python3 scripts/generate-traffic.py --scenario normal --duration 300
```

### Run Health Check
```bash
bash scripts/health-check.sh
```

### View Logs
```bash
docker-compose logs -f
docker logs railroad-master-plc
docker logs railroad-scada
```

### Access Services
```bash
# SCADA Web Interface
firefox http://localhost:8080

# Kibana Dashboards
firefox http://localhost:5601

# Elasticsearch API
curl http://localhost:9200/_cluster/health
```

---

## 🔧 Configuration

### Environment Variables

Edit `.env` file to customize:

```bash
# Master PLC
MASTER_PLC_IP=172.25.0.10
MASTER_PLC_PORT=502

# SCADA
SCADA_PORT=8080
SCADA_LOG_LEVEL=INFO

# Elasticsearch
ES_JAVA_OPTS=-Xms512m -Xmx512m
ES_CLUSTER_NAME=railroad-north

# Docker
COMPOSE_PROJECT_NAME=railroad-north
```

### Network Configuration

Modify in `docker-compose.yml`:

```yaml
networks:
  ot-network:
    ipam:
      config:
        - subnet: 172.25.0.0/16
  
  dmz-network:
    ipam:
      config:
        - subnet: 172.27.0.0/16
  
  it-network:
    ipam:
      config:
        - subnet: 172.26.0.0/16
```

---

## 📊 System Requirements

### Minimum
- CPU: 4 cores
- RAM: 8GB
- Disk: 20GB free
- Docker: 20.10+

### Recommended
- CPU: 8+ cores
- RAM: 16GB
- Disk: 50GB free
- Docker Desktop with 4+ cores allocated

---

## 🚨 Troubleshooting

### Containers Won't Start
```bash
# Check Docker resources
docker system df

# Check logs
docker-compose logs

# Clean up and retry
docker system prune
docker-compose up -d
```

### Master PLC Shows Offline
```bash
# Check logs
docker logs railroad-master-plc

# Verify network
docker exec railroad-master-plc ping 172.25.1.10

# Restart
docker-compose restart railroad-master-plc
```

### No Logs in Kibana
```bash
# Check Elasticsearch
curl http://localhost:9200/_cat/indices

# Check Logstash
docker logs railroad-logstash

# Send test log
docker exec railroad-collector logger -h 172.25.0.40 "TEST"
```

See [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) for more solutions.

---

## 📖 What You'll Learn

✅ **OT Architecture**
- Industrial control systems
- PLC programming
- Master-slave coordination
- Safety-critical systems

✅ **Industrial Protocols**
- Modbus TCP/RTU
- Syslog
- Real-time communication
- Protocol analysis

✅ **Cybersecurity**
- Network segmentation
- Firewall rules
- Intrusion detection
- Incident response
- Attack simulation

✅ **Monitoring**
- Log aggregation
- SIEM integration
- Anomaly detection
- Alert creation

---

## 🎓 Who Is This For?

- 🔒 **Security Professionals** - Learn OT security
- 👨‍💻 **Network Engineers** - Understand industrial networks
- 🏭 **Operations Teams** - Operate safely
- 📚 **Students** - Learn cyber-physical systems
- 👨‍🏫 **Instructors** - Teach hands-on labs
- 🔬 **Researchers** - Study OT threats

---

## 📜 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -am 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open Pull Request

### Areas for Contribution
- Additional PLC segments
- New industrial protocols (EtherNet/IP, OPC UA, BACnet)
- Custom attack scenarios
- Enhanced dashboards
- Documentation improvements

---

## 📞 Support

### Documentation
- Read [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) for setup help
- Check [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) for common issues
- See [docs/API.md](docs/API.md) for API reference

### Community
- Open an issue for bugs
- Start a discussion for questions
- Check existing issues first

---

## 🔗 Related Resources

- [Modbus Protocol](http://www.modbus.org/)
- [IEC 62443 Standard](https://isa99.isa.org/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework/)
- [Zeek IDS](https://zeek.org/)
- [Elasticsearch](https://www.elastic.co/)

---

## ⭐ Star History

If you find this useful, please star the repository!

---

## 📝 Citation

If you use Railroad North in research or publications, please cite:

```bibtex
@software{railroadnorth2024,
  title={Railroad North: OT Security Training Lab},
  author={DeepTrustxAI},
  year={2024},
  url={https://github.com/yourusername/railroad-north}
}
```

---

## 🚂 Getting Started Now

```bash
# 1. Clone
git clone https://github.com/yourusername/railroad-north.git
cd railroad-north

# 2. Deploy
docker-compose up -d

# 3. Learn
firefox http://localhost:8080
firefox http://localhost:5601

# 4. Train
cat training/exercises/exercise-1.md
```

**Happy training! 🚂**

---

**Last Updated:** June 2024  
**Version:** 1.0.0  
**Maintainer:** DeepTrustxAI Pvt Ltd
#   R a i l R o a d - N o r t h - A r p i t  
 