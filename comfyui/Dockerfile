FROM python:3.12-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    libgl1-mesa-glx \
    libglib2.0-0 \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Clone ComfyUI repository
RUN git clone https://github.com/comfyanonymous/ComfyUI.git .

# Install PyTorch with CUDA support
RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126

# Install Python dependencies
RUN pip install -r requirements.txt

# Create necessary directories
RUN mkdir -p /app/models/checkpoints \
    /app/models/vae \
    /app/models/loras \
    /app/models/embeddings \
    /app/models/controlnet \
    /app/models/upscale_models \
    /app/output \
    /app/input

# Expose the port ComfyUI runs on
EXPOSE 8188

# Start ComfyUI
CMD ["python", "main.py", "--listen", "0.0.0.0", "--port", "8188"]