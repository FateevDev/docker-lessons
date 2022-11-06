FROM node

WORKDIR /app

COPY ./package.json /app

RUN npm install

COPY . /app

EXPOSE 80

#VOLUME ["/app/temp", "/app/node_modules"]

CMD ["npm", "start"]
