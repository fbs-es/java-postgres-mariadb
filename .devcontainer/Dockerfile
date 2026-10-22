FROM mcr.microsoft.com/devcontainers/java:1-21

# Install Maven directly from Apache distribution — bypasses SDKMAN API
ARG MAVEN_VERSION=3.9.9
RUN set -eux; \
    curl -fsSL "https://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz" \
        | tar -xz -C /opt; \
    ln -s "/opt/apache-maven-${MAVEN_VERSION}/bin/mvn" /usr/local/bin/mvn

# Install Gradle directly from Gradle distribution — bypasses SDKMAN API
ARG GRADLE_VERSION=9.2.0
RUN set -eux; \
    curl -fsSL "https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" -o /tmp/gradle.zip; \
    unzip -q /tmp/gradle.zip -d /opt; \
    rm /tmp/gradle.zip; \
    ln -s "/opt/gradle-${GRADLE_VERSION}/bin/gradle" /usr/local/bin/gradle

# Install gum (used by run.sh / doc.sh)
ARG GUM_VERSION=2.0.2
RUN set -eux; \
    case "$(uname -m)" in \
        x86_64) GUM_ARCH=x86_64 ;; \
        aarch64) GUM_ARCH=arm64 ;; \
        *) echo "Unsupported architecture" && exit 1 ;; \
    esac; \
    curl -fsSL "https://github.com/charmbracelet/gum/releases/download/v${GUM_VERSION}/gum_${GUM_VERSION}_Linux_${GUM_ARCH}.tar.gz" \
        | tar -xz -C /tmp; \
    mv "/tmp/gum_${GUM_VERSION}_Linux_${GUM_ARCH}/gum" /usr/local/bin/gum; \
    rm -rf "/tmp/gum_${GUM_VERSION}_Linux_${GUM_ARCH}"

# Database command line clients (psql, mariadb)
# The yarn apt source of the base image has an expired key — remove it so apt-get update works
RUN rm -f /etc/apt/sources.list.d/yarn.list \
    && apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends postgresql-client mariadb-client \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
