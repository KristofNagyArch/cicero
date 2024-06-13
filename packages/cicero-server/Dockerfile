FROM node:14
ENV CICERO_DIR=/usr/src/app/example
ENV CICERO_PORT=6001

WORKDIR /usr/src/app

COPY package*.json ./
COPY ./example /etc

RUN npm install

COPY . .

EXPOSE 6001

CMD ["node", "app.js"]
