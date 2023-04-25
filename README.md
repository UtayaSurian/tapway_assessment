# Tapway Assessment

In this docker compose file, it will provide three services for the user such as producer,
queue and consumer. All these three services are dockerized and can be setup easiliy through the docker-compose.yaml file which has been attached in the provided GitHub link. All those three services explained as follows:


## Producer (Service Name: producer_api)

Producer is an API that accepts POST request from sender. Producer is responsible for processing data and
publishing to RabbitMQ queue server. It is developed based on Django framework with REST API concept. 


## Consumer (Service Name: consumer_backend)
Consumer service can be considered as worker for handling queues from RabbitMQ server. The consumer.py is the main file (starting point) where it will keep listening on any available jobs in queue server. It will use worker basis instead round-robin. This is to ensure all consumers able to process jobs. All data will be stored inside **data.csv** in the root folder.
User can access/read the data.csv file by running the following commands:
1) _docker exec -it consumer-backend bash_
2) _ls_

**Note:** The number of consumers is depended on how many consumer containers are running or consumer.py scripts are running at the same time. As per the requirement, only one consumer container is required for this assessment.

## RabbitMQ Server (Service Name: rabbitmq)

The RabbitMQ docker container is responsible for handling incoming message published by producer and allowing
the queue worker (consumer-backend) to process jobs from its queue server.

---

# Setup
**Note:**: In order to setup this project, user should have docker and docker-compose utilities/services installed in their local machine. And all the commands is recommended to run in linux based terminal. For Windows based user,
user still can follow the similar setup but commands will be different. Recommended to use Google to know how to run below commands in Windows CMD terminal.

### Steps
A) Access the GitHub link sent by the developer and git clone or directly download the docker-compose.yaml file into user's local machine.

B) Once docker-compose.yaml file cloned/downloaded into user's local machine, user can run the following command to setup the system:
1) _docker compose up -d_

   **Note:** Run the above command where your docker-compose.yaml is located.

C) Now, user can start to access the RabbitMQ service on its admin webpage by opening the link http://127.0.0.1:15672 on browser. Keep refreshing the page if rabbitmq web management still not loaded in browser. It might take seconds to load the rabbitmq server page. Once page is loaded,
it will ask the user to login. User can login as _guest_ for the username and _guest_ as the password.

D) Once user logged in, user can create queue name as "tapway" with classic/default configuration.
**Note:** The Env file in both producer and consumer already predefined to access the only queue with the name _tapway_. Hence user must create 
queue name based on "tapway" on the web management page of rabbitmq server. If user wants to change the queue name to something else,
user has to modify the queue name env variable in docker-compose.yaml file for producer_api and consumer_backend services, run "docker-compose down" and rerun the docker compose up command to resetup. And also required to add new queue name in Rabbit MQ server that user configured by themselves similar with the queue env.

E) Finally, user can now restart the consumer-backend container since user has recently created 
a new queue in the rabbitmq server. Use this command to restart:
1) _docker restart consumer-backend_

F) Now user can start to post any data into the producer API by using curl command or Postman. Below describes how to send POST request to the producer endpoint in order to publish message into RabbitMQ queue:

**Endpoint:** 
http://127.0.0.1:8000/process-data/

**Sample POST Data:**
_{ "device_id":"", "client_id":"", "created_at":"", "data":{ "license_id":"1fdbb175-81d3-4062-b76d-d274674348e9", "preds":[ { "image_frame":"QW55IGltYWdlIGZyYW1lcyBmcm9tIHNlbmRlcg==", "prob":"0.23", "tags":[] } ] } }_

**Note:** No authentication needed to access the endpoint since it is developed to achieve the basic needs.

G) User can open the Consumer's logs to monitor any queue it has processed after user sent data to producer recently by running the below command on terminal:
1) docker logs -f --tail 200 consumer-backend

___
### Developer's Details:
Name: Utayasurian Rajoo |
Email ID: utayasurian97@gmail.com |
Mobile Number: +60167132394