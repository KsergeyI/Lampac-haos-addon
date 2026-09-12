FROM ghcr.io/lampac-nextgen/lampac:latest

USER root

ARG TARGETARCH

RUN case "$TARGETARCH" in \
      amd64) JQ_ARCH="amd64" ;; \
      arm64) JQ_ARCH="arm64" ;; \
      *) echo "Unsupported architecture: $TARGETARCH" && exit 1 ;; \
    esac \
    && curl -fSL \
       "https://github.com/jqlang/jq/releases/download/jq-1.8.2/jq-linux-${JQ_ARCH}" \
       -o /usr/local/bin/jq \
    && chmod +x /usr/local/bin/jq \
    && /usr/local/bin/jq --version

WORKDIR /lampac

COPY run.sh /run.sh

RUN chmod +x /run.sh \
    && (cp init.conf init.conf.default 2>/dev/null || true) \
    && (cp passwd passwd.default 2>/dev/null || true)

ENTRYPOINT ["/run.sh"]
CMD ["dotnet", "Core.dll"]
