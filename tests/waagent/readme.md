# Vergleich VM mit und ohne WA Agent
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
> Link zu Microsoft https://github.com/Azure/WALinuxAgent/wiki/VMs-without-WALinuxAgent
## Zusammenfassung:
-	der Hoststatus wird bei beiden VMs richtig erkannt
unhealthy wenn VM mit „halt“ gestoppt wurde
-	Beide VMs konnten über das Portal neugestartet werden und waren wieder erreichbar
-	Metriken werden bei beiden VMs angezeigt (Host, Disk, Network usw.)
-	Remote Commandos funktionieren an VM ohne Agent nicht
-	Passwort und SSH Keys konnten nur bei der VM mit Agent durchgeführt werden
- die Seriele Console funktioniert bei beiden VMs und auch darüber ist eine Anmeldung möglich
 
## Ergebnisse im Detail
### Hostmetriken verfügbar
VM mit Agent
![Image](https://github.com/ckoehler99/Azure/blob/master/tests/waagent/images/vmwith-hostmetrik.png)
VM ohne Agent
![Image](https://github.com/ckoehler99/Azure/blob/master/tests/waagent/images/vmwithout-hostmetrik.png)
### Health Status
VM mit Agent
![Image](https://github.com/ckoehler99/Azure/blob/master/tests/waagent/images/vmwith-healthstatus.png)
VM ohne Agent
Nach „sudo halt“
![Image](https://github.com/ckoehler99/Azure/blob/master/tests/waagent/images/vmwithout-healthstatus.png)
Nach Restart über Portal:
![Image](https://github.com/ckoehler99/Azure/blob/master/tests/waagent/images/vmwithout-healthstatus2.png)
### Passwort reset
VM mit Agent
![Image](https://github.com/ckoehler99/Azure/blob/master/tests/waagent/images/vmwith-password.png)
VM ohne Agent
![Image](https://github.com/ckoehler99/Azure/blob/master/tests/waagent/images/vmwithout-password.png)
