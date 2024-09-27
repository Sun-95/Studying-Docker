<h1>Getting Started</h1>
<h2>Setting up your computer</h2>
Once you are done installing Docker, test your Docker installation by running the following: <a href="screenshots/1.png">Screenshot 1</a>

<h1>Hello World</h1>
<h2>Playing with Busybox</h2>
In this section, we are going to run a Busybox container on our system and get a taste of the docker run command. To get started, let's run the following in our terminal: <a href="screenshots/2.png">Screenshot 2</a> The pull command fetches the busybox image from the Docker registry and saves it to our system. You can use the docker images command to see a list of all images on your system. <a href="screenshots/3.png">Screenshot 3</a>
<h2>Docker Run</h2>
Let's now run a Docker container based on this image. To do that we are going to use the almighty docker run command. <a href="screenshots/4.png">Screenshot 4</a> Behind the scenes, a lot of stuff happened. When you call run, the Docker client finds the busybox, loads up the container and then runs a command in that container. When we run docker run busybox, we didn't provide a command, so the container booted up, ran an empty command and then exited. <a href="screenshots/5.png">Screenshot 5</a> Finally we see some output. In this case, the Docker client dutifully ran the echo command in our busybox container and then exited it. If you've noticed, all of that happened pretty quickly. The docker ps command shows you all containers that are currently running. <a href="screenshots/6.png">Screenshot 6</a> Since no containers are running, we see a blank line. Let's try a more useful variant: docker ps -a <a href="screenshots/7.png">Screenshot 7</a> So what we see above is a list of all containers that we ran. Running the run command with the -it flags attaches us to an interactive tty in the container. Now we can run as many commands in the container as we want. <a href="screenshots/8.png">Screenshot 8</a> Leaving homeless containers will eat up disk space. So, as a rule of thumb, you should clean up containers when you're done working with them. To do this, you can run the docker rm command. Just copy the container IDs from above and paste them next to the command. <a href="screenshots/9.png">Screenshot 9</a> 

<h1>Webapps with Docker</h1>
<h2>Static Sites</h2>
The image that we are going to use is a single-page website that I've already created for the purpose of this demo and hosted on the registry - prakhar1989/static-site. We can download and run the image directly in one go using docker run. As noted above, the --rm flag automatically removes the container when it exits and the -it flag specifies an interactive terminal which makes it easier to kill the container with Ctrl+C. <a href="screenshots/10.png">Screenshot 10</a> Since the image doesn't exist locally, the client will first fetch the image from the registry and then run the image. If all goes well, you should see a Nginx is running... message in your terminal. The client is not exposing any ports so we need to re-run the docker run command to publish ports. While we're at it, we should also find a way so that our terminal is not attached to the running container. This way, you can happily close your terminal and keep the container running. This is called detached mode. <a href="screenshots/11.png">Screenshot 11</a> In the above command, -d will detach our terminal, -P will publish all exposed ports to random ports and finally --name corresponds to a name we want to give. Now we can see the ports by running the docker port [CONTAINER] command. <a href="screenshots/12.png">Screenshot 12</a> You can also specify a custom port to which the client will forward connections to the container. <a href="screenshots/13.png">Screenshot 13</a> <a href="screenshots/14.png">Screenshot 14</a> To stop a detached container, run docker stop by giving the container ID. In this case, we can use the name static-site we used to start the container. <a href="screenshots/15.png">Screenshot 15</a> 
<h2>Docker Images</h2>
Docker images are the basis of containers. In the previous example, we pulled the Busybox image from the registry and asked the Docker client to run a container based on that image. To see the list of images that are available locally, use the docker images command. <a href="screenshots/16.png">Screenshot 16</a> 
<h2>Our First Image</h2>
It's time to create our own image. Our goal in this section will be to create an image that sandboxes a simple Flask application. For the purposes of this workshop, Its already created a fun little Flask app that displays a random cat .gif every time it is loaded. If you haven't already, please go ahead and clone the repository locally like so  <a href="screenshots/17.png">Screenshot 17</a> The next step now is to create an image with this web app. As mentioned above, all user images are based on a base image. Since our application is written in Python, the base image we're going to use will be Python 3.
<h2>Dockerfile</h2>
Now that we have our Dockerfile, we can build our image. The docker build command does the heavy-lifting of creating a Docker image from a Dockerfile. <a href="screenshots/18.png">Screenshot 18</a>
<h2>Docker push</h2>
If this is the first time you are pushing an image, the client will ask you to login. Provide the same credentials that you used for logging into Docker Hub. <a href="screenshots/19.png">Screenshot 19</a> To publish, just type the below command remembering to replace the name of the image tag above with yours. It is important to have the format of yourusername/image_name so that the client knows where to publish. <a href="screenshots/20.png">Screenshot 20</a> 

<h1>Multi-container Environments</h1>
<h2>Food Trucks</h2>
First up, let's clone the repository locally. <a href="screenshots/21.png">Screenshot 21</a> Let's think of how we can Dockerize the app. We can see that the application consists of a Flask backend server and an Elasticsearch service. A natural way to split this app would be to have two containers - one running the Flask process and another running the Elasticsearch (ES) process. That way if our app becomes popular, we can scale it by adding more containers depending on where the bottleneck lies. So we need two containers. We've already built our own Flask container in the previous section. And for Elasticsearch, let's see if we can find something on the hub. <a href="screenshots/22.png">Screenshot 22</a> Quite unsurprisingly, there exists an officially supported image for Elasticsearch. To get ES running, we can simply use docker run and have a single-node ES container running locally within no time. Let's first pull the image <a href="screenshots/23.png">Screenshot 23</a> and then run it in development mode by specifying ports and setting an environment variable that configures the Elasticsearch cluster to run as a single-node. <a href="screenshots/24.png">Screenshot 24</a> As seen above, we use --name es to give our container a name which makes it easy to use in subsequent commands. Once the container is started, we can see the logs by running docker container logs with the container name to inspect the logs. You should see logs similar to below if Elasticsearch started successfully. <a href="screenshots/25.1.png">Screenshot 25.1</a> <a href="screenshots/25.2.png">Screenshot 25.2</a>
Finally, we can go ahead, build the image and run the container <a href="screenshots/26.png">Screenshot 26</a> In the first run, this will take some time as the Docker client will download the ubuntu image, run all the commands and prepare your image. Re-running docker build after any subsequent changes you make to the application code will almost be instantaneous. Now let's try running our app. <a href="screenshots/27.png">Screenshot 27</a> Oops! Our flask app was unable to run since it was unable to connect to Elasticsearch.
<h2>Docker Network</h2>
Okay, so let's run docker container ls and see what we have. <a href="screenshots/28.png">Screenshot 28</a> Now is a good time to start our exploration of networking in Docker. When docker is installed, it creates three networks automatically. <a href="screenshots/29.png">Screenshot 29</a> The bridge network is the network in which containers are run by default. So that means that when I ran the ES container, it was running in this bridge network. To validate this, let's inspect the network. <a href="screenshots/30.png">Screenshot 30</a> Let's first go ahead and create our own network. <a href="screenshots/31.png">Screenshot 31</a> The network create command creates a new bridge network, which is what we need at the moment. In terms of Docker, a bridge network uses a software bridge which allows containers connected to the same bridge network to communicate, while providing isolation from containers which are not connected to that bridge network. The Docker bridge driver automatically installs rules in the host machine so that containers on different bridge networks cannot communicate directly with each other. Now that we have a network, we can launch our containers inside this network using the --net flag. Let's do that - but first, in order to launch a new container with the same name, we will stop and remove our ES container that is running in the bridge network. <a href="screenshots/32.png">Screenshot 32</a> Let's launch our Flask container for real now <a href="screenshots/33.png">Screenshot 33</a> 
<h2>Docker Compose</h2>
The first step is to install Docker Compose. If you're running Windows or Mac, Docker Compose is already installed as it comes in the Docker Toolbox. Since Compose is written in Python, you can also simply do pip install docker-compose. Test your installation with <a href="screenshots/34.png">Screenshot 34</a> 

























