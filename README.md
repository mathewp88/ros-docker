# Custom ROS Docker Images

Personal Docker configurations with extensive modifications based on the [OSRF ROS packages](https://github.com/osrf/docker_images). These images are customized for my development workflow and include personalized tooling and configurations.

## Available Images

Images are automatically built and published to GitHub Container Registry (GHCR) from the folders in this repository:

- `ghcr.io/mathewp88/ros-docker/jazzy:latest`
- `ghcr.io/mathewp88/ros-docker/humble:latest`
- `ghcr.io/mathewp88/ros-docker/noetic:latest`

## Quick Start

Pull and run an image:

```bash
docker pull ghcr.io/mathewp88/ros-docker/jazzy:latest
docker run -it ghcr.io/mathewp88/ros-docker/jazzy:latest
```

## Building Locally

Each folder contains a Dockerfile and supporting configuration files. To build locally:

```bash
cd jazzy
docker build -t my-ros-image .
```

## Example Configuration

Here's an example of how to use these images with a minimal Docker Compose:

```yaml
services:
  ros-dev:
    image: ghcr.io/mathewp88/ros-docker/jazzy:latest
    container_name: ros-workspace
    stdin_open: true
    tty: true
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix:rw
      - ./workspace:/root/jazzy
    environment:
      - DISPLAY=${DISPLAY}
      - QT_X11_NO_MITSHM=1
    network_mode: host
    privileged: true
    command: /bin/zsh
```

Each folder has a my docker-compose for these configurations using a Nvidia GPU

### Running with Docker Compose

```bash
# Start the container
docker-compose up -d

# Attach to the container
docker exec -it ros-workspace /bin/zsh

# Stop the container
docker-compose down
```

## Key Modifications

These images differ from the base OSRF packages with:

- Custom shell configuration (`.zshrc`)
- Additional development tools and utilities
- Personalized aliases and environment setup
- Project-specific dependencies

## Structure

```
.
├── jazzy/
│   ├── Dockerfile
│   └── [other config files]
├── humble/
│   ├── Dockerfile
│   └── [other config files]
└── .github/
    └── workflows/
        └── publish_ghcr.yml
```

## Automated Builds

Images are automatically built and pushed to GHCR on:
- Pushes to the `main` branch
- Manual workflow dispatch

## Important Notes

These configurations are made for my personal use and install a very opinionated setup for long term work. If you want a simple ros docker image that just works I reccomend the [OSRF Docker Images](https://github.com/osrf/docker_images). I reccomend that these images are used as a starting point for your own configurations. If you want to do so, clone or fork this repo, make your changes to the Dockerfiles, and enter a GitHub personal access token in your repo secrets in order to make working automated builds.

## Credits

Based on the excellent work by [OSRF](https://github.com/osrf/docker_images) with significant personal customizations.
