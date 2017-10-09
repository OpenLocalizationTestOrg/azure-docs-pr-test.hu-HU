1. Másolja a hello telepítő tooa helyi mappát (például C:\Temp), amelyet az tooprotect hello kiszolgálón. Futtassa a következő parancsok meg egy parancssori ablakot rendszergazdaként hello:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. tooinstall mobilitási szolgáltatást, futtassa a következő parancs hello:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Most hello ügynök toobe hello konfigurációs kiszolgáló regisztrálva van szüksége.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Parancssori argumentumok a mobilitási szolgáltatás telepítő

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Paraméter|Típus|Leírás|Lehetséges értékek|
|-|-|-|-|
|/ Szerepkör|Kötelező|Megadja, hogy kell telepíteni a mobilitási szolgáltatás (MS), vagy MasterTarget(MT) kell telepíteni.|MS </br> FŐ CÉLKISZOLGÁLÓ|
|/InstallLocation|Optional|Hely, ahol telepítve van|Egyetlen mappa hello számítógépen|
|/ Platform|Kötelező|Meghatározza, melyik hello a mobilitási szolgáltatás telepítve van első hello platform </br> </br>- **VMware** : használja ezt az értéket, ha egy virtuális gépen futó mobilitási szolgáltatás telepít *VMware vSphere ESXi-gazdagépek*, *Hyper-V-gazdagépek* és *Phsyical kiszolgálók* </br> - **Azure** : használja ezt az értéket, ha telepíti az ügynököt egy Azure IaaS virtuális Gépen| VMware </br> Azure|
|/ Beavatkozás nélküli|Optional|Adja meg a toorun hello telepítő csendes módban| NA|

>[!TIP]
> hello telepítési naplókban találhatók %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log

#### <a name="mobility-service-registration-command-line-arguments"></a>Parancssori argumentumok a mobilitási szolgáltatás regisztrációs

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Paraméter|Típus|Leírás|Lehetséges értékek|
  |-|-|-|-|
  |/ CSEndPoint |Kötelező|Hello konfigurációs kiszolgáló IP-címe| Bármilyen érvényes IP-cím|
  |/PassphraseFilePath|Kötelező|Hello jelszót helye |Bármilyen érvényes UNC vagy helyi elérési útja|


>[!TIP]
> hello AgentConfiguration naplók %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log alatt találhatók
