FROM node

WORKDIR /app

COPY ./package.json /app

RUN npm install

COPY . /app

ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

#VOLUME ["/app/temp", "/app/node_modules"]

CMD ["npm", "start"]
