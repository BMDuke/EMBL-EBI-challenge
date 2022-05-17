# Select base image
FROM maven:3.8.5-jdk-11

# Install required libraries
RUN apt update && apt upgrade -y
RUN apt-get install libfuse-dev -y

# Set up the working environment
RUN mkdir /app
WORKDIR /app

# Build the project
RUN git clone https://github.com/EGA-archive/ega-fuse-client.git
WORKDIR /app/ega-fuse-client
RUN mvn clean install

RUN mkdir /tmp/mnt
# Create aliases for ease of use
#### Add commands to extract version from target/ rather than hardcode 
# RUN echo 'alias fuse=$"./fuseclient.sh start \"--cf=test_credentials.txt\""' >> ~/.bashrc
RUN echo 'alias fuse_start="java -jar target/ega-fuse-client-2.1.1-SNAPSHOT.jar --cf=test_credentials.txt --m=/tmp/mnt --cache=1000 --c=4 --cpf=4"' >> ~/.bashrc