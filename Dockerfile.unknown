# Use an official Python runtime as a parent image
FROM python:3.11-slim

# Set the working directory in the container
WORKDIR /app

# Copy only necessary files into the container
COPY requirements.txt /app/

# Install any dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application files
COPY . /app

# Specify the command to run on container start
CMD ["python", "app.py"]
