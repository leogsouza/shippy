FROM golang:alpine as builder

RUN apk --no-cache add git

# Set our workdir to our current service in the gopath
WORKDIR /app/shippy-vessel-service

# Copy the current code into our workdir
COPY . .

RUN go mod download

# Build the binary, with a few flags which will allow
# us to run binary in Alpine.
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o shippy-vessel-service

# Here we're using a second FROM statement, which is strange,
# but this tells Docker to start a new build process with this
# image.
FROM alpine:latest

# Security related package, good to have.
RUN apk --no-cache add ca-certificates

# Same as before, create a directory for our app.
RUN mkdir /app
WORKDIR /app

# Here, instead of copying the bynary from our host machine,
# we pull the binary from the container named 'builder', within
# this build context. This reaches into our previous image, finds
# the binary we built, and pulls it into this container.
COPY --from=builder /app/shippy-vessel-service/shippy-vessel-service .

# Run the binary as per usual! This time with a binary build in a 
# separate container, with all of the correct dependencies and
# run-time libraries.
CMD [ "./shippy-vessel-service" ]