# AEM author image

FROM aem-base:latest

# use env vars to control AEM behavior. (see aem-base Dockerfile for additional
# CQ_* env vars.)
#
# ENV CQ_RUNMODE    "dev,author,nosamplecontent"
ENV CQ_RUNMODE    "author"
ENV CQ_PORT       4502

# name / hostname of the publish image (currently unused)
ENV PUBLISH_HOST publish

# install AEM and get it into the initial state for the desired runmode.
RUN crx-quickstart/bin/aem-first-boot

# expose the port this instance of AEM is running on
EXPOSE $CQ_PORT

# run quickstart (fixed) to keep AEM in the foreground (instead of start, which
# backgrounds it).
ENTRYPOINT ["crx-quickstart/bin/quickstart-fixed"]
