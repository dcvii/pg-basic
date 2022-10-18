FROM postgres:15.0-alpine
ADD scripts/1_copy.sql /docker-entrypoint-initdb.d
ADD scripts/2_init.sql /docker-entrypoint-initdb.d
RUN chmod a+r /docker-entrypoint-initdb.d/*
EXPOSE 6666