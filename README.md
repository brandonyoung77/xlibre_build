This is a set of Dockerfiles and a docker-compose.yaml file that will build XLibre from source and create deb packages to install it.

Includes the base xlibre-server-core , xlibre-server-dev headers, xf86-input-libinput, and xf86-video-amdgpu

To use, 
1. put the files in a base folder, and in that same folder checkout the xlibre xserver, xf86-input-libinput, and xf86-video-amdgpu projects.
2. Check out the specific tag version you want
3.  update the docker-compose.yaml with the version numbers to match the versions you checked out
4.  run docker compose up  and it should build xlibre and put the deb files in a folder called output
