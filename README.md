# Run Docker External Storage in macOS 10.13+
Docker can be useful to test projects without having to install software locally to test components. However, it is resource hungry. Newer machines with smaller internal storage provide a challenge to developers. This tutorial will explain how to setup an external disk to store docker images and containers.

This solution was suggested in this [Docker Forum Post](https://forums.docker.com/t/change-docker-image-directory-for-mac/18891/8)

## Limitations 
The limitations of this tutorial are that it is only applicable to macOS and currently works for 14.1 Sonomo.

## Setup 
This section will detail the process pre and post the installation of Docker. Start the setup *without* docker installed

## Pre Docker Installation 
1. [Download and install Docker CE for macOS](https://docs.docker.com/docker-for-mac/install/).

1. Setup your external drive to create symbolic links.
    ```
     mkdir -p /Volumes/<replace_with_path_to_external_ssd>/Containers
    ```

    The p option on the mkdir command will create the entire path's directory structure.

2. Create environment variables and symbolic links
    ``` bash

    
    ## Symbolic links

    # Create a symlink to External Drive to preserve internal drive.
    ln -s /Volumes/<replace_with_external_ssd_name>/Containers/com.docker.docker /Users/<replace_with_your_user>/Library/Containers
    
    ```



## Post Docker Install

This set involves either using cp/rsync or mv to get the com.docker.docker directory and content to your external device. I used both, but rsync will transfer the port files which mv will not (mv/cp will throw the following error *"Cannot listen: ~/Library/Containers/com.docker.docker/Data/vms/0/<your file\>: failed bind: Permission denied"*). This will not stop you from copying. You can ignore this as these files are created when the docker daemon starts up. My preference would be to use mv, since it is faster.

1. Find where docker is storing images: 

    Run the following command to see if Docker is running:

    ```
    launchctl list | grep docker
    -       0       com.docker.helper
    1949    0       com.docker.docker.2716
    ```

    1. If docker is not started yet, start it using the application.  
    2. Stop Docker once it fully booted up *"Quit Docker Desktop"*

2. Transfer the files. This step can take up to 1 hour 


### Using rsync 
```
$ rsync -avhP /Users/<replace_with_your_user>/Library/Containers/com.docker.docker /Volumes/<replace_with_external_ssd_name>/Containers
```
