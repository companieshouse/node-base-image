FROM node:16-alpine

WORKDIR /app

ONBUILD COPY --from=0 /build/node_modules.runtime/node_modules/ ./node_modules
ONBUILD COPY --from=0 /build/dist/ ./dist

ENTRYPOINT [ "node" ]
