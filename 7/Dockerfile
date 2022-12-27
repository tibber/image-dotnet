FROM mcr.microsoft.com/dotnet/aspnet:7.0

# Disable core clr profiling by default due to new relic performance issues
ENV CORECLR_ENABLE_PROFILING 0

RUN apt-get update && apt-get install -y wget curl ca-certificates gnupg \
  && echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
  && wget https://download.newrelic.com/548C16BF.gpg \
  && apt-key add 548C16BF.gpg \
  && apt-get update \
  && apt-get install -y newrelic-netcore20-agent
