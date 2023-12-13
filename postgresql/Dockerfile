# Utiliza la imagen base de Ubuntu 22.04
FROM ubuntu:22.04

# Establece la variable de entorno DEBIAN_FRONTEND
ARG DEBIAN_FRONTEND=noninteractive

# Actualiza los repositorios e instala PostgreSQL
RUN apt-get update && \ 
    apt-get install -y postgresql

# Configura el usuario y la base de datos de PostgreSQL
USER postgres

# Inicia PostgreSQL y realiza la configuración
RUN /etc/init.d/postgresql start && \
    psql --command "CREATE USER myuser WITH SUPERUSER PASSWORD 'mypassword';" && \
    createdb -O myuser mydatabase && \
    echo "host all all 0.0.0.0/0 trust" >> /etc/postgresql/14/main/pg_hba.conf && \
    echo "listen_addresses='*'" >> /etc/postgresql/14/main/postgresql.conf && \
    /etc/init.d/postgresql stop

# Establece el directorio de datos de PostgreSQL (PGDATA)
ENV PGDATA=/var/lib/postgresql/data

# Agrega el directorio de ejecutables de PostgreSQL al PATH
ENV PATH="/usr/lib/postgresql/14/bin:${PATH}"

# Crea el directorio de datos y asigna los permisos
RUN mkdir -p $PGDATA && chown -R postgres:postgres $PGDATA

# Exponer el contenedor al puerto 5432 (default de PostgreSQL)
EXPOSE 5432

# CMD para iniciar PostgreSQL y mantener el contenedor en ejecución
CMD ["postgres", "-D", "/var/lib/postgresql/data", "-c", "config_file=/etc/postgresql/14/main/postgresql.conf"]