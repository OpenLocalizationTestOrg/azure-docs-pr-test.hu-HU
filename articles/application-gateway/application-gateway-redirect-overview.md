---
title: "az Azure Application Gateway aaaRedirect áttekintése |} Microsoft Docs"
description: "További tudnivalók az Azure alkalmazás átjáró hello átirányítási funkció"
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: amsriva
ms.openlocfilehash: 7149e3bd000d336c51995fb0e90f971b4d9ba2be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-redirect-overview"></a>Átjáró átirányítási – áttekintés

Egy általános forgatókönyv számos webes alkalmazásokhoz toosupport automatikus HTTP tooHTTPS átirányítási tooensure alkalmazás és a felhasználók közötti összes kommunikáció titkosított elérési útjának keresztül történik. Az elmúlt hello az ügyfelek használt módszerek például létrehozhat egy dedikált háttérkészlet, amelynek kizárólagos célja a HTTP tooHTTPS kap tooredirect kérelmek.  Alkalmazásátjáró képességét tooredirect forgalom mostantól támogatja a hello Application Gateway. Ez egyszerűbbé teszi az alkalmazás konfigurációját, hello Erőforrás kihasználtsága optimalizálja, és többek között a globális és útvonal-alapú átirányítási új átirányítási forgatókönyveket támogat. Átjáró átirányítást alkalmazástámogatási nincs korlátozva tooHTTP -> kizárólag HTTPS-átirányítás. Ez az egy általános átirányítási mechanizmus, lehetővé teszi egy figyelő tooanother figyelő alkalmazás átjáró fogadja forgalom átirányítását. Átirányítás tooan külső webhely is is támogatja. Átjáró átirányítást alkalmazástámogatási hello a következő lehetőségeket kínálja:

1. Egy figyelő tooanother figyelő hello átjáró a globális átirányítást. Ez lehetővé teszi a HTTP-tooHTTPS átirányítás egy helyen.
2. Elérési út-alapú átirányítási. Átirányítás az ilyen típusú lehetővé teszi, hogy csak egy adott hely területen, a HTTP-tooHTTPS átirányítás példa egy vásárlásra szolgáló bevásárlókocsiból területre jelölik/bevásárlókocsiból / *.
3. Az átirányítási tooexternal hely.

![az átirányítási](./media/application-gateway-redirect-overview/redirect.png)

Ez a változás az ügyfelek kellene toocreate új átirányítási konfigurációs objektum, amely hello cél figyelő adja meg, vagy külső webhely toowhich átirányítási van szükség. hello konfigurációs elem beállítások tooenable fűznek hello URI elérési útja és a lekérdezési karakterlánc toohello átirányított URL-címet is támogatja. Az ügyfelek is sikerült adja átirányítási egyikét (HTTP-állapotkód 302) ideiglenes és állandó átirányítás (HTTP-állapotkód 301). Ez a konfiguráció átirányítási létrehozása után van a csatolt toohello forrás figyelő keresztül egy új szabályt. Egy alapszintű szabály használata esetén a hello átirányítási konfigurációs forrás figyelő társított, és a globális átirányítás. Elérési út alapú szabály használata esetén a hello átirányítási konfigurációs hello URL-cím elérési út leképezés van definiálva, és ezért csak érvényes toohello a megadott elérési út terület hely.

### <a name="next-steps"></a>Következő lépések

[Alkalmazásátjáró átirányítási URL-cím konfigurálása](application-gateway-configure-redirect-powershell.md)
