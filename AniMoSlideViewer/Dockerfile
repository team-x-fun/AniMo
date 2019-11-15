FROM node:12.13.0-alpine as builder

WORKDIR /client

RUN apk add --no-cache --virtual=build-dependencies python make g++ && \
  apk add --no-cache curl libc6-compat && \
  apk add --no-cache yarn

COPY package.json .
COPY yarn.lock .

RUN yarn --frozen-lockfile --production && \
  yarn cache clean && \
  apk del build-dependencies && \
  rm -rf /var/cache/apk/*

COPY src src/
COPY public public/
COPY ionic.config.json .
COPY tsconfig.json .

RUN yarn build

FROM alpine
RUN apk add --no-cache yarn
RUN yarn global add serve
COPY --from=builder /client/build /build
expose 5000
CMD ["serve", "-s", "build"]
