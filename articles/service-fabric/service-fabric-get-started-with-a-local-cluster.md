---
title: "aaaDeploy, és végezze el az Azure mikroszolgáltatások helyileg |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset be egy helyi Service Fabric-fürt egy meglévő alkalmazás tooit, és frissítse az alkalmazást."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi;mikhegn
ms.openlocfilehash: e5f5adc9edb71433b2a7635e9d661ff92a4b18ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>A helyi fürtön lévő alkalmazások üzembe helyezésének és frissítésének elsajátítása
hello Azure Service Fabric SDK teljes helyi fejlesztőkörnyezetet, melyekkel tartalmaz tooquickly telepítése és kezelése a helyi fürtön lévő alkalmazások első lépések. Ebben a cikkben létrehoz egy helyi fürtöt, központi telepítése egy meglévő alkalmazás tooit, és frissítse az adott tooa új verziója, mindezt a Windows PowerShell.

> [!NOTE]
> A cikk feltételezi, hogy Ön már [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).
> 
> 

## <a name="create-a-local-cluster"></a>Helyi fürt létrehozása
A Service Fabric-fürt olyan hardver-erőforrások készletét képviseli, amelyeken alkalmazásokat helyezhet üzembe. Általában egy fürt áll bárhol az öt toomany több ezer gépek. Service Fabric SDK hello azonban egy gépen futtatható fürtkonfiguráció tartalmaz.

Fontos toounderstand, amely hello Service Fabric helyi fürtje nem emulátor vagy szimulátor. Az futtatása hello ugyanazt a platformkódot is megtalálható a többgépes fürtök. hello egyetlen különbség, hogy fut-e hello platform folyamatok, amelyek általában öt számítógép egy számítógépen vannak elosztva.

hello SDK biztosít a helyi fürt két módon tooset: a Windows PowerShell parancsfájl és hello Local Cluster Manager rendszertálca alkalmazásban. Ebben az oktatóanyagban hello PowerShell-parancsfájlt használjuk.

> [!NOTE]
> Ha a Visual Studióból egy alkalmazás üzembe helyezésével már létrehozott egy a helyi fürtöt, ezt a szakaszt kihagyhatja.
> 
> 

1. Nyisson meg egy új PowerShell-ablakot rendszergazdaként.
2. Futtassa a fürtbeállítási parancsfájlt hello hello SDK mappából:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    A fürt beállítása hosszabb időt vehet igénybe. A beállítást követően a következőhöz hasonló kimenetnek kell megjelennie:
   
    ![A fürtbeállítás kimenete][cluster-setup-success]
   
    Most már áll készen tootry alkalmazás tooyour fürt telepítése.

## <a name="deploy-an-application"></a>Alkalmazás üzembe helyezése
Service Fabric SDK hello alkalmazások létrehozásához szükséges keretrendszerek és fejlesztőeszközök gazdag készletét tartalmazza. Ha érdekli, hogyan toocreate alkalmazásokat a Visual Studio: tanulási [az első Service Fabric-alkalmazás létrehozása a Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

Ebben az oktatóanyagban használhat egy meglévő mintaalkalmazást (a neve WordCount), hogy hello kezelést a hello platform összpontosíthat: telepítés, a figyelés és a frissítés.

1. Nyisson meg egy új PowerShell-ablakot rendszergazdaként.
2. Hello Service Fabric SDK PowerShell modul importálásához.
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. Hozzon létre egy könyvtár toostore hello alkalmazás letöltése és telepítése, például: C:\ServiceFabric.
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. [Töltse le a WordCount alkalmazás hello](http://aka.ms/servicefabric-wordcountapp) létrehozott toohello helyre.  Megjegyzés: hello Microsoft Edge böngésző menti hello fájlt egy *.zip* bővítmény.  Hello fájlkiterjesztés túl módosítása*.sfpkg*.
5. Csatlakozás a helyi fürt toohello:
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. Hozzon létre egy új alkalmazást hello SDK telepítési parancs használata egy nevet és egy elérési utat toohello alkalmazáscsomagot.
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    Ha minden megfelelően működik, a következő kimeneti hello kell megjelennie:
   
    ![Az alkalmazás toohello helyi fürt központi telepítése][deploy-app-to-local-cluster]
7. a művelet toosee hello alkalmazás hello böngésző indítása, és keresse meg a túl[http://localhost: 8081/WordCount/index.HTML](http://localhost:8081/wordcount/index.html). A következőnek kell megjelennie:
   
    ![Az üzembe helyezett alkalmazás felhasználói felülete][deployed-app-ui]
   
    hello WordCount alkalmazás egyszerű. Ez magában foglalja az ügyféloldali JavaScript code toogenerate véletlenszerű öt karakterből álló "szavak", amely majd továbbítódnak toohello alkalmazás ASP.NET Web API-n keresztül. Az állapotalapú szolgáltatás nyomon követi az hello számlált szavak száma. Ezek particionáltak hello hello szó első karaktere alapján. Hello forráskód hello WordCount alkalmazás hello található [klasszikus bevezetés minták](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).
   
    hello üzembe helyezett alkalmazás négy partíciót tartalmaz. Így az A – G kezdetű szavak hello első partíció vannak tárolva, H – N kezdetű szavak vannak tárolva hello második partíció, és így tovább.

## <a name="view-application-details-and-status"></a>Az alkalmazás részleteinek és állapotának megtekintése
Most, hogy a Microsoft hello alkalmazást telepített, vizsgáljuk meg néhány hello alkalmazás adatait a PowerShellben.

1. Hello fürt összes üzembe helyezett alkalmazásának lekérdezése:
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    Feltételezve, hogy csak telepített hello WordCount alkalmazást, akkor valami hasonló:
   
    ![Az összes üzembe helyezett alkalmazás lekérdezése a PowerShellben][ps-getsfapp]
2. Nyissa meg a toohello új szintre hello WordCount alkalmazásban szereplő hello készlete lekérdezésével.
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![PowerShell hello alkalmazáshoz használható szolgáltatások listája][ps-getsfsvc]
   
    hello alkalmazás áll két szolgáltatást, hello előtér-webkiszolgáló és hello állapotalapú szolgáltatásból, amely hello szavakat kezeli.
3. Végül tekintse meg hello partícióinak listáját a WordCountService:
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![Hello szolgáltatáspartíciók megtekintése a PowerShellben][ps-getsfpartitions]
   
    hello használt, például az összes Service Fabric PowerShell-parancsok parancsokat, érhetők el, hogy előfordulhat, hogy kapcsolódni, helyi vagy távoli fürthöz.
   
    Egy vizuálisabban módon toointeract hello fürttel, használhatja hello webalapú Service Fabric Explorer eszközt túl navigálva[19080/Explorer](http://localhost:19080/Explorer) hello böngészőben.
   
    ![Az alkalmazás részleteinek megtekintése a Service Fabric Explorerben][sfx-service-overview]
   
   > [!NOTE]
   > További információ a Service Fabric Explorer toolearn lásd [a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
   > 
   > 

## <a name="upgrade-an-application"></a>Alkalmazás frissítése
A Service Fabric állásidő nélküli frissítéseket biztosít a hello hello alkalmazás állapotának figyelésével, miközben megtörténik a bevezetése hello fürtön. Hajtsa végre a hello WordCount alkalmazás frissítését.

most már a hello alkalmazás új verziójának hello száma csak a magánhangzóval kezdődő szavakat. Hello frissítés bevezeti, mivel két változást hello alkalmazás viselkedését látható. Első lépésként hello sebesség, amellyel hello számok növekedésének le kell lassulnia, mivel kevesebb szót kell megszámolni. Második mivel hello első partíció két magánhangzót tartalmaz (A és E), és az összes többi partíció mindegyike csak egyet, a számláló végül elindul toooutpace hello mások.

1. [Hello WordCount 2-es csomag](http://aka.ms/servicefabric-wordcountappv2) toohello hello az 1-es csomag kezelőportálon ugyanazon a helyen.
2. Térjen vissza a tooyour PowerShell-ablakot, és hello SDK frissítési parancsával tooregister hello hello fürt új verziót használja. Majd lásson hello fabric: / WordCount alkalmazás.
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    Meg kell jelennie a PowerShell kimeneti, a frissítés hello hello következő kezdődik.
   
    ![Folyamatban lévő frissítés a PowerShellben][ps-appupgradeprogress]
3. Míg hello frissítési eljárás, előfordulhat, hogy találja könnyebb toomonitor annak állapotát a Service Fabric Explorerből. Indítsa el a böngészőablakot, és keresse meg a túl[19080/Explorer](http://localhost:19080/Explorer). Bontsa ki a **alkalmazások** hello bal oldali fában hello, majd válassza a **WordCount**, és végül **fabric: / WordCount**. Hello alapvető erőforrások lapon látható hello állapot hello frissítési tartományain keresztül hello fürt frissítési tartományt.
   
    ![A frissítés folyamata a Service Fabric Explorerben][sfx-upgradeprogress]
   
    Hello frissítés előrehalad az egyes tartományokon keresztül, állapot-ellenőrzési eredményeire során a rendszer elvégezte tooensure, amely hello alkalmazás megfelelően működik-e.
4. Ha Újrafuttatja hello korábbi lekérdezése a szolgáltatások hello háló hello készlete: / WordCount alkalmazás, figyelje meg, hogy hello WordCountService verziója megváltozott, de hello wordcountwebservice verziója verziója nem történt meg:
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Az alkalmazásszolgáltatások lekérdezése frissítés után][ps-getsfsvc-postupgrade]
   
    Ez a példa rávilágít arra, hogyan kezeli a Service Fabric az alkalmazásfrissítéseket. Csak hello beállítása (vagy a kód, illetve konfigurációs csomagokat ezekbe a szolgáltatásokba belül), amelyek megváltoztak, ezáltal gyorsabb és megbízhatóbb frissítésének hello folyamata érinti.
5. Végül térjen vissza a toohello böngésző tooobserve hello viselkedését hello új verziója. Elvárás lassabban hello száma történik, és hello első partíció valamivel nagyobb hello kötet fejeződik be.
   
    ![Nézet hello alkalmazás új verzióját hello hello böngészőben][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Takarítás
Lezárása előtt fontos, hogy a helyi fürt hello tooremember valós. Alkalmazások továbbra is toorun hello háttérben, amíg el nem távolítja azokat.  Az alkalmazások hello természetétől függően a futó alkalmazások jelentős erőforrásokat a számítógépen is eltarthat. Több beállítások toomanage alkalmazások és hello fürt rendelkezik:

1. tooremove az egyes alkalmazások és az összes azt tartozó adatok, futtassa a következő parancs hello:
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    Vagy hello alkalmazás törlése a Service Fabric Explorer hello **műveletek** vagy hello helyi menüjének hello bal alkalmazás listanézetben.
   
    ![Alkalmazás törlése a Service Fabric Explorerrel][sfe-delete-application]
2. Ha töröl hello alkalmazás hello fürtből, regisztrációjának törlése 1.0.0 és a WordCount alkalmazás típusának hello 2.0.0 verziói. Törlés hello alkalmazáscsomagok, beleértve hello kód és a konfigurációt, a hello fürt lemezképtárolóhoz törli.
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    Vagy a Service Fabric Explorerben válassza **Unprovision típus** hello alkalmazáshoz.
3. tooshut hello fürt, de tartsa hello alkalmazásadatok és nyomkövetési adatok, kattintson a **helyi fürt leállítása** a hello rendszertálca alkalmazásban.
4. toodelete hello fürt teljesen, kattintson a **helyi fürt eltávolítása** a hello rendszertálca alkalmazásban. Ez a beállítás egy másik lassú üzembe helyezést hello legközelebb a Visual Studióban lenyomja az F5 okozza. Hello helyi fürt eltávolítása csak akkor, ha nem szeretné toouse azt egy kis ideig, vagy ha szüksége van-e tooreclaim erőforrásokat.

## <a name="one-node-and-five-node-cluster-mode"></a>Egycsomópontos és ötcsomópontos fürt üzemmód
Alkalmazások fejlesztésekor gyakran hajtja végre a kódírás, a hibakeresés, a kódmódosítás és a hibakeresés gyors iterációit. toohelp optimalizálása ezt a folyamatot, hello helyi fürt két módban futtatható: egycsomópontos vagy öt csomópontból. Mindkét fürt üzemmódnak megvannak az előnyei. Öt csomópontból álló fürt mód lehetővé teszi egy valódi fürttel toowork. Tesztelheti a feladatátvételi forgatókönyveket, a szolgáltatások több példányát és replikáját használhatja. Optimalizált toodo gyors telepítéshez és nyilvántartási szolgáltatások, gyorsan érvényesítése kód megadása a Service Fabric-futtatókörnyezet hello toohelp egy csomópontos fürtre módja.

Sem az egycsomópontos sem az ötcsomópontos fürt üzemmód nem emulátor vagy szimulátor. hello helyi fejlesztési fürtök futtatásakor hello ugyanazt a platformkódot is megtalálható a többgépes fürtök.

> [!WARNING]
> Hello fürt mód módosításakor a jelenlegi fürthöz hello eltávolítják a rendszerből, és egy új fürt létrehozása. hello fürt hello adat törlődik a fürt mód módosításakor.
> 
> 

toochange hello mód tooone csomópontos fürt esetén jelölje be **kapcsoló fürt mód** a Service Fabric Local Cluster Manager hello.

![Fürt üzemmód átkapcsolása][switch-cluster-mode]

Vagy hello fürt módváltás PowerShell használatával:

1. Nyisson meg egy új PowerShell-ablakot rendszergazdaként.
2. Futtassa a fürtbeállítási parancsfájlt hello hello SDK mappából:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    A fürt beállítása hosszabb időt vehet igénybe. A beállítást követően a következőhöz hasonló kimenetnek kell megjelennie:
   
    ![A fürtbeállítás kimenete][cluster-setup-success-1-node]

## <a name="next-steps"></a>Következő lépések
* Most, hogy már üzembe helyezett és frissített néhány előre létrehozott alkalmazást, [megpróbálhatja felépíteni a saját alkalmazását a Visual Studióban](service-fabric-create-your-first-application-in-visual-studio.md).
* Ebben a cikkben hello helyi fürtön végzett összes hello műveletet lehetne végrehajtani egy [Azure-fürttel](service-fabric-cluster-creation-via-portal.md) is.
* Ebben a cikkben végrehajtott frissítés hello alapszintű volt. Lásd: hello [frissítési dokumentációjában](service-fabric-application-upgrade.md) toolearn hello hatékonyságát és rugalmasságát Service Fabric frissítéskezelésének többet.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
