# Part 1

### Links:
- Website content: https://github.com/WSU-kduncan/cicdf25-RabidNinja/tree/main/web-content
- Dockerfile: https://github.com/WSU-kduncan/cicdf25-RabidNinja/blob/main/Dockerfile

### Commands Used:
- Build Image from Dockerfile: `docker build -t cs3120proj4:latest .`
- Tagging for DockerHub: `docker tag cs3120proj4:latest lordrabid/cs3120proj4:latest`
- Logging in to DockerHub: `docker login -u lordrabid`a
- Push to DockerHub: `docker push lordrabid/cs3120proj4:latest`

# Part 2
## Configuring GitHub Repository Secrets

### How to Create a Personal Access Token (PAT):
- Docker -> `Account Settings` -> `Settings` -> `Personal access tokens`
- Click `Generate new token`.
- Fill out information.
- For this project, a token with with permission to read/write is appropriate to push images.
- Create the token and copy the password (it will be inaccessible later).

### Setting Repository Secrets for use by GitHub Actions:
- GitHub -> `Settings` -> `Secrets and Variables` -> `Actions`
- Click `New repository secret`

### Secrets for this Project
- DOCKER_USERNAME: username used to authenticate
- DOCKER_TOKEN: password for using a PAT

## CI with GitHub Actions
- The workflow is configured to run only when a commit is pushed to `main`.
- Checkout repository: Pulls repository contents so `Dockerfile` and `web-content/` are available.
- Login: Uses previously saved secrets to authenticate to DockerHub
- Build & Push: builds the image using `Dockerfile`, tags it with `lordrabid/cs3120proj4:latest`, and pushes to DockerHub

## Testing & Validating

### GitHub Workflow:
- Push a commit
- GitHub -> `Actions` -> `Docker Image CI`

### DockerHub:
- DockerHub repo -> Check if last push time matches workflow time

### Links
- Workflow file: https://github.com/WSU-kduncan/cicdf25-RabidNinja/blob/a374291e6f81aeb117fa4eee099c355fe9318512/.github/workflows/docker-image.yml
- DockerHub repo: https://hub.docker.com/repository/docker/lordrabid/cs3120proj4/general

# Part 3

### GitHub Tags
- See tags in git repo: `git tag`
- Generate a tag for the most recent commit (+ message): `git tag -a v*.*.* -m ""`
- Pushing a tag: `git push origin v*.*.*`

## Semantic Versioning Container Images with GitHub Actions
- Workflow trigger: runs when a tag is pushed with a semantic-versioned tag (like v1.1.1)
- Pulls repo (checkout@v4), logs into DockerHub using PAT (login-action@v3), generates tags for docker (metadata-action@v5), builds and pushes image (build-push-action@v6)

### Changes for Other Repos
- Docker image name (`images:`)
- Docker username (`DOCKER_USERNAME`)
- PAT (`DOCKER_TOKEN`)

### Links
- Workflow file: https://github.com/WSU-kduncan/cicdf25-RabidNinja/blob/a374291e6f81aeb117fa4eee099c355fe9318512/.github/workflows/docker-image.yml
- DockerHub repo: https://hub.docker.com/repository/docker/lordrabid/cs3120proj4/general

## Testing & Validating
- push a tag
- pull associated image from docker
- run the image
- check if the associated website is correct (in this case, go to http://localhost:8080/index.html)

# Resources:

### ChapGPT:
- "give me some test cases" (for testing tag versions)
- "help me format my README" (ended up formmatting the entire thing myself anyway)
- "how does this look" (pasted my readme)

### Docker Docs
- https://docs.docker.com/build/ci/github-actions/manage-tags-labels/ (General info about tags)

### GitHub 
- https://github.com/docker/metadata-action?tab=readme-ov-file#semver (used info for docker-image-yml, specifically about the metadata)