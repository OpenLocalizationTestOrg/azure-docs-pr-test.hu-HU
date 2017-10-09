---
title: "Figyelés megoldás az Azure Naplóelemzés aaaContainer |} Microsoft Docs"
description: "hello tároló figyelésére szolgáló megoldás a Log Analyticshez segít megtekintése és kezelése a Docker és a Windows tároló állomások egyetlen helyen megvalósítható."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte;banders
ms.openlocfilehash: 2eed1dd81c22faef78a375fca3ebece9e5300c09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a>A Naplóelemzési tároló figyelés megoldás

![Tárolók szimbólum](./media/log-analytics-containers/containers-symbol.png)

Ez a cikk ismerteti, hogyan tooset fel és -felhasználási hello tároló figyelésére szolgáló megoldás Naplóelemzési, amely segít a Docker és a Windows megtekintéséhez és kezeléséhez a tároló állomások egyetlen helyen megvalósítható. Docker a virtualizációs szoftver rendszer használt toocreate tárolók, amely automatizálja a szoftverfrissítési központi telepítési tootheir informatikai infrastruktúra.

hello megoldást mutat be tárolók futtat, amely futtatja, tároló képet, és a tárolók futtató. Tárolók használt parancsok bemutató részletes naplózási információk is megtekinthetők. És tárolók megtekintésével és központosított naplók keresése anélkül, hogy a Docker tooremotely nézet vagy a Windows gazdagépek is elháríthatók. Tárolók, amelyek esetleg zajos vagy a felhasználó felesleges erőforrások egy gazdagépen található. És megtekintheti a központi Processzor, memória, tárolási és hálózati használati és teljesítményadatokat adatai tárolók. Windows rendszerű számítógépeken központosítása és hasonlítsa össze a napló, a Windows Server Hyper-V és a Docker-tárolók. hello megoldás támogatja a következő tároló orchestrators hello:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift


hello alábbi ábrán látható hello kapcsolatai különböző tároló gazdagépek és az ügynökök az OMS Szolgáltatáshoz.

![Tárolók diagramja](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a>Rendszerkövetelmények

Megkezdése előtt tekintse át a következő részleteket tooverify megfelel a hello Előfeltételek hello.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Figyelési megoldást igényelnek tároló Docker Orchestrator és az operációs rendszer platform támogatása
hello következő tábla vázol fel hello Docker vezénylési és az operációs rendszer figyelési tároló készlet, a teljesítmény és a naplók és a Naplóelemzési támogatást.   

| | ACS | Linux | Windows | Tároló<br>Szoftverleltár | Kép<br>Szoftverleltár | Csomópont<br>Szoftverleltár | Tároló<br>Teljesítmény | Tároló<br>Esemény | Esemény<br>Napló | Tároló<br>Napló |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Szolgáltatás<br>Háló | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Red Hat megnyitása<br>SHIFT | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(önálló) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux-kiszolgáló<br>(önálló) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |


### <a name="docker-versions-supported-on-linux"></a>Támogatott Linux docker-verziók

- Docker 1.11 too1.13
- Docker CE és EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 tároló gazdagépként támogatott Linux-disztribúciók

- Ubuntu 14.04 LTS és 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE, 13.2
- openSUSE LEAP 42.2
- 7.2 és 7.3 centOS
- SLES 12
- RHEL 7.2 és 7.3.
- Red Hat OpenShift tároló Platform (OCP) 3.4-es és 3.5-ös verzióját
- ACS Mesosphere DC/OS 1.7.3 too1.8.8
- ACS Kubernetes 1.4.5 too1.6
- ACS Docker swarm használata

### <a name="supported-windows-operating-system"></a>Támogatott Windows operációs rendszer

- Windows Server 2016
- Windows 10 évforduló Edition (Professional vagy vállalati)

### <a name="docker-versions-supported-on-windows"></a>A Windows támogatott docker-verziók

- Docker 1.12 és 1,13.
- Docker 17.03.0 és újabb verziók

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás
A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

1. Adja hozzá a hello tároló figyelési megoldást tooyour OMS-munkaterület a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldások hello megoldások gyűjteménye a](log-analytics-add-solutions.md).

2. Telepítheti és használhatja a Docker az OMS-ügynököt.  Az operációs rendszer alapján, választhat a következő módszerek hello:

  * Telepítse a támogatott Linux operációs rendszerek, és futtassa a Docker majd és telepíteni hello konfigurálása [Linux OMS-ügynököt](log-analytics-agent-linux.md).  
  * A CoreOS Linux hello OMS-ügynököt nem futtatható. Ehelyett futtassa a hello OMS-ügynököt indexelése verziója Linux. Felülvizsgálati [Linux tároló gazdagépek, köztük a CoreOS](#for-all-linux-container-hosts-including-coreos) vagy [Azure Government Linux tároló gazdagépek, köztük a CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) Ha Azure Government felhőalapú tárolók dolgozik.
  * A Windows Server 2016 és a Windows 10 Docker-motorhoz hello és az ügyfél telepítése, majd csatlakozzon egy ügynök toogather adatait, és elküldi a tooLog Analytics.  

### <a name="container-services"></a>Tároló szolgáltatások

- Ha a Red Hat OpenShift környezettel rendelkezik, tekintse át a [OMS-ügynököt a Red Hat OpenShift konfigurálása](#configure-an-oms-agent-for-red-hat-openshift).
- Ha egy Kubernetes hello Azure Tárolószolgáltatási fürtöt, tekintse át a [figyelése az Azure Tárolószolgáltatás-fürtöt a Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).
- Ha egy Azure tároló szolgáltatás DC/OS-fürtről, további tudnivalókért olvassa el [figyelése az Azure tároló szolgáltatás DC/OS fürtben, az Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).
- Ha a Docker Swarm mód környezettel rendelkezik, további tudnivalókért olvassa el [OMS-ügynököt a Docker Swarmra konfigurálása](#configure-an-oms-agent-for-docker-swarm).
- A Service Fabric tárolók használja, ha további tudnivalókért olvassa el [áttekintés az Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Felülvizsgálati hello [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) további információt a cikk tooinstall és a Docker motorok konfigurálása a Windows rendszert futtató számítógépeket.

> [!IMPORTANT]
> Futnia kell az docker **előtt** hello telepítése [Linux OMS-ügynököt](log-analytics-agent-linux.md) tároló gazdagépei. Ha már telepített hello ügynök Docker telepítése előtt, Linux kell tooreinstall hello OMS-ügynököt. Docker kapcsolatos további információkért lásd: hello [Docker webhely](https://www.docker.com).


## <a name="linux-container-hosts"></a>Linux-tároló állomások

Ha már telepítette a Docker, használja a hello beállításait a tároló tooconfigure hello állomásügynök Docker való használathoz a következő. Először szükség van a OMS-munkaterület azonosítója és a kulcs, amely hello Azure-portálon találja. A munkaterületen kattintson **gyors üzembe helyezés** > **számítógépek** tooview a **munkaterület azonosítója** és **elsődleges kulcs**.  Másolással illessze be a kedvenc szerkesztő mindkettő.

### <a name="for-all-linux-container-hosts-except-coreos"></a>CoreOS kivételével az összes Linux tároló állomás

- További információk és bemutatjuk, hogyan tooinstall hello OMS-ügynököt Linux [csatlakozzon a Linux rendszerű számítógépek tooOperations felügyeleti csomaggal (OMS)](log-analytics-agent-linux.md).

### <a name="for-all-linux-container-hosts-including-coreos"></a>Az összes Linux tároló gazdagépek, köztük a CoreOS

Indítsa el a megjeleníteni kívánt toomonitor hello OMS-tárolóban. Módosítsa és használja fel a következő példa hello:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a>Minden Azure Government Linux tároló gazdagépek, köztük a CoreOS

Indítsa el a megjeleníteni kívánt toomonitor hello OMS-tárolóban. Módosítsa és használja fel a következő példa hello:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-tooone-in-a-container"></a>Váltás a egy telepített Linux ügynök tooone tároló használata
Ha korábban már használt hello közvetlenül telepített ügynök, és tooinstead használja a tárolóban futó ügynök, előbb hello OMS-ügynököt az Linux eltávolítása. Lásd: [Linux OMS-ügynök eltávolítása hello](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) hogyan toosuccessfully eltávolítása toounderstand hello ügynök.  

### <a name="configure-an-oms-agent-for-docker-swarm"></a>A Docker Swarmra OMS-ügynököt konfigurálása

A Docker Swarmról hello OMS-ügynököt is futhat, a globális szolgáltatás. A következő információk toocreate OMS-ügynököt szolgáltatásnak hello használata. Tooinsert az OMS-munkaterület azonosítója és elsődleges kulcs szükséges.

- Futtassa a hello következő hello fő csomóponton.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Red Hat OpenShift az OMS-ügynököt konfigurálása
Nincsenek három módon tooadd hello OMS-ügynököt tooRed Hat OpenShift toostart gyűjtését tároló figyelési adatokat.

* [Hello OMS-ügynök telepítése Linux](log-analytics-agent-linux.md) közvetlenül a OpenShift csomópontokon  
* [Napló Analytics Virtuálisgép-bővítmény engedélyezése](log-analytics-azure-vm-extension.md) minden OpenShift csomóponton található az Azure-ban  
* Készletként OpenShift démon-hello OMS-ügynök telepítése  

Ebben a szakaszban egy OpenShift démon beállított azt foglalkozik hello lépéseket szükséges tooinstall hello OMS-ügynököt.  

1. Jelentkezzen be toohello OpenShift csomópont, és másolja hello yam mesterfájl [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) a Githubból tooyour fő csomópont, és módosítsa az OMS-munkaterület azonosítója és az elsődleges kulcs hello értékét.
2. Futtassa a következő parancsok toocreate hello a projektet az OMS Szolgáltatáshoz, és állítsa be a hello felhasználói fiókot.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. toodeploy hello démon-készlet, futtassa a hello következő:

    `oc create -f ocp-omsagent.yaml`

5. tooverify van konfigurálva, és megfelelően működik, írja be a hello következő:

    `oc describe daemonset omsagent`  

    és hello kimeneti kell hasonlítania:

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

Ha azt szeretné toouse titkok toosecure az OMS-munkaterület azonosítója és az elsődleges kulcs hello OMS-ügynököt démon-set yam fájl használata esetén, hajtsa végre a lépéseket követve hello.

1. Jelentkezzen be toohello OpenShift csomópont, és másolja hello yam mesterfájl [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) és a titkos kulcs parancsfájljának [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) a Githubról.  Ez a parancsfájl hello titkok yam fájl OMS-munkaterület azonosítója és az elsődleges kulcs toosecure hoz létre a secrete információkat.  
2. Futtassa a következő parancsok toocreate hello a projektet az OMS Szolgáltatáshoz, és állítsa be a hello felhasználói fiókot. hello titkos parancsfájljának kér az OMS-munkaterület azonosítója <WSID> és az elsődleges kulcs <KEY> és befejezésekor hello ocp-secret.yaml fájlt hoz létre.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Hello titkos fájl telepítése hello következő futtatásával:

    `oc create -f ocp-secret.yaml`

5. Telepítés ellenőrzése hello következő futtatásával:

    `oc describe secret omsagent-secret`  

    és hello kimeneti kell hasonlítania:  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. Hello OMS-ügynököt démon-set yam fájl telepítése hello következő futtatásával:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Telepítés ellenőrzése hello következő futtatásával:

    `oc describe ds oms`

    és hello kimeneti kell hasonlítania:

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a>A Docker Swarm és Kubernetes titkos adatok védelméhez

A titkos OMS-munkaterület azonosítója és az elsődleges kulcsok Docker Swarm-és tárolószolgáltatások Kubernetes biztonságát.

#### <a name="secure-secrets-for-docker-swarm"></a>Biztonságos kulcsok Docker Swarm esetén

A Docker Swarmra, hello titkot a munkaterület azonosítója és az elsődleges kulcs létrehozása után futtathatja hello OMSagent hello Docker szolgáltatás létrehozása. A következő információk toocreate hello a titkos adatokat használják.

1. Futtassa a hello következő hello fő csomóponton.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Győződjön meg arról, hogy a titkos kulcsok megfelelően hozta létre.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Futtassa a következő parancs toomount hello titkok toohello hello indexelése OMS-ügynököt.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a>Biztonságos titkot Kubernetes yam-fájlokkal

A Kubernetes, a parancsfájl toogenerate hello titkok yam fájlt használja a munkaterület azonosítója és az elsődleges kulcs. A hello [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) lapon, a fájl vagy a titkos adatait anélkül is használhatja.

- hello OMS-ügynök DaemonSet alapértelmezett nem rendelkezik titkos információk (omsagent.yaml)
- hello OMS-ügynök DaemonSet yam fájl titkos generációs parancsfájlok toogenerate hello titkok yam (omsagentsecret.yaml) fájl titkos információk (omsagent – ds-secrets.yaml) használ.

Kiválaszthatja a toocreate omsagent DaemonSets vagy titkos kulcsok nélkül.

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a>Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok nélkül

- Hello alapértelmezett OMS-ügynök DaemonSet yam-fájl, cserélje le a hello `<WSID>` és `<KEY>` tooyour WSID és a kulcsot. Másolja a hello fájl tooyour főcsomópont és futtatási hello következő:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a>Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok

1. toouse OMS-ügynök DaemonSet titkos információk használatával hozzon létre hello titkok először.
    1. Hello parancsfájl és titkos sablonfájl másolja, és győződjön meg arról, hogy a hello ugyanabban a könyvtárban.
        - Titkos kulcs parancsprogram - secret-gen.sh létrehozása
        - titkos sablon - secret-template.yaml
    2. Hello parancsfájl, például a következő példa hello futtatása. hello parancsprogram kéri a hello OMS-munkaterület azonosítója és az elsődleges kulcs, és beírt őket, hello parancsfájl létrehoz egy titkos yam ezért is futtathatja.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Hozzon létre hello titkok pod hello következő futtatásával:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Futtassa a következő hello tooverify:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Kimeneti kell hasonlítania:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Kimeneti kell hasonlítania:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. A omsagent futó démon-készlet létrehozása``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Győződjön meg arról, hogy a következő hasonló toohello OMS-ügynök DaemonSet fut, hello:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


A Kubernetes használjon egy toogenerate hello titkok yam-parancsfájl munkaterület azonosítója és az elsődleges kulcs. A következő példa információk hello használata hello [omsagent yam fájl](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure a titkos adatokat.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

## <a name="windows-container-hosts"></a>Windows-tároló gazdagépek

### <a name="preparation-before-installing-windows-agents"></a>Windows-ügynökök telepítése előtt előkészítése

Ügynökök telepítése Windows rendszerű számítógépekre, meg kell tooconfigure hello Docker szolgáltatás. hello konfigurációs lehetővé hello Windows-ügynököt vagy hello Naplóelemzési virtuális gép bővítmény toouse hello Docker TCP szoftvercsatorna érdekében, hogy a hello ügynökök hello Docker démon távolról és a toocapture adatok figyelését.

#### <a name="toostart-docker-and-verify-its-configuration"></a>toostart Docker és a konfiguráció ellenőrzése

Nincsenek szükséges lépéseket tooset nevesített cső a Windows Server TCP fel:

1. A Windows PowerShellben engedélyezze a TCP cső és a nevesített cső.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Hello konfigurációs fájljának TCP cső Docker konfigurálásához, valamint a nevesített cső. hello konfigurációs fájl itt található: C:\ProgramData\docker\config\daemon.json.

    Hello daemon.json fájlban a hello következőkre lesz szüksége:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Windows tárolók használt hello Docker démon konfigurációjával kapcsolatos további információkért lásd: [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Windows-ügynökök telepítése

tooenable Windows és a Hyper-V tárolófigyelő, telepíthető Windows rendszerű számítógépek, amelyek tároló állomások hello Microsoft Monitoring Agent (MMA). A helyszíni környezetben futó Windows-számítógépek esetén lásd: [csatlakozás Windows számítógépek tooLog Analytics](log-analytics-windows-agents.md). A virtuális gépek Azure-ban futó csatlakoztassa őket tooLog Analytics hello segítségével [virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md).

A Service Fabric futó Windows-tárolók figyelheti. Azonban csak [az Azure-ban futó virtuális gépek](log-analytics-azure-vm-extension.md) és [a helyszíni környezetben a Windows rendszert futtató számítógépek](log-analytics-windows-agents.md) jelenleg a Service Fabric támogatottak.

Ellenőrizheti, hogy hello tároló figyelésére szolgáló megoldás megfelelően van-e megadva a Windows. hello felügyeleti csomag letölthető volt megfelelően, hogy hely toocheck *ContainerManagement.xxx*. hello fájlok hello C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok mappában kell lennie.


## <a name="solution-components"></a>Megoldás-összetevők

Ha Windows-ügynökök használ, majd hello következő felügyeleti csomag telepítve van minden számítógépen, amelyen ügynök ebben a megoldásban hozzáadásakor. Nincs konfigurációs és karbantartási hello felügyeleti csomag szükség.

- *ContainerManagement.xxx* telepítve a C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok

## <a name="container-data-collection-details"></a>Tároló az gyűjtemény adatait
hello tároló figyelésére szolgáló megoldás különböző metrikák és a napló teljesítményadatokat gyűjt tároló állomások és a tárolók ügynököt, amely engedélyezi a használ.

A következő ügynök típusok hello három percenként adatokat gyűjti.

- [Linux OMS-ügynök](log-analytics-linux-agents.md)
- [Windows-ügynök](log-analytics-windows-agents.md)
- [Napló Analytics Virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a>Tárolórekordok

hello következő táblázat példákat mutat hello tároló figyelésére szolgáló megoldás és hello adattípusok, amelyek szerepelnek a találatok között napló által gyűjtött rekordok.

| Adattípus | A naplófájl-keresési adattípust tartalmaz | Mezők |
| --- | --- | --- |
| A gazdagépek és a tárolók teljesítmény | `Type=Perf` | Számítógép, ObjectName, CounterName &#40; a processzor kihasználtsága, lemez beolvassa MB, szabad MB memória kihasználtsága (MB), írja hálózati fogadott bájtok, hálózati küldését bájt, a processzor kihasználtsága mp hálózati &#41; ellenértéknek, TimeGenerated, Számláló_elérési_útja, SourceSystem |
| Tároló leltár | `Type=ContainerInventory` | TimeGenerated, a számítógép, a tároló neve, ContainerHostname, kép, ImageTag, ContainerState, ExitCode, EnvironmentVar, a parancs, CreatedTime, StartedTime, FinishedTime, SourceSystem, Tárolóazonosító, ImageID |
| Tároló kép készlet | `Type=ContainerImageInventory` | TimeGenerated, számítógép, kép, ImageTag, ImageSize, VirtualSize, futó, szünetel, leállítását követően nem sikerült, SourceSystem, ImageID, TotalContainer |
| Tároló-napló | `Type=ContainerLog` | TimeGenerated, a számítógép, a lemezkép-Azonosítót, a tároló neve, LogEntrySource, LogEntry, SourceSystem, Tárolóazonosító |
| Tároló szolgáltatás bejelentkezési | `Type=ContainerServiceLog`  | TimeGenerated, számítógép, TimeOfCommand, kép, a parancs, SourceSystem, Tárolóazonosító |
| Tároló csomópont készlet | `Type=ContainerNodeInventory_CL`| TimeGenerated, számítógép, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kubernetes készlet | `Type=KubePodInventory_CL` | TimeGenerated, számítógép, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem |
| Tároló folyamat | `Type=ContainerProcess_CL` | TimeGenerated, számítógép, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Kubernetes események | `Type=KubeEvents_CL` | TimeGenerated, számítógép, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, üzenet |

Címkék fűzött túl*PodLabel* adattípusok a következők saját címkét. hello hozzáfűzött PodLabel címkék hello táblázatban szereplő példák. Igen `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` lesznek a környezet adatkészlet különböznek és általános hasonlítanak `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>A figyelő tárolók
Miután az OMS-portálon hello hello megoldás, hello **tárolók** csempe a tároló-gazdagépeken és az állomáson futó hello tárolók összegző információit jeleníti meg.

![Tárolók csempe](./media/log-analytics-containers/containers-title.png)

hello csempe is áttekintheti, hogy hány tárolók hello környezetben, és hogy azok még nem sikerült, fut vagy leállt.

### <a name="using-hello-containers-dashboard"></a>Hello tárolók irányítópult használata
Kattintson a hello **tárolók** csempére. Ott látni fogja a nézetek szerint vannak rendszerezve:

- **Tároló események** -tároló és a sikertelen tárolók rendelkező számítógépeket jeleníti meg.
- **Tároló naplók** -időt és azon számítógépek listáját a naplófájlok maximális számú hello generált tároló naplófájlok látható.
- **Kubernetes események** -látható Kubernetes eseményeit idő és miért három munkaállomás-csoporttal hello események létrehozása hello okok listáját. *Az adatkészlet csak Linux környezetben használatos.*
- **Kubernetes Namespace készlet** - névterek és három munkaállomás-csoporttal hello számát jeleníti meg, és megjeleníti a hierarchiában. *Az adatkészlet csak Linux környezetben használatos.*
- **Tároló csomópont készlet** -tároló csomópontok/állomáson használt vezénylési típusok hello számát jeleníti meg. hello csomópontok vagy a számítógépen is tárolók hello száma alapján vannak listázva. *Az adatkészlet csak Linux környezetben használatos.*
- **Tároló képek készlet** -tároló képek használt hello száma és kép típusú mutatja. lemezképek hello számát is hello képcímke alapján vannak listázva.
- **Tárolók állapot** -tárolók futó csomópontok és a gazdagép olyan számítógépet jeleníti meg a tároló hello teljes száma. Számítógépek is hello futtató gazdagépek száma alapján vannak listázva.
- **Tároló folyamat** -tároló folyamatok az állomásokon futó sor látható. Tárolók is találhatók futtatásával parancs/folyamat tárolók belül. *Az adatkészlet csak Linux környezetben használatos.*
- **Tároló processzorteljesítmény** -hello számítógép csomópontok/gazdagépek időbeli átlagos processzorkihasználtság sor látható. Is listák hello számítógép csomópontok/futtatja alapján átlagos CPU-felhasználást.
- **Tároló memóriateljesítmény** -memóriahasználatának vonaldiagram időbeli változását ábrázolja. Számítógép memória kihasználtsága a példány neve alapján is tartalmazza.
- **A számítógép teljesítménye** -időbeli változását ábrázolja vonaldiagramok processzorteljesítmény idővel hello százaléka memóriahasználat idő és a megabájtokban kifejezett szabad lemezterület százalékában. Is rámutat egy diagram tooview bármely sorára további részleteket.


Mindegyik hello irányítópult területe a Keresés a gyűjtött adatok futó vizuális ábrázolását.

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash01.png)

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash02.png)

A hello **tároló** területen hello felső területen kattintson a lent látható módon.

![Tárolók állapota](./media/log-analytics-containers/containers-status.png)

Naplófájl-keresési jelenik meg, amelyen a tárolók hello állapotával kapcsolatos információk.

![Tárolók napló keresése](./media/log-analytics-containers/containers-log-search.png)

Itt szerkesztheti hello keresési lekérdezés toomodify azt toofind hello konkrét információk kíváncsiak vagyunk. Napló keresések kapcsolatos további információkért lásd: [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Keresse meg a sikertelen tároló hibaelhárítása

A Naplóelemzési jelöli meg a tárolóban **sikertelen** Ha egy nem nulla kilépési kóddal kilépett. Az áttekintésben hello hibák és a hibákat a hello hello környezetben **sikertelen tárolók** területen.

### <a name="toofind-failed-containers"></a>nem sikerült toofind tárolók
1. Kattintson a hello **tároló** területen.  
   ![tárolók állapota](./media/log-analytics-containers/containers-status.png)
2. Naplófájl-keresési megnyílik, és a tároló, a következő hasonló toohello hello állapotát jeleníti meg.  
   ![tárolók állapota](./media/log-analytics-containers/containers-log-search.png)
3. Ezután kattintson a hello összesített értékének sikertelen tárolók tooview további információt. Bontsa ki a **megjelenítése további** tooview hello kép azonosítója.  
   ![nem sikerült tárolók](./media/log-analytics-containers/containers-state-failed.png)  
4. Írja be a következő hello következőt hello keresési lekérdezés. `Type=ContainerInventory <ImageID>`például a kép mérete és a leállított és sikertelen képek hello kép toosee adatait.  
   ![nem sikerült tárolók](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Keresési naplókat a további adatai
Ha egy adott hiba hibaelhárítás segíthet toosee hol lépett fel a környezetben. a következő napló típusok hello segítségével hozhat létre lekérdezéseket kívánt tooreturn hello információt.


- **ContainerImageInventory** – ezt a típust használja a lemezkép és tooview kép információt például a lemezkép-azonosítók vagy méretek szerint vannak rendszerezve toofind információk regisztráláskor.
- **ContainerInventory** – Ez a típus használható, ha azt szeretné, hogy információt tároló helye, Mik azok a nevük, és mi képek azok futtatja.
- **ContainerLog** – Ez a típus használható, ha toofind adott hibanaplóban szereplő információkat, és bejegyzéseket.
- **ContainerNodeInventory_CL** használni ehhez a típushoz hello információ a gazdacsomópont/ahol tárolók tartózkodnak. Azt teszi lehetővé Docker verzióval, vezénylés típusát, tárolási és hálózati információit.
- **ContainerProcess_CL** használja a típus tooquickly lásd: hello tárolóban futó hello folyamata.
- **ContainerServiceLog** – Ez a típus használható regisztráláskor toofind ellenőrzési útvonalat nyújt információkat a hello Docker démon, például Indítás, Leállítás, törléséhez vagy parancsok lekéréses.
- **KubeEvents_CL** használja a típus toosee hello Kubernetes eseményeket.
- **KubePodInventory_CL** ezt a típust használja, ha azt szeretné, hogy toounderstand hello fürt hierarchiára vonatkozó információk.


### <a name="toosearch-logs-for-container-data"></a>a tároló adatok toosearch naplók
* Válassza ki, hogy tudja, hogy a képfájl nemrég sikertelen volt, és hello hibanaplók keresése. Indítsa el a tároló neve, amelyen fut. a lemezkép keresése a **ContainerInventory** keresési. Például keresése`Type=ContainerInventory ubuntu Failed`  
    ![Ubuntu tárolók keresése](./media/log-analytics-containers/search-ubuntu.png)

  hello hello tároló neve melletti túl**neve**, majd keresse meg a lesznek a naplók. Ebben a példában ez `Type=ContainerLog cranky_stonebreaker`.

**Teljesítmény-információk megtekintése**

Amikor tooconstruct lekérdezések verziótól segíthet toosee Újdonságok lehetséges először. Például minden teljesítményadat, írja be a széles körű lekérdezés hello követően próbálja meg keresési toosee lekérdezése.

```
Type=Perf
```

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf01.png)

Hello teljesítményadatokat is lát tooa adott tárolóhoz azt hello nevének beírásával toohello sarkában a lekérdezés hatókörét megadhatja.

```
Type=Perf <containerName>
```

Amely az egyes tároló összegyűjtött teljesítménymutatók hello listáját jeleníti meg.

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Példa napló keresési lekérdezések
Fennállt gyakran hasznos toobuild lekérdezi például vagy két kezdve, és adja őket az toofit módosítása a környezetben. Kiindulási pontként, kísérletezhet hello **mintalekérdezések** terület toohelp összetettebb lekérdezések létrehozása.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Tárolók lekérdezések](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Naplófájl-keresési lekérdezések mentése
Lekérdezések mentése az Naplóelemzési szabványos szolgáltatása. Mentve, konfigurálnia kell az alábbiakhoz hasznos talált későbbi használatra lesz szüksége.

Miután létrehozott egy lekérdezést, amely akkor hasznosak, mentse kattintva **Kedvencek** hello napló keresése oldal hello tetején. Ezután könnyen elérhető azt később hello **saját irányítópult** lap.

## <a name="next-steps"></a>Következő lépések
* [Naplók keresése](log-analytics-log-searches.md) tooview részletes tároló rekordok.
