FROM erlang:23.3.4.9

# elixir expects utf8.
ENV ELIXIR_VERSION="v1.12.3" \
	LANG=C.UTF-8 \
	PROTOC_VERSION=3.15.8

ARG UID=1000

RUN useradd -d /home/iex_user -ms /bin/bash -g root -G sudo -u $UID iex_user

RUN set -xe \
	&& PB_REL="https://github.com/protocolbuffers/protobuf/releases" \
	&& curl -fSL -o ./protoc.zip "$PB_REL/download/v3.15.8/protoc-${PROTOC_VERSION}-linux-x86_64.zip" \
	&& unzip ./protoc.zip -d /usr \
	&& rm protoc.zip \
	&& ELIXIR_DOWNLOAD_URL="https://github.com/elixir-lang/elixir/archive/${ELIXIR_VERSION}.tar.gz" \
	&& ELIXIR_DOWNLOAD_SHA256="c5affa97defafa1fd89c81656464d61da8f76ccfec2ea80c8a528decd5cb04ad" \
	&& curl -fSL -o elixir-src.tar.gz $ELIXIR_DOWNLOAD_URL \
	&& echo "$ELIXIR_DOWNLOAD_SHA256  elixir-src.tar.gz" | sha256sum -c - \
	&& mkdir -p /usr/local/src/elixir \
	&& tar -xzC /usr/local/src/elixir --strip-components=1 -f elixir-src.tar.gz \
	&& rm elixir-src.tar.gz \
	&& cd /usr/local/src/elixir \
	&& make install clean

USER iex_user

RUN set -xe \
	&& mix local.hex --force \
   	&& mix local.rebar --force \
   	&& mix escript.install hex protobuf --force
	
CMD ["/bin/sleep", "3600"]

