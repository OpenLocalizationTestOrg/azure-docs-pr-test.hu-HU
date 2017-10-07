---
title: "aaaService háló programozási modell áttekintése |} Microsoft Docs"
description: "A Service Fabric kínál a szolgáltatások két keretrendszerek: hello szereplő keretrendszer és hello szolgáltatások keretében. Különböző kompromisszumot alakítson ki az egyszerűség és vezérlő kínálnak."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: vturecek
ms.openlocfilehash: b48af2a7b41935bdf0e4594c765f363e520c254e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-programming-model-overview"></a>A Service Fabric programozási modell áttekintése
A Service Fabric több módon toowrite kínál, és a szolgáltatások kezeléséhez. Szolgáltatások választható toouse hello Service Fabric API-k tootake teljes mértékben hello platform szolgáltatásai és alkalmazás-keretrendszerek számára. Szolgáltatások bármilyen nyelven vagy egyszerűen csak a Service Fabric fürt által futtatott tároló futó lefordított végrehajtható programok is lehet.

## <a name="guest-executables"></a>Vendég végrehajtható fájlok
A [Vendég végrehajtható](service-fabric-deploy-existing-app.md) -e egy meglévő, tetszőleges végrehajtható fájl (bármilyen nyelven írt) az alkalmazás szolgáltatásként futtatható. Vendég végrehajtható fájlokat ne hívja közvetlenül hello Service Fabric SDK API-k. Azonban továbbra is részesülnek szolgáltatások hello platformot kínál, például a szolgáltatás felderíthetőség, egyéni állapotát és a Service Fabric által elérhetővé tett REST API-k hívásával reporting. Teljes alkalmazás életciklusa támogatási is rendelkeznek.

Ismerkedés a Vendég végrehajtható fájlok az első üzembe helyezésével [Vendég futtatható alkalmazás](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Tárolók
Alapértelmezés szerint a Service Fabric telepíti, és aktiválja a szolgáltatást is folyamatokat. A Service Fabric is telepítheti a szolgáltatások [tárolók](service-fabric-containers-overview.md). A Service Fabric Windows Server 2016 Linux-tárolók és a Windows-tárolók központi telepítését támogatja. Tároló képek is lehet bármely tároló tárház lekért és toohello gépen telepítve. A meglévő alkalmazások Vendég exectuables, a Service Fabric állapot nélküli és állapotalapú Reliable services telepítheti vagy tárolókban, és a Reliable Actors kombinálhatja a folyamatokat a szolgáltatások, és a tárolókban lévő szolgáltatások hello ugyanahhoz az alkalmazáshoz.

[További információk a szolgáltatások a Windows vagy Linux containerizing](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Megbízható Services rendszer integrálása hello Service Fabric-platformról és igénybe vehesse az hello teljes készletét platform funkciói szolgáltatások írásra passzív keretrendszere. Megbízható szolgáltatások adjon meg egy minimális API-k, amelyek lehetővé teszik a hello Service Fabric futásidejű toomanage hello életciklusát a szolgáltatások és, amelyek lehetővé teszik, hogy a szolgáltatások toointeract hello futtatási idő mellett. hello alkalmazás-keretrendszer minimális, felkínálva teljes tervezési és megvalósítási döntések vezérlése, és minden más alkalmazás-keretrendszert, például az ASP.NET Core lehet használt toohost.

Megbízható szolgáltatások állapotmentes, hasonló toomost szolgáltatás platformokat, például a webkiszolgálók, amelyben hello szolgáltatás minden példányának egyenlő jön létre, és a külső megoldás például Azure DB vagy az Azure Table Storage megőrződjenek állapota lehet.

Megbízható szolgáltatásokat is állapotalapú, kizárólagos tooService háló, közvetlenül a megbízható gyűjtemények használatával hello szolgáltatás állapota tároló. Állapot magas rendelkezésre állású-replikáció útján, és a particionálás, az összes kezelt Service Fabric által automatikusan keresztül terjesztett.

[További tudnivalók a Reliable Services](service-fabric-reliable-services-introduction.md) vagy Kezdésként [írása az első megbízható szolgáltatás](service-fabric-reliable-services-quick-start.md).

## <a name="reliable-actors"></a>Reliable Actors
Reliable Services platformra épül, hello megbízható szereplő keretrendszer egy alkalmazás-keretrendszer, amely megvalósítja az hello virtuális szereplő mintát, hello szereplő kialakítási minta alapján. hello megbízható szereplő keretrendszer független egység a számítási műveletek és az állapot nevű szereplője egyszálas végrehajtási használ. hello megbízható szereplő keretrendszer szereplője és előre beállított állapot megőrzését és kibővített konfigurációk beépített kommunikációt biztosít.

Mivel a Reliable Actors magát a Reliable Services épülő alkalmazás-keretrendszer, teljesen integrálva van a Service Fabric-platformról hello és hello teljes készletét hello platform által nyújtott szolgáltatások előnyeit.

[További tudnivalók a Reliable Actors](service-fabric-reliable-actors-introduction.md) vagy Kezdésként [az első megbízható szereplő szolgáltatás írása](service-fabric-reliable-actors-get-started.md)

## <a name="aspnet-core"></a>ASP.NET-mag
Integrálható a Service Fabric [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) tartalmazzák az alkalmazás a Web-és API létrehozása, amely lehet. 

[ASP.NET Core segítségével egy előtér-szolgáltatás létrehozása](service-fabric-add-a-web-frontend.md)

## <a name="next-steps"></a>Következő lépések
[A Service Fabric és a tárolók – áttekintés](service-fabric-containers-overview.md)

[Megbízható szolgáltatások – áttekintés](service-fabric-reliable-services-introduction.md)

[Megbízható szolgáltatások – áttekintés](service-fabric-reliable-actors-introduction.md)

[A Service Fabric és az ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




