{% include docker-series.md %}

## Terminology

The basic concepts of Docker are Images and Containers: 

- Image: Ordered collection of filesystem changes and execution parameters
- Container: Runtime instance of a docker image

There are other concepts to know about :

- Volume: System to persist data, independent of the container’s life cycle
- Dockerfile: Text document that contains all the commands to create an image
- Layer: modification to an image, represented by an instruction in the Dockerfile 
- Tag: Label applied to a Docker image in a repository.
- Repository: Set of Docker images
- Registry: Hosted service containing repositories of images
- Union File System: conjunction with copy-on-write techniques to provide the building blocks for containers
