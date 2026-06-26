FROM python:3.11-slim

WORKDIR /app

RUN apt-get update && apt-get install -y \
    curl \
    iputils-ping \
    telnet \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY slave_plc.py .
COPY config/ /app/config/

HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import socket; socket.create_connection(('localhost', 502), timeout=2)" || exit 1

EXPOSE 502 8080

CMD ["python", "slave_plc.py"]