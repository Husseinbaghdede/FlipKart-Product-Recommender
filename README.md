# 🛍️ Flipkart Product Recommendation RAG Chatbot

An intelligent e-commerce chatbot that provides personalized product recommendations based on real Flipkart customer reviews using Retrieval-Augmented Generation (RAG) architecture.

## 🎯 Overview

This project implements a conversational AI system that helps users make informed purchasing decisions by analyzing authentic product reviews and providing contextual recommendations. The system combines semantic search with large language models to deliver accurate, review-based insights.

## ✨ Features

- **🧠 RAG Architecture**: Semantic search through product reviews with LLM-powered responses
- **💬 Conversational Memory**: Maintains chat history for contextual follow-up questions
- **📊 Real-time Analytics**: Request tracking and prediction metrics with Prometheus
- **🔄 Session Management**: Personalized conversation history per user
- **🚀 Production Ready**: Containerized deployment with Kubernetes orchestration
- **📈 Monitoring Dashboard**: Grafana visualization for system metrics

## 🏗️ Architecture

```
User Query → Flask API → RAG Chain → Vector Search (AstraDB) → LLM (Groq) → Response
                ↓
        Prometheus Metrics → Grafana Dashboard
```

## 🛠️ Tech Stack

### Backend & AI
- **Framework**: Flask (Python 3.10)
- **RAG Pipeline**: LangChain
- **Vector Database**: AstraDB
- **Embeddings**: HuggingFace (BAAI/bge-base-en-v1.5)
- **LLM**: Groq API (Llama-3.1-8b-instant)

### Deployment & Infrastructure
- **Containerization**: Docker
- **Cloud Platform**: Google Cloud Platform (GCP VM)
- **Orchestration**: Kubernetes
- **Monitoring**: Prometheus + Grafana
- **Service**: LoadBalancer with health checks

## 🚀 Quick Start

### Prerequisites
- Python 3.10+
- Docker & Docker Compose
- GCP Account (for deployment)
- API Keys: Groq, AstraDB, HuggingFace

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd flipkart-rag-chatbot
   ```

2. **Environment Setup**
   ```bash
   # Create .env file
   cp .env.example .env
   
   # Add your API keys
   ASTRA_DB_API_ENDPOINT=your_astra_endpoint
   ASTRA_DB_APPLICATION_TOKEN=your_astra_token
   ASTRA_DB_KEYSPACE=your_keyspace
   GROQ_API_KEY=your_groq_key
   ```

3. **Install Dependencies**
   ```bash
   pip install -e .
   ```

4. **Data Ingestion** (First time only)
   ```bash
   python -c "from flipkart.data_ingestion import DataIngestor; DataIngestor().ingest(load_existing=False)"
   ```

5. **Run Application**
   ```bash
   python app.py
   ```

Visit `http://localhost:5000` to access the chatbot interface.

## 🐳 Docker Deployment

### Build and Run
```bash
# Build Docker image
docker build -t flipkart-chatbot .

# Run container
docker run -p 5000:5000 --env-file .env flipkart-chatbot
```

### Docker Compose
```bash
docker-compose up -d
```

## ☁️ Production Deployment (GCP + Kubernetes)

### 1. Build and Push Image
```bash
# Build for production
docker build -t gcr.io/your-project/flipkart-chatbot .

# Push to registry
docker push gcr.io/your-project/flipkart-chatbot
```

### 2. Deploy to Kubernetes
```bash
# Create namespace
kubectl create namespace monitoring

# Deploy secrets
kubectl create secret generic llmops-secrets \
  --from-env-file=.env

# Deploy application
kubectl apply -f flask-deployment.yaml

# Deploy monitoring
kubectl apply -f prometheus/
kubectl apply -f grafana/
```

### 3. Access Services
```bash
# Get LoadBalancer IP
kubectl get service flask-service

# Access monitoring
kubectl get service -n monitoring
```

## 📊 Monitoring & Metrics

### Available Metrics
- `http_requests_total`: Total HTTP requests received
- `prediction_requests_count`: Total prediction requests made

### Grafana Dashboards
- Request rate and response times
- Prediction accuracy metrics
- System health monitoring

Access Grafana at `http://<node-ip>:32000`

## 📁 Project Structure

```
├── flipkart/                 # Core application package
│   ├── config.py            # Configuration management
│   ├── data_converter.py    # CSV to Document conversion
│   ├── data_ingestion.py    # Vector DB ingestion
│   └── rag_chain.py         # RAG pipeline implementation
├── templates/               # HTML templates
├── static/                  # CSS and frontend assets
├── prometheus/              # Monitoring configuration
├── grafana/                 # Dashboard configuration
├── utils/                   # Utilities (logging, exceptions)
├── app.py                   # Flask application
├── Dockerfile              # Container configuration
└── requirements.txt        # Python dependencies
```

## 🔧 Configuration

### Environment Variables
```bash
ASTRA_DB_API_ENDPOINT=         # AstraDB endpoint URL
ASTRA_DB_APPLICATION_TOKEN=    # AstraDB authentication token
ASTRA_DB_KEYSPACE=            # Database keyspace name
GROQ_API_KEY=                 # Groq API key for LLM
```

### Model Configuration
- **Embedding Model**: `BAAI/bge-base-en-v1.5`
- **LLM Model**: `llama-3.1-8b-instant`
- **Temperature**: `0.5`
- **Retrieval K**: `3` documents

## 🛡️ Security

- API keys managed through Kubernetes secrets
- No sensitive data in container images
- Environment-based configuration
- Secure container practices

## 📈 Performance

- **Response Time**: ~2-3 seconds per query
- **Concurrent Users**: Scales with Kubernetes replicas
- **Memory Usage**: ~500MB per instance
- **Vector Search**: Sub-second semantic retrieval

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


---

⭐ **Star this repo if you found it helpful!**
