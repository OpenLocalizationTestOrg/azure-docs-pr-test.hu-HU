---
title: "aaaRepairing Azure Import/Export exportálási feladat - 1-es verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toorepair lett létrehozva, és futtassa az exportálási feladatot hello Azure Import/Export szolgáltatás."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 728e2a42-04ce-4be8-9375-e9e2bc6827a5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 96c674fc7c697c37882fb2980c340303896ac6c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-export-job"></a><span data-ttu-id="82fd9-103">Exportálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="82fd9-103">Repairing an export job</span></span>
<span data-ttu-id="82fd9-104">Exportálási feladat befejezése után a Microsoft Azure Import/Export eszköz helyszíni hello is futtathatja:</span><span class="sxs-lookup"><span data-stu-id="82fd9-104">After an export job has completed, you can run hello Microsoft Azure Import/Export Tool on-premises to:</span></span>  
  
1.  <span data-ttu-id="82fd9-105">Töltse le a fájlokat, hogy hello Azure Import/Export szolgáltatás nem tooexport volt-e.</span><span class="sxs-lookup"><span data-stu-id="82fd9-105">Download any files that hello Azure Import/Export service was unable tooexport.</span></span>  
  
2.  <span data-ttu-id="82fd9-106">Ellenőrizze, hogy helyesen lettek-e exportálva hello hello meghajtón.</span><span class="sxs-lookup"><span data-stu-id="82fd9-106">Validate that hello files on hello drive were correctly exported.</span></span>  
  
<span data-ttu-id="82fd9-107">Ez a funkció kapcsolat tooAzure tárolási toouse kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="82fd9-107">You must have connectivity tooAzure Storage toouse this functionality.</span></span>  
  
<span data-ttu-id="82fd9-108">az importálási feladat javítási hello parancs **RepairExport**.</span><span class="sxs-lookup"><span data-stu-id="82fd9-108">hello command for repairing an import job is **RepairExport**.</span></span>

## <a name="repairexport-parameters"></a><span data-ttu-id="82fd9-109">RepairExport paraméterek</span><span class="sxs-lookup"><span data-stu-id="82fd9-109">RepairExport parameters</span></span>

<span data-ttu-id="82fd9-110">hello következő paraméterek adható **RepairExport**:</span><span class="sxs-lookup"><span data-stu-id="82fd9-110">hello following parameters can be specified with **RepairExport**:</span></span>  
  
|<span data-ttu-id="82fd9-111">Paraméter</span><span class="sxs-lookup"><span data-stu-id="82fd9-111">Parameter</span></span>|<span data-ttu-id="82fd9-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="82fd9-112">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="82fd9-113">**r: < RepairFile\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-113">**/r:<RepairFile\>**</span></span>|<span data-ttu-id="82fd9-114">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="82fd9-114">Required.</span></span> <span data-ttu-id="82fd9-115">Elérési út toohello javítási fájl, amely hello javítási hello előrehaladását követi nyomon, és lehetővé teszi a tooresume az megszakított javítását.</span><span class="sxs-lookup"><span data-stu-id="82fd9-115">Path toohello repair file, which tracks hello progress of hello repair, and allows you tooresume an interrupted repair.</span></span> <span data-ttu-id="82fd9-116">Minden olyan meghajtó meg kell adni egy javítási fájlt.</span><span class="sxs-lookup"><span data-stu-id="82fd9-116">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="82fd9-117">Amikor elindít egy adott meghajtó javítása, át lesz fájlban hello elérési tooa javítás még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="82fd9-117">When you start a repair for a given drive, you will pass in hello path tooa repair file which does not yet exist.</span></span> <span data-ttu-id="82fd9-118">tooresume az megszakított javítását, meg kell adjon át egy meglévő javítási fájl hello neve.</span><span class="sxs-lookup"><span data-stu-id="82fd9-118">tooresume an interrupted repair, you should pass in hello name of an existing repair file.</span></span> <span data-ttu-id="82fd9-119">hello javítási fájl megfelelő toohello célmeghajtó mindig meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="82fd9-119">hello repair file corresponding toohello target drive must always be specified.</span></span>|  
|<span data-ttu-id="82fd9-120">**/ logdir: < LogDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-120">**/logdir:<LogDirectory\>**</span></span>|<span data-ttu-id="82fd9-121">Választható.</span><span class="sxs-lookup"><span data-stu-id="82fd9-121">Optional.</span></span> <span data-ttu-id="82fd9-122">hello naplókönyvtár.</span><span class="sxs-lookup"><span data-stu-id="82fd9-122">hello log directory.</span></span> <span data-ttu-id="82fd9-123">Részletes naplófájlok toothis directory lesz írva.</span><span class="sxs-lookup"><span data-stu-id="82fd9-123">Verbose log files will be written toothis directory.</span></span> <span data-ttu-id="82fd9-124">Ha nincs naplókönyvtár meg van adva, hello aktuális könyvtárhoz hello naplókönyvtár lesz.</span><span class="sxs-lookup"><span data-stu-id="82fd9-124">If no log directory is specified, hello current directory will be used as hello log directory.</span></span>|  
|<span data-ttu-id="82fd9-125">**/ d: < TargetDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-125">**/d:<TargetDirectory\>**</span></span>|<span data-ttu-id="82fd9-126">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="82fd9-126">Required.</span></span> <span data-ttu-id="82fd9-127">hello directory toovalidate és javítása.</span><span class="sxs-lookup"><span data-stu-id="82fd9-127">hello directory toovalidate and repair.</span></span> <span data-ttu-id="82fd9-128">Ez általában hello hello exportálási meghajtó gyökérkönyvtárába, de sikerült is kell egy hálózati fájlmegosztásra tartalmazó hello exportált fájlokat egy példányát.</span><span class="sxs-lookup"><span data-stu-id="82fd9-128">This is usually hello root directory of hello export drive, but could also be a network file share containing a copy of hello exported files.</span></span>|  
|<span data-ttu-id="82fd9-129">**/BK: < BitLockerKey\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-129">**/bk:<BitLockerKey\>**</span></span>|<span data-ttu-id="82fd9-130">Választható.</span><span class="sxs-lookup"><span data-stu-id="82fd9-130">Optional.</span></span> <span data-ttu-id="82fd9-131">Hello BitLocker kulcsot kell megadnia, ha azt szeretné, hogy hello eszköz toounlock egy titkosított amikor hello exportált fájlokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="82fd9-131">You should specify hello BitLocker key if you want hello tool toounlock an encrypted where hello exported files are stored.</span></span>|  
|<span data-ttu-id="82fd9-132">**/sn: < StorageAccountName\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-132">**/sn:<StorageAccountName\>**</span></span>|<span data-ttu-id="82fd9-133">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="82fd9-133">Required.</span></span> <span data-ttu-id="82fd9-134">hello hello hello storage-fiók nevét a feladat exportálja.</span><span class="sxs-lookup"><span data-stu-id="82fd9-134">hello name of hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="82fd9-135">**/SK: < StorageAccountKey\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-135">**/sk:<StorageAccountKey\>**</span></span>|<span data-ttu-id="82fd9-136">**Szükséges** csak, ha a tároló SAS nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="82fd9-136">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="82fd9-137">hello fiók kulcsának hello hello a feladat exportálja.</span><span class="sxs-lookup"><span data-stu-id="82fd9-137">hello account key for hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="82fd9-138">**/csas: < ContainerSas\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-138">**/csas:<ContainerSas\>**</span></span>|<span data-ttu-id="82fd9-139">**Szükséges** csak, ha nincs megadva hello tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="82fd9-139">**Required** if and only if hello storage account key is not specified.</span></span> <span data-ttu-id="82fd9-140">hello tároló SAS hello exportálási feladat társított hello blobok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="82fd9-140">hello container SAS for accessing hello blobs associated with hello export job.</span></span>|  
|<span data-ttu-id="82fd9-141">**/ CopyLogFile: < DriveCopyLogFile\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-141">**/CopyLogFile:<DriveCopyLogFile\>**</span></span>|<span data-ttu-id="82fd9-142">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="82fd9-142">Required.</span></span> <span data-ttu-id="82fd9-143">hello elérési toohello meghajtó másolása naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="82fd9-143">hello path toohello drive copy log file.</span></span> <span data-ttu-id="82fd9-144">hello fájl hello Windows Azure Import/Export szolgáltatás által generált és tölthető le: hello hello feladattal társított blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="82fd9-144">hello file is generated by hello Windows Azure Import/Export service and can be downloaded from hello blob storage associated with hello job.</span></span> <span data-ttu-id="82fd9-145">hello napló-fájl másolása sikertelen blobokkal vagy a fájlokat, amelyek javítani toobe információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="82fd9-145">hello copy log file contains information about failed blobs or files which are toobe repaired.</span></span>|  
|<span data-ttu-id="82fd9-146">**/ ManifestFile: < DriveManifestFile\>**</span><span class="sxs-lookup"><span data-stu-id="82fd9-146">**/ManifestFile:<DriveManifestFile\>**</span></span>|<span data-ttu-id="82fd9-147">Választható.</span><span class="sxs-lookup"><span data-stu-id="82fd9-147">Optional.</span></span> <span data-ttu-id="82fd9-148">elérési út toohello exportálási meghajtó jegyzékfájl hello.</span><span class="sxs-lookup"><span data-stu-id="82fd9-148">hello path toohello export drive's manifest file.</span></span> <span data-ttu-id="82fd9-149">Ez a fájl hello Windows Azure Import/Export szolgáltatás által létrehozott és hello exportálási meghajtón, és szükség esetén egy blobba hello feladattal társított hello tárfiókban tárolja.</span><span class="sxs-lookup"><span data-stu-id="82fd9-149">This file is generated by hello Windows Azure Import/Export service and stored on hello export drive, and optionally in a blob in hello storage account associated with hello job.</span></span><br /><br /> <span data-ttu-id="82fd9-150">hello hello exportálási meghajtón lévő fájlokat a rendszer ellenőrzi az ebben a fájlban található hello MD5 kivonatok hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="82fd9-150">hello content of hello files on hello export drive will be verified with hello MD5 hashes contained in this file.</span></span> <span data-ttu-id="82fd9-151">Sérült meghatározott toobe fájlokon letöltött és egy átírt toohello cél könyvtárak lesz.</span><span class="sxs-lookup"><span data-stu-id="82fd9-151">Any files that are determined toobe corrupted will be downloaded and rewritten toohello target directories.</span></span>|  
  
## <a name="using-repairexport-mode-toocorrect-failed-exports"></a><span data-ttu-id="82fd9-152">RepairExport használatával mód toocorrect kivitel sikertelen</span><span class="sxs-lookup"><span data-stu-id="82fd9-152">Using RepairExport mode toocorrect failed exports</span></span>  
<span data-ttu-id="82fd9-153">Nem sikerült tooexport hello Azure Import/Export eszköz toodownload fájlok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="82fd9-153">You can use hello Azure Import/Export Tool toodownload files that failed tooexport.</span></span> <span data-ttu-id="82fd9-154">hello másolási naplófájlt fogja tartalmazni, melyeknél nem sikerült tooexport fájlok listáját.</span><span class="sxs-lookup"><span data-stu-id="82fd9-154">hello copy log file will contain a list of files that failed tooexport.</span></span>  
  
<span data-ttu-id="82fd9-155">hello exportálási hiba okai hello a következő lehetőségeket:</span><span class="sxs-lookup"><span data-stu-id="82fd9-155">hello causes of export failures include hello following possibilities:</span></span>  
  
-   <span data-ttu-id="82fd9-156">Sérült meghajtók</span><span class="sxs-lookup"><span data-stu-id="82fd9-156">Damaged drives</span></span>  
  
-   <span data-ttu-id="82fd9-157">tárfiók kulcsa hello hello átviteli folyamat során megváltozott</span><span class="sxs-lookup"><span data-stu-id="82fd9-157">hello storage account key changed during hello transfer process</span></span>  
  
<span data-ttu-id="82fd9-158">toorun hello eszköz **RepairExport** mód, először tooconnect hello meghajtóján hello exportált fájlokat tooyour számítógép.</span><span class="sxs-lookup"><span data-stu-id="82fd9-158">toorun hello tool in **RepairExport** mode, you first need tooconnect hello drive containing hello exported files tooyour computer.</span></span> <span data-ttu-id="82fd9-159">Ezután futtassa az Azure Import/Export eszköz hello elérési toothat meghajtó megadása a hello hello `/d` paraméter.</span><span class="sxs-lookup"><span data-stu-id="82fd9-159">Next, run hello Azure Import/Export Tool, specifying hello path toothat drive with hello `/d` parameter.</span></span> <span data-ttu-id="82fd9-160">Meg kell toospecify hello elérési toohello meghajtó másolása naplófájl letöltött is.</span><span class="sxs-lookup"><span data-stu-id="82fd9-160">You also need toospecify hello path toohello drive's copy log file that you downloaded.</span></span> <span data-ttu-id="82fd9-161">hello következő parancssor például az alábbi futtató hello eszköz toorepair azokat a fájlokat, tooexport sikertelen:</span><span class="sxs-lookup"><span data-stu-id="82fd9-161">hello following command line example below runs hello tool toorepair any files that failed tooexport:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
<span data-ttu-id="82fd9-162">hello következő másolási naplófájl jeleníti meg, hogy egy blokk hello meghiúsult tooexport példája:</span><span class="sxs-lookup"><span data-stu-id="82fd9-162">hello following is an example of a copy log file that shows that one block in hello blob failed tooexport:</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
<span data-ttu-id="82fd9-163">hello másolási naplófájl azt jelzi, hogy hiba történt a Windows Azure Import/Export szolgáltatás hello lett letöltése egyik hello blob blokkok toohello hello exportálási meghajtón.</span><span class="sxs-lookup"><span data-stu-id="82fd9-163">hello copy log file indicates that a failure occurred while hello Windows Azure Import/Export service was downloading one of hello blob's blocks toohello file on hello export drive.</span></span> <span data-ttu-id="82fd9-164">hello más összetevők hello fájl letöltése sikeresen befejeződött, és hello fájl hosszát helyesen be lett állítva.</span><span class="sxs-lookup"><span data-stu-id="82fd9-164">hello other components of hello file downloaded successfully, and hello file length was correctly set.</span></span> <span data-ttu-id="82fd9-165">Ebben az esetben hello eszköz hello meghajtón hello fájl megnyitása, letölthesse hello blokk hello tárfiókból, és írja a 65536 értékű eltolás hosszúságú 65536-től kezdődő toohello fájl tartományon.</span><span class="sxs-lookup"><span data-stu-id="82fd9-165">In this case, hello tool will open hello file on hello drive, download hello block from hello storage account, and write it toohello file range starting from offset 65536 with length 65536.</span></span>  
  
## <a name="using-repairexport-toovalidate-drive-contents"></a><span data-ttu-id="82fd9-166">RepairExport toovalidate meghajtó-tartalmak segítségével</span><span class="sxs-lookup"><span data-stu-id="82fd9-166">Using RepairExport toovalidate drive contents</span></span>  
<span data-ttu-id="82fd9-167">Azure Import/Export is használható hello **RepairExport** beállítás toovalidate hello tartalma hello meghajtón helyesek.</span><span class="sxs-lookup"><span data-stu-id="82fd9-167">You can also use Azure Import/Export with hello **RepairExport** option toovalidate hello contents on hello drive are correct.</span></span> <span data-ttu-id="82fd9-168">minden exportálási meghajtón hello jegyzékfájl MD5s hello meghajtó hello tartalmát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="82fd9-168">hello manifest file on each export drive contains MD5s for hello contents of hello drive.</span></span>  
  
<span data-ttu-id="82fd9-169">hello Azure Import/Export szolgáltatás is mentheti hello jegyzékfájlt tooa tárfiók hello exportálási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="82fd9-169">hello Azure Import/Export service can also save hello manifest files tooa storage account during hello export process.</span></span> <span data-ttu-id="82fd9-170">hello hello fájlok helye elérhető hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet, amikor a hello feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="82fd9-170">hello location of hello manifest files is available via hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation when hello job has completed.</span></span> <span data-ttu-id="82fd9-171">Lásd: [Import/Export service Manifest fájlformátum](storage-import-export-file-format-metadata-and-properties.md) meghajtó jegyzékfájl hello formátumban olvashat.</span><span class="sxs-lookup"><span data-stu-id="82fd9-171">See [Import/Export service Manifest File Format](storage-import-export-file-format-metadata-and-properties.md) for more information about hello format of a drive manifest file.</span></span>  
  
<span data-ttu-id="82fd9-172">hello következő példa bemutatja, hogyan toorun hello-Azure Import/Export eszköz hello **/ManifestFile** és **/CopyLogFile** paraméterek:</span><span class="sxs-lookup"><span data-stu-id="82fd9-172">hello following example shows how toorun hello Azure Import/Export Tool with hello **/ManifestFile** and **/CopyLogFile** parameters:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
<span data-ttu-id="82fd9-173">hello az alábbiakban látható egy példa a jegyzékfájlt:</span><span class="sxs-lookup"><span data-stu-id="82fd9-173">hello following is an example of a manifest file:</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
<span data-ttu-id="82fd9-174">Hello javítási folyamat befejeződik, miután hello eszköz olvassa végig a minden fájl hello jegyzékfájl hivatkozik, és ellenőrizze a hello MD5 kivonatok hello fájl integritását.</span><span class="sxs-lookup"><span data-stu-id="82fd9-174">After finishing hello repair process, hello tool will read through each file referenced in hello manifest file and verify hello file's integrity with hello MD5 hashes.</span></span> <span data-ttu-id="82fd9-175">A fenti hello jegyzék hibaállapotba kerül, a következő összetevők hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="82fd9-175">For hello manifest above, it will go through hello following components.</span></span>  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

<span data-ttu-id="82fd9-176">Hello ellenőrzése sikertelen összetevők közül bármelyik hello eszköz letöltődik és írni toohello ugyanazon fájl hello meghajtón.</span><span class="sxs-lookup"><span data-stu-id="82fd9-176">Any component failing hello verification will be downloaded by hello tool and rewritten toohello same file on hello drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="82fd9-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82fd9-177">Next steps</span></span>
 
* [<span data-ttu-id="82fd9-178">Hello Azure Import/Export eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="82fd9-178">Setting Up hello Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="82fd9-179">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="82fd9-179">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="82fd9-180">Feladatok állapotának áttekintése a másolási naplófájlok segítségével</span><span class="sxs-lookup"><span data-stu-id="82fd9-180">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="82fd9-181">Importálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="82fd9-181">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="82fd9-182">Hibaelhárítási hello Azure Import/Export eszköz</span><span class="sxs-lookup"><span data-stu-id="82fd9-182">Troubleshooting hello Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
