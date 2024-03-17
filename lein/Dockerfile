FROM ghcr.io/dependabot/dependabot-updater-core

USER root
RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y --no-install-recommends \
    openssh-client \
  && rm -rf /var/lib/apt/lists/*

USER root
# Install leiningen
RUN apt-get update \
  && apt-get install -y openjdk-8-jre-headless \
  && java -version \
  && curl https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein > lein \
  && mkdir -p /usr/local/lein/bin \
  && mv lein /usr/local/lein/bin \
  && chmod +x /usr/local/lein/bin/lein \
  && /usr/local/lein/bin/lein --version

ENV PATH="$PATH:/usr/local/lein/bin" \
  LEIN_SNAPSHOTS_IN_RELEASE="yes"

ENV DEPENDABOT_NATIVE_HELPERS_PATH="/opt"
COPY --chown=dependabot:dependabot lein/helpers /opt/lein/helpers
RUN bash /opt/lein/helpers/build /opt/lein

USER dependabot
COPY --chown=dependabot:dependabot lein $DEPENDABOT_HOME/lein
COPY --chown=dependabot:dependabot common $DEPENDABOT_HOME/common
COPY --chown=dependabot:dependabot updater $DEPENDABOT_HOME/dependabot-updater
