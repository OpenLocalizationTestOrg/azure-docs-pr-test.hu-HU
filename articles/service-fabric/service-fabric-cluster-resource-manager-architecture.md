---
title: "aaaResource Manager architektúra |} Microsoft Docs"
description: "Az architektúra áttekintése a Service Fabric fürt Resource Manager."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9ea80273d0566a2ac25143ada3662875656b57b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cluster-resource-manager-architecture-overview"></a>Fürt resource manager architektúrájának áttekintése
Service Fabric fürt erőforrás-kezelő hello központi szolgáltatás, amely az hello fürt. Hello szolgáltatásainak hello fürtben, különösen a tekintetben tooresource használati és elhelyezési szabályokat szükséges hello állapotát kezeli. 

toomanage hello erőforrások a fürt, a Service Fabric fürt erőforrás-kezelő hello több adatra kell rendelkeznie:

- Mely szolgáltatások jelenleg található.
- Minden szolgáltatás aktuális (vagy alapértelmezett) erőforrás-felhasználás 
- hello fennmaradó fürt kapacitás 
- hello fürtben található csomópontok hello hello kapacitás 
- minden egyes csomóponton felhasznált erőforrások hello mennyisége

hello hálózatierőforrás-fogyasztás adott szolgáltatás idővel megváltozhat, és a szolgáltatások általában érdeklik egynél több erőforrás típusát. Különböző szolgáltatásban valószínűleg mért tényleges fizikai mind a fizikai erőforrásokat. Szolgáltatások például lemez- és memória-felhasználás fizikai metrikák követheti nyomon. Szolgáltatások gyakrabban, logikai metrikák – többek között a "WorkQueueDepth" vagy "TotalRequests" lehet, hogy érdeklik. Logikai és fizikai metrikák hello használható ugyanabban a fürtben. Metrikák számos szolgáltatás között megoszthatók vagy adott tooa adott szolgáltatáshoz.

## <a name="other-considerations"></a>Egyéb szempontok
hello tulajdonosok és hello fürt operátorok különbözhet szolgáltatások és alkalmazások szerzők hello, vagy a minimális vannak egy hello azonos különböző kalap elhasználódó személyek. Amikor az alkalmazás fejlesztése ismernie néhány dolgot kapcsolatos akkor van szükség. Becsült rendelkezik a hello fog felhasználni erőforrásokat és különböző szolgáltatásokat kell telepíteni. Hello webes réteg például kell a közzétett csomópontok toohello Internet, a toorun a hello adatbázis szolgáltatások nem kell. Másik példaként hello webszolgáltatások vannak valószínűleg által korlátozott Processzor- és hálózati hello adatok réteg szolgáltatások gondot bővebben memóriát és lemezterületet használat során. Hello személy kezelési frissítési toohello szolgáltatásnak kezel, vagy a live hely, amelyhez az adott szolgáltatás éles környezetben azonban egy másik feladat toodo rendelkezik, és különböző eszközök. 

Hello fürt és a szolgáltatások a következők dinamikus:

- hello fürtben található csomópontok számának hello növelhető vagy csökkenthető
- Különböző méretű és csomópontok származnak, és nyissa meg
- Szolgáltatások is létrehozható, eltávolítva, és módosítsa a kívánt erőforrás-hozzárendelések és elhelyezési szabályokat
- Frissítések vagy az egyéb felügyeleti műveleteket lehet vonni keresztül hello alkalmazás hello fürthöz infrastruktúra szinten
- Hiba fordulhat elő, tetszőleges időpontban.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Fürt resource manager összetevőit és adatfolyama
hello fürt erőforrás-kezelő követelményei tootrack hello minden egyes szolgáltatást és hello fogyasztás az erőforrások ezek a szolgáltatások belül minden szolgáltatás objektum. hello fürt erőforrás-kezelő két részből áll fogalmi: minden csomópont- és hibatűrő szolgáltatásként futó ügynököket. csomópont követése tartozó ügynökök hello összesített szolgáltatások jelentéseinek őket, és rendszeresen jelenti. hello fürt erőforrás-kezelő szolgáltatás hello helyi ügynököktől származó összes hello információk összesíti és reagál aktuális konfigurációja alapján.

A következő diagram hello vizsgáljuk meg:

<center>
![Erőforrás terheléselosztó architektúrája][Image1]
</center>

Futásidőben, sok módosítások vannak, amely akkor fordulhat elő. Például tételezzük fel hello összeg erőforrások egyes szolgáltatások használnak a módosításokat, bizonyos szolgáltatások sikertelen, és egyes csomópontok csatlakoztatja, és hello fürtöt hagyja. Összesített értéket, rendszeres időközönként küldött toohello fürt erőforrás-kezelő szolgáltatással (1,2) ahol ezek újra összesítve, elemzése, és tárolja a csomóponton található összes hello módosításokat. Minden néhány másodpercben, amely a szolgáltatás ellenőrzi, hogy az hello módosításokat, és határozza meg, ha szükség-e azokat a műveleteket (3). Például azt sikerült figyelje meg, hogy néhány üres csomópontok vannak adva toohello fürt. Emiatt úgy dönt, toomove egyes szolgáltatások toothose csomópontok. hello fürt erőforrás-kezelő sikerült is vegye figyelembe, hogy egy adott csomópont túl van terhelve, vagy az, hogy egyes szolgáltatások nem sikerült-e vagy törölve lett, szabadítson fel más erőforrásokat.

Most nézze meg a következő diagram hello, és tekintse meg, mi a következő lépés. Most mondja ki, hogy hello fürt erőforrás-kezelő azt határozza meg, hogy módosításokra szükség. Az koordinálja az egyéb system (az adott hello Feladatátvevőfürt-kezelő) szolgáltatások toomake hello szükséges változtatásokat. Majd hello szükséges parancsokat küldése toohello megfelelő csomópontok (4). Például tételezzük fel hello erőforrás-kezelő első fellépése Csomópont5 volt terhelve, és ezért úgy döntött, Csomópont5 tooNode4 B toomove szolgáltatást. Hello újrakonfigurálás (5) a hello végén hello fürt néz ki:

<center>
![Erőforrás terheléselosztó architektúrája][Image2]
</center>

## <a name="next-steps"></a>Következő lépések
- hello fürt erőforrás-kezelő számos lehetőséget leíró hello fürt rendelkezik. toofind további információk a őket, tekintse meg a cikk a [leíró a Service Fabric-fürt](./service-fabric-cluster-resource-manager-cluster-description.md)
- hello fürt erőforrás-kezelő elsődleges feladatokat hello fürt kiegyenlítését és elhelyezési szabályokat kényszerítése. Ezek közül a viselkedésmódok konfigurálásával kapcsolatos további információkért lásd: [terheléselosztási a Service Fabric-fürt](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
