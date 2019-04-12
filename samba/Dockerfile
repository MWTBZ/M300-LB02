FROM alpine:latest

RUN apk add --no-cache --update \
    samba-common-tools \
    samba-client \
    samba-server

COPY /samba/smb.conf /etc/samba/smb.conf

EXPOSE 445/tcp 139/tcp

CMD ["smbd", "--foreground", "--no-process-group", "--log-stdout"]

