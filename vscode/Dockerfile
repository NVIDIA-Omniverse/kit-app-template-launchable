# SPDX-FileCopyrightText: Copyright (c) 2025 NVIDIA CORPORATION & AFFILIATES. All rights reserved.
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
FROM nvcr.io/nvidia/omniverse/ov-base-ubuntu-22:2025.2.0

USER root

RUN cp /etc/apt/sources.list.canonical /etc/apt/sources.list.d/canonical.list
RUN rm /root/.pip/pip.conf

RUN apt update && \
    apt install -y \
	--no-install-recommends \
	build-essential \
	cmake \
	curl \
	git \
	python3 \
	python3-pip

RUN curl -fsSL https://code-server.dev/install.sh | sh -s -- --version=4.96.4

RUN apt update && \
	apt install -y \
	--no-install-recommends \
	unzip \
	dnsutils \
	iproute2 \
	jq

USER ubuntu

WORKDIR /home/ubuntu/workspace

ARG KIT_APP_TEMPLATE_BRANCH=108.0
RUN git clone -b ${KIT_APP_TEMPLATE_BRANCH} https://github.com/NVIDIA-Omniverse/kit-app-template.git

COPY ./README.md ${WORKSPACE_DIR}/README.md
COPY ./settings.json ${WORKSPACE_DIR}/.vscode/settings.json

RUN code-server \
    --install-extension ms-python.python \
    --install-extension ms-toolsai.jupyter \
    --force ${WORKSPACE_DIR}

COPY entrypoint.sh /entrypoint.sh

# OV Cache Directories
RUN mkdir -p /home/ubuntu/.cache/ov
RUN mkdir -p /home/ubuntu/.local/share/ov
RUN chown ubuntu:ubuntu /home/ubuntu/.cache/ov
RUN chown ubuntu:ubuntu /home/ubuntu/.local/share/ov


ENTRYPOINT ["/bin/bash"]

CMD ["/entrypoint.sh"]
