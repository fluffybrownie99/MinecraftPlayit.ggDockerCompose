FROM alpine:latest
RUN mkdir -p /minecraft-server/jar

# Install Java 21 instead of Java 17
RUN apk add openjdk21-jre-headless

# Copy versions.csv file
COPY versions.csv /minecraft-server/jar/versions.csv

# Set Minecraft version from argument
ARG MINECRAFT_VERSION
ENV MINECRAFT_VERSION ${MINECRAFT_VERSION}

# Download the Minecraft server jar based on the version
RUN wget $(awk -v ref="$MINECRAFT_VERSION" -F','  ' $1 == ref  { print substr($2, 1, length($2)-1) }' /minecraft-server/jar/versions.csv) -O /minecraft-server/jar/server.jar

# Set the working directory
WORKDIR /minecraft-server/data

# Copy the entrypoint script
COPY entrypoint.sh /minecraft-server/jar/entrypoint.sh
RUN chmod +x /minecraft-server/jar/entrypoint.sh

# Set entrypoint and start the Minecraft server
ENTRYPOINT ["/minecraft-server/jar/entrypoint.sh", "java", "-jar", "/minecraft-server/jar/server.jar"]
