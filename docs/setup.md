# Environment setup

## Getting the database and the ontop client to run using Docker
The first step would be for you to actually get the database with the Open Data Hub (ODH) data running and then having a running instance of our Ontop endpoint which can be used to query the VKG we have built. To facilitate this process and allow everyone to develop in their own machine we make use of Docker. To get started:
   1) Install Docker for Desktop (make sure it comes with Docker Compose)
   2) Modify the .env file if you would like to change port configurations for the Ontop endpoint (DOCKER_SERVER_PORT) and the PostgreDB port (PG_DOCKER_PORT)
   3) Run the containers by using `docker-compose pull && docker-compose up`. Note that if it is your first time doing this it will take some time.
   4) If you want to run your container in the background then add the -d flag to the command above: `docker-compose pull && docker-compose up -d`. You can stop the running containers by using the classic Ctrl+C in your terminal. If you want to stop and remove containers, networks, volumes, and images created by `docker-compose up` run `docker-compose down`.
   5) Now you go to localhost:8080 (or whatever port you specified in the .env file) to see the Ontop endpoint and query the VKG using SPARQL. There is a [set of SPARQL queries](queries) that we provide which you might find interesting and useful. Give them a try if you want.

## Using ngrok to create a safe tunnel to your localhost
The next step now is to make it possible for you Alexa skill to reach the endpoint. Since you are on localhost, Alexa would normally not be able to communicate with the endpoint and this is why we 'll use ngrok to expose our localhost port to the internet over a secure tunnel. In order to do this: 
   1) [Download ngrok](https://ngrok.com/download). The following steps are also listed in the download page, but we will add them here for transparency and comodity.
   2) Unzip the ngrok archive and place the file somewhere to have easy command-line access
   3) Get your auth token by creating a free accout (more details on the ngrok website)
   4) Create a tunnel to your localhost port where the docker container of the Optop endpoint is running (by default 8080) by using the command `./ngrok http 8080`. Keep in mind that you will need to be in the folder where you placed the ngrok executable in step 2 for this command to work and also make sure you know on which port the endpoint is running.
