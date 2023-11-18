# Using External Storage for Docker on macOS 10.13 and Above
Docker is a powerful tool for testing projects without needing to install software locally. However, its resource-intensive nature poses a challenge for newer machines with limited internal storage. This guide explains how to set up an external disk to store Docker images and containers on macOS, enhancing your development workflow.

This solution was suggested in this [Docker Forum Post](https://forums.docker.com/t/change-docker-image-directory-for-mac/18891/8)

## Limitations 
This tutorial is specifically tailored for macOS users and has been tested on macOS version 14.1 Sonomo. It may not be applicable to other versions or operating systems.

## Setup 
Follow these steps both before and after installing Docker. Begin the setup without Docker installed on your system.  Uninstall if you do

## Pre Docker Installation 
1. [Download and install Docker CE for macOS](https://docs.docker.com/docker-for-mac/install/).

1. Setup your external drive to create symbolic links.

-  Replace <replace_with_external_ssd_name> with your external SSD’s name in the following commands.
    ```
     mkdir -p /Volumes/<replace_with_external_ssd_name>/Containers
    ```

    The p option on the mkdir command will create the entire path's directory structure.

3. Create environment variables and symbolic links
    ``` bash

    
    ## Symbolic links

    # Create a symlink to External Drive to preserve internal drive.
    ln -s /Volumes/<replace_with_external_ssd_name>/Containers/com.docker.docker /Users/<replace_with_your_user>/Library/Containers
    
    ```



## Post Docker Install

This set involves either using cp/rsync or mv to get the com.docker.docker directory and content to your external device. I used both, but rsync will transfer the port files which mv will not (mv/cp will throw the following error *"Cannot listen: ~/Library/Containers/com.docker.docker/Data/vms/0/<your file\>: failed bind: Permission denied"*). This will not stop you from copying. You can ignore this as these files are created when the docker daemon starts up. My preference would be to use mv, since it is faster.

1. Find where docker is storing images: 
    1. Check if Docker is running
    2. Once Docker is fully booted, select "Quit Docker Desktop" to stop it.

2. Transfer docker storage to external. This step can take up to 1 hour 


### Using rsync 
```
$ rsync -avhP /Users/<replace_with_your_user>/Library/Containers/com.docker.docker /Volumes/<replace_with_external_ssd_name>/Containers
```

### Delete your old docker storage
Verify docker is operational then remove old storage 
```
$ rm -r /Users/<replace_with_your_user>/Library/Containers/com.docker.docker/Data
```
