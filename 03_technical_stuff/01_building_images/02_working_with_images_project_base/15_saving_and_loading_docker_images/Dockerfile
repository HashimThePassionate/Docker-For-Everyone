FROM node:14.17.0-alpine3.13
RUN addgroup app &&  adduser -S -G app app
USER app
WORKDIR /app
COPY package*.json .
RUN npm install 
COPY . .
ENV API_URL=http://localhost:3000/myapi
EXPOSE 3000
CMD npm start