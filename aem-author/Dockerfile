# AEM author image

FROM aem-base:latest

# use env vars to control AEM behavior. (see aem-base Dockerfile for additional
# CQ_* env vars.)
#
# ENV CQ_RUNMODE    "dev,author,nosamplecontent"
ENV CQ_RUNMODE    "author"
ENV CQ_PORT       4502

# name / hostname of the publish image
ENV PUBLISH_HOST publish


# install AEM and get it into the initial state for the desired runmode.
#
# the RUN command starts up AEM, waits until it responds, and then shuts down.
#
# NOTE: setting up replication agent directly after the startup does not seem
# completely reliable, so it's removed for now. (It was previously in between
# "done!" and "Stopping AEM...".) The replication agent will therefore need
# configuring in the running container via alternative methods.
#
#   echo "Setting up author replication agent..." && \
#   curl -u admin:admin --output /dev/null --silent http://localhost:4502/etc/replication/agents.author/publish/jcr:content -F transportUri=http://${PUBLISH_HOST}:4503/bin/receive?sling:authRequestLogin=1 && \
#
# TODO: change 'pauseCompaction' to 'false' in /opt/aem/crx-quickstart/launchpad/config/org/apache/jackrabbit/oak/plugins/segment/SegmentNodeStoreService.config
#
# FIXME: consider moving this to an external script now that author and
# publish startups are indentical.
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
