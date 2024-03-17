# sample dockerfile for next.js application
FROM node:16-alpine AS builder
MAINTAINER admin@portal
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
# Build the Next.js application
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/out /usr/share/nginx/html
# Expose port 80
EXPOSE 80
# Command to run NGINX
CMD ["nginx", "-g", "daemon off;"]
