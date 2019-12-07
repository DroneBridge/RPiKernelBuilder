FROM debian:jessie

# Install build dependencies
RUN apt-get update && apt-get install -y \
  bc \
  zip \
  bison \
  build-essential \
  curl \
  flex \
  cpio \
  git-core \
  libncurses5-dev \
  libssl-dev \
  module-init-tools

# Install the cross compiler tools
RUN git clone https://github.com/raspberrypi/tools /opt/tools
ENV TOOLS /opt/tools

# Create working directory
RUN mkdir -p /workdir
WORKDIR /workdir
ENV WORKDIR /workdir

# Run the kernel builder from the current directory
CMD ["./build.sh"]
