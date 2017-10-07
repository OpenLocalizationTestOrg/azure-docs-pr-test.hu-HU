---
title: "aaaOverview több-bérlős vissza végződő Azure Application Gateway |} Microsoft Docs"
description: "Ezen a lapon áttekintést hello Application Gateway-támogatás a több-bérlős hátsó végződik."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: b7da8c9c68e34bd83ad2b828fab62c00caea354a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Az Application Gateway támogatása több-bérlős háttérrendszerekhez

Az Azure Application Gateway háttérkészleteiben virtuálisgép-méretezési csoportok, hálózati adapterek, nyilvános vagy magánhálózati IP-címek, illetve teljes tartománynevek (FQDN) szerepelhetnek. Alapértelmezés szerint az Alkalmazásátjáró nem változtatja meg a bejövő HTTP állomásfejléc hello hello ügyfélről, és küld hello fejléc változatlan toohello háttér. Például számos szolgáltatást [Azure Web Apps](../app-service-web/app-service-web-overview.md) és [API Management](../api-management/api-management-key-concepts.md) , amely több-bérlős jellegűvé és egy adott állomásfejléc vagy SNI bővítmény tooresolve toohello helyes végpontját. Alkalmazásátjáró most támogatja a felhasználók toooverwrite hello bejövő HTTP állomásfejléc hello háttér HTTP-beállításai alapján a hello képességét. Ez a képesség teszi lehetővé a több-bérlős háttérrendszerek Azure Web Apps és API Management szolgáltatásának támogatását. Ez a funkció hello standard és a WAF SKU érhető el. Több-bérlős háttérhálózatként támogatási is SSL lezárást és end tooend SSL-forgatókönyvek esetében működik.

![webalkalmazás-forgatókönyv](./media/application-gateway-web-app-overview/scenario.png)

hello képességét toospecify egy állomáson van definiálva: hello HTTP-beállítások, és lehet vissza alkalmazott tooany fejezheti készlet szabály létrehozása során. Több-bérlős vissza kétféle módon állomásfejléc és SNI-bővítmény a következő támogatási hello ér véget.

1. hello képességét tooset hello állomás neve tooa rögzített hello érték HTTP-beállításait. Ez a funkció biztosítja, hogy hello állomásfejléc felülbírálja toothis érték, az összes forgalom toohello háttérkészlethez ahol hello HTTP-beállítások alkalmazása. Záró tooend SSL használata esetén a felülbírált állomásnevet hello SNI bővítmény használatos. Ez a funkció lehetővé teszi, hogy a forgatókönyvek, ahol egy háttér címkészletet farm vár bejövő hello-ügyfél állomásfejléc eltérő állomásfejléc.

2. hello képességét tooderive hello állomásnévvel hello IP vagy FQDN-jét hello háttér a készlet tagjainak. HTTP-beállítások is egy háttér címkészletet tag teljesen minősített Tartományneve, ha a hello beállítás tooderive állomásnévvel egy egyéni hátteret készlettag konfigurálva a adjon meg egy beállítás toopick hello állomás neve. Záró tooend SSL használata esetén a állomásnevet hello FQDN származik, és hello SNI bővítmény használatban van. Ez a funkció lehetővé teszi, hogy az esetek, amikor egy háttérkészlethez rendelkezhet kettő vagy több több-bérlős PaaS hasonló szolgáltatások az Azure web apps, így hello kérelem host fejléc tooeach tag hello állomás nevét a teljesen minősített tartománynév a származtatott tartalmazza.

> [!NOTE]
> Mindkét esetben megelőző hello hello-beállítások csak hello élő forgalom viselkedést és érintik nem hello állapot-mintavételi viselkedését. Egyéni vizsgálat már támogatási hello képességét toospecify állomásfejléc hello mintavételi konfigurációban. Egyéni mintavételt most is támogatja a hello képességét tooderive hello host fejléc viselkedés hello jelenleg konfigurált HTTP-beállításait. Ez a konfiguráció hello használatával adható meg `PickHostNameFromback endAddress` hello mintavételi konfigurációja paramétert. Záró tooend funkció toowork hello mintavételi és a HTTP-beállítások hello kell módosított tooreflect hello helyes konfiguráció.

Ezzel a lehetőséggel az ügyfelek meg hello beállítások hello HTTP-beállításait, és egyéni vizsgálat toohello megfelelő konfiguráció. Ez a beállítás kötődik majd tooa figyelő és biztonsági készlet végén szabály segítségével.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan Alkalmazásátjáró egy webalkalmazást, mint a biztonsági mentést tooset végén ellátogatva készlettag: [konfigurálása az App Service web Apps alkalmazások Alkalmazásátjáró](application-gateway-web-app-powershell.md)
