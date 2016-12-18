# openhab
Openhab 2.0 for Amazon Echo

So just a few heads up here.

* I modified the Dockerfile from https://github.com/openhab/openhab-docker to change the user id to 1001
```
RUN adduser --disabled-password --gecos '' --home ${APPDIR} openhab &&\
```
to
```
RUN adduser -u 1001 --disabled-password --gecos '' --home ${APPDIR} openhab &&\
```
* Added the Amazon Echo Discover port 1900/udp to EXPOSE
* From https://github.com/openhab/openhab-docker I then copied the entrypoint.sh to my disk also and then ran the build command
```
docker build -t openhab .
```
* Created a user on my host called openhab with
```
sudo useradd -u 1001 openhab
```
* Make the openhab user the owner of the 'openhab' dir on my host
```
sudo chown -R openhab:openhab openhab
```
* Start docker without any config volumes shared: 
```
docker run --name=openhab --net=host -v /etc/localtime:/etc/localtime:ro -v /etc/timezone:/etc/timezone:ro --restart always -d openhab:amd64-offline
```
* Copied the docker contents to my host with the below (find your own container ID via docker ps
```
docker cp 07860dca4899:/openhab /home/kodi/docker/openhab
```
* Stop/remove the docker container and started another one that mapped the containers volumes to my host
```
docker run --name=openhab --net=host -v /etc/localtime:/etc/localtime:ro -v /etc/timezone:/etc/timezone:ro -v /home/kodi/docker/openhab/conf:/openhab/conf -v /home/kodi/docker/openhab/userdata:/openhab/userdata -v /home/kodi/docker/openhab/addons:/openhab/addons --restart always -d openhab/openhab
```

* Now we have a stable and persistent openhab running.

Hope this helps someone.
