# Part 1

## Links
- Website content: https://github.com/WSU-kduncan/cicdf25-RabidNinja/tree/main/web-content
- Dockerfile: https://github.com/WSU-kduncan/cicdf25-RabidNinja/blob/main/Dockerfile

### Commands Used:
- Build Image from Dockerfile: `docker build -t cs3120proj4:latest .`
- Tagging for DockerHub: `docker tag cs3120proj4:latest lordrabid/cs3120proj4:latest`
- Logging in to DockerHub: `docker login -u lordrabid`
- Push to DockerHub: `docker push lordrabid/cs3120proj4:latest`

# Part 2

### How to Create a Personal Access Token (PAT)
- Docker -> `Account Settings` -> `Settings` -> `Personal access tokens`
- Click `Generate new token`.
- Fill out information.
- For this project, a token with with permission to read/write is appropriate to push images.
- Create the token and copy the password (it will be inaccessible later).

## Setting Repository Secrets for use by GitHub Actions
- GitHub -> `Settings` -> `Secrets and Variables` -> `Actions`
- Click `New repository secret`

### Secrets for this Project
- DOCKER_USERNAME: username used to authenticate
- DOCKER_TOKEN: password for using a PAT

#

### ??
- Link to workflow file: https://github.com/WSU-kduncan/cicdf25-RabidNinja/blob/a374291e6f81aeb117fa4eee099c355fe9318512/.github/workflows/docker-image.yml


# Part 3


