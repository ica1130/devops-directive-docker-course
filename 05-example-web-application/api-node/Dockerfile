FROM node:19.6-alpine

WORKDIR /usr/src/app

ENV NODE_ENV production

COPY package*.json ./

# npm ci stands for npm clean install, it is used to install dependencies from package-lock.json
# and --only=production flag is used to install only production dependencies and to exclude devDependencies
RUN npm ci --only=production

USER node

# All the files copied are owned by the node user
COPY --chown=node:node ./src .

EXPOSE 3000

CMD ["node", "index.js"]
