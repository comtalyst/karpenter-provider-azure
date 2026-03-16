FROM mcr.microsoft.com/oss/go/microsoft/golang:1.26-bookworm@sha256:b35139ded89d3da7fdb87735a84e94016ea3a9547db792269e7dcca7a7c9ee4c

# [Optional] Uncomment this section to install additional OS packages.
# graphviz for pprof
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends graphviz
