FROM node:18.19.1-alpine3.19

WORKDIR /app

COPY . .

RUN npm install yarn && yarn install

EXPOSE 5001

CMD ["yarn", "dev"]

