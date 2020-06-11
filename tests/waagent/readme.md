### Vergleich VM mit und ohne WA Agent
## Testdurchführung
-	2 VMs über das gleiche Template installiert
-	Im Template Variable über provisionVMAgent True/False die Installation beeinflusst
```
"osProfile": {
                    "computerName": "[parameters('virtualMachineComputerName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "linuxConfiguration": {
                        "disablePasswordAuthentication": true,
                        "provisionVMAgent": "[parameters('provisionVMAgent')]"
```
-	bei der VM ohne WA Agent habe ich anschließend noch alle Dienste deaktiviert und komplett deinstalliert
```
sudo systemctl list-unit-files | grep agent
sudo systemctl disable waagent
sudo yum remove WALinuxAgent
```
## Zusammenfassung:
-	der Hoststatus wird bei beiden VMs richtig erkannt
unhealthy wenn VM mit „halt“ gestoppt wurde
-	Beide VMs konnten über das Portal neugestartet werden und waren wieder erreichbar
-	Metriken werden bei beiden VMs angezeigt (Host, Disk, Network usw.)
-	Remote Commandos funktionieren an VM ohne Agent nicht
-	Passwort und SSH Keys konnten nur bei der VM mit Agent durchgeführt werden
 


