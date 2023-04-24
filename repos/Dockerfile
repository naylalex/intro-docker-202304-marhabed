FROM postgres:14.3-alpine as db

WORKDIR /wordsmith/db

COPY /wordsmith/db/words.sql /docker-entrypoint-initdb.d/

ENV POSTGRES_HOST_AUTH_METHOD=trust

EXPOSE 5432


# RUN ./dispatcher


FROM ubuntu as words

WORKDIR /wordsmith/words


COPY /wordsmith/words/pom.xml .
COPY /wordsmith/words/src src

COPY /dockerfiles_multi_stage/start.sh .

RUN apt-get update &&  apt-get install -y maven openjdk-8-jdk && mvn verify

FROM golang:1.16 as web

WORKDIR /wordsmith/web

COPY /wordsmith/web/dispatcher.go .
COPY /wordsmith/web/static static
RUN go build dispatcher.go

EXPOSE 80
EXPOSE 8080

CMD ["sh", "start.sh"]


