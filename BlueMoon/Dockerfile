# Base image with Java 23
FROM maven:3.9.9-eclipse-temurin-23

# Metadata
LABEL author="Lu Thanh Duong"
LABEL version="1.0"

# Set working directory
WORKDIR /app

# Copy entire project
COPY . /app

# Install required packages for JavaFX in container
RUN apt-get update && apt-get install -y \
    mesa-utils \
    libgl1 \
    libx11-6 \
    libxext6 \
    libxtst6 \
    libxrender1 \
    libxi6 \
    libxxf86vm1 \
    && rm -rf /var/lib/apt/lists/*

# Build the application
RUN mvn clean package -DskipTests

# Configure display for Windows with VcXsrv
ENV DISPLAY=host.docker.internal:0

# Allow connections to X server
CMD mvn javafx:run