# Portainer Docker Swarm Setup

This project contains a Docker Compose configuration for deploying Portainer and its agent in a Docker Swarm environment. Portainer is a lightweight management UI that allows you to easily manage your Docker environments.

## Prerequisites

- Docker Swarm initialized on your host machines
- Docker Compose installed
- A pre-existing external network named `swarm-net`

## Components

1. **Portainer Agent**: Deployed globally across all nodes in the swarm.
2. **Portainer**: The main Portainer application, deployed as a single instance on a manager node.

## Configuration

### Portainer Agent

- Image: `portainer/agent:latest`
- Deployed globally across all Linux nodes in the swarm
- Connects to other agent instances for cluster communication
- Mounts the Docker socket and volumes directory

### Portainer

- Image: `portainer/portainer-ce:latest`
- Deployed as a single instance on a manager node
- Exposes port 9000 for web access
- Persists data in `/mnt/appdata/portainer` on the host

## Deployment

1. Ensure your Docker Swarm is initialized and you have at least one manager node.

2. Create the external network if it doesn't exist:
   ```
   docker network create --driver overlay swarm-net
   ```

3. Deploy the stack:
   ```
   docker stack deploy -c docker-compose.yml portainer
   ```

4. Access Portainer by navigating to `http://<manager-node-ip>:9000` in your web browser.

## Notes

- The Portainer instance is configured to communicate with the agent nodes securely, but SSL verification is skipped (`--tlsskipverify`). In a production environment, you may want to set up proper SSL certificates.
- Ensure that the `/mnt/appdata/portainer` directory exists on the manager node where Portainer will be deployed, or adjust the volume mapping as needed.

## Security Considerations

- The configuration exposes the Docker socket to the Portainer agent. Ensure your swarm is properly secured.
- Consider implementing additional security measures such as using secrets for sensitive data and setting up proper network isolation in a production environment.

## Support

For more information on Portainer and its configuration options, please refer to the [official Portainer documentation](https://docs.portainer.io/).