WebLogic on Docker
===============
Docker configurations to facilitate installation, configuration, and environment setup for developers. This project includes [dockerfiles](/images/12.1.3/wls) for WebLogic 12c 12.1.3 with JDK 7.

## Based on Official Oracle Linux Docker images
For more information please read the [Docker Images from Oracle Linux](https://registry.hub.docker.com/_/oraclelinux/) page.

## How to build and run
This project offers Dockerfiles for WebLogic 12c (12.1.3) for the 'generic' distribution with JDK 7.	
Download the the JDK 7 and WebLogic 12c packages from the [eDelivery](http://edelivery.oracle.com) Oracle site
- jdk-7u55-linux-x64.rpm -> V44959-01.zip
- fmw_12.1.3.0.0_wls.jar -> V44413-01.zip

Copy V44959-01.zip and V44413-01.zip files in the same directory as this Dockerfile (images/copy-version/12.1.3/wls)
 

### Building WebLogic Images

To Build the WebLogic image, follow the steps below:

- Go to folder /images/copy-version/12.1.3/wls


- Run the following command:
 
        sudo docker build -t fmw12c/wls:12.1.3 .

- Make sure you now have this image in place with

        sudo docker images




