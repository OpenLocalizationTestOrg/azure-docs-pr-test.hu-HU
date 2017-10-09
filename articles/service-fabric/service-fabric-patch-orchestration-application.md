---
title: "Service Fabric-javítás vezénylési alkalmazás aaaAzure |} Microsoft Docs"
description: "Alkalmazás tooautomate operációs rendszer a Service Fabric-fürt javítását."
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: nachandr
ms.openlocfilehash: fbb89aa2ea418181ee908a01850178c113c462fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="patch-hello-windows-operating-system-in-your-service-fabric-cluster"></a>Javítás hello Windows operációs rendszer a Service Fabric-fürt

hello javítás vezénylési alkalmazása az Azure Service Fabric-alkalmazás, amely automatizálja az operációs rendszer a leállás nélküli Azure Service Fabric-fürt javítását.

hello javítás vezénylési app hello következőket biztosítja:

- **Operációs rendszer automatikus frissítés telepítése**. Az operációs rendszer frissítéseinek automatikusan letöltődjön és települjön. Fürtcsomópont újraindítása van szükség esetén a fürt leállítása nélkül.

- **Fürttámogató javítását és állapotfigyelő integrációs**. Frissítések alkalmazása során hello javítás vezénylési app hello fürtcsomópontok hello állapotát figyeli. Fürtcsomópontok frissített egy csomópont egyszerre egy időben vagy több frissítési tartományt. Hello fürt hello állapotának miatt toohello a javítási folyamat leáll, ha leállított tooprevent súlyosbító hello probléma javítását.

## <a name="internal-details-of-hello-app"></a>Hello alkalmazás belső részletei

hello javítás vezénylési alkalmazás a következő alösszetevők hello áll:

- **Koordinátora szolgáltatás**: az állapotalapú szolgáltatás felelős:
    - Hello Windows Update feladat koordináló hello teljes fürtöt.
    - A végrehajtott Windows Update-műveletek hello eredményét tárolásához.
- **Csomópont-ügynökszolgáltatás**: Ez az állapot nélküli szolgáltatás fut a Service Fabric-fürt összes csomópontján. hello szolgáltatás felelős:
    - Hello csomópont ügynök NTService rendszerindítása.
    - Figyelési hello csomópont ügynök NTService.
- **Csomópont ügynök NTService**: A Windows NT-szolgáltatás lefuttat egy magasabb szintű jogosultság (rendszer). Ezzel szemben hello csomópont ügynökszolgáltatás és hello koordinátor szolgáltatást, futtassa alacsonyabb szintű jogosultság (hálózati szolgáltatás). hello a szolgáltatás felelős a Windows Update feladatok a hello fürt összes csomópontján a következő hello végrehajtásához:
    - A Windows Update automatikus hello csomópont letiltása.
    - Letöltése és telepítése a Windows Update nyújtott toohello házirend hello felhasználó alapján történik.
    - Hello gép post Windows Update telepítés újraindítása.
    - Windows-frissítések toohello koordinátora szolgáltatás hello eredményeinek feltöltése.
    - Jelentéskészítési rendszerállapot-jelentéseket abban az esetben, ha egy művelet sikertelen volt a kimerítsék összes ismételt próbálkozás után.

> [!NOTE]
> hello javítás vezénylési alkalmazás által használt hello Service Fabric manager rendszer szolgáltatás toodisable javítsa vagy hello csomópont engedélyezése és állapotát ellenőrzi. hello javítási feladat hozta létre hello javítás vezénylési app számok hello minden csomópont Windows-frissítés folyamatban.

## <a name="prerequisites"></a>Előfeltételek

### <a name="minimum-supported-service-fabric-runtime-version"></a>Legkisebb támogatott verzió a Service Fabric futásidejű

#### <a name="azure-clusters"></a>Az Azure-fürtök
Azure Service Fabric futásidejű verzióját v5.5 rendelkező fürtök hello javítás vezénylési alkalmazást kell futtatni, vagy később.

#### <a name="standalone-on-premises-clusters"></a>Önálló helyszíni fürtök
hello javítás vezénylési alkalmazást kell futtatni, amelyeken a Service Fabric futásidejű verzióját v5.6 önálló fürtökön vagy újabb.

### <a name="enable-hello-repair-manager-service-if-its-not-running-already"></a>Engedélyezze a hello manager szolgáltatás (Ha nem már fut)

hello javítás vezénylési alkalmazásnak hello javítási manager rendszer szolgáltatás toobe hello fürtön engedélyezve van szüksége.

#### <a name="azure-clusters"></a>Az Azure-fürtök

A hello ezüst tartóssági szint Azure fürt rendelkezik hello javítása kezelő szolgáltatás alapértelmezés szerint engedélyezve van. Az Azure-fürtöket helyez hello arany tartóssági szint előfordulhat, hogy, vagy nem rendelkezik hello manager szolgáltatás engedélyezve van, attól függően, hogy ezek a fürt létrehozásakor volt. Az Azure-fürtöket helyez hello bronz tartóssági szint, alapértelmezés szerint nem rendelkeznek hello javítása kezelő szolgáltatás engedélyezve van. Ha hello szolgáltatás már engedélyezve van, láthatja, hello rendszer szolgáltatások szakaszának hello Service Fabric Explorer futnak.

##### <a name="azure-portal"></a>Azure Portal
Azure-portálon manager javítást engedélyezheti a fürt beállításához hello időpontban. Válassza ki `Include Repair Manager` lehetőség alatt `Add on features` a fürtkonfiguráció hello időpontjában.
![Az engedélyezés Manager javítást kép Azure-portálon](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sablon
Másik lehetőségként használhatja a hello [Azure Resource Manager sablon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello javítási manager szolgáltatás a meglévő és új Service Fabric-fürtök. Hello sablon lekérése hello fürt, amelyet az toodeploy. Hello minta sablonok, vagy hozzon létre egy egyéni erőforrás-kezelő sablont. 

tooenable hello javítási manager szolgáltatás használ [Azure Resource Manager sablon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. Először ellenőrizze, hogy hello `apiversion` értéke túl`2017-07-01-preview` a hello `Microsoft.ServiceFabric/clusters` erőforrás, ahogy az alábbi részlet hello. Ha nem egyezik, akkor meg kell tooupdate hello `apiVersion` toohello érték `2017-07-01-preview`:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Most lehetővé teszi a hello manager szolgáltatás hello következő hozzáadásával `addonFeatures` szakasz után hello `fabricSettings` szakasz:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. A fürt sablon ezekkel a módosításokkal frissítése után alkalmazza őket, és lehetővé teszik a hello frissítés befejezéséhez. Most már megtekintheti hello javítási manager szolgáltatás fut a fürtön. Azt nevezzük `fabric:/System/RepairManagerService` hello rendszer szolgáltatások szakaszának hello Service Fabric Explorerben talál. 

### <a name="standalone-on-premises-clusters"></a>Önálló helyszíni fürtök

Használhatja a hello [önálló Windows-fürt konfigurációs beállításainak](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello javítási manager szolgáltatás a meglévő és új Service Fabric-fürt.

tooenable hello manager szolgáltatás:

1. Először ellenőrizze, hogy hello `apiversion` a [általános fürtkonfigurációk](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) értéke túl`04-2017` vagy magasabb:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Most lehetővé teszi a kezelő szolgáltatás hello következő hozzáadásával `addonFeaturres` szakasz után hello `fabricSettings` szakasz a lent látható módon:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. A fürtjegyzékben frissítéséhez ezekkel a módosításokkal, frissített hello fürtjegyzékben használatával [hozzon létre egy új fürtöt](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) vagy [frissítési hello fürtkonfiguráció](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration). Miután frissített a fürtjegyzékben hello fürt fut, hello javítási manager szolgáltatás fut a fürtön, melynek neve most már megtekintheti `fabric:/System/RepairManagerService`, a rendszer services hello Service Fabric Explorer szakaszban.

### <a name="disable-automatic-windows-update-on-all-nodes"></a>Tiltsa le a Windows Update automatikus minden csomópont

Windows automatikus frissítések tooavailability adatvesztés vezethet, mivel több fürtcsomóponton is újraindíthatja hello azonos idő. hello javítás vezénylési alkalmazást, és alapértelmezés szerint záma toodisable hello automatikus Windows-frissítés a fürt minden csomópontján. Azonban ha egy rendszergazda vagy a csoportházirend hello-beállítások kezelik, ajánlott beállítás hello házirend túl "értesítés letöltés előtt" kifejezetten a Windows Update.

### <a name="optional-enable-azure-diagnostics"></a>Választható: Engedélyezze az Azure Diagnostics

A Service Fabric futásidejű verzióját futtató fürtök `5.6.220.9494` és fent gyűjtése javítás vezénylési app naplók Service Fabric részeként naplóz.
Ezt a lépést kihagyhatja, ha a fürtön futó Service Fabric futásidejű verzióját `5.6.220.9494` vagy újabb verzió.

A Service Fabric futásidejű verzióját futtató fürtök kisebb, mint `5.6.220.9494`, hello javítás vezénylési alkalmazáshoz tartozó naplók összegyűjtését helyileg az egyes fürtcsomópontok hello.
Azt javasoljuk, hogy konfigurálja-e Azure Diagnostics tooupload naplók minden csomópont tooa központi helyről.

Azure Diagnostics engedélyezésével kapcsolatos információkért lásd: [Naplógyűjtéshez Azure Diagnostics használatával](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).

Hello javítás vezénylési alkalmazás naplók a következő rögzített szolgáltató azonosítók hello hozhatók létre:

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

A Resource Manager sablon goto `EtwEventSourceProviderConfiguration` szakaszában `WadCfg` , és adja hozzá a következő tételek hello:

```json
  {
    "provider": "e39b723c-590c-4090-abb0-11e3e6616346",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
      "eventDestination": "PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "fc0028ff-bfdc-499f-80dc-ed922c52c5e9",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "24afa313-0d3b-4c7c-b485-1047fd964b60",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "05dc046c-60e9-4ef7-965e-91660adffa68",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  }
```

> [!NOTE]
> Ha a Service Fabric-fürt több csomóponttípusok, akkor az előző szakaszban hello hozzá kell adni az összes hello `WadCfg` szakaszok.

## <a name="download-hello-app-package"></a>Hello csomag letöltése

Hello alkalmazás letöltését hello [letöltése hivatkozásra](https://go.microsoft.com/fwlink/P/?linkid=849590).

## <a name="configure-hello-app"></a>Hello alkalmazás konfigurálása

hello hello javítás vezénylési alkalmazás viselkedése lehet konfigurált toomeet igényeinek. Hello az alapértelmezett érték felülírására történő hello alkalmazás paraméter az alkalmazás létrehozása vagy frissítése során. Alkalmazás paraméterek megadásával megadható `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` vagy `New-ServiceFabricApplication` parancsmagok.

|**A paraméter**        |**Típus**                          | **Részletek**|
|:-|-|-|
|MaxResultsToCache    |Hosszú                              | A Windows Update eredmények, amelyek gyorsítótárazza maximális száma. <br>Alapértelmezett érték 3000 feltéve, hogy a: <br> -Csomópontok száma 20. <br> -A csomópont havonta történik frissítések száma: öt. <br> -Művelet eredmények száma 10 is lehet. <br> -Az elmúlt három hónap hello eredmények kell tárolni. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |TaskApprovalPolicy azt jelzi, amely hello Service Fabric-fürt csomópontjai között hello koordinátora szolgáltatás tooinstall Windows-frissítések által használt toobe hello házirend.<br>                         Engedélyezett értékek a következők: <br>                                                           <b>NodeWise</b>. A Windows Update telepítve egy csomópont egyszerre. <br>                                                           <b>UpgradeDomainWise</b>. Windows Update telepítve egy frissítési tartományt egyszerre. (A maximális hello tooan frissítési tartomány tartozó összes hello számítógépen lépjen a Windows Update.)
|LogsDiskQuotaInMB   |Hosszú  <br> (Alapértelmezett: 1024)               |Javítás vezénylési alkalmazás maximális mérete (MB), amely helyileg őrizhető csomópontján naplózza.
| WUQuery               | Karakterlánc<br>(Alapértelmezett: "IsInstalled = 0")                | A lekérdezés tooget Windows-frissítések. További információkért lásd: [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| InstallWindowsOSOnlyUpdates | logikai érték <br> (alapértelmezett: igaz)                 | Ez a jelző lehetővé teszi, hogy a Windows operációs rendszer frissítéseinek toobe telepítve.            |
| WUOperationTimeOutInMinutes | int <br>(Alapértelmezett: 90).                   | A Windows Update művelet (keresési vagy letöltése vagy telepítése) hello időtúllépés megadása. Ha hello művelet nem fejeződött be belül hello megadott időkorlát, akkor végrehajtása megszakadt.       |
| WURescheduleCount     | int <br> (Alapértelmezett: 5).                  | hello maximálisan megengedett számú hello szolgáltatás reschedules hello Windows update abban az esetben, ha egy művelet hiba folyamatosan fennáll.          |
| WURescheduleTimeInMinutes | int <br>(Alapértelmezett: 30). | hello időköz, mely hello szolgáltatás reschedules hello Windows update, ha a probléma továbbra is fennáll. |
| WUFrequency           | Vesszővel elválasztott karakterlánc (alapértelmezett: "Heti, szerda, 7:00:00")     | frissítés a Windows hello gyakorisága. hello formátum és a lehetséges értékek a következők: <br>– Havonta, NN ÓÓ: pp:, például havi, 5, 12: 22:32. <br> -Hetente, nap, formátumban, például hetente, kedd, 12:22:32.  <br> -Napi, óó: pp:, ha például naponta, 12:22:32.  <br> -Nincs azt jelzi, hogy a Windows Update nem végezhető el.  <br><br> Ne feledje, hogy az összes hello idő (UTC).|
| AcceptWindowsUpdateEula | logikai érték <br>(Alapértelmezett: igaz) | Ez a jelző beállításával hello alkalmazás hello végfelhasználói licenc megállapodás a Windows Update hello gép hello tulajdonosa nevében fogad el.              |

> [!TIP]
> Ha azonnal szeretné a Windows Update toohappen, `WUFrequency` relatív toohello alkalmazás telepítési idejét. Tegyük fel, hogy rendelkezik-e olyan öt csomópontból teszt fürt és a terv toodeploy hello app, körülbelül 5:00 PM UTC. Ha azt feltételezi, hogy a hello az alkalmazásfrissítés vagy a központi telepítési: 30 percet vesz igénybe hello maximális, beállíthatja hello WUFrequency "Naponta, 17:30:00."

## <a name="deploy-hello-app"></a>Hello alkalmazás üzembe helyezése

1. Fejezze be az összes hello előzetesen szükséges lépések leírását tooprepare hello fürt.
2. Hello javítás vezénylési alkalmazás, mint bármely más Service Fabric-alkalmazás telepítése. A PowerShell használatával telepítheti hello alkalmazást. Hello kövesse [központi telepítése és törlése alkalmazás PowerShell-lel](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).
3. a központi telepítési, hozzáférési hello hello időpontjában tooconfigure hello alkalmazás `ApplicationParamater` toohello `New-ServiceFabricApplication` parancsmag. Az Ön kényelme érdekében mellékelt hello parancsfájl Deploy.ps1 hello kérelemmel együtt. toouse hello parancsfájlt:

    - Csatlakozás a Service Fabric-fürt tooa segítségével `Connect-ServiceFabricCluster`.
    - Hello PowerShell parancsfájlt Deploy.ps1 megfelelő hello `ApplicationParameter` érték.

> [!NOTE]
> Tartsa hello parancsfájl és hello alkalmazásmappa PatchOrchestrationApplication hello ugyanabban a könyvtárban.

## <a name="upgrade-hello-app"></a>Hello alkalmazás frissítése

PowerShell, a meglévő javítás vezénylési alkalmazás tooupgrade kövesse hello [az alkalmazásfrissítés Service Fabric PowerShell-lel](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).

## <a name="remove-hello-app"></a>Hello alkalmazás eltávolítása

tooremove hello alkalmazás hello lépéseit kövesse [központi telepítése és törlése alkalmazás PowerShell-lel](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).

Az Ön kényelme érdekében mellékelt hello parancsfájl Undeploy.ps1 hello kérelemmel együtt. toouse hello parancsfájlt:

  - Csatlakozás a Service Fabric-fürt tooa segítségével ```Connect-ServiceFabricCluster```.

  - Hello PowerShell-parancsfájl Undeploy.ps1 hajtható végre.

> [!NOTE]
> Tartsa hello parancsfájl és hello alkalmazásmappa PatchOrchestrationApplication hello ugyanabban a könyvtárban.

## <a name="view-hello-windows-update-results"></a>Hello Windows Update eredmények megtekintése

hello javítás vezénylési alkalmazás közzététele a REST API-k toodisplay hello korábbi eredmények toohello felhasználó. Hello eredmény JSON példát:
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of hello issues that are included in this update, see hello associated Microsoft Knowledge Base article. After you install this update, you may have toorestart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```
Ha még nincs frissítés ütemezett, hello JSON eredménye üres.

Jelentkezzen be toohello fürt tooquery Windows Update eredmények. Majd hello replika cím hello elsődleges a hello koordinátor szolgáltatást, majd nyomja le az hello böngészőből hello URL-cím: http://&lt;REPLIKA IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.

hello REST-végpont hello koordinátor Service dinamikus-porttal rendelkezik. toocheck hello pontos URL-t, tekintse meg a Service Fabric Explorer toohello. Például hello eredmények érhetők el `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.

![REST-végpont képe](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Ha hello fordított proxy hello fürt engedélyezve van, hozzáférhet a hello URL-CÍMÉT, valamint a fürt hello kívül.
végpont toobe igénylő hello nyomja le az http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.

tooenable hello fordított proxy hello fürtön kövesse hello [fordított proxy az Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). 

> 
> [!WARNING]
> Hello fordított proxy konfigurálása után minden micro szolgáltatások hello fürt HTTP-végponttal visszaállítását hello fürtön kívüli megcímezhető.

## <a name="diagnosticshealth-events"></a>Diagnosztika/állapotával kapcsolatos események

### <a name="collect-patch-orchestration-app-logs"></a>A gyűjtés javítás vezénylési app naplói

Javítás vezénylési app naplók begyűjti a Service Fabric naplók részeként futásidejű verzióját `5.6.220.9494` vagy újabb verzió.
A Service Fabric futásidejű verzióját futtató fürtök kisebb, mint `5.6.220.9494`, naplók hello a következő módszerek egyikének használatával gyűjthetők össze.

#### <a name="locally-on-each-node"></a>Minden egyes csomóponton helyileg

Naplók összegyűjtését helyileg a Service Fabric-fürt minden csomópontján Ha Service Fabric futásidejű verziószáma kisebb, mint `5.6.220.9494`. hello hely tooaccess hello naplók van \[Service Fabric\_telepítési\_meghajtó\]:\\PatchOrchestrationApplication\\naplókat.

Például akkor, ha a Service Fabric a D meghajtón van telepítve, hello elérési út D:\\PatchOrchestrationApplication\\naplókat.

#### <a name="central-location"></a>Központi hely

Ha Azure Diagnostics előzetesen szükséges lépések leírását részeként van konfigurálva, az Azure Storage hello javítás vezénylési alkalmazás naplók érhetők el.

### <a name="health-reports"></a>Állapotjelentések száma

hello javítás vezénylési app állapotjelentések hello koordinátor szolgáltatást vagy a csomópont ügynökszolgáltatás hello ellen is közzéteszi a következő esetekben hello:

#### <a name="a-windows-update-operation-failed"></a>A Windows Update művelet sikertelen volt

A Windows-frissítési művelet nem sikerül, a csomópont, a jelentés elleni hello csomópont ügynökszolgáltatás jön létre. Hello állapotjelentése részleteit tartalmazzák hello problematikus csomópont nevét.

Miután javítását hello problematikus csomóponton sikeresen befejeződött, a rendszer automatikusan törli a hello jelentés.

#### <a name="hello-node-agent-ntservice-is-down"></a>hello csomópont ügynök NTService nem működik

Csomópont ügynök NTService hello szolgáltatás leállt csomóponton található, a figyelmeztetési szintű állapotjelentése elleni hello csomópont ügynökszolgáltatás jön létre.

#### <a name="hello-repair-manager-service-is-not-enabled"></a>hello manager szolgáltatás nincs engedélyezve

Hello manager szolgáltatás nem található a hello fürt, hello koordinátor szolgáltatást a figyelmeztetési szintű állapotjelentése jön létre.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

Q. **Miért látom azt a fürt állapota hello javítás vezénylési alkalmazás futtatásakor?**

A. Hello telepítési folyamat során hello javítás vezénylési app letiltja, vagy újraindítja a csomópontot, ideiglenesen is hello fürt hello állapotának ennek eredményeként.

Hello alkalmazás hello házirend alapján, vagy több csomópont is leáll a javítási művelet során *vagy* teljes frissítési tartományok egyidejűleg is leáll.

Hello telepítése Windows hello végére csomópontok újra vannak engedélyezve hello utáni újraindítás.

A következő példa hello hello fürt hiba tooan hibaállapot ideiglenesen mert két csomópont le volt, és MaxPercentageUnhealthNodes házirend hello megsértettek. hello hiba ideiglenes, amíg a javítás művelet hello folyamatban.

![A nem megfelelő fürt képe](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Ha hello a probléma továbbra is fennáll, nézze meg a toohello hibaelhárítás részt.

Q. **Javítás vezénylési app figyelmeztetési állapotban van.**

A. Toosee ellenőrizze, hogy egy feladott hello alkalmazás állapotjelentése hello alapvető okát. Általában a hello figyelmeztetés hello probléma részleteit tartalmazza. Hello a hiba csak átmeneti, hello alkalmazás esetén ebből az állapotból várt tooauto-helyreállítás.

Q. **Mi a teendő, ha a fürt állapota nem megfelelő, és toodo sürgős operációs rendszer frissítésre van szükség?**

A. hello javítás vezénylési app nem telepíti a frissítések hello fürt pedig a nem megfelelő. Próbálja meg a fürt tooa állapota kifogástalan toounblock hello javítás vezénylési app munkafolyamat toobring.

Q. **Miért több fürtjére kiterjedő javítását valóban túl sokáig toorun?**

A. hello hello javítás vezénylési alkalmazás által igényelt ideje többnyire hello a következő tényezőktől függ:

- hello házirendjében hello koordinátor szolgáltatást. 
  - alapértelmezett házirend hello `NodeWise`, egyszerre csak egy csomópont javítását eredményez. Különösen a nagyobb fürtök hello esetben azt javasoljuk, hogy használja-e hello `UpgradeDomainWise` házirend tooachieve gyorsabb több fürtjére kiterjedő javítását.
- a letöltés és telepítés elérhető frissítések hello száma. 
- hello szükséges átlagos idő toodownload, és telepítse a frissítést, meg nem haladó néhány óra múlva.
- virtuális gép és a hálózati sávszélesség hello hello teljesítményét.

Q. **Miért látom néhány frissítést, a Windows Update eredmények REST api használatával, de nem a számítógépen a Windows Update előzmények hello?**

A. Egyes frissítéseket kell toobe be van jelölve, a megfelelő frissítési/javítás az előzményekben. Példa: A Windows Defender frissítések nem jelennek meg a Windows Server 2016-os Windows Update előzmények.

## <a name="disclaimers"></a>Felelősséget kizáró nyilatkozatok

- hello javítás vezénylési app elfogadja a végfelhasználói licenc megállapodás a Windows Update hello hello felhasználó nevében. Másik lehetőségként hello beállítást is ki kell kapcsolni hello konfigurációban hello alkalmazás.

- hello javítás vezénylési app telemetriai tootrack használati és teljesítményadatokat gyűjt. hello application telemetria követi hello beállítás hello Service Fabric futásidejű telemetriai beállítás (amely alapértelmezés szerint be van).

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="a-node-is-not-coming-back-tooup-state"></a>A csomópont nem érkezik vissza tooup állapota

**hello csomópont Beragadt letiltása állapotban mert**:

A biztonsági ellenőrzés folyamatban. tooremedy ebben az esetben győződjön meg arról, hogy elegendő csomópontok találhatók állapota kifogástalan.

**hello csomópont Beragadt letiltott állapotban, mert**:

- hello csomópont manuálisan le lett tiltva.
- hello csomópont tooan Azure infrastruktúra folyamatban lévő feladat miatt le lett tiltva.
- hello csomópont ideiglenesen letiltotta hello javítás vezénylési app toopatch hello csomópont.

**hello csomópont Beragadt lefelé állapotban mert**:

- hello csomópont manuálisan lefelé állapotba helyezték.
- Újraindítás (ami előfordulhat, hogy a hello javítás vezénylési alkalmazás által indított) hello csomópont alatt állnak.
- hello csomópont tooa hibás VM vagy a gép vagy a hálózati csatlakozási problémák miatt nem működik.

### <a name="updates-were-skipped-on-some-nodes"></a>Frissítések kihagyta egyes csomópontokon

hello javítás vezénylési app megpróbál tooinstall Windows update függően toohello átütemezése házirend. hello szolgáltatás megpróbál toorecover hello csomópont, és hagyja ki a hello frissítés függően toohello alkalmazás-házirendhez.

Ebben az esetben a figyelmeztetési szintű állapotjelentése elleni hello csomópont ügynökszolgáltatás jön létre. a Windows Update hello eredménye hello hello hiba lehetséges okát is tartalmaz.

### <a name="hello-health-of-hello-cluster-goes-tooerror-while-hello-update-installs"></a>hello fürt hello állapotának tooerror kerül, amíg hello frissítés telepítése

A hibás Windows update leállíthatja egy alkalmazás vagy a fürt egy adott csomópont vagy a frissítési tartomány hello állapotát. hello javítás vezénylési app bármely későbbi Windows-frissítési művelet megszünteti az addig, amíg hello fürt újra nem működik megfelelően.

A rendszergazda beavatkozásra, és meg kell meghatározni, miért hello alkalmazás vagy a fürt nem kifogástalanná válásának miatt tooWindows frissítés.

## <a name="release-notes-"></a>Kibocsátási megjegyzések:

### <a name="version-110"></a>1.1.0-ás verzió
- Nyilvános kiadás

### <a name="version-111"></a>1.1.1 verziója
- Programhiba rögzített SetupEntryPoint a NodeAgentService, amely megakadályozta a NodeAgentNTService telepítését.

### <a name="version-120-latest"></a>1.2.0 verzió (legújabb)

- Hibajavítások körül rendszer indítsa újra a munkafolyamatot.
- Hibajavítás miatt a helyreállítási feladatok előkészítése során az állapot-ellenőrzéssel toowhich RM feladatok létrehozása a várt módon nem történik.
- Windows-szolgáltatás POANodeSvc automatikus toodelayed-automatikus módosított hello indítási módját.
