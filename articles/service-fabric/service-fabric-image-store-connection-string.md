---
title: "a Service Fabric lemezképet tároló karakterláncában aaaAzure |} Microsoft Docs"
description: "Hello lemezkép tárolási kapcsolati karakterlánc ismertetése"
services: service-fabric
documentationcenter: .net
author: alexwun
manager: timlt
editor: 
ms.assetid: 00f8059d-9d53-4cb8-b44a-b25149de3030
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: alexwun
ms.openlocfilehash: 83f5ad75b5df07726997da3173722028255b8cae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-imagestoreconnectionstring-setting"></a>Hello előtaggal beállítás ismertetése

Egyes dokumentációban azt röviden említse meg egy "Előtaggal" paraméter hello megléte valóban jelentés leíró nélkül. És egy cikk például elvégzése után [központi telepítése és törlése alkalmazás PowerShell-lel][10], úgy tűnik, tegye az összes érték másolási és beillesztési hello hello fürtjegyzékben hello TARGET megjelenő fürt. Ezért hello beállítással konfigurálható, fürtönként meg kell adni, de egy fürt keresztül hello [Azure-portálon][11], nincs lehetőség tooconfigure ezt a beállítást, és annak mindig "fabric: Lemezképtárolóba" van. Mi az a célja hello ezt a beállítást, majd?

![A Fürtjegyzékben][img_cm]

Service Fabric a belső Microsoft felhasználásához platformként által indított számos különböző csapatok, így nagy mértékben testre szabható bizonyos aspektusainak - hello "Image Store" az egyik ilyen tulajdonsága. Alapvetően hello Image Store tárháza moduláris alkalmazáscsomagok tárolására. Ha az alkalmazás telepített tooa hello fürt csomópontja, a csomópont letölti a alkalmazáscsomag hello tartalmát a hello Image Store. hello előtaggal, amely tartalmazza az összes hello szükséges információt az ügyfelek és a csomópontok toofind hello megfelelő lemezképtárolóhoz adott fürt beállítás.

Jelenleg nincsenek Image Store szolgáltatók három lehetséges típusú, és a megfelelő kapcsolati karakterláncok a következők:

1. Image Store szolgáltatás: "fabric: Lemezképtárolóba"

2. Fájlrendszer: "file:[file rendszer path]"

3. Az Azure Storage: "xstore:DefaultEndpointsProtocol = https; AccountName = [...]; AccountKey = [...]; Tároló = [...] "

hello szolgáltató éles környezetben használt típus hello kép Store szolgáltatást, ez az állapot-nyilvántartó megőrzött rendszerszolgáltatás is látható, a Service Fabric Explorerből. 

![Image Store szolgáltatás][img_is]

Rendszerszolgáltatás maga hello fürtön belül az üzemeltetési hello Image Store kiküszöböli hello csomag tárház vonatkozó külső függőségeket és hello helység tárolási több ellenőrzést ad. Jövőbeli fejlesztések hello Image Store körül, ha nem kizárólag először valószínűleg tootarget hello lemezkép tárolási szolgáltató. hello kapcsolati karakterlánc hello lemezkép tárolási szolgáltató nem rendelkezik egyedi információt, mert hello ügyfél már csatlakoztatott toohello cél fürtnek. hello ügyfélnek csak kell tooknow hello rendszerszolgáltatás célzó protokollok kell használni.

hello fájlrendszer szolgáltató hello kép Store szolgáltatás helyett egy beépített helyi fürtök során használatos fejlesztési toobootstrap hello fürt némileg gyorsabb. hello különbség általában kicsi, de a legtöbb segítsen a fejlesztés során hasznos optimalizálás. Annak lehetséges toodeploy-fürtöt egy helyi egy beépített hello egyéb tárolási szolgáltató típusok is, de általában nincs ok toodo, mivel hello fejlesztés/tesztelés munkafolyamat marad hello azonos szolgáltató függetlenül. A használati eltérő hello fájlrendszert és Azure Storage-szolgáltatók csak léteznek örökölt támogatása.

Ezért amíg hello előtaggal konfigurálható, általában csak használhatja hello alapértelmezett beállítás. TooAzure keresztül közzétételekor [Visual Studio][12], hello paraméter értéke automatikusan, ennek megfelelően. Az Azure-ban üzemeltetett a programozott telepítés tooclusters hello kapcsolati karakterlánca mindig "fabric: Lemezképtárolóba". Ha kétségei vannak, az értéke mindig ellenőrizhet hello fürtjegyzékben által lekérésével [PowerShell](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), vagy [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest). Mind a helyszíni tesztelése, és éles fürtök mindig konfigurált toouse hello lemezkép tárolási szolgáltató is.

### <a name="next-steps"></a>Következő lépések
[Központi telepítése, és távolítsa el az alkalmazásokat a PowerShell használatával][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-publish-app-remote-cluster.md
