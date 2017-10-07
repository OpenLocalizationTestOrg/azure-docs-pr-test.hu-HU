---
title: "aaaEnable access tooAzure DC/OS tároló alkalmazásnak |} Microsoft Docs"
description: "Tooenable nyilvános miként férhetnek hozzá az Azure Tárolószolgáltatás tooDC/OS tárolók."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 1ba251ba5a176a6a5e1c7831655164e380a62b27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-public-access-tooan-azure-container-service-application"></a>Nyilvános hozzáférés tooan Azure Tárolószolgáltatás-alkalmazás engedélyezése
A DC/OS tárolóhoz hello ACS [nyilvános ügynök készlet](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) automatikusan elérhetővé toohello van internet. Alapértelmezés szerint portok **80-as**, **443-as**, **8080** nyitva vannak, és bármilyen (nyilvános) tároló, ezeket a portokat figyeli érhetők el. Ez a cikk bemutatja, hogyan tooopen több portok az alkalmazások az Azure Tárolószolgáltatásban.

## <a name="open-a-port-portal"></a>Nyisson meg egy portot (portál)
Először létre kell azt szeretnénk, ha tooopen hello port.

1. Jelentkezzen be toohello portálon.
2. Keresés hello erőforráscsoport központilag telepített hello Azure Tárolószolgáltatás számára.
3. Válassza ki az ügynök terheléselosztó hello (amely nevű hasonló túl**XXXX-ügynök-lb-XXXX**).
   
    ![Azure-tárolót szolgáltatás terheléselosztó](./media/container-service-enable-public-access/agent-load-balancer.png)
4. Kattintson a **vizsgálat** , majd **hozzáadása**.
   
    ![Azure-tárolót szolgáltatás terheléselosztó-mintavétel](./media/container-service-enable-public-access/add-probe.png)
5. Töltse ki hello mintavételi űrlapot, és kattintson **OK**.
   
   | Mező | Leírás |
   | --- | --- |
   | Név |Hello mintavétel egy leíró nevet. |
   | Port |hello tároló tootest hello portja. |
   | Elérési út |(Ha HTTP módban) relatív webhely elérési útja tooprobe hello. A HTTPS nem támogatott. |
   | időköz |hello időn közötti mintavételi kísérletek, másodpercben. |
   | Sérült küszöbérték |Próbálkozások száma egymást követő mintavétel az előtt annak eldöntéséhez, hogy hello tároló nem kifogástalan. |
6. Eközben hello ügynök terheléselosztójának hello tulajdonságait, kattintson a **terheléselosztási szabályok** , majd **Hozzáadás**.
   
    ![Azure-tárolót szolgáltatás terheléselosztási szabály](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Töltse ki hello load balancer űrlapot, és kattintson a **OK**.
   
   | Mező | Leírás |
   | --- | --- |
   | Név |Hello terheléselosztó egy leíró nevet. |
   | Port |hello nyilvános bejövő portot. |
   | Háttér-port |hello belső nyilvános port hello tároló tooroute forgalom számára. |
   | Háttérkészlet |Ebben a készletben hello tárolók lesz hello célja a terheléselosztóhoz. |
   | Hálózatfigyelő |hello használt mintavételi toodetermine, ha a célként a hello **háttérkészlet** működik megfelelően. |
   | Munkamenet megőrzését |Meghatározza, hogy egy ügyféltől érkező forgalmat hello munkamenet idejére hello kezelésének módját.<br><br>**Nincs**: hello ugyanazt az ügyfelet bármely tárolóban kell kezelnie az egymást követő kéréseket.<br>**Ügyfél IP**: hello azonos ügyfél IP kezeli az egymást követő kéréseket hello ugyanabban a tárolóban.<br>**Ügyfél IP protokoll és**: hello azonos ügyfél IP-protokoll kombináció kezeli az egymást követő kéréseket hello ugyanabban a tárolóban. |
   | Üresjárati időtúllépés |(Csak a TCP) (Percben), a TCP/HTTP-ügyfél nyissa meg a anélkül, hogy az idő tookeep hello *életben tartási* üzeneteket. |

## <a name="add-a-security-rule-portal"></a>Adja hozzá a biztonsági szabály (portál)
Ezt követően kell tooadd egy biztonsági szabályt, amely a megnyitott portról hello tűzfalon keresztül irányítja a forgalmat.

1. Jelentkezzen be toohello portálon.
2. Keresés hello erőforráscsoport központilag telepített hello Azure Tárolószolgáltatás számára.
3. Jelölje be hello **nyilvános** ügynök hálózati biztonsági csoport (amely nevű hasonló túl**XXXX-ügynök – nyilvános-nsg-XXXX**).
   
    ![Azure-tárolót szolgáltatás hálózati biztonsági csoport](./media/container-service-enable-public-access/agent-nsg.png)
4. Válassza ki **bejövő biztonsági szabályok** , majd **Hozzáadás**.
   
    ![Azure-tárolót szolgáltatás hálózati biztonsági csoportszabályok](./media/container-service-enable-public-access/add-firewall-rule.png)
5. A nyilvános port hello tűzfal szabály tooallow kitöltése, és kattintson a **OK**.
   
   | Mező | Leírás |
   | --- | --- |
   | Név |Egy tűzfalszabály hello leíró nevet. |
   | Prioritás |Prioritás dimenziószáma hello szabályhoz. hello alacsonyabb hello számú hello hello elsőbbséget. |
   | Forrás |Korlátozza a hello bejövő IP cím tartomány toobe amelyet e szabály engedélyez vagy elutasít. Használjon **bármely** toonot korlátozás adja meg. |
   | Szolgáltatás |Válassza ki a biztonsági szabály csak az előre meghatározott szolgáltatásokat foglal. Ellenkező esetben használja **egyéni** toocreate saját. |
   | Protokoll |Korlátozzák a forgalmat a alapján **TCP** vagy **UDP**. Használjon **bármely** toonot korlátozás adja meg. |
   | Porttartomány |Ha **szolgáltatás** van **egyéni**, adja meg a hello azon portok tartományát, ez a szabály vonatkozik. Használhatja például egyetlen port, **80**, vagy egy tartományt, például **1024-1500**. |
   | Műveletek |Adatforgalom engedélyezéséhez vagy letiltásához hello feltételeknek megfelelő. |

## <a name="next-steps"></a>Következő lépések
További tudnivalók: hello különbségének [nyilvános és titkos DC/OS-ügynökök](container-service-dcos-agents.md).

További információt olvashat [a DC/OS-tárolók kezelése](container-service-mesos-marathon-ui.md).

