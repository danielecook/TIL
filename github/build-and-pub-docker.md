# Build and publish a docker container using GitHub Actions

GitHub Actions can be configured to build multiple docker images located within a repo, only when they are updated.

```yaml
# Build dockerfile on change
name: Docker Build mutect

on:
  push:
    paths:
    - 'env/mutect.Dockerfile'
    - '.github/workflows/build_mutect.yml'
  pull_request:
    paths:
    - 'env/mutect.Dockerfile'
    - '.github/workflows/build_mutect.yml'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Build and Publish
      uses: elgohr/Publish-Docker-Github-Action@master
      with:
        name: swantonlab/mutect
        tag: "${{ steps.current-time.formattedTime }}"
        username: ${{ secrets.DOCKER_HUB_USER }}
        password: ${{ secrets.DOCKER_HUB_PASSWORD }}
        snapshot: true
        dockerfile: mutect.Dockerfile
        workdir: "env"
        tags: "latest"
```

```Dockerfile
# docker build --file mutect.Dockerfile -t swantonlab/mutect .
FROM ubuntu:14.04
RUN apt-get update && \
	apt-get install -y parallel samtools openjdk-7-jre wget unzip && \
	apt-get clean
ENV IGVTOOLS_VERSION=2.3.98
RUN  wget --quiet -O igvtools_${IGVTOOLS_VERSION}.zip \
    http://data.broadinstitute.org/igv/projects/downloads/2.3/igvtools_${IGVTOOLS_VERSION}.zip \
  	&& unzip igvtools_${IGVTOOLS_VERSION}.zip \
  	&& rm igvtools_${IGVTOOLS_VERSION}.zip \
  	&& mv IGVTools/* /usr/local/bin
LABEL Name="mutect" Author="Daniel Cook"
```
