```sh
#!/usr/bin/env bash

if [ -z $1 ]; then
	echo "Usage: $0 <Token>"
	exit 1
fi

AGENT_LOCATION=/tmp
cd $AGENT_LOCATION

AGENT_FILE_NAME='rapid7-insight-agent_4.0.17.21-1_amd64.deb'

touch /var/log/r7logfile.txt

if [ ! -f "$AGENT_FILE_NAME" ]; then
    echo "The Agent file is missing"
    exit 1
fi

dpkg -i $AGENT_FILE_NAME > /var/log/r7logfile.txt 2>&1


if [ $? -ne 0 ]; then
    echo "Unable to install R7 agent due to following error"
    cat /var/log/r7logfile.txt
    exit 1
fi

cd /opt/rapid7/ir_agent/components/insight_agents || exit 1

VERSION_DIR=$(ls -d */ | head -n 1 | cut -d'/' -f1)

cd "$VERSION_DIR"

CONFIG_FILE_NAME="configure_agent.sh"

if [ ! -f "$CONFIG_FILE_NAME" ]; then
    echo "Config file is missing"
    exit 1
fi

./$CONFIG_FILE_NAME --token $1 -v --start

if [ $? -ne 0 ]; then
    echo "Unable to set the token"
    exit 1
fi

systemctl start ir_agent
systemctl enable ir_agent
systemctl status ir_agent

```