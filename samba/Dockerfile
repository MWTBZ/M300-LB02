FROM alpine:latest
MAINTAINER Mischa Wolf

# Install Samba
RUN apk add --no-cache --update \
    samba-common-tools \
    samba-client \
    samba-server \
    bash


# Copy smb.conf
COPY /samba/smb.conf /etc/samba/smb.conf

# Expose Ports 445 / 139
EXPOSE 445/tcp 139/tcp

CMD ["smbd", "--foreground", "--no-process-group", "--log-stdout"]