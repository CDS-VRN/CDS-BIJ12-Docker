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
ADD http://ftp.geodan.nl/vrn/sql/create-database.sql /tmp/create-database.sql
ADD http://ftp.geodan.nl/vrn/sql/add-themes.sql /tmp/add-themes.sql
RUN chmod +r /tmp/*.sql


# Run the remainder of the commands under the postgres user:
USER postgres

RUN echo "localhost:5432:cds:cds_owner:cds_owner"  >> ~/.pgpass && \
    chmod 0600 ~/.pgpass

RUN echo "host all all 0.0.0.0/0 md5" >> /etc/postgresql/9.3/main/pg_hba.conf

RUN echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf

# Create a CDS owner, postgis extendsion and CDS database:
RUN /etc/init.d/postgresql start && \
        psql --command "CREATE USER inspire WITH PASSWORD 'inspire';" && \
		psql --command "CREATE USER cds_owner WITH PASSWORD 'cds_owner';" && \
        psql --command "CREATE USER nagios;" && \
        createdb -l en_US.UTF-8 -O cds_owner -E UTF-8 -T template0 cds && \
        psql -d cds --command "CREATE EXTENSION postgis;" && \
        psql -U cds_owner -h localhost -d cds -f /tmp/create-database.sql && \
        psql -U cds_owner -h localhost -d cds -f /tmp/add-themes.sql && \
		psql -d cds --command "GRANT ALL ON SCHEMA vrn TO cds_owner;" && \
		psql -d cds --command "INSERT INTO manager.gebruiker (gebruikersnaam, superuser) VALUES ('admin', true);"

EXPOSE 5432

VOLUME ["/etc/postgresql", "/var/log/postgresql"]

CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]

        
