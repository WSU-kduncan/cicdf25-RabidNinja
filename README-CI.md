



docker build -t my-httpd:latest .

docker tag my-httpd:latest lordrabid/my-httpd:latest

docker login -u lordrabid

docker push lordrabid/my-httpd:latest
