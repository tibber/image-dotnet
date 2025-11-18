FROM mcr.microsoft.com/dotnet/aspnet:8.0@sha256:47091f7cee02e448630df85542579e09b7bbe3b10bd4e1991ff59d3adbddd720 AS build

RUN apt-get update && apt-get install -y wget curl ca-certificates gnupg \
  && echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
  && wget https://download.newrelic.com/548C16BF.gpg \
  && apt-key add 548C16BF.gpg \
  && apt-get update \
  && apt-get install -y newrelic-dotnet-agent

FROM mcr.microsoft.com/dotnet/aspnet:8.0-jammy-chiseled-extra@sha256:58a5599f59104d6a7e32800850f6417fcb382e2c32b1e70df822c99298659a0c AS final

ENV CORECLR_NEWRELIC_HOME /usr/local/newrelic-dotnet-agent
ENV CORECLR_PROFILER {36032161-FFC0-4B61-B559-F6C5D41BAE5A}
ENV CORECLR_PROFILER_PATH /usr/local/newrelic-dotnet-agent/libNewRelicProfiler.so
ENV NEW_RELIC_APPLICATION_LOGGING_ENABLED false
ENV NEW_RELIC_APPLICATION_LOGGING_FORWARDING_ENABLED false
ENV NEW_RELIC_DISTRIBUTED_TRACING_ENABLED true
# This variable enables New Relic (disabled by default)
ENV CORECLR_ENABLE_PROFILING 0

COPY --from=build /usr/local/newrelic-dotnet-agent/ /usr/local/newrelic-dotnet-agent/
COPY --from=build /usr/bin/sleep /usr/bin/
