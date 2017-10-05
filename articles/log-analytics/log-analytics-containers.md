---
title: "Tároló figyelés megoldás az Azure Naplóelemzés |} Microsoft Docs"
description: "A tároló figyelésére szolgáló megoldás a Log Analyticshez segít megtekintése és kezelése a Docker és a Windows tároló állomások egyetlen helyen megvalósítható."
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
ms.openlocfilehash: b2e03531ee401f4552198e5dd50fbfe1d970f0e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a>A Naplóelemzési tároló figyelés megoldás

![Tárolók szimbólum](./media/log-analytics-containers/containers-symbol.png)

Ez a cikk ismerteti, hogyan állítsa be, és a tároló figyelésére szolgáló megoldás használata Naplóelemzési, amely segít megtekintése és kezelése a Docker és a Windows tároló állomások egyetlen helyen megvalósítható. Docker egy olyan szoftver virtualizálási a rendszer, az informatikai infrastruktúra szoftver központi telepítését automatizáló tárolók létrehozásához használt.

A megoldás bemutatja, hogy mely tárolók fut, a tároló képet azok futtatja, és ahol tárolók futnak. Tárolók használt parancsok bemutató részletes naplózási információk is megtekinthetők. És tárolók megtekintésével és központosított naplók keresése távolról megtekintheti a Docker vagy a Windows rendszerű nélkül is elháríthatók. Tárolók, amelyek esetleg zajos vagy a felhasználó felesleges erőforrások egy gazdagépen található. És megtekintheti a központi Processzor, memória, tárolási és hálózati használati és teljesítményadatokat adatai tárolók. Windows rendszerű számítógépeken központosítása és hasonlítsa össze a napló, a Windows Server Hyper-V és a Docker-tárolók. A megoldás a következő tároló orchestrators támogatja:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift


Az alábbi ábrán látható a különböző tároló gazdagépek és az OMS ügynökök közötti kapcsolatokat.

![Tárolók diagramja](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a>Rendszerkövetelmények

Megkezdése előtt tekintse át az alábbi részleteket megfelel az Előfeltételek ellenőrzése.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Figyelési megoldást igényelnek tároló Docker Orchestrator és az operációs rendszer platform támogatása
Az alábbi táblázat ismerteti a Docker vezénylési és az operációs rendszer figyelési tároló készlet, a teljesítmény és a naplók és a Naplóelemzési támogatást.   

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

- Docker 1.11-1,13.
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
- ACS Mesosphere DC/OS 1.7.3 való 1.8.8
- ACS Kubernetes 1.4.5 az 1.6-os
- ACS Docker swarm használata

### <a name="supported-windows-operating-system"></a>Támogatott Windows operációs rendszer

- Windows Server 2016
- Windows 10 évforduló Edition (Professional vagy vállalati)

### <a name="docker-versions-supported-on-windows"></a>A Windows támogatott docker-verziók

- Docker 1.12 és 1,13.
- Docker 17.03.0 és újabb verziók

## <a name="installing-and-configuring-the-solution"></a>Telepítése és a megoldás konfigurálása
Az alábbi információk segítségével telepítse és konfigurálja a megoldást.

1. A tároló figyelésére szolgáló megoldás hozzáadása az OMS-munkaterület a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) vagy ismertetett folyamatot követve [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).

2. Telepítheti és használhatja a Docker az OMS-ügynököt.  Az operációs rendszer alapján, választhat a következőkkel:

  * A támogatott Linux operációs rendszerek telepítése futtatásához Docker és telepítse és konfigurálja a [Linux OMS-ügynököt](log-analytics-agent-linux.md).  
  * A CoreOS Linux rendszeren futtatható, az OMS-ügynököt. Az OMS-ügynököt a tárolóalapú változata ehelyett Linux fusson. Felülvizsgálati [Linux tároló gazdagépek, köztük a CoreOS](#for-all-linux-container-hosts-including-coreos) vagy [Azure Government Linux tároló gazdagépek, köztük a CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) Ha Azure Government felhőalapú tárolók dolgozik.
  * A Windows Server 2016 és a Windows 10 a Docker-motorhoz és az ügyfél telepítése, majd kösse a az ügynök információkat gyűjt, és elküldi a Naplóelemzési.  

### <a name="container-services"></a>Tároló szolgáltatások

- Ha a Red Hat OpenShift környezettel rendelkezik, tekintse át a [OMS-ügynököt a Red Hat OpenShift konfigurálása](#configure-an-oms-agent-for-red-hat-openshift).
- Ha az Azure Tárolószolgáltatás Kubernetes fürtöt, tekintse át a [figyelése az Azure Tárolószolgáltatás-fürtöt a Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).
- Ha egy Azure tároló szolgáltatás DC/OS-fürtről, további tudnivalókért olvassa el [figyelése az Azure tároló szolgáltatás DC/OS fürtben, az Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).
- Ha a Docker Swarm mód környezettel rendelkezik, további tudnivalókért olvassa el [OMS-ügynököt a Docker Swarmra konfigurálása](#configure-an-oms-agent-for-docker-swarm).
- A Service Fabric tárolók használja, ha további tudnivalókért olvassa el [áttekintés az Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Tekintse át a [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) cikk telepítése és konfigurálása a Docker motorok Windows rendszerű számítógépekre vonatkozó további információért.

> [!IMPORTANT]
> Futnia kell az docker **előtt** telepítése a [Linux OMS-ügynököt](log-analytics-agent-linux.md) tároló gazdagépei. Ha már telepítette az ügynököt Docker telepítése előtt, telepítse újra az OMS-ügynököt Linux szeretné. További információ a Docker: a [Docker webhely](https://www.docker.com).


## <a name="linux-container-hosts"></a>Linux-tároló állomások

Ha már telepítette a Docker, használja a következő beállításokat, a tároló állomás Docker használható az ügynök konfigurálása. Először szükség van a OMS-munkaterület azonosítója és a kulcs, amely az Azure portálon található. A munkaterületen kattintson **gyors üzembe helyezés** > **számítógépek** megtekintéséhez a **munkaterület azonosítója** és **elsődleges kulcs**.  Másolással illessze be a kedvenc szerkesztő mindkettő.

### <a name="for-all-linux-container-hosts-except-coreos"></a>CoreOS kivételével az összes Linux tároló állomás

- További információkat és lépéseket az OMS-ügynök telepítése Linux [csatlakozzon a Linux rendszerű számítógépek Operations Management Suite (OMS)](log-analytics-agent-linux.md).

### <a name="for-all-linux-container-hosts-including-coreos"></a>Az összes Linux tároló gazdagépek, köztük a CoreOS

Indítsa el az OMS-tároló, amely segítségével nyomon követni kívánt. Módosítsa és használja a következő példát:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a>Minden Azure Government Linux tároló gazdagépek, köztük a CoreOS

Indítsa el az OMS-tároló, amely segítségével nyomon követni kívánt. Módosítsa és használja a következő példát:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-to-one-in-a-container"></a>Egy tároló telepített Linux ügynök használatával való váltás
Ha korábban a közvetlenül telepített ügynök használt, és használja a tárolóban futó ügynök szeretne, el kell távolítania az OMS-ügynököt Linux. Lásd: [az OMS-ügynök eltávolítása Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) annak megértése, hogyan sikeresen távolítsa el az ügynököt.  

### <a name="configure-an-oms-agent-for-docker-swarm"></a>A Docker Swarmra OMS-ügynököt konfigurálása

A Docker Swarmról az OMS-ügynököt is futhat, a globális szolgáltatás. Az alábbi információk használatával hozzon létre egy OMS-ügynököt szolgáltatást. Helyezze be az OMS-munkaterület azonosítója és az elsődleges kulcs van szüksége.

- Futtassa a következő fő csomóponton.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Red Hat OpenShift az OMS-ügynököt konfigurálása
Nincsenek három módszerrel adhat hozzá az OMS-ügynököt Red Hat OpenShift tároló figyelési adatok gyűjtése elindításához.

* [Az OMS-ügynök telepítése Linux](log-analytics-agent-linux.md) közvetlenül a OpenShift csomópontokon  
* [Napló Analytics Virtuálisgép-bővítmény engedélyezése](log-analytics-azure-vm-extension.md) minden OpenShift csomóponton található az Azure-ban  
* Az OMS-ügynököt telepíteni készletként OpenShift démon-  

Ebben a szakaszban foglalkozunk azt egy OpenShift démon beállított az OMS-ügynök telepítéséhez szükséges lépéseket.  

1. Jelentkezzen be a OpenShift fő csomópont, és másolja az yam [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) a Githubból a fő csomópontra, és módosítsa az OMS-munkaterület azonosítója és az elsődleges kulcs értékét.
2. A következő parancsokat az OMS-projekt létrehozása és beállítása a felhasználói fiók.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. A szűrődémon-set telepítéséhez futtassa a következő:

    `oc create -f ocp-omsagent.yaml`

5. Ellenőrizze, hogy van-e konfigurálva, és megfelelően működik, írja be a következőt:

    `oc describe daemonset omsagent`  

    és a kimeneti kell hasonlítania:

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

Ha szeretné védeni kell az OMS-munkaterület azonosítója és az elsődleges kulcs, amikor az OMS-ügynököt démon-set yam fájl használata a titkos kulcsok segítségével, a következő lépésekkel.

1. Jelentkezzen be a OpenShift fő csomópont, és másolja az yam [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) és a titkos kulcs parancsfájljának [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) a Githubról.  Ezt a parancsfájlt hoz létre a titkos kulcsok yam fájlt az OMS-munkaterület azonosítója és az elsődleges kulcs biztonságossá tételéhez a secrete információkat.  
2. A következő parancsokat az OMS-projekt létrehozása és beállítása a felhasználói fiók. A titkos kulcs parancsfájljának kér az OMS-munkaterület azonosítója <WSID> és az elsődleges kulcs <KEY> és befejezésekor a ocp-secret.yaml fájlt hoz létre.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. A titkos fájl központi telepítése a következő futtatásával:

    `oc create -f ocp-secret.yaml`

5. Telepítés ellenőrzése a következő futtatásával:

    `oc describe secret omsagent-secret`  

    és a kimeneti kell hasonlítania:  

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

6. Az OMS-ügynököt démon-set yam fájl központi telepítése a következő futtatásával:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Telepítés ellenőrzése a következő futtatásával:

    `oc describe ds oms`

    és a kimeneti kell hasonlítania:

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

A Docker Swarmra a titkos kulcsot a munkaterület azonosítója és az elsődleges kulcs létrehozása után is futtathatja a létrehozás OMSagent a Docker szolgáltatáshoz. Az alábbi információk használatával hozzon létre a titkos adatokat.

1. Futtassa a következő fő csomóponton.

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

3. A következő parancsot a titkos kulcsok számára a tárolóalapú OMS-ügynököt csatlakoztatni.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a>Biztonságos titkot Kubernetes yam-fájlokkal

Kubernetes, a parancsfájl hozza létre a titkos kulcsok yam fájlt a munkaterület azonosítója és az elsődleges kulcs használata. : A [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) lapon, a fájl vagy a titkos adatait anélkül is használhatja.

- Az OMS-ügynök alapértelmezett DaemonSet nem rendelkezik titkos információk (omsagent.yaml)
- Az OMS-ügynök DaemonSet yam fájl titkos generációs parancsfájlok titkos információk (omsagent – ds-secrets.yaml) segítségével hozza létre a titkos kulcsok yam (omsagentsecret.yaml) fájlt.

Ha szeretné omsagent DaemonSets létrehozása, vagy a titkos kulcsok nélkül.

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a>Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok nélkül

- Az alapértelmezett OMS-ügynök DaemonSet yam fájl cserélje le a `<WSID>` és `<KEY>` a WSID és a kulcsot. A fájl átmásolása a fő csomópont, és futtassa az alábbi parancsot:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a>Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok

1. Titkos információk alapján OMS-ügynök DaemonSet használatához először hozza létre a titkos kulcsok.
    1. Másolja a parancsfájlt és a titkos sablonfájl, és győződjön meg arról, ezek is ugyanabban a könyvtárban.
        - Titkos kulcs parancsprogram - secret-gen.sh létrehozása
        - titkos sablon - secret-template.yaml
    2. Futtassa a parancsfájlt, az alábbi példához hasonló. A parancsprogram kéri az OMS-munkaterület azonosítója és az elsődleges kulcs, és azokat megadása után a parancsfájl létrehoz egy titkos yam ezért is futtathatja.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Hozza létre a titkos kulcsok fogyasztanak a következő futtatásával:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Győződjön meg arról, hogy futtassa az alábbi parancsot:

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

2. Győződjön meg arról, hogy az OMS-ügynök DaemonSet fut, a következőhöz hasonló:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Kubernetes, a parancsfájl segítségével hozza létre a titkos kulcsok yam fájlt a munkaterület azonosítója és az elsődleges kulcs. A következő példa információk a [omsagent yam fájl](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) a titkos adatok védelméhez.

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

Ügynökök telepítése Windows rendszerű számítógépekre, előtt kell beállítani a Docker-szolgáltatást. A konfiguráció lehetővé teszi, hogy a Windows-ügynök vagy a Naplóelemzési virtuálisgép-bővítmény, hogy az ügynökök távolról elérjék a Docker démon a Docker TCP szoftvercsatorna használandó és a figyelési adatok rögzítéséhez.

#### <a name="to-start-docker-and-verify-its-configuration"></a>Indítsa el a Docker és a konfiguráció ellenőrzése

A Windows Server nevesített cső TCP beállításához szükséges lépésből áll:

1. A Windows PowerShellben engedélyezze a TCP cső és a nevesített cső.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. A konfigurációs fájl TCP pipe Docker konfigurálásához, valamint a nevesített cső. A konfigurációs fájl nem található: C:\ProgramData\docker\config\daemon.json.

    A daemon.json fájlban a következőkre lesz szüksége:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Windows tárolók használt Docker démon konfigurációjával kapcsolatos további információkért lásd: [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Windows-ügynökök telepítése

Windows és a Hyper-V tároló figyelés engedélyezéséhez telepítse a Microsoft Monitoring Agent (MMA) Windows rendszerű számítógépeken, amelyek tároló gazdagépek. A helyszíni környezetben futó Windows-számítógépek esetén lásd: [Log Analyticshez való csatlakozás Windows számítógépek](log-analytics-windows-agents.md). A virtuális gépek Azure-ban futó csatlakoztassa őket Naplóelemzési használatával a [virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md).

A Service Fabric futó Windows-tárolók figyelheti. Azonban csak [az Azure-ban futó virtuális gépek](log-analytics-azure-vm-extension.md) és [a helyszíni környezetben a Windows rendszert futtató számítógépek](log-analytics-windows-agents.md) jelenleg a Service Fabric támogatottak.

Ellenőrizheti, hogy a tároló figyelésére szolgáló megoldás helyesen van megadva a Windows. Ellenőrizze, hogy a felügyeleti csomag megfelelő volt-e letöltésre, keressen *ContainerManagement.xxx*. A fájlok a C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok mappában kell lennie.


## <a name="solution-components"></a>Megoldás-összetevők

Ha Windows-ügynökök használ, akkor a következő felügyeleti csomag telepítve van minden számítógépen, amelyen ügynök ebben a megoldásban hozzáadásakor. Nincs konfigurációs és karbantartási szükség a felügyeleti csomag.

- *ContainerManagement.xxx* telepítve a C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok

## <a name="container-data-collection-details"></a>Tároló az gyűjtemény adatait
A tároló figyelésére szolgáló megoldás különböző metrikák és a napló teljesítményadatokat gyűjt tároló állomások és a tárolók ügynököt, amely engedélyezi a használ.

Adatgyűjtés három percenként a következő ügynök típusok.

- [Linux OMS-ügynök](log-analytics-linux-agents.md)
- [Windows-ügynök](log-analytics-windows-agents.md)
- [Napló Analytics Virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a>Tárolórekordok

Az alábbi táblázat a tároló figyelésére szolgáló megoldás és az adatok, amelyek szerepelnek a találatok között napló által gyűjtött rekordok példát.

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

Címkék fűzött *PodLabel* adattípusok a következők saját címkét. A hozzáfűzött PodLabel címkék a táblázatban szereplő példák. Igen `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` lesznek a környezet adatkészlet különböznek és általános hasonlítanak `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>A figyelő tárolók
Miután az OMS-portálon, a megoldás a **tárolók** csempe a tároló gazdagépek és a tárolók az állomáson fut összegző információit jeleníti meg.

![Tárolók csempe](./media/log-analytics-containers/containers-title.png)

A csempén áttekintheti hány tárolók van a környezetben, és hogy azok még nem sikerült, fut vagy leállt.

### <a name="using-the-containers-dashboard"></a>A tárolók irányítópult használata
Kattintson a **tárolók** csempére. Ott látni fogja a nézetek szerint vannak rendszerezve:

- **Tároló események** -tároló és a sikertelen tárolók rendelkező számítógépeket jeleníti meg.
- **Tároló naplók** -tároló naplófájlok idő és a számítógépeknek a listáját, a naplófájlok maximális számú generált diagramot ábrázol.
- **Kubernetes események** -Kubernetes eseményeit idő és a okok miért három munkaállomás-csoporttal generált események listája látható. *Az adatkészlet csak Linux környezetben használatos.*
- **Kubernetes Namespace készlet** - névterek és három munkaállomás-csoporttal számát jeleníti meg, és megjeleníti a hierarchiában. *Az adatkészlet csak Linux környezetben használatos.*
- **Tároló csomópont készlet** -tároló csomópontok/állomáson használt vezénylési típusok számát jeleníti meg. A csomópontok vagy a számítógépen is a tárolók száma alapján vannak listázva. *Az adatkészlet csak Linux környezetben használatos.*
- **Tároló képek készlet** -tároló rendszerképek használt teljes száma és típusú kép számát jeleníti meg. A lemezképek számát is a képcímke alapján vannak listázva.
- **Tárolók állapot** -csomópontok és a gazdagép számítógépek, amelyeken a futó tárolók tároló teljes számát jeleníti meg. Számítógépek is futtató gazdagépek száma alapján vannak listázva.
- **Tároló folyamat** -tároló folyamatok az állomásokon futó sor látható. Tárolók is találhatók futtatásával parancs/folyamat tárolók belül. *Az adatkészlet csak Linux környezetben használatos.*
- **Tároló processzorteljesítmény** -sor látható a számítógép-csomópont/gazdagépek időbeli átlagos CPU-használatot. Is a számítógép-csomópont/gazdagépek listák alapuló CPU-felhasználást.
- **Tároló memóriateljesítmény** -memóriahasználatának vonaldiagram időbeli változását ábrázolja. Számítógép memória kihasználtsága a példány neve alapján is tartalmazza.
- **A számítógép teljesítménye** -időbeli változását ábrázolja az idő múlásával processzorteljesítmény százaléka vonaldiagramok memóriahasználat idő és a megabájtokban kifejezett szabad lemezterület százalékában. Minden sor egy diagram megtekintheti annak további részleteit is mutat.


Az irányítópult területenként a Keresés a gyűjtött adatok futó vizuális ábrázolását.

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash01.png)

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash02.png)

Az a **tároló** területen kattintson a felső terület alább látható módon.

![Tárolók állapota](./media/log-analytics-containers/containers-status.png)

Naplófájl-keresési jelenik meg, amelyen a tárolók állapotára vonatkozó adatokat.

![Tárolók napló keresése](./media/log-analytics-containers/containers-log-search.png)

Itt szerkesztheti a keresési lekérdezést úgy, hogy megtalálhatók a keresett információk módosításához kíváncsiak vagyunk. Napló keresések kapcsolatos további információkért lásd: [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Keresse meg a sikertelen tároló hibaelhárítása

A Naplóelemzési jelöli meg a tárolóban **sikertelen** Ha egy nem nulla kilépési kóddal kilépett. A hibák és a környezet hibáinak áttekintését láthatja a **sikertelen tárolók** területen.

### <a name="to-find-failed-containers"></a>Nem sikerült tárolók kereséséhez
1. Kattintson a **tároló** területen.  
   ![tárolók állapota](./media/log-analytics-containers/containers-status.png)
2. Naplófájl-keresési megnyílik, és a tárolók, az alábbihoz hasonló állapotát jeleníti meg.  
   ![tárolók állapota](./media/log-analytics-containers/containers-log-search.png)
3. Ezután kattintson a további információk megjelenítéséhez sikertelen tárolók összesített értékét. Bontsa ki a **megjelenítése további** megtekintéséhez a lemezkép-azonosítót.  
   ![nem sikerült tárolók](./media/log-analytics-containers/containers-state-failed.png)  
4. Ezután írja be a következőt a keresési lekérdezés. `Type=ContainerInventory <ImageID>`a kép, például a lemezkép mérete és a leállított és sikertelen csomópontképek száma kapcsolatos részletek megtekintéséhez.  
   ![nem sikerült tárolók](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Keresési naplókat a további adatai
Ha egy adott hiba elhárításához van, hogy hol lépett fel a környezetben segíthet. A következő napló típusok segítségével hozhat létre a kívánt lekérdezések adott vissza az adatokat.


- **ContainerImageInventory** – ezt a típust használja, a lemezkép szerint vannak rendszerezve kapcsolatos információk és lemezkép információk például az azonosítók vagy méretű kép megjelenítéséhez regisztráláskor.
- **ContainerInventory** – Ez a típus használható, ha azt szeretné, hogy információt tároló helye, Mik azok a nevük, és mi képek azok futtatja.
- **ContainerLog** – Ez a típus használja, ha a keresett adott hibanaplóban szereplő információkat, és bejegyzéseket.
- **ContainerNodeInventory_CL** használni ehhez a típushoz a gazdacsomópont/vonatkozó információkat ahol tárolók tartózkodnak. Azt teszi lehetővé Docker verzióval, vezénylés típusát, tárolási és hálózati információit.
- **ContainerProcess_CL** ennek használatával gyorsan megtekintheti a tárolóban futó folyamatot.
- **ContainerServiceLog** – ezt a típust használja, ellenőrzési útvonalat nyújt információkat a Docker démon, például Indítás, Leállítás, törlése, illetve parancsok lekéréses kereséséhez regisztráláskor.
- **KubeEvents_CL** ennek használatával az Kubernetes eseményeket.
- **KubePodInventory_CL** ezt a típust használja, ha szeretné tudni, hogy a fürt hierarchiára vonatkozó információk.


### <a name="to-search-logs-for-container-data"></a>Keresés a naplókat a további adatai
* Válassza ki, hogy tudja, hogy a képfájl nemrég sikertelen volt, és a hibanaplók keresése. Indítsa el a tároló neve, amelyen fut. a lemezkép keresése a **ContainerInventory** keresési. Például keresése`Type=ContainerInventory ubuntu Failed`  
    ![Ubuntu tárolók keresése](./media/log-analytics-containers/search-ubuntu.png)

  A tároló neve **neve**, majd keresse meg a lesznek a naplók. Ebben a példában ez `Type=ContainerLog cranky_stonebreaker`.

**Teljesítmény-információk megtekintése**

Ha éppen kezdődő, lekérdezések összeállításához, megtudhatja, mi lehetséges először segítséget. Például minden teljesítményadat megtekintéséhez próbálja írja be a következő keresési lekérdezés széleskörű lekérdezést.

```
Type=Perf
```

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf01.png)

Hatókörét megadhatja a teljesítményadatokat is lát egy adott tárolóhoz jobb oldalán a lekérdezést, hogy a név beírásával.

```
Type=Perf <containerName>
```

Amely az egyes tároló összegyűjtött teljesítménymutatók listáját jeleníti meg.

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Példa napló keresési lekérdezések
Gyakran érdemes hozhatók létre olyan lekérdezések például vagy két és megfeleljenek a környezet módosításával. Kiindulási pontként, kísérletezhet az **mintalekérdezések** terület segíteni bonyolultabb lekérdezéseket.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Tárolók lekérdezések](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Naplófájl-keresési lekérdezések mentése
Lekérdezések mentése az Naplóelemzési szabványos szolgáltatása. Mentve, konfigurálnia kell az alábbiakhoz hasznos talált későbbi használatra lesz szüksége.

Miután létrehozott egy lekérdezést, amely akkor hasznosak, mentse kattintva **Kedvencek** a napló keresése oldal tetején. Ezután egyszerűen hozzáférhet az azt később a **saját irányítópult** lap.

## <a name="next-steps"></a>Következő lépések
* [Naplók keresése](log-analytics-log-searches.md) részletes tároló rekordok megtekintéséhez.
