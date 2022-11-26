FROM node

WORKDIR /app

COPY ./package.json .

RUN npm install

COPY . .

ENV MONGO_USERNAME=""
ENV MONGO_PASSWORD=""

EXPOSE 80

CMD ["npm", "start"]
