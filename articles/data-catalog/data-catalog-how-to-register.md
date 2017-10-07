---
title: "az Azure Data Catalog aaaRegister adatforrások |} Microsoft Docs"
description: "Ez a cikk mutatja be, hogyan tooregister adatforrások az Azure Data Catalog, beleértve a hello metaadatmezőket kibontott regisztrálás során."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: efc8a852ddc9fb4bbacc7b0280477bd47814936f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Az Azure Data Catalog adatforrások regisztrálása
## <a name="introduction"></a>Bevezetés
Az Azure Data Catalog egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a vállalati adatforrások felderítését a rendszer funkcionál. Más szóval a Data Catalog segít személyek felderítése, megismeréséhez és használatához adatforrások, és segít a szervezeteknek több érték lekérése a meglévő adatokat. első lépés toomaking adatforrás hello Data Catalog által felfedezhetők van tooregister, hogy az adatforrás.

## <a name="register-data-sources"></a>Adatforrások regisztrálása
Regisztráció az hello folyamat metaadatok beolvasása az hello az adatforrást, és adott adatok toohello Data Catalog szolgáltatás másolása. hello adatok marad, ahol jelenleg van, és hello irányítását hello rendszergazdák és a házirendek hello aktuális rendszer alatt marad.

egy adatforrás tooregister hello a következő:
1. Hello Azure Data Catalog-portált a kiindulási pont hello Data Catalog adatforrás-regisztráló eszköz. 
2. Jelentkezzen be munkahelyi vagy iskolai fiókjával az hello azonos Azure Active Directory hitelesítő adatok használt toosign toohello portálon.
3. Válassza ki a kívánt tooregister hello adatforrást.

További részletes információkért lásd: hello [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md) oktatóanyag.

Hello adatforrás regisztrálása után hello katalógus nyomon követi az helyére, és a metaadatok indexelése. Felhasználók keresésében, böngészésében, és hello adatforrás felderíteni, és kövesse a hely tooconnect tooit hello alkalmazás vagy az általuk választott eszköz segítségével.

## <a name="supported-data-sources"></a>Támogatott adatforrások
A jelenleg támogatott adatforrások listájáért lásd: [Data Catalog DSR](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Szerkezeti metaadatok
Egy adatforrás regisztrálásakor hello regisztrációs eszköz ki hello objektumokhoz hello szerkezete kapcsolatos információkat. Ezekkel az információkkal már említett tooas szerkezeti metaadatokat.

Az összes objektum a szerkezeti metaadatok hello objektum helyét tartalmazza, hogy a felhasználók, akik hello adatok felderítése, hogy információkat tooconnect toohello objektum segítségével hello ügyféleszközökben az általuk választott. Egyéb szerkezeti metaadatokat objektum neve és típusa, és attribútum-vagy nevét és az adatok írja be.

## <a name="descriptive-metadata"></a>Leíró metaadatok
Ezenkívül toohello alapszintű hello adatforrás kinyerésének szerkezeti metaadatokat, hello adatforrás-regisztráló eszköz leíró metaadatok bontja ki. Az SQL Server Analysis Services és az SQL Server Reporting Services a metaadatok hello leírás tulajdonságai jelennek meg, ha ezek a szolgáltatások származik. Az SQL Server, a megadott hello ms használatával\_bővített tulajdonság leírás ki kell olvasni. Oracle-adatbázishoz hello adatforrás regisztrációs eszköz kivonatok hello megjegyzések oszlopát hello összes\_lapon\_megjegyzések megtekintése.

Továbbá toohello leíró metaadatokból hello adatforrásból ki kell olvasni, a felhasználók adhat meg a leíró metaadatok hello adatforrás-regisztráló eszköz használatával. Címkék a felhasználók hozzáadhatnak, és szakértők regisztrált hello objektumok azonosítani tudják. Minden a leíró metaadatok másolni, toohello Data Catalog szolgáltatás hello szerkezeti metaadatok együtt.

## <a name="include-previews"></a>Tartalmazza az előzetes verziójú funkciók
Alapértelmezés szerint csak a metaadatok kinyerésének adatforrások és a másolt toohello Data Catalog szolgáltatást, de az adatforrás gyakran könnyebbé hello adatokhoz mintát megtekintésekor ismertetése.

Hello Data Catalog adatforrás-regisztráló eszköz használatával felvehető hello adatok pillanatkép előnézete minden tábla és nézet, amely regisztrálva van-e. Ha úgy dönt, tooinclude előzetes regisztráció során, hello frissítésregisztráló eszköz too20 rekordok minden tábla és nézet tartalmazza. A pillanatkép majd át lesznek másolva toohello katalógus hello szerkezeti és leíró metaadatok együtt.

> [!NOTE]
> Széles táblákon, amelyekhez nagy számú oszlopot a 20-nál kevesebb bejegyzés szerepel az előzetes tartozhat.
>
>

## <a name="include-data-profiles"></a>Adatok kísérhetők
Többek között a következőket az előzetes verziójú funkciók értékes környezetet biztosíthat a felhasználók számára a Data Catalog tárolt adatforrások keresésére, mint például egy adatok profil teheti, hogy könnyebben felderített toounderstand adatforrások.

Hello Data Catalog adatforrás-regisztráló eszköz használatával minden tábla és nézet, amellyel regisztrálva van-e az adatok profilt is megadhat. Ha úgy dönt, tooinclude adatok profil regisztráció során, hello frissítésregisztráló eszköz szerepel összesített statisztikák hello adatokkal kapcsolatos minden tábla és nézet, beleértve:

* hello hány sort és hello objektum hello adatok méretét.
* legutóbbi frissítés hello hello adatok és hello séma hello dátuma.
* null rekordok és a különböző értékeket az oszlopok száma hello.
* hello minimum, maximum, átlagos és szórás értékekkel.

A statisztikai információk majd kerülnek toohello katalógus hello szerkezeti és leíró metaadatok együtt.

> [!NOTE]
> Szöveg és a dátum oszlop nem tartalmaz átlagos vagy a szórás statisztika a adatok profilban.
>
>

## <a name="update-registrations"></a>Regisztráció frissítése
Egy adatforrás regisztrálása lehetővé teszi a Data Catalog felderíthető hello metaadatok és regisztráció során kibontott választható preview használatakor. Ezért ha hello adatforrás frissítése hello katalógus (például ha egy objektum hello séma megváltozott, eredetileg kizárt táblák tartalmaznia kell, vagy szeretné hello előzetes szereplő tooupdate hello adatokat), az toobe hello adatforrás-regisztráló eszköz futtatható újra.

Egy már regisztrált adatforrás újraregisztrálása egyesítési "upsert" műveletet hajt végre: meglévő objektumok frissülnek, és új objektumok létrehozásakor. Minden metaadatot, felhasználókon hello Data Catalog-portál által biztosított megmaradnak.

## <a name="summary"></a>Összefoglalás
Szerkezeti és leíró metaadatok egy forrás toohello katalógus szolgáltatás másolja át, mert a Data Catalog hello adatforrás regisztrálása hello adatok egyszerűbb toodiscover teszi és megérteni. Miután regisztrálta az hello adatforrás, megjegyzésekkel, kezelése és tudja azt deríteni hello Data Catalog-portál használatával.

## <a name="next-steps"></a>Következő lépések
Adatforrások nyilvántartására vonatkozó további információkért lásd: hello [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md) oktatóanyag.
