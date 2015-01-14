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
ADD https://raw.githubusercontent.com/CDS-VRN/CDS-BIJ12/master/vrn-themes/src/main/sql/1_create_schema.sql /tmp/1_create_schema.sql
ADD https://raw.githubusercontent.com/CDS-VRN/CDS-BIJ12/master/vrn-themes/src/main/sql/2_vrn_ddl.sql /tmp/2_vrn_ddl.sql
ADD https://raw.githubusercontent.com/CDS-VRN/CDS-BIJ12/master/vrn-themes/src/main/sql/3_create_constraints.sql /tmp/3_create_constraints.sql
ADD https://raw.githubusercontent.com/CDS-VRN/CDS-BIJ12/master/vrn-themes/src/main/sql/4_create_indexes.sql /tmp/4_create_indexes.sql
ADD https://raw.githubusercontent.com/CDS-VRN/CDS-BIJ12/master/vrn-themes/src/main/sql/5_insert_metadata.sql /tmp/5_insert_metadata.sql
ADD https://raw.githubusercontent.com/CDS-VRN/CDS-BIJ12/master/vrn-themes/src/main/sql/6_insert_codemappings.sql /tmp/6_insert_codemappings.sql
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
		psql -d cds -f /tmp/1_create_schema.sql && \
		psql -d cds --command "GRANT ALL ON SCHEMA vrn TO cds_owner;" && \
		psql -U cds_owner -h localhost -d cds -f /tmp/2_vrn_ddl.sql && \
		psql -U cds_owner -h localhost -d cds -f /tmp/3_create_constraints.sql && \
		psql -U cds_owner -h localhost -d cds -f /tmp/4_create_indexes.sql && \
		psql -U cds_owner -h localhost -d cds -f /tmp/5_insert_metadata.sql && \
		psql -U cds_owner -h localhost -d cds -f /tmp/6_insert_codemappings.sql

EXPOSE 5432

VOLUME ["/etc/postgresql", "/var/log/postgresql"]

CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]

        
