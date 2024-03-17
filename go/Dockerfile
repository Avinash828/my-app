FROM golang:1.17-alpine AS builder
MAINTAINER admin
WORKDIR /myapp
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -o app
# Use a lightweight base image

FROM alpine:latest
WORKDIR /myapp
# Copy the built executable from the builder stage
COPY --from=builder /app/app .
# Expose the port the application runs on
EXPOSE 8080

CMD ["./app"]
