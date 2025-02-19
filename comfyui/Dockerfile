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

# Expose the port ComfyUI runs on
EXPOSE 8188

# Create startup script
COPY <<'EOF' /app/startup.sh
#!/bin/bash
if [ ! -d "/app/custom_nodes/ComfyUI-Manager" ]; then
    cd /app/custom_nodes
    git clone https://github.com/ltdrdata/ComfyUI-Manager.git
    cd ComfyUI-Manager
    pip install -r requirements.txt
fi
cd /app
python main.py --listen 0.0.0.0 --port 8188
EOF

RUN chmod +x /app/startup.sh

# Start ComfyUI
CMD ["/app/startup.sh"]