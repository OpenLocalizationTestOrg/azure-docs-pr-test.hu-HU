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
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a><span data-ttu-id="7d9c5-103">Meghajtó biztonsági másolatának akkor jelentkezik, az Azure importálási/exportálási feladatok</span><span class="sxs-lookup"><span data-stu-id="7d9c5-103">Backing up drive manifests for Azure Import/Export jobs</span></span>

<span data-ttu-id="7d9c5-104">Meghajtó jegyzékfájlokban automatikusan biztonsági másolat készíthető tooblobs hello beállítása által `BackupDriveManifest` tulajdonság túl`true` a hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) REST API-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="7d9c5-104">Drive manifests can be automatically backed up tooblobs by setting hello `BackupDriveManifest` property too`true` in hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) REST API operations.</span></span> <span data-ttu-id="7d9c5-105">Alapértelmezés szerint hello meghajtó jegyzékfájlokat nem készül biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="7d9c5-105">By default, hello drive manifests are not backed up.</span></span> <span data-ttu-id="7d9c5-106">hello meghajtó jegyzék másolatoknak blokkblobként tároló hello feladattal társított hello tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="7d9c5-106">hello drive manifest backups are stored as block blobs in a container within hello storage account associated with hello job.</span></span> <span data-ttu-id="7d9c5-107">Alapértelmezés szerint hello Tárolónév van `waimportexport`, azonban egy másik nevet adhat meg hello `DiagnosticsPath` tulajdonság hello meghívásakor `Put Job` vagy `Update Job Properties` műveletek.</span><span class="sxs-lookup"><span data-stu-id="7d9c5-107">By default, hello container name is `waimportexport`, but you can specify a different name in hello `DiagnosticsPath` property when calling hello `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="7d9c5-108">hello biztonsági mentési jegyzék blob neve a következő formátum hello: `waies/jobname_driveid_timestamp_manifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="7d9c5-108">hello backup manifest blob are named in hello following format: `waies/jobname_driveid_timestamp_manifest.xml`.</span></span>

 <span data-ttu-id="7d9c5-109">Hello biztonsági mentési meghajtó URI jelentkezik feladat által hívó hello hello le [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="7d9c5-109">You can retrieve hello URI of hello backup drive manifests for a job by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="7d9c5-110">hello blob URI eredmény abban az esetben hello `ManifestUri` minden meghajtó tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="7d9c5-110">hello blob URI is returned in hello `ManifestUri` property for each drive.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d9c5-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d9c5-111">Next steps</span></span>

* [<span data-ttu-id="7d9c5-112">Hello Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="7d9c5-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
