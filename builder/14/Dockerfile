FROM node:14-alpine

RUN apk add --no-cache git=~2.36.3 openssh-client=~9.0 build-base=~0.5

WORKDIR /build

ONBUILD ARG SSH_PRIVATE_KEY
ONBUILD ARG SSH_PRIVATE_KEY_PASSPHRASE

ONBUILD RUN if [ "${SSH_PRIVATE_KEY}" != "" ]; then \
               mkdir -m 0600 ~/.ssh \
               && ssh-keyscan github.com >> ~/.ssh/known_hosts \
               && echo "${SSH_PRIVATE_KEY}" > ~/.ssh/id_rsa \
               && chmod 600 ~/.ssh/id_rsa \
               && ssh-keygen -p -f ~/.ssh/id_rsa -P "${SSH_PRIVATE_KEY_PASSPHRASE}" -N ""; \
            fi

ONBUILD COPY package.json package-lock.json ./

ONBUILD RUN npm install \
            && mkdir ./node_modules.runtime \
            && cp package.json package-lock.json ./node_modules.runtime \
            && npm install --only=production --prefix ./node_modules.runtime

ONBUILD COPY .. ./

ONBUILD RUN npm run build
