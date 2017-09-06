# base on 16.07 until new workflow run menu can handle mothur workflow without crashing
FROM quay.io/shiltemann/galaxy-ireport:16.07

MAINTAINER Saskia Hiltemann, zazkia@gmail.com

ENV GALAXY_CONFIG_BRAND="Galaxy Mothur Toolset (GmT)"

RUN pip install -U ephemeris
ADD docker/galaxy-sleep.py $GALAXY_ROOT/sleep.py

# install tools
ADD docker/tools.yaml $GALAXY_ROOT/tools.yaml

RUN startup_lite && \
	$GALAXY_ROOT/sleep.py http://localhost:8080 && \
	shed-install -a $GALAXY_CONFIG_MASTER_API_KEY -t $GALAXY_ROOT/tools.yaml -g "http://localhost:8080" --toolshed "https://testtoolshed.g2.bx.psu.edu/"

ADD workflow.ga $GALAXY_ROOT/workflow.ga

# install workflows
RUN startup_lite && \
	$GALAXY_ROOT/sleep.py http://localhost:8080 && \
    workflow-install -g http://localhost:8080 -a admin -w $GALAXY_ROOT/workflow.ga
