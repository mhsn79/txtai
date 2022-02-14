# Cloud

![cloud](images/cloud.png#only-light)
![cloud](images/cloud-dark.png#only-dark)

Scalable cloud-native applications can be built with txtai. The following runtimes are supported.

- Container Orchestration (i.e. Kubernetes)
- Docker Engine
- Serverless

Images for txtai are available on Docker Hub for [CPU](https://hub.docker.com/r/neuml/txtai-cpu) and [GPU](https://hub.docker.com/r/neuml/txtai-gpu) installs. The CPU install is recommended when GPUs aren't available given the image is half the size.

The base txtai images have no models installed and models will be downloaded each time the container starts. Caching the models is recommended as that will significantly reduce container start times. This can be done a couple different ways.

- Create a container with the [models cached](#cache-models-in-container-images)
- Set the transformers cache environment variable and mount that volume when starting the image
    ```bash
    docker run -v <local dir>:/models -e TRANSFORMERS_CACHE=/models --rm --it <docker image>
    ```

## Build txtai images

The txtai images found on Docker Hub are configured to support most situations. The Dockerfile is [available on GitHub](https://github.com/neuml/txtai/docker/base/Dockerfile). This image can be locally built with different options as desired.

Examples build commands below

```bash
# Get Dockerfile
wget https://raw.githubusercontent.com/neuml/txtai/master/docker/base/Dockerfile

# Build Ubuntu 18.04 image running Python 3.7
docker build -t txtai --build-arg BASE_IMAGE=ubuntu:18.04 --build-arg PYTHON_VERSION=3.7 .

# Build image with GPU support
docker build -t txtai --build-arg GPU=1 .

# Build minimal image with the base txtai components
docker build -t txtai --build-arg COMPONENTS= .
```

## Cache models in container images

As mentioned previously, model caching is recommended to reduce container start times. The following commands demonstrate this. In all cases, it is assumed a config.yml file is present in the local directory with the desired configuration set.

### API
```bash
# Get Dockerfile
wget https://raw.githubusercontent.com/neuml/txtai/master/docker/api/Dockerfile

# CPU build
docker build -t txtai-api .

# GPU build
docker build -t txtai-api --build-arg BASE_IMAGE=neuml/txtai-gpu .

# Run
docker run -p 8000:8000 --rm -it txtai-api
```

### Service
```bash
# Get Dockerfile
wget https://raw.githubusercontent.com/neuml/txtai/master/docker/service/Dockerfile

# CPU build
docker build -t txtai-service .

# GPU build
docker build -t txtai-service --build-arg BASE_IMAGE=neuml/txtai-gpu .
```

### Workflow
```bash
# Get Dockerfile
wget https://raw.githubusercontent.com/neuml/txtai/master/docker/workflow/Dockerfile

# CPU build
docker build -t txtai-workflow . 

# GPU build
docker build -t txtai-workflow --build-arg BASE_IMAGE=neuml/txtai-gpu .
```