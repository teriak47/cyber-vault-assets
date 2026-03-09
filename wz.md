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

la sécurité dans observateur d'événements
****************************************************
Télécharger le tableau avec les codes par exemple de création d'user
Télécharger le logiciel Event Log Explorer Standard Edition (qui fait le même que dans l'observateur d'événements

On a fait plusieurs filtres 4625 - 4624, etc … 

** A comparer avec la photo codes.png **

Question examen ex: sur base d'un fichier d'events, pourquoi l'utilisateur a eu sa connexion échouée, qui, ect …

Test aussi avec une commande powershell exécutée, il faut se rendre dans la section powershell de l'observeur d'événements.

Stratgie de l'audit (dans paramètres de sécurtité) --> afin de configurer ce qui est à "écouter", les réussites, les échecs, etc ...
commande windows pour accéder à la stratégie --> secpol

Dans ce porgramme tout est "à auditer", il est aussi possible d'avoir des recommandations de ce qui est à activer
--> https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations?tabs=winclient

Résumé
******


1. Visibilité --> secpol.msc

2. Trouver les logs -- c:\Windows\System32\winvt\Logs

3. Comprendre les logs
Logs Importants --> Connexions, Process création, Logs Powershell

4. Automatisation
Il existe des scripts qui permettent d'automatiser ce travail, au lieu d'analyser TOUT
HAYABUSA ou DEEPBLUECLI

- https://github.com/Yamato-Security/hayabusa
- https://github.com/sans-blue-team/DeepBlueCLI
- https://github.com/WithSecureLabs/chainsaw

4.1 HAYABUSA

Télécharger le zip selon la version du windows
Décomprésser
Dans un Fchier (LOGSDIR), coller le dossier de Logs à récupérer dans c:\windows\System32\winevt\Logs
avec un terminal .\hayabusa-3.7.0-win-x64.exe csv-timeline --directory NOMDUDOSSIER\Logs -o test.csv
ou test.csv est le fichier de sortie

Répondre au questions après avoir lancé la commande

4.2 DEEPBLUECLI

** AVEC POWERSHELL, si erreur --> Set-ExecutionPolicy RemoteSigned**

Télécharger le zip du projet sur git
dans le dossier evtx, copier/coller tous les logs --> c:\windows\System32\winevt\Logs

.\DeepBlue.ps1 .\evtx\Security.evtx | Out-GridView

4.3 CHAINSAW

AVEC POWERSHELL


.\chainsaw_x86_64-pc-windows-msvc.exe hunt XXXXX\Logs  -s .\sigma\ --mapping mappings/sigma-event-logs-all.yml -r rules/ -o resultat
 
.\chainsaw_x86_64-pc-windows-msvc.exe search whoami XXXXXX\Logs
