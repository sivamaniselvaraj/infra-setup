## execute the below comments in command prompt or terminal
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

# Product Service
git clone https://github.com/sivamaniselvaraj/productservice.git
cd ProductService
## Run
mvn clean package
docker build -t product-service .


# infra-setup
https://github.com/sivamaniselvaraj/infra-setup.git
cd infra-setup
## Run
docker compose up

# UI
https://github.com/sivamaniselvaraj/order-ui.git
cd order-ui

## Run

# npm run build

docker build -t angular-docker-image .
or
npm run docker-build

# Remove the stopped container
docker rm order-ui

docker run --name order-ui -p 8080:80 angular-docker-image
or 
npm run docker-run