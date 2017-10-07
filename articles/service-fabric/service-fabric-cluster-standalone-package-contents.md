---
title: "Service Fabric önálló csomag a Windows Server aaaAzure |} Microsoft Docs"
description: "Leírás és a Windows Server hello Azure Service Fabric önálló csomag tartalmát."
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik
ms.openlocfilehash: e4c6cb9128d659144e559fcad06bb78c9969dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>A Windows Server a Service Fabric önálló csomag tartalma
A hello [letöltött](http://go.microsoft.com/fwlink/?LinkId=730690) Service Fabric önálló csomag, a következő fájlok hello találhatja:

| **Fájlnév** | **Rövid leírás** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |Egy PowerShell-parancsfájlt, amely hello beállításokkal található nyomkövetési naplókat hello fürtöt hoz létre. |
| RemoveServiceFabricCluster.ps1 |Egy PowerShell-parancsfájlt, amely eltávolítja a fürt hello beállítások található nyomkövetési naplókat. |
| AddNode.ps1 |A PowerShell parancsfájl a csomópont tooan meglévő fürt hello erre a számítógépre telepített. |
| RemoveNode.ps1 |Egy PowerShell-parancsfájl a csomópont eltávolítása egy meglévő fürt hello aktuális gépen telepítve. |
| CleanFabric.ps1 |A Service Fabric telepítési ki hello aktuális gépet önálló tisztításhoz PowerShell-parancsfájlt. Előző MSI-telepítések el kell távolítani a saját társított uninstallers használatával. |
| TestConfiguration.ps1 |Egy PowerShell-parancsfájlt a hello Cluster.json hello infrastruktúra elemzéséhez. |
| DownloadServiceFabricRuntimePackage.ps1 |Egy PowerShell-parancsfájlt a forgatókönyvekben, ahol hello központi telepítése a számítógép nem csatlakozott toohello hello legújabb futásidejű csomag sávon kívül a felhasznált internet. |
| DeploymentComponentsAutoextractor.exe |Önkicsomagoló archív tartalmazó telepítési összetevők által használt hello önálló csomag parancsfájlok. |
| EULA_ENU.txt |hello licencfeltételeit hello használata a Microsoft Azure Service Fabric önálló Windows Server csomag. Is [hello EULA másolatának letöltése](http://go.microsoft.com/fwlink/?LinkID=733084) most. |
| Readme.txt |A hivatkozás toohello kibocsátási megjegyzések és alapvető telepítési utasításokat. Ebben a dokumentumban hello utasításokat egy részét is. |
| ThirdPartyNotice.rtf |Figyelje meg a harmadik felek szoftvereivel, hogy hello csomag megtalálható. |
| Tools\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |Az igény szerinti toocollect és feltöltése nyomkövetési futtatva StandaloneLogCollector.exe támogatási célra tooMicrosoft naplózza. |
| Tools\ServiceFabricUpdateService.zip |Egy eszköz fürtöket, amelyek nem rendelkeznek interneteléréssel használt tooenable automatikus kód frissítése. További részleteket talál [Itt](service-fabric-cluster-upgrade-windows-server.md)|

**Sablonok** 
| **Fájlnév** | **Rövid leírás** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |A fürt mintát tartalmazó konfigurációs fájlt egy nem biztonságos, három csomópontos egyszámítógépes (vagy a virtuális gép) fejlesztési fürtöt, vele együtt hello információt az egyes csomópontok hello fürt hello beállításait. |
| ClusterConfig.Unsecure.MultiMachine.json |A fürt mintát tartalmazó konfigurációs fájlt egy nem biztonságos, több számítógép (vagy virtuális gép) fürt, többek között az egyes gépek hello információk hello fürt hello-beállítások. |
| ClusterConfig.Windows.DevCluster.json |A fürt mintát tartalmazó konfigurációs fájl összes hello biztonságos, három csomópontos egyszámítógépes (vagy virtuális gép) beállításainak fejlesztési fürtöt, vele együtt hello információt, amely hello fürt minden csomópontja számára. hello fürt használatával lett biztonságossá [Windows identitások](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.Windows.MultiMachine.json |A fürt mintát tartalmazó konfigurációs fájlt használja a Windows biztonsági, minden gép, amely hello biztonságos tagja hello párbeszédeket biztonságos, több számítógép (vagy virtuális gép) fürt összes hello beállításainak. hello fürt használatával lett biztonságossá [Windows identitások](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.x509.DevCluster.json |A fürt mintát tartalmazó konfigurációs fájlt egy biztonságos, három csomópontos egyszámítógépes (vagy a virtuális gép) fejlesztési fürtöt, vele együtt hello információt az egyes csomópontok hello fürt összes hello beállítása. hello fürt használatával lett biztonságossá téve x509 tanúsítványokat. |
| ClusterConfig.x509.MultiMachine.json |A fürt mintát tartalmazó konfigurációs fájlt hello biztonságos, több számítógép (vagy virtuális gép) fürtöt, vele együtt hello információt az egyes csomópontok hello biztonságos fürt összes hello beállítása. hello fürt használatával lett biztonságossá téve x509 tanúsítványokat. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |A fürt mintát tartalmazó konfigurációs fájlt hello biztonságos, több számítógép (vagy virtuális gép) fürtöt, vele együtt hello információt az egyes csomópontok hello biztonságos fürt összes hello beállítása. hello fürt használatával lett biztonságossá téve [csoportosan felügyelt szolgáltatásfiókok](https://technet.microsoft.com/en-us/library/jj128431(v=ws.11).aspx). |

## <a name="cluster-configuration-samples"></a>Fürt konfigurációs minták
A fürt konfigurációs sablonok legfrissebb verziója található a GitHub-oldalon hello: [önálló fürt konfigurációs minták](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Független futásidejű csomag
hello legújabb futásidejű csomag automatikusan letölti a fürt telepítésekor [- Service Fabric-futtatókörnyezet - hivatkozás töltse le a Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).

## <a name="related"></a>Kapcsolódó
* [Önálló Azure Service Fabric-fürt létrehozása](service-fabric-cluster-creation-for-windows-server.md)
* [A Service Fabric-fürt biztonsági forgatókönyvek](service-fabric-windows-cluster-windows-security.md)
