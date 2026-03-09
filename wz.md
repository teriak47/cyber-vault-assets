Sur les machines virtuelles Windows 10 et Windows Server

--> En PowerShell admin -> Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.1-1.msi
-OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='10.1.82.10'

Une fois le téléchargement fait, taper la commande NET START Wazuh

Sur les machines virtuelles Ubuntu et Ubuntu Serveur

--> wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.1-1_amd64.deb

--> sudo WAZUH_MANAGER='10.1.82.10' dpkg -i ./wazuh-agent_4.14.1-1_amd64.deb

--> sudo systemctl daemon-reload

--> sudo systemctl enable wazuh-agent.service

--> sudo systemctl start wazuh-agent
