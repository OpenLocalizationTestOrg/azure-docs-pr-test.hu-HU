---
title: "egy önálló Azure Service Fabric-fürt aaaCreate |} Microsoft Docs"
description: "Az Azure Service Fabric-fürt létrehozása (fizikai vagy virtuális) bármely gépen Windows Server rendszert futtató, hogy helyszíni-e, vagy a felhőben."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 444970816290a0448d88a8b2082c75eb7a64cb44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>A Windows Server rendszert futtató önálló fürt létrehozása
Azure Service Fabric toocreate Service Fabric-fürtök használatát a virtuális gépek vagy a Windows Server rendszerű számítógépek. Ez azt jelenti, telepítése és a Service Fabric-alkalmazások futtatása bármely összekapcsolt Windows Server számítógépek tartalmazó környezetben is kell azt a helyszíni vagy bármely felhőalapú szolgáltatóhoz. A Service Fabric biztosít a Service Fabric-fürtök telepítő csomag toocreate hello önálló Windows Server csomag neve.

Ez a cikk bemutatja, hogyan hello különálló Service Fabric-fürt létrehozásának lépései.

> [!NOTE]
> Csomag különálló Windows Server kereskedelmi forgalomban elérhető, és az üzemi környezetek használható. Ez a csomag a Service Fabric funkciói "Előzetes verziójú" tartalmazhat. Görgessen lefelé, túl"[az előzetes funkciók a csomagban](#previewfeatures_anchor)." szakasz hello előzetes verziójú funkciók hello listáját. Is [hello EULA másolatának letöltése](http://go.microsoft.com/fwlink/?LinkID=733084) most.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-hello-service-fabric-for-windows-server-package"></a>Segítségre van szüksége hello Service Fabric for Windows Server-csomag
* Kérdezzen hello közösségi hello Service Fabric önálló csomag a Windows Server a hello [Azure Service Fabric fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).
* Nyissa meg a jegy [Professional támogatása a Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).  További tudnivalók a Microsoft-támogatást Professional [Itt](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* Is kaphat támogatási csomag részeként [Microsoft Premier támogatási](https://support.microsoft.com/en-us/premier).
* További részletekért lásd: [Azure Service Fabric támogatási lehetőségek](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).
* hello futtassa a támogatáshoz toocollect naplók [Service Fabric önálló naplógyűjtő](service-fabric-cluster-standalone-package-contents.md).

<a id="downloadpackage"></a>

## <a name="download-hello-service-fabric-for-windows-server-package"></a>Hello Service Fabric for Windows Server-csomag
toocreate hello fürt használata hello Service Fabric Windows Server csomag (Windows Server 2012 R2 és újabb) itt található: <br>
[Töltse le a Windows Server - háló önálló szolgáltatáscsomag - hivatkozás](http://go.microsoft.com/fwlink/?LinkId=730690)

Részletek található hello csomagok tartalmának [Itt](service-fabric-cluster-standalone-package-contents.md).

Service Fabric-futtatókörnyezet csomag hello automatikusan lett letöltve a fürt létrehozásakor. Ha telepítése a gép nincs csatlakoztatva a toohello internet, töltse le innen hello futásidejű csomag sávon kívül: <br>
[Töltse le a Windows Server - Service Fabric-futtatókörnyezet - hivatkozás](https://go.microsoft.com/fwlink/?linkid=839354)

Önálló fürtkonfiguráció minták keresése: <br>
[Önálló fürtkonfiguráció – minták](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-hello-cluster"></a>Hello fürt létrehozása
A Service Fabric telepített tooa egy gépi fejlesztési tárolófürt is lehet hello segítségével *ClusterConfig.Unsecure.DevCluster.json* fájl [minták](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

Bontsa ki a hello önálló csomag tooyour gép, a Másolás hello minta konfigurációs fájlt toohello helyi számítógépen, majd a futtatási hello *CreateServiceFabricCluster.ps1* egy rendszergazda PowerShell-munkameneten keresztül, az önálló hello parancsfájl csomag mappájának:
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a>1A. lépés: egy nem biztonságos helyi fejlesztési fürtök létrehozása
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Lásd: hello környezet telepítése szakasz [tervezze meg és készítse elő a fürtöt tartalmazó környezetben](service-fabric-cluster-standalone-deployment-preparation.md) hibaelhárítási részleteket.

Ha végzett a futó fejlesztési forgatókönyvre, eltávolíthatja hello Service Fabric-fürt hello gépről szakaszban toosteps hivatkozással "[távolítsa el a fürt](#removecluster_anchor)". 

### <a name="step-1b-create-a-multi-machine-cluster"></a>1B. lépés: hozzon létre egy többgépes fürtben
Után és a hello tervezés megtettünk mindent, és előkészítő lépések részletes: hello hivatkozáson, kész toocreate az éles fürt a fürt konfigurációs fájl használatával. <br>
[Tervezze meg és készítse elő a fürtöt tartalmazó környezetben](service-fabric-cluster-standalone-deployment-preparation.md)

1. Hello futtatásával írt hello konfigurációs fájl ellenőrzése *TestConfiguration.ps1* parancsfájl hello önálló csomag mappából:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Kimenetet kell látnia, például alatt. Ha hello alsó a "Passed" mezőben adja vissza a rendszer "True", megerősítések átadott és hello fürt toobe telepíthető hello bemeneti konfigurációja alapján keres.

    ```
    Trace folder already exists. Traces will be written tooexisting trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. Hello fürt létrehozása: hello futtatása *CreateServiceFabricCluster.ps1* parancsfájl toodeploy hello Service Fabric-fürt minden gép hello konfiguráció között. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Központi telepítés nyomainak toohello virtuális gép vagy gépek hello CreateServiceFabricCluster.ps1 PowerShell-parancsfájl futtatásának készültek. Ezek a hello almappájában DeploymentTraces, mely hello a parancsfájl futtatásának hello könyvtárban alapú találhatók. toosee, ha a Service Fabric nem a megfelelő telepítést tooa számítógép, telepített hello fájlok található hello FabricDataRoot könyvtárban, ahogy az az hello fürt konfigurációs fájl FabricSettings szakaszban (az alapértelmezett c:\ProgramData\SF). Valamint FabricHost.exe és Fabric.exe folyamatok látható, a Feladatkezelő futtató.
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a>1 c. lépés: kapcsolat nélküli (internet-kapcsolatát) fürt létrehozása
Service Fabric-futtatókörnyezet csomag hello automatikusan letölti a fürt létrehozásakor. Ha nincs központi telepítése egy fürt toomachines csatlakoztatva toohello internet, szüksége lesz a Service Fabric futásidejű külön csomagot, majd adja meg a fürt létrehozásakor hello elérési tooit toodownload hello.
hello futásidejű csomag külön is letölthetők, egy másik gépről csatlakoztatott toohello internet, a [- Service Fabric-futtatókörnyezet - hivatkozás töltse le a Windows Server](https://go.microsoft.com/fwlink/?linkid=839354). Hello futásidejű csomag toowhere hello offline fürtöt telepít, és hello fürt létrehozása futtatásával másolja `CreateServiceFabricCluster.ps1` a hello `-FabricRuntimePackagePath` paraméter szerepel, a lent látható módon: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
Ha `.\ClusterConfig.json` és `.\MicrosoftAzureServiceFabric.cab` hello elérési utak toohello fürtkonfiguráció és hello futásidejű .cab fájl rendre vannak.


### <a name="step-2-connect-toohello-cluster"></a>2. lépés: Csatlakozás toohello fürt
tooconnect tooa biztonságos fürt, lásd: [a Service fabric csatlakoztassa toosecure fürtöt](service-fabric-connect-to-secure-cluster.md).

tooconnect tooan nem biztonságos fürthöz, futtassa a következő PowerShell-paranccsal hello:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
Példa:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a>3. lépés: Elindítani a Service Fabric Explorerrel
Most a Service Fabric Explorer vagy közvetlenül az hello gépek http://localhost:19080/Explorer/index.html vagy távolról http://< egyik kapcsolatba léphet a toohello fürt*IPAddressofaMachine*>: 19080 / Explorer/index.html.

## <a name="add-and-remove-nodes"></a>Hozzáadása és eltávolítása, csomópontok
Adja hozzá, vagy távolítsa el a csomópontok tooyour különálló Service Fabric-fürt, az üzleti igényeinek változását. Lásd: [hozzáadása vagy eltávolítása, csomópontok tooa Service Fabric-fürt önálló](service-fabric-cluster-windows-server-add-remove-nodes.md) a részletes lépéseket.

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>A fürt eltávolítása
tooremove egy fürthöz, futtassa a hello *RemoveServiceFabricCluster.ps1* PowerShell-parancsfájl hello csomag mappából, és adjon át hello elérési toohello JSON-konfigurációs fájlt. Opcionálisan megadhat egy helyet hello napló hello törlését.

Ezt a parancsfájlt, amely rendelkezik rendszergazdai hozzáférési tooall hello gépek csomópontok hello fürt konfigurációs fájlban felsorolt gépi. Ezt a parancsfájlt futtató hello számítógép nem rendelkezik toobe hello fürt része.

```
# Removes Service Fabric from each machine in hello configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from hello current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-tooopt-out-of-it"></a>Az összegyűjtött telemetrikus adatok és hogyan belőle tooopt
Alapértelmezés szerint hello termék hello Service Fabric használati tooimprove hello termék telemetriai adatokat gyűjti. Ajánlott eljárásokat elemző eszköz, amely futtatható, mint egy hello telepítés részeként túl ellenőrzi a kapcsolatot hello[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Ha nem érhető el, hello beállítása sikertelen lesz, kivéve, ha kikapcsolja a telemetriai adatokat.

1. hello telemetria-feldolgozási folyamat megpróbál túl a következő adatok tooupload hello[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) naponta egyszer. A legjobb feltöltés, és nincs hatással van a hello fürt működését. hello telemetriai csak küldi hello csomópont, hogy fut hello Feladatátvevőfürt-kezelő elsődleges. Nincs más csomópontok küldött telemetriai adatokat.
2. hello telemetriai hello következő tevődik össze:

* Szolgáltatások száma
* ServiceTypes száma
* Alkalmazások száma
* ApplicationUpgrades száma
* Failoverunits egységek száma
* Található inbuildfailoverunits egységek száma
* UnhealthyFailoverUnits száma
* Replikák száma
* InBuildReplicas száma
* StandByReplicas száma
* OfflineReplicas száma
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Csomópontok száma
* IsContextComplete: Igaz/hamis értékű
* ClusterId: Ez az egyes fürtök véletlenszerűen előállított GUID
* ServiceFabricVersion
* IP-cím hello virtuális gép vagy a számítógép melyik hello a telemetriai adatok feltöltése

toodisable telemetriai hello következő túl*tulajdonságok* a fürt config: *enableTelemetry: hamis*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Előzetes verziójú funkciók a csomagban
nincs.


> [!NOTE]
> Új hello kezdve [GA verziójával hello önálló fürt Windows Server (verzió 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), a fürt toofuture kiadásokban manuálisan vagy automatikusan frissítheti. Tekintse meg a túl[frissítése egy különálló Service Fabric-fürt verziószáma](service-fabric-cluster-upgrade-windows-server.md) dokumentum.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [Központi telepítése, és távolítsa el az alkalmazásokat a PowerShell használatával](service-fabric-deploy-remove-applications.md)
* [Önálló Windows-fürt konfigurációs beállításai](service-fabric-cluster-manifest.md)
* [Hozzáadása vagy eltávolítása, csomópontok tooa különálló Service Fabric-fürt](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Frissítse a különálló Service Fabric-fürt verziószáma](service-fabric-cluster-upgrade-windows-server.md)
* [Hozzon létre egy különálló Service Fabric-fürt Windowst futtató Azure virtuális gépeken](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Biztonságos Windows használja-e a Windows biztonsági önálló fürtben](service-fabric-windows-cluster-windows-security.md)
* [Biztonságos Windows X509 használata önálló fürtben, tanúsítványok](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
