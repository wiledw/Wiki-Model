# GPT-2 Fine-tuning on WikiText

This project fine-tunes GPT-2 on the WikiText-2 dataset to improve its ability to generate Wikipedia-style content.

Our end goal after fine-tuning is to deploy the model in AWS
![alt text](image.png)

## Project Overview

- Fine-tuned GPT-2 model on WikiText-2 dataset
- FastAPI endpoint for text generation
- Docker containerization
- Ready for AWS deployment
- Includes both training and inference capabilities

## Training Details

- Base model: GPT-2 (small)
- Dataset: WikiText-2-raw-v1
- Training epochs: 3
- Final training loss: 3.096
- Final test loss: 3.446
- Training time: ~9.1 hours

### Training Progress
- Initial loss: ~4.23
- Final loss: 3.096
- Test loss: 3.446

The small gap between training and test loss (0.35) indicates good generalization without overfitting.

## Getting Started

### Local Installation
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME

# Install dependencies
pip install -r requirements.txt
```

### Docker Installation
```bash
# Build the Docker image
docker build -t gpt2-wiki-api .

# Run the container
docker run -p 8000:8000 gpt2-wiki-api
```

The API will be available at `http://localhost:8000`

### Training the Model
To train the model from scratch:
```bash
python model_script.py
```
This will:
1. Download the WikiText-2 dataset
2. Fine-tune GPT-2 for 3 epochs
3. Save the model to ./gpt2_finetuned/

## API Usage

### Starting the API

#### Using Python directly:
```bash
# Start the FastAPI server
uvicorn api:app --reload
```

#### Using Docker:
```bash
# Run the container
docker run -p 8000:8000 gpt2-wiki-api
```

The API will be available at `http://localhost:8000`

### API Endpoints

1. **Root Endpoint**
```bash
GET /
# Returns API information and available endpoints
```

2. **Health Check**
```bash
GET /health
# Returns API and model health status
```

3. **Text Generation**
```bash
POST /generate
```
Request body:
```json
{
    "prompt": "Your text prompt here",
}
```
Response:
```json
{
    "generated_text": "Generated text here",
    "prompt": "Your original prompt"
}
```

### Example API Usage

Using curl:
```bash
curl -X POST "http://localhost:8000/generate" \
     -H "Content-Type: application/json" \
     -d '{"prompt": "The history of artificial intelligence"}'
```

## Directory Structure
```
.
├── app/
│   ├── model_script.py   # Training script
│   ├── test_model.py    # Text generation script
│   └── api.py          # FastAPI application
├── gpt2_finetuned/     # Fine-tuned model directory
├── Dockerfile          # Docker configuration
├── .dockerignore       # Docker ignore file
├── requirements.txt    # Python dependencies
└── README.md          # This file
```

## Docker Configuration
The project includes a Dockerfile that:
- Uses Python 3.11-slim as base image
- Installs all required dependencies
- Sets up the application environment
- Exposes port 8000 for the API
- Runs the FastAPI server

To build and run with Docker:
```bash
# Build the image
docker build -t gpt2-wiki-api .

# Run the container
docker run -p 8000:8000 gpt2-wiki-api

# Run with GPU support (if available)
docker run --gpus all -p 8000:8000 gpt2-wiki-api
```

## Training Configuration
```python
training_args = TrainingArguments(
    num_train_epochs=3,
    per_device_train_batch_size=4,
    learning_rate=5e-5,
    weight_decay=0.01,
    warmup_steps=500
)
```

## Example Usage
```python
from transformers import GPT2LMHeadModel, GPT2Tokenizer

# Train the model first or download pre-trained weights
model = GPT2LMHeadModel.from_pretrained("./gpt2_finetuned")
tokenizer = GPT2Tokenizer.from_pretrained("./gpt2_finetuned")

# Generate text
prompt = "The history of artificial intelligence"
# See test_model.py for full generation code
```
