# app/Dockerfile

FROM python:3.10-slim
RUN apt-get update && apt-get install -y curl && apt-get clean
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt

EXPOSE 5000
CMD ["python", "app.py"]

