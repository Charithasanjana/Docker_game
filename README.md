# Docker_game
Project Documentation: Dockerized 2048 Game Deployment on AWS

# 1. Introduction:

This project involves containerizing a simple game, 2048, within a Docker container, and deploying it to AWS using Elastic Beanstalk. The project demonstrates containerization with Docker, cloud deployment, and basic web application development.

Project Objectives:
Create a Docker container for hosting the 2048 game.
Deploy the Docker container to AWS using Elastic Beanstalk.
Learn DevOps concepts like containerization, image building, and cloud deployment.
Tools & Technologies Used:
Docker: For containerization of the game application.
AWS Elastic Beanstalk: For cloud deployment.
Ubuntu 22.04: Base Docker image.
Nginx: Web server to serve the game.
GitHub: Hosting the game repository.
Curl & Zip utilities: For downloading and extracting game files.

# 2. Architecture Overview:

The project architecture consists of two main components:

Local Development Environment (Docker):
The Docker container hosts the 2048 game, using an Nginx server to serve the game via a web interface.
The Docker container listens on port 80.

Cloud Deployment (AWS Elastic Beanstalk):
The Docker container is deployed to AWS using Elastic Beanstalk.
Elastic Beanstalk manages the deployment, scaling, and monitoring of the application.

# 3. Step-by-Step Implementation:
   
Step 1: Setting Up the Project Directory
Create a new folder for the project:
bash
code:
mkdir 2048-game
cd 2048-game

Step 2: Dockerfile Creation
Create a Dockerfile in the project directory with the following content:
Dockerfile
code:
Use Ubuntu 22.04 as base image
FROM ubuntu:22.04

 Update and install necessary packages
RUN apt-get update
RUN apt-get install -y nginx zip curl

 Configure Nginx
RUN echo "daemon off;" >>/etc/nginx/nginx.conf

 Download and unzip the game files
RUN curl -o /var/www/html/master.zip -L https://codeload.github.com/gabrielecirulli/2048/zip/master
RUN cd /var/www/html/ && unzip master.zip && mv 2048-master/* . && rm -rf 2048-master master.zip

 Expose port 80 for web traffic
EXPOSE 80

 Start the Nginx service
CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]

Step 3: Build Docker Image
Build the Docker image using the following command:
bash
code
docker build -t 2048-game .

Step 4: Run the Docker Container Locally
Run the Docker container on port 80:
bash
Copy code
docker run -d -p 80:80 <image id>
Verify the game is accessible by opening a browser and navigating to http://localhost.

# 4. Deploying to AWS Elastic Beanstalk:

Step 1: Prepare for AWS Deployment
Log in to AWS Console and navigate to Elastic Beanstalk.

Create a new Elastic Beanstalk application:

Choose the application name (e.g., 2048-game).
Select the environment as Docker.
Upload the Dockerfile from your local device:
In the Elastic Beanstalk console, select the "Upload your code" option and upload the Dockerfile.

Step 2: Configure the Elastic Beanstalk Environment
Choose the platform as Docker and ensure you are using the latest version.
You can leave the environment configuration as default or customize based on your needs.

Step 3: Deploy the Application
After uploading the Dockerfile, Elastic Beanstalk will automatically deploy the container.
Once deployment is complete, Elastic Beanstalk will provide a URL where the game can be accessed online.

# 5. Testing the Application:
   
After the Elastic Beanstalk deployment is finished, visit the provided URL (e.g., http://clone-env.eba-tgnemzht.us-east-1.elasticbeanstalk.com/).
Ensure the 2048 game loads correctly and can be played in the browser.

# 6. Troubleshooting:

Common Issues:

Docker Image Build Failures: Ensure that all commands in the Dockerfile execute without errors (e.g., installing dependencies, downloading the game).

Elastic Beanstalk Deployment Issues: Ensure that the Docker image is correctly configured, and check the Elastic Beanstalk logs for any errors.

Useful Commands for Debugging:

View Docker image logs
bash
code
docker logs <container_id>
Check AWS Elastic Beanstalk logs
Navigate to the Elastic Beanstalk console and view logs for any deployment errors.

# 7. Conclusion:

This project successfully demonstrates how to containerize a simple game using Docker and deploy it on AWS using Elastic Beanstalk. It serves as a basic yet effective example for understanding the integration of containerization and cloud services in a DevOps workflow.

Additional Steps:

Implement CI/CD pipelines for automated Docker image builds and deployments.
Enhance the game with additional features or interactive elements.
Explore using AWS services like Amazon RDS for game data storage or Amazon CloudWatch for monitoring.

# 8. References:

Docker Documentation--https://docs.docker.com/reference/dockerfile/

AWS Elastic Beanstalk Documentation---https://docs.aws.amazon.com/elastic-beanstalk/

GitHub Repository for 2048 Game--https://github.com/gabrielecirulli/2048
