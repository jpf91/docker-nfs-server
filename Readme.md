# [jpf91/nfs-server](https://github.com/jpf91/docker-nfs-server)

This image provides an NFS3/4 server image based on the latest fedora release.

## Supported Architectures

Currently only `x86_64` images are being built, allthough the `Dockerfile` is not architecture dependent.

## Usage

Here are some instructions and snippets to help you get started creating a container.

### Exports configuration

Before starting the container the first time, you need to setup an exports file:

Here's an example:
```bash
/mnt/storage	192.168.0.0/16(rw,fsid=0,no_subtree_check)
```

For more information, refer to the man page for `/etc/exports`.


### Running using podman cli

```
podman run --name nfs-server --privileged \
  -h nas.example.com \
  --net host \
  -e container=podman \
  -v /mnt/storage:/mnt/storage \
  -v </path/to/appdata/exports>:/etc/exports \
  -v </path/to/ipa-client-data/data/krb5.keytab>:/etc/krb5.keytab \
  -v </path/to/ipa-client-data/data/krb5.conf>:/etc/krb5.conf \
  -v </path/to/ipa-client-data/data/krb5.conf.d>:/etc/krb5.conf.d \
  -v </path/to/ipa-client-data/data/pipes>:/var/lib/sss/pipes \
  docker.io/jpf91/nfs-server
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 22:22` would expose port `22` from inside the container to be accessible from the host's IP on port `22` outside the container.

| Parameter | Function |
| :----: | --- |
| `-privileged` | Container needs to be run in privileged mode. |
| `--net host` | Using host networking easily enables all tcp/udp ports. You can use normal networking and specify the NFS ports as well. |
| `-h nas.example.com` | Set the hostname, must be fully qualified. |
| `-v /etc/exports` | NFS export configuration. See above for an example. |
| `-v /etc/krb5.keytab` | The SSSD container will write kerberos information to this file. |
| `-v /etc/krb5.conf` | For SSSD integration, share this with your SSSD container. |
| `-v /etc/krb5.conf.d` | For SSSD integration, share this with your SSSD container. |
| `-v /var/lib/sss/pipes/` | For SSSD integration, share this with your SSSD container. |

## Support Info

* Shell access whilst the container is running: `podman exec -it nfs-server /bin/bash`
* To monitor the logs of the container in realtime: `podman logs -f nfs-server`
* Report bugs [here](https://github.com/jpf91/docker-nfs-server).

## Building locally

If you want to make local modifications to these images for development purposes or just to customize the logic:
```
git clone https://github.com/jpf91/docker-nfs-server.git
cd docker-nfs-server
podman build \
  -t docker.io/jpf91/nfs-server:latest .
```

## Versions

* **05.04.21:** - Use fedora-systemd base image.
* **03.04.21:** - Initial Release.
