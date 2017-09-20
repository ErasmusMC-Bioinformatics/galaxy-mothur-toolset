# base on 16.07 until new workflow run menu can handle mothur workflow without crashing
FROM quay.io/shiltemann/galaxy-ireport:16.07

MAINTAINER Saskia Hiltemann, zazkia@gmail.com

ENV GALAXY_CONFIG_BRAND="Galaxy Mothur Toolset (GmT)" \
    GALAXY_CONFIG_CONDA_ENSURE_CHANNELS="iuc,bioconda,conda-forge,defaults,r"

RUN apt-get update && apt-get install libxrender1
RUN pip install -U ephemeris
ADD docker/galaxy-sleep.py $GALAXY_ROOT/sleep.py

# install tools
ADD docker/tools.yaml $GALAXY_ROOT/tools.yaml

RUN startup_lite && \
	$GALAXY_ROOT/sleep.py http://localhost:8080 && \
	shed-install -a $GALAXY_CONFIG_MASTER_API_KEY -t $GALAXY_ROOT/tools.yaml -g "http://localhost:8080" --toolshed "https://testtoolshed.g2.bx.psu.edu/" && \
    sleep 360

# everything is awful (conda package comes with wrong version of boost in this version of Galaxy for some reason)
RUN /tool_deps/_conda/bin/conda install -n __mothur@1.36.1 -y boost=1.57.0

# everything is more awful
RUN /tool_deps/_conda/bin/conda install -n __fontconfig@2.11.1 pango -c asmeurer

# install workflows and data libraries
ADD workflow.ga $GALAXY_ROOT/workflow.ga
ADD data_libraries.yaml $GALAXY_ROOT/data_libraries.yaml

RUN startup_lite && \
	$GALAXY_ROOT/sleep.py "http://localhost:8080" && \
    workflow-install -g "http://localhost:8080" -a admin -w $GALAXY_ROOT/workflow.ga && \
    setup-data-libraries -g "http://localhost:8080" -a admin -i $GALAXY_ROOT/data_libraries.yaml

# install tour
ADD tour.yaml $GALAXY_ROOT/config/plugins/tours/mothur-miseq-sop.yaml
