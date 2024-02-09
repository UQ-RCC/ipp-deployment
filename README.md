1. Use ansible to bringup a openstack stack and install neccessary software

2. Use docker-compose
   * Update docker-compose.yml file with new images
   * If required update the .env with new environmental values
   * Run "source .env" to apply .env changes
   * If required, clean the unused docker images (docker stop <container_id>, docker rmi -f <image_id>, docker compose rm or docker system prune )
   * Deploy new images and start containers:
       -- docker compose -f docker-compose.yml up -d
   * Maintenance: docker-compose -f docker-compose-maintenance.yml up -d
