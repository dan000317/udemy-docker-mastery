# use this empty Dockerfile to build your assignment

# NOTE: The instructions are now in the README.md file in this directory.
FROM node:6-alpine

EXPOSE 3000

RUN apk add --no-cache tini 

RUN mkdir -p /usr/src/app 

COPY package.json /usr/src/app/

RUN npm install package.json && npm cache clean --force

COPY . /usr/src/app

CMD [ "npm","start"]