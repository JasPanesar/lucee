# Build lucee image
docker build -t blueriver/lucee:5.3.3.54-SNAPSHOT-tomcat9.0-jre11 .

# build mura standard image
docker build -t blueriver/mura:7.1.#PUT_LATEST_VERSION_OF_MURA_HERE#-lucee5.3.3.54-SNAPSHOT-tomcat9.0-jre11  -f core/docker/build-5.3.3.54-SNAPSHOT-tomcat9.0-jre11/Dockerfile .

# build custom for Proect image SNAPSHOT
docker build -t blueriver/mura:7.1.477.1-lucee5.3.3.54-SNAPSHOT-tomcat9.0-jre11  -f core/docker/build-5.3.3.54-SNAPSHOT-tomcat9.0-jre11/Dockerfile .
