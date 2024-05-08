# Dockerfile for WSO2 Micro Integrator
[Source of this doc](https://github.com/wso2/docker-ei/blob/master/dockerfiles/centos/micro-integrator/README.md)

[GitHub Source files](https://github.com/wso2/docker-ei/releases/tag/v4.2.0.6)

This section defines the step-by-step instructions to build an [CentOS](https://hub.docker.com/_/centos/) Linux based Docker image for WSO2 Micro Integrator 4.2.0.

## Prerequisites

* [Docker](https://www.docker.com/get-docker) v17.09.0 or above
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) client

## How to build an image and run

#### 1. Checkout this repository into your local machine using the following Git client command.

```
git clone https://github.com/antonioPetrocelli/petro-docker-ei.git
```

>The local copy of the `dockerfiles/centos/micro-integrator` directory will be referred to as `MI_DOCKERFILE_HOME` from this point onwards.

#### 2. Build the Docker image.

- Download wso2mi-4.2.0.zip from [here](https://wso2.com/micro-integrator)
- Host the product pack using a webserver.
- Navigate to `<MI_DOCKERFILE_HOME>` directory. <br>
- Change <MI_DIST_URL> in Dockerfile to the URL of the product pack.  (http://15.161.184.189/wso2/wso2mi-4.2.0.zip)
- Execute `docker build` command as shown below.

```
docker build -t wso2mi:4.2.0-centos .
```

> By default, the Docker image will prepackage the General Availability (GA) release version of the relevant WSO2 product.

#### 3. Push the Docker image to your DockerHub Repository.

* Get images list
```
docker image ls
```

|REPOSITORY                    |TAG            |IMAGE ID       |CREATED         |SIZE   |
|------------------------------|---------------|---------------|----------------|-------|
|wso2mi                        |4.2.0-centos   |e1879d5a0b72   |9 minutes ago   |949MB  |
|gcr.io/k8s-minikube/kicbase   |v0.0.43        |619d67e74933   |2 weeks ago     |1.26GB |

* Push command sample
```
docker tag local-image:tagname petroanvasys/imagename:tagname
docker push petroanvasys/imagename:tagname
Make sure to replace tagname with your desired image repository tag.
```

* Push the image
```
docker login -u "petroanvasys" -p "TUJ7AhwqT4xZ9.#" docker.io
docker tag wso2mi:4.2.0-centos petroanvasys/wso2mi:4.2.0-centos
docker push petroanvasys/wso2mi:4.2.0-centos
```

#### 4. Running the Docker image.

- `docker run -it -p 8253:8253 -p 8290:8290 wso2mi:4.2.0-centos`

## How to update configurations

Configurations would lie on the Docker host machine and they can be volume mounted to the container. <br>
As an example, steps required to change the port offset using `deployment.toml` is as follows:

#### 1. Stop the MI container if it's already running.

In WSO2 MI 4.2.0 product distribution, `deployment.toml` configuration file can be found at `<DISTRIBUTION_HOME>/conf`.<br>
Copy the file to some suitable location of the host machine, referred to as `<SOURCE_CONFIGS>/deployment.toml` and change the<br>
offset value (`[server]->offset`) to 11.

#### 2. Grant read permission to `other` users for `<SOURCE_CONFIGS>/deployment.toml`.

```
chmod o+r <SOURCE_CONFIGS>/deployment.toml
```

#### 3. Run the image by mounting the file to container as follows:

```
docker run -it \
-p 8254:8254 \
--volume <SOURCE_CONFIGS>/deployment.toml:<TARGET_CONFIGS>/deployment.toml \
wso2mi:4.2.0-centos
```

> In here, <TARGET_CONFIGS> refers to /home/wso2carbon/wso2mi-4.2.0/conf folder of the container.

## WSO2 Private Docker images

If you have a valid WSO2 subscription you can have access to WSO2 private Docker images. These images will get updated frequently with bug fixes, security fixes and new improvements. To view available images visit [WSO2 Docker Repositories](https://docker.wso2.com/)

## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Docker run command reference](https://docs.docker.com/engine/reference/run/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)