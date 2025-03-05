FROM node:lts-alpine AS development
WORKDIR /usr/src/app
COPY . .
CMD ["sh", "-c", "npm install && npm run dev"]

FROM node:lts-alpine AS build
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine AS production
COPY --from=build /usr/src/app/dist /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]
