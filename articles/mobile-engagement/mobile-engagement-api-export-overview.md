---
title: "aaaMobile Engagement exportálása API – áttekintés"
description: "Ismerje meg, a nyers adatok exportálása hello alapjai állítja elő a felhasználói eszközök tooleverage azt a saját eszközök"
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: 
ms.assetid: 9380d47b-d7fa-4d4c-888f-97e6482196bb
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kapiteir
ms.openlocfilehash: f55be29a29878e74f6a33419f08a5574a07a7478
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mobile-engagement-export-api-overview"></a>Mobile Engagement exportálási API áttekintése
## <a name="introduction"></a>Bevezetés
Ebből a dokumentumból megtudhatja, a nyers adatok exportálása hello alapjai állítja elő a felhasználói eszközök tooleverage azt a saját eszközök.

## <a name="pre-requisites"></a>Előfeltételek
Hello nyers adatainak exportálása a Mobile Engagement van szükség:

* API authentication telepítő toobe képes toouse hello API-k (lásd: [hitelesítési manuális telepítési módra](mobile-engagement-api-authentication-manual.md)),
* Használja a REST API-k hello vagy hello [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
* Egy Azure Storage-fiókot.

> [!NOTE]
> Ajánlott is kiváló hello [Microsoft Azure Tártallózó](http://storageexplorer.com/), legalább, mert a hello fejlesztési fázisban biztosít egy egyszerű toouse felhasználói felület közötti kommunikáció során az Azure Storage.
> 
> 

## <a name="what-can-be-exported"></a>Mi lehet exportálni?
A Mobile Engagement lehetővé teszi, hogy a felhasználók toocollect számos különböző típusú adatok, és ezért rendelkezik számos különböző típusú exportálás alkalmas toothese különböző adattípusokkal.
Exportálás 2 alapvető típusa van:

* Pillanatkép: használt általában tooexport adatokat egy állapotának felel meg, és, amelynek a Mobile Engagement nincs előzményeit. Ez magában foglalja-jogkivonatok és a leküldéses kampány visszajelzések címkék (app-info), például. Ennek következtében az ilyen típusú exportálás nincsenek kapcsolódó tooa dátum.
* korábbi: az Exportálás típusú az adatokat, például az események vagy a tevékenységek időbeli például összesít szolgál.

hello az alábbi táblázat körűen összes hello lehetséges export:

| Exportálás típusa | Adattípus | Leírás |
| --- | --- | --- |
| Pillanatkép |Leküldéses értesítések |Állít elő, leküldéses kampányokra visszajelzések deviceid/userid / alapon exportálása |
| Pillanatkép |Címke |Állít elő, hello társított címkék (app-info) tooeach eszközök exportálása |
| Pillanatkép |Eszköz |Állít elő, eszközök, például hello technicals (modell, területi beállítás, az időzónát,...), hello címkék, először látott hello adatait a legtöbb exportálása... |
| Pillanatkép |Token |Állít elő az összes hello érvényes jogkivonatot exportálása |
| Korábbi |Tevékenység |Létrehoz egy adott időszakban az egyes eszközök összes hello tevékenység exportálása |
| Korábbi |Esemény |Létrehoz egy adott időszakban az egyes eszközök összes hello tevékenység exportálása |
| Korábbi |Feladat |Létrehoz egy adott időszakban az egyes eszközökre vonatkozó összes hello feladat exportálása |
| Korábbi |Hiba |Létrehoz egy adott időszakban az egyes eszközökre vonatkozó hibákat hello exportálása |

## <a name="how-does-it-work"></a>Hogyan működik?
Export hosszúak futó feladatok is eredményezhet nagy fájlok. Éppen ezért azok nem lehet meghívott tooreturn azonnal fájl toodownload.
A Mobile Engagement tooexport adatokat ahhoz, hogy toocreate egy **exportálása feladat** megadott általában API-n keresztül:

* hello típusú (pillanatképes vagy korábbi), exportálás
* hello adattípus,
* Hello **Azure-tárolót** (beleértve az írási hozzáférés egy érvényes SAS) ahová hello exportálási hello eredményét kerülnek.
* pl. példa tároló URL-paramétert lenne https://[StorageAccountName].blob.core.windows.net/[ContainerName]? [SASWritePermissionsToken]  

Itt látható a valós életben példa. https://testazmeexport.BLOB.Core.Windows.NET/test1234azme?SV=2015-12-11&SS=b&SRT=SCO&SP=rwdlac&se=2016-12-17T04:59:26Z & st = 2016-12-16T20:59:26Z & spr = https & sig = KRF3aVWjp2NEJDzjlmoplmu0M9HHlLdkBWRPAFmw90Q % 3D

Vegye figyelembe, hogy a feladat toobe lépések néhány percig is eltarthat, és majd futtathatja az alkalmazások a felhasználók vagy a tevékenység számos apró alkalmazások tooseveral órában néhány másodperc.

Hello feladat jön létre, ha lehetséges toocheck annak állapota toosee, ha továbbra is fut, vagy ha befejeződött.

Miután hello feladat van sikeresen befejeződött, a megadott tároló hello hello eredményül kapott adatfájl érhető el.

