# build lucee image
docker build -t blueriver/lucee:4.5.5.015-nginx .

# build mura standard image
docker build -t blueriver/mura:7.1.#PUT_LATEST_VERSION_OF_MURA_HERE#-lucee4.5.5.015-nginx  -f core/docker/build-4.5.5.015-nginx/Dockerfile .

# build custom for project image
docker build -t blueriver/mura:7.1.477.custom.1-lucee4.5.5.015-nginx  -f core/docker/build-4.5.5.015-nginx/Dockerfile .
