FROM node:14.17.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install 
ENV API_URL=http://localhost:3000/myapi
EXPOSE 3000