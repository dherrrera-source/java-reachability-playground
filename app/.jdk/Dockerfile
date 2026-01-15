# Dockerfile.vuln (INTENCIONALMENTE INSEGURO - SOLO DEMO/LAB)
# Objetivo: generar hallazgos claros en Trivy IaC + Trivy Image

# 1) Imagen base sin pinning por digest
FROM ubuntu:latest

# 2) FORZAR ejecución como root (mala práctica explícita)
USER root

# 3) Instalación de paquetes sin version pinning y con herramientas innecesarias
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    openssh-server \
    sudo \
    netcat-traditional \
    && rm -rf /var/lib/apt/lists/*

# 4) Secretos hardcodeados (solo para detección)
ENV AWS_ACCESS_KEY_ID="AKIAFAKEEXAMPLE1234"
ENV AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYFAKEEXAMPLEKEY"
ENV DB_PASSWORD="P@ssw0rd123!"

# 5) Descarga y ejecución de script remoto sin verificación
RUN curl -sSL https://example.com/install.sh | bash || true

# 6) Permisos excesivos
RUN mkdir -p /app /data && chmod -R 777 /app /data

# 7) Usuario con sudo sin password (mala práctica)
RUN useradd -m appuser && echo "appuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 8) Habilitar SSH y credenciales débiles (solo demo)
RUN mkdir -p /var/run/sshd && \
    echo 'root:root' | chpasswd

# 9) Copiar todo el repositorio (riesgo de filtración)
WORKDIR /app
COPY . /app

# 10) Exponer puerto innecesario
EXPOSE 22

# 11) Ejecutar servicio como root
CMD ["/usr/sbin/sshd", "-D"]
