---
title: "Az Azure Import/Export naplófájlformátum |} Microsoft Docs"
description: "További információk a lépéseket az Import/Export szolgáltatás feladat végrehajtásakor létrehozott naplófájlok formátuma."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 16234ccaf13ce1d85cfd207ed4734e683070faa6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-importexport-service-log-file-format"></a><span data-ttu-id="65bb4-103">Az Azure Import/Export szolgáltatás naplófájlformátum</span><span class="sxs-lookup"><span data-stu-id="65bb4-103">Azure Import/Export service log file format</span></span>
<span data-ttu-id="65bb4-104">Amikor a Microsoft Azure Import/Export szolgáltatás hajt végre, az importálási feladat vagy exportálási feladat részeként egy műveletet a meghajtón, írja a naplókat, hogy a feladathoz kapcsolódó tárfiókban blokkblobokat.</span><span class="sxs-lookup"><span data-stu-id="65bb4-104">When the Microsoft Azure Import/Export service performs an action on a drive as part of an import job or an export job, logs are written to block blobs in the storage account associated with that job.</span></span>  
  
<span data-ttu-id="65bb4-105">Az Import/Export szolgáltatás írható két naplók van:</span><span class="sxs-lookup"><span data-stu-id="65bb4-105">There are two logs that may be written by the Import/Export service:</span></span>  
  
-   <span data-ttu-id="65bb4-106">A hibanapló hiba esetén mindig létrejön.</span><span class="sxs-lookup"><span data-stu-id="65bb4-106">The error log is always generated in the event of an error.</span></span>  
  
-   <span data-ttu-id="65bb4-107">A részletes naplózás alapértelmezés szerint nincs engedélyezve, de úgy, hogy engedélyezhető a `EnableVerboseLog` tulajdonságának egy [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) műveletet.</span><span class="sxs-lookup"><span data-stu-id="65bb4-107">The verbose log is not enabled by default, but may be enabled by setting the `EnableVerboseLog` property on a [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation.</span></span>  
  
## <a name="log-file-location"></a><span data-ttu-id="65bb4-108">Naplófájl helye</span><span class="sxs-lookup"><span data-stu-id="65bb4-108">Log file location</span></span>  
<span data-ttu-id="65bb4-109">A naplók írni a tároló vagy a megadott virtuális könyvtár blokkblobokat a `ImportExportStatesPath` beállítás, amely állíthat be egy `Put Job` műveletet.</span><span class="sxs-lookup"><span data-stu-id="65bb4-109">The logs are written to block blobs in the container or virtual directory specified by the `ImportExportStatesPath` setting, which you can set on a `Put Job` operation.</span></span> <span data-ttu-id="65bb4-110">A helyet, amelyhez a naplók írt, attól függ, hogyan hitelesítés van megadva, a feladat a megadott együtt `ImportExportStatesPath`.</span><span class="sxs-lookup"><span data-stu-id="65bb4-110">The location to which the logs are written depends on how authentication is specified for the job, together with the value specified for `ImportExportStatesPath`.</span></span> <span data-ttu-id="65bb4-111">A feladat hitelesítési a tárfiók kulcsa, vagy a tároló SAS (közös hozzáférésű jogosultságkód) keresztül adható meg.</span><span class="sxs-lookup"><span data-stu-id="65bb4-111">Authentication for the job may be specified via a storage account key, or a container SAS (shared access signature).</span></span>  
  
<span data-ttu-id="65bb4-112">A tároló vagy a virtuális könyvtár neve lehet, hogy vagy az alapértelmezett nevet: kell `waimportexport`, vagy egy másik tárolóhoz vagy a megadott virtuális könyvtár neve.</span><span class="sxs-lookup"><span data-stu-id="65bb4-112">The name of the container or virtual directory may either be the default name of `waimportexport`, or another container or virtual directory name that you specify.</span></span>  
  
<span data-ttu-id="65bb4-113">Az alábbi táblázatban a lehetséges beállítások:</span><span class="sxs-lookup"><span data-stu-id="65bb4-113">The table below shows the possible options:</span></span>  
  
|<span data-ttu-id="65bb4-114">Hitelesítési módszer</span><span class="sxs-lookup"><span data-stu-id="65bb4-114">Authentication Method</span></span>|<span data-ttu-id="65bb4-115">Az érték `ImportExportStatesPath`elem</span><span class="sxs-lookup"><span data-stu-id="65bb4-115">Value of `ImportExportStatesPath`Element</span></span>|<span data-ttu-id="65bb4-116">Blobok napló helye</span><span class="sxs-lookup"><span data-stu-id="65bb4-116">Location of Log Blobs</span></span>|  
|---------------------------|----------------------------------------------|---------------------------|  
|<span data-ttu-id="65bb4-117">Tárfiók kulcsa</span><span class="sxs-lookup"><span data-stu-id="65bb4-117">Storage account key</span></span>|<span data-ttu-id="65bb4-118">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="65bb4-118">Default value</span></span>|<span data-ttu-id="65bb4-119">Nevű tárolót `waimportexport`, amely egy, az alapértelmezett tároló.</span><span class="sxs-lookup"><span data-stu-id="65bb4-119">A container named `waimportexport`, which is the default container.</span></span> <span data-ttu-id="65bb4-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="65bb4-120">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|<span data-ttu-id="65bb4-121">Tárfiók kulcsa</span><span class="sxs-lookup"><span data-stu-id="65bb4-121">Storage account key</span></span>|<span data-ttu-id="65bb4-122">A felhasználó által megadott érték</span><span class="sxs-lookup"><span data-stu-id="65bb4-122">User-specified value</span></span>|<span data-ttu-id="65bb4-123">A felhasználó által nevű tárolót.</span><span class="sxs-lookup"><span data-stu-id="65bb4-123">A container named by the user.</span></span> <span data-ttu-id="65bb4-124">Példa:</span><span class="sxs-lookup"><span data-stu-id="65bb4-124">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|<span data-ttu-id="65bb4-125">A tároló SAS</span><span class="sxs-lookup"><span data-stu-id="65bb4-125">Container SAS</span></span>|<span data-ttu-id="65bb4-126">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="65bb4-126">Default value</span></span>|<span data-ttu-id="65bb4-127">Nevű virtuális könyvtár `waimportexport`, amely az alapértelmezett nevet, a biztonsági Társítások megadott tároló alatt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-127">A virtual directory named `waimportexport`, which is the default name, beneath the container specified in the SAS.</span></span><br /><br /> <span data-ttu-id="65bb4-128">Például, ha a megadott SAS a folyamat `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, akkor válassza a napló helye`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span><span class="sxs-lookup"><span data-stu-id="65bb4-128">For example, if the SAS specified for the job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, then the log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span></span>|  
|<span data-ttu-id="65bb4-129">A tároló SAS</span><span class="sxs-lookup"><span data-stu-id="65bb4-129">Container SAS</span></span>|<span data-ttu-id="65bb4-130">A felhasználó által megadott érték</span><span class="sxs-lookup"><span data-stu-id="65bb4-130">User-specified value</span></span>|<span data-ttu-id="65bb4-131">A felhasználó, a biztonsági Társítások megadott tároló alatt nevű virtuális könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="65bb4-131">A virtual directory named by the user, beneath the container specified in the SAS.</span></span><br /><br /> <span data-ttu-id="65bb4-132">Például, ha a megadott SAS a folyamat `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, és a megadott virtuális könyvtár neve `mylogblobs`, akkor válassza a napló helyét `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span><span class="sxs-lookup"><span data-stu-id="65bb4-132">For example, if the SAS specified for the job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, and the specified virtual directory is named `mylogblobs`, then the log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span></span>|  
  
<span data-ttu-id="65bb4-133">Az URL-címet a hibaüzeneteket és részletes naplókat meghívásával kérheti le a [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet.</span><span class="sxs-lookup"><span data-stu-id="65bb4-133">You can retrieve the URL for the error and verbose logs by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="65bb4-134">A naplók a meghajtó feldolgozás befejezése után érhetők el.</span><span class="sxs-lookup"><span data-stu-id="65bb4-134">The logs are available after processing of the drive is complete.</span></span>  
  
## <a name="log-file-format"></a><span data-ttu-id="65bb4-135">Naplófájlformátum</span><span class="sxs-lookup"><span data-stu-id="65bb4-135">Log file format</span></span>  
<span data-ttu-id="65bb4-136">Mindkét napló formátuma megegyezik: blobok a merevlemez-meghajtóról, és a felhasználói fiókhoz közötti másolás közben előforduló eseményeket XML leírását tartalmazó blob.</span><span class="sxs-lookup"><span data-stu-id="65bb4-136">The format for both logs is the same: a blob containing XML descriptions of the events that occurred while copying blobs between the hard drive and the customer's account.</span></span>  
  
<span data-ttu-id="65bb4-137">A részletes naplózás minden egyes blob (hogy az importálás) vagy a fájlt (exportálási feladat), a másolási művelet állapotára vonatkozó részletes adatokat tartalmaz, mivel a hibanapló csak blobok vagy hibákat észlelt az importálási vagy exportálási feladat során fájlok adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="65bb4-137">The verbose log contains complete information about the status of the copy operation for every blob (for an import job) or file (for an export job), whereas the error log contains only the information for blobs or files that encountered errors during the import or export job.</span></span>  
  
<span data-ttu-id="65bb4-138">A részletes napló formátuma alább látható.</span><span class="sxs-lookup"><span data-stu-id="65bb4-138">The verbose log format is shown below.</span></span> <span data-ttu-id="65bb4-139">A hibanapló hasonló struktúrával rendelkezik, de sikeres műveletek kiszűri.</span><span class="sxs-lookup"><span data-stu-id="65bb4-139">The error log has the same structure, but filters out successful operations.</span></span>  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

<span data-ttu-id="65bb4-140">A következő táblázat ismerteti a naplófájl az elemek.</span><span class="sxs-lookup"><span data-stu-id="65bb4-140">The following table describes the elements of the log file.</span></span>  
  
|<span data-ttu-id="65bb4-141">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-141">XML Element</span></span>|<span data-ttu-id="65bb4-142">Típus</span><span class="sxs-lookup"><span data-stu-id="65bb4-142">Type</span></span>|<span data-ttu-id="65bb4-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="65bb4-143">Description</span></span>|  
|-----------------|----------|-----------------|  
|`DriveLog`|<span data-ttu-id="65bb4-144">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-144">XML Element</span></span>|<span data-ttu-id="65bb4-145">A meghajtó napló jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-145">Represents a drive log.</span></span>|  
|`Version`|<span data-ttu-id="65bb4-146">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-146">Attribute, String</span></span>|<span data-ttu-id="65bb4-147">A naplófájl formátumának verziója.</span><span class="sxs-lookup"><span data-stu-id="65bb4-147">The version of the log format.</span></span>|  
|`DriveId`|<span data-ttu-id="65bb4-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-148">String</span></span>|<span data-ttu-id="65bb4-149">A meghajtó hardver sorozatszáma.</span><span class="sxs-lookup"><span data-stu-id="65bb4-149">The drive's hardware serial number.</span></span>|  
|`Status`|<span data-ttu-id="65bb4-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-150">String</span></span>|<span data-ttu-id="65bb4-151">A meghajtó feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="65bb4-151">Status of the drive processing.</span></span> <span data-ttu-id="65bb4-152">Tekintse meg a `Drive Status Codes` tábla alább olvashat.</span><span class="sxs-lookup"><span data-stu-id="65bb4-152">See the `Drive Status Codes` table below for more information.</span></span>|  
|`Blob`|<span data-ttu-id="65bb4-153">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-153">Nested XML element</span></span>|<span data-ttu-id="65bb4-154">Egy blob jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-154">Represents a blob.</span></span>|  
|`Blob/BlobPath`|<span data-ttu-id="65bb4-155">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-155">String</span></span>|<span data-ttu-id="65bb4-156">A BLOB URI.</span><span class="sxs-lookup"><span data-stu-id="65bb4-156">The URI of the blob.</span></span>|  
|`Blob/FilePath`|<span data-ttu-id="65bb4-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-157">String</span></span>|<span data-ttu-id="65bb4-158">A meghajtón a fájl relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="65bb4-158">The relative path to the file on the drive.</span></span>|  
|`Blob/Snapshot`|<span data-ttu-id="65bb4-159">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="65bb4-159">DateTime</span></span>|<span data-ttu-id="65bb4-160">A blob exportálási feladat csak a pillanatfelvétel-verzió.</span><span class="sxs-lookup"><span data-stu-id="65bb4-160">The snapshot version of the blob, for an export job only.</span></span>|  
|`Blob/Length`|<span data-ttu-id="65bb4-161">Egész szám</span><span class="sxs-lookup"><span data-stu-id="65bb4-161">Integer</span></span>|<span data-ttu-id="65bb4-162">A teljes hossza a blob bájtban.</span><span class="sxs-lookup"><span data-stu-id="65bb4-162">The total length of the blob in bytes.</span></span>|  
|`Blob/LastModified`|<span data-ttu-id="65bb4-163">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="65bb4-163">DateTime</span></span>|<span data-ttu-id="65bb4-164">A dátum/idő, amelyet a blob, csak az exportálási feladat.</span><span class="sxs-lookup"><span data-stu-id="65bb4-164">The date/time that the blob was last modified, for an export job only.</span></span>|  
|`Blob/ImportDisposition`|<span data-ttu-id="65bb4-165">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-165">String</span></span>|<span data-ttu-id="65bb4-166">A blob, hogy az importálás csak importálási rendezése.</span><span class="sxs-lookup"><span data-stu-id="65bb4-166">The import disposition of the blob, for an import job only.</span></span>|  
|`Blob/ImportDisposition/@Status`|<span data-ttu-id="65bb4-167">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-167">Attribute, String</span></span>|<span data-ttu-id="65bb4-168">Az importálás törlése állapota.</span><span class="sxs-lookup"><span data-stu-id="65bb4-168">The status of the import disposition.</span></span>|  
|`PageRangeList`|<span data-ttu-id="65bb4-169">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-169">Nested XML element</span></span>|<span data-ttu-id="65bb4-170">Az oldalakra vonatkozó blob tartományokat listáját jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-170">Represents a list of page ranges for a page blob.</span></span>|  
|`PageRange`|<span data-ttu-id="65bb4-171">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-171">XML element</span></span>|<span data-ttu-id="65bb4-172">Egy laptartomány jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-172">Represents a page range.</span></span>|  
|`PageRange/@Offset`|<span data-ttu-id="65bb4-173">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="65bb4-173">Attribute, Integer</span></span>|<span data-ttu-id="65bb4-174">A BLOB a laptartomány kezdő eltolása.</span><span class="sxs-lookup"><span data-stu-id="65bb4-174">Starting offset of the page range in the blob.</span></span>|  
|`PageRange/@Length`|<span data-ttu-id="65bb4-175">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="65bb4-175">Attribute, Integer</span></span>|<span data-ttu-id="65bb4-176">A lap tartomány hossza bájtban.</span><span class="sxs-lookup"><span data-stu-id="65bb4-176">Length in bytes of the page range.</span></span>|  
|`PageRange/@Hash`|<span data-ttu-id="65bb4-177">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-177">Attribute, String</span></span>|<span data-ttu-id="65bb4-178">Base16 kódolású MD5 kivonatoló a lap tartomány.</span><span class="sxs-lookup"><span data-stu-id="65bb4-178">Base16-encoded MD5 hash of the page range.</span></span>|  
|`PageRange/@Status`|<span data-ttu-id="65bb4-179">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-179">Attribute, String</span></span>|<span data-ttu-id="65bb4-180">A laptartomány feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="65bb4-180">Status of processing the page range.</span></span>|  
|`BlockList`|<span data-ttu-id="65bb4-181">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-181">Nested XML element</span></span>|<span data-ttu-id="65bb4-182">Egy blokkblobhoz tartoznak blokkok listája jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-182">Represents a list of blocks for a block blob.</span></span>|  
|`Block`|<span data-ttu-id="65bb4-183">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-183">XML element</span></span>|<span data-ttu-id="65bb4-184">A blokk jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-184">Represents a block.</span></span>|  
|`Block/@Offset`|<span data-ttu-id="65bb4-185">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="65bb4-185">Attribute, Integer</span></span>|<span data-ttu-id="65bb4-186">A blob eltolása a blokk kezdve.</span><span class="sxs-lookup"><span data-stu-id="65bb4-186">Starting offset of the block in the blob.</span></span>|  
|`Block/@Length`|<span data-ttu-id="65bb4-187">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="65bb4-187">Attribute, Integer</span></span>|<span data-ttu-id="65bb4-188">A blokk hossza bájtban.</span><span class="sxs-lookup"><span data-stu-id="65bb4-188">Length in bytes of the block.</span></span>|  
|`Block/@Id`|<span data-ttu-id="65bb4-189">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-189">Attribute, String</span></span>|<span data-ttu-id="65bb4-190">A blokk-azonosító.</span><span class="sxs-lookup"><span data-stu-id="65bb4-190">The block ID.</span></span>|  
|`Block/@Hash`|<span data-ttu-id="65bb4-191">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-191">Attribute, String</span></span>|<span data-ttu-id="65bb4-192">Base16 kódolású MD5 kivonatoló a blokk.</span><span class="sxs-lookup"><span data-stu-id="65bb4-192">Base16-encoded MD5 hash of the block.</span></span>|  
|`Block/@Status`|<span data-ttu-id="65bb4-193">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-193">Attribute, String</span></span>|<span data-ttu-id="65bb4-194">A blokk feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="65bb4-194">Status of processing the block.</span></span>|  
|`Metadata`|<span data-ttu-id="65bb4-195">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-195">Nested XML element</span></span>|<span data-ttu-id="65bb4-196">A blob metaadatai jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-196">Represents the blob's metadata.</span></span>|  
|`Metadata/@Status`|<span data-ttu-id="65bb4-197">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-197">Attribute, String</span></span>|<span data-ttu-id="65bb4-198">A blob metaadatai feldolgozásának állapotát.</span><span class="sxs-lookup"><span data-stu-id="65bb4-198">Status of processing of the blob metadata.</span></span>|  
|`Metadata/GlobalPath`|<span data-ttu-id="65bb4-199">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-199">String</span></span>|<span data-ttu-id="65bb4-200">A globális metaadatokat relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="65bb4-200">Relative path to the global metadata file.</span></span>|  
|`Metadata/GlobalPath/@Hash`|<span data-ttu-id="65bb4-201">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-201">Attribute, String</span></span>|<span data-ttu-id="65bb4-202">Base16 kódolású MD5 kivonatoló a globális metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="65bb4-202">Base16-encoded MD5 hash of the global metadata file.</span></span>|  
|`Metadata/Path`|<span data-ttu-id="65bb4-203">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-203">String</span></span>|<span data-ttu-id="65bb4-204">A metaadatok relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="65bb4-204">Relative path to the metadata file.</span></span>|  
|`Metadata/Path/@Hash`|<span data-ttu-id="65bb4-205">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-205">Attribute, String</span></span>|<span data-ttu-id="65bb4-206">Base16 kódolású MD5 kivonatoló a metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="65bb4-206">Base16-encoded MD5 hash of the metadata file.</span></span>|  
|`Properties`|<span data-ttu-id="65bb4-207">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="65bb4-207">Nested XML element</span></span>|<span data-ttu-id="65bb4-208">A blob tulajdonságai jelöli.</span><span class="sxs-lookup"><span data-stu-id="65bb4-208">Represents the blob properties.</span></span>|  
|`Properties/@Status`|<span data-ttu-id="65bb4-209">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-209">Attribute, String</span></span>|<span data-ttu-id="65bb4-210">Feldolgozása a blob tulajdonságai, például a fájl nem található, állapotának befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-210">Status of processing the blob properties, e.g. file not found, completed.</span></span>|  
|`Properties/GlobalPath`|<span data-ttu-id="65bb4-211">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-211">String</span></span>|<span data-ttu-id="65bb4-212">A globális tulajdonságok relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="65bb4-212">Relative path to the global properties file.</span></span>|  
|`Properties/GlobalPath/@Hash`|<span data-ttu-id="65bb4-213">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-213">Attribute, String</span></span>|<span data-ttu-id="65bb4-214">Base16 kódolású MD5 kivonatoló a globális tulajdonságok fájl.</span><span class="sxs-lookup"><span data-stu-id="65bb4-214">Base16-encoded MD5 hash of the global properties file.</span></span>|  
|`Properties/Path`|<span data-ttu-id="65bb4-215">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-215">String</span></span>|<span data-ttu-id="65bb4-216">A tulajdonságok relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="65bb4-216">Relative path to the properties file.</span></span>|  
|`Properties/Path/@Hash`|<span data-ttu-id="65bb4-217">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-217">Attribute, String</span></span>|<span data-ttu-id="65bb4-218">Base16 kódolású MD5 kivonatoló tulajdonságok fájl.</span><span class="sxs-lookup"><span data-stu-id="65bb4-218">Base16-encoded MD5 hash of the properties file.</span></span>|  
|`Blob/Status`|<span data-ttu-id="65bb4-219">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="65bb4-219">String</span></span>|<span data-ttu-id="65bb4-220">A blob feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="65bb4-220">Status of processing the blob.</span></span>|  
  
# <a name="drive-status-codes"></a><span data-ttu-id="65bb4-221">Meghajtó állapotkódok</span><span class="sxs-lookup"><span data-stu-id="65bb4-221">Drive status codes</span></span>  
<span data-ttu-id="65bb4-222">Az alábbi táblázat a meghajtó feldolgozása állapotkódjai.</span><span class="sxs-lookup"><span data-stu-id="65bb4-222">The following table lists the status codes for processing a drive.</span></span>  
  
|<span data-ttu-id="65bb4-223">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="65bb4-223">Status code</span></span>|<span data-ttu-id="65bb4-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="65bb4-224">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="65bb4-225">A meghajtó feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-225">The drive has finished processing without any errors.</span></span>|  
|`CompletedWithWarnings`|<span data-ttu-id="65bb4-226">A meghajtó figyelmeztetésekkel fejeződött be az egy vagy több blobot a blobok megadott importálási dispositions / feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-226">The drive has finished processing with warnings in one or more blobs per the import dispositions specified for the blobs.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="65bb4-227">A meghajtó egy vagy több blobot, vagy adattömbök befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-227">The drive has finished with errors in one or more blobs or chunks.</span></span>|  
|`DiskNotFound`|<span data-ttu-id="65bb4-228">Nincs lemez a meghajtón található.</span><span class="sxs-lookup"><span data-stu-id="65bb4-228">No disk is found on the drive.</span></span>|  
|`VolumeNotNtfs`|<span data-ttu-id="65bb4-229">Az első adatok lemezen levő kötetet a nem nem NTFS formátumú.</span><span class="sxs-lookup"><span data-stu-id="65bb4-229">The first data volume on the disk is not in NTFS format.</span></span>|  
|`DiskOperationFailed`|<span data-ttu-id="65bb4-230">Ismeretlen hiba történt a meghajtón műveletek végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="65bb4-230">An unknown failure occurred when performing operations on the drive.</span></span>|  
|`BitLockerVolumeNotFound`|<span data-ttu-id="65bb4-231">A BitLocker encryptable kötet található.</span><span class="sxs-lookup"><span data-stu-id="65bb4-231">No BitLocker encryptable volume is found.</span></span>|  
|`BitLockerNotActivated`|<span data-ttu-id="65bb4-232">A BitLocker nincs engedélyezve a köteten.</span><span class="sxs-lookup"><span data-stu-id="65bb4-232">BitLocker is not enabled on the volume.</span></span>|  
|`BitLockerProtectorNotFound`|<span data-ttu-id="65bb4-233">A numerikus jelszó kulcsvédőjének nem létezik a köteten.</span><span class="sxs-lookup"><span data-stu-id="65bb4-233">The numerical password key protector does not exist on the volume.</span></span>|  
|`BitLockerKeyInvalid`|<span data-ttu-id="65bb4-234">A numerikus, a megadott jelszó nem lehet oldani a kötet zárolását.</span><span class="sxs-lookup"><span data-stu-id="65bb4-234">The numerical password provided cannot unlock the volume.</span></span>|  
|`BitLockerUnlockVolumeFailed`|<span data-ttu-id="65bb4-235">Ismeretlen hiba történt a kötet zárolásának feloldására tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="65bb4-235">Unknown failure has happened when trying to unlock the volume.</span></span>|  
|`BitLockerFailed`|<span data-ttu-id="65bb4-236">Ismeretlen hiba történt a BitLocker műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="65bb4-236">An unknown failure occurred while performing BitLocker operations.</span></span>|  
|`ManifestNameInvalid`|<span data-ttu-id="65bb4-237">A jegyzékfájl neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="65bb4-237">The manifest file name is invalid.</span></span>|  
|`ManifestNameTooLong`|<span data-ttu-id="65bb4-238">A jegyzékfájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="65bb4-238">The manifest file name is too long.</span></span>|  
|`ManifestNotFound`|<span data-ttu-id="65bb4-239">A jegyzékfájl nem található.</span><span class="sxs-lookup"><span data-stu-id="65bb4-239">The manifest file is not found.</span></span>|  
|`ManifestAccessDenied`|<span data-ttu-id="65bb4-240">A jegyzékfájl való hozzáférés megtagadva.</span><span class="sxs-lookup"><span data-stu-id="65bb4-240">Access to the manifest file is denied.</span></span>|  
|`ManifestCorrupted`|<span data-ttu-id="65bb4-241">Az Alkalmazásjegyzék-fájl sérült (a tartalom nem egyezik a a kivonata).</span><span class="sxs-lookup"><span data-stu-id="65bb4-241">The manifest file is corrupted (the content does not match its hash).</span></span>|  
|`ManifestFormatInvalid`|<span data-ttu-id="65bb4-242">A jegyzék tartalom nem felel meg a szükséges formátumnak.</span><span class="sxs-lookup"><span data-stu-id="65bb4-242">The manifest content does not conform to the required format.</span></span>|  
|`ManifestDriveIdMismatch`|<span data-ttu-id="65bb4-243">A jegyzékfájl a meghajtó azonosítója nem egyezik meg a meghajtón egy olvasás.</span><span class="sxs-lookup"><span data-stu-id="65bb4-243">The drive ID in the manifest file does not match the one read from the drive.</span></span>|  
|`ReadManifestFailed`|<span data-ttu-id="65bb4-244">A lemez i/o-hiba történt a jegyzék olvasása közben.</span><span class="sxs-lookup"><span data-stu-id="65bb4-244">A disk I/O failure occurred while reading from the manifest.</span></span>|  
|`BlobListFormatInvalid`|<span data-ttu-id="65bb4-245">Az Exportálás blob lista blob nem felel meg a szükséges formátumnak.</span><span class="sxs-lookup"><span data-stu-id="65bb4-245">The export blob list blob does not conform to the required format.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="65bb4-246">A blobot, amely a storage-fiók nem férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="65bb4-246">Access to the blobs in the storage account is forbidden.</span></span> <span data-ttu-id="65bb4-247">Ez akkor lehet érvénytelen tárfiók kulcsa vagy a tároló SAS miatt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-247">This might be due to invalid storage account key or container SAS.</span></span>|  
|`InternalError`|<span data-ttu-id="65bb4-248">És belső hiba történt a meghajtó feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="65bb4-248">And internal error occurred while processing the drive.</span></span>|  
  
## <a name="blob-status-codes"></a><span data-ttu-id="65bb4-249">A BLOB állapotkódok</span><span class="sxs-lookup"><span data-stu-id="65bb4-249">Blob status codes</span></span>  
<span data-ttu-id="65bb4-250">Az alábbi táblázat a állapotkódok blob feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="65bb4-250">The following table lists the status codes for processing a blob.</span></span>  
  
|<span data-ttu-id="65bb4-251">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="65bb4-251">Status code</span></span>|<span data-ttu-id="65bb4-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="65bb4-252">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="65bb4-253">A blob feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-253">The blob has finished processing without errors.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="65bb4-254">A blob hibák egy vagy több tartományokat vagy blokkok, metaadatok vagy-tulajdonságok feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-254">The blob has finished processing with errors in one or more page ranges or blocks, metadata, or properties.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="65bb4-255">A fájlnév érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="65bb4-255">The file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="65bb4-256">A fájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="65bb4-256">The file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="65bb4-257">A fájl nem található.</span><span class="sxs-lookup"><span data-stu-id="65bb4-257">The file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="65bb4-258">A fájlhoz való hozzáférés megtagadva.</span><span class="sxs-lookup"><span data-stu-id="65bb4-258">Access to the file is denied.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="65bb4-259">A blob eléréséhez a Blob szolgáltatási kérelem sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-259">The Blob service request to access the blob has failed.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="65bb4-260">A blob eléréséhez a Blob szolgáltatási kérelem tiltott.</span><span class="sxs-lookup"><span data-stu-id="65bb4-260">The Blob service request to access the blob is forbidden.</span></span> <span data-ttu-id="65bb4-261">Ez akkor lehet érvénytelen tárfiók kulcsa vagy a tároló SAS miatt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-261">This might be due to invalid storage account key or container SAS.</span></span>|  
|`RenameFailed`|<span data-ttu-id="65bb4-262">Nem sikerült átnevezni a blobot (hogy az importálás) vagy a fájlt (exportálási feladat).</span><span class="sxs-lookup"><span data-stu-id="65bb4-262">Failed to rename the blob (for an import job) or the file (for an export job).</span></span>|  
|`BlobUnexpectedChange`|<span data-ttu-id="65bb4-263">Egy váratlan módosítás történt a blob (exportálási feladat).</span><span class="sxs-lookup"><span data-stu-id="65bb4-263">An unexpected change has occurred with the blob (for an export job).</span></span>|  
|`LeasePresent`|<span data-ttu-id="65bb4-264">A blob megtalálható a címbérlet van.</span><span class="sxs-lookup"><span data-stu-id="65bb4-264">There is a lease present on the blob.</span></span>|  
|`IOFailed`|<span data-ttu-id="65bb4-265">A lemez- vagy hálózati i/o-hiba történt a blob feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="65bb4-265">A disk or network I/O failure occurred while processing the blob.</span></span>|  
|`Failed`|<span data-ttu-id="65bb4-266">Ismeretlen hiba történt a blob feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="65bb4-266">An unknown failure occurred while processing the blob.</span></span>|  
  
## <a name="import-disposition-status-codes"></a><span data-ttu-id="65bb4-267">Importálás törlése állapotkódok</span><span class="sxs-lookup"><span data-stu-id="65bb4-267">Import disposition status codes</span></span>  
<span data-ttu-id="65bb4-268">Az alábbi táblázat a állapotkódok megoldása érdekében egy importálási törlése.</span><span class="sxs-lookup"><span data-stu-id="65bb4-268">The following table lists the status codes for resolving an import disposition.</span></span>  
  
|<span data-ttu-id="65bb4-269">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="65bb4-269">Status code</span></span>|<span data-ttu-id="65bb4-270">Leírás</span><span class="sxs-lookup"><span data-stu-id="65bb4-270">Description</span></span>|  
|-----------------|-----------------|  
|`Created`|<span data-ttu-id="65bb4-271">A blob létrehozását.</span><span class="sxs-lookup"><span data-stu-id="65bb4-271">The blob has been created.</span></span>|  
|`Renamed`|<span data-ttu-id="65bb4-272">A blob egy átnevezési importálási törlése át lett nevezve.</span><span class="sxs-lookup"><span data-stu-id="65bb4-272">The blob has been renamed per rename import disposition.</span></span> <span data-ttu-id="65bb4-273">A `Blob/BlobPath` elem tartalmazza-e az átnevezett blob URI-JÁNAK.</span><span class="sxs-lookup"><span data-stu-id="65bb4-273">The `Blob/BlobPath` element contains the URI for the renamed blob.</span></span>|  
|`Skipped`|<span data-ttu-id="65bb4-274">A blob ki lett hagyva `no-overwrite` importálása törlése.</span><span class="sxs-lookup"><span data-stu-id="65bb4-274">The blob has been skipped per `no-overwrite` import disposition.</span></span>|  
|`Overwritten`|<span data-ttu-id="65bb4-275">A blob felülírta egy meglévő blob / `overwrite` importálása törlése.</span><span class="sxs-lookup"><span data-stu-id="65bb4-275">The blob has overwritten an existing blob per `overwrite` import disposition.</span></span>|  
|`Cancelled`|<span data-ttu-id="65bb4-276">Egy korábbi hiba leállt, az importálás témakör további feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="65bb4-276">A prior failure has stopped further processing of the import disposition.</span></span>|  
  
## <a name="page-rangeblock-status-codes"></a><span data-ttu-id="65bb4-277">Tartomány/blokk állapotkódok lap</span><span class="sxs-lookup"><span data-stu-id="65bb4-277">Page range/block status codes</span></span>  
<span data-ttu-id="65bb4-278">A következő táblázat a tartományt vagy egy tiltólista feldolgozása állapotkódjai.</span><span class="sxs-lookup"><span data-stu-id="65bb4-278">The following table lists the status codes for processing a page range or a block.</span></span>  
  
|<span data-ttu-id="65bb4-279">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="65bb4-279">Status code</span></span>|<span data-ttu-id="65bb4-280">Leírás</span><span class="sxs-lookup"><span data-stu-id="65bb4-280">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="65bb4-281">A nyomtatási tartomány vagy blokk feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-281">The page range or block has finished processing without any errors.</span></span>|  
|`Committed`|<span data-ttu-id="65bb4-282">A blokk véglegesítve, de nem a teljes blokkot a listán, mert más blokkok sikertelen, vagy helyezze magát teljes tiltólista sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-282">The block has been committed,  but not in the full block list because other blocks have failed, or put full block list itself has failed.</span></span>|  
|`Uncommitted`|<span data-ttu-id="65bb4-283">A blokk feltölteni, de a nem véglegesített.</span><span class="sxs-lookup"><span data-stu-id="65bb4-283">The block is uploaded but not committed.</span></span>|  
|`Corrupted`|<span data-ttu-id="65bb4-284">A nyomtatási tartomány vagy blokk sérült (a tartalom nem egyezik a a kivonata).</span><span class="sxs-lookup"><span data-stu-id="65bb4-284">The page range or block is corrupted (the content does not match its hash).</span></span>|  
|`FileUnexpectedEnd`|<span data-ttu-id="65bb4-285">Egy nem várt fájlvégjel történt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-285">An unexpected end of file has been encountered.</span></span>|  
|`BlobUnexpectedEnd`|<span data-ttu-id="65bb4-286">Egy blob váratlan vége történt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-286">An unexpected end of blob has been encountered.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="65bb4-287">A Blob szolgáltatási kérelem hozzáféréssel a lap tartomány vagy blokk nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="65bb4-287">The Blob service request to access the page range or block has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="65bb4-288">Lemez- vagy i/o-hiba történt a nyomtatási tartomány vagy blokk feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="65bb4-288">A disk or network I/O failure occurred while processing the page range or block.</span></span>|  
|`Failed`|<span data-ttu-id="65bb4-289">Ismeretlen hiba történt a nyomtatási tartomány vagy blokk feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="65bb4-289">An unknown failure occurred while processing the page range or block.</span></span>|  
|`Cancelled`|<span data-ttu-id="65bb4-290">Egy korábbi hiba leállt, az oldal tartomány vagy blokk további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="65bb4-290">A prior failure has stopped further processing of the page range or block.</span></span>|  
  
## <a name="metadata-status-codes"></a><span data-ttu-id="65bb4-291">Metaadatok állapotkódok</span><span class="sxs-lookup"><span data-stu-id="65bb4-291">Metadata status codes</span></span>  
<span data-ttu-id="65bb4-292">Az alábbi táblázat a állapotkódjai blob metaadatok feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="65bb4-292">The following table lists the status codes for processing blob metadata.</span></span>  
  
|<span data-ttu-id="65bb4-293">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="65bb4-293">Status code</span></span>|<span data-ttu-id="65bb4-294">Leírás</span><span class="sxs-lookup"><span data-stu-id="65bb4-294">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="65bb4-295">A metaadatok feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-295">The metadata has finished processing without errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="65bb4-296">A metaadat neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="65bb4-296">The metadata file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="65bb4-297">A metaadatok fájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="65bb4-297">The metadata file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="65bb4-298">A metaadat-fájl nem található.</span><span class="sxs-lookup"><span data-stu-id="65bb4-298">The metadata file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="65bb4-299">A metaadatfájl való hozzáférés megtagadva.</span><span class="sxs-lookup"><span data-stu-id="65bb4-299">Access to the metadata file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="65bb4-300">A metaadat-fájl sérült (a tartalom nem egyezik a a kivonata).</span><span class="sxs-lookup"><span data-stu-id="65bb4-300">The metadata file is corrupted (the content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="65bb4-301">A metaadat-tartalom nem felel meg a szükséges formátumnak.</span><span class="sxs-lookup"><span data-stu-id="65bb4-301">The metadata content does not conform to the required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="65bb4-302">A Metaadatok írása nem sikerült XML.</span><span class="sxs-lookup"><span data-stu-id="65bb4-302">Writing the metadata XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="65bb4-303">A metaadatok eléréséhez a Blob szolgáltatási kérelem sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-303">The Blob service request to access the metadata has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="65bb4-304">A lemez- vagy hálózati i/o-hiba történt a metaadatok feldolgozásakor a program.</span><span class="sxs-lookup"><span data-stu-id="65bb4-304">A disk or network I/O failure occurred while processing the metadata.</span></span>|  
|`Failed`|<span data-ttu-id="65bb4-305">Egy ismeretlen hiba történt a metaadatok feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="65bb4-305">An unknown failure occurred while processing the metadata.</span></span>|  
|`Cancelled`|<span data-ttu-id="65bb4-306">Egy korábbi hiba leállt, a metaadatok további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="65bb4-306">A prior failure has stopped further processing of the metadata.</span></span>|  
  
## <a name="properties-status-codes"></a><span data-ttu-id="65bb4-307">Tulajdonságok állapotkódok</span><span class="sxs-lookup"><span data-stu-id="65bb4-307">Properties status codes</span></span>  
<span data-ttu-id="65bb4-308">A következő táblázat a blob-tulajdonságok feldolgozása állapotkódjai.</span><span class="sxs-lookup"><span data-stu-id="65bb4-308">The following table lists the status codes for processing blob properties.</span></span>  
  
|<span data-ttu-id="65bb4-309">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="65bb4-309">Status code</span></span>|<span data-ttu-id="65bb4-310">Leírás</span><span class="sxs-lookup"><span data-stu-id="65bb4-310">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="65bb4-311">A Tulajdonságok feldolgozása hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="65bb4-311">The properties have finished processing without any errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="65bb4-312">A tulajdonságok neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="65bb4-312">The properties file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="65bb4-313">A tulajdonságok fájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="65bb4-313">The properties file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="65bb4-314">A tulajdonságok fájl nem található.</span><span class="sxs-lookup"><span data-stu-id="65bb4-314">The properties file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="65bb4-315">A Tulajdonságok fájlhoz való hozzáférés megtagadva.</span><span class="sxs-lookup"><span data-stu-id="65bb4-315">Access to the properties file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="65bb4-316">A tulajdonságok fájl sérült (a tartalom nem egyezik a a kivonata).</span><span class="sxs-lookup"><span data-stu-id="65bb4-316">The properties file is corrupted (the content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="65bb4-317">A Tulajdonságok tartalom nem felel meg a szükséges formátumnak.</span><span class="sxs-lookup"><span data-stu-id="65bb4-317">The properties content does not conform to the required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="65bb4-318">A Tulajdonságok írása nem sikerült XML.</span><span class="sxs-lookup"><span data-stu-id="65bb4-318">Writing the properties XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="65bb4-319">Nem sikerült a Blob szolgáltatási kérelem tulajdonságainak eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="65bb4-319">The Blob service request to access the properties has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="65bb4-320">A lemez- vagy hálózati i/o-hiba történt a Tulajdonságok feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="65bb4-320">A disk or network I/O failure occurred while processing the properties.</span></span>|  
|`Failed`|<span data-ttu-id="65bb4-321">Ismeretlen hiba történt a Tulajdonságok feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="65bb4-321">An unknown failure occurred while processing the properties.</span></span>|  
|`Cancelled`|<span data-ttu-id="65bb4-322">Egy korábbi hiba a tulajdonságok további feldolgozás leállt.</span><span class="sxs-lookup"><span data-stu-id="65bb4-322">A prior failure has stopped further processing of the properties.</span></span>|  
  
## <a name="sample-logs"></a><span data-ttu-id="65bb4-323">A minta-naplók</span><span class="sxs-lookup"><span data-stu-id="65bb4-323">Sample logs</span></span>  
<span data-ttu-id="65bb4-324">A következő példában látható egy részletes napló.</span><span class="sxs-lookup"><span data-stu-id="65bb4-324">The following is an example of verbose log.</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
<span data-ttu-id="65bb4-325">A megfelelő hibanapló alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="65bb4-325">The corresponding error log is shown below.</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 <span data-ttu-id="65bb4-326">A kövesse hibanaplóját, hogy az importálás a fájl nem található az importálási meghajtón kapcsolatos hibát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="65bb4-326">The follow error log for an import job contains an error about a file not found on the import drive.</span></span> <span data-ttu-id="65bb4-327">Vegye figyelembe, hogy további összetevők állapota `Cancelled`.</span><span class="sxs-lookup"><span data-stu-id="65bb4-327">Note that the status of subsequent components is `Cancelled`.</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

<span data-ttu-id="65bb4-328">Exportálási feladat a következő hibanaplóban azt jelzi, hogy a blob tartalma írása sikeresen befejeződött a meghajtóra, azonban, hogy hiba történt a blob tulajdonságai exportálása során.</span><span class="sxs-lookup"><span data-stu-id="65bb4-328">The following error log for an export job indicates that the blob content has been successfully written to the drive, but that an error occurred while exporting the blob's properties.</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a><span data-ttu-id="65bb4-329">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65bb4-329">Next steps</span></span>
 
* [<span data-ttu-id="65bb4-330">Storage Import/Export REST API felülete</span><span class="sxs-lookup"><span data-stu-id="65bb4-330">Storage Import/Export REST API</span></span>](/rest/api/storageimportexport/)
