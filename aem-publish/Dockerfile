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
RUN crx-quickstart/bin/aem-first-boot

# expose the port this instance of AEM is running on
EXPOSE $CQ_PORT

# run quickstart (fixed) to keep AEM in the foreground (instead of start, which
# backgrounds it).
ENTRYPOINT ["crx-quickstart/bin/quickstart-fixed"]
