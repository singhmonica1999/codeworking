FROM python:alpine3.19

WORKDIR /app
# Copy the requirements file into the container at /app
COPY requirements.txt /app/

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

COPY . /app/

# Specify the command to run on container start
CMD ["python", "app.py"]