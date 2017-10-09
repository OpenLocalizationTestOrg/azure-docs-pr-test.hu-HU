---
title: "aaaCancel és az Azure importálási/exportálási feladatok törlése |} Microsoft Docs"
description: "Ismerje meg, hogyan toocancel és törlési feladatok a Microsoft Azure Import/Export szolgáltatás hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 5d2aba510dafd0ca9a10f5643f721e7059a6a8f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>A megszakítás és az Azure importálási/exportálási feladatok törlése

Kérheti, hogy a feladat szakítható meg, mielőtt hello `Packaging` állapot szerint hívó hello [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) művelet és a beállítás hello `CancelRequested` elem túl`true`. hello feladat megszakad legjobb alapon. Ha meghajtók adatátvitel hello folyamatán, adatok megszakítását kérték után is át toobe továbbra is.

 A megszakított feladatok áthelyezi toohello `Completed` állapot és 90 nap, amikor a rendszer törli azt meg kell őrizni.

 egy feladat toodelete hívás hello [törlése feladat](/rest/api/storageimportexport/jobs#Jobs_Delete) művelet előtt hello feladat van kiadva (*azaz*, hello feladat hello közben `Creating` állapot). Törölheti a feladatok is ha hello `Completed` állapotát. Egy feladat törlését követően annak adatait és állapotát már nem érhetők el hello REST API-n keresztül, vagy hello Azure-portálon.

## <a name="next-steps"></a>Következő lépések

* [Hello Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
