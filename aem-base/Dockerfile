# AEM base image (used by -author and -publish)

FROM ubuntu-java

# set TERM so basic things like screen clearing and 'top' will work.
ENV TERM xterm

ENV AEM_VERSION 6.1
# NOTE: we're not using *-runmode-port.jar naming convention for jar because it
# doesn't matter for unpacking. run modes are set via CQ_RUNMODE in the -author
# and -publish Dockerfiles.
ENV AEM_QUICKSTART_FILE aem-quickstart.jar
ENV AEM_USER aem
ENV AEM_WORKING_DIR /opt/aem

# set some common env vars to be used by the -author and -publish images.
#
# NOTE: the following vars are only used by the 'quickstart' script, so they
# don't apply during initial startup (installation) via 'start'.
ENV CQ_VERBOSE    true
ENV CQ_GUI        ''
ENV CQ_NOBROWSER  true
ENV CQ_NOFORK     true

# increase memory to 1.5GB. (this assumes docker has enough RAM.)
#
# since we're increasing the memory via overriding the opts, we need to set the
# other options normally set via the 'quickstart' script so remain applied. the
# default options are:
#
#   CQ_JVM_OPTS='-server -Xmx1024m -XX:MaxPermSize=256M'
#
# NOTE: the ' -Djava.awt.headless=true' below is from the start script, which
# seems like a good idea, though it's not clear if it's duplicative of CQ_GUI.
#
ENV CQ_JVM_OPTS   "-server -Xms512M -Xmx1524M -XX:MaxPermSize=512M -Djava.awt.headless=true"

# all future commands will be relative to $AEM_WORKING_DIR
WORKDIR $AEM_WORKING_DIR

# Create a user & group for AEM to run under, with the homedir set to the WORKDIR
RUN groupadd -r $AEM_USER && \
    useradd -r -g $AEM_USER $AEM_USER -d $AEM_WORKING_DIR

# Add and then unpack the quickstart jar
ADD ./$AEM_QUICKSTART_FILE ./
RUN java -jar ./$AEM_QUICKSTART_FILE -unpack


# Add some files for use by -author and -publish Docker images
#
# Add the AEM license file
ADD ./license.properties   ./

# Add a "fixed" quickstart script. See code comments in file for rationale.
ADD ./quickstart-fixed ./crx-quickstart/bin/

# Add a first-boot script for "priming" an AEM instance.
ADD ./aem-first-boot ./crx-quickstart/bin/


# Adjust ownership of WORKDIR after all files have been added. (Files are
# added with ownership set to root.)
RUN chown -R $AEM_USER:$AEM_USER .


USER $AEM_USER
