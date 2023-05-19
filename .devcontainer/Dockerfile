# syntax=docker/dockerfile:1.4

ARG USERNAME=vscode

FROM public.ecr.aws/lambda/python:3.10 as base
ARG USERNAME

FROM base as devcontainer
ARG USERNAME

RUN yum install -y tar gzip git yum-utils make
RUN yum-config-manager --add-repo https://cli.github.com/packages/rpm/gh-cli.repo && \
    yum install -y gh

RUN yum install amazon-linux-extras -y && \
    PYTHON=python2 amazon-linux-extras install docker -y

RUN /usr/sbin/useradd -s /bin/bash -m ${USERNAME} && \
    /usr/sbin/usermod -aG docker ${USERNAME}

USER ${USERNAME}
RUN pip install --upgrade pip
RUN pip install aws-sam-cli
RUN echo "PATH=\$HOME/.local/bin:\$PATH" >> ~/.zshrc
USER root

COPY app.py ${LAMBDA_TASK_ROOT}
CMD [ "app.handler" ]

RUN yum install -y zsh util-linux-user && \
    chsh -s /bin/zsh ${USERNAME}

USER ${USERNAME}

RUN curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
RUN sed -i 's/zmodule asciiship/zmodule gitster\nzmodule prompt-pwd/' ~/.zimrc

USER root


