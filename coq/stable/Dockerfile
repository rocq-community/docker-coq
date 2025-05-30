ARG BASE_TAG="latest"
FROM rocq/base:${BASE_TAG}

# The following variable should be nonempty for coq >= 8.17
ARG COQ_CORE_PINNED

ARG COQ_EXTRA_OPAM="coq-bignums"
ENV COQ_EXTRA_OPAM="${COQ_EXTRA_OPAM}"

# The following variable should be empty for ocaml < 4.06 or coq <= 8.7
ARG COQ_INSTALL_SERAPI

ARG COQ_VERSION="dev"
ENV COQ_VERSION=${COQ_VERSION}

# This line is actually unneeded (was already enabled in rocq/base)
SHELL ["/bin/bash", "--login", "-o", "pipefail", "-c"]

# hadolint ignore=SC2046
RUN set -x \
  && eval $(opam env "--switch=${COMPILER}" --set-switch) \
  && opam update -y -u \
  && opam pin add -n -k version coq ${COQ_VERSION} \
  && if [ -n "${COQ_CORE_PINNED}" ]; then \
     opam pin add -n -k version coq-core ${COQ_VERSION} && \
     opam pin add -n -k version coq-stdlib ${COQ_VERSION} && \
     opam pin add -n -k version coqide-server ${COQ_VERSION}; fi \
  && opam install -y -v -j "${NJOBS}" coq ${COQ_EXTRA_OPAM} ${COQ_INSTALL_SERAPI:+coq-serapi} \
  && opam clean -a -c -s --logs \
  && chmod -R g=u /home/coq/.opam \
  && opam config list && opam list

ARG BUILD_DATE
ARG VCS_REF
LABEL org.label-schema.build-date=${BUILD_DATE} \
  org.label-schema.name="The Coq Proof Assistant" \
  org.label-schema.description="Coq is a formal proof management system. It provides a formal language to write mathematical definitions, executable algorithms and theorems together with an environment for semi-interactive development of machine-checked proofs." \
  org.label-schema.url="https://coq.inria.fr/" \
  org.label-schema.vcs-ref=${VCS_REF} \
  org.label-schema.vcs-url="https://github.com/coq/coq" \
  org.label-schema.vendor="The Coq Development Team" \
  org.label-schema.version=${COQ_VERSION} \
  org.label-schema.schema-version="1.0" \
  maintainer="erik@martin-dorel.org"
