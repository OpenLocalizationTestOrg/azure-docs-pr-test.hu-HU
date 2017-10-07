---
title: "az exportálási feladat az Azure Import/Export aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate exportálása a Microsoft Azure Import/Export szolgáltatás hello feladat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 4a10b42cc86dbf3bcea3a515bc065e2259228ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-export-job-for-hello-azure-importexport-service"></a>Az Azure Import/Export szolgáltatás hello exportálási feladat létrehozása
A lépéseket követve hello hello Microsoft Azure Import/Export szolgáltatás REST API hello segítségével exportálási feladat létrehozása foglal magában:

-   Hello kiválasztásával blobok tooexport.

-   Az beszerzése szállítási helyről.

-   Hello exportálási feladat létrehozása.

-   A szállítási a üres meghajtók tooMicrosoft támogatott szolgáltatónként szolgáltatáson keresztül.

-   Hello exportálási feladatok frissítése hello csomag információval.

-   A fogadó vissza a Microsoft hello meghajtókat.

 Lásd: [hello Windows Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használatával](storage-import-export-service.md) hello Import/Export szolgáltatás és mutatja be a áttekintését hogyan toouse hello [Azure-portálon](https://portal.azure.com/) toocreate és kezelése az importálás és exportálni a feladatokat.

## <a name="selecting-blobs-tooexport"></a>Blobok tooexport kiválasztása
 exportálási feladat toocreate, szüksége lesz a tooprovide megjeleníteni kívánt tooexport importáljon a tárfiókba blobok listáját. Néhány módon tooselect blobok exportált toobe:

-   Egy blob relatív elérési út tooselect egyetlen blob és az összes a pillanatképek használhatja.

-   Egy blob relatív elérési út tooselect használhatja a pillanatképek nélkül egyetlen blob.

-   Egy blob relatív elérési utat és egy pillanatkép idő tooselect egyetlen pillanatkép is használhatja.

-   Használható a blob előtagja tooselect blobok és a pillanatfelvételeket hello megadott előtag.

-   Exportálhatja a blobok és a pillanatképek hello tárfiókban.

 További információ a blobok tooexport, lásd: hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet.

## <a name="obtaining-your-shipping-location"></a>A szállítási hely beszerzése
Exportálási feladat létrehozásához szükséges tooobtain szállítási hely nevét és címét hívó hello [első hely](https://portal.azure.com) vagy [lista helyek](/rest/api/storageimportexport/listlocations) műveletet. `List Locations`helyek és a levelezési címek listáját adja vissza. Jelöljön ki egy helyet a hello-listát adott vissza, és küldje el a merevlemez-meghajtók toothat címét. Is használhatja a hello `Get Location` művelet tooobtain hello közvetlenül szállítási címeket az adott helyen.

Kövesse az alábbi tooobtain hello szállítási hely hello lépéseket:

-   A tárfiók helye hello hello nevének azonosítása. Ez az érték található hello **hely** hello tárolási fiók található **irányítópult** hello klasszikus portál vagy hello service management API-művelet használatával lekérdezett a [beolvasása A tárfiók tulajdonságai](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Lekérni hello helyét, amelyek a rendelkezésre álló tooprocess ezt a tárfiókot hívó hello `Get Location` műveletet.

-   Ha hello `AlternateLocations` hello hely tulajdonsága tartalmazza magát hello helye, akkor célszerű rendben toouse ezen a helyen. Ellenkező esetben hívható hello `Get Location` hello másodlagos helyek egyikén újra a műveletet. hello eredeti helyre ideiglenesen le lehet, hogy a következő karbantartási.

## <a name="creating-hello-export-job"></a>Hello exportálási feladat létrehozása
 toocreate hello exportálási feladat, hívás hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet. Szüksége lesz a következő információ tooprovide hello:

-   Hello feladat nevét.

-   hello tárfiók neve.

-   a hely neve, hello előző lépésben beolvasott szállítási hello.

-   A feladat típusa (Exportálás).

-   hello Válaszcím ahol hello meghajtók küldjön hello exportálási feladat befejeződése után.

-   hello azoknak a blobok (blob előtagok) toobe exportált.

## <a name="shipping-your-drives"></a>A meghajtók szállítási
 A következő hello Azure Import/Export eszköz toodetermine hello meghajtók száma toosend exportált toobe kijelölt hello blobok alapján kell használni, és hello meghajtó mérete. Lásd: hello [Azure Import/Export eszköz hivatkozás](storage-import-export-tool-how-to-v1.md) részleteiről.

 Csomag hello meghajtók egyetlen csomagot, majd küldje el azokat toohello hello beolvasott cím korábbi lépésben. Vegye figyelembe a hello nyomon követése a következő lépéshez hello csomag száma.

> [!NOTE]
>  A meghajtók, amely előállít egy azonosítószám a csomag támogatott szolgáltatónként szolgáltatáson keresztül kell küldje el.

## <a name="updating-hello-export-job-with-your-package-information"></a>A csomagadatok hello exportálási feladatok frissítése
 Miután a nyilvántartási szám, hívja hello [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) művelet tooupdated hello szolgáltatónként neve és nyomon követni a hello feladat számát. Opcionálisan megadhat meghajtók, hello válaszcím és hello szállítási dátumot is hello száma.

## <a name="receiving-hello-package"></a>A fogadó hello csomag
 Az exportálási feladat feldolgozása után a meghajtók visszatér a titkosított adatok tooyou. Hello BitLocker kulcs le hello meghajtókhoz által hívó hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet. Majd fel is oldhatja hello meghajtó hello kulcs használatával. hello meghajtó jegyzékfájl egyes fájlok hello meghajtó, valamint hello eredeti blob cím az egyes fájlok hello listáját tartalmazza.

## <a name="next-steps"></a>Következő lépések

* [Hello Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
