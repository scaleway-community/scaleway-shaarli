## -*- docker-image-name: "scaleway/ipfs:latest" -*-
ARG SCW_ARCH

FROM scaleway/ubuntu:${SCW_ARCH}-xenial

MAINTAINER Scaleway <opensource@scaleway.com> (@scaleway)

# Prepare rootfs for image-builder
RUN /usr/local/sbin/builder-enter

# Install some apps
RUN apt-get update \
    && apt-get install -y \
       apache2 \
       php7.0 \
       libapache2-mod-php7.0 \
       jq

# Download Shaarli
RUN wget $(curl -s https://api.github.com/repos/shaarli/Shaarli/releases \
           | jq -r '.[0]["assets"][0]["browser_download_url"]') \
         -O shaarli.tar.gz \
    && tar xvzf shaarli.tar.gz \
    && mv Shaarli /var/www/html/ \ 
    && chown -R root:www-data /var/www/html/Shaarli/

# Copy files
COPY overlay ./

# Create server specific doc in /root/ for first login
RUN systemctl enable create-doc

# Symlink doc readme to /root/
RUN ln -s /usr/share/doc/scaleway/shaarli/README /root/README

# Clean rootfs from image-builder
RUN /usr/local/sbin/builder-leave
