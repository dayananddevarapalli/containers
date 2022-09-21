# Copyright (c) 2022, Dayanand Devarapalli
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

FROM databricksruntime/python-virtualenv:latest

RUN apt-get update \
  && apt-get install -y \
        curl \
	wget \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN wget https://github.com/tsl0922/ttyd/releases/download/1.6.3/ttyd.x86_64 && \
   mkdir -p /databricks/driver/logs && \
   mkdir -p /databricks/spark/scripts/ttyd/ && \
   mkdir -p /etc/monit/conf.d/ && \
   mv ttyd.x86_64 /databricks/spark/scripts/ttyd/ttyd && \
   export TTYD_BIN_FILE=/databricks/spark/scripts/ttyd/ttyd


ENV TTYD_DIR=/databricks/spark/scripts/ttyd
ENV TTYD_BIN_FILE=$TTYD_DIR/ttyd
   
COPY scripts/setup_ttyd_daemon.sh $TTYD_DIR/setup_ttyd_daemon.sh
COPY scripts/stop_ttyd_daemon.sh $TTYD_DIR/stop_ttyd_daemon.sh
COPY scripts/start_ttyd_daemon.sh $TTYD_DIR/start_ttyd_daemon.sh
COPY scripts/webTerminalBashrc $TTYD_DIR/webTerminalBashrc
COPY scripts/monit /etc/init.d/monit

RUN mkdir -p /etc/monit/conf.d/ 
COPY scripts/ttyd-daemon-not-active /etc/monit/conf.d/ttyd-daemon-not-active

