1. Másolja hello telepítő tooa helyi mappát (például /tmp), amelyet az tooprotect hello kiszolgálón. Egy terminált futtassa a következő parancsok hello:
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. tooinstall mobilitási szolgáltatást, futtassa a következő parancs hello:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Telepítés befejezése után hello Mobilitásiszolgáltatás kell tooget regisztrált toohello konfigurációs kiszolgáló. Futtassa a következő parancs tooregister hello mobilitási szolgáltatás konfigurációs kiszolgáló hello.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Mobilitási szolgáltatások telepítőjének parancssori

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Paraméter|Típus|Leírás|Lehetséges értékek|
|-|-|-|-|
|-r |Kötelező|Megadja, hogy kell telepíteni a mobilitási szolgáltatás (MS), vagy MasterTarget(MT) kell telepíteni.|MS </br> FŐ CÉLKISZOLGÁLÓ|
|-d |Optional|Hely, ahol a mobilitási szolgáltatás telepítve lesz|/usr/local/ASR|
|-v|Kötelező|Meghatározza, melyik hello a mobilitási szolgáltatás telepítve van első hello platform </br> </br>- **VMware** : használja ezt az értéket, ha egy virtuális gépen futó mobilitási szolgáltatás telepít *VMware vSphere ESXi-gazdagépek*, *Hyper-V-gazdagépek* és *Phsyical kiszolgálók* </br> - **Azure** : használja ezt az értéket, ha telepíti az ügynököt egy Azure IaaS virtuális Gépen| VMware </br> Azure|
|-k|Optional|Adja meg a toorun telepítő csendes módban| N/A|


#### <a name="mobility-service-configuration-command-line"></a>Parancssori mobilitási szolgáltatás konfigurációja

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Paraméter|Típus|Leírás|Lehetséges értékek|
|-|-|-|-|
|-i |Kötelező|IP-címe hello konfigurációs kiszolgálón|Bármilyen érvényes IP-cím|
|-P |Kötelező|Hello kapcsolat jelszava mentett tartalmazó fájl teljes elérési útja hello fájl|Bármilyen érvényes mappa|
