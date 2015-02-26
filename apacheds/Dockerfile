FROM jjhughes57/apacheds-docker
MAINTAINER Reinold Pasterkamp

# Use config with cds partition
COPY ./config.ldif /var/lib/apacheds-2.0.0-M19/default/conf/
COPY ./admin.ldif /tmp/admin.ldif

#load default users and groups
ADD https://raw.githubusercontent.com/CDS-VRN/CDS-Assembly/vrn-assembly/src/main/docker/cds-ldap/ldap-init.ldif /root/ldap-init.ldif
RUN /opt/apacheds-2.0.0-M19/bin/apacheds start default && \
    sleep 20 && \
	ldapadd -h localhost -p 10389 -w secret -f /root/ldap-init.ldif -D uid=admin,ou=system -c && \
	ldapadd -h localhost -p 10389 -w secret -f /tmp/admin.ldif -D uid=admin,ou=system -c && \
	sleep 5 && \
	/opt/apacheds-2.0.0-M19/bin/apacheds stop default && \
	sleep 5 

EXPOSE 10389


ENTRYPOINT /opt/apacheds-2.0.0-M19/bin/apacheds console default