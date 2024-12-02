# Cheshire-Terminal-Cluster
The official Github of The Cheshire Terminal Ai Solana Exo Labs Cluster Powered by $Grin token on solana
https://zany-cantaloupe-d3d.notion.site/Cheshire-Terminal-150ea810c1f780ad98d9d7f4e5e9124f
# Cheshire Terminal RAG Swarm Deployment Guide

## Prerequisites
- MacBook Pro M3 Max (36GB RAM)
- Mac Mini M4 (16GB RAM)
- High-speed local network connection
- Python 3.12+
- Docker Desktop
- Solana CLI Tools

## Step 1: Control Center Setup (MacBook Pro)

### 1.1 Environment Setup
```bash
# Create project directory
mkdir cheshire-terminal
cd cheshire-terminal

# Create Python virtual environment
python3 -m venv venv
source venv/bin/activate

# Install base dependencies
pip install mlx langchain astrapy solana langsmith
```

### 1.2 Clone Exo Repository
```bash
git clone https://github.com/8bitsats/exo.git
cd exo
./configure_mlx.sh
```

### 1.3 Configure Control Dashboard
```bash
# Install dashboard dependencies
cd control-center
npm install

# Create environment configuration
cat > .env << EOL
ASTRA_DB_TOKEN=your_token
LANGSMITH_API_KEY=your_key
SOLANA_RPC_URL=http://localhost:8899
EOL
```

## Step 2: Compute Node Setup (Mac Mini)

### 2.1 MLX Configuration
```bash
# Install MLX optimized for Apple Silicon
pip install mlx
pip install mlx-lm

# Configure GPU acceleration
export MLX_USE_GPU=1
```

### 2.2 Neural Network Setup
```python
# neural_network.py
import mlx.core as mx
import mlx.nn as nn

class RAGNetwork(nn.Module):
    def __init__(self):
        super().__init__()
        self.encoder = nn.Sequential(
            nn.Linear(512, 256),
            nn.ReLU(),
            nn.Linear(256, 128)
        )
        self.decoder = nn.Sequential(
            nn.Linear(128, 256),
            nn.ReLU(),
            nn.Linear(256, 512)
        )
```

## Step 3: Data Storage Configuration

### 3.1 AstraDB Setup
```python
# astra_config.py
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider

auth_provider = PlainTextAuthProvider(
    username='token',
    password='AstraCS:your_token'
)

cluster = Cluster(
    cloud={'secure_connect_bundle': 'secure-connect-bundle.zip'},
    auth_provider=auth_provider
)
session = cluster.connect()
```

### 3.2 Vector Store Integration
```python
# vector_store.py
from langchain.vectorstores import Cassandra
from langchain.embeddings import HuggingFaceEmbeddings

embeddings = HuggingFaceEmbeddings()
vector_store = Cassandra(
    embedding=embeddings,
    session=session,
    keyspace="cheshire_cluster"
)
```

## Step 4: Solana Integration

### 4.1 Validator Node Setup
```bash
# Install Solana tools
sh -c "$(curl -sSfL https://release.solana.com/v1.17.0/install)"

# Configure local validator
solana-validator --ledger ledger --rpc-port 8899
```

### 4.2 GRIN Token Setup
```typescript
// token_setup.ts
import { 
    Connection, 
    Keypair,
    TokenProgram 
} from '@solana/web3.js';

const connection = new Connection('http://localhost:8899');
const tokenMint = await TokenProgram.createMint(
    connection,
    adminKeypair,
    adminKeypair.publicKey,
    null,
    9
);
```

## Step 5: LangSmith Integration

### 5.1 Agent Configuration
```python
# agent_config.py
from langsmith import Client
from langchain.agents import create_react_agent
from langchain.memory import AstraDBMemory

client = Client()
memory = AstraDBMemory(
    session=session,
    keyspace="cheshire_memory"
)

agent = create_react_agent(
    llm=llm,
    tools=tools,
    memory=memory
)
```

## Step 6: System Launch

### 6.1 Start Core Services
```bash
# Start Docker services
docker-compose up -d

# Initialize neural network
python neural_network.py

# Launch control dashboard
cd control-center
npm start
```

### 6.2 Verify Deployment
- Access dashboard: http://localhost:3000
- Check Solana validator: solana validators
- Verify AstraDB connection
- Test RAG neural network
- Monitor system metrics

## System Monitoring

### Dashboard Metrics
- GPU Utilization
- Neural Network Performance
- Token Transaction Volume
- Agent Activity Logs
- Storage Usage

### Health Checks
```bash
# Check service status
docker-compose ps

# Monitor logs
docker-compose logs -f

# View neural network metrics
python monitor_network.py
```

## Troubleshooting

### Common Issues
1. MLX GPU Acceleration
   - Ensure MLX_USE_GPU=1 is set
   - Check GPU availability

2. Network Connectivity
   - Verify local network settings
   - Check port forwarding

3. Memory Management
   - Monitor RAM usage
   - Adjust batch sizes

4. Token Integration
   - Verify Solana connection
   - Check token account balance
