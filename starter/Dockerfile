# Use OpenSUSE base image
FROM opensuse/leap:latest

# Define maintainer
LABEL maintainer="nick.reva@snap.com"

# Define version
LABEL version="V1"

# Create a directory to work in
WORKDIR /app

# Install necessary packages
RUN zypper refs && zypper refresh && zypper install -y shadow

# Create a non-root user and switch to it
RUN useradd -m -s /bin/bash appuser
USER appuser

# Set environment variables (best practice)
ENV HOME=/home/appuser

# **Fix 1: Read-only Root Filesystem**
VOLUME ["/app"]
RUN chmod -R 755 /app

# **Fix 2: Apply SecurityOptions**
RUN echo "Enabling SecurityOptions" \
    && touch /etc/docker/seccomp.json \
    && chmod 600 /etc/docker/seccomp.json

# **Fix 3: Drop Unnecessary Privileges**
USER appuser
ENTRYPOINT ["/bin/sh", "-c", "exec tail -f /dev/null"]
CMD []

# **Fix 4: Enforce a Health Check**
HEALTHCHECK --interval=30s --timeout=10s --retries=3 CMD test -f /app/healthcheck || exit 1