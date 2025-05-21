# Stage 1: Build with static linking
FROM alpine:latest AS builder

# Install build tools
RUN apk add --no-cache g++ make

# Set working directory
WORKDIR /app

# Copy source code
COPY main.cpp .

# Compile statically-linked binary
RUN g++ -static -O2 -o hello main.cpp

# Stage 2: Minimal Alpine runtime
FROM alpine:latest

WORKDIR /app

# Copy statically compiled binary
COPY --from=builder /app/hello .

# Set entrypoint
ENTRYPOINT ["./hello"]