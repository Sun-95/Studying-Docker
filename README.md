<h1>Getting Started</h1>
<h2>Setting up your computer</h2>
Once you are done installing Docker, test your Docker installation by running the following: <a href="screenshots/1.png">Screenshot 1</a>

<h1>Hello World</h1>
<h2>Playing with Busybox</h2>
In this section, we are going to run a Busybox container on our system and get a taste of the docker run command. To get started, let's run the following in our terminal: <a href="screenshots/2.png">Screenshot 2</a> The pull command fetches the busybox image from the Docker registry and saves it to our system. You can use the docker images command to see a list of all images on your system. <a href="screenshots/3.png">Screenshot 3</a>
<h2>Docker Run</h2>
Let's now run a Docker container based on this image. To do that we are going to use the almighty docker run command. <a href="screenshots/4.png">Screenshot 4</a> Behind the scenes, a lot of stuff happened. When you call run, the Docker client finds the busybox, loads up the container and then runs a command in that container. When we run docker run busybox, we didn't provide a command, so the container booted up, ran an empty command and then exited. <a href="screenshots/5.png">Screenshot 5</a> Finally we see some output. In this case, the Docker client dutifully ran the echo command in our busybox container and then exited it. If you've noticed, all of that happened pretty quickly. The docker ps command shows you all containers that are currently running. <a href="screenshots/6.png">Screenshot 6</a> Since no containers are running, we see a blank line. Let's try a more useful variant: docker ps -a <a href="screenshots/7.png">Screenshot 7</a> So what we see above is a list of all containers that we ran. Running the run command with the -it flags attaches us to an interactive tty in the container. Now we can run as many commands in the container as we want. <a href="screenshots/8.png">Screenshot 8</a> Leaving homeless containers will eat up disk space. So, as a rule of thumb, you should clean up containers when you're done working with them. To do this, you can run the docker rm command. Just copy the container IDs from above and paste them next to the command. <a href="screenshots/9.png">Screenshot 9</a> 

<h1>Webapps with Docker</h1>
<h2>Static Sites/h2>
The image that we are going to use is a single-page website that I've already created for the purpose of this demo and hosted on the registry - prakhar1989/static-site. We can download and run the image directly in one go using docker run. As noted above, the --rm flag automatically removes the container when it exits and the -it flag specifies an interactive terminal which makes it easier to kill the container with Ctrl+C. <a href="screenshots/10.png">Screenshot 10</a>




