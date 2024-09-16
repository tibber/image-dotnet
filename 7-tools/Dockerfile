# Use the SDK image to build and install tools
FROM mcr.microsoft.com/dotnet/sdk:7.0@sha256:d32bd65cf5843f413e81f5d917057c82da99737cb1637e905a1a4bc2e7ec6c8d AS build
RUN dotnet tool install --global dotnet-trace
RUN dotnet tool install --global dotnet-counters
RUN dotnet tool install --global dotnet-dump
RUN dotnet tool install --global dotnet-gcmon
RUN dotnet tool install --global dotnet-stack

FROM mcr.microsoft.com/dotnet/aspnet:7.0@sha256:c7d9ee6cd01afe9aa80642e577c7cec9f5d87f88e5d70bd36fd61072079bc55b
# Copy installed tools from the build image. This increases image size from 162MB to 269MB <<<<------------------ CHECK THIS
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

RUN apt-get update && apt-get install -y wget curl ca-certificates gnupg \
  && echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
  && wget https://download.newrelic.com/548C16BF.gpg \
  && apt-key add 548C16BF.gpg \
  && apt-get update \
  && apt-get install -y newrelic-dotnet-agent
