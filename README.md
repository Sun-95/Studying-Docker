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
<h2>SF Food Trucks</h2>






