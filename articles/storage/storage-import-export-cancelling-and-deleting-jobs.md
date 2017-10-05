---
title: "Szakítsa meg, és az Azure importálási/exportálási feladatok törlése |} Microsoft Docs"
description: "Megtudhatja, hogyan megszakításához és a Microsoft Azure Import/Export szolgáltatás feladatok törlése."
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
ms.openlocfilehash: e0a7ff391e5a03ed563912dea54c7cfe73111bcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>A megszakítás és az Azure importálási/exportálási feladatok törlése

Kérheti, hogy a feladatok megszakadnak ahhoz, hogy az a `Packaging` állapot meghívásával a [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) művelet és a beállítás a `CancelRequested` elem `true`. A feladat meg lesz szakítva legjobb alapon. Ha meghajtók jelenleg másolja át az adatokat, adatok továbbra is helyezhető át, akkor is megszakítását kérték.

 A megszakított feladatok helyezi át a `Completed` állapot és 90 nap, amikor a rendszer törli azt meg kell őrizni.

 Törli a feladatot, hívja meg a [törlése feladat](/rest/api/storageimportexport/jobs#Jobs_Delete) művelet előtt a feladat rendelkezik-e kiadva (*azaz*, amíg a feladat a `Creating` állapot). Törölheti is a feladat a esetén a `Completed` állapotát. Egy feladat törlését követően annak adatait és állapotát már nincs a REST API-t vagy az Azure-portálon keresztül érhető el.

## <a name="next-steps"></a>Következő lépések

* [Az Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
