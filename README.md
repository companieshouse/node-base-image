# node-base-image

Base Docker images for Node.js applications.

- [Supported images](#supported-images)
- [Image properties](#image-properties)
- [Sample use](#sample-use)
- [Licence](#license)

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
  
3. build Docker image using `docker build -t 169942020521.dkr.ecr.eu-west-1.amazonaws.com/local/[application name] .` command or `docker build --build-arg SSH_PRIVATE_KEY="$(cat ~/.ssh/id_rsa)"
   --build-arg SSH_PRIVATE_KEY_PASSPHRASE -t 169942020521.dkr.ecr.eu-west-1.amazonaws.com/local/[application name] .` command if project requires dependencies stored in private GitHub repository

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
