FROM ruby:3.3.4-slim

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# The ruby image comes with a base non-root 'ruby' user which this Dockerfile
# gives sudo access. Hoewver, for Linux, this user's GID/UID must match your local
# user UID/GID to avoid permission issues with bind mounts. Update USER_UID / USER_GID
# if yours is not 1000. See https://aka.ms/vscode-remote/containers/non-root-user.
ARG USERNAME=ruby
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Configure apt and install packages
RUN apt-get update
RUN apt-get -y install --no-install-recommends apt-utils build-essential dialog 2>&1

# Verify git and needed tools are installed
RUN apt-get install -y git procps
RUN apt-get update

# Create a non-root user to use if preferred - see https://aka.ms/vscode-remote/containers/non-root-user.
RUN groupadd --gid $USER_GID $USERNAME
RUN useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

# [Optional] Add sudo support for non-root users
RUN apt-get install -y sudo
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME
RUN chmod 0440 /etc/sudoers.d/$USERNAME

# Clean up
RUN apt-get autoremove -y
RUN apt-get clean -y
RUN rm -rf /var/lib/apt/lists/*

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=

ENV HOME /home/$USERNAME
