# Select base image
FROM maven:3.8.5-jdk-11

# Install required libraries
RUN apt update && apt upgrade -y
RUN apt-get install libfuse-dev -y

RUN apt-get install bcftools -y 
RUN apt-get install systemd -y

# Set up the working environment
WORKDIR /
RUN mkdir /tmp/mnt /app 
WORKDIR /app

# Build the project
RUN git clone https://github.com/EGA-archive/ega-fuse-client.git
WORKDIR /app/ega-fuse-client
RUN mvn clean install

# Create aliases for ease of use
RUN echo 'alias fuse_start=$"./fuseclient.sh start \"--c=${C} --cf=${CF} --cpf=${CPF} --cache=${CACHE} --t=${T}\""' >> ~/.bashrc
RUN echo 'alias fuse_stop="./fuseclient.sh stop"' >> ~/.bashrc
RUN echo 'alias fuse_restart=$"./fuseclient.sh restart \"--c=${C} --cf=${CF} --cpf=${CPF} --cache=${CACHE} --t=${T}\""' >> ~/.bashrc
