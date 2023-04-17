# How to use Docker with simple Node.js project
1. Use PowerShell. VSCode terminal is OK  
2. All words in capital are to be changed with your ones
3. I will use 8000 port, you can use your one 
4. The author is not responsible for errors
5. Read <code><a href="https://docs.docker.com/get-started/overview/">Docker Overview</a></code> for full and the most correct experience!

## Prepare your simple project 
FOLDER_NAME/  
- package.json    
- server.js 
- ... 

## Build an image
- Make Dokerfile file in FOLDER_NAME with:
``` bash
FROM node:18-alpine
WORKDIR .
COPY . . 
RUN npm i 
CMD ["node", "server.js"]
EXPOSE 8000
```
- Build an image <code>docker build -t IMAGE_NAME .</code>  
-t to tag the image as human-readable IMAGE_NAME  
. should look in current dir  

## Run container
- Run container from the image <code>docker run -dp 8000:8000 INAGE_NAME</code>  
-d to run the new container in “detached” mode (in the background)   
-p to create a mapping between the host’s port 8000 to the container’s port 8000  

## Operate your container & image
- Whatch all containers you run             <code>docker ps</code>
- Stop a container                          <code>docker stop CONTAINER_ID</code>
- Remove container                          <code>docker rm CONTAINER_ID</code>
- Stop&Remove container                     <code>docker rm -f CONTAINER_ID</code>
- Update image after changes in folder      <code>docker built -t IMAGE_NAME .</code>
- Access to your container(cat for example) <code>docker exec CONTAINER_ID cat ./SOMEFILE</code>
- Whatch logs                               <code>docker logs CONTAINER_ID</code>
- Rename image                              <code>docker tag IMAGE_NAME NEW_IMAGE_NAME</code>

## Work with DockerHub  
- Login to DockerHub                    <code>docker login -u YOUR_USER_NAME</code>
- Tag your image to have rights to push <code>docker tag IMAGE_NAME YOUR_USER_NAME/IMAGE_NAME</code>
- Push image to DockerHub               <code>docker push YOUR_USER_NAME/IMAGE_NAME</code>

## Work with Volume
#### When you use a volume, a new directory is created within Docker’s storage directory on the host machine, and Docker manages that directory’s contents
- Create Volume                           <code>docker volume create VOLUME_NAME</code>
- Run your app with somefiles in a volume  
<code>docker run -dp 8000:8000 --mount type=volume,src=VOLUME_NAME,target=/DIR_OR_FILE_TO_BE_IN_VOLUME IMAGE_NAME</code>
- Inspect volume                          <code>docker volume inspect VOLUME_NAME</code>

## Work with Bind mounts
#### Bind mounts will mount a file or directory on to your container from your host machine, which you can then reference via its absolute path
- Create bind mount <code>docker run -it --mount type=bind, src="$(pwd)", target=/DIR_NAME_TO_CREATE_MOUNT IMAGE_NAME bash</code>
- Its an interactive session. You can work here if you want. Ctrl+D to stop it
- Run your app with bind mount 
``` bash
docker run -dp 8000:8000 `
-w /FOLDER --mount type=bind,src="$(pwd)",target=/FOLDER `
node:18-alpine `
sh -c "yarn install && yarn run dev"
```
-w /FOLDER_NAME - sets the “working directory” or the current directory that the command will run from
--mount type=bind,src="$(pwd)",target=/FOLDER bind mount the current directory from the host into the /FOLDER directory in the container
node:18-alpine - the image to use
sh -c "yarn install && yarn run dev" - to install packages and then start the development server.
