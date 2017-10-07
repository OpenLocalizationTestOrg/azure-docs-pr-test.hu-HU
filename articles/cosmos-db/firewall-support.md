---
title: "aaaAzure Cosmos DB tűzfaltámogatás & IP hozzáférés-vezérlés |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatják a toouse IP hozzáférés-vezérlési házirendeket tűzfal az Azure Cosmos DB adatbázis fiókot."
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
ms.date: 05/22/2017
ms.author: ankshah
ms.openlocfilehash: b5cdbdb28e9d7ee0fd0ea54aad277167b699929f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-firewall-support"></a>Az Azure Cosmos DB-tűzfaltámogatás
toosecure egy Azure Cosmos DB adatbázisfiók tárolt adatok, Azure Cosmos DB nyújtott támogatás a titkos kulcs alapú [engedélyezési modellt](https://msdn.microsoft.com/library/azure/dn783368.aspx) , amely erős kivonat-alapú üzenethitelesítő kódot (HMAC) használja. Most továbbá toohello titkos kulcs alapú engedélyezési modellt, Azure Cosmos DB támogatja az IP-alapú hozzáférés-vezérléssel bejövő tűzfaltámogatás vezérelt házirend. Ez a modell toohello tűzfalszabályok nagyon hasonló a hagyományos adatbázis rendszer, és egy további biztonsági toohello Azure Cosmos DB adatbázisfiók szintet. Ebben a modellben most is konfigurálhatja egy Azure Cosmos DB adatbázis fiók toobe csak egy jóváhagyott gépek halmazát jelenti érhetők el vagy a felhőalapú szolgáltatások. Hozzáférés tooAzure Cosmos DB erőforrásokhoz a gépek és szolgáltatások jóváhagyott halmazok továbbra is szükséges hello hívó toopresent érvényes engedélyezési jogkivonatot.

## <a name="ip-access-control-overview"></a>IP hozzáférés-vezérlés áttekintése
Alapértelmezés szerint egy Azure Cosmos DB adatbázisfiók nem érhető el a nyilvános interneten számít hello kérelem érvényes engedélyezési jogkivonatot együtt. tooconfigure IP csoportházirend-alapú hozzáférés-vezérlés, hello felhasználónak meg kell adnia IP-címek vagy IP-címtartományok engedélyezettek listájához, az ügyfél IP-címek egy adott adatbázis fiókjához tartozó hello néven CIDR formátumban toobe a hello készlete. Ha ez a konfiguráció van érvényben, kívül az engedélyezett bővítmények listájához készülékekről származó összes kérelem le lesz tiltva hello kiszolgáló.  hello IP-alapú hozzáférés-vezérlés folyamata feldolgozása hello kapcsolat a következő diagram hello.

![IP-alapú hozzáférés-vezérlési hello kapcsolódási folyamatot bemutató ábra](./media/firewall-support/firewall-support-flow.png)

## <a name="connections-from-cloud-services"></a>Felhőszolgáltatások közötti kapcsolatok
Az Azure felhőszolgáltatások, amelyek egy nagyon gyakori középső réteg szolgáltatás logika Azure Cosmos DB használatával üzemeltetéséhez. tooenable hozzáférés tooan Azure Cosmos DB adatbázisfiókot az egy felhőszolgáltatás, hello nyilvános IP-cím hello felhőalapú szolgáltatás által az Azure Cosmos DB adatbázisfiók társított IP-címek listája engedélyezett hozzáadott toohello kell lennie [konfigurálása hozzáférés-vezérlési házirend IP hello](#configure-ip-policy).  Ez biztosítja, hogy a felhőszolgáltatások szerepkör példányainak hozzáférés tooyour Azure Cosmos DB adatbázisfiók. IP-címek kérheti le a felhőszolgáltatások hello Azure-portálon, ahogy az alábbi képernyőfelvétel a hello.

![Képernyőfelvétel a hello Azure-portálon jelenik meg a felhőalapú szolgáltatás hello nyilvános IP-cím](./media/firewall-support/public-ip-addresses.png)

A horizontális a felhőszolgáltatás további szerepkör-példányokat, hozzáadásával, ha új logikailemez automatikusan rendelkezik hozzáférési toohello Azure Cosmos DB adatbázisfiók mert azok hello része megegyezik a felhőalapú szolgáltatás.

## <a name="connections-from-virtual-machines"></a>Virtuális gépek közötti kapcsolatok
[Virtuális gépek](https://azure.microsoft.com/services/virtual-machines/) vagy [virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) is használt toohost középső réteg szolgáltatások, Azure Cosmos DB használatával.  tooconfigure hello Azure Cosmos DB adatbázis fiók tooallow hozzáférést a virtuális gépek, virtuális gép és/vagy a virtuálisgép-méretezési csoport nyilvános IP-címeket úgy kell konfigurálni a rendelkezésre álló IP-címek az Azure Cosmos DB adatbázisfiók által engedélyezett hello [hello IP hozzáférés-vezérlési házirend beállítása](#configure-ip-policy). Ahogy az alábbi képernyőfelvétel a hello le hello Azure-portálon, a virtuális gépek IP-címet.

![Képernyőfelvétel egy nyilvános IP-címet a virtuális gép hello Azure-portálon jelenik meg](./media/firewall-support/public-ip-addresses-dns.png)

Amikor további virtuális gép toohello példánycsoport, azok számára automatikusan biztosított hozzáférés tooyour Azure Cosmos DB adatbázisfiók.

## <a name="connections-from-hello-internet"></a>A kapcsolatok hello internet
Amikor access, az Azure Cosmos DB adatbázisról egy számítógép-fiókot a hello internet, hello ügyfél IP-cím vagy IP-címtartomány hello gép kell hozzáadott toohello engedélyezett hello Azure Cosmos DB adatbázisfiók tartozó IP-címek listáját. 

## <a id="configure-ip-policy"></a>Hello IP hozzáférés-vezérlési házirend beállítása
hello IP hozzáférés-vezérlési szabályzatok állíthatók hello Azure-portálon vagy programozottan [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), vagy hello [REST API](/rest/api/documentdb/) hello frissítésével`ipRangeFilter` tulajdonság. IP-címeken/tartományokon vesszővel elválasztott, és nem tartalmazhat szóközt kell lennie. Példa: "13.91.6.132,13.91.6.1/24". Az adatbázisfiók keresztül ezen módszerek frissítésekor legyen, hogy toopopulate összes hello tulajdonságok tooprevent toodefault-beállításainak visszaállításáról.

> [!NOTE]
> Azáltal, hogy IP hozzáférés-vezérlési szabályzatok Azure Cosmos DB adatbázis fiókjához, minden hozzáférési tooyour gépekről kívül konfigurált hello Azure Cosmos DB adatbázisfiók engedélyezett IP-címtartományok listájának le vannak tiltva. Ez a modell alapján böngészés vezérlősík Adatműveletek hello hello portálról is letiltott tooensure hello integritását, hozzáférés-vezérlés.

toosimplify fejlesztési hello Azure portál segítségével azonosítsa és hello IP, az ügyfél gépek toohello engedélyezettek listájához, adja meg, hogy a gépen futó alkalmazások hozzáférhessenek hello Azure Cosmos DB fiók. Ügyeljen arra, hogy az hello ügyfél IP-cím szerinti hello portal észleli-e. Hello ügyfél IP-cím a gép lehet, de az is lehet a hálózati átjáró hello IP-címe. Ne feledje tooremove, amelyet tooproduction előtt.

tooset hello IP hozzáférés-vezérlési szabályzat az Azure-portálon hello toohello Azure Cosmos DB-fiók panelen keresse meg, kattintson a **tűzfal** hello navigációs menüben, majd kattintson a **ON** 

![Hogyan tooopen hello tűzfal panel az Azure-portálon hello ábrázoló képernyőfelvétel](./media/firewall-support/azure-portal-firewall.png)

Hello új panelen adja meg, hogy hello Azure-portálon is hello fiók eléréséhez és más címek és tartományok hozzáadása szükség szerint, majd kattintson **mentése**.  

> [!NOTE]
> Egy IP-hozzáférés-vezérlési házirend engedélyezésekor hello Azure portál toomaintain hozzáférést kell tooadd hello IP-cím. hello portál IP-címek a következők:
> |Régió|IP-cím|
> |------|----------|
> |Minden egyes megadott kivéve alatt| 104.42.195.92|
> |Németország|51.4.229.218|
> |Kína|139.217.8.252|
> |USA-beli államigazgatás – Arizona|52.244.48.71|
>

![Képernyőfelvétel egy hogyan tooconfigure tűzfal beállításaival hello Azure-portálon](./media/firewall-support/azure-portal-firewall-configure.png)

## <a name="troubleshooting-hello-ip-access-control-policy"></a>Hibaelhárítási hello IP hozzáférés-vezérlési házirend
### <a name="portal-operations"></a>Portál műveletek
Azáltal, hogy IP hozzáférés-vezérlési szabályzatok Azure Cosmos DB adatbázis fiókjához, minden hozzáférési tooyour gépekről kívül konfigurált hello Azure Cosmos DB adatbázisfiók engedélyezett IP-címtartományok listájának le vannak tiltva. Ezért ha azt szeretné, hogy tooenable portál adatok vezérlősík műveletek tartoznak, mint a gyűjtemények és dokumentumok lekérdezés tallózása, meg kell tooexplicitly hozzáférést Azure portál használatával hello **tűzfal** hello portál panel. 

![Képernyőfelvétel egy hogyan tooenable hozzáférés toohello Azure-portálon](./media/firewall-support/azure-portal-access-firewall.png)

### <a name="sdk--rest-api"></a>SDK & Rest API
A biztonsági okokból nem engedélyezettek listájához hello gépek SDK vagy a REST API-n keresztül hozzáférést egy általános 404-es nem található response további részletek nem ad vissza. Ellenőrizze, hogy hello IP az engedélyezett listában konfigurálva az Azure Cosmos DB adatbázis fiók tooensure hello megfelelő házirendeket alkalmazott tooyour Azure Cosmos-adatbázis adatbázis-fiók.

## <a name="next-steps"></a>Következő lépések
További információ a hálózati kapcsolatos teljesítményadatokat tippek: [teljesítmény tippek](performance-tips.md).

