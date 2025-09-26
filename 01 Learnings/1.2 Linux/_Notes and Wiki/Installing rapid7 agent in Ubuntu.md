---
tags:
  - rapid7
  - installation
  - SOP
  - linux
---
---

#### Commands
1. Install the agent using `dpkg`
```sh 
sudo dpkg -i rapid7-insight-agent_4.0.17.21-1_amd64.deb
```

2. Set the token
```sh 
cd /opt/rapid7/ir_agent/components/insight_agents/4.0.17.21
sudo ./configure_agent.sh --token <Your-Token> -v --start
```

3. Start and enable the service
```sh
sudo systemctl enable ir_agent
sudo systemctl start ir_agent
```

4. Check the agent status
```sh
sudo systemctl status ir_agent
```

#### Script
![[Installing rapid7 agent on Ubuntu | Script to install R7 agent in Ubuntu]]


