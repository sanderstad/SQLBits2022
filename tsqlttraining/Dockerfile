# building from ubuntu
FROM mcr.microsoft.com/mssql/server:2019-latest

# creating directories
USER root
RUN mkdir /shared; chown mssql /shared
RUN mkdir /var/opt/sqlserver; chown mssql /var/opt/sqlserver
RUN mkdir /var/opt/sqlserver/data; chown mssql /var/opt/sqlserver/data
RUN mkdir /var/opt/sqlserver/log; chown mssql /var/opt/sqlserver/log
RUN mkdir /var/opt/sqlserver/backup; chown mssql /var/opt/sqlserver/backup
RUN mkdir /tmp/tsqlt; chown mssql /tmp/tsqlt

# set the pwd and chown to the service account
WORKDIR /tmp
RUN chown mssql /tmp

# copy scripts and make bash files executable
# use copy instead of add, it's safer apparently
COPY sql scripts /tmp/
RUN chmod +x /tmp/*.sh

COPY tsqlt /tmp/tsqlt

# convert CRLF to LF in case Windows or VS Code changed it
RUN find . -type f \( -name "*.sql" -o -name "*.env" -o -name "*.sh" \) -exec sed -i 's/\r$//' {} \;

# run initial setup scripts
RUN /bin/bash /tmp/install-sqlcmd.sh

# run a rootless container
USER mssql

# run initial setup scripts
RUN /bin/bash /tmp/start-sql.sh

ENTRYPOINT /opt/mssql/bin/sqlservr

# label the container
LABEL org.opencontainers.image.vendor="sanderstad"
LABEL org.opencontainers.image.title="tsqlttraining"
LABEL org.opencontainers.image.version=1.0
LABEL org.label-schema.description="SQL Server image to help with training in tSQLt"
LABEL org.opencontainers.image.authors="Sander Stad <sander@sqlstad.nl>"