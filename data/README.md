
# Docker Compose (Virtual Cluster) Environment on a Single Machine 

## Step1 :

Before building docker container, we first need to download following two files and place them under `courses-cs239-winter2018/docker/`.
1. `spark-2.1.1-SNAPSHOT-bin-2.2.0.tgz` available at [BigSift](https://drive.google.com/file/d/1aSajeJM-LvvaDXCAk5sRjDkSQ8ZbBddO/view?usp=sharing) 
2. `Zeppelin binary` with all interpreters . Available at [Zeppelin](https://drive.google.com/file/d/1t_HXrJFrKdvC_xW4CBTOT636OzcsBFA7/view?usp=sharing). Extract the file using `tar -xzf zeppelin.tar.gz` in the docker directory.
3.  Download data files for Weather Analysis and Airport Transit Analysis from [Weather Data](https://drive.google.com/file/d/1a34DdsYlMmfKwd1FKvhm4QtlW84bXe2H/view?usp=sharing) and [Airport Data](https://drive.google.com/open?id=1t09DX36jJfswblAwufM8i1IKfXiV4aHE)
## Step 2: Docker Installation
Now install Docker in your local machine (Follow instructions [here](https://docs.docker.com/engine/installation/)). After the installation is complete, launch the 'Docker' application that will start the Docker service (e.g., Whale-like icon on your Mac status bar). If this step is successful, you should be able to type 'docker' on your command line console.  
## Step 3: Create a Docker Image
Assuming  that you have installed Docker and currently in the `courses-cs239-witner2018/docker` directory, you should be able to see "DockerFile" under this directory. The following command creates a docker image using the DockerFile under the current directory ('.') and assigns "spark" as the name of the image. 

```bash
docker build -t spark .
```
This step will take several minutes to build a docker container from the recipe. You should see the messages similar to the following on your screen. It will then pull the required packages, run each command, etc. This process will take **long** time, as it downloads Spark, Scala, and other tools required to do your subsequent assignments. You need to ensure that your machine has enough hard disk space (several GBs, mine is about ~2.17GB) and memory to finish this step. 
 
```bash 
bash-3.2$ docker build -t spark .
Sending build context to Docker daemon  1.954GB
Step 1/34 : FROM debian:jessie
 ---> 25fc9eb3417f
Step 2/34 : MAINTAINER Getty Images "https://github.com/gettyimages"
 ---> Using cache
 ---> 3106ccca439d
...
```
To list all the images along with their status. Run 
```bash
docker ps -a
```

## Step 4: Starting the cluster using Docker-Compose

Once the docker image is built, you can start the cluster using docker-compose.

`docker-compose up`

Use this command only when launching the cluster for the first time. Afterwords, use `docker-compose start` to start the cluster.

This command will initiate the cluster using the recipe `docker-compose.yml`. 

```bash
Starting dockerspark_master_1 ... 
Starting dockerspark_master_1 ... done
Starting dockerspark_worker_1 ... 
Starting dockerspark_worker_1
Starting dockerspark_zeppelin_1 ... 
Starting dockerspark_zeppelin_1 ... done
Attaching to dockerspark_master_1, dockerspark_worker_1, dockerspark_zeppelin_1
zeppelin_1  | Zeppelin start                               [  OK  ]
```

Give this step a few seconds to set up everything and start all the nodes.

## Step 4: Access Zeppelin 
Now the cluster has been setup. Go to port 6060 of your local machine [localhost:6060](http://localhost:6060) to access Zeppeling notebook. 
![alt text](https://github.com/miryung/courses-cs239-winter2018/blob/master/docker/zeppelin.png "Zeppelin")

## Step 5: Access a container in the cluster
Use the following command to attach to any container in the cluster.
`dcoker exec -it <container-name > /bin/bash ` where the name of containers are printed on the screen in Step 4 such as dockerspark_master_1

## Step 6: Shutting Down the Cluster
Use the following command to shutdown the cluster. Make sure you have transferred all the important data from the containers to the host machine. Otherwise the data lying on the containers will be lost
` docker-compose stop`

## Note:
In case a spark job can not be submitted through the notebook (Spark Context not present exception), restart the cluster using `docker-compose down` and then `docker-compose up`. 
The `down` command will bring down the entire application and remove the containers, images, volumes, and networks entirely,
