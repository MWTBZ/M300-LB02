#
#	Samba Umgebung
#
FROM ubuntu:latest
MAINTAINER Mischa Wolf


# upgrade base system and install samba and supervisord
RUN apt-get update
RUN apt-get install -y samba

# create a dir for the config and the share
RUN mkdir /var/FileShare

# copy config files from project folder to get a default config going for samba and supervisord
COPY /config/smb.conf /etc/samba/

RUN useradd samba --shell /bin/false
#RUN echo Test | passwd -q samba

RUN useradd samba2 --shell /bin/false
#RUN echo Test2 | passwd samba2 -q

RUN (echo Test; echo Test) | smbpasswd -s -a samba 
RUN (echo Test2; echo Test2) | smbpasswd -s -a samba2

RUN /etc/init.d/smbd restart
# add a non-root user and group called "rio" with no password, no home dir, no shell, and gid/uid set to 1000
#RUN sudo addgroup --group smbusers && sudo adduser -D -H -G smbusers -s /bin/false -u 1000 samba1 && sudo adduser -D -H -G smbusers -s /bin/false -u 1000 samba2

# create a samba user matching our user from above with a very simple password ("letsdance")
#RUN echo -e "letsdance\nletsdance" | smbpasswd -a -s -c /config/smb.conf rio

# volume mappings
    #VOLUME /config /shared ../apache

# exposes samba's default ports (137, 138 for nmbd and 139, 445 for smbd)
EXPOSE 137/udp 138/udp 139 445

#ENTRYPOINT ["supervisord", "-c", "/config/supervisord.conf"]