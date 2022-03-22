FROM ghcr.io/antonbabenko/pre-commit-terraform:latest

RUN apk update
RUN apk add git


COPY ["src", "/src/"]
RUN ["chmod", "+x", "/src/main.sh"]

# Checkov happy
HEALTHCHECK NONE

# Reduce Privileges
#RUN groupadd -r user && useradd --no-log-init -r -g user user
#USER user
USER root
ENTRYPOINT ["/src/main.sh"]
