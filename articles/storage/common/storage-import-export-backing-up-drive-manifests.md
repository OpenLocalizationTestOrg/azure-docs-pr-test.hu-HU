---
title: "be Azure Import/Export meghajtó jegyzékfájlokban aaaBacking |} Microsoft Docs"
description: "Ismerje meg, hogyan toohave a meghajtó jelentkezik hello Microsoft Azure Import/Export szolgáltatás automatikusan biztonsági másolat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 594abd80-b834-4077-a474-d8a0f4b7928a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: f48b97a2cce62714aace2b30a393305202c7ecd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Meghajtó biztonsági másolatának akkor jelentkezik, az Azure importálási/exportálási feladatok

Meghajtó jegyzékfájlokban automatikusan biztonsági másolat készíthető tooblobs hello beállítása által `BackupDriveManifest` tulajdonság túl`true` a hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) REST API-műveleteket. Alapértelmezés szerint hello meghajtó jegyzékfájlokat nem készül biztonsági mentés. hello meghajtó jegyzék másolatoknak blokkblobként tároló hello feladattal társított hello tárfiókon belül. Alapértelmezés szerint hello Tárolónév van `waimportexport`, azonban egy másik nevet adhat meg hello `DiagnosticsPath` tulajdonság hello meghívásakor `Put Job` vagy `Update Job Properties` műveletek. hello biztonsági mentési jegyzék blob neve a következő formátum hello: `waies/jobname_driveid_timestamp_manifest.xml`.

 Hello biztonsági mentési meghajtó URI jelentkezik feladat által hívó hello hello le [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet. hello blob URI eredmény abban az esetben hello `ManifestUri` minden meghajtó tulajdonság.

## <a name="next-steps"></a>Következő lépések

* [Hello Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
