# 🐳 Docker – One Page Command Poster

## IMAGES
docker build -t app:1.0 .        # Build image
docker images                    # List images
docker pull nginx                # Pull image
docker rmi image_id              # Remove image

## CONTAINERS
docker run -d -p 8080:80 --name c1 app   # Run container
docker ps                               # Running containers
docker ps -a                            # All containers
docker stop c1                          # Stop container
docker start c1                         # Start container
docker restart c1                      # Restart container
docker rm c1                            # Remove container

## Logs and Exec
docker logs c1                          # View logs
docker exec -it c1 /bin/bash            # Enter container

## Inspect and stats
docker inspect c1                       # ENV, LABEL, IP
docker stats                            # CPU & Memory usage

## NETWORK & VOLUMES
docker network ls                       # List networks
docker volume ls                        # List volumes

## CLEANUP

docker system prune                     # Remove unused objects
docker image prune                      # Remove unused images
docker container prune                  # Remove stopped containers

## SAVE & LOAD

docker save -o app.tar app:1.0           # Export image
docker load -i app.tar                  # Import image

