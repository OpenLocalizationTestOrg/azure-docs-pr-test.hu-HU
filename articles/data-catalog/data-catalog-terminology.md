---
title: "a Data Catalog terminológia aaaAzure |} Microsoft Docs"
description: "Ez a cikk egy bevezető tooconcepts és kifejezés, amely az Azure Data Catalog dokumentációja tartalmazza."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: b5f071db4f62c914d2c1cdef9aa686b18d5297d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-terminology"></a>Az Azure Data Catalog-terminológia
## <a name="catalog"></a>Alkalmazáskatalógus
Azure Data Catalog hello tárháza felhőalapú metaadatok, amely során az adatforrások és az eszközök regisztrálhatók. hello katalógus egy központi tárolási helye az adatforrások kinyert metaadatokat, és a felhasználók által hozzáadott leíró metaadatok funkcionál.

## <a name="data-source"></a>Adatforrás
Egy adatforrás, a rendszer vagy a tároló, amely kezeli az adategységeket. Ilyen például az SQL Server-adatbázisok, Oracle-adatbázisok, SQL Server Analysis Services adatbázisok (táblázatos vagy többdimenziós) és az SQL Server Reporting Services-kiszolgálók.

## <a name="data-asset"></a>Adategységet
Adategységek hello katalógus regisztrálhatók adatforrások objektumok. Ilyen például az SQL Server-táblák és nézetek, Oracle táblák és nézetek, SQL Server Analysis Services intézkedések, dimenziók és KPI-k, és az SQL Server Reporting Services-jelentések.

## <a name="data-asset-location"></a>Adatok eszköz helye
hello katalógus tárolja hello helyét egy adatforrás vagy az adategységhez, amely lehet használt tooconnect toohello ügyfélalkalmazás használatával. hello formátum és hello hely részleteit hello adatforrástípust függően változhat. Például egy SQL Server tábla is lehet a név által meghatározott négy rész – kiszolgálónév, az adatbázisnév, sémanév, objektumnév – során egy SQL Server Reporting Services jelentés azonosíthatja azokat az URL-CÍMÉT.

## <a name="structural-metadata"></a>Szerkezeti metaadatok
Szerkezeti metaadatai hello metaadatokat egy adatforrásban, amely egy adategységet hello szerkezete ismerteti. Ez magában foglalja a hello eszközök hellyel, az objektum neve és típusa és további típusra vonatkozó mutatók. Hello szerkezeti metaadatok táblák és nézetek magukban foglalják például hello nevét és hello objektum oszlopok adattípusát.

## <a name="descriptive-metadata"></a>Leíró metaadatok
Leíró metaadatok hello célját vagy egy adategységet célját leíró metaadatok. Általában hozzáadott leíró metaadatok katalógus felhasználó hello Azure Data Catalog-portált használja, de azt is történő kinyerésének hello adatforrás regisztráció során. Hogy hello Azure Data Catalog regisztráló eszközzel például kibontása leírások az hello Description tulajdonságot az SQL Server Analysis Services és az SQL Server Reporting Services és a hello [bővített tulajdonságms_description](https://technet.microsoft.com/library/ms190243.aspx)az SQL Server-adatbázisok, ha ezek a tulajdonságok vannak-e töltve értékekkel.

## <a name="request-access"></a>Hozzáférés kérése
Egy adategységet leíró metaadatok közé tartozhatnak a hogyan toorequest adatforrásának toohello eszköz vagy az adatok. Ezek az információk hello adatok eszköz helye számára jelenik meg, és tartalmazhatnak hello alábbi beállítások közül:

* hello e-mail címe hello felhasználó vagy csoport hozzáférési toohello adatforrás felelős.
* hello hello URL-CÍMÉT, hogy a felhasználók kell követnie toogain hozzáférés toohello adatforrás folyamat ismertetését.
* egy identitás- és hozzáférés-kezelési eszköz (például a Microsoft Identity Manager), amely használt toogain hozzáférés toohello adatforrás hello URL-címe
* Egy szabad szöveges bejegyzést, amely leírja, hogyan kaphatnak a felhasználók a hozzáférés toohello adatforrás.

## <a name="preview"></a>Előzetes verzió
Az Azure Data Catalog előnézete too20 rekordok hello adatforrás kinyert a regisztráció során, és hello adatok eszköz metaadatokkal hello-katalógusban tárolt pillanatképe nem. hello alapján könnyebben felhasználók, akik egy adategységet felderítése jobb megértése érdekében a funkciójára és céljára. Ez azt jelenti megtekintheti a mintaadatok lehet a több mint jelent meg, csak hello oszlopneveket és adattípusokat.
Az előzetes verziójú funkciók csak a táblák és nézetek támogatják, és kell külön megadnia hello felhasználói regisztráció során.

## <a name="data-profile"></a>Adatok profil
Az Azure Data Catalog adatok profil egy regisztrált eszköz hello adatforrás kinyert a regisztráció során, és hello adatok eszköz metaadatokkal hello-katalógusban tárolt tábla- és oszlop metaadatainak pillanatképet. hello adatok profil segítségével felhasználók, akik egy adategységet felderítése jobb megértése érdekében a funkciójára és céljára. Hasonló toopreviews, adatok profilok kell explicit módon megadnia hello felhasználói regisztráció során.

> [!NOTE]
> Adatok profil kibontása nagy táblák és nézetek az erőforrás-igényes művelet lehet, és előfordulhat, hogy jelentősen növekedése hello szükséges idő tooregister adatforrás.
>
>

## <a name="user-perspective"></a>Felhasználói perspektíva
Az Azure Data Catalog minden olyan felhasználó, egy regisztrált eszköz biztosíthat leíró metaadatok. Minden felhasználó rendelkezik-e a különböző perspektíva hello adatokkal és a használatával. Például hello-rendszergazdájának a kiszolgáló által biztosított hello részleteit a szolgáltatásiszint-szerződéssel (SLA) vagy a biztonsági mentési Időablakok; egy adatok steward által biztosított hivatkozások toodocumentation hello üzleti folyamatok hello adatok támogatja; és valamelyik elemzőnek előfordulhat, hogy írjon be egy leírást a hello feltételeknek, amelyek leginkább megfelelő tooother elemzők, és amely lehet legértékesebb toothose toodiscover kell és felhasználók hello adatok megismeréséhez.

A szempontok mindegyikének eredendően értékes, és az Azure Data Catalog minden felhasználó hello információkat jelentéssel bíró toothem, amíg minden felhasználó használhatja információk toounderstand hello adatok és mindössze egyetlen célra szorítkoznak biztosíthat.

## <a name="expert"></a>szakértői
Szakértőnek olyan felhasználó, aki rendelkezik azonosított, egy adategységet egy tájékozott "szakértői" szempont. Minden olyan felhasználó, eszköz szakértő szerint is felvehet maguk vagy egy másik felhasználó. Szakértőnek alatt felsorolva nem továbbítja az Azure Data Catalog; további jogosultságokra lehetővé teszi a felhasználók tooeasily keresse meg azokat, amelyek valószínűleg toobe hasznos, ha egy eszköz leíró metaadatok áttekintése perspektívákat.

## <a name="owner"></a>Tulajdonos
Tulajdonos az az Azure Data Catalog egy adategységet további jogosultságokkal rendelkező felhasználóként. Felhasználók saját tulajdonukba vehetik regisztrált adategységeket, és tulajdonosok társtulajdonosok más felhasználók is hozzáadni. További információ: [hogyan toomanage adategységeket](data-catalog-how-to-manage.md)  

> [!NOTE]
> Tulajdonjog és felügyeleti csak a Standard Edition Azure Data Catalog hello érhetők el.
>
>

## <a name="registration"></a>Regisztráció
Regisztráció az adatok eszköz metaadatok beolvasása az adatforrás és toohello Azure Data Catalog szolgáltatás másolás hello act. Regisztrált adategységeket majd feliratozva és felderített.

## <a name="see-also"></a>Lásd még:
* [Mi az az Azure Data Catalog?](data-catalog-what-is-data-catalog.md) – Ez a cikk áttekintése hello Azure Data Catalog szolgáltatás hello értékeket nyújt és hello forgatókönyveket támogatja.
* [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md) – Ez a cikk ismerteti egy végpont oktatóanyag, amely bemutatja, hogyan toouse Azure adatkatalógus az adatforrás-felderítés.  
