# Part 1

## EC2 Instance Details
- AMI ID: `ami-0341d95f75f311023` (Amazon Linux)
- Instance type: `t2.medium`
    - vCPUs: 2
    - Memory: 4 GB
- Volume Size: 30 GB
- Security Group:
    - allows SSH from anywhere (Just to keep it simple) Normally would just make it so you can ssh from home and WSU, but sometimes I use a hotspot.
    - open port 80 and 8080 for testing

## Docker Setup
- Install Docker: `sudo dnf install -y docker`
- Enable Docker: `sudo systemctl enable --now docker`
- Add ec2-user to docker group: `sudo usermod -aG docker ec2-user`
- Confirm Docker Installation: `docker --version`
- Initial Docker Test: `docker run --rm hello-world`

## Testing 
- `docker pull lordrabid/cs3120proj4:latest`
- `docker run --rm -it -p 80:80 lordrabid/cs3120proj4:latest`
- check `http://INSERT_IP_HERE`

## Scripting
- checks if a container with the same name exists
- if it does, stops that container, and deletes it
- pulls docker image with the "latest" tag and runs it
- Link: https://github.com/WSU-kduncan/ceg3120f25-RabidNinja/blob/main/Project5/deployment/bashscript
- Testing
    - push a new image and tag it as "latest"
    - pull newest image with the "latest" tag
    - check if the new image is updated

## Other notes for part 1
- after connecting to instance, `docker --version` to check if docker is installed
- PAT for proj 5: 
    - `docker login -u lordrabid`
    - `dckr_pat_n8myse3P7JKDo9IeuqArr-McpFM`

# Part 2

## How to install adnanh's webhook: 
1. Go to temp directory: `cd /tmp`
2. Download webhook archive `wget https://github.com/adnanh/webhook/releases/download/2.8.2/webhook-linux-amd64.tar.gz`
3. Extract files: `tar -xzf webhook-linux-amd64.tar.gz`
4. Move webhook executable to /usr/local/bin/: `sudo mv webhook-linux-amd64/webhook /usr/local/bin/webhook`
5. Add execute permission: `chmod 755 /usr/local/bin/webhook`
6. Check installation: `webhook --version`

## Hook Definition (hooks.json)
- Hook is exposed at `/hooks/deploy-app` on port 9000
- Executes pullrun
- Uses ec2-user as working directory
- Checks for "super-secret-password"
- Testing:
    - `webhook -hooks /home/ec2-user/deployment/hooks.json -port 9000 -verbose`
    - From outside: `curl -X POST "http://54.221.237.50:9000/hooks/deploy-app?token=super-secret-password"`
- Link: https://github.com/WSU-kduncan/ceg3120f25-RabidNinja/blob/main/Project5/deployment/hooks.json

## Webhook Service
- Located in `/etc/systemd/system/`
- Runs as ec2-user
- Starts webhook on port 9000
- Always restarts
- Enable & Start:
    - `sudo systemctl daemon-reload`
    - `sudo systemctl enable webhook`
    - `sudo systemctl start webhook`
    - `sudo systemctl status webhook`
- From outside: `curl -X POST "http://54.221.237.50:9000/hooks/deploy-app?token=super-secret-password"`
- Link: https://github.com/WSU-kduncan/ceg3120f25-RabidNinja/blob/main/Project5/deployment/webhook

# Part 3

## Configuring payload sender
- Why GitHub? 
    - Felt simple to add webhook sending at the end of the already-established GitHub Actions workflow
- How?
    - Added instructions in the GitHub Actions workflow file
- Triggers:
    - A commit with a v*.*.* tag is pushed
- Verifying successful payload: `journalctl -u webhook -f`
- Requires tokens from GitHub (EC2_PUBLIC_IP and WEBHOOK_TOKEN)
