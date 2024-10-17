FROM mcr.microsoft.com/dotnet/aspnet:8.0@sha256:3ded9ccf06f222ec934311be4f9facda83d144331c028340e3a694733cad7d4b

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
