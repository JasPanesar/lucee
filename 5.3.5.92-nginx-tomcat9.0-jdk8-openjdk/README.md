# build lucee image
docker build -t blueriver/lucee:5.3.5.92-nginx-tomcat9.0-jdk8-openjdk .

# build mura standard image
docker build -t blueriver/mura:7.1.#PUT_LATEST_VERSION_OF_MURA_HERE#-lucee5.3.5.92-nginx-tomcat9.0-jdk8-openjdk  -f core/docker/build-5.3.5.92-nginx-tomcat9.0-jdk8-openjdk/Dockerfile .

# build custom for project image
docker build -t blueriver/mura:7.1.477.custom.1-lucee5.3.5.92-nginx-tomcat9.0-jdk8-openjdk  -f core/docker/build-5.3.5.92-nginx-tomcat9.0-jdk8-openjdk/Dockerfile .
