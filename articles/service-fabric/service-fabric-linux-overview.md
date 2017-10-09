---
title: a Service Fabric Linux aaaAzure |} Microsoft Docs
description: "Service Fabric-fürtök támogatja a Linux és a Java, ami azt jelenti, hogy be tudja toodeploy és Linux Java és a C# nyelven írt alkalmazások üzemeltetését a Service Fabric fog."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 459afade-145d-4ee6-b72b-ddf380ccd1bf
ms.service: service-fabric
ms.devlang: Java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: SubramaR
ms.openlocfilehash: ca0e092b00faec560963c0d6cc379593d085f6a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-on-linux"></a>A Service Fabric Linux rendszeren
hello előzetes verzióját a Service Fabric Linux lehetővé teszi a toobuild, telepítéséhez és felügyeletéhez a magas rendelkezésre állású, nagymértékben méretezhető alkalmazások Linux, ugyanúgy, mint Windows rendszeren. hello Service Fabric keretrendszerek (Reliable Services és Reliable Actors) érhetők el a Linux Java hozzáadása tooC # (.NET Core).  Is létrehozható [végrehajtható vendégszolgáltatások](service-fabric-deploy-existing-app.md) rendelkező bármilyen nyelven vagy keretrendszerben. Emellett a hello előzetes verziója is támogatja a koordinálása Docker-tároló. Docker-tároló Vendég végrehajtható fájlok, illetve hello Service Fabric-keretrendszert használó natív Service Fabric szolgáltatások futtathatók.

A Service Fabric Linux fogalmilag egyenértékű tooService Windows Fabric (kivéve az operációs rendszer jellemzőit és a támogatott programozási nyelvek). Így a legtöbb a [meglévő dokumentáció](http://aka.ms/servicefabricdocs) ismerkedhet meg vele hello technológia, segítve a vonatkozik.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a>Támogatott operációs rendszerek és programozási nyelvek
korlátozott hello preview támogat hello létrehozása egy beépített fejlesztési fürtök emellett toomulti-gép fürtök Ubuntu Server 16.04 futó Azure-ban. hello Reliable Actors és megbízható állapotmentes szolgáltatások keretrendszerek Java és C# hozzáadása tooguest végrehajtható fájlok és a Docker-tároló koordinálása hello hello preview támogat.  

> [!NOTE]
> Megbízható gyűjtemények nem támogatottak a Linux még. Készenléti önmagában nem használhatók - vagy csak egy mezőt és az Azure Linux többgépes fürtök hello előzetes verzió támogatott.
>
>


## <a name="supported-tooling"></a>Támogatott tooling eszköz
hello előzetes verziója támogatja a Service Fabric CLI hello fürtön való együttműködéshez. Java-fejlesztők számára az integrációra az eclipse-ben és a Yeoman által biztosított Eclipse OSX és Linux rendszeren támogatott. hello OSX integrációs hello technikai vagrant keresztül egy Linux virtuális gép használja. C#-fejlesztők számára toogenerate alkalmazássablonok Yeoman integrációja biztosítja.

## <a name="next-steps"></a>Következő lépések

* Ismerkedjen meg hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) és [Reliable Services](service-fabric-reliable-services-introduction.md) keretrendszerek programozási
* [A fejlesztőkörnyezet előkészítése Linuxon](service-fabric-get-started-linux.md)
* [A fejlesztőkörnyezet előkészítése OSX-en](service-fabric-get-started-mac.md)
* [Az első Service Fabric Java-alkalmazás létrehozása Linux rendszeren](service-fabric-create-your-first-linux-application-with-java.md)
* [A telepítő a Service Fabric folyamatos integrációt és Jenkins és a GitHub megadott központi telepítés](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [Service Fabric – Különbségek Windows és Linux rendszeren](service-fabric-linux-windows-differences.md)
