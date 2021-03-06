# To build this Dockerfile:
#
# From the root of configuration:
#
# docker build -f docker/build/course-discovery/Dockerfile .
#
# This allows the dockerfile to update /edx/app/edx_ansible/edx_ansible
# with the currently checked-out configuration repo.

FROM edxops/trusty-common:hacking
MAINTAINER edxops

ARG COURSE_DISCOVERY_VERSION=master
ARG REPO_OWNER=edx

ADD . /edx/app/edx_ansible/edx_ansible

USER docker
WORKDIR /edx/app/edx_ansible/edx_ansible/docker/plays

COPY docker/build/course-discovery/ansible_overrides.yml /
RUN sudo ansible-playbook course_discovery.yml \
    -c local -i '127.0.0.1,' \
    -t 'install,assets,devstack:install' \
    --extra-vars="@/ansible_overrides.yml" \
    --extra-vars="COURSE_DISCOVERY_VERSION=$COURSE_DISCOVERY_VERSION" \
    --extra-vars="COMMON_GIT_PATH=$REPO_OWNER"

USER root 
CMD ["/edx/app/supervisor/venvs/supervisor/bin/supervisord", "-n", "--configuration", "/edx/app/supervisor/supervisord.conf"]
