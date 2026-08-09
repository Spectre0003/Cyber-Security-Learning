# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# Basics on DockerFiles

The syntax of Docker File formatting is as follows:
[Instruction] [Argument]

# Basic Instructions:

## FROM

This instruction sets a build stage for the container as well as setting the base image. All Dockerfiles must start with this.

## RUN

This instruction will execute commands in the container within a new layer.

## COPY	

This instruction copies files from the local system to the working directory in the container.

## WORKDIR	

This instruction sets the working directory of the container. 

## CMD	

This instruction determines what command is run when the container starts.	

## EXPOSE

This instruction is used to tell the person who runs the container what port they should publish when running the container.