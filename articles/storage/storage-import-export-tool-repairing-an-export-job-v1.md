---
title: "Javítása Azure Import/Export exportálási feladat - 1-es verzió |} Microsoft Docs"
description: "Megtudhatja, hogyan javítsa ki, hogy létrejött, de az Azure Import/Export szolgáltatás használatával futtassa exportálási feladat."
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
ms.openlocfilehash: 30ca0f8d06cb1927c19e66035ff485db0fc09e5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="repairing-an-export-job"></a><span data-ttu-id="55d69-103">Exportálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="55d69-103">Repairing an export job</span></span>
<span data-ttu-id="55d69-104">Miután exportálási feladat befejeződött, a Microsoft Azure Import/Export eszköz helyszíni is futtathatja:</span><span class="sxs-lookup"><span data-stu-id="55d69-104">After an export job has completed, you can run the Microsoft Azure Import/Export Tool on-premises to:</span></span>  
  
1.  <span data-ttu-id="55d69-105">Töltse le azokat a fájlokat, az Azure Import/Export szolgáltatás nem tudta exportálni.</span><span class="sxs-lookup"><span data-stu-id="55d69-105">Download any files that the Azure Import/Export service was unable to export.</span></span>  
  
2.  <span data-ttu-id="55d69-106">Ellenőrizze, hogy a meghajtón lévő fájlokat helyesen lettek-e exportálva.</span><span class="sxs-lookup"><span data-stu-id="55d69-106">Validate that the files on the drive were correctly exported.</span></span>  
  
<span data-ttu-id="55d69-107">A funkció használatához Azure-tárhellyel létesített kapcsolat kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="55d69-107">You must have connectivity to Azure Storage to use this functionality.</span></span>  
  
<span data-ttu-id="55d69-108">A parancs az importálási feladat javítási **RepairExport**.</span><span class="sxs-lookup"><span data-stu-id="55d69-108">The command for repairing an import job is **RepairExport**.</span></span>

## <a name="repairexport-parameters"></a><span data-ttu-id="55d69-109">RepairExport paraméterek</span><span class="sxs-lookup"><span data-stu-id="55d69-109">RepairExport parameters</span></span>

<span data-ttu-id="55d69-110">A következő paraméterekkel rendelkező adható meg **RepairExport**:</span><span class="sxs-lookup"><span data-stu-id="55d69-110">The following parameters can be specified with **RepairExport**:</span></span>  
  
|<span data-ttu-id="55d69-111">Paraméter</span><span class="sxs-lookup"><span data-stu-id="55d69-111">Parameter</span></span>|<span data-ttu-id="55d69-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="55d69-112">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="55d69-113">**r: < RepairFile\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-113">**/r:<RepairFile\>**</span></span>|<span data-ttu-id="55d69-114">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="55d69-114">Required.</span></span> <span data-ttu-id="55d69-115">A fájl elérési útját javítása, amely nyomon követi a folyamatot, a javítási, és lehetővé teszi az megszakított javítását folytatása.</span><span class="sxs-lookup"><span data-stu-id="55d69-115">Path to the repair file, which tracks the progress of the repair, and allows you to resume an interrupted repair.</span></span> <span data-ttu-id="55d69-116">Minden olyan meghajtó meg kell adni egy javítási fájlt.</span><span class="sxs-lookup"><span data-stu-id="55d69-116">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="55d69-117">Egy adott meghajtó javítás indításakor meg fogja továbbítani az elérési út egy javítási fájlt, amely még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="55d69-117">When you start a repair for a given drive, you will pass in the path to a repair file which does not yet exist.</span></span> <span data-ttu-id="55d69-118">Az megszakított javítását folytatásához meglévő javítási fájl nevében kell átadni.</span><span class="sxs-lookup"><span data-stu-id="55d69-118">To resume an interrupted repair, you should pass in the name of an existing repair file.</span></span> <span data-ttu-id="55d69-119">A célmeghajtó megfelelő javítási fájlt mindig meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="55d69-119">The repair file corresponding to the target drive must always be specified.</span></span>|  
|<span data-ttu-id="55d69-120">**/ logdir: < LogDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-120">**/logdir:<LogDirectory\>**</span></span>|<span data-ttu-id="55d69-121">Választható.</span><span class="sxs-lookup"><span data-stu-id="55d69-121">Optional.</span></span> <span data-ttu-id="55d69-122">A naplózási könyvtár.</span><span class="sxs-lookup"><span data-stu-id="55d69-122">The log directory.</span></span> <span data-ttu-id="55d69-123">Ez a könyvtár részletes naplófájlok fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="55d69-123">Verbose log files will be written to this directory.</span></span> <span data-ttu-id="55d69-124">Ha nincs naplókönyvtár meg van adva, a naplózási könyvtár az aktuális könyvtár lesz.</span><span class="sxs-lookup"><span data-stu-id="55d69-124">If no log directory is specified, the current directory will be used as the log directory.</span></span>|  
|<span data-ttu-id="55d69-125">**/ d: < TargetDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-125">**/d:<TargetDirectory\>**</span></span>|<span data-ttu-id="55d69-126">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="55d69-126">Required.</span></span> <span data-ttu-id="55d69-127">Ellenőrizze és javítsa ki a könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="55d69-127">The directory to validate and repair.</span></span> <span data-ttu-id="55d69-128">Ez általában az Exportálás meghajtó gyökérkönyvtárában, de sikerült is kell egy hálózati fájlmegosztásra tartalmazó az exportált fájlokat egy példányát.</span><span class="sxs-lookup"><span data-stu-id="55d69-128">This is usually the root directory of the export drive, but could also be a network file share containing a copy of the exported files.</span></span>|  
|<span data-ttu-id="55d69-129">**/BK: < BitLockerKey\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-129">**/bk:<BitLockerKey\>**</span></span>|<span data-ttu-id="55d69-130">Választható.</span><span class="sxs-lookup"><span data-stu-id="55d69-130">Optional.</span></span> <span data-ttu-id="55d69-131">A BitLocker kulcsot kell megadnia, ha azt szeretné, hogy az eszköz zárolásának feloldásához egy titkosított az exportált fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="55d69-131">You should specify the BitLocker key if you want the tool to unlock an encrypted where the exported files are stored.</span></span>|  
|<span data-ttu-id="55d69-132">**/sn: < StorageAccountName\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-132">**/sn:<StorageAccountName\>**</span></span>|<span data-ttu-id="55d69-133">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="55d69-133">Required.</span></span> <span data-ttu-id="55d69-134">Az exportálási feladat a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="55d69-134">The name of the storage account for the export job.</span></span>|  
|<span data-ttu-id="55d69-135">**/SK: < StorageAccountKey\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-135">**/sk:<StorageAccountKey\>**</span></span>|<span data-ttu-id="55d69-136">**Szükséges** csak, ha a tároló SAS nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="55d69-136">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="55d69-137">A fiók az exportálási feladat a tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="55d69-137">The account key for the storage account for the export job.</span></span>|  
|<span data-ttu-id="55d69-138">**/csas: < ContainerSas\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-138">**/csas:<ContainerSas\>**</span></span>|<span data-ttu-id="55d69-139">**Szükséges** csak, ha nincs megadva a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="55d69-139">**Required** if and only if the storage account key is not specified.</span></span> <span data-ttu-id="55d69-140">A tároló SAS az exportálási feladat társított BLOB eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="55d69-140">The container SAS for accessing the blobs associated with the export job.</span></span>|  
|<span data-ttu-id="55d69-141">**/ CopyLogFile: < DriveCopyLogFile\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-141">**/CopyLogFile:<DriveCopyLogFile\>**</span></span>|<span data-ttu-id="55d69-142">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="55d69-142">Required.</span></span> <span data-ttu-id="55d69-143">A naplófájl elérési útja a meghajtó másolása.</span><span class="sxs-lookup"><span data-stu-id="55d69-143">The path to the drive copy log file.</span></span> <span data-ttu-id="55d69-144">A fájlt a Windows Azure Import/Export szolgáltatás által generált és tölthető le: a feladathoz társított blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="55d69-144">The file is generated by the Windows Azure Import/Export service and can be downloaded from the blob storage associated with the job.</span></span> <span data-ttu-id="55d69-145">A napló fájl másolása sikertelen blobokkal vagy a fájlokat, amelyek javítani kell információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="55d69-145">The copy log file contains information about failed blobs or files which are to be repaired.</span></span>|  
|<span data-ttu-id="55d69-146">**/ ManifestFile: < DriveManifestFile\>**</span><span class="sxs-lookup"><span data-stu-id="55d69-146">**/ManifestFile:<DriveManifestFile\>**</span></span>|<span data-ttu-id="55d69-147">Választható.</span><span class="sxs-lookup"><span data-stu-id="55d69-147">Optional.</span></span> <span data-ttu-id="55d69-148">Az Exportálás meghajtó Alkalmazásjegyzék-fájl elérési útja.</span><span class="sxs-lookup"><span data-stu-id="55d69-148">The path to the export drive's manifest file.</span></span> <span data-ttu-id="55d69-149">Ezt a fájlt a Windows Azure Import/Export szolgáltatás által létrehozott és exportálás a meghajtón, és szükség esetén egy blobba a feladathoz társított tárfiókban tárolja.</span><span class="sxs-lookup"><span data-stu-id="55d69-149">This file is generated by the Windows Azure Import/Export service and stored on the export drive, and optionally in a blob in the storage account associated with the job.</span></span><br /><br /> <span data-ttu-id="55d69-150">Az Exportálás meghajtón a fájlok tartalmának fogja ellenőrizni a ebben a fájlban található MD5-kivonatok.</span><span class="sxs-lookup"><span data-stu-id="55d69-150">The content of the files on the export drive will be verified with the MD5 hashes contained in this file.</span></span> <span data-ttu-id="55d69-151">Azokat a fájlokat, sérült határozza meg a rendszer letölti és a cél könyvtárak írni.</span><span class="sxs-lookup"><span data-stu-id="55d69-151">Any files that are determined to be corrupted will be downloaded and rewritten to the target directories.</span></span>|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a><span data-ttu-id="55d69-152">Javítsa ki a hibás kivitel RepairExport mód használata</span><span class="sxs-lookup"><span data-stu-id="55d69-152">Using RepairExport mode to correct failed exports</span></span>  
<span data-ttu-id="55d69-153">Az Azure Import/Export eszköz segítségével, amely nem sikerült exportálni a fájlok letöltése.</span><span class="sxs-lookup"><span data-stu-id="55d69-153">You can use the Azure Import/Export Tool to download files that failed to export.</span></span> <span data-ttu-id="55d69-154">A másolási naplófájlt fogja tartalmazni, amely nem sikerült exportálni a fájlok listáját.</span><span class="sxs-lookup"><span data-stu-id="55d69-154">The copy log file will contain a list of files that failed to export.</span></span>  
  
<span data-ttu-id="55d69-155">Exportálási hiba okai a következő lehetőségek állnak rendelkezésére:</span><span class="sxs-lookup"><span data-stu-id="55d69-155">The causes of export failures include the following possibilities:</span></span>  
  
-   <span data-ttu-id="55d69-156">Sérült meghajtók</span><span class="sxs-lookup"><span data-stu-id="55d69-156">Damaged drives</span></span>  
  
-   <span data-ttu-id="55d69-157">A tárfiók hívóbetűjét módosult az átvitel során</span><span class="sxs-lookup"><span data-stu-id="55d69-157">The storage account key changed during the transfer process</span></span>  
  
<span data-ttu-id="55d69-158">Az eszköz futtatásához **RepairExport** mód, először a meghajtót, amely tartalmazza az exportált fájlokat a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="55d69-158">To run the tool in **RepairExport** mode, you first need to connect the drive containing the exported files to your computer.</span></span> <span data-ttu-id="55d69-159">Ezután futtassa az Azure Import/Export eszköz, az elérési út megadását, hogy a meghajtót, amelyen a `/d` paraméter.</span><span class="sxs-lookup"><span data-stu-id="55d69-159">Next, run the Azure Import/Export Tool, specifying the path to that drive with the `/d` parameter.</span></span> <span data-ttu-id="55d69-160">Meg kell adnia a naplófájl elérési útja a meghajtó másolása letöltött is.</span><span class="sxs-lookup"><span data-stu-id="55d69-160">You also need to specify the path to the drive's copy log file that you downloaded.</span></span> <span data-ttu-id="55d69-161">A következő parancsot az alábbi példa használatával javítsa ki azokat a fájlokat, nem sikerült exportálni az eszköz fut:</span><span class="sxs-lookup"><span data-stu-id="55d69-161">The following command line example below runs the tool to repair any files that failed to export:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
<span data-ttu-id="55d69-162">A következő egy példa bemutatja, hogy egy blokk a BLOB másolási naplófájlt nem tudott exportálni:</span><span class="sxs-lookup"><span data-stu-id="55d69-162">The following is an example of a copy log file that shows that one block in the blob failed to export:</span></span>  
  
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
  
<span data-ttu-id="55d69-163">A másolási naplófájl azt jelzi, hogy hiba történt a Windows Azure Import/Export szolgáltatás lett letöltését a blob blokkok egyikét a fájlt az exportálási meghajtón.</span><span class="sxs-lookup"><span data-stu-id="55d69-163">The copy log file indicates that a failure occurred while the Windows Azure Import/Export service was downloading one of the blob's blocks to the file on the export drive.</span></span> <span data-ttu-id="55d69-164">A többi összetevő a fájl letöltése sikeresen befejeződött, és a fájl hosszát helyesen be lett állítva.</span><span class="sxs-lookup"><span data-stu-id="55d69-164">The other components of the file downloaded successfully, and the file length was correctly set.</span></span> <span data-ttu-id="55d69-165">Ebben az esetben az eszköz nyissa meg a fájlt a meghajtón, a blokk letöltését a tárfiók, és írja a 65536 értékű eltolás hosszúságú 65536-től kezdődő fájl tartománynak.</span><span class="sxs-lookup"><span data-stu-id="55d69-165">In this case, the tool will open the file on the drive, download the block from the storage account, and write it to the file range starting from offset 65536 with length 65536.</span></span>  
  
## <a name="using-repairexport-to-validate-drive-contents"></a><span data-ttu-id="55d69-166">RepairExport segítségével ellenőrzi a meghajtó tartalmát</span><span class="sxs-lookup"><span data-stu-id="55d69-166">Using RepairExport to validate drive contents</span></span>  
<span data-ttu-id="55d69-167">Használhatja az Azure Import/Export a **RepairExport** lehetőséget a meghajtón a tartalmát érvényesíteni helyesek.</span><span class="sxs-lookup"><span data-stu-id="55d69-167">You can also use Azure Import/Export with the **RepairExport** option to validate the contents on the drive are correct.</span></span> <span data-ttu-id="55d69-168">A jegyzékfájl minden exportálási meghajtón MD5s a meghajtó tartalmát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="55d69-168">The manifest file on each export drive contains MD5s for the contents of the drive.</span></span>  
  
<span data-ttu-id="55d69-169">Az Azure Import/Export szolgáltatás is mentheti a jegyzékfájlt tárfiókba az exportálási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="55d69-169">The Azure Import/Export service can also save the manifest files to a storage account during the export process.</span></span> <span data-ttu-id="55d69-170">Fájlok helye keresztül érhető el a [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet, amikor a feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="55d69-170">The location of the manifest files is available via the [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation when the job has completed.</span></span> <span data-ttu-id="55d69-171">Lásd: [Import/Export service Manifest fájlformátum](storage-import-export-file-format-metadata-and-properties.md) meghajtó jegyzékfájl formátuma további információt.</span><span class="sxs-lookup"><span data-stu-id="55d69-171">See [Import/Export service Manifest File Format](storage-import-export-file-format-metadata-and-properties.md) for more information about the format of a drive manifest file.</span></span>  
  
<span data-ttu-id="55d69-172">A következő példa bemutatja, hogyan együtt az Azure Import/Export eszköz futtatásához a **/ManifestFile** és **/CopyLogFile** paraméterek:</span><span class="sxs-lookup"><span data-stu-id="55d69-172">The following example shows how to run the Azure Import/Export Tool with the **/ManifestFile** and **/CopyLogFile** parameters:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
<span data-ttu-id="55d69-173">A következő egy példa a jegyzékfájlt:</span><span class="sxs-lookup"><span data-stu-id="55d69-173">The following is an example of a manifest file:</span></span>  
  
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
  
<span data-ttu-id="55d69-174">A javítási folyamat végén, az eszköz olvassa végig a jegyzékfájl hivatkozott minden fájl és a fájl integritását a MD5 kivonatok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="55d69-174">After finishing the repair process, the tool will read through each file referenced in the manifest file and verify the file's integrity with the MD5 hashes.</span></span> <span data-ttu-id="55d69-175">A jegyzékfájl felett, a hibaállapotba kerül, a következő összetevők keresztül.</span><span class="sxs-lookup"><span data-stu-id="55d69-175">For the manifest above, it will go through the following components.</span></span>  

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

<span data-ttu-id="55d69-176">Az ellenőrzés sikertelen összetevők közül bármelyik a eszköz által letöltődik és írni a meghajtón ugyanazt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="55d69-176">Any component failing the verification will be downloaded by the tool and rewritten to the same file on the drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="55d69-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="55d69-177">Next steps</span></span>
 
* [<span data-ttu-id="55d69-178">Az Azure Import/Export eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="55d69-178">Setting Up the Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="55d69-179">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="55d69-179">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="55d69-180">Feladatok állapotának áttekintése a másolási naplófájlok segítségével</span><span class="sxs-lookup"><span data-stu-id="55d69-180">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="55d69-181">Importálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="55d69-181">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="55d69-182">Az Azure Import/Export eszköz hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="55d69-182">Troubleshooting the Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
