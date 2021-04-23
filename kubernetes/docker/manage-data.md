

#  writable container layer

- The data doesn't persist when that container in no longer running.
- you can't easily move the data
- writable layer is not performance compared to  data volumes.

# store files in the host macchine

- volumes   managed by docker.
- bind mounts  unmanage , not recomand  , but can change the file system on the host
- tmpfs    secrets and so on


# use case volumes

- sharing data  , volumes only remove when your explicity do it, create if not exist.
- decope docker runing with host
- store data on a remote  host
- back up , restore , migrate data from  one docker host to another.

# use case bind mounts

- sharing configuration files from the host
- sharing source code or build artifacts on the docker host and a container  mount maven target/ directory into a container, build maven on the host
- when you have a rule define the host directory structure with container


# use case for tmpfs

- security  performance
