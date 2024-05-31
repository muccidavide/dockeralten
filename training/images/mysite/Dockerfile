#Multistage we use the first container to clone repo from git
#After that a new container is created and copy only the repo inside the new one
#The image generated contained only the LAST container! 

FROM bitnami/git as GIT_CONTAINER
# https://github.com/muccidavide/html-css-boolando
ARG REPOSITORY
# An argument is kind an an envirmonmet var:
# - We can use the argument as an env var
# - But... the arg is not going to be persisted to the final IMAGE
# - We can supply its value when calling the biuld command
RUN git clone $REPOSITORY /mysite
RUN rm -rf /mysite/.git

FROM nginx:latest
RUN rm -rf /usr/share/nginx/html
COPY --from=GIT_CONTAINER /mysite /usr/share/nginx/html
#This container is the one commited to an image
LABEL NGINX_VERSION=1.12
LABEL MAINTAINER="Davide Mucci <dmucci@email.com>"

#This is deprecated.. the above label should be use
# MAINTAINER IVAN OSUNA

HEALTHCHECK  --interval=10s \
            --start-period=30s \
            --timeout=5s \
            --retries=3 \
            CMD curl http://localhost:80/