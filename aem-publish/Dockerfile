# AEM publish image

FROM aem-base:latest

# use env vars to control AEM behavior. (see aem-base Dockerfile for additional
# CQ_* env vars.)
#
# ENV CQ_RUNMODE    "dev,publish,nosamplecontent"
ENV CQ_RUNMODE    "publish"
ENV CQ_PORT       4503

# name / hostname of the author image (currently unused)
ENV AUTHOR_HOST author


# install AEM and get it into the initial state for the desired runmode.
#
# the RUN command starts up AEM, waits until it responds, installs replication
# agent to publish instance, and then shuts down.
#
RUN echo "Starting AEM with env:" && \
    env | sort | sed -e 's/^/  - /' && \
    echo -n "Initial startup (installation) of AEM" && \
    crx-quickstart/bin/start && \
    until $(curl -u admin:admin --output /dev/null --silent --head --fail http://localhost:$CQ_PORT); do printf '.'; sleep 5; done && \
    echo "done!" && \
    echo "Stopping AEM..." && \
    crx-quickstart/bin/stop



EXPOSE $CQ_PORT

# run quickstart (fixed) to keep AEM in the foreground (instead of start, which
# backgrounds it).
ENTRYPOINT crx-quickstart/bin/quickstart-fixed
