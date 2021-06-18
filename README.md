# Ex180-Notes

[student@workstation ~]$ sudo podman login quay.io
Username: vnjsharma
Password: 
Login Succeeded!
[student@workstation ~]$ sudo podman run -d --name official-httpd -p 8180:80 quay.io/redhattraining/httpd-parent
Error: error creating container storage: the container name "official-httpd" is already in use by "ba5df518d29738d3d30d7eff6ed94421f236f48e123a60e511354d7ca16e0220". You have to remove that container to be able to reuse that name.: that name is already in use
[student@workstation ~]$ sudo podman images
REPOSITORY                            TAG             IMAGE ID       CREATED         SIZE
localhost/do180/mynginx               v1.0-SNAPSHOT   b88f4f133a76   22 hours ago    131 MB
localhost/do180-custom-httpd          latest          c38e798ab85d   23 hours ago    236 MB
quay.io/redhattraining/nginx          1.17            9beeba249f3e   13 months ago   131 MB
quay.io/redhattraining/httpd-parent   latest          4346d3cace25   2 years ago     236 MB
[student@workstation ~]$ sudo podman ps
CONTAINER ID  IMAGE  COMMAND  CREATED  STATUS  PORTS  NAMES
[student@workstation ~]$ sudo podman ps -a
CONTAINER ID  IMAGE                                       COMMAND               CREATED       STATUS                   PORTS                 NAMES
d1a1539b20ef  localhost/do180/mynginx:v1.0-SNAPSHOT       nginx -g daemon o...  22 hours ago  Exited (0) 22 hours ago  0.0.0.0:8080->80/tcp  official-nginx-dev
926cce98f30d  quay.io/redhattraining/nginx:1.17           nginx -g daemon o...  23 hours ago  Exited (0) 22 hours ago  0.0.0.0:8080->80/tcp  official-nginx
ba5df518d297  quay.io/redhattraining/httpd-parent:latest  /bin/sh -c /usr/s...  23 hours ago  Exited (0) 23 hours ago  0.0.0.0:8180->80/tcp  official-httpd
[student@workstation ~]$ ^C
[student@workstation ~]$ sudo podman rm ba5df518d297
ba5df518d29738d3d30d7eff6ed94421f236f48e123a60e511354d7ca16e0220
[student@workstation ~]$ sudo podman run -d --name official-httpd -p 8180:80 quay.io/redhattraining/httpd-parent
ab6e601dd8eaafea90057a6ff3165b09e1bd2d3c57f16cd82820d3e621ab64a5
[student@workstation ~]$ sudo podman exec -it official-httpd /bin/bash
bash-4.4# echo "DO180 Page" > /var/www/html/do180.html
bash-4.4#  curl 127.0.0.1:8180/do180.html
curl: (7) Failed to connect to 127.0.0.1 port 8180: Connection refused
bash-4.4# exit
exit
Error: non zero exit code: 7: OCI runtime error
[student@workstation ~]$  curl 127.0.0.1:8180/do180.html
DO180 Page
[student@workstation ~]$  sudo podman diff official-httpd
C /root
A /root/.bash_history
C /run/httpd
A /run/httpd/cgisock.1
A /run/httpd/httpd.pid
C /tmp
C /var
C /var/log
C /var/log/httpd
A /var/log/httpd/access_log
A /var/log/httpd/error_log
C /var/www
C /var/www/html
A /var/www/html/do180.html
[student@workstation ~]$ sudo podman stop official-httpd
ab6e601dd8eaafea90057a6ff3165b09e1bd2d3c57f16cd82820d3e621ab64a5
[student@workstation ~]$ sudo podman commit -a 'vnjsharma' official-httpd do180-custom-httpd
Getting image source signatures
Copying blob 24d85c895b6b skipped: already exists
Copying blob c613b100be16 skipped: already exists
Copying blob 574bcc187eda skipped: already exists
Copying blob 7f9108fde4a1 skipped: already exists
Copying blob 48dc04419440 done
Copying config 3060638076 done
Writing manifest to image destination
Storing signatures
30606380765386d17ada63001e771e9660e2c3a2a86679bc047212abaa0f1b39
[student@workstation ~]$ sudo podman images
REPOSITORY                            TAG             IMAGE ID       CREATED          SIZE
localhost/do180-custom-httpd          latest          306063807653   12 seconds ago   236 MB
localhost/do180/mynginx               v1.0-SNAPSHOT   b88f4f133a76   23 hours ago     131 MB
<none>                                <none>          c38e798ab85d   23 hours ago     236 MB
quay.io/redhattraining/nginx          1.17            9beeba249f3e   13 months ago    131 MB
quay.io/redhattraining/httpd-parent   latest          4346d3cace25   2 years ago      236 MB
[student@workstation ~]$ cat /usr/local/etc/ocp4.config
cat: /usr/local/etc/ocp4.config: No such file or directory
[student@workstation ~]$ cd /usr/local/etc/
[student@workstation etc]$ ls
ocp4.defaults
[student@workstation etc]$ sudo podman whoami
Error: unrecognized command `podman whoami`
Try 'podman --help' for more information.
[student@workstation etc]$ podman whoami
Error: unrecognized command `podman whoami`
Try 'podman --help' for more information.
[student@workstation etc]$ podman help
manage pods and images

Usage:
  podman [flags]
  podman [command]

Available Commands:
  attach      Attach to a running container
  build       Build an image using instructions from Containerfiles
  commit      Create new image based on the changed container
  container   Manage Containers
  cp          Copy files/folders between a container and the local filesystem
  create      Create but do not start a container
  diff        Inspect changes on container's file systems
  events      Show podman events
  exec        Run a process in a running container
  export      Export container's filesystem contents as a tar archive
  generate    Generated structured data
  healthcheck Manage Healthcheck
  help        Help about any command
  history     Show history of a specified image
  image       Manage images
  images      List images in local storage
  import      Import a tarball to create a filesystem image
  info        Display podman system information
  init        Initialize one or more containers
  inspect     Display the configuration of a container or image
  kill        Kill one or more running containers with a specific signal
  load        Load an image from container archive
  login       Login to a container registry
  logout      Logout of a container registry
  logs        Fetch the logs of a container
  mount       Mount a working container's root filesystem
  network     Manage Networks
  pause       Pause all the processes in one or more containers
  play        Play a pod
  pod         Manage pods
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image from a registry
  push        Push an image to a specified destination
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Removes one or more images from local storage
  run         Run a command in a new container
  save        Save image to an archive
  search      Search registry for image
  start       Start one or more containers
  stats       Display a live stream of container resource usage statistics
  stop        Stop one or more containers
  system      Manage podman
  tag         Add an additional name to a local image
  top         Display the running processes of a container
  umount      Unmounts working container's root filesystem
  unpause     Unpause the processes in one or more containers
  unshare     Run a command in a modified user namespace
  varlink     Run varlink interface
  version     Display the Podman Version Information
  volume      Manage volumes
  wait        Block on one or more containers

Flags:
      --cgroup-manager string     Cgroup manager is not supported in rootless mode
      --cni-config-dir string     Path of the configuration directory for CNI networks
      --config string             Path of a libpod config file detailing container server configuration options
      --conmon string             Path of the conmon binary
      --cpu-profile string        Path for the cpu profiling results
      --events-backend string     Events backend to use
      --help                      Help for podman
      --hooks-dir strings         Set the OCI hooks directory path (may be set multiple times)
      --log-level string          Log messages above specified level: debug, info, warn, error, fatal or panic (default "error")
      --namespace string          Set the libpod namespace, used to create separate views of the containers and pods on the system
      --network-cmd-path string   Path to the command for configuring the network
      --root string               Path to the root directory in which data, including images, is stored
      --runroot string            Path to the 'run directory' where all state information is stored
      --runtime string            Path to the OCI-compatible binary used to run containers, default is /usr/bin/runc
      --storage-driver string     Select which storage driver is used to manage storage of images and containers (default is overlay)
      --storage-opt stringArray   Used to pass an option to the storage driver
      --syslog                    Output logging information to syslog as well as the console
      --tmpdir string             Path to the tmp directory
      --trace                     Enable opentracing output
  -v, --version                   Version of podman

Use "podman [command] --help" for more information about a command.
[student@workstation etc]$ podman login
Error: please specify a registry to login to
[student@workstation etc]$  source /usr/local/etc/ocp4.config
bash: /usr/local/etc/ocp4.config: No such file or directory
[student@workstation etc]$ cd ~
[student@workstation ~]$ sudo podman tag do180-custom-httpd quay.io/vnjsharma/do180-custom-httpd:v1.0^C
[student@workstation ~]$ sudo podman images
REPOSITORY                            TAG             IMAGE ID       CREATED         SIZE
localhost/do180-custom-httpd          latest          306063807653   8 minutes ago   236 MB
localhost/do180/mynginx               v1.0-SNAPSHOT   b88f4f133a76   23 hours ago    131 MB
<none>                                <none>          c38e798ab85d   24 hours ago    236 MB
quay.io/redhattraining/nginx          1.17            9beeba249f3e   13 months ago   131 MB
quay.io/redhattraining/httpd-parent   latest          4346d3cace25   2 years ago     236 MB
[student@workstation ~]$ sudo podman tag do180-custom-httpd quay.io/vnjsharma/do180-custom-httpd:v1.0
[student@workstation ~]$ sudo podman images
REPOSITORY                             TAG             IMAGE ID       CREATED         SIZE
localhost/do180-custom-httpd           latest          306063807653   9 minutes ago   236 MB
quay.io/vnjsharma/do180-custom-httpd   v1.0            306063807653   9 minutes ago   236 MB
localhost/do180/mynginx                v1.0-SNAPSHOT   b88f4f133a76   23 hours ago    131 MB
<none>                                 <none>          c38e798ab85d   24 hours ago    236 MB
quay.io/redhattraining/nginx           1.17            9beeba249f3e   13 months ago   131 MB
quay.io/redhattraining/httpd-parent    latest          4346d3cace25   2 years ago     236 MB
[student@workstation ~]$ sudo podman push quay.io/vnjsharma/do180-custom-httpd:v1.0
Getting image source signatures
Copying blob 48dc04419440 done
Copying blob 7f9108fde4a1 skipped: already exists
Copying blob c613b100be16 skipped: already exists
Copying blob 574bcc187eda skipped: already exists
Copying blob 24d85c895b6b skipped: already exists
Error: Error copying image to the remote destination: Error writing blob: Error initiating layer upload to /v2/vnjsharma/do180-custom-httpd/blobs/uploads/ in quay.io: unauthorized: access to the requested resource is not authorized
[student@workstation ~]$ sudo podman logout
Error: registry must be given
[student@workstation ~]$ sudo podman logout quay.io
Removed login credentials for quay.io
[student@workstation ~]$ sudo podman login quay.io
Username: vnjsharma
Password: 
Login Succeeded!
[student@workstation ~]$ sudo podman push quay.io/vnjsharma/do180-custom-httpd:v1.0
Getting image source signatures
Copying blob 48dc04419440 done
Copying blob 24d85c895b6b skipped: already exists
Copying blob 574bcc187eda skipped: already exists
Copying blob 7f9108fde4a1 skipped: already exists
Copying blob c613b100be16 skipped: already exists
Copying config 3060638076 done
Writing manifest to image destination
Copying config 3060638076 done
Writing manifest to image destination
Writing manifest to image destination
Storing signatures
[student@workstation ~]$ 
