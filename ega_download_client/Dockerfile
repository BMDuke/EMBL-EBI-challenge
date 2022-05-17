# Select base image
FROM python:3

# Install pyEGA3 client
RUN pip install pyega3
RUN pip install pyega3 --upgrade

# Set up the working environment
RUN mkdir /app
WORKDIR /app

# Set up alias to handle credentials
RUN echo 'alias pyega3="pyega3 -cf CREDENTIALS_FILE"' >> ~/.bashrc