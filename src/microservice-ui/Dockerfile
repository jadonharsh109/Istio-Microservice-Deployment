# Stage 1: Build the Angular project
FROM node:12.13.1 AS builder
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 4200
CMD ["npm", "start"]
