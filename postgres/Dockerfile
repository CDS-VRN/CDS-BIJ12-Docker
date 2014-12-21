FROM ubuntu:12.04.5
MAINTAINER Reinold Pasterkamp

# Install postgresql/postgis see http://trac.osgeo.org/postgis/wiki/UsersWikiPostGIS21UbuntuPGSQL93Apt
RUN apt-get -y install wget
RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" >> /etc/apt/sources.list'
RUN wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add -
RUN apt-get update
RUN apt-get -y install postgresql-9.3-postgis 
RUN apt-get -y install language-pack-en

# get sources
ADD http://buildserver.geodan.nl/teamcity/guestAuth/repository/download/CdsAssembly_Build/72300:id/sql/create-database.sql /tmp/create-database.sql
ADD http://buildserver.geodan.nl/teamcity/guestAuth/repository/download/CdsAssembly_Build/72300:id/sql/add-themes.sql /tmp/add-themes.sql
RUN chmod +r /tmp/create-database.sql
RUN chmod +r /tmp/add-themes.sql

# Run the remainder of the commands under the postgres user:
USER postgres

RUN echo "host all all 0.0.0.0/0 md5" >> /etc/postgresql/9.3/main/pg_hba.conf

RUN echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf

# Create a CDS owner, postgis template and CDS database:
RUN /etc/init.d/postgresql start && \
        psql --command "CREATE USER inspire WITH PASSWORD 'inspire';" && \
        psql --command "CREATE USER nagios;" && \
        createdb -l en_US.UTF-8 -O inspire -E UTF-8 -T template0 cds && \
        psql -d cds --command "CREATE EXTENSION postgis;" && \
        psql -d cds -f /tmp/create-database.sql && \
        psql -d cds -f /tmp/add-themes.sql

EXPOSE 5432

VOLUME ["/etc/postgresql", "/var/log/postgresql"]

CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]

        
