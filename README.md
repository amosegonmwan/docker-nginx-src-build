# Nginx Source Compilation with Docker

This repository provides Docker setups to build and run an Nginx web server from source using two different Dockerfiles. Each Dockerfile demonstrates a different approach to handle the Nginx source files, including manual download and dynamic download via build arguments.

## Pre-requisites

- **Nginx Source File**: Ensure that the `nginx-1.25.4.tar.gz` file has been downloaded and placed in the current working directory before building the Docker image using `Dockerfile1`.

## Files in this Repository

### `Dockerfile1`

This Dockerfile outlines the steps to build an Nginx server from source using a pre-downloaded Nginx tarball:

- **Base Image**: Uses the latest Ubuntu image as the base.
- **Port Exposure**: Exposes port 80 to allow HTTP traffic.
- **Package Installation**: Installs essential build tools and libraries required for compiling Nginx, such as `build-essential`, `libpcre3`, `libpcre3-dev`, `zlib1g`, `zlib1g-dev`, `libssl3`, and `libssl-dev`.
- **File Copy**: Copies the pre-downloaded `nginx-1.25.4.tar.gz` from the local directory into the Docker container.
- **Nginx Compilation**: Extracts the Nginx source files, configures the build with options for SSL and PCRE support, and installs Nginx.
- **Cleanup**: Removes the source directory to reduce the image size.
- **Startup Command**: Configures the container to run Nginx in the foreground, allowing the container to stay alive.

### `Dockerfile2`

This Dockerfile demonstrates a more dynamic approach by downloading the Nginx source file during the build process:

- **Base Image**: Uses the latest Ubuntu image as the base.
- **Port Exposure**: Exposes port 80 for HTTP traffic.
- **Package Installation**: Installs the same essential build tools and libraries required for compiling Nginx.
- **Build Arguments**: Introduces `ARG` commands to allow dynamic specification of the Nginx version and file extension, which are used to download the source files directly from the official Nginx website during the build.
- **File Download and Extraction**: Uses the `ADD` command to download and extract the specified Nginx tarball.
- **Nginx Compilation**: Configures and compiles Nginx with the same options as `Dockerfile1`.
- **Cleanup**: Removes the source directory to reduce the image size.
- **Startup Command**: Configures the container to run Nginx in the foreground, keeping the container alive.

## How to Use

### Building and Running with `Dockerfile1`

Ensure that the `nginx-1.25.4.tar.gz` file is present in the working directory, then run the following commands:

1. **Build the Docker Image**:
    ```bash
    docker image build -t nginx-src:1.0 -f Dockerfile1 .
    ```

2. **Run the Container**:
    ```bash
    docker container run -d --name my-src -p 8080:80 nginx-src:1.0
    ```

3. **Access the Nginx Server**: 
   Open a browser and navigate to `http://localhost:8080` to see the Nginx welcome page.

### Building and Running with `Dockerfile2`

This process is similar, but `Dockerfile2` downloads the Nginx source file during the build:

1. **Build the Docker Image**:
    ```bash
    docker image build -t nginx-src-dynamic:1.0 -f Dockerfile2 .
    ```

2. **Run the Container**:
    ```bash
    docker container run -d --name my-dynamic-src -p 8080:80 nginx-src-dynamic:1.0
    ```

3. **Access the Nginx Server**: 
   Open a browser and navigate to `http://localhost:8080` to see the Nginx welcome page.
