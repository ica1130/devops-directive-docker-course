# This is a multi-stage build Dockerfile
# The first stage is the build stage where we build the application
# The second stage is the deployable stage where we deploy the application
# The build stage uses the golang:1.19-buster image
# The deployable stage uses the scratch image
# This makes the final image very small
FROM golang:1.19-buster as build

WORKDIR /app

RUN useradd -u 1001 nonroot

COPY go.mod go.sum ./

RUN go mod download

COPY . .

# this is the command to build the binary to run the application
# it helps to reduce the size of the image by not including the source code
# and only including the binary
RUN go build -ldflags="-linkmode external -extldflags -static" -tags netgo -o api-golang 


###
FROM scratch

COPY --from=build /etc/passwd /etc/passwd

COPY --from=build /app/api-golang api-golang

ENV GIN_MODE release

USER nonroot

EXPOSE 8080

# by doing this it means that everytime we start our application
# we dont need to build it because we have already built it
CMD ["./api-golang"]
