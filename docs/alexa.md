# Alexa Skill Development

As mentioned in our presentation, a voice interface like Alexa would make for a very interesting way of querying a Virtual Knowledge Graph (VKG). In this section we will explore how to get started with this idea.

## Environment set-up

### Getting the database and the ontop client to run using Docker
The first step would be for you to actually get the database with the Open Data Hub (ODH) data running and then having a running instance of our Ontop endpoint which can be used to query the VKG we have built. To facilitate this process and allow everyone to develop in their own machine we make use of Docker. To get started:
   1) Install Docker for Desktop (make sure it comes with Docker Compose)
   2) Modify the .env file if you would like to change port configurations for the Ontop endpoint (DOCKER_SERVER_PORT) and the PostgreDB port (PG_DOCKER_PORT)
   3) Run the containers by using `docker-compose pull && docker-compose up`. Note that if it is your first time doing this it will take some time.
   4) If you want to run your container in the background then add the -d flag to the command above: `docker-compose pull && docker-compose up -d`. You can stop the running containers by using the classic Ctrl+C in your terminal. If you want to stop and remove containers, networks, volumes, and images created by `docker-compose up` run `docker-compose down`.
   5) Now you go to localhost:8080 (or whatever port you specified in the .env file) to see the Ontop endpoint and query the VKG using SPARQL. There is a [set of SPARQL queries](queries.md) that we provide which you might find interesting and useful. Give them a try if you want.

### Using ngrok to create a safe tunnel to your localhost
The next step now is to make it possible for you Alexa skill to reach the endpoint. Since you are on localhost, Alexa would normally not be able to communicate with the endpoint and this is why we 'll use ngrok to expose our localhost port to the internet over a secure tunnel. In order to do this: 
   1) [Download ngrok](https://ngrok.com/download). The following steps are also listed in the download page, but we will add them here for transparency and comodity.
   2) Unzip the ngrok archive and place the file somewhere to have easy command-line access
   3) Get your auth token by creating a free accout (more details on the ngrok website)
   4) Create a tunnel to your localhost port where the docker container of the Optop endpoint is running (by default 8080) by using the command `./ngrok http 8080`. Keep in mind that you will need to be in the folder where you placed the ngrok executable in step 2 for this command to work and also make sure you know on which port the endpoint is running.


## Alexa Skills

In the odh-alexa folder we can distinguish the two main components that make up an Alexa Skill:

1. models

    The models folder contains inside a .json file named after the locale specification. This file is the interaction model of our skill, basically where we define the skill logic and also the VI (voice interface). You can edit the .json file to make changes to the model but we highly suggest using the Alexa developer console and loading the file there so that you can rely on the GUI, it makes the process much simpler. More on skill models can be found [here](https://developer.amazon.com/docs/custom-skills/create-the-interaction-model-for-your-skill.html) and [here](https://developer.amazon.com/docs/devconsole/create-a-skill-and-choose-the-interaction-model.html).

2. lambda

    This directory contains the code which handles how Alexa responds to intents defined in the interaction model explained above. You can see that there are four files inside:
    - data.py:

         Contains static variables and the queries that the main code file (lambda_function.py) will use

    - lambda_function.py:

         This is the "main code file" that will contain all the functions which will dictate how Alexa responds and also how we handle user intent. Inside this file you are essentially deciding how your skill will operate. It is the most important part of skill development along with the interaction model so read more about the actual technology [here](https://developer.amazon.com/docs/custom-skills/host-a-custom-skill-as-an-aws-lambda-function.html) and more about the actual coding by following [this Amazon tutorial](https://developer.amazon.com/en-US/alexa/alexa-skills-kit/resources/training-resources/cake-walk/cake-walk-1)

    - utils.py:

         This file is created when you initially create the skill in the Alexa developer console as a Python project and it is basically needed to generate a presigned URL to share an S3 object (which is where you can persist memory changes for Alexa hosted skills). You can see how your Alexa hosted S3 bucket can help the skill [here](https://developer.amazon.com/en-US/alexa/alexa-skills-kit/resources/training-resources/cake-walk/cake-walk-5)

    - requirements.txt:

         Contains all the external python modules we want to install for the skill to run. All of the module installation is handled by Lambda so all we do here is just specify the module names. If you have worked with Python before then you will be quite familiar with this file.


    ### Amazon ASK CLI
    This project was downloaded from the Alexa development console using the ASK CLI from amazon. To understand how to use it in regard to a skill and how it helps development and testing you can check [here](https://developer.amazon.com/docs/hosted-skills/build-a-skill-end-to-end-using-an-alexa-hosted-skill.html#askcli).
    We highly reccomend reading through all the links provided here since the Alexa skill building workflow is not "the most typical". Hopefully after reading the Amazon tutorials and guides you will have a clearer idea of how the whole process works and be able to contribute or create your own skills.

    ### Development Ideas
    The first step towards developing a new skill would be to create a new one from the Alexa developer console. We suggest using the console to build and edit the model but after creating the new skill you can either copy paste the code in models/ and lambda/ in the respesctively the Build and Code tabs inside the developer console or use the ask cli to clone the skill on your computer and copy paste the code there. Afterwards you can deploy the skill to the developer console using the cli. If you wish to use the ask cli then your workflow could follow roughly the following pattern:
    1) Initialize the ASK-CLI with your Amazon developer account credentials using `ask init`
    2) Clone the skill you created in the developer console: `ask clone`
    3) Add and commit the changes made in dev and then merge dev to master by using `ask deploy`
    4) My suggestion: edit the model using the developer console and you can edit the code locally. You can then work on the code on your favourite edit and deploy only the code by using `ask deploy -t lambda` so that the deployment only affects the model

    ### Useful links:
      - [Alexa: Skill building basics](https://developer.amazon.com/docs/custom-skills/steps-to-build-a-custom-skill.html)
      - [Alexa: Skill building tutorial from scratch](https://developer.amazon.com/en-US/alexa/alexa-skills-kit/resources/training-resources/cake-walk)
      - [Alexa: Skill building from a template](https://developer.amazon.com/docs/custom-skills/create-custom-skill-from-quick-start-template.html)
      - [Alexa: Template for a skill on Github](https://github.com/alexa/skill-sample-python-quiz-game)
      - [Python SPARQL](https://rdflib.github.io/sparqlwrapper/)

