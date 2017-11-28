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
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a><span data-ttu-id="b1cb7-103">Diagnosztika és a hiba helyreállítási az Azure importálási/exportálási feladatok</span><span class="sxs-lookup"><span data-stu-id="b1cb7-103">Diagnostics and error recovery for Azure Import/Export jobs</span></span>
<span data-ttu-id="b1cb7-104">Minden meghajtó feldolgozott hello Azure Import/Export szolgáltatás hibanaplót hello kapcsolódó tárfiók hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-104">For each drive processed, hello Azure Import/Export service creates an error log in hello associated storage account.</span></span> <span data-ttu-id="b1cb7-105">Is engedélyezheti a részletes naplózás beállítás hello `LogLevel` tulajdonság túl`Verbose` hello meghívásakor [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) műveletek.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-105">You can also enable verbose logging by setting hello `LogLevel` property too`Verbose` when calling hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operations.</span></span>

 <span data-ttu-id="b1cb7-106">Alapértelmezés szerint írja a naplókat nevű tooa tárolót `waimportexport`.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-106">By default, logs are written tooa container named `waimportexport`.</span></span> <span data-ttu-id="b1cb7-107">Egy másik nevet is megadhat hello beállítása használatával `DiagnosticsPath` tulajdonság hello meghívásakor `Put Job` vagy `Update Job Properties` műveletek.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-107">You can specify a different name by setting hello `DiagnosticsPath` property when calling hello `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="b1cb7-108">hello naplók tárolja blokkblobként hello a következő elnevezési konvenció: `waies/jobname_driveid_timestamp_logtype.xml`.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-108">hello logs are stored as block blobs with hello following naming convention: `waies/jobname_driveid_timestamp_logtype.xml`.</span></span>

 <span data-ttu-id="b1cb7-109">Hello URI hello naplók feladat által hívó hello le [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-109">You can retrieve hello URI of hello logs for a job by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="b1cb7-110">hello URI hello részletes napló eredmény abban az esetben hello `VerboseLogUri` minden meghajtó, amíg a hello URI hello hibanapló az eredmény abban az esetben hello tulajdonság `ErrorLogUri` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-110">hello URI for hello verbose log is returned in hello `VerboseLogUri` property for each drive, while hello URI for hello error log is returned in hello `ErrorLogUri` property.</span></span>

<span data-ttu-id="b1cb7-111">A következő problémák adatok tooidentify hello naplózás hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-111">You can use hello logging data tooidentify hello following issues.</span></span>

## <a name="drive-errors"></a><span data-ttu-id="b1cb7-112">Meghajtó hibák</span><span class="sxs-lookup"><span data-stu-id="b1cb7-112">Drive errors</span></span>

<span data-ttu-id="b1cb7-113">a következő elemek hello besorolt meghajtó hibák:</span><span class="sxs-lookup"><span data-stu-id="b1cb7-113">hello following items are classified as drive errors:</span></span>

-   <span data-ttu-id="b1cb7-114">A jegyzékfájl elérése vagy hello olvasása</span><span class="sxs-lookup"><span data-stu-id="b1cb7-114">Errors in accessing or reading hello manifest file</span></span>

-   <span data-ttu-id="b1cb7-115">Helytelen a BitLocker-kulcsok</span><span class="sxs-lookup"><span data-stu-id="b1cb7-115">Incorrect BitLocker keys</span></span>

-   <span data-ttu-id="b1cb7-116">A meghajtó olvasási/írási hibák</span><span class="sxs-lookup"><span data-stu-id="b1cb7-116">Drive read/write errors</span></span>

## <a name="blob-errors"></a><span data-ttu-id="b1cb7-117">A BLOB hibák</span><span class="sxs-lookup"><span data-stu-id="b1cb7-117">Blob errors</span></span>

<span data-ttu-id="b1cb7-118">a következő elemek hello besorolt blob hibák:</span><span class="sxs-lookup"><span data-stu-id="b1cb7-118">hello following items are classified as blob errors:</span></span>

-   <span data-ttu-id="b1cb7-119">Hibás vagy ütköző blob vagy -nevek</span><span class="sxs-lookup"><span data-stu-id="b1cb7-119">Incorrect or conflicting blob or names</span></span>

-   <span data-ttu-id="b1cb7-120">Hiányzó fájlok</span><span class="sxs-lookup"><span data-stu-id="b1cb7-120">Missing files</span></span>

-   <span data-ttu-id="b1cb7-121">A BLOB nem található</span><span class="sxs-lookup"><span data-stu-id="b1cb7-121">Blob not found</span></span>

-   <span data-ttu-id="b1cb7-122">Csonkolt fájlok (hello fájlok hello lemezen hello jegyzékben megadott kisebb)</span><span class="sxs-lookup"><span data-stu-id="b1cb7-122">Truncated files (hello files on hello disk are smaller than specified in hello manifest)</span></span>

-   <span data-ttu-id="b1cb7-123">Sérült fájlt tartalom (importálási feladatok észlelt egy MD5 ellenőrzőösszeg eltérése)</span><span class="sxs-lookup"><span data-stu-id="b1cb7-123">Corrupted file content (for import jobs, detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="b1cb7-124">(Az MD5 ellenőrzőösszeg eltérése az észlelt) sérült blob metaadatait és tulajdonság fájlok</span><span class="sxs-lookup"><span data-stu-id="b1cb7-124">Corrupted blob metadata and property files (detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="b1cb7-125">Hibás sémát hello blob tulajdonságok, illetve a metaadat-fájlok</span><span class="sxs-lookup"><span data-stu-id="b1cb7-125">Incorrect schema for hello blob properties and/or metadata files</span></span>

<span data-ttu-id="b1cb7-126">Előfordulhat, ha egyes részei egy importálási vagy exportálási feladat nem fejeződött be sikeresen, hello általános feladat még befejezésére.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-126">There may be cases where some parts of an import or export job do not complete successfully, while hello overall job still completes.</span></span> <span data-ttu-id="b1cb7-127">Ebben az esetben feltölteni, vagy töltse le a hello hello adatok hiányoznak a hálózaton keresztül, vagy létrehozhat egy új feladat tootransfer hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-127">In this case, you can either upload or download hello missing pieces of hello data over network, or you can create a new job tootransfer hello data.</span></span> <span data-ttu-id="b1cb7-128">Lásd: hello [Azure Import/Export eszköz hivatkozás](storage-import-export-tool-how-to-v1.md) toolearn hogyan toorepair hello hálózaton keresztül adatokat.</span><span class="sxs-lookup"><span data-stu-id="b1cb7-128">See hello [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) toolearn how toorepair hello data over network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b1cb7-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b1cb7-129">Next steps</span></span>

* [<span data-ttu-id="b1cb7-130">Hello Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="b1cb7-130">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
