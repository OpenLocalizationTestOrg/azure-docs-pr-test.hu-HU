---
title: "Az Azure importálási/exportálási feladatok diagnosztika és a hiba helyreállítási |} Microsoft Docs"
description: "Megtudhatja, hogyan engedélyezi a részletes naplózás a Microsoft Azure Import/Export szolgáltatás feladatok."
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
ms.openlocfilehash: 0068aae9d6780aa41a070db0eb191d0d5a165d21
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a><span data-ttu-id="6c04b-103">Diagnosztika és a hiba helyreállítási az Azure importálási/exportálási feladatok</span><span class="sxs-lookup"><span data-stu-id="6c04b-103">Diagnostics and error recovery for Azure Import/Export jobs</span></span>
<span data-ttu-id="6c04b-104">Minden meghajtó feldolgozott az Azure Import/Export szolgáltatás hibanaplót a kapcsolódó tárfiók hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6c04b-104">For each drive processed, the Azure Import/Export service creates an error log in the associated storage account.</span></span> <span data-ttu-id="6c04b-105">Részletes naplózás engedélyezése úgy, hogy a `LogLevel` tulajdonságot `Verbose` meghívásakor a [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) műveletek.</span><span class="sxs-lookup"><span data-stu-id="6c04b-105">You can also enable verbose logging by setting the `LogLevel` property to `Verbose` when calling the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operations.</span></span>

 <span data-ttu-id="6c04b-106">Alapértelmezés szerint írja a naplókat nevű tárolót `waimportexport`.</span><span class="sxs-lookup"><span data-stu-id="6c04b-106">By default, logs are written to a container named `waimportexport`.</span></span> <span data-ttu-id="6c04b-107">Megadhatja egy másik nevet úgy, hogy a `DiagnosticsPath` tulajdonság meghívásakor a `Put Job` vagy `Update Job Properties` műveletek.</span><span class="sxs-lookup"><span data-stu-id="6c04b-107">You can specify a different name by setting the `DiagnosticsPath` property when calling the `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="6c04b-108">A naplók a következő elnevezés szabály szerint blokkblobként tárolja: `waies/jobname_driveid_timestamp_logtype.xml`.</span><span class="sxs-lookup"><span data-stu-id="6c04b-108">The logs are stored as block blobs with the following naming convention: `waies/jobname_driveid_timestamp_logtype.xml`.</span></span>

 <span data-ttu-id="6c04b-109">Az URI-feladat a naplók meghívásával kérheti le a [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="6c04b-109">You can retrieve the URI of the logs for a job by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="6c04b-110">A részletes naplózás URI-JÁNAK eredmény abban az esetben a `VerboseLogUri` tulajdonság minden meghajtó, amíg a hibanapló URI-JÁNAK eredmény abban az esetben a `ErrorLogUri` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="6c04b-110">The URI for the verbose log is returned in the `VerboseLogUri` property for each drive, while the URI for the error log is returned in the `ErrorLogUri` property.</span></span>

<span data-ttu-id="6c04b-111">A naplózási adatok segítségével a következő problémák azonosításához.</span><span class="sxs-lookup"><span data-stu-id="6c04b-111">You can use the logging data to identify the following issues.</span></span>

## <a name="drive-errors"></a><span data-ttu-id="6c04b-112">Meghajtó hibák</span><span class="sxs-lookup"><span data-stu-id="6c04b-112">Drive errors</span></span>

<span data-ttu-id="6c04b-113">A következő elemek besorolt meghajtó hibák:</span><span class="sxs-lookup"><span data-stu-id="6c04b-113">The following items are classified as drive errors:</span></span>

-   <span data-ttu-id="6c04b-114">Való hozzáférés, vagy a jegyzékfájl olvasása</span><span class="sxs-lookup"><span data-stu-id="6c04b-114">Errors in accessing or reading the manifest file</span></span>

-   <span data-ttu-id="6c04b-115">Helytelen a BitLocker-kulcsok</span><span class="sxs-lookup"><span data-stu-id="6c04b-115">Incorrect BitLocker keys</span></span>

-   <span data-ttu-id="6c04b-116">A meghajtó olvasási/írási hibák</span><span class="sxs-lookup"><span data-stu-id="6c04b-116">Drive read/write errors</span></span>

## <a name="blob-errors"></a><span data-ttu-id="6c04b-117">A BLOB hibák</span><span class="sxs-lookup"><span data-stu-id="6c04b-117">Blob errors</span></span>

<span data-ttu-id="6c04b-118">A következő elemek besorolt blob hibák:</span><span class="sxs-lookup"><span data-stu-id="6c04b-118">The following items are classified as blob errors:</span></span>

-   <span data-ttu-id="6c04b-119">Hibás vagy ütköző blob vagy -nevek</span><span class="sxs-lookup"><span data-stu-id="6c04b-119">Incorrect or conflicting blob or names</span></span>

-   <span data-ttu-id="6c04b-120">Hiányzó fájlok</span><span class="sxs-lookup"><span data-stu-id="6c04b-120">Missing files</span></span>

-   <span data-ttu-id="6c04b-121">A BLOB nem található</span><span class="sxs-lookup"><span data-stu-id="6c04b-121">Blob not found</span></span>

-   <span data-ttu-id="6c04b-122">Csonkolt fájlok (a fájlok a lemezen kisebb, mint a jegyzékben megadott)</span><span class="sxs-lookup"><span data-stu-id="6c04b-122">Truncated files (the files on the disk are smaller than specified in the manifest)</span></span>

-   <span data-ttu-id="6c04b-123">Sérült fájlt tartalom (importálási feladatok észlelt egy MD5 ellenőrzőösszeg eltérése)</span><span class="sxs-lookup"><span data-stu-id="6c04b-123">Corrupted file content (for import jobs, detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="6c04b-124">(Az MD5 ellenőrzőösszeg eltérése az észlelt) sérült blob metaadatait és tulajdonság fájlok</span><span class="sxs-lookup"><span data-stu-id="6c04b-124">Corrupted blob metadata and property files (detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="6c04b-125">A blob tulajdonságai és/vagy metaadatfájlokban hibás sémát</span><span class="sxs-lookup"><span data-stu-id="6c04b-125">Incorrect schema for the blob properties and/or metadata files</span></span>

<span data-ttu-id="6c04b-126">Előfordulhat, ha egyes részei egy importálási vagy exportálási feladat nem fejeződött be sikeresen, a teljes feladat még elvégzése közben.</span><span class="sxs-lookup"><span data-stu-id="6c04b-126">There may be cases where some parts of an import or export job do not complete successfully, while the overall job still completes.</span></span> <span data-ttu-id="6c04b-127">Ebben az esetben feltölteni, vagy töltse le a hiányzó rész az adatok hálózaton keresztül, vagy létrehozhat egy új feladatot, amely az adatok átvitelét.</span><span class="sxs-lookup"><span data-stu-id="6c04b-127">In this case, you can either upload or download the missing pieces of the data over network, or you can create a new job to transfer the data.</span></span> <span data-ttu-id="6c04b-128">Tekintse meg a [Azure Import/Export eszköz hivatkozás](storage-import-export-tool-how-to-v1.md) hogyan javítsa ki az adatok hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="6c04b-128">See the [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) to learn how to repair the data over network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c04b-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c04b-129">Next steps</span></span>

* [<span data-ttu-id="6c04b-130">Az Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="6c04b-130">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
