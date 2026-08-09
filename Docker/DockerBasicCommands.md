# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# Basic Commands

## docker pull
To download images from the internet
Syntax: docker pull [name]

## docker image
To explore different image commands available.
Syntax: docker image [command]

    # image commands

        - build
        To build an image from DockerFile

        -history     
        Show the history of an image

        -import      
        Import the contents from a tarball to create a filesystem image

        -inspect     
        Display detailed information on one or more images

        -load        
        Load an image from a tar archive.

        -ls          
        List images

        -prune       
        Remove unused images

        -pull        
        Pull an image or a repository from a registry

        -push        
        Push an image or a repository to a registry

        -rm          
        Remove one or more images

        -save        
        Save one or more images to a tar archive

        -tag         
        Create a tag that refers to the source image

## docker run
Create running containers from images.
Syntax: docker run [options] image [command]

## docker ps
List containers that are running currently
Syntax: docker ps