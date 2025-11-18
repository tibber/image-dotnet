FROM mcr.microsoft.com/dotnet/aspnet:9.0@sha256:925a86ec775fe737f1d65371ad7b2e01e81390c3c327ce3c06866a8ce8a4e1a2 AS build

RUN apt-get update && apt-get install -y wget curl ca-certificates gnupg \
  && echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
  && wget https://download.newrelic.com/548C16BF.gpg \
  && apt-key add 548C16BF.gpg \
  && apt-get update \
  && apt-get install -y newrelic-dotnet-agent

# Create a named pipes to send and ack the signal to stop the service
RUN mkdir /app 
RUN mkfifo /app/prestop.pipe && chown 65534:65534 /app/prestop.pipe
RUN mkfifo /app/prestop-ack.pipe && chown 65534:65534 /app/prestop-ack.pipe

# Download tool for sending a signal to the named pipe
RUN set -e && \
  arch="$(uname -m)" && \
  case "$arch" in \
  x86_64) arch_tag="amd64" ;; \
  aarch64) arch_tag="arm64" ;; \
  esac && \
  curl -sSL "https://github.com/tibber/prestop-signal/releases/download/v0.0.18/prestop-signal_0.0.18_linux_${arch_tag}.tar.gz" \
  | tar -xz -C /usr/bin

FROM mcr.microsoft.com/dotnet/aspnet:9.0-noble-chiseled-extra@sha256:4bdd950e53eb04dc20eee68e36c91a1cda1426c128272f488e1fe9acb166581e AS final

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

# Copy named pipes for sending and acking a signal to stop the service
COPY --from=build /app/prestop.pipe /app/
COPY --from=build /app/prestop-ack.pipe /app/

# Copy command for sending signal to the named pipe
COPY --from=build /usr/bin/prestop-signal /usr/bin/
