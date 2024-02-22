FROM gcr.io/cloud-builders/gcloud

ARG HELM_VERSION
ARG HELMFILE_VERSION

COPY helmfile.bash /builder/helmfile.bash

# install curl
RUN apt-get update && \
  apt-get install -y curl && \
  apt-get --purge -y autoremove && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

# add soon to be created helmfile dir to PATH
ENV PATH=/builder/helmfile/:$PATH

# install helm, helmfile and helm-diff plugin
RUN chmod +x /builder/helmfile.bash && \
  mkdir -p /builder/helmfile && \
  curl -SL https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz -o helm.tar.gz && \
  tar zxvf helm.tar.gz --strip-components=1 -C /builder/helmfile linux-amd64 && \
  rm helm.tar.gz && \
  curl -SL https://github.com/helmfile/helmfile/releases/download/${HELMFILE_VERSION}/helmfile_$(echo ${HELMFILE_VERSION} | sed 's/^v//')_linux_amd64.tar.gz -o helmfile.tar.gz && \
  tar zxvf helmfile.tar.gz -C /builder/helmfile && \
  rm helmfile.tar.gz && \
  chmod 700 /builder/helmfile/helmfile && \
  helm plugin install https://github.com/databus23/helm-diff

# install gcloud-auth-plugin
RUN gcloud components install gke-gcloud-auth-plugin

ENTRYPOINT ["/builder/helmfile.bash"]
