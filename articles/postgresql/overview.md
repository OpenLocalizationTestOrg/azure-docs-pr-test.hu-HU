---
title: "Azure-adatbázis relációs adatbázis-szolgáltatás PostgreSQL aaaOverview |} Microsoft Docs"
description: "Azure-adatbázis áttekintést nyújt a PostgreSQL relációs adatbázis-szolgáltatás."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 08/01/2017
ms.openlocfilehash: fd6821b56e5295b8b341d87b14d113f7a4b247c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Mi az Azure-adatbázis a PostgreSQL?

Azure-adatbázis PostgreSQL a hello nyílt forráskódú hello közösségi változata alapján fejlesztők számára a Microsoft felhőalapú relációs adatbázis-szolgáltatás [PostgreSQL](https://www.postgresql.org/) adatbázismotor. Ez a szolgáltatás nyilvános előzetes verziójában van. Azure-adatbázis PostgreSQL biztosítja:
- Több szolgáltatási szinten a kiszámítható teljesítmény
- Az alkalmazásleállás dinamikus méretezhetőség
- Beépített magas rendelkezésre állás
- Adatvédelem

Ezekre a képességekre igényel szinte semmilyen felügyeleti, és minden további költségek nélkül kínál. Ezek a képességek lehetővé teszik a Gyors alkalmazásfejlesztés és az idő toomarket felgyorsítása, hanem értékes időt és erőforrásokat toomanaging virtuális gépek és infrastruktúra toofocus. Emellett tovább toodevelop hello az alkalmazásba nyissa meg a forrás-eszközök és a kiválasztott platform és hello sebességgel és anélkül, hogy az új képességek toolearn követelményeket az üzleti hatékonyságot biztosítanak. 

Ez a cikk egy PostgreSQL alapfogalmakat és a kapcsolódó szolgáltatások tooperformance, méretezhetőség és kezelhetőség az adatbázis bemutatása tooAzure. Tekintse meg a quick elindul használatba tooget:

- [Hozzon létre egy Azure-adatbázis PostgreSQL Azure-portál használatával](quickstart-create-server-database-portal.md)
- [Hozzon létre egy Azure-adatbázis PostgreSQL hello Azure parancssori felület használatával](quickstart-create-server-database-azure-cli.md)

Több Azure CLI és PowerShell-mintát talál itt:

- [Azure CLI-minták PostgreSQL Azure-adatbázis](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Teljesítmény módosítása és skálázása leállási idő nélkül

Azure-adatbázis PostgreSQL szolgáltatás jelenleg két szolgáltatásszintek kínál: Basic és Standard. Minden egyes kínál [különböző szintű teljesítmény, IOPS garanciák és képességek](concepts-service-tiers.md) toosupport egyszerűsített tooheavyweight adatbázis számítási feladatait. Hozhat létre első alkalmazását egy kis kiszolgálón havi pár dollárért, majd [hello teljesítmény szint](scripts/sample-scale-server-up-or-down.md) szolgáltatáson belül réteg manuálisan vagy programon keresztül, a megoldás bármely idő toomeet hello igényeinek. Ezt megteheti a leállás tooyour alkalmazás vagy az ügyfelek tooyour nélkül. Dinamikus méretezhetőség lehetővé teszi, hogy az adatbázis tootransparently válaszolni toorapidly erőforrás-követelmények és lehetővé teszi, hogy Ön tooonly díj ellenében hello erőforrásokat kell, amikor szüksége van rájuk.

## <a name="monitoring-and-alerting"></a>Figyelés és riasztás
Hogyan Ön dönt, hogy mikor növekszik és csökken toodial? Hello beépített teljesítményfigyelő és riasztási szolgáltatásokat, hello teljesítmény minősítések számítási egység alapján együtt használja. Ezek az eszközök gyorsan felmérheti a számítási egység vertikális felskálázásával hello hatását vagy le a jelenlegi vagy tervezett teljesítmény igények alapján. További információkért lásd: [Azure adatbázis PostgreSQL beállításai és teljesítménye: az egyes szolgáltatásszinteken elérhető](./concepts-service-tiers.md).

## <a name="keep-your-app-and-business-running"></a>Biztosítsa alkalmazása és vállalkozása folyamatos működését
Azure iparágvezető 99,99 % (nem érhető el az előzetes verzió) rendelkezésre állási szolgáltatásiszint-szerződés (SLA), a Microsoft által kezelt adatbázisok globális hálózata technológiával segít a 24 és Windows 7 rendszerben futó alkalmazást. Minden Azure adatbázissal PostgreSQL-kiszolgáló használatakor élvezheti a beépített biztonsági hibatűrést és az adatvédelem, amelyeket egyébként külön toobuy vagy kialakítást, elkészítéséhez és kezeléséhez. Az Azure Database a PostgreSQL minden kínál széles választékát üzleti folytonosságot biztosító szolgáltatásokat és a beállítások mentése használható tooget helyezhet és futtathat ily módon. Használhat [pont időponthoz kötött visszaállítás](howto-restore-server-portal.md) tooreturn egy adatbázis tooan korábbi állapotba kerül, a kötött 35 napon. Emellett az adatbázisokat üzemeltető adatközpontban hello során kimaradás lép fel, adatbázisok a legutóbbi biztonsági mentés georedundáns másolatát a is helyreállíthatja.

## <a name="secure-your-data"></a>Az adatok védelme
Azure-adatbázis szolgáltatások rendelkeznek hagyományokkal adatok biztonságáról, hogy PostgreSQL az Azure-adatbázis garantálják a szolgáltatásokkal, amelyek korlátozzák a hozzáférést, adatokat nyugalmi és a mozgási védheti meg és tevékenységek figyeléséhez nyújt segítséget. A Microsoft hello [Azure biztonsági és adatkezelési központ](https://www.microsoft.com/TrustCenter/Security/default.aspx) Azure platform biztonsággal kapcsolatos információk.

hello Azure adatbázis PostgreSQL szolgáltatás adatokat nyugalmi tárolási titkosítást használ. Adatokat, többek között a biztonsági másolatokat a lemezen (kivétellel hello hello motor lekérdezések futtatása során hozza létre az ideiglenes fájlok) vannak titkosítva. hello szolgáltatást használja, amely megtalálható az Azure storage encryption AES 256 bites titkosítás, és hello kulcsok rendszer kezeli. Tárolás titkosítása mindig be van kapcsolva, és nem tiltható le.

Alapértelmezés szerint hello Azure adatbázis PostgreSQL szolgáltatás érték konfigurált toorequire [SSL-kapcsolat biztonsági](./concepts-ssl-connection-security.md) az adatokat a-mozgásérzékelő – hello hálózaton keresztül. Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.  Másik lehetőségként letilthatja SSL megkövetelése tooyour adatbázis-szolgáltatás Kapcsolódás, ha az ügyfélalkalmazást nem támogatja az SSL-kapcsolatot.

## <a name="next-steps"></a>Következő lépések
- Lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/postgresql/) a költségeinek összehasonlításáért és árkalkulációjáért.
- Első lépések [az első Azure-adatbázis létrehozása a PostgreSQL](./quickstart-create-server-database-portal.md).
- Az első alkalmazás Python, PHP, Ruby, C elkészítésére\#, Java, Node.js: [adatkapcsolattárak](./concepts-connection-libraries.md)
