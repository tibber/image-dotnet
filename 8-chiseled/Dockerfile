FROM mcr.microsoft.com/dotnet/aspnet:8.0@sha256:9442a1ae01c8f5b2fe3c5cc153c731af4187e091b86391db92601252799afc11 AS build

RUN apt-get update && apt-get install -y wget curl ca-certificates gnupg \
  && echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
  && wget https://download.newrelic.com/548C16BF.gpg \
  && apt-key add 548C16BF.gpg \
  && apt-get update \
  && apt-get install -y newrelic-dotnet-agent

FROM mcr.microsoft.com/dotnet/aspnet:8.0-jammy-chiseled-extra@sha256:69c85cf25a133a45aa6d0d7faebbaee3acedd92b7eeef63fe83492baacfe2336 AS final

ENV CORECLR_NEWRELIC_HOME /usr/local/newrelic-dotnet-agent
ENV CORECLR_PROFILER {36032161-FFC0-4B61-B559-F6C5D41BAE5A}
ENV CORECLR_PROFILER_PATH /usr/local/newrelic-dotnet-agent/libNewRelicProfiler.so
ENV NEW_RELIC_APPLICATION_LOGGING_ENABLED false
ENV NEW_RELIC_APPLICATION_LOGGING_FORWARDING_ENABLED false
ENV NEW_RELIC_DISTRIBUTED_TRACING_ENABLED true
# This variable enables New Relic (disabled by default)
ENV CORECLR_ENABLE_PROFILING 0

COPY --from=build /usr/local/newrelic-dotnet-agent /usr/local/newrelic-dotnet-agent
