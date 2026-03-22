# Order Service
git clone https://github.com/sivamaniselvaraj/order_service.git
cd order_service
## Run
mvn clean package
docker build -t order-service .

# Processing Service
git clone https://github.com/sivamaniselvaraj/processing.git
cd processing
## Run
mvn clean package
docker build -t processing-service .


# Notification Service
git clone https://github.com/sivamaniselvaraj/notification.git
cd notification
## Run
mvn clean package
docker build -t notification-service .

# infra-setup
https://github.com/sivamaniselvaraj/infra-setup.git
cd infra-setup
## Run
docker compose up
