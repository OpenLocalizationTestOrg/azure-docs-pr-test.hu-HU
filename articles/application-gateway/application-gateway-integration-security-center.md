---
title: "az Azure Security Center átjáró integrációja aaaApplication |} Microsoft Docs"
description: "Ezen a lapon bemutatja, hogyan Alkalmazásátjáró integrálva van-e az Azure Security Center."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: gwallace
ms.openlocfilehash: 6f6ace105e84c01f525ab02938e81ce040c5c9d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Alkalmazásátjáró és az Azure Security Center közötti integráció áttekintése

Ismerje meg, hogyan Application Gateway és a Security Center védelmét a webes alkalmazás erőforrásaihoz. Alkalmazás átjáró webalkalmazási tűzfal (WAF) integrálható [Security Center](../security-center/security-center-intro.md) tooprovide egy zökkenőmentes nézet tooprevent észlelése és válasz toothreats toounprotected webalkalmazások a környezetben.

## <a name="overview"></a>Áttekintés

Alkalmazás átjáró WAF várja, hogy egy érték, a Security Center webalkalmazások biztonsági réseket és biztonsági rések elleni védelme. Magas súlyosságú javaslatok, hello a security center engedélyezve van a webes erőforrásokat WAF által nem védett megjelenítése. Webalkalmazási tűzfalak javaslatok látható a hello **áttekintése** lap **alkalmazások**.

![a security Center Integration][1]

Webalkalmazási tűzfal vonatkozó javaslatokkal kattintva megnyílik egy új hello hello ajánlás részleteit megjelenítő panelen.

## <a name="add-a-web-application-firewall-tooan-existing-resource"></a>A webes alkalmazás tűzfal tooan meglévő erőforrás hozzáadása

Keresse meg a túl**több szolgáltatások** > **biztonság + identitás szakaszában** > **Security Center** a hello **Security Center – áttekintés**  panelen kattintson a **alkalmazások**. A hello **Security Center - alkalmazások** panelen hello tábla Security Center az előfizetésében észlelt alkalmazások listáját tartalmazza.

![webalkalmazások][3]

Egy kritikus problémát webalkalmazás parancsával hello beolvasása **biztonsági állapotának** panelen. Az alábbi hello kép webalkalmazás, amelyet nem véd a webalkalmazási tűzfal hello. 

![nem védett webes erőforrások][2]

Kattintson a **webalkalmazási tűzfal hozzáadása** alatt **javaslatok** tooopen hello **webalkalmazási tűzfal hozzáadása** panelen.

Ha nem meglévő Alkalmazásátjáró, esetleg toocreate egy új, kattintson a **hozzon létre új** a hello **hozzon létre egy új webalkalmazási tűzfal** panelen, majd kattintson **Microsoft - Alkalmazásátjáró**. Ez végigvezeti hello lépéseket toocreate Alkalmazásátjáró. Ezen a ponton a webes alkalmazás meg van adva egy védett erőforrásokhoz, most már a Security Center követi nyomon, hogy ehhez az erőforráshoz webalkalmazási tűzfal védi. Ez nem adja hozzá azt egy háttér címkészletet tag.

Ha egy meglévő Alkalmazásátjáró, kiválaszthatja azt a **meglévő megoldással**

![webalkalmazási tűzfal hozzáadása panel][4]

Hozzáadásával egy webes alkalmazás tooan Alkalmazásátjáró Security Center keresztül nem hello erőforrás háttér-készlet tagja, ez a kell elvégezni hello alkalmazás átjáró erőforrás közvetlenül.

## <a name="add-a-resource-tooan-existing-web-application-firewall"></a>Egy erőforrás tooan meglévő webalkalmazási tűzfal hozzáadása

Keresse meg a túl**több szolgáltatások** > **biztonság + identitás szakaszában** > **Security Center** a hello **Security Center – áttekintés**  panelen kattintson a **partneri megoldások**. Meglévő Security Center felismerésre képes alkalmazást átjárók megjelenítése a hello **partneri megoldások** panelen.

![Partneri megoldások][7]

Kattintson a **összekapcsolás** tooopen hello **összekapcsolás** panelen Itt lehetősége van hello beállítások tooselect a meglévő alkalmazásokat. Hello alkalmazások tooprotect válasszon, majd kattintson a **OK**. Ez nem bővíti ki hello webes alkalmazás toohello háttérkészlet hello Alkalmazásátjáró. Ez beállítja hello erőforrások védett erőforrásként, a Security Center nyomon követheti. tooadd hello erőforrás háttér-készlet tagja, ez a hello Alkalmazásátjáró kell elvégezni, hello aktuális paneljén kattintson **megoldáskonzol** toobe végrehajtott toohello alkalmazás átjáró erőforrás hello webes adhat hozzá alkalmazás toohello háttérkészlet.

![Partneri megoldások alkalmazások][6]

## <a name="finalize-configuration"></a>Konfigurálás lezárásaképpen

A Security Center számok alkalmazások tooan Alkalmazásátjáró fel, mert egy védett erőforrásokhoz.  Ehhez az erőforráshoz hello állapotát figyeli, és biztosítja, hogy egy alkalmazás-átjáró által védett. következő lépés hello tooadd hello magánhálózati IP-címe, nyilvános IP-cím vagy a virtuális gép toohello háttérkészlet hello Alkalmazásátjáró hálózati Adapterének. Amíg ez történik, egy további ajánlása **alkalmazás védelem véglegesítése** látható, amíg nincs hozzáadva a hello erőforrás.

![webalkalmazási tűzfal hozzáadása panel][5]

## <a name="security-alerts"></a>Biztonsági riasztások

A Security Center Ugrás túl**ÉSZLELÉSI** > **biztonsági riasztások**.  Itt megtalálja az Ön WAF riasztások az alkalmazás átjárók. Riasztások WAF szabály bontásban.

![biztonsági riasztások][8]

Kattintson egy szabály nyújtják adott WAF szabályhoz tartozó riasztások listája. Minden riasztás további részleteket hello megállapítás mutatja be. hello részletek adjon meg egy hivatkozást toohello Alkalmazásátjáró.
 
![riasztás részletei][9]

## <a name="next-steps"></a>Következő lépések

Hogyan tooenable webalkalmazási tűzfal egy meglévő alkalmazás átjárón, látogasson el toolearn [létrehozás vagy frissítés webalkalmazási tűzfal az Azure-Alkalmazásátjáró](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png