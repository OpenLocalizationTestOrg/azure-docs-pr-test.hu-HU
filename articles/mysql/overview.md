---
title: "a MySQL adatbázis-szolgáltatás Azure-adatbázis aaaOverview |} Microsoft Docs"
description: "Hello Azure-adatbázis a MySQL adatbázis-szolgáltatás áttekintése."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 08/02/2017
ms.custom: mvc
ms.openlocfilehash: f02493e2c3c38ccab408a718f98861600481812d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-database-for-mysql-service-introduction"></a>Mi az Azure-adatbázis a MySQL? Szolgáltatás bemutatása
Azure MySQL-adatbázis rendszer hello a Microsoft felhőalapú relációs adatbázis szolgáltatása [MySQL Community Edition](https://www.mysql.com/products/community/) adatbázismotor.  Azure MySQL-adatbázis biztosítja:

- Több szolgáltatási szinten a kiszámítható teljesítmény
- Az alkalmazásleállás dinamikus méretezhetőség
- Beépített magas rendelkezésre állás
- Adatvédelem

Ezek a képességek igényel szinte semmilyen felügyeleti, és minden további költségek nélkül kínál. Ezek lehetővé teszik az alkalmazások gyors fejlesztésére és az idő toomarket felgyorsítása, hanem értékes időt és erőforrásokat toomanaging virtuális gépek és infrastruktúra toofocus. Emellett tovább toodevelop hello az alkalmazásba nyissa meg a forrás-eszközök és a kiválasztott platform és hello sebességgel és anélkül, hogy az új képességek toolearn követelményeket az üzleti hatékonyságot biztosítanak.

![Azure-adatbázis a MySQL fogalmi diagramja](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Ez a cikk egy bevezető tooAzure adatbázis MySQL Alapfogalmak és szolgáltatások kapcsolódó tooperformance, méretezhetőség és kezelhetőség hivatkozások tooexplore adatokkal. Tekintse meg a quick elindul használatba tooget:
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával](quickstart-create-mysql-server-database-using-azure-cli.md)

Az Azure parancssori felület mintákat mutat be lásd:
- [Azure CLI-minták MySQL az Azure-adatbázis](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Teljesítmény módosítása és skálázása leállási idő nélkül
A MySQL-szolgáltatás Azure-adatbázis két szolgáltatásszintek kínál: Basic és Standard. Minden más-más teljesítménybeli és képességek toosupport egyszerűsített tooheavyweight adatbázis-terhelések kínál. Hozhat létre az első alkalmazás kisméretű adatbázis néhány dollár az egy hónap, majd módosítsa a szolgáltatási réteg tooscale a szükséges állásidő nélkül a megoldás. Dinamikus méretezhetőség lehetővé teszi, hogy az adatbázis tootransparently válaszoljon toorapidly erőforrásigények módosítása. Csak kell fizetnie hello szükséges erőforrások, ha szüksége van rájuk.

## <a name="monitoring-and-alerting"></a>Figyelés és riasztás
Honnan tudhatja hello kattintson a jobb gombbal-stop felfelé és lefelé tárcsázás? Hello beépített teljesítmény figyelés és riasztás szolgáltatások hello teljesítmény minősítések számítási egység alapján együtt használja. Használja ezeket a funkciókat, gyorsan felmérheti hello hatása skálázás felfelé vagy lefelé alapján a jelenlegi vagy a teljesítményigény projektre. Lásd: [fogalmak: szolgáltatásszintek](concepts-service-tiers.md) részleteiről.

## <a name="keep-your-app-and-business-running"></a>Biztosítsa alkalmazása és vállalkozása folyamatos működését
Azure iparágvezető 99,99 % rendelkezésre állási szolgáltatásiszint-szerződés (SLA), a Microsoft által kezelt adatbázisok globális hálózata technológiával segít a 24 és Windows 7 rendszerben futó alkalmazást. Minden Azure adatbázissal MySQL-kiszolgáló használatakor élvezheti a beépített biztonsági hibatűrést és az adatvédelem, amelyeket egyébként külön toobuy vagy kialakítást, elkészítéséhez és kezeléséhez. MySQL az Azure-adatbázissal, használhatja a kiszolgáló tooan pont időponthoz kötött visszaállítás toorecover korábbi állapotba kerül, a kötött 35 napon.

## <a name="secure-your-data"></a>Az adatok védelme
Azure-adatbázis szolgáltatások adatok biztonságáról, hogy a MySQL az Azure-adatbázis a szolgáltatásokkal, amelyek korlátozzák a hozzáférést, adatokat nyugalmi és a mozgási védheti meg és tevékenységek figyeléséhez nyújt segítséget garantálják hagyományokkal rendelkeznek. A Microsoft hello [Azure biztonsági és adatkezelési központ](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) Azure platform biztonsággal kapcsolatos információk.

hello Azure adatbázis MySQL szolgáltatás adatokat nyugalmi tárolási titkosítást használ. Adatok biztonsági mentése, beleértve a lemezen (kivétellel hello hello motor lekérdezések futtatása során hozza létre az ideiglenes fájlok) vannak titkosítva. hello szolgáltatást használja, amely megtalálható az Azure storage encryption AES 256 bites titkosítás, és hello kulcsok rendszer kezeli. Tárolás titkosítása mindig be van kapcsolva, és nem tiltható le.

Alapértelmezés szerint hello Azure-adatbázis a MySQL-szolgáltatás konfigurált toorequire [SSL-kapcsolat biztonsági](./concepts-ssl-connection-security.md) az adatokat a-mozgásérzékelő – hello hálózaton keresztül. Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.  Másik lehetőségként letilthatja SSL megkövetelése tooyour adatbázis-szolgáltatás Kapcsolódás, ha az ügyfélalkalmazást nem támogatja az SSL-kapcsolatot.

## <a name="next-steps"></a>Következő lépések
Most, hogy már egy bevezető tooAzure adatbázis olvasásakor: MySQL és hello kérdést megválaszolták "Mi az Azure MySQL-adatbázis?", készen áll:
- Lásd: hello árképzést ismertető oldalra a költségeinek összehasonlításáért és árkalkulációjáért. [Díjszabás](https://azure.microsoft.com/pricing/details/mysql/)
- Ismerkedés az első kiszolgáló létrehozásával. [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával](quickstart-create-mysql-server-database-using-azure-portal.md)
- Az első alkalmazás Python, PHP, Ruby, C elkészítésére\#, Java, Node.js: [kapcsolat szalagtárak használt tooconnect tooAzure adatbázis MySQL](concepts-connection-libraries.md)
