# Base image for building the Go application
FROM golang:1.23.4-alpine AS builder

WORKDIR /src

# Copy Go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the application code
COPY . .

# Build the application
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o /go/bin/frontend .

# Runtime image
FROM alpine:latest
WORKDIR /src

# Copy the built binary and static files from the builder stage
COPY --from=builder /go/bin/frontend /src/server
COPY ./templates ./templates
COPY ./static ./static

# Set environment variables
ENV GOTRACEBACK=single

# Expose the port used by the application
EXPOSE 8080

# Define the entry point for the application
ENTRYPOINT ["/src/server"]
