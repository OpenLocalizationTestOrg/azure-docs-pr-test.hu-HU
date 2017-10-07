---
title: "az Azure importálási/exportálási feladatok aaaDiagnostics és a hiba helyreállítási |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable részletes naplózás a Microsoft Azure Import/Export szolgáltatás feladatok."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 48164279e7904c78fed802aa3cff66e589c3f12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnosztika és a hiba helyreállítási az Azure importálási/exportálási feladatok
Minden meghajtó feldolgozott hello Azure Import/Export szolgáltatás hibanaplót hello kapcsolódó tárfiók hoz létre. Is engedélyezheti a részletes naplózás beállítás hello `LogLevel` tulajdonság túl`Verbose` hello meghívásakor [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) műveletek.

 Alapértelmezés szerint írja a naplókat nevű tooa tárolót `waimportexport`. Egy másik nevet is megadhat hello beállítása használatával `DiagnosticsPath` tulajdonság hello meghívásakor `Put Job` vagy `Update Job Properties` műveletek. hello naplók tárolja blokkblobként hello a következő elnevezési konvenció: `waies/jobname_driveid_timestamp_logtype.xml`.

 Hello URI hello naplók feladat által hívó hello le [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet. hello URI hello részletes napló eredmény abban az esetben hello `VerboseLogUri` minden meghajtó, amíg a hello URI hello hibanapló az eredmény abban az esetben hello tulajdonság `ErrorLogUri` tulajdonság.

A következő problémák adatok tooidentify hello naplózás hello is használhatja.

## <a name="drive-errors"></a>Meghajtó hibák

a következő elemek hello besorolt meghajtó hibák:

-   A jegyzékfájl elérése vagy hello olvasása

-   Helytelen a BitLocker-kulcsok

-   A meghajtó olvasási/írási hibák

## <a name="blob-errors"></a>A BLOB hibák

a következő elemek hello besorolt blob hibák:

-   Hibás vagy ütköző blob vagy -nevek

-   Hiányzó fájlok

-   A BLOB nem található

-   Csonkolt fájlok (hello fájlok hello lemezen hello jegyzékben megadott kisebb)

-   Sérült fájlt tartalom (importálási feladatok észlelt egy MD5 ellenőrzőösszeg eltérése)

-   (Az MD5 ellenőrzőösszeg eltérése az észlelt) sérült blob metaadatait és tulajdonság fájlok

-   Hibás sémát hello blob tulajdonságok, illetve a metaadat-fájlok

Előfordulhat, ha egyes részei egy importálási vagy exportálási feladat nem fejeződött be sikeresen, hello általános feladat még befejezésére. Ebben az esetben feltölteni, vagy töltse le a hello hello adatok hiányoznak a hálózaton keresztül, vagy létrehozhat egy új feladat tootransfer hello adatokat. Lásd: hello [Azure Import/Export eszköz hivatkozás](storage-import-export-tool-how-to-v1.md) toolearn hogyan toorepair hello hálózaton keresztül adatokat.

## <a name="next-steps"></a>Következő lépések

* [Hello Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
