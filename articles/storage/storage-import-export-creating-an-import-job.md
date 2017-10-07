---
title: "az importálási feladat az Azure Import/Export aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate a Microsoft Azure Import/Export szolgáltatás hello importálásakor."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: da974c33a3688bb5e2412c8bfcbeca704096c2fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-import-job-for-hello-azure-importexport-service"></a>Az importálási feladat hello Azure Import/Export szolgáltatás létrehozása

Az importálási feladat hello Microsoft Azure Import/Export szolgáltatás REST API hello segítségével létrehozása magában foglalja a hello a következő lépéseket:

-   Az Azure Import/Export eszköz hello meghajtók előkészítése.

-   Nem sikerült beolvasni hello hely toowhich tooship hello meghajtó.

-   Hello importálási feladat létrehozása.

-   TooMicrosoft hello szállítási meghajtók támogatott szolgáltatónként szolgáltatáson keresztül.

-   Hello importálási feladat részletei szállítási hello frissítésekor.

 Lásd: [hello Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használatával](storage-import-export-service.md) hello Import/Export szolgáltatás és mutatja be a áttekintését hogyan toouse hello [Azure-portálon](https://portal.azure.com/) toocreate és kezelése az importálás és exportálni a feladatokat.

## <a name="preparing-drives-with-hello-azure-importexport-tool"></a>Az Azure Import/Export eszköz hello meghajtók előkészítése

hello lépéseket tooprepare meghajtók, hogy az importálás vannak hello azonos e hoz létre jobvia hello portal hello vagy keresztül hello REST API-t.

Az alábbiakban van meghajtó előkészítése rövid áttekintést. Tekintse meg a toohello [Azure Import-ExportTool hivatkozás](storage-import-export-tool-how-to-v1.md) részletes utasításokért. Hello Azure Import/Export eszközt letöltheti [Itt](http://go.microsoft.com/fwlink/?LinkID=301900).

A meghajtó előkészítése foglal magában:

-   Azonosító hello adatok toobe importálva.

-   Hello cél blobot, amely a Windows Azure Storage azonosítása.

-   Hello Azure Import/Export eszköz toocopy használatával, az adatok tooone vagy további merevlemez-meghajtókat.

 hello Azure Import/Export eszköz is létrehoz egy jegyzékfájl hello meghajtókhoz előkészítettként van. A jegyzékfájl tartalmazza:

-   Egy feltöltési és a fájlok tooblobs hello hozzárendelések szánt fájlok hello enumerálása.

-   Az egyes fájlok hello szegmensek ellenőrzőösszegeket.

-   Minden egyes blob a metaadatok és a Tulajdonságok tooassociate hello kapcsolatos információkat.

-   Egy listája hello művelet tootake hello éppen feltöltött blob-e azonos nevet, amint egy meglévő blob hello tárolóban. A lehetséges értékek: a) hello blob felülírása hello fájl, a b) megtartani hello meglévő blob és a skip hello fájl feltöltése, a c) fűzzön hozzá utótag toohello nevet úgy, hogy más fájlok nem ütköznek.

## <a name="obtaining-your-shipping-location"></a>A szállítási hely beszerzése

Mielőtt létrehozna egy importálási feladat, szükséges tooobtain szállítási hely nevét és címét hívó hello [lista helyek](/rest/api/storageimportexport/listlocations) műveletet. `List Locations`helyek és a levelezési címek listáját adja vissza. Jelöljön ki egy helyet a hello-listát adott vissza, és küldje el a merevlemez-meghajtók toothat címét. Is használhatja a hello `Get Location` művelet tooobtain hello közvetlenül szállítási címeket az adott helyen.

 Kövesse az alábbi tooobtain hello szállítási hely hello lépéseket:

-   A tárfiók helye hello hello nevének azonosítása. Ez az érték hello alatt található **hely** hello tárolási fiók található **irányítópult** az Azure portál vagy hello service management API-művelet használatával lekérdezett hello [tárolási beolvasása Tulajdonságok fiók](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Ezt a tárfiókot hello helyre, amely elérhető tooprocess beolvasása hívó hello által `Get Location` műveletet.

-   Ha hello `AlternateLocations` hello hely tulajdonsága tartalmazza magát hello helye, akkor célszerű rendben toouse ezen a helyen. Ellenkező esetben hívható hello `Get Location` hello másodlagos helyek egyikén újra a műveletet. hello eredeti helyre ideiglenesen le lehet, hogy a következő karbantartási.

## <a name="creating-hello-import-job"></a>Hello importálási feladat létrehozása
toocreate hello importálási feladat, hívás hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet. Szüksége lesz a következő információ tooprovide hello:

-   Hello feladat nevét.

-   hello tárfiók neve.

-   a hely neve, hello előző lépésben beszerzett szállítási hello.

-   A feladat típusa (importálás).

-   hello Válaszcím ahol hello meghajtók küldjön hello importálási feladat befejeződése után.

-   a meghajtók hello feladat hello listáját. Minden olyan meghajtó meg kell adni a következő információ hello meghajtó előkészítési lépés során kapott hello:

    -   hello meghajtó azonosítója

    -   hello BitLocker kulcs

    -   hello jegyzékfájl relatív elérési út hello merevlemezről.

    -   hello Base16 kódolású jegyzékfájl MD5 kivonatoló

## <a name="shipping-your-drives"></a>A meghajtók szállítási
Kell küldje el a meghajtók toohello cím hello előző lépésben beszerzett, és meg kell adnia a hello csomag számú követési hello az Import/Export szolgáltatás hello.

> [!NOTE]
>  A meghajtók, amely előállít egy azonosítószám a csomag támogatott szolgáltatónként szolgáltatáson keresztül kell küldje el.

## <a name="updating-hello-import-job-with-your-shipping-information"></a>A szállítási adatokat hello importálási feladat frissítése
Miután a nyilvántartási szám, hívja hello [frissítés Feladattulajdonság](/api/storageimportexport/jobs#Jobs_Update) művelet tooupdate hello szállítási szolgáltatónként nevét, hello azonosítószám hello feladat és hello szolgáltatónként számát a visszatérési szállításra. Opcionálisan megadhat meghajtók és a szállítási dátumot is hello hello száma.

## <a name="next-steps"></a>Következő lépések

* [Hello Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
