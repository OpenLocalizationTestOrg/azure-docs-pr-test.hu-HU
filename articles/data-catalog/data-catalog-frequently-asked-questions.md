---
title: "aaaAzure Data Catalog gyakori kérdések |} Microsoft Docs"
description: "Azure Data Catalog, beleértve az adatforrás-felderítés, Megjegyzés és felügyeleti képességek kapcsolatos gyakori kérdésekre."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 03f9f4b801640b2e14232c62c8fc168e42e2c393
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Az Azure Data Catalog gyakori kérdések
Ez a cikk ismerteti a válaszok toofrequently feltett kérdések kapcsolódó toohello Azure Data Catalog szolgáltatást.

## <a name="what-is-azure-data-catalog"></a>Mi az az Azure Data Catalog?
A Data Catalog egy teljes körűen felügyelt szolgáltatás, a Microsoft Azure, a regisztráció és a vállalati adatforrások felderítését a rendszer ellátó üzemeltetett. A Data Catalog bármely felhasználó, az elemzők toodata kutatók és fejlesztők számára is regisztrálni, felderítését, megértéséhez, és felhasználását adatforrások.

## <a name="what-customer-challenges-does-it-solve"></a>Milyen felhasználói felkéri minderre megoldani?
Data Catalog címek kihívásai adatforrás felderítés és az "sötét adatok" hello, így a felhasználók felderítésére és megérteni a vállalati adatforrások.

## <a name="what-are-its-target-audiences"></a>Mik a célközönséget?
A Data Catalog célja a műszaki és nem technikai jellegű felhasználók számára, beleértve:

* Adatok fejlesztők és BI és az elemzések szakemberek számára: adatok és analitikák előállító felelős személyek tartalom mások tooconsume.
* Adatok megbízott: személyek, akiknek hello hello ismerete, adatok, az azt jelenti, és hogyan használni kívánt toobe.
* Az adatfelhasználók: személyek, akiknek kell toobe képes tooeasily ismerje meg, megértése és toohello szükséges adatok toodo munkájukhoz, az általuk választott hello eszköz használatával csatlakoztassa.
* Központi informatikai: igénylő adatforrások több száz toomake felderíthető üzleti felhasználók, és az adatok használatának, illetve akik toomaintain felügyeletet igénylő személyeket.

## <a name="what-is-its-availability-by-region"></a>Mi az a régió rendelkezésre állása?
Adatszolgáltatások katalógus jelenleg érhetők el a következő adatközpontokban hello:

* USA nyugati régiója
* USA keleti régiója
* Nyugat-Európa
* Észak-Európa
* Kelet-Ausztrália
* Délkelet-Ázsia

## <a name="what-are-its-limits-on-hello-number-of-data-assets"></a>Hello számára az adategységek saját korlátai
hello a Data Catalog szabad Edition rendszer korlátozott too5, 000 regisztrált adategységeket.

a Data Catalog Standard Edition támogatott hello too100 be 000 regisztrált adategységek keresése.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Mik azok a támogatott forrás- és eszköz adattípust?
A jelenleg támogatott adatforrások listájáért lásd: [Data Catalog DSR](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Hogyan a egy másik adatforrás támogatási kérelem?
toosubmit funkció kérések és más visszajelzést, nyissa meg toohello [Azure Data Catalog fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-do-i-get-started-with-data-catalog"></a>Hogyan kezdjem el a Data Catalog?
legjobb módja tooget hello címen túl van elindítva[Bevezetés a Data Catalog](data-catalog-get-started.md). Ez a cikk a végpontok közötti áttekintést hello képességek hello szolgáltatásban.

## <a name="how-do-i-register-my-data"></a>Hogyan regisztrálhatom adataimat?
tooregister Data Catalog adatai:
1. Hello Azure Data Catalog portálon, a hello **közzététel** területen start hello Azure Data Catalog regisztráló eszközzel. 
2. A Data Catalog hello közzétételi alkalmazás, jelentkezzen be hello azonos tooaccess használt hitelesítő adatok hello Data Catalog-portált.
3. Válassza ki a hello adatforrás és, hogy szeretné-e tooregister hello meghatározott eszközök.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Mely tulajdonságok nem azt kibontásához regisztrált adategységeket?
hello tulajdonságokat eltér a forrás toodata adatforrás, de általában hello Data Catalog közzétételi szolgáltatás kibontja a következő információ hello:

* Eszköz neve
* Eszköz típusa
* Eszköz leírása
* Attribútum-vagy oszlopnév
* Attribútum/oszloptípus-adatokat
* Attribútum/oszlop leírása

> [!IMPORTANT]
> A Data Catalog adategységeket regisztrálása nem helyezhet át az adatok toohello felhő. Eszközök regisztrálása adatforrásból származó másolatok hello eszközök metaadatok tooAzure, de hello adatok marad a hello meglévő adatforrás helyen. hello kivétel toothis szabály, ha úgy dönt tooupload preview rekordok vagy adatok profil hello eszközök regisztrálásakor. Ha egy előzetes, too20 mentése rekordok másolta át minden eszköz, és tárolja a Data Catalog pillanatképként. Ha egy adat-profilt, összesített adatát számított, hello-katalógusban tárolt metaadatok a hello része. Összesített adatát közé tartozik a táblák hello méretét, a null értékek oszlopban, vagy hello minimális, maximális és átlagos értékeit oszlopok száma százalékos hello. 
>
>

> [!NOTE]
> Adatforrások, például az SQL Server Analysis Services, amelyek rendelkeznek egy első osztályú **leírás** tulajdonság, hello adatkatalógus közzétételéhez alkalmazás kivonatok adott tulajdonság értéke. Az SQL Server rendszerű relációs adatbázisokat, amelyek nem rendelkeznek egy első osztályú **leírás** tulajdonság, a Data Catalog közzétételi alkalmazás hello hello érték kiolvassa a hello **ms_description** bővített tulajdonság objektumok és oszlopokat. További információkért lásd: [bővített tulajdonságai párbeszédpanel használata adatbázis-objektumokon](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-tooappear-in-hello-catalog"></a>Mennyi ideig kell vesz igénybe, hogy hello katalógus az újonnan regisztrált eszközig tooappear?
A Data Catalog a eszközök regisztrálása után 5 too10 másodpercig ahhoz, azok megjelennek a Data Catalog-portál hello lehet.

## <a name="how-do-i-annotate-and-enrich-hello-metadata-for-my-registered-data-assets"></a>Hogyan megjegyzésekkel és a regisztrált adategységeket a hello metaadatok kiegészítése?
hello legegyszerűbb módja tooprovide metaadatok regisztrált eszközök tooselect hello eszköz hello Data Catalog portálon, majd adjon meg hello értékeket hello tulajdonságok vagy séma ablaktáblán hello kijelölt objektum.

Néhány metaadatokat, például szakértők és címkék, hello regisztrációs folyamat során is megadhatja. Megadja a Data Catalog közzétételi szolgáltatás hello hello értékeket a jelenleg regisztrált tooall eszközök alkalmazni. tooview hello nemrég regisztrált néhány objektumot további megjegyzéseket, jelölje be hello hello portálon **View Portal** gombra a Data Catalog közzétételi alkalmazás hello hello utolsó képernyőjén.

## <a name="how-do-i-delete-my-registered-data-objects"></a>A regisztrált adategységeket objektumok törlése
Jelölje ki a hello objektum hello portálon, majd kattintson a hello a Data Catalog objektum segítségével törölheti **törlése** gombra. Eltávolításakor hello objektum Data Catalog is eltávolítja a metaadatait, de nincs hatással az hello alapul szolgáló adatforrásban.

## <a name="what-is-an-expert"></a>Mi az a szakértő?
Szakértőnek az egy személy, aki rendelkezik egy tájékozott perspektíva kapcsolatos egy objektumot. Az objektum rendelkezhet több szakértői. Szakértőnek kell toobe hello "owner" objektumhoz, de olyan egyszerűen személy, aki tudja, hogyan hello adatok is, és használható.

## <a name="how-do-i-share-information-with-hello-data-catalog-team-if-i-encounter-problems"></a>Megosztása információk hello Data Catalog csapattal Ha problémát észlel a lemezen?
tooreport problémák, az információkat, és kérdéseket, nyissa meg toohello [Azure Data Catalog fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-hello-catalog-work-with-another-data-source-that-im-interested-in"></a>Nem érdeklik vagyok egy másik adatforrás katalógus használata hello?
Aktívan dolgozunk felvételéről a további adatok források tooData katalógus. Ha azt szeretné, hogy egy adott adatforrást támogatott toosee, javasoljuk, hogy (vagy hangos támogatásához, ha már javasolt) által is toohello [Azure Data Catalog fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-is-azure-data-catalog-related-toohello-data-catalog-in-power-bi-for-office-365"></a>Hogyan van az Azure Data Catalog kapcsolódó toohello Adatkatalógusába a Power BI az Office 365?
Azure Data Catalog egy hello a Power BI Adatkatalógusába fejlődéséhez, tulajdonképpen. Frissítésétől rugó 2017 az Azure Data Catalog használt tooenable hello megosztása és lekérdezéseket az Excel 2016 és a Power Query az Excel felderítése. Az Excel Azure Data Catalog képességei a Power BI Pro-licenccel rendelkező elérhető toousers.

## <a name="what-permissions-do-i-need-tooregister-assets-with-data-catalog"></a>Engedélyek mire van szükségem az tooregister eszközök a Data Catalog?
toorun hello adatkatalógus-regisztráló eszköz, engedélyekre van szükség, amely lehetővé teszi tooread hello metaadatok hello forrásból hello adatforráson. tooalso előképet, amelyek lehetővé teszik a regisztrált hello objektumok hello adatait tooread engedélyekkel kell rendelkeznie.

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>A Data Catalog lesz elérhető, valamint a helyszíni telepítéshez?
A Data Catalog egy felhőalapú szolgáltatás, amely használható a felhőalapú és a helyszíni adatforrások toodeliver hibrid adatforrás felderítés megoldás. Jelenleg nem terv a Data Catalog szolgáltatás, amely a helyszíni hello verziójához.

## <a name="can-i-extract-more-or-richer-metadata-from-hello-data-sources-i-register"></a>Is további vagy szélesebb metaadatokat is kibonthat hello adatforrásokból választanom?
Dolgozunk a Data Catalog képességeinek tooexpand hello aktívan. Ha azt szeretné, hogy toohave további kinyert metaadatokat hello adatforrás regisztráció során, javasoljuk, (vagy az, ha már javasolt szavazás) a hello [Azure Data Catalog fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). A jövőbeli hello azt lehetővé teszi harmadik felek tooadd új adatforrásokat egy bővítési API használatával.

## <a name="how-do-i-restrict-hello-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Hogyan korlátozhatom hello regisztrált adategységek láthatóságának, úgy, hogy csak bizonyos felhasználók derítheti fel azokat?
Jelölje ki a Data Catalog hello hello adategységek, és kattintson a hello **tulajdonba** gombra. Adategységhez a Data Catalog tulajdonosai módosíthatják hello látható beállítások tooeither engedélyezése minden felhasználó toodiscover hello tulajdonában lévő eszközök, vagy korlátozhatja a láthatóság toospecific felhasználókat.

## <a name="how-do-i-update-hello-registration-for-a-data-asset-so-that-changes-in-hello-data-source-are-reflected-in-hello-catalog"></a>Hogyan frissíthetők a hello regisztrálása egy adategységet, hogy a módosítások hello adatforrás megjelennek hello katalógus?
hello katalógusban már regisztrált adategységeket tooupdate hello metaadatai egyszerűen újból a regisztrálást hello eszközöket tartalmazó hello adatforrás. Bármely hello adatforrás, például az oszlopok telepítenek vagy eltávolítja a tábla vagy nézet, változtatások hello katalógusban, de a felhasználók által biztosított jegyzetek megmaradnak.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>A fentiekben itt nem választ. Ahol folytathatja a válaszokat?
Nyissa meg toohello [Azure Data Catalog fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). A kérdések nem itt úton található.
