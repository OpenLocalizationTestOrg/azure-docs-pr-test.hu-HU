---
title: "Az Azure Cosmos DB tűzfaltámogatás & IP hozzáférés-vezérlés |} Microsoft Docs"
description: "Megtudhatja, hogyan IP hozzáférés-vezérlési házirendeket tűzfal támogatásához az Azure Cosmos DB adatbázis fiókot használjon."
keywords: "IP-hozzáférés-vezérlés, tűzfaltámogatás"
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: ankshah
ms.openlocfilehash: 85f5e0e076f92afc79ed8f4ce652bb0f31923113
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/03/2018
---
# <a name="azure-cosmos-db-firewall-support"></a>Az Azure Cosmos DB-tűzfaltámogatás
Egy Azure Cosmos-adatbázis adatbázis-fiókban tárolt adatok védelme érdekében Azure Cosmos DB nyújtott támogatás a titkos kulcs alapú [engedélyezési modellt](https://msdn.microsoft.com/library/azure/dn783368.aspx) , amely erős kivonat-alapú üzenethitelesítő kódot (HMAC) használja. Most a titkos alapú engedélyezési modell mellett Azure Cosmos DB bejövő tűzfaltámogatás IP-alapú hozzáférés-vezérléssel vezérelt házirend támogatja. Ebben a modellben a hagyományos adatbázis rendszer tűzfalszabályok hasonló, és egy további Azure Cosmos DB adatbázis fiókhoz biztonsági szintet. Ez a modell segítségével mostantól beállíthatja egy Azure Cosmos DB adatbázisfiók csak egy jóváhagyott gépek halmazát jelenti érhetők el, illetve a felhőalapú szolgáltatások. A jóváhagyott halmazok gépek és szolgáltatások Azure Cosmos DB erőforrásokhoz való hozzáférés továbbra is szükség van egy érvényes hitelesítési jogkivonatot a hívó.

## <a name="ip-access-control-overview"></a>IP hozzáférés-vezérlés áttekintése
Alapértelmezés szerint egy Azure Cosmos DB adatbázisfiók érhető el nyilvános internetről mindaddig, amíg a kérelem egy érvényes engedélyezési jogkivonat együtt. IP-csoportházirend-alapú hozzáférés-vezérlés konfigurálása, a felhasználónak meg kell adnia az IP-címek vagy IP-címtartományt a CIDR formátumban része, mint az engedélyezett bővítmények listájához ügyfél IP-címek egy adott adatbázis fiókjához tartozó készletét. Ha ez a konfiguráció alkalmazása esetén a kiszolgáló blokkolja a kívül az engedélyezett bővítmények listájához készülékekről származó összes kérelem.  A kapcsolat feldolgozása a IP-alapú hozzáférés-vezérlés folyamata a következő ábra szemlélteti:

![IP-alapú hozzáférés-vezérlés folyamata bemutató ábra](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a>Az IP-hozzáférés-vezérlési házirend beállítása
Az IP-hozzáférés-vezérlési szabályzat állítható be az Azure portálon vagy programozottan a [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), vagy a [REST API](/rest/api/documentdb/) a frissítésével**ipRangeFilter** tulajdonság. 

Az IP-hozzáférés-vezérlési házirend beállítása az Azure portálon, nyissa meg az Azure Cosmos DB fiók lapot, kattintson a **tűzfal** a navigációs menü, majd módosítsa a **IP hozzáférés-vezérlés engedélyezése** érték**ON**. 

![Képernyőfelvétel: a tűzfal lap megnyitása a az Azure-portálon](./media/firewall-support/azure-portal-firewall.png)

IP hozzáférés-vezérlés válik, a portál biztosít engedélyezze a hozzáférést az Azure-portálon, más Azure-szolgáltatásokkal és az aktuális IP-kapcsolókra. Kapcsolók További információt a következő szakaszokban találhatók.

![Képernyőfelvétel egy hogyan tűzfal beállításainak konfigurálása az Azure-portálon](./media/firewall-support/azure-portal-firewall-configure.png)

> [!NOTE]
> Azáltal, hogy IP hozzáférés-vezérlési szabályzatok Azure Cosmos DB adatbázis fiókjához, minden Azure Cosmos DB adatbázis fiókja gépekről kívül a beállított elérését az IP-címtartományok listájának le vannak tiltva. Ez a modell alapján keresse meg az vezérlősík művelet a portálról is blokkolja a hozzáférés-vezérlés sértetlenségének biztosítása érdekében.

## <a name="connections-from-the-azure-portal"></a>Azure-portálról kapcsolatok 

Amikor egy IP-hozzáférés-vezérlési házirend programozott módon engedélyezi, hozzá kell adnia az IP-címek az Azure-portál a **ipRangeFilter** fér tulajdonság. A portál IP-címek a következők:

|Régió|IP-cím|
|------|----------|
|Minden egyes megadott kivéve alatt|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Németország|51.4.229.218|
|Kína|139.217.8.252|
|USA-beli államigazgatás|52.244.48.71|

Az Azure-portálon, az Azure portálon keresztül történő elérésének beállítása a **hozzáférés engedélyezése az Azure-portálra** egy érték **ON** az Azure portálon (a **IP hozzáférés-vezérlés engedélyezése** érték meg kell **ON** megtekintése és módosítása a **hozzáférés engedélyezése az Azure-portálra** érték).

![Képernyőfelvétel: az Azure portál hozzáférés engedélyezése](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-other-azure-paas-services"></a>A többitől Azure PaaS kapcsolatok 
Az Azure PaaS-szolgáltatásokat, mint az Azure Stream analytics, az Azure Functions és Azure App Service szolgáltatásban használt Azure Cosmos DB együtt. Azure Cosmos DB való hozzáférésének engedélyezésére ezeket a szolgáltatásokat, amelyek IP-címei nem azonnal elérhetők legyenek az adatbázis-fiók hozzáadása a 0.0.0.0 IP-címét az engedélyezett bővítmények listájához programozott módon a Azure Cosmos DB adatbázis fiókjához társított IP-címek, vagy állítsa a **Azure-szolgáltatásokhoz való hozzáférés engedélyezése** értéke ON értékre állítása, az Azure portálon (a **IP hozzáférés-vezérlés engedélyezése** értéket úgy kell beállítani, **ON** megtekintése és módosítása a **engedélyezése Azure-szolgáltatásokhoz való hozzáférés** érték). Ez biztosítja, hogy Azure PaaS szolgáltatások férhessenek hozzá az Azure Cosmos DB fiók. 

![Képernyőfelvétel: a tűzfal lap megnyitása a az Azure-portálon](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Az aktuális IP-kapcsolatok

Egyszerűbbé teheti a fejlesztési, az Azure-portálon segítségével azonosítsa és adja meg az IP-címe az ügyfélszámítógép az engedélyezettek listájához, hogy a gépen futó alkalmazások hozzáférhessenek az Azure Cosmos DB fiók. Az ügyfél IP-cím szerinti a portál észlel. Elképzelhető, hogy az ügyfél IP-cím a gép, de az is lehet a hálózati átjáró IP-címét. Ne feledje éles előtt távolítsa el.

![A bemutató Képernyőkép az aktuális IP-tűzfal beállításainak konfigurálása](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Felhőszolgáltatások közötti kapcsolatok
Az Azure felhőszolgáltatások, amelyek egy közös középső réteg szolgáltatás logika Azure Cosmos DB használatával üzemeltetéséhez. Hozzáférés engedélyezése egy Azure Cosmos-adatbázis adatbázis-fiókhoz az egy felhőszolgáltatás, a felhőalapú szolgáltatás nyilvános IP-címe hozzá kell adni az engedélyezettek listájához, az IP-címek az Azure Cosmos DB adatbázisfiók által társított [az IP-hozzáférés konfigurálása Szabályozza a házirend](#configure-ip-policy).  Ez biztosítja, hogy a felhőalapú szolgáltatások minden szerepkör példányát az Azure Cosmos DB adatbázisfiók hozzáféréssel rendelkeznek. IP-címek kérheti le az Azure-portálon a felhőszolgáltatások, az alábbi képernyőfelvételen látható módon:

![Képernyőfelvétel: a nyilvános IP-cím egy felhőalapú szolgáltatás, az Azure-portálon jelenik meg](./media/firewall-support/public-ip-addresses.png)

A horizontális a felhőszolgáltatás további szerepkör-példányokat, hozzáadásával, ha új logikailemez automatikusan hozzáférhetnek az Azure Cosmos DB adatbázisfiók óta az ugyanazon a felhőalapú szolgáltatás részét képezik.

## <a name="connections-from-virtual-machines"></a>Virtuális gépek közötti kapcsolatok
[Virtuális gépek](https://azure.microsoft.com/services/virtual-machines/) vagy [virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) is használható Azure Cosmos DB használatával középső réteg szolgáltatások futtatásához.  Az Azure Cosmos DB adatbázis fiókot, hogy a hozzáférést a virtuális gépek, a virtuális gép és/vagy a virtuális gép nyilvános IP-címek konfigurálása méretezési be kell állítani az engedélyezett IP-címek az Azure Cosmos DB adatbázis fiókjához tartozó egyik által[IP hozzáférés-vezérlési házirend beállítása](#configure-ip-policy). Az Azure portálon, a virtuális gépek IP-címet az alábbi képernyőfelvételen látható módon kérheti le.

![Képernyőfelvétel egy nyilvános IP-címet a virtuális gép az Azure-portálon jelenik meg](./media/firewall-support/public-ip-addresses-dns.png)

Ha további virtuálisgép-példányok felvétele a csoportba, azok automatikusan kapnak hozzáférést az Azure Cosmos DB adatbázisfiók.

## <a name="connections-from-the-internet"></a>Az internetről érkező kapcsolatokat
Amikor egy Azure Cosmos DB adatbázisfiók fér hozzá egy számítógépről az interneten, az ügyfél IP-cím vagy IP-címtartomány a gép hozzá kell adni az Azure Cosmos DB adatbázis fiókjához tartozó IP-cím az engedélyezett bővítmények listájához. 

## <a name="troubleshooting-the-ip-access-control-policy"></a>Hibaelhárítás az IP-hozzáférés-vezérlési házirend
### <a name="portal-operations"></a>Portál műveletek
Azáltal, hogy IP hozzáférés-vezérlési szabályzatok Azure Cosmos DB adatbázis fiókjához, minden Azure Cosmos DB adatbázis fiókja gépekről kívül a beállított elérését az IP-címtartományok listájának le vannak tiltva. Ezért ha portál adatok vezérlősík műveletek tartoznak, mint a gyűjtemények és dokumentumok lekérdezés tallózása engedélyezni szeretné, akkor kell explicit módon az Azure portál hozzáférés engedélyezése a **tűzfal** oldal a portálon. 

![A bemutató Képernyőkép az Azure-portálon való hozzáférés engedélyezése](./media/firewall-support/enable-azure-portal.png)

### <a name="sdk--rest-api"></a>SDK & Rest API
A biztonsági okokból nem található az engedélyezési listához gépek SDK vagy a REST API-n keresztül hozzáférést egy általános 404-es nem található response további részletek nem ad vissza. Ellenőrizze a megfelelő házirend-konfiguráció alkalmazásának Azure Cosmos DB adatbázis fiókjába engedélyezettek listájához, a Azure Cosmos DB adatbázis fiókjához beállított IP.

## <a name="next-steps"></a>További lépések
A teljesítmény a hálózattal kapcsolatos tippek kapcsolatos információkért lásd: [teljesítmény tippek](performance-tips.md).

