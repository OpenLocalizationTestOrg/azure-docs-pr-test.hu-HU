---
title: "egy Azure mikroszolgáltatások a fejlesztési környezet létrehozása aaaSet |} Microsoft Docs"
description: "Hello futtatókörnyezet, az SDK és az eszközök telepítése, és hozzon létre egy helyi fejlesztési fürtöt. A telepítés befejezése után készen áll a toobuild alkalmazások fogja."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: 9b0442778999d4c3d2b99adb98f6596dcbdc36d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment"></a>A fejlesztőkörnyezet előkészítése
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 toobuild, és futtassa [Azure Service Fabric-alkalmazások] [ 1] a fejlesztői számítógépén, telepítse a hello futtatókörnyezet, az SDK és az eszközök. Meg kell tooenable hello hello SDK szereplő Windows PowerShell-parancsfájlok végrehajtását is.

## <a name="prerequisites"></a>Előfeltételek
### <a name="supported-operating-system-versions"></a>Támogatott operációsrendszer-verziók
a következő operációsrendszer-verziók hello fejlesztési támogatottak:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Alapértelmezés szerint a Windows 7 csak a Windows PowerShell 2.0-t tartalmazza. A Service Fabric PowerShell-parancsmagokhoz a PowerShell 3.0 vagy újabb verziója szükséges. Is [töltse le a Windows PowerShell 5.0] [ powershell5-download] a hello Microsoft Download Center.
> 
> 

## <a name="install-hello-sdk-and-tools"></a>Hello SDK és az eszközök telepítése
### <a name="toouse-visual-studio-2017"></a>a Visual Studio 2017 toouse
Service Fabric-eszközök Visual Studio 2017 hello Azure fejlesztői és felügyeleti munkaterhelés részét képezik. A Visual Studio telepítésének részeként engedélyezze ezt a munkafolyamatot.
Ezenkívül a Microsoft Azure Service Fabric SDK, Webplatform-telepítővel történő tooinstall hello van szüksége.

* [Telepítse a Microsoft Azure Service Fabric SDK hello][core-sdk]

### <a name="toouse-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>Visual Studio 2015-öt toouse (a Visual Studio 2015 Update 2 vagy újabb szükséges)
A Visual Studio 2015 a Service Fabric eszközök hello SDK használatával hello Webplatform-telepítővel együtt vannak telepítve:

* [A Microsoft Azure Service Fabric SDK hello és eszközök telepítése][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Csak az SDK telepítése
Ha csak hello SDK, telepítheti ezt a csomagot:
* [Telepítse a Microsoft Azure Service Fabric SDK hello][core-sdk]

hello aktuális verziók az alábbiak:
* Service Fabric SDK 2.7.198
* Service Fabric-futtatókörnyezet 5.7.198
* Service Fabric Tools for Visual Studio 2015 1.7.50721
* A Visual Studio 2017 Update 2 tartalmazza a Service Fabric Tools for Visual Studio 1.6.20170504-es verzióját
* A Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) tartalmazza a Service Fabric Tools for Visual Studio 1.7.20170721-es verzióját

A támogatott verziók listáját lásd: [Service Fabric-támogatás](service-fabric-support.md).

## <a name="enable-powershell-script-execution"></a>A PowerShell-parancsfájl végrehajtásának engedélyezése
A Service Fabric Windows PowerShell-parancsfájlokat használ a helyi fejlesztési fürtök létrehozásához és az alkalmazások Visual Studióból történő üzembe helyezéséhez. Alapértelmezés szerint a Windows blokkolja ezen szkriptek futását. tooenable őket, módosítania kell a PowerShell végrehajtási házirendjét. Nyissa meg a Powershellt rendszergazdaként, és írja be a következő parancs hello:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Következő lépések
Most, hogy végzett a fejlesztőkörnyezet beállításával, belefoghat az alkalmazások létrehozásába és futtatásába.

* [Az első Service Fabric-alkalmazás létrehozása a Visual Studióban](service-fabric-create-your-first-application-in-visual-studio.md)
* [Megtudhatja, hogyan toodeploy és a helyi fürtön lévő alkalmazások kezelése](service-fabric-get-started-with-a-local-cluster.md)
* [További tudnivalók a programozási modellek hello: Reliable Services és Reliable Actors](service-fabric-choose-framework.md)
* [Tekintse meg a hello Service Fabric mintakódjainak megtekintése a Githubon](https://aka.ms/servicefabricsamples)
* [A fürt megjelenítése a Service Fabric Explorer segítségével](service-fabric-visualizing-your-cluster.md)
* [Hajtsa végre a Service Fabric képzési elérési tooget a széles körű bevezetés toohello platform hello](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "A Service Fabric kampányoldala"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI-hivatkozás"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI-hivatkozás"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI-hivatkozás"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
