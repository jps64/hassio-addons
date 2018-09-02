# Use an official Python small runtime as a parent image
ARG BUILD_FROM=2.7-alpine
FROM python:${BUILD_FROM}

# Add curl support to the image
RUN apk update \
    && apk add curl

# Now fetch the dustlink distribution
ARG SDK_VERSION=REL-1.0.2.63
WORKDIR /opt
RUN /usr/bin/curl -sL -o dustlink.tar.gz https://github.com/dustcloud/dustlink/archive/${SDK_VERSION}.tar.gz \
    && tar xzpf dustlink.tar.gz \
    && rm -rf dustlink.tar.gz

# Install any needed packages specified in requirements.txt
WORKDIR /opt/dustlink-${SDK_VERSION}
RUN pip install --no-cache-dir -r requirements.txt

# Make port 8080 available to the world outside this container
EXPOSE 8080

# Start the webserver application
WORKDIR /opt/dustlink-${SDK_VERSION}/bin/dustLinkFullWeb
CMD ["python", "dustLinkFullWeb.py"]
