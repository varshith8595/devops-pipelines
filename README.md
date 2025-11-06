This is to start my devops learning journey and in this commit i wanted to test whether my pipeline is running on every push to the repository that i am currently working or not

// pipelines : This is written by a person to make sure certain steps are followed whenever a specific action is being performed to our repository
// eg: in this i learnt how to write a pipeline that will be running when ever i push chages to main branch that i am currently working in 
// This is possible by using yaml file where syntax is whitespace sensitive and indentation is very important as i got syntax error for the first time in my CI pipeline
// CI CD pipeline are continuous integration and continuous deployment pipelines
// we can specify the tests that are to be performed in our pipeline under -steps section (there are multiple sections name: on: jobs: build: runson: - step: name: uses: with: run: )

//Docker is a lunchbox which will not only have rice (code) but also curry (dependencies) and whatever the environment we are in it will be tasting (working) in the same way as it is supposed to work. --->(this is the practical way in which we can understand docker in a simple way)
//Main thing is it resolves the issue of application working on different types of machines it makes application compatible on every type of machine
//makes our development much more easier and makes it more efficient
//Docker containers are light weight and can be moved easily across multiple devices
//it can also be easily scalable like we can have multiple menu cards whenever there are more number of customers in our restaraunt then we can use it to handle the crowd and have smooth servicing 
//Docker has 2 main concepts --> images and containers
//image -> consider image as a recipie where we have ingredients (code) along with the process (dependencies) by using which we can make a dish (software)
//container -> imagine it as raw-materials and utensils that are required to cook the dish image is important (we need to understand this)
//*** by using one image and multiple containers we can serve multiple instances of our software 
//volume -> shared folder for storage to which multiple containers can be connected and volumes can also be connected to other servers
//Docker Network -> think that there are multiple containers and a centralized docker network to which all the containers are connected to by which different softwares on different containers can communicate among those without any interruption to the services that are being provided by them
//Docker workflow  ---1, docker client(run commands and instructs like a master chef), 2, docker host(manages all the images and containers) 3, docker registry(storage to store all images whenever host has no images then it will providing images and git -> github => docker -> dockerhub)
//installed docker in my local system in which i can look at images,containers,docker hub,volumes,docker scout
//to create an image in docker we need to use dockerfile which is similar to syntax of docker
//docker file -> setup instructions to create docker image
//from : picking base image to choose to build a new image
//copy : copies filer or direcrtories from build context to image
//run  : executes commands 
//expose: informs on which port it runs
//env : environment varibale setup
//arg : runtime variables
//vol: similar to external storage 
//cmd: command to run on initialization (this can be overriden) default executable 
//entryPoint : it is fixed and can not be changed -> fixed starting point
//docker pull ubuntu -> eg for using a public image from docker hub in our local
//docker run -it ubuntu -> creating a container for using our image and getting the system into our terminal
//*** creation of image -> create Dockerfile in our repo without any extension and then open terminal in a folder where docker file is present in then run this command  docker build -t <path> . (by using this we can create an image) to verify we can use docker images
//to use image in a container just use command docker run <name of the created image>

<!-- set the base image of node to create react app image
FROM node:20-alpine

create a user with permissions to run the app
-S create a system user
-G adds a system user to the group
This is done to avoid running the app as root
If app is run as the root, then any vulnerability in the app can be exploited to gain access to the host system
RUN addgroup app && adduser -S -G app app

set the user to app
USER app

set the working directory to /app
WORKDIR /app

copy package.json and package.lock.json to the working directory
This is done before copying rest of the files to take the advantage of docker's cache
If the package.json and package.lock.json file does not change then it will be using cached dependencies
COPY package*.json ./

sometimes the ownership of the files in the working directory is changed to root
and thus app cant access the files and throws errors -> EACCESS : permission denied
to avoid this change the permission of the files to root user
USER root

change the ownership of the ./app to the app user
RUN chown -R <user>:<group> <directory>
RUN chown -R app:app .

change the app to app user
USER app

install dependencies
RUN npm install

copy the rest of the files to the working directory
COPY . .

expose port 5173 to tell docker that the container is listening to specified network ports at runtime
EXPOSE 5173

command to run
CMD npm run dev --> these are the content to be present in the Dockerfile when we want to create a image in docker by using docker build -t <path> .

// another important thing is we need to add .dockerignore file where node_modules is to be present which indicated that it will be ignored when an image is being created

// we need to add --host in dev of package.json 
// expose we are telling the docker container that it will listening on port 5173 and for localhost mapping we need to use docker run -p 5173:5173 <folder-name>