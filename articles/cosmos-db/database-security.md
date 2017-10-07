---
title: "aaaDatabase biztonsági - Azure Cosmos DB |} Microsoft Docs"
description: "Ismerje meg, hogy az Azure Cosmos DB biztosítja az adatok az adatbázis védelmét, és az adatok biztonsági."
keywords: "nosql-adatbázis biztonsági, információ-biztonság, adatok biztonsága, az adatbázis-titkosítás, az adatbázis védelme, biztonsági házirendeket, biztonsági tesztelése"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: a02a6a82-3baf-405c-9355-7a00aaa1a816
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: mimig
ms.openlocfilehash: 85ffa62f611bfad00bf3fc5dbe536f91f97f1113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-database-security"></a>Azure Cosmos DB adatbázis biztonsági

Ez a cikk ismerteti, amelyek adatbázis ajánlott biztonsági eljárások és a legfontosabb jellemzők Azure Cosmos DB toohelp által kínált, megakadályozása, észlelésében és kezelésében toodatabase megszegése.
 
## <a name="whats-new-in-azure-cosmos-db-security"></a>Azure Cosmos DB biztonsági újdonságai?

Titkosítását már elérhető a dokumentumok és Azure Cosmos DB minden Azure régióban található biztonsági másolataihoz. Aktívan titkosítása automatikusan alkalmazza az új és meglévő ügyfelek ezeken a területeken. Nincs nincs szükség tooconfigure semmit; és a kapott hello azonos nagy késleltetésű, teljesítményt, rendelkezésre állási és funkció, előtt a hello előnye, hogy tudnák, az adatok titkosítását a biztonságos.

## <a name="how-do-i-secure-my-database"></a>Hogyan biztonságos a saját adatbázis? 

Adatok biztonsági megosztott feladata, hello ügyfél és az adatbázis-szolgáltató között. Hello mennyisége felelősségi hajt hello adatbázis-szolgáltató úgy dönt, attól függően eltérőek lehetnek. Ha úgy dönt, hogy egy helyszíni megoldás, kell tooprovide a végpont védelmi toophysical biztonsági hardver -, amely nem egyszerű feladat. Ha úgy dönt, hogy a PaaS felhő adatbázis-szolgáltató például az Azure Cosmos DB, jelentősen zsugorítja probléma, amely az Ön lakóhelyén. lemezkép, tárolt, közös a Microsoft következő hello [megosztott feladatkörök a felhőalapú informatika](https://aka.ms/sharedresponsibility) tanulmány, bemutatja, hogyan Ön felelőssége csökkenti, például Azure Cosmos DB PaaS szolgáltatóhoz.

![Ügyfél- és adatbázis-szolgáltató feladatkörei](./media/database-security/nosql-database-security-responsibilities.png)

hello előző ábra azt mutatja be magas szintű felhőalapú biztonsági összetevők, de milyen elemek szükséges tooworry kapcsolatos kifejezetten az adatbázis-megoldás? És hogyan lehet összehasonlítja más megoldások tooeach? 

Azt javasoljuk, hogy a következő ellenőrzőlista követelmények melyik toocompare adatbázis rendszereken hello:

- Hálózati biztonság és a tűzfal beállításai
- Felhasználó hitelesítése és részletes felhasználói vezérlők
- Képes tooreplicate adatok globálisan a regionális meghibásodásokkal
- Képes tooperform egy adatainak indított feladatátvételi tesztek center tooanother
- Helyi adatreplikációját adatközponton belül
- Automatikus biztonsági mentések
- A biztonsági mentésekből törölt adatok helyreállítása
- Védeni, és a bizalmas adatok elkülönítése
- A támadások figyelése
- Tooattacks válaszol
- Képes toogeo-időkorlát adatok tooadhere toodata irányítás korlátozások
- Fizikai kiszolgálók védett adatközpontokban védelme

És bár nyilvánvaló, legutóbbi tűnhet [nagyméretű adatbázis behatolás](http://thehackernews.com/2017/01/mongodb-database-security.html) hello egyszerű, de a kritikus fontosságú hello követelményeknek, emlékeztesse us:
- Javított kiszolgálók, melyek folyamatosan toodate mentése
- Alapértelmezett/SSL-titkosítás által HTTPS
- Az erős jelszóval rendelkező rendszergazdai fiókok

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Hogyan Azure Cosmos DB saját adatbázis védelme?

Nézzük vissza listát megelőző hello - hány biztonsági követelményekről Azure Cosmos DB nyújt? Minden egyetlen egyes.

Most dig részletesen mindegyiknél be.

|Biztonsági követelmény|Azure Cosmos-adatbázis biztonsági módszer|
|---|---|---|
|Hálózati biztonság|Az IP-tűzfal használata van hello védelmi toosecure első réteget az adatbázis. Azure Cosmos DB támogatja az IP-alapú hozzáférés-vezérléssel bejövő tűzfaltámogatás vezérelt házirend. hello IP-alapú hozzáférés-vezérlést hagyományos adatbázis-rendszerek által használt hasonló toohello tűzfalszabályok, de úgy, hogy az Azure Cosmos DB adatbázisfiók csak érhető el a gépek vagy felhőszolgáltatások jóváhagyott készlete bontva. <br><br>Azure Cosmos DB lehetővé teszi, hogy Ön tooenable egy adott IP-cím (168.61.48.0), egy IP-címtartomány (168.61.48.0/8) és IP-címek és tartományok kombinációi. <br><br>Az engedélyezett bővítmények listájához kívül gépekről származó összes kérelem Azure Cosmos DB blokkolja. Érkező kéréseket jóváhagyni a gépek és felhőszolgáltatások kell fejezze hello hitelesítési folyamat toobe megadott vezérlő toohello erőforrások eléréséhez.<br><br>További információ: [Azure Cosmos DB tűzfaltámogatás](firewall-support.md).|
|Engedélyezés|Azure Cosmos-adatbázis kivonat-alapú üzenethitelesítési kóddal (HMAC) használ a hitelesítéshez. <br><br>Minden egyes kérelem kivonatolt hello fiók titkos kulccsal, és minden hívás tooAzure Cosmos DB hello későbbi base-64 kódolású kivonatoló jut. toovalidate hello kérelmezéséhez hello Azure Cosmos DB szolgáltatás által használt hello megfelelő titkos kulccsal és tulajdonságok toogenerate kivonatát, majd összeveti hello hello kérelem hello értéket. Ha hello két érték egyezik, hello művelet sikeresen engedélyezve van, és hello kérés feldolgozása van, egyébként engedélyezési hiba és a hello vonatkozó kérés elutasítva.<br><br>Választhat egy [főkulcs](secure-access-to-data.md#master-keys), vagy egy [erőforrás-jogkivonat](secure-access-to-data.md#resource-tokens) lehetővé téve a minden részletre kiterjedő hozzáférési tooa erőforrás, például egy dokumentum.<br><br>További információ: [tooAzure Cosmos DB erőforrások biztonságossá tétele a hozzáférés](secure-access-to-data.md).|
|Felhasználók és engedélyek|Hello segítségével [főkulcs](#master-key) hello fiók felhasználói engedélyt erőforrásokat adatbázisonként és hozhat létre. A [erőforrás-jogkivonat](#resource-token) társított adatbázis engedélyt, és meghatározza, hogy hello felhasználói hozzáférési (írható-olvasható, csak olvasható, vagy nincs hozzáférése) tooan alkalmazás erőforrás hello adatbázisban. Alkalmazás erőforrások közé tartoznak a gyűjtemények, dokumentumok, a mellékletek, tárolt eljárások, eseményindítók és felhasználó által megadott függvények. erőforrás-jogkivonat hello majd hitelesítési tooprovide során használt vagy megtagadja a hozzáférést toohello erőforrás.<br><br>További információ: [tooAzure Cosmos DB erőforrások biztonságossá tétele a hozzáférés](secure-access-to-data.md).|
|Active directory-integráció (RBAC)| Hozzáférés toohello adatbázisfiókot hozzáférés-vezérlés (IAM) hello Azure-portálon a táblázatot követő hello képernyőfelvételen látható módon is megadhatja. IAM szerepköralapú hozzáférés-vezérlés és jól integrálható az Active Directory. A beépített szerepkörök vagy egyéni szerepkörök használható személyeknek és csoportoknak hello kép a következő ábrán.|
|Globális replikáció|Azure Cosmos DB kínál kulcsrakész globális terjesztési, ami lehetővé teszi a tooreplicate az adatok tooany rendelkező hello Azure világszerte adatközpontokra egyik kattintson egy gomb. Globális replikációs globálisan méretezhető legyen, és adja meg a kis késleltetésű access tooyour adatok körül hello world teszi lehetővé.<br><br>A biztonsági hello környezetében globális replikációs regionális meghibásodásokkal szemben az adatvédelem biztosítja.<br><br>További információ: [adatok globálisan terjesztése](distribute-data-globally.md).|
|Regionális feladatátvétel|Ha az adatok több adatközpont replikálása, Azure Cosmos DB automatikusan áthalad az operatív kell egy regionális adatközpont kapcsolat nélküli módba. Hozhat létre feladatátvevő régiókat hello régióban, amelyben a rendszer replikálja az adatokat a rangsorolt listáját. <br><br>További információ: [regionális feladatátvétel az Azure Cosmos Adatbázisba](regional-failover.md).|
|A helyi replikációt|Még belül egyetlen adatközpontba, Azure Cosmos DB automatikusan replikálja az adatokat a magas rendelkezésre állású, hello választott [konzisztenciaszintek](consistency-levels.md). Ez biztosítja, hogy egy [hasznos üzemidő rendelkezésre állás 99,99 % SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) és pénzügyi garanciális - valami más adatbázis-szolgáltatás által biztosított származnak.|
|Online biztonsági mentések automatikus|Az Azure Cosmos DB adatbázisok rendszeresen végeznek biztonsági mentést és egy georedundant tárolja. <br><br>További információ: [automatikus online biztonsági mentés és helyreállítás Azure Cosmos DB](online-backup-and-restore.md).|
|Törölt adatok helyreállításához|hello automatizált az online biztonsági mentés lehet előfordulhat, hogy véletlenül törölt mentése túl használt toorecover adatok ~ 30 nap hello esemény után. <br><br>További információ: [automatikus online biztonsági mentés és helyreállítás Azure Cosmos DB](online-backup-and-restore.md)|
|Védeni, és a bizalmas adatok elkülönítése|A felsorolt hello régiókban található összes adat [Újdonságok?](#whats-new) most titkosítása.<br><br>Elkülönített toospecific gyűjtemények és írható-olvasható lehet személyhez köthető adatokat és más bizalmas adatokat, vagy csak olvasási hozzáféréssel korlátozott toospecific felhasználók.|
|A figyelő támadások|A naplózás és tevékenységi naplóit, a normál és rendellenes tevékenységet fiókjának lehet figyelni. Tekintse meg az erőforrások, amikor hello művelet történt, hello műveletet, és sokkal több, mint a táblázatot követő hello képernyőképe látható hello állapotának hello művelet elindító a végrehajtott műveleteket.|
|Válaszoljon tooattacks|Miután az Azure támogatási tooreport potenciális támadási csatlakoztak, a program egy 5. lépés az incidensekre adott reakciók folyamat kezdődött el. hello hello 5 lépéses folyamat célja toorestore normál szolgáltatás biztonsága és műveletei minél gyorsabban után a rendszer problémát észlelt, és a vizsgálat elindult.<br><br>További információ: [Microsoft Azure biztonsági válasz hello felhő](https://aka.ms/securityresponsepaper).|
|Geokerítések|Azure Cosmos DB biztosítja az adatok irányítási és a megfelelőségi szuverén régiókhoz (például Németország, Kína Velünk – (kormányzati)).|
|Védett létesítményekben|Az Azure Cosmos Adatbázisba tárolja az Azure által védett adatközpontokban SSD-k.<br><br>További információ: [Microsoft globális adatközpont](https://www.microsoft.com/en-us/cloud-platform/global-datacenters)|
|HTTPS/SSL/TLS titkosítás|Minden ügyfél – szolgáltatás Azure Cosmos DB kapcsolati SSL/TLS 1.2 kényszerített. Minden helyen belüli datacenter és határokon datacenter replikációs is SSL/TLS 1.2 kényszerített.|
|Titkosítás inaktív állapotban|Az Azure Cosmos DB tárolt összes adat titkosítása. További információ: [Azure Cosmos DB titkosítását](.\database-encryption-at-rest.md)|
|Javított kiszolgálók|Felügyelt adatbázisként Azure Cosmos DB hello kell toomanage és javítás kiszolgálók által elvégzett, automatikusan megszünteti.|
|Az erős jelszóval rendelkező rendszergazdai fiókok|Annak rögzített toobelieve azt is kell toomention ezt a követelményt, de eltérően néhány a versenytársak lehetetlen toohave egy rendszergazdai fiókot az Azure Cosmos adatbázis jelszó nélküli.<br><br> Az SSL és HMAC titkos alapú hitelesítéssel biztonsági alapértelmezés szerint a sütik.|
|Biztonság és a data protection minősítései közül|Az Azure Cosmos DB rendelkezik [ISO 27001](https://www.microsoft.com/en-us/TrustCenter/Compliance/ISO-IEC-27001), [európai modell záradékok (EUKB)](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses), és [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) minősítései közül. További minősítései közül jelenleg folyamatban vannak.|

hello alábbi képernyőfelvételen látható Active directory-integráció (RBAC) használata a hozzáférés-vezérlés (IAM) a hello Azure-portálon: ![hello Azure portál – az adatbázis, amely tartalmazza a hozzáférés-vezérlés (IAM)](./media/database-security/nosql-database-security-identity-access-management-iam-rbac.png)

hello alábbi képernyőfelvételen látható naplózásra használatát, és a tevékenység naplókat toomonitor fiókját: ![tevékenység naplózza az Azure Cosmos DB rendszerhez](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Következő lépések

Főkulcsok és erőforrás-jogkivonatokat kapcsolatos további részletekért lásd: [Securing hozzáférés tooAzure Cosmos DB adatok](secure-access-to-data.md).

Microsoft minősítései közül kapcsolatos további tudnivalókért lásd: [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/).
