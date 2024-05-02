# docker-openresty - Docker tooling for OpenResty

### [Docker Image GitHub Repo](https://github.com/wojiushixiaobai/docker-openresty)

`docker-openresty` is [Docker](https://www.docker.com) tooling for OpenResty (https://www.openresty.org).

Docker is a container management platform. OpenResty is a full-fledged web application server by
bundling the standard nginx core, lots of 3rd-party nginx modules, as well as most of their external dependencies.

# OpenResty Image Tags

It is best practice to pin your images to an explicit image tag.

| Image  | Description |
| --- | --- |
| `wojiushixiaobai/openresty:1.25.3.1-bookworm` | Built-from-source Debian Bookworm |

These are examples of untagged image names, for reference:

| Image | Description |
| --- | --- |
| `wojiushixiaobai/openresty:latest` | Latest Debian Bookworm |

----


Usage
=====

If you are happy with the build defaults, then you can use the openresty image from the [Docker Hub](https://hub.docker.com/r/wojiushixiaobai/openresty/).  The image tags available there are listed at the top of this README.

```
docker run [options] wojiushixiaobai/openresty:latest
```

*[options]* would be things like -p to map ports, -v to map volumes, and -d to daemonize.

`docker-openresty` symlinks `/usr/local/openresty/nginx/logs/access.log` and `error.log` to `/dev/stdout` and `/dev/stderr` respectively, so that Docker logging works correctly.  If you change the log paths in your `nginx.conf`, you should symlink those paths as well. This is not possible with the `windows` image.

Temporary directories such as `client_body_temp_path` are stored in `/var/run/openresty/`.  You may consider mounting that volume, rather than writing to a container-local directory.  This is not done for `windows`.

Supported tags and respective `Dockerfile` links
=========

The following "flavors" are available and built from [upstream OpenResty packages](https://openresty.org/en/linux-packages.html):

- [`bookworm`, (*Dockerfile*)](https://github.com/wojiushixiaobai/docker-openresty/blob/master/Dockerfile)

The `wojiushixiaobai/openresty:latest` tag points to the latest `bookworm` image.


Nginx Config Files
==================

The Docker tooling installs its own [`nginx.conf` file](https://github.com/openresty/docker-openresty/blob/master/nginx.conf).  If you want to directly override it, you can replace it in your own Dockerfile or via volume bind-mounting.

For the Linux images, that `nginx.conf` has the directive `include /etc/nginx/conf.d/*.conf;` so all nginx configurations in that directory will be included.  The [default virtual host configuration](https://github.com/openresty/docker-openresty/blob/master/nginx.vh.default.conf) has the original OpenResty configuration and is copied to `/etc/nginx/conf.d/default.conf`.

You can override that `default.conf` directly or volume bind-mount the `/etc/nginx/conf.d` directory to your own set of configurations:

```
docker run -v /my/custom/conf.d:/etc/nginx/conf.d wojiushixiaobai/openresty:latest
```

If you are running on an `selinux` host (e.g. CentOS), you may need to add `:Z` to your [volume bind-mount argument](https://docs.docker.com/storage/bind-mounts/#configure-the-selinux-label):
```
docker run -v /my/custom/conf.d:/etc/nginx/conf.d:Z wojiushixiaobai/openresty:latest
```


Building (from source)
======================

This Docker image can be built and customized by cloning the repo and running `docker build` with the desired Dockerfile:

```
git clone https://github.com/wojiushixiaobai/docker-openresty.git
cd docker-openresty
docker build -t myopenresty .
docker run myopenresty
```
