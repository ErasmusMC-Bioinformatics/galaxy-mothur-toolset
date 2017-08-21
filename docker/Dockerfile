# Galaxy - Metagenomics

## base on 16.07 until new workflow run menu can handle mothur workflow without crashing
FROM bgruening/galaxy-stable:16.07

MAINTAINER Saskia Hiltemann, zazkia@gmail.com

ENV GALAXY_CONFIG_BRAND Galaxy Mothur Toolset

RUN pip install ephemeris -U

# install tools
ADD docker/tools.yaml $GALAXY_ROOT/tools.yaml
RUN install-tools $GALAXY_ROOT/tools.yaml && \
    /tool_deps/_conda/bin/conda clean --tarballs

# install workflow
RUN startup_lite && \
    workflow-install -g http://localhost:8080 -a admin -w workflow.ga
