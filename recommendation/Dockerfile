
FROM golang:1.20-alpine AS builder

# Set the working directory in the container
WORKDIR /app

# Copy the go.mod and go.sum files
COPY go.mod go.sum ./

# Download dependencies
RUN go mod tidy

# Copy the entire project into the container
COPY . .

# Build the Go application (adjust the binary name accordingly)
RUN go build -o myapp .

# Production stage
FROM alpine:latest

# Install any dependencies your Go app needs in production (like certificates, etc.)
RUN apk --no-cache add ca-certificates

# Set the working directory
WORKDIR /app

# Copy the compiled binary from the builder stage
COPY --from=builder /app/myapp .
COPY --from=builder /app/templates ./templates

# Expose the port your app is listening on (usually 8080)
EXPOSE 8080

# Command to run your app, binding to 0.0.0.0 for external access
CMD ["./myapp", "-listen", "0.0.0.0:8080"]