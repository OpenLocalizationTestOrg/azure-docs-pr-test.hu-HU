---
title: "aaaHow tooconnect toodata források |} Microsoft Docs"
description: "Hogyan-tooarticle kiemelés felderítésének az Azure Data Catalog tooconnect toodata források módját."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 4e6b27a5-cf75-4012-b88c-333c1fe638e8
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 01d659510c8e67c1238ed488f4eebf511aab7217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-toodata-sources"></a>Hogyan tooconnect toodata források
## <a name="introduction"></a>Bevezetés
**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál. Más szóval **Azure Data Catalog** összes segítve a személyek ismerje meg, megértése és adatforrások használja, és segíti a szervezeteket tooget több értékét a meglévő adatok. Kulcsfontosságú ebben a forgatókönyvben az által használt hello adatforrást – Miután a felhasználó felderíti a adatokat, és tisztában van azzal a céllal, hello tovább lépés tooconnect toohello adatforrás tooput az adatok toouse.

## <a name="data-source-locations"></a>Adatforrás helye
Az adatforrás regisztrációja során **Azure Data Catalog** hello adatforrás metaadatainak kap. Ezeket a metaadatokat tartalmazza a hello hello az adatforrás helyét. forrás toodata adatforrásból változó hello hely hello részleteit, de mindig hello szükséges információkat tooconnect fogja tartalmazni. Például egy SQL Server tábla hello helyét tartalmaz hello kiszolgáló neve, a adatbázis neve, a séma neve és a táblázat nevét, miközben egy SQL Server Reporting Services jelentés hello helyét tartalmazza hello kiszolgálónevet és -hello elérési toohello jelentés. Más adatforrásokat helyeket az hello struktúra és hello forrásrendszerben képességeit megfelelően fog rendelkezni.

## <a name="integrated-client-tools"></a>Integrált ügyféleszközök
hello legegyszerűbb módja tooconnect tooa adatforrás toouse hello "Megnyitás..." hello menüjében **Azure Data Catalog** portálon. Ebben a menüben a Kapcsolódás a kiválasztott adategységet toohello beállítások listáját jeleníti meg.
Hello alapértelmezett mozaik elrendezés nézetben használata esetén érhető el ebben a menüben a hello mindegyik mozaiknál.

 ![Egy SQL Server tábla megnyitása Excel hello adatok eszköz csempe](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Hello listanézet használata esetén hello keresősávban hello portál ablakának felső hello hello menüjéből érhető el.

 ![Hello keresősáv származó a jelentéskezelőben egy SQL Server Reporting Services jelentés megnyitása](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Ügyfelek támogatott alkalmazások
Hello használata esetén "Megnyitás..." az adatok menü adatforrásokat hello Azure Data Catalog-portál, hello helyes ügyfélalkalmazás hello ügyfélszámítógépen telepíteni kell.

| Nyissa meg az alkalmazás | Kiterjesztése / protokoll | Alkalmazás támogatott verziói |
| --- | --- | --- |
| Excel |.odc |Az Excel 2010-es vagy újabb |
| Excel (első 1000) |.odc |Az Excel 2010-es vagy újabb |
| A Power Query |.xlsx |Excel 2016 vagy az Excel 2010 vagy az Excel 2013 hello Power Query az Excel-bővítmény telepítése |
| A Power BI Desktop |a .pbix |A Power BI Desktop július 2016 vagy újabb |
| SQL Server Data Tools |vsweb: / / |A Visual Studio 2013 Update 4 vagy újabb rendszert telepített SQL Server tooling |
| Jelentéskezelő |http:// |Lásd: [SQL Server Reporting Services böngészőre vonatkozó követelményei](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Az adatok, az eszközök
hello található hello menü lehetőségeit a jelenleg kiválasztott adategységet hello típusú függ. Természetesen nem minden lehetséges eszközök szerepelni fog a hello "Megnyitás..." menü, de még könnyen tooconnect toohello adatforrás bármely ügyfél eszközzel. Ha kiválasztott egy adategységet hello **Azure Data Catalog** portálon hello teljes helye látható hello tulajdonságok panelen.

 ![Egy SQL Server tábla-kapcsolódási információt](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

hello kapcsolatadatok részletei eltérőek forrás típusa toodata adatforrás típusa, de hello portal hello kacsolódjanak Erre azért van szükség minden tooconnect toohello adatforrás bármely ügyfél eszköz szükséges. Felhasználók hello adatforrások azok észlelt használatával hello kapcsolati adatainak másolhatja **Azure Data Catalog**, így toowork tetszőleges eszköz általi hello adatokat.

## <a name="connecting-and-data-source-permissions"></a>Csatlakozás és az adatforrás-engedélyek
Bár a **Azure Data Catalog** adatforrások felderíthető, access lesz toohello adatokat mozgatná hello adatok forrás tulajdonos vagy a rendszergazda hello vezérlése alatt marad. Az adatforrás felderítéséhez **Azure Data Catalog** nem segítségével a felhasználók bármilyen engedéllyel tooaccess hello adatforrás saját magát.

toomake adathalmazokban a felhasználók, akik egy adatok felderítése forrás-, de nem rendelkeznek engedéllyel tooaccess az adatokat, a felhasználók megadhatják hello hozzáférés kérése tulajdonság található információk amikor ellátása megjegyzésekkel adatforrás. – Beleértve a hivatkozások toohello folyamat és a data source hozzáférjenek kapcsolódási pontja – az itt megadott információk mellett hello adatforrás hely adatait hello portálon jelenik meg.

 ![A kérés hozzáférési utasításokat kapcsolatadatok](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Összefoglalás
Adatforrás regisztrálása **Azure Data Catalog** lehetővé teszi, hogy az adatok felderíthető másolásával szerkezeti és leíró metaadatok hello adatforrásból az Alkalmazáskatalógus webszolgáltatás hello. Miután regisztrált egy adatforrást, és felderített, a felhasználók kapcsolódhatnak-e a toohello adatforrás a hello **Azure Data Catalog** portal "Megnyitás..." " menü vagy a kiválasztott data tools használatával.

## <a name="see-also"></a>Lásd még:
* [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md) oktatóprogram lépésről lépésre részletei tooconnect toodata források.
