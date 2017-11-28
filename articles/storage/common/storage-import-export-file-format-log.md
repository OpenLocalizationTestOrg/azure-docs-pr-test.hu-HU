---
title: "aaaAzure importálási/exportálási formátumát |} Microsoft Docs"
description: "További információk a lépéseket az Import/Export szolgáltatás feladat végrehajtásakor létrehozott hello naplófájlok hello formátuma."
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
ms.openlocfilehash: 15a652455aa947922af0aa39ccefe68811a3db19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-log-file-format"></a><span data-ttu-id="f5e47-103">Az Azure Import/Export szolgáltatás naplófájlformátum</span><span class="sxs-lookup"><span data-stu-id="f5e47-103">Azure Import/Export service log file format</span></span>
<span data-ttu-id="f5e47-104">Amikor hello Microsoft Azure Import/Export szolgáltatás hajt végre, az importálási feladat vagy exportálási feladat részeként egy műveletet a meghajtón, írja a naplókat tooblock blobok adott feladattal társított hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="f5e47-104">When hello Microsoft Azure Import/Export service performs an action on a drive as part of an import job or an export job, logs are written tooblock blobs in hello storage account associated with that job.</span></span>  
  
<span data-ttu-id="f5e47-105">Hello Import/Export szolgáltatás által írható két naplók van:</span><span class="sxs-lookup"><span data-stu-id="f5e47-105">There are two logs that may be written by hello Import/Export service:</span></span>  
  
-   <span data-ttu-id="f5e47-106">hello hibanapló mindig egy hiba hello esemény jön létre.</span><span class="sxs-lookup"><span data-stu-id="f5e47-106">hello error log is always generated in hello event of an error.</span></span>  
  
-   <span data-ttu-id="f5e47-107">hello részletes naplózás alapértelmezés szerint nincs engedélyezve, de előfordulhat, hogy engedélyezni lehessen hello beállítása `EnableVerboseLog` tulajdonságának egy [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) műveletet.</span><span class="sxs-lookup"><span data-stu-id="f5e47-107">hello verbose log is not enabled by default, but may be enabled by setting hello `EnableVerboseLog` property on a [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation.</span></span>  
  
## <a name="log-file-location"></a><span data-ttu-id="f5e47-108">Naplófájl helye</span><span class="sxs-lookup"><span data-stu-id="f5e47-108">Log file location</span></span>  
<span data-ttu-id="f5e47-109">hello írja a naplókat blobok tooblock hello tároló vagy hello által megadott virtuális könyvtár `ImportExportStatesPath` beállítás, amely állíthat be egy `Put Job` műveletet.</span><span class="sxs-lookup"><span data-stu-id="f5e47-109">hello logs are written tooblock blobs in hello container or virtual directory specified by hello `ImportExportStatesPath` setting, which you can set on a `Put Job` operation.</span></span> <span data-ttu-id="f5e47-110">Írja a hello hely toowhich hello naplókat attól függ, hogyan hitelesítési hello feladat együtt megadott hello értéke meg van adva `ImportExportStatesPath`.</span><span class="sxs-lookup"><span data-stu-id="f5e47-110">hello location toowhich hello logs are written depends on how authentication is specified for hello job, together with hello value specified for `ImportExportStatesPath`.</span></span> <span data-ttu-id="f5e47-111">A tárfiók kulcsa, vagy a tároló SAS (közös hozzáférésű jogosultságkód) hitelesítési hello feladat adható meg.</span><span class="sxs-lookup"><span data-stu-id="f5e47-111">Authentication for hello job may be specified via a storage account key, or a container SAS (shared access signature).</span></span>  
  
<span data-ttu-id="f5e47-112">hello hello tároló vagy a virtuális könyvtár nevét is vagy hello alapértelmezett neve `waimportexport`, vagy egy másik tárolóhoz vagy a megadott virtuális könyvtár neve.</span><span class="sxs-lookup"><span data-stu-id="f5e47-112">hello name of hello container or virtual directory may either be hello default name of `waimportexport`, or another container or virtual directory name that you specify.</span></span>  
  
<span data-ttu-id="f5e47-113">az alábbi táblázat hello hello lehetséges lehetőségeket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="f5e47-113">hello table below shows hello possible options:</span></span>  
  
|<span data-ttu-id="f5e47-114">Hitelesítési módszer</span><span class="sxs-lookup"><span data-stu-id="f5e47-114">Authentication Method</span></span>|<span data-ttu-id="f5e47-115">Az érték `ImportExportStatesPath`elem</span><span class="sxs-lookup"><span data-stu-id="f5e47-115">Value of `ImportExportStatesPath`Element</span></span>|<span data-ttu-id="f5e47-116">Blobok napló helye</span><span class="sxs-lookup"><span data-stu-id="f5e47-116">Location of Log Blobs</span></span>|  
|---------------------------|----------------------------------------------|---------------------------|  
|<span data-ttu-id="f5e47-117">Tárfiók kulcsa</span><span class="sxs-lookup"><span data-stu-id="f5e47-117">Storage account key</span></span>|<span data-ttu-id="f5e47-118">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f5e47-118">Default value</span></span>|<span data-ttu-id="f5e47-119">Nevű tárolót `waimportexport`, ez az alapértelmezett tároló hello.</span><span class="sxs-lookup"><span data-stu-id="f5e47-119">A container named `waimportexport`, which is hello default container.</span></span> <span data-ttu-id="f5e47-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="f5e47-120">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|<span data-ttu-id="f5e47-121">Tárfiók kulcsa</span><span class="sxs-lookup"><span data-stu-id="f5e47-121">Storage account key</span></span>|<span data-ttu-id="f5e47-122">A felhasználó által megadott érték</span><span class="sxs-lookup"><span data-stu-id="f5e47-122">User-specified value</span></span>|<span data-ttu-id="f5e47-123">Hello felhasználó nevű tárolót.</span><span class="sxs-lookup"><span data-stu-id="f5e47-123">A container named by hello user.</span></span> <span data-ttu-id="f5e47-124">Példa:</span><span class="sxs-lookup"><span data-stu-id="f5e47-124">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|<span data-ttu-id="f5e47-125">A tároló SAS</span><span class="sxs-lookup"><span data-stu-id="f5e47-125">Container SAS</span></span>|<span data-ttu-id="f5e47-126">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f5e47-126">Default value</span></span>|<span data-ttu-id="f5e47-127">Nevű virtuális könyvtár `waimportexport`, amely hello alapértelmezett nevét, hello SAS megadott hello tároló alatt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-127">A virtual directory named `waimportexport`, which is hello default name, beneath hello container specified in hello SAS.</span></span><br /><br /> <span data-ttu-id="f5e47-128">Például, ha a megadott hello SAS hello folyamat `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, majd hello napló helyét lenne.`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span><span class="sxs-lookup"><span data-stu-id="f5e47-128">For example, if hello SAS specified for hello job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, then hello log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span></span>|  
|<span data-ttu-id="f5e47-129">A tároló SAS</span><span class="sxs-lookup"><span data-stu-id="f5e47-129">Container SAS</span></span>|<span data-ttu-id="f5e47-130">A felhasználó által megadott érték</span><span class="sxs-lookup"><span data-stu-id="f5e47-130">User-specified value</span></span>|<span data-ttu-id="f5e47-131">Hello felhasználó alatt megadott hello SAS hello tároló nevű virtuális könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="f5e47-131">A virtual directory named by hello user, beneath hello container specified in hello SAS.</span></span><br /><br /> <span data-ttu-id="f5e47-132">Például, ha a megadott hello SAS hello folyamat `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, és hello megadott virtuális könyvtár neve `mylogblobs`, majd hello napló helyét lenne `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span><span class="sxs-lookup"><span data-stu-id="f5e47-132">For example, if hello SAS specified for hello job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, and hello specified virtual directory is named `mylogblobs`, then hello log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span></span>|  
  
<span data-ttu-id="f5e47-133">Hello URL-címe hello hiba és a részletes naplókat hívó hello olvashatók be [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet.</span><span class="sxs-lookup"><span data-stu-id="f5e47-133">You can retrieve hello URL for hello error and verbose logs by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="f5e47-134">hello érhetők el naplók hello meghajtó feldolgozás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f5e47-134">hello logs are available after processing of hello drive is complete.</span></span>  
  
## <a name="log-file-format"></a><span data-ttu-id="f5e47-135">Naplófájlformátum</span><span class="sxs-lookup"><span data-stu-id="f5e47-135">Log file format</span></span>  
<span data-ttu-id="f5e47-136">hello mindkét naplók formátumban van hello ugyanaz: hello az eseményeket, amelyek a blobok hello merevlemezre és hello felhasználói fiókhoz közötti másolás közben történt az XML-leírását tartalmazó blob.</span><span class="sxs-lookup"><span data-stu-id="f5e47-136">hello format for both logs is hello same: a blob containing XML descriptions of hello events that occurred while copying blobs between hello hard drive and hello customer's account.</span></span>  
  
<span data-ttu-id="f5e47-137">hello részletes teljes vonatkozó információkat tartalmaz hello állapot hello másolási művelet minden egyes blob (hogy az importálás) vagy a fájlt (exportálási feladat), mivel hello hibanapló csak hello adatait blobok vagy hibát közben hello fájlokat tartalmazza importálhatja és exportálhatja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="f5e47-137">hello verbose log contains complete information about hello status of hello copy operation for every blob (for an import job) or file (for an export job), whereas hello error log contains only hello information for blobs or files that encountered errors during hello import or export job.</span></span>  
  
<span data-ttu-id="f5e47-138">hello részletes napló formátuma alább látható.</span><span class="sxs-lookup"><span data-stu-id="f5e47-138">hello verbose log format is shown below.</span></span> <span data-ttu-id="f5e47-139">hello hibanapló hello azonos struktúra, de sikeres műveletek kiszűri rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f5e47-139">hello error log has hello same structure, but filters out successful operations.</span></span>  

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

<span data-ttu-id="f5e47-140">hello következő táblázatban hello elemek hello naplófájl.</span><span class="sxs-lookup"><span data-stu-id="f5e47-140">hello following table describes hello elements of hello log file.</span></span>  
  
|<span data-ttu-id="f5e47-141">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-141">XML Element</span></span>|<span data-ttu-id="f5e47-142">Típus</span><span class="sxs-lookup"><span data-stu-id="f5e47-142">Type</span></span>|<span data-ttu-id="f5e47-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5e47-143">Description</span></span>|  
|-----------------|----------|-----------------|  
|`DriveLog`|<span data-ttu-id="f5e47-144">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-144">XML Element</span></span>|<span data-ttu-id="f5e47-145">A meghajtó napló jelöli.</span><span class="sxs-lookup"><span data-stu-id="f5e47-145">Represents a drive log.</span></span>|  
|`Version`|<span data-ttu-id="f5e47-146">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-146">Attribute, String</span></span>|<span data-ttu-id="f5e47-147">hello naplóformátumban hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="f5e47-147">hello version of hello log format.</span></span>|  
|`DriveId`|<span data-ttu-id="f5e47-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-148">String</span></span>|<span data-ttu-id="f5e47-149">hello meghajtó hardver sorozatszámát.</span><span class="sxs-lookup"><span data-stu-id="f5e47-149">hello drive's hardware serial number.</span></span>|  
|`Status`|<span data-ttu-id="f5e47-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-150">String</span></span>|<span data-ttu-id="f5e47-151">Hello meghajtó feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="f5e47-151">Status of hello drive processing.</span></span> <span data-ttu-id="f5e47-152">Lásd: hello `Drive Status Codes` tábla alább olvashat.</span><span class="sxs-lookup"><span data-stu-id="f5e47-152">See hello `Drive Status Codes` table below for more information.</span></span>|  
|`Blob`|<span data-ttu-id="f5e47-153">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-153">Nested XML element</span></span>|<span data-ttu-id="f5e47-154">Egy blob jelöli.</span><span class="sxs-lookup"><span data-stu-id="f5e47-154">Represents a blob.</span></span>|  
|`Blob/BlobPath`|<span data-ttu-id="f5e47-155">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-155">String</span></span>|<span data-ttu-id="f5e47-156">hello hello blob URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f5e47-156">hello URI of hello blob.</span></span>|  
|`Blob/FilePath`|<span data-ttu-id="f5e47-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-157">String</span></span>|<span data-ttu-id="f5e47-158">hello relatív elérési út toohello fájl hello meghajtón.</span><span class="sxs-lookup"><span data-stu-id="f5e47-158">hello relative path toohello file on hello drive.</span></span>|  
|`Blob/Snapshot`|<span data-ttu-id="f5e47-159">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f5e47-159">DateTime</span></span>|<span data-ttu-id="f5e47-160">hello blob, csak exportálási feladat hello pillanatkép verzióját.</span><span class="sxs-lookup"><span data-stu-id="f5e47-160">hello snapshot version of hello blob, for an export job only.</span></span>|  
|`Blob/Length`|<span data-ttu-id="f5e47-161">Egész szám</span><span class="sxs-lookup"><span data-stu-id="f5e47-161">Integer</span></span>|<span data-ttu-id="f5e47-162">hello teljes hossza hello blob bájtban.</span><span class="sxs-lookup"><span data-stu-id="f5e47-162">hello total length of hello blob in bytes.</span></span>|  
|`Blob/LastModified`|<span data-ttu-id="f5e47-163">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f5e47-163">DateTime</span></span>|<span data-ttu-id="f5e47-164">hello dátum/idő, hogy a hello blob voltak utoljára módosítva, csak az exportálási feladat.</span><span class="sxs-lookup"><span data-stu-id="f5e47-164">hello date/time that hello blob was last modified, for an export job only.</span></span>|  
|`Blob/ImportDisposition`|<span data-ttu-id="f5e47-165">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-165">String</span></span>|<span data-ttu-id="f5e47-166">hello importálása hello BLOB, hogy az importálás csak törlése.</span><span class="sxs-lookup"><span data-stu-id="f5e47-166">hello import disposition of hello blob, for an import job only.</span></span>|  
|`Blob/ImportDisposition/@Status`|<span data-ttu-id="f5e47-167">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-167">Attribute, String</span></span>|<span data-ttu-id="f5e47-168">hello hello állapotának importálása törlése.</span><span class="sxs-lookup"><span data-stu-id="f5e47-168">hello status of hello import disposition.</span></span>|  
|`PageRangeList`|<span data-ttu-id="f5e47-169">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-169">Nested XML element</span></span>|<span data-ttu-id="f5e47-170">Az oldalakra vonatkozó blob tartományokat listáját jelöli.</span><span class="sxs-lookup"><span data-stu-id="f5e47-170">Represents a list of page ranges for a page blob.</span></span>|  
|`PageRange`|<span data-ttu-id="f5e47-171">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-171">XML element</span></span>|<span data-ttu-id="f5e47-172">Egy laptartomány jelöli.</span><span class="sxs-lookup"><span data-stu-id="f5e47-172">Represents a page range.</span></span>|  
|`PageRange/@Offset`|<span data-ttu-id="f5e47-173">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="f5e47-173">Attribute, Integer</span></span>|<span data-ttu-id="f5e47-174">Hello blob eltolása hello laptartomány kezdve.</span><span class="sxs-lookup"><span data-stu-id="f5e47-174">Starting offset of hello page range in hello blob.</span></span>|  
|`PageRange/@Length`|<span data-ttu-id="f5e47-175">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="f5e47-175">Attribute, Integer</span></span>|<span data-ttu-id="f5e47-176">Hello laptartomány hossza (bájt).</span><span class="sxs-lookup"><span data-stu-id="f5e47-176">Length in bytes of hello page range.</span></span>|  
|`PageRange/@Hash`|<span data-ttu-id="f5e47-177">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-177">Attribute, String</span></span>|<span data-ttu-id="f5e47-178">Base16 kódolású MD5 kivonatoló hello lap tartomány.</span><span class="sxs-lookup"><span data-stu-id="f5e47-178">Base16-encoded MD5 hash of hello page range.</span></span>|  
|`PageRange/@Status`|<span data-ttu-id="f5e47-179">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-179">Attribute, String</span></span>|<span data-ttu-id="f5e47-180">Hello laptartomány feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="f5e47-180">Status of processing hello page range.</span></span>|  
|`BlockList`|<span data-ttu-id="f5e47-181">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-181">Nested XML element</span></span>|<span data-ttu-id="f5e47-182">Egy blokkblobhoz tartoznak blokkok listája jelöli.</span><span class="sxs-lookup"><span data-stu-id="f5e47-182">Represents a list of blocks for a block blob.</span></span>|  
|`Block`|<span data-ttu-id="f5e47-183">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-183">XML element</span></span>|<span data-ttu-id="f5e47-184">A blokk jelöli.</span><span class="sxs-lookup"><span data-stu-id="f5e47-184">Represents a block.</span></span>|  
|`Block/@Offset`|<span data-ttu-id="f5e47-185">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="f5e47-185">Attribute, Integer</span></span>|<span data-ttu-id="f5e47-186">Kezdő eltolása hello blob hello blokkja.</span><span class="sxs-lookup"><span data-stu-id="f5e47-186">Starting offset of hello block in hello blob.</span></span>|  
|`Block/@Length`|<span data-ttu-id="f5e47-187">Attribútum, egész szám</span><span class="sxs-lookup"><span data-stu-id="f5e47-187">Attribute, Integer</span></span>|<span data-ttu-id="f5e47-188">Hello blokk hossza bájtban.</span><span class="sxs-lookup"><span data-stu-id="f5e47-188">Length in bytes of hello block.</span></span>|  
|`Block/@Id`|<span data-ttu-id="f5e47-189">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-189">Attribute, String</span></span>|<span data-ttu-id="f5e47-190">hello blokk-azonosító.</span><span class="sxs-lookup"><span data-stu-id="f5e47-190">hello block ID.</span></span>|  
|`Block/@Hash`|<span data-ttu-id="f5e47-191">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-191">Attribute, String</span></span>|<span data-ttu-id="f5e47-192">Base16 kódolású MD5 kivonatoló hello blokk.</span><span class="sxs-lookup"><span data-stu-id="f5e47-192">Base16-encoded MD5 hash of hello block.</span></span>|  
|`Block/@Status`|<span data-ttu-id="f5e47-193">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-193">Attribute, String</span></span>|<span data-ttu-id="f5e47-194">Hello blokk feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="f5e47-194">Status of processing hello block.</span></span>|  
|`Metadata`|<span data-ttu-id="f5e47-195">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-195">Nested XML element</span></span>|<span data-ttu-id="f5e47-196">Hello blob metaadatai jelöli.</span><span class="sxs-lookup"><span data-stu-id="f5e47-196">Represents hello blob's metadata.</span></span>|  
|`Metadata/@Status`|<span data-ttu-id="f5e47-197">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-197">Attribute, String</span></span>|<span data-ttu-id="f5e47-198">Hello blob metaadatok feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="f5e47-198">Status of processing of hello blob metadata.</span></span>|  
|`Metadata/GlobalPath`|<span data-ttu-id="f5e47-199">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-199">String</span></span>|<span data-ttu-id="f5e47-200">Relatív elérési út toohello globális metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="f5e47-200">Relative path toohello global metadata file.</span></span>|  
|`Metadata/GlobalPath/@Hash`|<span data-ttu-id="f5e47-201">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-201">Attribute, String</span></span>|<span data-ttu-id="f5e47-202">Base16 kódolású MD5 kivonatoló hello globális metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="f5e47-202">Base16-encoded MD5 hash of hello global metadata file.</span></span>|  
|`Metadata/Path`|<span data-ttu-id="f5e47-203">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-203">String</span></span>|<span data-ttu-id="f5e47-204">Relatív elérési út toohello metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="f5e47-204">Relative path toohello metadata file.</span></span>|  
|`Metadata/Path/@Hash`|<span data-ttu-id="f5e47-205">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-205">Attribute, String</span></span>|<span data-ttu-id="f5e47-206">Base16 kódolású MD5 kivonatoló hello metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="f5e47-206">Base16-encoded MD5 hash of hello metadata file.</span></span>|  
|`Properties`|<span data-ttu-id="f5e47-207">Beágyazott XML-elem.</span><span class="sxs-lookup"><span data-stu-id="f5e47-207">Nested XML element</span></span>|<span data-ttu-id="f5e47-208">Hello blob tulajdonságait adja meg.</span><span class="sxs-lookup"><span data-stu-id="f5e47-208">Represents hello blob properties.</span></span>|  
|`Properties/@Status`|<span data-ttu-id="f5e47-209">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-209">Attribute, String</span></span>|<span data-ttu-id="f5e47-210">Feldolgozási hello blob tulajdonságai, például a fájl nem található, állapotának befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-210">Status of processing hello blob properties, e.g. file not found, completed.</span></span>|  
|`Properties/GlobalPath`|<span data-ttu-id="f5e47-211">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-211">String</span></span>|<span data-ttu-id="f5e47-212">Toohello globális tulajdonságok fájl relatív elérési út.</span><span class="sxs-lookup"><span data-stu-id="f5e47-212">Relative path toohello global properties file.</span></span>|  
|`Properties/GlobalPath/@Hash`|<span data-ttu-id="f5e47-213">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-213">Attribute, String</span></span>|<span data-ttu-id="f5e47-214">Base16 kódolású MD5 kivonatoló hello globális tulajdonságok fájl.</span><span class="sxs-lookup"><span data-stu-id="f5e47-214">Base16-encoded MD5 hash of hello global properties file.</span></span>|  
|`Properties/Path`|<span data-ttu-id="f5e47-215">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-215">String</span></span>|<span data-ttu-id="f5e47-216">Toohello tulajdonságok fájl relatív elérési út.</span><span class="sxs-lookup"><span data-stu-id="f5e47-216">Relative path toohello properties file.</span></span>|  
|`Properties/Path/@Hash`|<span data-ttu-id="f5e47-217">Attribútum, karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-217">Attribute, String</span></span>|<span data-ttu-id="f5e47-218">Base16 kódolású MD5 kivonatoló hello tulajdonságok fájl.</span><span class="sxs-lookup"><span data-stu-id="f5e47-218">Base16-encoded MD5 hash of hello properties file.</span></span>|  
|`Blob/Status`|<span data-ttu-id="f5e47-219">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f5e47-219">String</span></span>|<span data-ttu-id="f5e47-220">Hello blob feldolgozási állapotát.</span><span class="sxs-lookup"><span data-stu-id="f5e47-220">Status of processing hello blob.</span></span>|  
  
# <a name="drive-status-codes"></a><span data-ttu-id="f5e47-221">Meghajtó állapotkódok</span><span class="sxs-lookup"><span data-stu-id="f5e47-221">Drive status codes</span></span>  
<span data-ttu-id="f5e47-222">hello következő táblázatban hello állapotkódok egy meghajtó feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="f5e47-222">hello following table lists hello status codes for processing a drive.</span></span>  
  
|<span data-ttu-id="f5e47-223">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="f5e47-223">Status code</span></span>|<span data-ttu-id="f5e47-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5e47-224">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="f5e47-225">hello meghajtó feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-225">hello drive has finished processing without any errors.</span></span>|  
|`CompletedWithWarnings`|<span data-ttu-id="f5e47-226">hello meghajtó figyelmeztetésekkel fejeződött be a / hello importálási dispositions hello blobok megadott egy vagy több blobot feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-226">hello drive has finished processing with warnings in one or more blobs per hello import dispositions specified for hello blobs.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="f5e47-227">egy vagy több blobot, vagy adattömbök hello meghajtó befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-227">hello drive has finished with errors in one or more blobs or chunks.</span></span>|  
|`DiskNotFound`|<span data-ttu-id="f5e47-228">Nincs lemez hello meghajtón található.</span><span class="sxs-lookup"><span data-stu-id="f5e47-228">No disk is found on hello drive.</span></span>|  
|`VolumeNotNtfs`|<span data-ttu-id="f5e47-229">hello első adatok lemezen levő kötetet a hello nem nem NTFS formátumú.</span><span class="sxs-lookup"><span data-stu-id="f5e47-229">hello first data volume on hello disk is not in NTFS format.</span></span>|  
|`DiskOperationFailed`|<span data-ttu-id="f5e47-230">Ismeretlen hiba történt, amikor hello meghajtón műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="f5e47-230">An unknown failure occurred when performing operations on hello drive.</span></span>|  
|`BitLockerVolumeNotFound`|<span data-ttu-id="f5e47-231">A BitLocker encryptable kötet található.</span><span class="sxs-lookup"><span data-stu-id="f5e47-231">No BitLocker encryptable volume is found.</span></span>|  
|`BitLockerNotActivated`|<span data-ttu-id="f5e47-232">A BitLocker nincs engedélyezve a hello kötet.</span><span class="sxs-lookup"><span data-stu-id="f5e47-232">BitLocker is not enabled on hello volume.</span></span>|  
|`BitLockerProtectorNotFound`|<span data-ttu-id="f5e47-233">hello numerikus jelszavas kötetvédő nem létezik hello köteten.</span><span class="sxs-lookup"><span data-stu-id="f5e47-233">hello numerical password key protector does not exist on hello volume.</span></span>|  
|`BitLockerKeyInvalid`|<span data-ttu-id="f5e47-234">hello numerikus jelszót adott meg, nem oldhatja fel hello kötet.</span><span class="sxs-lookup"><span data-stu-id="f5e47-234">hello numerical password provided cannot unlock hello volume.</span></span>|  
|`BitLockerUnlockVolumeFailed`|<span data-ttu-id="f5e47-235">Ismeretlen hiba történt a toounlock hello kötet tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="f5e47-235">Unknown failure has happened when trying toounlock hello volume.</span></span>|  
|`BitLockerFailed`|<span data-ttu-id="f5e47-236">Ismeretlen hiba történt a BitLocker műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="f5e47-236">An unknown failure occurred while performing BitLocker operations.</span></span>|  
|`ManifestNameInvalid`|<span data-ttu-id="f5e47-237">hello jegyzékfájl neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="f5e47-237">hello manifest file name is invalid.</span></span>|  
|`ManifestNameTooLong`|<span data-ttu-id="f5e47-238">hello jegyzékfájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="f5e47-238">hello manifest file name is too long.</span></span>|  
|`ManifestNotFound`|<span data-ttu-id="f5e47-239">hello jegyzékfájl nem található.</span><span class="sxs-lookup"><span data-stu-id="f5e47-239">hello manifest file is not found.</span></span>|  
|`ManifestAccessDenied`|<span data-ttu-id="f5e47-240">Toohello jegyzékfájl-hozzáférés megtagadva.</span><span class="sxs-lookup"><span data-stu-id="f5e47-240">Access toohello manifest file is denied.</span></span>|  
|`ManifestCorrupted`|<span data-ttu-id="f5e47-241">hello Alkalmazásjegyzék-fájl sérült (hello tartalom nem felel meg a kivonatoló).</span><span class="sxs-lookup"><span data-stu-id="f5e47-241">hello manifest file is corrupted (hello content does not match its hash).</span></span>|  
|`ManifestFormatInvalid`|<span data-ttu-id="f5e47-242">hello jegyzék tartalom nem felel meg a toohello megkívánt formátumban.</span><span class="sxs-lookup"><span data-stu-id="f5e47-242">hello manifest content does not conform toohello required format.</span></span>|  
|`ManifestDriveIdMismatch`|<span data-ttu-id="f5e47-243">hello meghajtó hello jegyzékfájl-azonosító nem egyezik meg a hello egy hello meghajtóról olvasni.</span><span class="sxs-lookup"><span data-stu-id="f5e47-243">hello drive ID in hello manifest file does not match hello one read from hello drive.</span></span>|  
|`ReadManifestFailed`|<span data-ttu-id="f5e47-244">A lemez i/o-hiba történt a következő jegyzékből hello olvasás közben.</span><span class="sxs-lookup"><span data-stu-id="f5e47-244">A disk I/O failure occurred while reading from hello manifest.</span></span>|  
|`BlobListFormatInvalid`|<span data-ttu-id="f5e47-245">hello exportálási blob lista blob nem felel meg a toohello megkívánt formátumban.</span><span class="sxs-lookup"><span data-stu-id="f5e47-245">hello export blob list blob does not conform toohello required format.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="f5e47-246">Blobok elérése az toohello hello tárfiókban tiltott.</span><span class="sxs-lookup"><span data-stu-id="f5e47-246">Access toohello blobs in hello storage account is forbidden.</span></span> <span data-ttu-id="f5e47-247">Előfordulhat, hogy ennek lehet tooinvalid tárfiók kulcsa vagy a tároló SAS.</span><span class="sxs-lookup"><span data-stu-id="f5e47-247">This might be due tooinvalid storage account key or container SAS.</span></span>|  
|`InternalError`|<span data-ttu-id="f5e47-248">És hello meghajtó feldolgozása közben belső hiba történt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-248">And internal error occurred while processing hello drive.</span></span>|  
  
## <a name="blob-status-codes"></a><span data-ttu-id="f5e47-249">A BLOB állapotkódok</span><span class="sxs-lookup"><span data-stu-id="f5e47-249">Blob status codes</span></span>  
<span data-ttu-id="f5e47-250">hello következő táblázatban hello állapotkódok blob feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="f5e47-250">hello following table lists hello status codes for processing a blob.</span></span>  
  
|<span data-ttu-id="f5e47-251">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="f5e47-251">Status code</span></span>|<span data-ttu-id="f5e47-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5e47-252">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="f5e47-253">hello blob feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-253">hello blob has finished processing without errors.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="f5e47-254">hello blob hibák egy vagy több tartományokat vagy blokkok, metaadatok vagy-tulajdonságok feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-254">hello blob has finished processing with errors in one or more page ranges or blocks, metadata, or properties.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="f5e47-255">hello neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="f5e47-255">hello file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="f5e47-256">hello fájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="f5e47-256">hello file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="f5e47-257">hello fájl nem található.</span><span class="sxs-lookup"><span data-stu-id="f5e47-257">hello file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="f5e47-258">Toohello fájl elérése megtagadva.</span><span class="sxs-lookup"><span data-stu-id="f5e47-258">Access toohello file is denied.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="f5e47-259">tooaccess hello blob hello Blob szolgáltatási kérelem sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-259">hello Blob service request tooaccess hello blob has failed.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="f5e47-260">hello Blob szolgáltatási kérelem tooaccess hello blob tiltott.</span><span class="sxs-lookup"><span data-stu-id="f5e47-260">hello Blob service request tooaccess hello blob is forbidden.</span></span> <span data-ttu-id="f5e47-261">Előfordulhat, hogy ennek lehet tooinvalid tárfiók kulcsa vagy a tároló SAS.</span><span class="sxs-lookup"><span data-stu-id="f5e47-261">This might be due tooinvalid storage account key or container SAS.</span></span>|  
|`RenameFailed`|<span data-ttu-id="f5e47-262">Nem sikerült toorename hello blob (hogy az importálás) vagy hello fájlt (exportálási feladat).</span><span class="sxs-lookup"><span data-stu-id="f5e47-262">Failed toorename hello blob (for an import job) or hello file (for an export job).</span></span>|  
|`BlobUnexpectedChange`|<span data-ttu-id="f5e47-263">(Az exportálási feladat) hello blob egy váratlan módosítás történt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-263">An unexpected change has occurred with hello blob (for an export job).</span></span>|  
|`LeasePresent`|<span data-ttu-id="f5e47-264">Nincs a címbérlet hello blob megtalálható.</span><span class="sxs-lookup"><span data-stu-id="f5e47-264">There is a lease present on hello blob.</span></span>|  
|`IOFailed`|<span data-ttu-id="f5e47-265">A lemez- vagy hálózati i/o-hiba történt a hello blob feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="f5e47-265">A disk or network I/O failure occurred while processing hello blob.</span></span>|  
|`Failed`|<span data-ttu-id="f5e47-266">Ismeretlen hiba történt a hello blob feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="f5e47-266">An unknown failure occurred while processing hello blob.</span></span>|  
  
## <a name="import-disposition-status-codes"></a><span data-ttu-id="f5e47-267">Importálás törlése állapotkódok</span><span class="sxs-lookup"><span data-stu-id="f5e47-267">Import disposition status codes</span></span>  
<span data-ttu-id="f5e47-268">hello következő táblázatban hello állapotkódjai feloldása egy importálási törlése.</span><span class="sxs-lookup"><span data-stu-id="f5e47-268">hello following table lists hello status codes for resolving an import disposition.</span></span>  
  
|<span data-ttu-id="f5e47-269">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="f5e47-269">Status code</span></span>|<span data-ttu-id="f5e47-270">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5e47-270">Description</span></span>|  
|-----------------|-----------------|  
|`Created`|<span data-ttu-id="f5e47-271">hello blob létrehozását.</span><span class="sxs-lookup"><span data-stu-id="f5e47-271">hello blob has been created.</span></span>|  
|`Renamed`|<span data-ttu-id="f5e47-272">hello blob egy átnevezési importálási törlése át lett nevezve.</span><span class="sxs-lookup"><span data-stu-id="f5e47-272">hello blob has been renamed per rename import disposition.</span></span> <span data-ttu-id="f5e47-273">Hello `Blob/BlobPath` elem átnevezése hello BLOB URI hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f5e47-273">hello `Blob/BlobPath` element contains hello URI for hello renamed blob.</span></span>|  
|`Skipped`|<span data-ttu-id="f5e47-274">hello blob ki lett hagyva `no-overwrite` importálása törlése.</span><span class="sxs-lookup"><span data-stu-id="f5e47-274">hello blob has been skipped per `no-overwrite` import disposition.</span></span>|  
|`Overwritten`|<span data-ttu-id="f5e47-275">hello blob felülírta egy meglévő blob / `overwrite` importálása törlése.</span><span class="sxs-lookup"><span data-stu-id="f5e47-275">hello blob has overwritten an existing blob per `overwrite` import disposition.</span></span>|  
|`Cancelled`|<span data-ttu-id="f5e47-276">Egy korábbi hiba hello importálási témakör további feldolgozása leállt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-276">A prior failure has stopped further processing of hello import disposition.</span></span>|  
  
## <a name="page-rangeblock-status-codes"></a><span data-ttu-id="f5e47-277">Tartomány/blokk állapotkódok lap</span><span class="sxs-lookup"><span data-stu-id="f5e47-277">Page range/block status codes</span></span>  
<span data-ttu-id="f5e47-278">hello következő táblázatban hello állapotkódok tartományt vagy egy tiltólista feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="f5e47-278">hello following table lists hello status codes for processing a page range or a block.</span></span>  
  
|<span data-ttu-id="f5e47-279">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="f5e47-279">Status code</span></span>|<span data-ttu-id="f5e47-280">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5e47-280">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="f5e47-281">hello lap tartomány vagy blokk feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-281">hello page range or block has finished processing without any errors.</span></span>|  
|`Committed`|<span data-ttu-id="f5e47-282">hello blokk véglegesítve, de nem a hello teljes tiltólista, mert más blokkok sikertelen, vagy helyezze magát teljes tiltólista sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-282">hello block has been committed,  but not in hello full block list because other blocks have failed, or put full block list itself has failed.</span></span>|  
|`Uncommitted`|<span data-ttu-id="f5e47-283">hello blokk feltölteni, de a nem véglegesített.</span><span class="sxs-lookup"><span data-stu-id="f5e47-283">hello block is uploaded but not committed.</span></span>|  
|`Corrupted`|<span data-ttu-id="f5e47-284">hello lap tartomány vagy blokk sérült (hello tartalom nem felel meg a kivonatoló).</span><span class="sxs-lookup"><span data-stu-id="f5e47-284">hello page range or block is corrupted (hello content does not match its hash).</span></span>|  
|`FileUnexpectedEnd`|<span data-ttu-id="f5e47-285">Egy nem várt fájlvégjel történt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-285">An unexpected end of file has been encountered.</span></span>|  
|`BlobUnexpectedEnd`|<span data-ttu-id="f5e47-286">Egy blob váratlan vége történt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-286">An unexpected end of blob has been encountered.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="f5e47-287">Blob szolgáltatási kérelem tooaccess hello laptartomány hello, vagy blokk nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="f5e47-287">hello Blob service request tooaccess hello page range or block has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="f5e47-288">A lemez- vagy hálózati i/o-hiba történt hello lap tartomány vagy blokk feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="f5e47-288">A disk or network I/O failure occurred while processing hello page range or block.</span></span>|  
|`Failed`|<span data-ttu-id="f5e47-289">Ismeretlen hiba történt a hello lap tartomány vagy blokk feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="f5e47-289">An unknown failure occurred while processing hello page range or block.</span></span>|  
|`Cancelled`|<span data-ttu-id="f5e47-290">Egy korábbi hiba hello lap tartomány vagy blokk további feldolgozás leállt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-290">A prior failure has stopped further processing of hello page range or block.</span></span>|  
  
## <a name="metadata-status-codes"></a><span data-ttu-id="f5e47-291">Metaadatok állapotkódok</span><span class="sxs-lookup"><span data-stu-id="f5e47-291">Metadata status codes</span></span>  
<span data-ttu-id="f5e47-292">hello következő táblázatban hello állapotkódjai feldolgozási blob metaadatait.</span><span class="sxs-lookup"><span data-stu-id="f5e47-292">hello following table lists hello status codes for processing blob metadata.</span></span>  
  
|<span data-ttu-id="f5e47-293">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="f5e47-293">Status code</span></span>|<span data-ttu-id="f5e47-294">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5e47-294">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="f5e47-295">hello metaadatok feldolgozási hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-295">hello metadata has finished processing without errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="f5e47-296">hello metaadat neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="f5e47-296">hello metadata file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="f5e47-297">hello metaadatok fájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="f5e47-297">hello metadata file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="f5e47-298">hello metaadatait tartalmazó fájl nem található.</span><span class="sxs-lookup"><span data-stu-id="f5e47-298">hello metadata file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="f5e47-299">Toohello metaadatfájl hozzáférés megtagadva.</span><span class="sxs-lookup"><span data-stu-id="f5e47-299">Access toohello metadata file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="f5e47-300">hello metaadatait tartalmazó fájl sérült (hello tartalom nem felel meg a kivonatoló).</span><span class="sxs-lookup"><span data-stu-id="f5e47-300">hello metadata file is corrupted (hello content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="f5e47-301">hello metaadatok tartalom nem felel meg a toohello megkívánt formátumban.</span><span class="sxs-lookup"><span data-stu-id="f5e47-301">hello metadata content does not conform toohello required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="f5e47-302">Írás hello metaadatok XML sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-302">Writing hello metadata XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="f5e47-303">tooaccess hello metaadatok hello Blob szolgáltatási kérelem sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-303">hello Blob service request tooaccess hello metadata has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="f5e47-304">Lemez- vagy i/o-hiba történt a hello metaadatok feldolgozásakor a program.</span><span class="sxs-lookup"><span data-stu-id="f5e47-304">A disk or network I/O failure occurred while processing hello metadata.</span></span>|  
|`Failed`|<span data-ttu-id="f5e47-305">Ismeretlen hiba történt a hello metaadatok feldolgozásakor a program.</span><span class="sxs-lookup"><span data-stu-id="f5e47-305">An unknown failure occurred while processing hello metadata.</span></span>|  
|`Cancelled`|<span data-ttu-id="f5e47-306">Egy korábbi hiba hello metaadatok további feldolgozás leállt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-306">A prior failure has stopped further processing of hello metadata.</span></span>|  
  
## <a name="properties-status-codes"></a><span data-ttu-id="f5e47-307">Tulajdonságok állapotkódok</span><span class="sxs-lookup"><span data-stu-id="f5e47-307">Properties status codes</span></span>  
<span data-ttu-id="f5e47-308">hello következő táblázatban hello állapotkódjai blob tulajdonságok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="f5e47-308">hello following table lists hello status codes for processing blob properties.</span></span>  
  
|<span data-ttu-id="f5e47-309">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="f5e47-309">Status code</span></span>|<span data-ttu-id="f5e47-310">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5e47-310">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="f5e47-311">hello tulajdonságok feldolgozása hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5e47-311">hello properties have finished processing without any errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="f5e47-312">hello tulajdonságok neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="f5e47-312">hello properties file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="f5e47-313">hello tulajdonságok fájl neve túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="f5e47-313">hello properties file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="f5e47-314">hello tulajdonságok fájl nem található.</span><span class="sxs-lookup"><span data-stu-id="f5e47-314">hello properties file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="f5e47-315">Toohello tulajdonságok fájl elérése megtagadva.</span><span class="sxs-lookup"><span data-stu-id="f5e47-315">Access toohello properties file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="f5e47-316">hello tulajdonságok fájl sérült (hello tartalom nem felel meg a kivonatoló).</span><span class="sxs-lookup"><span data-stu-id="f5e47-316">hello properties file is corrupted (hello content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="f5e47-317">hello tulajdonságok tartalom nem felel meg a toohello megkívánt formátumban.</span><span class="sxs-lookup"><span data-stu-id="f5e47-317">hello properties content does not conform toohello required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="f5e47-318">A tulajdonságoknak a hello XML sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-318">Writing hello properties XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="f5e47-319">tooaccess hello tulajdonságok hello Blob szolgáltatási kérelem sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-319">hello Blob service request tooaccess hello properties has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="f5e47-320">A lemez- vagy hálózati i/o-hiba történt a hello tulajdonságok feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="f5e47-320">A disk or network I/O failure occurred while processing hello properties.</span></span>|  
|`Failed`|<span data-ttu-id="f5e47-321">Ismeretlen hiba történt a hello tulajdonságok feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="f5e47-321">An unknown failure occurred while processing hello properties.</span></span>|  
|`Cancelled`|<span data-ttu-id="f5e47-322">Egy korábbi hiba hello tulajdonságok további feldolgozás leállt.</span><span class="sxs-lookup"><span data-stu-id="f5e47-322">A prior failure has stopped further processing of hello properties.</span></span>|  
  
## <a name="sample-logs"></a><span data-ttu-id="f5e47-323">A minta-naplók</span><span class="sxs-lookup"><span data-stu-id="f5e47-323">Sample logs</span></span>  
<span data-ttu-id="f5e47-324">hello az alábbiakban látható egy példa részletes naplózás.</span><span class="sxs-lookup"><span data-stu-id="f5e47-324">hello following is an example of verbose log.</span></span>  
  
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
  
<span data-ttu-id="f5e47-325">hello megfelelő hibanapló alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="f5e47-325">hello corresponding error log is shown below.</span></span>  
  
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

 <span data-ttu-id="f5e47-326">hello kövesse hibanaplóját, hogy az importálás hello importálási meghajtón nem található fájlokról hibát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f5e47-326">hello follow error log for an import job contains an error about a file not found on hello import drive.</span></span> <span data-ttu-id="f5e47-327">Vegye figyelembe, hogy további összetevők hello állapotának `Cancelled`.</span><span class="sxs-lookup"><span data-stu-id="f5e47-327">Note that hello status of subsequent components is `Cancelled`.</span></span>  
  
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

<span data-ttu-id="f5e47-328">hello exportálási feladat következő hibanaplóban azt jelzi, hogy hello blob tartalma sikeresen írt toohello meghajtó, azonban, hogy hiba történt a hello blob tulajdonságai exportálása során.</span><span class="sxs-lookup"><span data-stu-id="f5e47-328">hello following error log for an export job indicates that hello blob content has been successfully written toohello drive, but that an error occurred while exporting hello blob's properties.</span></span>  
  
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
  
## <a name="next-steps"></a><span data-ttu-id="f5e47-329">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5e47-329">Next steps</span></span>
 
* [<span data-ttu-id="f5e47-330">Storage Import/Export REST API felülete</span><span class="sxs-lookup"><span data-stu-id="f5e47-330">Storage Import/Export REST API</span></span>](/rest/api/storageimportexport/)
