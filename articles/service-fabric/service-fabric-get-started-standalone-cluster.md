---
title: "önálló Azure Service Fabric fürt aaaSet |} Microsoft Docs"
description: "Hozzon létre egy fejlesztési önálló fürtöt hello futó három csomópontja ugyanazon a számítógépen. A telepítés befejezése után készen áll a toocreate egy többgépes fürtben fog."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2017
ms.author: dekapur
ms.openlocfilehash: e4d0ea9fc3b8475160bd8ed19fd3716463791cc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a>Az első önálló Service Fabric-fürt létrehozása
Létrehozhat különálló Service Fabric-fürt virtuális gépek vagy Windows Server 2012 R2 vagy Windows Server 2016-ot, a helyszíni rendszerű számítógépeket vagy hello felhőben. A gyors üzembe helyezés segít toocreate fejlesztési önálló fürt csak néhány perc múlva.  Amikor végzett, egy egyetlen számítógépen futó, három csomópontot tartalmazó fürttel fog rendelkezni, amelyre appokat telepíthet.

## <a name="before-you-begin"></a>Előkészületek
A Service Fabric biztosít egy telepítő csomag toocreate Service Fabric-fürtök önálló.  [Hello telepítő csomag](http://go.microsoft.com/fwlink/?LinkId=730690).  Bontsa ki a hello beállítása tooa mappájába hello számítógép vagy virtuális gép ahol beállítja hello fejlesztési fürtöt.  részletesen ismerteti a hello rendszerhez készült hello [Itt](service-fabric-cluster-standalone-package-contents.md).

hello Fürtfelügyelő központi telepítését és konfigurálását hello fürt hello számítógépre rendszergazdai jogosultságokkal kell rendelkeznie. A Service Fabric tartományvezérlőn nem telepíthető.

## <a name="validate-hello-environment"></a>Hello környezet ellenőrzése
Hello *TestConfiguration.ps1* hello önálló csomag parancsfájl szolgál egy ajánlott eljárásokat elemző eszköz toovalidate e fürt telepíthető egy adott környezetben. [Központi telepítés előkészítése](service-fabric-cluster-standalone-deployment-preparation.md) listák hello szükséges előfeltételek és környezeti követelmények. Hello parancsfájl tooverify üzemeltetéséhez hello fejlesztési fürtöt hozhat létre:

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-hello-cluster"></a>Hello fürt létrehozása
Több fürt konfigurációs mintafájlok hello telepítőcsomagját együtt települnek. *ClusterConfig.Unsecure.DevCluster.json* hello legegyszerűbb fürtkonfiguráció van: egy nem biztonságos, három csomópontos fürt egyetlen számítógépen futó.  A többi konfigurációs fájl X.509 tanúsítványokkal vagy a Windows biztonsági szolgáltatásaival védett egy- vagy többgépes fürtöket ír le.  Nem kell toomodify hello alapértelmezett konfigurációs beállításokat a jelen oktatóanyag esetében, de nézze át a hello konfigurációs fájlban, és ismerkedjen meg hello-beállítások.  Hello **csomópontok** a szakasz ismerteti a hello három hello fürt csomópontja: név, IP-cím, [típusú csomópont, a tartalék tartomány és a frissítési tartomány](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  Hello **tulajdonságok** szakasz határozza meg a hello [biztonsági, megbízhatósági szint, diagnosztika gyűjtemény és csomópontok típusú](service-fabric-cluster-manifest.md#cluster-properties) hello fürthöz.

Ez a fürt nem biztonságos.  Bárki csatlakozhat hozzá névtelenül és végrehajthat kezelési műveleteket, ezért az üzemben lévő fürtöket mindig X.509 tanúsítványok vagy a Windows rendszerbiztonság használatával kell védeni.  Biztonsági csak konfigurálni kell a fürt létrehozásának idejét, és már nem lehetséges tooenable biztonsági hello fürt létrehozása után.  Olvasási [fürt biztonságos](service-fabric-cluster-security.md) további információk a Service Fabric-fürt biztonsági toolearn.  

toocreate hello három csomópontos fejlesztési fürtöt, futtassa a hello *CreateServiceFabricCluster.ps1* parancsfájl egy rendszergazda PowerShell-munkamenetben:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Service Fabric-futtatókörnyezet csomag hello automatikusan letölti és a fürt létrehozásakor telepített.

## <a name="connect-toohello-cluster"></a>Csatlakoztassa toohello fürtöt
A három csomópontot tartalmazó fejlesztési fürt most már üzemel. hello futásidejű hello ServiceFabric PowerShell-modul telepítve van.  Ellenőrizheti a hello hello fürtben fut ugyanazon a számítógépen vagy egy távoli számítógépről hello Service Fabric-futtatókörnyezet.  Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag létesít kapcsolatot toohello fürt.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
Lásd: [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md) csatlakozó tooa fürt más példákat. Miután toohello fürthöz kapcsolódó, használja a hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmag toodisplay hello fürt és a állapot információt az egyes csomópontok csomópontok listáját. A **HealthState** tulajdonságnak *OK* értékűnek kell lennie minden csomópont esetében.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-hello-cluster-using-service-fabric-explorer"></a>A Service Fabric Explorerrel hello fürt megjelenítése
A [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) hatékony eszköz a fürtök megjelenítéséhez és az alkalmazások kezeléséhez.  Service Fabric Explorerben talál egy olyan szolgáltatás, hello fürt, amely egy böngésző segítségével túl útvonalon érhető el futtató[19080/Explorer](http://localhost:19080/Explorer). 

hello fürt irányítópult áttekintése a fürt, beleértve az alkalmazás és a csomópont állapotának összegzését. hello csomópont nézetben látható hello hello fürt fizikai elrendezését. Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-hello-cluster"></a>Hello fürt eltávolítása
tooremove egy fürthöz, futtassa a hello *RemoveServiceFabricCluster.ps1* PowerShell-parancsfájl hello csomag mappából, és adjon át hello elérési toohello JSON-konfigurációs fájlt. Opcionálisan megadhat egy helyet hello napló hello törlését.

```powershell
# Removes Service Fabric cluster nodes from each computer in hello configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

tooremove hello Service Fabric-futtatókörnyezet hello a számítógépről, futtassa a következő PowerShell-parancsfájl hello csomag mappából hello.

```powershell
# Removes Service Fabric from hello current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a>Következő lépések
Most, hogy állította be a fejlesztési önálló fürthöz, próbálja meg a következő cikkek hello:
* [Telepíthet egy többgépes önálló fürtöt](service-fabric-cluster-creation-for-windows-server.md), és engedélyezheti a védelmet.
* [Appokat helyezhet üzembe a PowerShell használatával](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
