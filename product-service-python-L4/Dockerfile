# # fot single container 
# #Use an official Python runtime as a parent image
# FROM python:3.10

# # Set the working directory
# WORKDIR /usr/src/app

# # Copy requirements.txt
# COPY requirements.txt .

# # Install dependencies
# RUN pip install --no-cache-dir -r requirements.txt

# # Copy the rest of the application code
# COPY . .

# # Expose the service port
# EXPOSE 3030

# # Set environment variables for the product-service
# ENV PORT=3030

# # Start the product-service
# CMD ["python", "app.py"]

FROM python:3.10

WORKDIR /usr/src/app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 3030

ENV PORT=3030

CMD ["python", "app.py"]