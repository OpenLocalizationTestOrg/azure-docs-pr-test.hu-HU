---
title: Az Azure Service Fabric Linux |} Microsoft Docs
description: "Service Fabric-fürtök Linux és a Java, ami azt jelenti, képes lesz központi telepítéséhez és Linux Java és a C# nyelven írt alkalmazások üzemeltetését a Service Fabric támogatja."
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
ms.openlocfilehash: dddc9f698d9776999d406117b46285a0f90d9620
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="service-fabric-on-linux"></a>A Service Fabric Linux rendszeren
Az előzetes verzióját a Service Fabric Linux lehetővé teszi, hogy hozhat létre, telepíthetnek és Linux magas rendelkezésre állású, nagymértékben méretezhető alkalmazásokat kezeléséhez, ugyanúgy, mint a Windows. A Service Fabric-keretrendszerek (Reliable Services és Reliable Actors) a C# (.NET Core) mellett a Linux Java nyelven érhetők el.  Is létrehozható [végrehajtható vendégszolgáltatások](service-fabric-deploy-existing-app.md) rendelkező bármilyen nyelven vagy keretrendszerben. Emellett az előzetes verziója is támogatja koordinálása Docker-tároló. Docker-tároló Vendég végrehajtható fájlok, illetve a Service Fabric-keretrendszert használó natív Service Fabric szolgáltatások futtathatók.

A Service Fabric Linux fogalmilag Service Fabric Windows (kivéve az operációs rendszer jellemzőit és a támogatott programozási nyelvek). Így a legtöbb a [meglévő dokumentáció](http://aka.ms/servicefabricdocs) , ismerkedjen meg a technológia segítséget nyújt a vonatkozik.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a>Támogatott operációs rendszerek és programozási nyelvek
A korlátozott előzetes verziója támogatja a többgépes fürtök mellett egy beépített fejlesztési fürtök létrehozásához az Ubuntu Server 16.04 futó Azure-ban. Az előzetes verziója támogatja a Reliable Actors és a megbízható állapotmentes szolgáltatások keretrendszerek Java és C# Vendég végrehajtható fájlok és a Docker-tároló koordinálása mellett.  

> [!NOTE]
> Megbízható gyűjtemények nem támogatottak a Linux még. Készenléti önmagában nem használhatók vagy - csak egy mezőt és az Azure Linux többgépes fürtök az előzetes verzió támogatott.
>
>


## <a name="supported-tooling"></a>Támogatott tooling eszköz
Az előzetes verziója támogatja a fürt Service Fabric parancssori felületen keresztül való együttműködéshez. Java-fejlesztők számára az integrációra az eclipse-ben és a Yeoman által biztosított Eclipse OSX és Linux rendszeren támogatott. Az os x-integráció a technikai részletek vagrant keresztül egy Linux virtuális gép használja. C#-fejlesztők számára Yeoman integrációja biztosítja alkalmazás sablonok létrehozásához.

## <a name="next-steps"></a>Következő lépések

* Ismerkedjen meg a [Reliable Actors](service-fabric-reliable-actors-introduction.md) és [Reliable Services](service-fabric-reliable-services-introduction.md) keretrendszerek programozási
* [A fejlesztőkörnyezet előkészítése Linuxon](service-fabric-get-started-linux.md)
* [A fejlesztőkörnyezet előkészítése OSX-en](service-fabric-get-started-mac.md)
* [Az első Service Fabric Java-alkalmazás létrehozása Linux rendszeren](service-fabric-create-your-first-linux-application-with-java.md)
* [A telepítő a Service Fabric folyamatos integrációt és Jenkins és a GitHub megadott központi telepítés](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [Service Fabric – Különbségek Windows és Linux rendszeren](service-fabric-linux-windows-differences.md)
