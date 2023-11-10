
# DEVOPS - Assignment 2 - Part 1

## Step 1: Creating a network witht the name my_network:
* Git hub repository created
* Executed the command "docker network create my_network"

## Step 2: Create a docker file with nginx:
* Modified the docker-compose.yml file to add the service with the name "nginx_container" and the container name is also set in this container. The nginx image is built using this container and it is connected to the network created with the name "my_network"
  ### File:
version: "3.9"
services:
  nginx_container:
    container_name: nginx_container
    image: nginx
    ports:
      - "80:80"
    networks:
      - my_network 
networks:
  my_network:
    driver: bridge
    external: true



    
* Executed the command "docker compose up" to build the image and run the container so that we can see the nginx image using the assigned ports
  ### Logs:
  nginx_container  | 2023/11/10 12:21:34 [notice] 1#1: OS: Linux 5.10.102.1-microsoft-standard-WSL2      
  nginx_container  | 2023/11/10 12:21:34 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
  nginx_container  | 2023/11/10 12:21:34 [notice] 1#1: start worker processes

## Step 3: Verify if nginx is accessible on port 80:
Nginx was accessible on port 80
  ### Logs:
  2023-11-10 17:22:09 nginx_container  | 172.24.0.1 - - [10/Nov/2023:12:22:09 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)  AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36" "-"
  2023-11-10 17:22:12 nginx_container  | 172.24.0.1 - - [10/Nov/2023:12:22:12 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.85 Safari/537.36" "-"

## Step 4: Creating a new docker image - httpd and connecting with the same network and naming it "httpd_container"
* Modifying the docker compose file:
  ### File:
version: "3.9"
services:
  nginx_container:
    container_name: nginx_container
    image: nginx
    ports:
      - "80:80"
    networks:
      - my_network 
  httpd:
    container_name: httpd_container
    image: httpd
    ports:
      - "81:80"
    networks:
      - my_network
  
networks:
  my_network:
    driver: bridge
    external: true
* Executed the command "docker compose up" to build the images and deploy the changes locally. This creates two containers, each visible on the port assigned
  ### Logs:
  [+] Running 2/2
 - Container nginx_container  Created                                                                  0.3s 
 - Container httpd_container  Created                                                                  0.3s 
  Attaching to httpd_container, nginx_container

## Step 5: Verify that the httpd container is deployed on port 81:
 ### Logs:
  2023-11-10 17:25:26 httpd_container  | [Fri Nov 10 12:25:26.260721 2023] [core:notice] [pid 1:tid 139797653432192] AH00094: Command line: 'httpd -D FOREGROUND'
  2023-11-10 17:25:26 nginx_container  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration

## Step 6: Execute the command "docker network inspect":
 ### Logs:
 [
    {
        "Name": "my_network",
        "Id": "a4cd069e5c2de59e2e5aaa175ed434b8ff5074055aeb0613755a706da478b0ca",
        "Created": "2023-11-10T12:33:00.1477092Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.26.0.0/16",
                    "Gateway": "172.26.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "09b6e366b297ffefb245f4e98bf3d0b77aa55edcc06ea6303fa20d0b734d7385": {
                "Name": "httpd_container",
                "EndpointID": "aa4244513d722067ab67ca775b5ea3cbe736a39fed542568ef0e135cf1efb17d",      
                "MacAddress": "02:42:ac:1a:00:03",
                "IPv4Address": "172.26.0.3/16",
                "IPv6Address": ""
            },
            "84f628ace4050280ce90773a5c7700538df6608d305da6405b181f509be9bd37": {
                "Name": "nginx_container",
                "EndpointID": "5ca1bbc129fb26775edafb36248a4a2ae7f92f740a603c7e750a3406e10a7aa2",      
                "MacAddress": "02:42:ac:1a:00:02",
                "IPv4Address": "172.26.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

## Step 7: Remove nginx_container
Executed the command "docker rm nginx_container"

## Step 8: Creating a new nginx container with the name nginx_container_2 and connecting it with the same network
* Modified the compose file and changed the name of nginx container
  ### File:
version: "3.9"
services:
  nginx_container:
    container_name: nginx_container_2
    image: nginx
    ports:
      - "82:80"
    networks:
      - my_network 
  httpd:
    container_name: httpd_container
    image: httpd
    ports:
      - "81:80"
    networks:
      - my_network
  
networks:
  my_network:
    driver: bridge
    external: true

    
* Executed "docker compose up" to build the images
  ### Logs:
  [+] Running 2/2
 - Container nginx_container_2  Created                                                           0.1s 
 - Container httpd_container    Created                                                           0.0s 
  Attaching to httpd_container, nginx_container_2

## Step 9: Check if the changes are visible on port 82
### Logs:
2023-11-10 17:51:00 2023/11/10 12:51:00 [error] 30#30: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.26.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:82", referrer: "http://localhost:82/"

## Step 10:
Docker container ls:
### Logs:
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                NAMES
ea7e82c1120c   httpd     "httpd-foreground"       2 minutes ago   Up 2 minutes   0.0.0.0:81->80/tcp   httpd_container
c3718fcc8dd9   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:82->80/tcp   nginx_container_2

## Step 11:
* Executed the command "docker stop nginx_container_2 httpd_container"
* Exceuted the command "docker rm nginx_container_2 httpd_container"

## Step 12:
Executed the command "docker rm network my_network"