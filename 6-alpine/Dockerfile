FROM mcr.microsoft.com/dotnet/aspnet:6.0-alpine@sha256:02a9f6b3d99095659228667f3c88aecf4fe331935b7b861a6c17f130b3cd97a1

# Defaults New Relic
ENV CORECLR_NEWRELIC_HOME	/usr/local/newrelic-dotnet-agent
ENV CORECLR_PROFILER	{36032161-FFC0-4B61-B559-F6C5D41BAE5A}
ENV CORECLR_PROFILER_PATH	/usr/local/newrelic-dotnet-agent/libNewRelicProfiler.so
ENV NEW_RELIC_APPLICATION_LOGGING_ENABLED	false
ENV NEW_RELIC_APPLICATION_LOGGING_FORWARDING_ENABLED	false
ENV NEW_RELIC_DISTRIBUTED_TRACING_ENABLED	true


# These variables need to be set in the task definition
# NEW_RELIC_APP_NAME	<service name>
# NEW_RELIC_LICENSE_KEY	<license key for dev / prod>

# install the agent
RUN  mkdir /usr/local/newrelic-dotnet-agent \
&& cd /usr/local \
&& export NEW_RELIC_DOWNLOAD_URI=https://download.newrelic.com/$(wget -qO - "https://nr-downloads-main.s3.amazonaws.com/?delimiter=/&prefix=dot_net_agent/latest_release/newrelic-dotnet-agent" | grep -E -o 'dot_net_agent/latest_release/newrelic-dotnet-agent_[[:digit:]]{1,3}(\.[[:digit:]]{1,3}){2}_amd64\.tar\.gz') \
&& echo "Downloading: $NEW_RELIC_DOWNLOAD_URI into $(pwd)" \
&& wget -O - "$NEW_RELIC_DOWNLOAD_URI" | gzip -dc | tar xf -

# This variable enables New Relic (disabled by default)
ENV CORECLR_ENABLE_PROFILING 0
