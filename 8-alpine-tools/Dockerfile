# Use the SDK image to build and install tools
FROM mcr.microsoft.com/dotnet/sdk:8.0-alpine@sha256:8e5832fb40462f8dd93f317d204d6f65d58b100a77884d0b132ad982aa65e5b5 AS build
RUN dotnet tool install --global dotnet-trace
RUN dotnet tool install --global dotnet-counters
RUN dotnet tool install --global dotnet-dump
RUN dotnet tool install --global dotnet-gcmon
RUN dotnet tool install --global dotnet-stack

FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine@sha256:f07b7f599e6f0e2e80957eceecb0dca04528b67b54d28e1dbd42f19a6dce8368
# Copy installed tools from the build image. This increases image size from 262MB to 370MB
COPY --from=build /root/.dotnet /root/.dotnet
ENV PATH="$PATH:/root/.dotnet/tools"

# Defaults New Relic
ENV CORECLR_NEWRELIC_HOME	/usr/local/newrelic-dotnet-agent
ENV CORECLR_PROFILER	{36032161-FFC0-4B61-B559-F6C5D41BAE5A}
ENV CORECLR_PROFILER_PATH	/usr/local/newrelic-dotnet-agent/libNewRelicProfiler.so
ENV NEW_RELIC_APPLICATION_LOGGING_ENABLED	false
ENV NEW_RELIC_APPLICATION_LOGGING_FORWARDING_ENABLED	false
ENV NEW_RELIC_DISTRIBUTED_TRACING_ENABLED	true

# This variable enables New Relic (disabled by default)
ENV CORECLR_ENABLE_PROFILING 0

# These variables need to be set in the task definition
# NEW_RELIC_APP_NAME	<service name>
# NEW_RELIC_LICENSE_KEY	<license key for dev / prod>

# install the agent
RUN  mkdir /usr/local/newrelic-dotnet-agent \
&& cd /usr/local \
&& export NEW_RELIC_DOWNLOAD_URI=https://download.newrelic.com/$(wget -qO - "https://nr-downloads-main.s3.amazonaws.com/?delimiter=/&prefix=dot_net_agent/latest_release/newrelic-dotnet-agent" | grep -E -o 'dot_net_agent/latest_release/newrelic-dotnet-agent_[[:digit:]]{1,3}(\.[[:digit:]]{1,3}){2}_amd64\.tar\.gz') \
&& echo "Downloading: $NEW_RELIC_DOWNLOAD_URI into $(pwd)" \
&& wget -O - "$NEW_RELIC_DOWNLOAD_URI" | gzip -dc | tar xf - \
&& apk add --no-cache icu-data-full icu-libs

ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false
