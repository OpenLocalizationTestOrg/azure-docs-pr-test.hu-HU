---
title: "Biztonsági mentése Azure Import/Export meghajtó jegyzékfájlokban |} Microsoft Docs"
description: "Útmutató a Microsoft Azure Import/Export szolgáltatás automatikusan biztonsági másolat a meghajtó jegyzékfájlokban."
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
ms.openlocfilehash: 33eb8e1eea8f8aa7b79ef3e54f2b1ed88dc794ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a><span data-ttu-id="472ba-103">Meghajtó biztonsági másolatának akkor jelentkezik, az Azure importálási/exportálási feladatok</span><span class="sxs-lookup"><span data-stu-id="472ba-103">Backing up drive manifests for Azure Import/Export jobs</span></span>

<span data-ttu-id="472ba-104">Meghajtó jegyzékfájlokban automatikusan biztonsági másolat készíthető a blobok úgy, hogy a `BackupDriveManifest` tulajdonságot `true` a a [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) REST API-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="472ba-104">Drive manifests can be automatically backed up to blobs by setting the `BackupDriveManifest` property to `true` in the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) REST API operations.</span></span> <span data-ttu-id="472ba-105">Alapértelmezés szerint a meghajtó jegyzékfájlokat nem készül biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="472ba-105">By default, the drive manifests are not backed up.</span></span> <span data-ttu-id="472ba-106">A meghajtó jegyzék másolatoknak blokkblobként egy tárolót a feladathoz társított tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="472ba-106">The drive manifest backups are stored as block blobs in a container within the storage account associated with the job.</span></span> <span data-ttu-id="472ba-107">Alapértelmezés szerint a tároló neve van `waimportexport`, de megadhat egy másik nevet a a `DiagnosticsPath` tulajdonság meghívásakor a `Put Job` vagy `Update Job Properties` műveletek.</span><span class="sxs-lookup"><span data-stu-id="472ba-107">By default, the container name is `waimportexport`, but you can specify a different name in the `DiagnosticsPath` property when calling the `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="472ba-108">A biztonsági mentési jegyzék blob neve a következő formátumban: `waies/jobname_driveid_timestamp_manifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="472ba-108">The backup manifest blob are named in the following format: `waies/jobname_driveid_timestamp_manifest.xml`.</span></span>

 <span data-ttu-id="472ba-109">Az URI-azonosítója egy feladat a biztonsági mentési meghajtó jegyzékfájlokban meghívásával kérheti le a [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="472ba-109">You can retrieve the URI of the backup drive manifests for a job by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="472ba-110">A blob URI eredmény abban az esetben a `ManifestUri` minden meghajtó tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="472ba-110">The blob URI is returned in the `ManifestUri` property for each drive.</span></span>

## <a name="next-steps"></a><span data-ttu-id="472ba-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="472ba-111">Next steps</span></span>

* [<span data-ttu-id="472ba-112">Az Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="472ba-112">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
