FROM edxops/precise-common:latest
MAINTAINER edxops

ENV CONFIGURATION_REPO="https://github.com/edx/configuration.git"
ENV CONFIGURATION_VERSION="hack2015/docker"

USER docker

RUN sudo apt-get update

WORKDIR /edx/app/edx_ansible/edx_ansible
RUN sudo git fetch --all
RUN sudo git checkout ${CONFIGURATION_VERSION}
RUN sudo git reset --hard origin/${CONFIGURATION_VERSION}
RUN sudo git pull

WORKDIR /edx/app/edx_ansible/edx_ansible/docker/plays
COPY ansible_overrides.yml /
# Remove when branch is pushed
COPY insights-temp.yml /edx/app/edx_ansible/edx_ansible/docker/plays/insights.yml
RUN sudo ansible-playbook insights.yml -i '127.0.0.1,' -c local -t "install:base,install:system-requirements,install:configuration,install:app-requirements,install:code" -e@/ansible_overrides.yml

CMD ["/edx/app/supervisor/venvs/supervisor/bin/supervisord -n --configuration /edx/app/supervisor/supervisord.conf"]
EXPOSE 8110 18110
