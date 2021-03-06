# **Deployer**

Scripts able to deploy an application

## **Prerequisites**

* Python 3 & Pip 3
* Ansible 2.8+
* RabbitMQ
  * The queue to receive a deployment order is `com.dorefactor.deploy.command`

## **Manual Testing**

### **Services**

* [rd-api](https://github.com/dorefactor/rd-api)

### **Steps**

* **[Requited]. You must provide the following as environment variables:**

  * **RD_API_URL. [rd-api](https://github.com/dorefactor/rd-api)**
  * **RABBITMQ_USER**
  * **RABBITMQ_PASSWORD**

* Listen queue in RabbitMQ to deploy an application

```sh
python3 deployer/rabbitmq_listener.py
```

* Execute a deployment
  * Send a message to `com.dorefactor.deploy.command` queue in RabbitMQ

### **Docker**

* Build an image

```sh
docker build -t rd-deployer:1.0 .
```

* Run the container

  * Command-line

  ```sh
  docker run --rm --name rd-deployer --network bridge --add-host rabbitmq-host:192.168.99.1 -e RD_API_URL=${RD_API_URL} -e RABBITMQ_USER=${RABBITMQ_USER} -e RABBITMQ_PASSWORD=${RABBITMQ_PASSWORD} rd-deployer:1.0
  ```

  * Docker Compose

  ```sh
  version: '3.7'

  services:

    deployer:
      image: rd-deployer:1.0
      container_name: rd-deployer
      environment:
        RD_API_URL: ${RD_API_URL}
        RABBITMQ_USER: ${RABBITMQ_USER}
        RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD}
      extra_hosts: 
        - rabbitmq-host:192.168.99.1
  ```
