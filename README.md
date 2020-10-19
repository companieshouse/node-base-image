# node-base-image

Base Docker images for Node.js applications.

- [Supported images](#supported-images)
- [Image properties](#image-properties)
- [Sample use](#sample-use)
- [Licence](#license)

## Known Issues

Docker releases `>=2.4.0.0` have [BuildKit](https://github.com/moby/buildkit) enabled by default - this breaks image builds due to a known issue (https://github.com/moby/buildkit/issues/816) with `BuildKit` and `ONBUILD COPY --from` directives which we use in our `runtime` image.

To fix this you can disable BuildKit in one of two ways:

1. Prefix `docker build` commands with `DOCKER_BUILDKIT=0` 
2. Disable BuildKit system wide by configuring the Docker daemon for your system - see: https://docs.docker.com/config/daemon/#configure-the-docker-daemon

## Supported images

| Tag                                                                        | OS     | Node.js version |
| ---------------------------------------------------------------------------| ------ | --------------- |
| `169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/node:14-alpine-builder` | Alpine | 14.x            |
| `169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/node:14-alpine-runtime` | Alpine | 14.x            |

## Image properties

Here are the build arguments used in the base builder images:

| Name                         | Default value | Required | Comment                                                                                                                       |
| ---------------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `SSH_PRIVATE_KEY`            | none          | no       | used to pull dependencies from GitHub during build phase, needed only if dependencies are stored in private GitHub repository |
| `SSH_PRIVATE_KEY_PASSPHRASE` | none          | no       | used to remove passphrase from key, needed only if key is protected                                                           | 

Here are the properties used in the base images:

| Directive    | Value  |
| ------------ | ------ |
| `WORKDIR`    | `/app` |
| `ENTRYPOINT` | `node` |

## Sample use

Follow below steps to package Node.js application as a Docker image: 

1. create `.dockeringore` file to filter out files that should not be copied to Docker image

2. create `Dockerfile` file with instructions below and adjust port number to one application uses:

   ```dockerfile
    FROM 169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/node:14-alpine-builder
    FROM 169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/node:14-alpine-runtime

    CMD ["/app/dist/server.js"]

    EXPOSE 9999
    ```
  
3. build Docker image with the command `DOCKER_BUILDKIT=0 docker build -t 169942020521.dkr.ecr.eu-west-1.amazonaws.com/local/[application name] .` or if project requires dependencies stored in private GitHub repository
   use command `DOCKER_BUILDKIT=0 docker build --build-arg SSH_PRIVATE_KEY="$(cat ~/.ssh/id_rsa)" --build-arg SSH_PRIVATE_KEY_PASSPHRASE -t 169942020521.dkr.ecr.eu-west-1.amazonaws.com/local/[application name] .`

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
