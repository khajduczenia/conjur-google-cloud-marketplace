FROM launcher.gcr.io/google/debian9

RUN apt-get update && apt-get install -y --no-install-recommends \
    bash \
    ca-certificates \
    gettext \
    jq \
    python \
    python-pip \
    python-setuptools \
    python-yaml \
    wget \
    && rm -rf /var/lib/apt/lists/*

RUN pip install wheel \
    && pip install google-cloud-storage

RUN wget -q -O /bin/kubectl \
        https://storage.googleapis.com/kubernetes-release/release/v1.9.0/bin/linux/amd64/kubectl \
    && chmod 755 /bin/kubectl

COPY marketplace-k8s-app-tools/marketplace/deployer_envsubst_base/* /bin/
COPY marketplace-k8s-app-tools/marketplace/deployer_util/* /bin/

RUN mkdir -p /data/manifest

COPY manifest /data/manifest
COPY apptest/deployer /data-test/

COPY schema.yaml /data

# Provide registry prefix and tag for default values for images.
ARG REGISTRY
ARG TAG
RUN cat /data/schema.yaml \
    | env -i "REGISTRY=$REGISTRY" "TAG=$TAG" envsubst \
    > /data/schema.yaml.new \
    && mv /data/schema.yaml.new /data/schema.yaml
RUN cat /data-test/schema.yaml \
    | env -i "REGISTRY=$REGISTRY" "TAG=$TAG" envsubst \
    > /data-test/schema.yaml.new \
    && mv /data-test/schema.yaml.new /data-test/schema.yaml

ENTRYPOINT ["/bin/bash", "/bin/deploy.sh"]
