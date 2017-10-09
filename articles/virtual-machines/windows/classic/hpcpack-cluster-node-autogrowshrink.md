---
title: "HPC Pack fürtcsomópontok aaaAutoscale |} Microsoft Docs"
description: "Automatikusan növelhető, vagy csökkenthető a számítási fürtcsomópontok HPC Pack az Azure-ban hello számára"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0bdf55625d337a2bbfe05677682d645a584798d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-grow-and-shrink-hello-hpc-pack-cluster-resources-in-azure-according-toohello-cluster-workload"></a>Automatikus növekedésének és zsugorítása hello HPC Pack fürterőforrások toohello fürtmunkaterhelés szerint az Azure-ban
Ha Azure "kapacitásnövelés" csomópontok HPC Pack fürt központi telepítését, vagy az Azure virtuális gépeken HPC Pack-fürtöt hoz létre, érdemes lehet egy módszerre, amellyel automatikusan növekedhet és zsugorítása hello fürt erőforrások, például a csomópontok vagy magok hello fürtön hello munkaterhelés szerint. Ily módon hello fürterőforrások skálázás lehetővé teszi a toouse az Azure-erőforrások hatékonyabban és azok kapcsolatos költségek szabályozását.

Ez a cikk bemutatja, amely HPC Pack tooautoscale számítási erőforrásokat biztosít két módon:

* HPC Pack fürt tulajdonság hello **AutoGrowShrink**

* Hello **AzureAutoGrowShrink.ps1** HPC PowerShell-parancsfájl

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Jelenleg akkor csak automatikusan növelhető vagy csökkenthető a Windows Server operációs rendszert futtató HPC Pack számítási csomópontok.


## <a name="set-hello-autogrowshrink-cluster-property"></a>Hello AutoGrowShrink fürt tulajdonságának beállítása
### <a name="prerequisites"></a>Előfeltételek

* **HPC Pack 2012 R2 Update 2 vagy újabb rendszerű fürt** -hello átjárócsomóponthoz lehet telepítve a helyi vagy egy Azure virtuális gép. Lásd: [HPC Pack hibrid fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget elindította egy helyszíni átjárócsomópont és az Azure "kapacitásnövelés" csomópont. Lásd: hello [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) tooquickly HPC Pack-fürt üzembe helyezése az Azure virtuális gépeken.

* **Az Azure (Resource Manager üzembe helyezési modellben) központi csomóponton fürt** - HPC Pack 2016-től kezdődően az Azure Active Directory alkalmazásban tanúsítványhitelesítés szolgál automatikusan növekvő és zsugorítását fürt virtuális gépek használatával telepíthetők. Az Azure Resource Manager. Tanúsítvány konfigurálása az alábbiak szerint:

  1. Fürt telepítést követően csatlakozzon a távoli asztal tooone átjárócsomópont.

  2. Hello tanúsítvány (PFX formátumban és a titkos kulcs) tooeach átjárócsomópont feltölteni, majd telepítse tooCert:\LocalMachine\My és a Cert: \LocalMachine\Root.

  3. Indítsa el az Azure Powershellt rendszergazdaként, és futtassa a következő parancsokat egy központi csomóponton hello:

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    Ha a fiók több mint egy Azure Active Directory-bérlő vagy az Azure-előfizetés, hello következő futtathatja tooselect hello megfelelő bérlői és az előfizetés parancsot:
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    Futtassa a következő parancs tooview hello hello jelenleg kijelölt a bérlők és az előfizetés:
    
    ```powershell
        Get-AzureRMContext
    ```

  4. Futtassa a következő parancsfájl hello

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    Ha

    **DisplayName** -Azure Active alkalmazás megjelenítési neve. Ha hello az alkalmazás nem létezik, az Azure Active Directoryban létrejön.

    **Kezdőlap** -hello kezdőlap hello alkalmazás. Beállíthatja, hogy egy üres URL-címet, példa megelőző hello hasonlóan.

    **IdentifierUri** -hello alkalmazás azonosítója. Beállíthatja, hogy egy üres URL-címet, példa megelőző hello hasonlóan.

    **CertificateThumbprint** -hello átjárócsomópont 1. lépésben telepített hello tanúsítvány ujjlenyomata.

    **A TenantId** -Azonosítót az Azure Active Directory-Bérlőazonosítóra. Hello Bérlőazonosító beszerzése hello Azure Active Directory portálon **tulajdonságok** lap.

    Ha szeretne többet megtudni **ConfigARMAutoGrowShrinkCert.ps1**- ben futtassa `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.


* **Az Azure (klasszikus üzembe helyezési modellel) központi csomóponton fürt** – Ha hello HPC Pack IaaS telepítési parancsfájl toocreate hello fürtön hello klasszikus üzembe helyezési modellel, engedélyezése hello **AutoGrowShrink** fürt a tulajdonság által a beállításnak a hello AutoGrowShrink hello fürt konfigurációs fájlban. További részletek a dokumentációban hello hello kísérő [parancsfájl letöltése](https://www.microsoft.com/download/details.aspx?id=44949).

    Azt is megteheti, engedélyezi az hello **AutoGrowShrink** hello a következő szakaszban leírt fürt tulajdonság HPC PowerShell használatával hello fürt telepítése után a parancsokat. a tooprepare, első teljes hello a következő lépéseket:

  1. Az Azure felügyeleti tanúsítványt hello átjárócsomópont és a hello Azure-előfizetés konfigurálása Teszt üzembe helyezés esetén hello Microsoft HPC Azure alapértelmezett önaláírt tanúsítványt használjon, HPC Pack hello átjárócsomópont telepít, és majd töltse fel az adott tanúsítvány tooyour Azure-előfizetés. A beállítások és lépéseivel kapcsolatban lásd: hello [TechNet Library útmutatást](https://technet.microsoft.com/library/gg481759.aspx).

  2. Futtatás **regedit** csomóponton hello központi, nyissa meg tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, és adjon hozzá egy karakterláncértéket. Hello Azonosítónév túl beállítása "Ujjlenyomata", és hello tanúsítvány az 1 érték adatok toohello ujjlenyomata.

### <a name="hpc-powershell-commands-tooset-hello-autogrowshrink-property"></a>HPC PowerShell parancsok tooset hello AutoGrowShrink tulajdonság
Az alábbiakban minta HPC PowerShell-parancsok tooset **AutoGrowShrink** és tootune további paramétereket a működését. Lásd: [AutoGrowShrink paraméterek](#AutoGrowShrink-parameters) hello beállítások teljes listáját az ebben a cikkben később.

toorun ezeket a parancsokat, indítsa el a HPC PowerShell fürtcsomóponton hello központi rendszergazdaként.

**tooenable hello AutoGrowShrink tulajdonság**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

**toodisable hello AutoGrowShrink tulajdonság**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

**toochange hello nő időköz percben**

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

**toochange hello zsugorítása időköz percben**

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

**tooview hello aktuális konfigurációja AutoGrowShrink**

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

**AutoGrowShrink csoportjait tooexclude csomópont**

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> Ez a paraméter támogatott HPC Pack 2016 indítása
>

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink paraméterek
hello következő AutoGrowShrink paraméterek tartoznak, amelyek még módosíthatók hello segítségével **Set-HpcClusterProperty** parancsot.

* **EnableGrowShrink** - tooenable kapcsolóhoz, vagy tiltsa le a hello **AutoGrowShrink** tulajdonság.
* **ParamSweepTasksPerCore** -paraméteres ismétlés száma toogrow egy core feladatok. hello alapértelmezés szerint egy alapvető toogrow feladat.

  > [!NOTE]
  > HPC Pack QFE KB3134307 módosítások **ParamSweepTasksPerCore** túl**TasksPerResourceUnit**. Hello feladat erőforrástípus alapul, és csomópont, szoftvercsatorna vagy core lehet.
  >
  >
* **GrowThreshold** -aszinkron feladatok tootrigger automatikus növekedési küszöbértéket. hello alapértelmezett érték 1, ami azt jelenti, hogy ha az 1, vagy további feladatokat hello aszinkron állapot, a csomópontok méretének automatikus növelése.
* **GrowInterval** -időköz percben tootrigger automatikus növekedési. hello alapértelmezett érték 5 perc.
* **ShrinkInterval** -időköz percben tootrigger automatikus zsugorítását. hello alapértelmezett érték 5 perc. |}
* **ShrinkIdleTimes** -folyamatos ellenőrzés tooshrink tooindicate hello csomópontok száma üresjáratban. hello alapértelmezett érték 3-szor. Például, ha hello **ShrinkInterval** 5 perc, a HPC Pack ellenőrzi, hogy hello csomópont tétlen 5 percenként. Ha hello csomópontok hello üresjárati állapotban után 3 folyamatos ellenőrzi (15 perc), majd HPC Pack zsugorítja csomóponton.
* **ExtraNodesGrowRatio** -csomópontok toogrow Message Passing Interface (MPI) feladatok további százalékát. hello alapértelmezett értéke 1, ami azt jelenti, hogy HPC Pack növekedésének csomópontok MPI-feladatok 1 %.
* **GrowByMin** -kapcsoló tooindicate, hogy hello automatikus növekedésre házirend hello hello feladat szükséges minimális erőforrásokat alapul. hello alapértelmezett értéke false, ami azt jelenti, hogy a HPC Pack feladatok hello maximális hello feladatok szükséges erőforrásokat alapján-csomópont növekszik.
* **SoaJobGrowThreshold** -küszöbértéket bejövő SOA kérelmek tootrigger hello automatikus növelési folyamat. hello alapértelmezett értéke 50000.

  > [!NOTE]
  > Ez a paraméter a HPC Pack 2012 R2 Update 3 verziójától kezdve alkalmazható.
  >
  >
* **SoaRequestsPerCore** -kérelmek toogrow egy alapvető bejövő SOA száma. hello alapértelmezett értéke 20000.

  > [!NOTE]
  > Ez a paraméter a HPC Pack 2012 R2 Update 3 verziójától kezdve alkalmazható.
  >
* **ExcludeNodeGroups** – hello csomópontjának megadott munkaterület-csoportok nem automatikusan növelhető vagy csökkenthető.
  
  > [!NOTE]
  > Ez a paraméter HPC Pack 2016 verziójától kezdve alkalmazható.
  >

### <a name="mpi-example"></a>MPI – példa
Alapértelmezés szerint a HPC Pack 1 % növekedésének MPI-feladatok extra csomópontok (**ExtraNodesGrowRatio** too1 van beállítva). hello oka az, hogy MPI több csomópont lehet szükség, és hello feladat csak futtatható, amikor készen áll az összes csomópont. Ha Azure csomópontok elindul, időnként egy csomópont több idő toostart, mint a többire, más csomópontok toobe üresjárati, miközben az adott csomópont tooget készen áll, amely lehet szükség. További csomópontokat karcsúsításával HPC Pack csökkenti a erőforrás várakozási időt, és potenciálisan menti a költségek. MPI-feladatok (például too10 %), további csomópontokat tooincrease hello százaléka hasonló parancs futtatása

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-példa
Alapértelmezés szerint **SoaJobGrowThreshold** too50000 beállítása és **SoaRequestsPerCore** too200000 van beállítva. Ha egy SOA-feladatok 70000 kérések, egy sorban álló feladat és bejövő kérelmek 70000. Ebben az esetben HPC Pack növekszik-e 1 core hello aszinkron feladatot, és a bejövő kéréseket, nő a (70000-50000) / 20000 = 1 alapvető, így a teljes növekszik a SOA-feladatok 2 magos.

## <a name="run-hello-azureautogrowshrinkps1-script"></a>Hello AzureAutoGrowShrink.ps1 parancsfájl futtatása
### <a name="prerequisites"></a>Előfeltételek

* **HPC Pack 2012 R2 Update 1 vagy újabb rendszerű fürt** – hello **AzureAutoGrowShrink.ps1** parancsfájl hello % CCP_HOME % bin mappájában telepítve van. hello átjárócsomóponthoz lehet telepítve a helyi vagy egy Azure virtuális gép. Lásd: [HPC Pack hibrid fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget elindította egy helyszíni átjárócsomópont és az Azure "kapacitásnövelés" csomópont. Lásd: hello [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) tooquickly HPC Pack-fürt üzembe helyezése az Azure virtuális gépeken, vagy használjon egy [Azure gyors üzembe helyezés sablon](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).
* **Az Azure PowerShell 1.4.0** -hello parancsfájl jelenleg ezt a verziót az Azure PowerShell függ.
* **A fürt az Azure-kapacitásnövelés csomópontok** -hello parancsfájlt, vagy egy ügyfélszámítógépen, amelyen telepítve van-e az HPC Pack hello átjárócsomópont futtassa. Ha egy ügyfélszámítógépen fut, győződjön meg arról, beállított hello változó $env: CCP_SCHEDULER toopoint toohello átjárócsomópont. hello Azure "kapacitásnövelés" csomópontok toohello fürt hozzá kell adni, de lehet, hogy azok hello állapot nincs telepítve.
* **Az Azure virtuális gépeken (Resource Manager üzembe helyezési modellben) telepített fürt** -hello Resource Manager üzembe helyezési modellben telepített Azure virtuális gépek a fürt hello parancsfájl az Azure authentication két módszert támogat: Jelentkezzen be Azure-fiók tooyour toorun hello parancsfájl minden alkalommal (futtatásával `Login-AzureRmAccount`, vagy a szolgáltatás egyszerű tooauthenticate konfigurálása tanúsítvánnyal. HPC Pack hello parancsfájlt tartalmaz **ConfigARMAutoGrowShrinkCert.ps** toocreate tanúsítvánnyal egy egyszerű szolgáltatást. hello parancsfájl létrehoz egy Azure Active Directory (Azure AD) alkalmazás és egy egyszerű szolgáltatást, és hozzárendeli a hello közreműködői szerepkör toohello egyszerű. toorun hello parancsfájl, indítsa el az Azure Powershellt rendszergazdaként, és futtassa a következő parancsok hello:

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    Ha szeretne többet megtudni **ConfigARMAutoGrowShrinkCert.ps1**- ben futtassa `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,

* **Az Azure virtuális gépeken (klasszikus üzembe helyezési modellel) telepített fürt** -parancsprogrammal hello hello központi csomóponton virtuális gép, mert hello előfeltétel **Start-HpcIaaSNode.ps1** és **Stop-HpcIaaSNode.ps1**parancsfájlok, hogy telepítve vannak. A parancsfájlok továbbá egy Azure felügyeleti tanúsítvánnyal kell rendelkezniük, vagy közzététele beállításfájl (lásd: [kezelése számítási csomópontok HPC csomagban fürtön, az Azure-ban](hpcpack-cluster-node-manage.md)). Győződjön meg arról, hogy minden számítási csomópont virtuális gépek kell már felvette az toohello fürt hello. A hello Leállítva állapotú lehet.



### <a name="syntax"></a>Szintaxis
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a>Paraméterek
* **NodeTemplates** -hello csomópont sablonok toodefine nevei hello csomópontok toogrow hatóköre hello vagy csökkenthető. Ha nincs megadva (hello alapértelmezett érték: @()), hello összes csomópontjának **AzureNodes** csomópontcsoport, a hatókör mikor **NodeType** értéke AzureNodes, és minden csomópont a hello **ComputeNodes** csomópont csoport, a hatókör mikor **NodeType** ComputeNodes értéke.
* **JobTemplates** -hello nevei sablonok toodefine hello hatókör hello csomópontok toogrow feladat.
* **A NodeType** - csomópont toogrow típusú hello vagy csökkenthető. Támogatott értékek a következők:

  * **AzureNodes** – Azure PaaS (kapacitásnövelés) csomópontján egy a helyszíni vagy Azure IaaS-fürt.
  * **ComputeNodes** - a számítási csomópont virtuális gépek csak Azure IaaS-fürtben lévő.

* **NumOfQueuedJobsPerNodeToGrow** -aszinkron feladatok száma szükséges toogrow egy csomópont.
* **NumOfQueuedJobsToGrowThreshold** -aszinkron feladatok toostart hello hello küszöbérték száma nő folyamat.
* **NumOfActiveQueuedTasksPerNodeToGrow** -hello aktív aszinkron feladatok száma szükséges toogrow egy csomópont. Ha **NumOfQueuedJobsPerNodeToGrow** van megadva 0,-nál nagyobb értéket a rendszer figyelmen kívül hagyja ezt a paramétert.
* **NumOfActiveQueuedTasksToGrowThreshold** -aktív aszinkron feladatok toostart hello hello küszöbérték száma nő folyamat.
* **NumOfInitialNodesToGrow** – hello kezdeti csomópontok toogrow minimális száma, ha minden hello csomópontra hatókörben van **nem telepített** vagy **leállítva (Deallocated)**.
* **GrowCheckIntervalMins** -hello időköz percben toogrow ellenőrzi.
* **ShrinkCheckIntervalMins** -hello időköz percben tooshrink ellenőrzi.
* **ShrinkCheckIdleTimes** -hello folyamatos zsugorítás ellenőrzések száma (elválasztott **ShrinkCheckIntervalMins**) tooindicate hello csomópontok üresjáratban.
* **UseLastConfigurations** -hello fenti konfiguráció hello argumentum fájlba menti.
* **ArgFile**– hello hello argumentum használt fájl toosave nevét, és frissítse a hello konfigurációk toorun hello parancsfájl.
* **LogFilePrefix** -hello előtag hello naplófájl nevét. Egy elérési utat is megadhat. Alapértelmezés szerint a hello napló az aktuális munkakönyvtárban írásbeli toohello.

### <a name="example-1"></a>1. példa
hello alábbi példa konfigurál hello Azure-kapacitásnövelés csomópontok az alapértelmezett AzureNode sablon toogrow telepítik, és automatikusan csökken. Ha a csomópontok kezdetben hello **nem telepített** állapotba kerül, legalább 3 csomópontok indulnak el. Ha hello aszinkron feladatok száma meghaladja a 8, hello parancsfájl kezdését csomópontok az aszinkron feladatok hello aránya túllépi **NumOfQueuedJobsPerNodeToGrow**. Ha egy csomópont tétlen 3 egymást követő üresjárati idő található toobe, le van állítva.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>2. példa
hello alábbi példa konfigurál hello Azure számítási csomópont virtuális gépek hello alapértelmezett Átjárócsomópontján sablon toogrow telepítik, és automatikusan csökken.
hello alapértelmezett projekt sablon által konfigurált hello feladatok hello fürtön a munkaterhelés hello hatókörének meghatározása. Ha minden hello csomópont kezdetben le van állítva, legalább 5 csomópontok indulnak el. Hello aktív aszinkron feladatok száma meghaladja a 15, ha hello parancsfájl kezdődik-e a csomópontok, amíg számuk meghaladja a aktív aszinkron feladatok hello aránya túl**NumOfActiveQueuedTasksPerNodeToGrow**. Ha egy csomópont tétlen 10 egymást követő üresjárati idő található toobe, le van állítva.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
