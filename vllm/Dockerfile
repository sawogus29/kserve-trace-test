# Using custom vllm image to support NVIDIA PASCAL GPU
FROM ghcr.io/sasha0552/vllm:v0.6.2

# Copy the requirements.txt file into the container
COPY requirements.txt /tmp

# Install the required opentelemetry packages 
RUN pip install --no-cache-dir -r /tmp/requirements.txt
