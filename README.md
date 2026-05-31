# Medical Chatbot with LLMs, LangChain, Pinecone, Flask & AWS
 
> Built with **Groq (LLaMA3)** + **HuggingFace Embeddings** instead of OpenAI — fully free to run locally.
 
---
 
## How to Run
 
### Step 1 — Clone the repository
 
```bash
git clone https://github.com/Ethical-Harsh846/Medical-Chatbot-with-LLMs-LangChain-Pinecone-Flask-AWS.git
```
 
---
 
### Step 2 — Create and activate a conda environment
 
```bash
conda create -n medibot python=3.10 -y
```
```bash
conda activate medibot
```
 
---
 
### Step 3 — Install the requirements
 
```bash
pip install -r requirements.txt
```
 
---
 
### Step 4 — Create a `.env` file in the root directory
 
Add your Pinecone and Groq credentials as follows:
 
```ini
PINECONE_API_KEY=your_pinecone_api_key_here
GROQ_API_KEY=your_groq_api_key_here
```
 
> **Get your free API keys:**
> - Pinecone → [pinecone.io](https://www.pinecone.io/)
> - Groq → [console.groq.com](https://console.groq.com/) (free, no credit card needed)
 
---
 
### Step 5 — Store embeddings to Pinecone
 
```bash
python store_index.py
```
 
> This uses **HuggingFace embeddings** (`sentence-transformers/all-MiniLM-L6-v2`) — no OpenAI key required.
 
---
 
### Step 6 — Run the application
 
```bash
python app.py
```
 
Then open your browser and go to:
```
http://localhost:8080
```
 
---
 
## Tech Stack Used
 
| Component | Technology |
|---|---|
| Language | Python 3.10 |
| Framework | Flask |
| LLM | Groq — LLaMA3 8B (`llama-3.1-8b-instant`) |
| Embeddings | HuggingFace — `all-MiniLM-L6-v2` |
| Vector Database | Pinecone |
| Orchestration | LangChain |
| Deployment | AWS EC2 + Docker + GitHub Actions |
 
---
 
## Why Groq Instead of OpenAI?
 
| | OpenAI | Groq |
|---|---|---|
| Free tier | ❌ No (credits expired) | ✅ Yes |
| Credit card required | ✅ Yes | ❌ No |
| Speed | Fast | Very fast |
| Model used | GPT-3.5/4 | LLaMA3 8B |
 
---
 
## AWS CI/CD Deployment with GitHub Actions
 
### 1. Login to AWS Console
 
### 2. Create IAM User for Deployment
 
Required access:
- **EC2** — virtual machine to run the app
- **ECR** — Elastic Container Registry to store Docker image
Required policies:
```
AmazonEC2ContainerRegistryFullAccess
AmazonEC2FullAccess
```
 
### 3. Create ECR Repository
 
Save the URI after creation, example:
```
315865595366.dkr.ecr.us-east-1.amazonaws.com/medicalbot
```
 
### 4. Create EC2 Machine (Ubuntu)
 
### 5. Install Docker on EC2
 
```bash
# Optional but recommended
sudo apt-get update -y
sudo apt-get upgrade -y
 
# Required
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```
 
### 6. Configure EC2 as Self-Hosted Runner
 
Go to your GitHub repo:
```
Settings → Actions → Runners → New self-hosted runner → Choose OS → Run commands one by one
```
 
### 7. Set Up GitHub Secrets
 
Go to `Settings → Secrets and variables → Actions` and add:
 
```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_DEFAULT_REGION
ECR_REPO
PINECONE_API_KEY
GROQ_API_KEY
```