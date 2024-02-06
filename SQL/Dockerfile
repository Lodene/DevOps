FROM postgres:14.1-alpine

ENV POSTGRES_DB=mydatabase
ENV POSTGRES_USER=myuser

COPY createandinsert.sql /docker-entrypoint-initdb.d/
