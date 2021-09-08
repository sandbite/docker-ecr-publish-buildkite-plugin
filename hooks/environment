# install docker buildx globally
DOCKER_DIR=/usr/libexec/docker
## get latest version or pin it to v0.4.2
BUILDX_VERSION=$(curl --silent "https://api.github.com/repos/docker/buildx/releases/latest" | jq -r '.tag_name')
mkdir -p $DOCKER_DIR/cli-plugins

## check architecture
UNAME_ARCH=`uname -m`
case $UNAME_ARCH in
  aarch64)
    BUILDX_ARCH="arm64";
    ;;
  amd64)
    BUILDX_ARCH="amd64";
    ;;
  *)
    BUILDX_ARCH="amd64";
    ;;
esac

wget \
  -O $DOCKER_DIR/cli-plugins/docker-buildx \
  -nv https://github.com/docker/buildx/releases/download/$BUILDX_VERSION/buildx-$BUILDX_VERSION.linux-$BUILDX_ARCH
chmod a+x $DOCKER_DIR/cli-plugins/docker-buildx

## Install in the buildkite-agent user
# install arm
runuser -l buildkite-agent -c 'docker run --privileged --rm tonistiigi/binfmt --install all'
# create docker builder
runuser -l buildkite-agent -c 'docker buildx create --platform linux/amd64,linux/arm64,linux/arm/v7 --name mybuilder --use'
runuser -l buildkite-agent -c 'docker buildx inspect --bootstrap'
