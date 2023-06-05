FROM postgres:14.1-alpine

ENV POSTGRES_DB=db \
    POSTGRES_USER=usr 

EXPOSE 5432

COPY CreateScheme.sql /docker-entrypoint-initdb.d
COPY InsertData.sql /docker-entrypoint-initdb.d

