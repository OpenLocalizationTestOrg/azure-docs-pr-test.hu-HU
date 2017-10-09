---
title: "aaaDefinine és az Azure mikroszolgáltatások állapot kezelése |} Microsoft Docs"
description: "Hogyan toodefine és kezelheti a Service Fabric szolgáltatás állapota"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4a24696da71753d0f343a86df3556b5b7c964834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-state"></a>Szolgáltatás állapota
**Szolgáltatás állapota** toohello a memóriában vagy a lemez adatokkal, hogy a szolgáltatás futtatásához szükséges toofunction hivatkozik. Ez magában foglalja, például hello adatstruktúrák és tagváltozók hello szolgáltatás olvas és ír toodo munkahelyi. Attól függően, hogy hogyan hello szolgáltatás tervezett azt is tartalmazhatnak fájlokat vagy más erőforrásokat, amelyek tárolása a lemezen. Hello fájlok, például egy adatbázis toostore adatok és a tranzakciós naplók használna.

Példa szolgáltatásként Mérlegeljük, egy Számológép. Egy egyszerű számológép szolgáltatás időt vesz igénybe a két szám, és az összegét adja vissza. Magában foglalja a számítást, nem tagváltozók és egyéb adatait.

Most pedig nézzük meg ugyanazon Számológép hello, de egy további módszer tárolásához és hello utolsó sum visszaadó kiszámította. Ez a szolgáltatás jelenleg állapot-nyilvántartó. Állapotalapú alkalmazások és szolgáltatások azt jelenti, hogy néhány állapotát, írja az toowhen kiszámítja új összege, és ha tegye fel azt tooreturn hello utolsó számított összege olvas tartalmaz.

Az Azure Service Fabric hello első szolgáltatás állapotmentes szolgáltatások neve. második hello szolgáltatást egy állapotalapú szolgáltatás neve.

## <a name="storing-service-state"></a>Tárolja a szolgáltatás állapota
Állam vagy externalized vagy közös elhelyezésű hello kóddal, amely van hello állapot kezelésére. Állapot externalization általában egy külső adatbázis használatával hajtható végre, vagy egyéb adatok tárolására, hogy fut a különböző gépeken hello hálózaton keresztül, vagy a folyamaton kívüli hello egyazon számítógépen. A Számológép példánkban hello adattár lehet egy SQL database vagy az Azure Table-tároló példányát. Minden kérelem toocompute hello sum frissítést végez ezeken az adatokon, és toohello szolgáltatás tooreturn hello értéket eredményez hello aktuális érték hello áruházból lehívott alatt álló kérelmek. 

Állapot is is elhelyezhető hello kóddal, amely kezeli a hello állapotát. Ez a modell használatával a Service Fabric állapotalapú szolgáltatások általában készített. A Service Fabric hello infrastruktúra tooensure, hogy magas rendelkezésre áll, következetes és tartós ebben az állapotban, és könnyedén méretezhető, hogy hello szolgáltatások beépített ily módon biztosít.

## <a name="next-steps"></a>Következő lépések
A Service Fabric fogalmakat további információkért tekintse meg a következő cikkek hello:

* [A Service Fabric-szolgáltatások rendelkezésre állása](service-fabric-availability-services.md)
* [Méretezhetőséget biztosít a Service Fabric-szolgáltatások](service-fabric-concepts-scalability.md)
* [A Service Fabric szolgáltatások particionálás](service-fabric-concepts-partitioning.md)
* [Service Fabric megbízható szolgáltatások](service-fabric-reliable-services-introduction.md)
