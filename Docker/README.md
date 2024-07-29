
*To install Docker and run your Maven project in a Docker container, follow these steps:*
## *Step 1: Install Docker*
*1.1 Update Package Information*</br>
```javascript
sudo apt-get update
```
*1.2 Install Required Packages*
```javascript
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

```
*1.3 Add Docker’s Official GPG Key*

```javascript
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

```
*1.4 Set Up the Stable Repository*
```javascript
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

*1.5 Install Docker*
```javascript
sudo apt-get update
sudo apt-get install docker-ce

```
*1.6 Verify Docker Installation*
```javascript
sudo docker --version

```
## *Step 2: Create a Dockerfile for Your Maven Project*
```javascript
cd~/myapp //root directory
```
*Create a file named Dockerfile in the root directory of your Maven project with the following content:*
```javascript
# Use an ARM64-compatible Maven image as a parent image
FROM arm64v8/maven:3.8.4-openjdk-11

# Set the working directory
WORKDIR /usr/src/app

# Install necessary packages
RUN apt-get update \
    && apt-get install -y \
        firefox-esr \
        wget \
        bzip2 \
        xvfb \GF
        libdbus-glib-1-2 \
    && rm -rf /var/lib/apt/lists/*

# Install GeckoDriver v0.33.0 (ARM64 version)
RUN wget -q https://github.com/mozilla/geckodriver/releases/download/v0.33.0/geckodriver-v0.33.0-linux-aarch64.tar.gz \
    && tar -xzf geckodriver-v0.33.0-linux-aarch64.tar.gz -C /usr/local/bin/ \
    && rm geckodriver-v0.33.0-linux-aarch64.tar.gz \
    && chmod +x /usr/local/bin/geckodriver

# Copy the pom.xml file and download dependencies
COPY pom.xml .

# Download dependencies
RUN mvn dependency:resolve

# Copy the rest of the application
COPY . .

# Build the application
RUN mvn clean package

# Run the tests
CMD ["mvn", "test"]
```
OR AWS
```javascript
# Use an ARM64-compatible Maven image as a parent image
FROM arm64v8/maven:3.8.4-openjdk-11

# Set the working directory
WORKDIR /usr/src/app

# Install necessary packages
RUN apt-get update \
    && apt-get install -y \
        firefox-esr \
        wget \
        bzip2 \
        xvfb \GF
        libdbus-glib-1-2 \
    && rm -rf /var/lib/apt/lists/*

# Install GeckoDriver v0.33.0 (ARM64 version)
RUN wget -q https://github.com/mozilla/geckodriver/releases/download/v0.33.0/geckodriver-v0.33.0-linux-aarch64.tar.gz \
    && tar -xzf geckodriver-v0.33.0-linux-aarch64.tar.gz -C /usr/local/bin/ \
    && rm geckodriver-v0.33.0-linux-aarch64.tar.gz \
    && chmod +x /usr/local/bin/geckodriver

# Copy the pom.xml file and download dependencies
COPY pom.xml .

# Download dependencies
RUN mvn dependency:resolve

# Copy the rest of the application
COPY . .

# Build the application
RUN mvn clean package

# Run the tests
CMD ["mvn", "test"]

```
## *Step 3: Build the Docker Image*
*Navigate to the root directory of your Maven project and build the Docker image:*
```javascript
sudo docker build -t myapp .

```

## *Step 4: Run the Docker Container*
*Run the Docker container, ensuring it maps to an available port on your host system. If port 8080 is occupied, use another port like 8081.*
```javascript
sudo docker run --rm myapp
    or aws
sudo docker run --rm -p 8081:8080 myapp 

```
*Docker  Clean Space :*
```javascript
sudo docker container prune -f
sudo docker image prune -a -f
sudo docker volume prune -f
sudo docker network prune -f
sudo docker system prune -a -f

```
## *Step 5: Stop Docker Service*
*First, stop the Docker service to ensure that all Docker containers and processes are terminated.*
```javascript
sudo systemctl stop docker
```

