---
title: "Az Azure Import/Export importálási feladat létrehozása |} Microsoft Docs"
description: "Útmutató a Microsoft Azure Import/Export szolgáltatás importálás létrehozásához."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: d373d2a0e601f2796719fc5efb8761f276ab24d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="creating-an-import-job-for-the-azure-importexport-service"></a><span data-ttu-id="b307e-103">Az importálási feladat az Azure Import/Export szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b307e-103">Creating an import job for the Azure Import/Export service</span></span>

<span data-ttu-id="b307e-104">A Microsoft Azure Import/Export szolgáltatás REST API használatával az importálási feladat létrehozása a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="b307e-104">Creating an import job for the Microsoft Azure Import/Export service using the REST API involves the following steps:</span></span>

-   <span data-ttu-id="b307e-105">Felkészülés az Azure Import/Export eszköz rendelkező meghajtókat.</span><span class="sxs-lookup"><span data-stu-id="b307e-105">Preparing drives with the Azure Import/Export Tool.</span></span>

-   <span data-ttu-id="b307e-106">A helyet, ahová a meghajtó szállítási megszerezni.</span><span class="sxs-lookup"><span data-stu-id="b307e-106">Obtaining the location to which to ship the drive.</span></span>

-   <span data-ttu-id="b307e-107">Az importálási feladat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b307e-107">Creating the import job.</span></span>

-   <span data-ttu-id="b307e-108">A szállítási a Microsoftnak a meghajtók támogatott szolgáltatónként szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="b307e-108">Shipping the drives to Microsoft via a supported carrier service.</span></span>

-   <span data-ttu-id="b307e-109">Az importálási feladat frissítése a szállítási adatokkal.</span><span class="sxs-lookup"><span data-stu-id="b307e-109">Updating the import job with the shipping details.</span></span>

 <span data-ttu-id="b307e-110">Lásd: [adatok átviteléhez a Blob Storage a Microsoft Azure Import/Export szolgáltatás használata](storage-import-export-service.md) áttekintését a Import/Export szolgáltatás és egy az oktatóanyag bemutatja, hogyan használható a [Azure-portálon](https://portal.azure.com/) létrehozása Importálás kezeléséhez, és exportálni a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b307e-110">See [Using the Microsoft Azure Import/Export service to Transfer Data to Blob Storage](storage-import-export-service.md) for an overview of the Import/Export service and a tutorial that demonstrates how to use the [Azure  portal](https://portal.azure.com/) to create and manage import and export jobs.</span></span>

## <a name="preparing-drives-with-the-azure-importexport-tool"></a><span data-ttu-id="b307e-111">Az Azure Import/Export eszközzel meghajtó előkészítése</span><span class="sxs-lookup"><span data-stu-id="b307e-111">Preparing drives with the Azure Import/Export Tool</span></span>

<span data-ttu-id="b307e-112">A meghajtók előkészítése az importálási feladat lépésekre azonos e hoz létre a jobvia a portálon, vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="b307e-112">The steps to prepare drives for an import job are the same whether you create the jobvia the portal or via the REST API.</span></span>

<span data-ttu-id="b307e-113">Az alábbiakban van meghajtó előkészítése rövid áttekintést.</span><span class="sxs-lookup"><span data-stu-id="b307e-113">Below is a brief overview of drive preparation.</span></span> <span data-ttu-id="b307e-114">Tekintse meg a [Azure Import-ExportTool hivatkozás](storage-import-export-tool-how-to-v1.md) részletes utasításokért.</span><span class="sxs-lookup"><span data-stu-id="b307e-114">Refer to the [Azure Import-ExportTool Reference](storage-import-export-tool-how-to-v1.md) for complete instructions.</span></span> <span data-ttu-id="b307e-115">Az Azure Import/Export eszközt letöltheti [Itt](http://go.microsoft.com/fwlink/?LinkID=301900).</span><span class="sxs-lookup"><span data-stu-id="b307e-115">You can download the Azure Import/Export Tool [here](http://go.microsoft.com/fwlink/?LinkID=301900).</span></span>

<span data-ttu-id="b307e-116">A meghajtó előkészítése foglal magában:</span><span class="sxs-lookup"><span data-stu-id="b307e-116">Preparing your drive involves:</span></span>

-   <span data-ttu-id="b307e-117">Az adatok, importálandók azonosítása.</span><span class="sxs-lookup"><span data-stu-id="b307e-117">Identifying the data to be imported.</span></span>

-   <span data-ttu-id="b307e-118">A célként megadott blobot, amely a Windows Azure Storage azonosítása.</span><span class="sxs-lookup"><span data-stu-id="b307e-118">Identifying the destination blobs in Windows Azure Storage.</span></span>

-   <span data-ttu-id="b307e-119">Az Azure Import/Export eszköz használatával másolja az adatokat legalább egy merevlemez-meghajtókat.</span><span class="sxs-lookup"><span data-stu-id="b307e-119">Using the Azure Import/Export Tool to copy your data to one or more hard drives.</span></span>

 <span data-ttu-id="b307e-120">Az Azure Import/Export eszköz is létrehoz a jegyzékfájlt a meghajtókhoz előkészítettként van.</span><span class="sxs-lookup"><span data-stu-id="b307e-120">The Azure Import/Export Tool will also generate a manifest file for each of the drives as it is prepared.</span></span> <span data-ttu-id="b307e-121">A jegyzékfájl tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b307e-121">A manifest file contains:</span></span>

-   <span data-ttu-id="b307e-122">Egy feltöltési és a leképezései ezeket a fájlokat a blobok szánt fájlok felsorolása.</span><span class="sxs-lookup"><span data-stu-id="b307e-122">An enumeration of all the files intended for upload and the mappings from these files to blobs.</span></span>

-   <span data-ttu-id="b307e-123">Az egyes fájlok szegmenst ellenőrzőösszegeket.</span><span class="sxs-lookup"><span data-stu-id="b307e-123">Checksums of the segments of each file.</span></span>

-   <span data-ttu-id="b307e-124">A metaadatok és a tulajdonságok minden egyes blob társítandó vonatkozó információk.</span><span class="sxs-lookup"><span data-stu-id="b307e-124">Information about the metadata and properties to associate with each blob.</span></span>

-   <span data-ttu-id="b307e-125">A művelet neve megegyezik egy már meglévő blob a tárolóban lévő blob, amely feltöltődik-e listáját.</span><span class="sxs-lookup"><span data-stu-id="b307e-125">A listing of the action to take if a blob that is being uploaded has the same name as an existing blob in the container.</span></span> <span data-ttu-id="b307e-126">A lehetséges értékek: a) a blob felülírja a fájlt, a b) megtartja a meglévő blob és a skip feltölteni a fájlt, a c) hozzáfűzése egy utótagot a nevét, hogy más fájlok nem ütköznek.</span><span class="sxs-lookup"><span data-stu-id="b307e-126">Possible options are: a) overwrite the blob with the file, b) keep the existing blob and skip uploading the file, c) append a suffix to the name so that it does not conflict with other files.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="b307e-127">A szállítási hely beszerzése</span><span class="sxs-lookup"><span data-stu-id="b307e-127">Obtaining your shipping location</span></span>

<span data-ttu-id="b307e-128">Mielőtt létrehozna egy importálási feladat, be kell szereznie a szállítási hely nevét és címét meghívásával a [lista helyek](/rest/api/storageimportexport/listlocations) műveletet.</span><span class="sxs-lookup"><span data-stu-id="b307e-128">Before creating an import job, you need to obtain a shipping location name and address by calling the [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="b307e-129">`List Locations`helyek és a levelezési címek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b307e-129">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="b307e-130">Jelöljön ki egy helyet a visszaadott listából, és küldje el a merevlemez-meghajtók adott címre.</span><span class="sxs-lookup"><span data-stu-id="b307e-130">You can select a location from the returned list and ship your hard drives to that address.</span></span> <span data-ttu-id="b307e-131">Használhatja a `Get Location` közvetlenül beszerzése a szállítási cím, egy adott helyre vonatkozó műveletet.</span><span class="sxs-lookup"><span data-stu-id="b307e-131">You can also use the `Get Location` operation to obtain the shipping address for a specific location directly.</span></span>

 <span data-ttu-id="b307e-132">A szállítási raktár beszerzése az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b307e-132">Follow the steps below to obtain the shipping location:</span></span>

-   <span data-ttu-id="b307e-133">Azonosítsa a tárfiók helyének nevét.</span><span class="sxs-lookup"><span data-stu-id="b307e-133">Identify the name of the location of your storage account.</span></span> <span data-ttu-id="b307e-134">Ez az érték található a **hely** található a tárfiók **irányítópult** az Azure portál vagy a service management API művelettel lekérdezett [Storage-fiók beszerzése Tulajdonságok](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="b307e-134">This value can be found under the **Location** field on the storage account's **Dashboard** in the Azure portal or queried for by using the service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="b307e-135">A hely, amelyhez feldolgozni ezt a tárfiókot meghívásával beolvasása a `Get Location` műveletet.</span><span class="sxs-lookup"><span data-stu-id="b307e-135">Retrieve the location that is available to process this storage account by calling the `Get Location` operation.</span></span>

-   <span data-ttu-id="b307e-136">Ha a `AlternateLocations` helyének tulajdonság tartalmaz maga a hely, akkor használja ezt a helyet kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="b307e-136">If the `AlternateLocations` property of the location contains the location itself, then it is okay to use this location.</span></span> <span data-ttu-id="b307e-137">Ellenkező esetben hívható meg a `Get Location` műveletet a másodlagos helyek.</span><span class="sxs-lookup"><span data-stu-id="b307e-137">Otherwise, call the `Get Location` operation again with one of the alternate locations.</span></span> <span data-ttu-id="b307e-138">Az eredeti helyre ideiglenesen le lehet, hogy a következő karbantartási.</span><span class="sxs-lookup"><span data-stu-id="b307e-138">The original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-the-import-job"></a><span data-ttu-id="b307e-139">Az importálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b307e-139">Creating the import job</span></span>
<span data-ttu-id="b307e-140">Az importálási feladat létrehozása, hívja meg a [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet.</span><span class="sxs-lookup"><span data-stu-id="b307e-140">To create the import job, call the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="b307e-141">Meg kell adnia a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b307e-141">You will need to provide the following information:</span></span>

-   <span data-ttu-id="b307e-142">A feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="b307e-142">A name for the job.</span></span>

-   <span data-ttu-id="b307e-143">A tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="b307e-143">The storage account name.</span></span>

-   <span data-ttu-id="b307e-144">A szállítási hely neve, az előző lépésben beszerzett.</span><span class="sxs-lookup"><span data-stu-id="b307e-144">The shipping location name, obtained from the previous step.</span></span>

-   <span data-ttu-id="b307e-145">A feladat típusa (importálás).</span><span class="sxs-lookup"><span data-stu-id="b307e-145">A job type (Import).</span></span>

-   <span data-ttu-id="b307e-146">A válaszcím, ahol a meghajtók kell-e küldeni az importálási feladat befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="b307e-146">The return address where the drives should be sent after the import job has completed.</span></span>

-   <span data-ttu-id="b307e-147">A feladat meghajtók listája.</span><span class="sxs-lookup"><span data-stu-id="b307e-147">The list of drives in the job.</span></span> <span data-ttu-id="b307e-148">Minden olyan meghajtó meg kell adni a következő adatokat, a meghajtó előkészítési lépés során kapott:</span><span class="sxs-lookup"><span data-stu-id="b307e-148">For each drive, you must include the following information that was obtained during the drive preparation step:</span></span>

    -   <span data-ttu-id="b307e-149">A meghajtó azonosítója</span><span class="sxs-lookup"><span data-stu-id="b307e-149">The drive Id</span></span>

    -   <span data-ttu-id="b307e-150">A BitLocker-kulcsot</span><span class="sxs-lookup"><span data-stu-id="b307e-150">The BitLocker key</span></span>

    -   <span data-ttu-id="b307e-151">Az Alkalmazásjegyzék-fájl relatív elérési út a merevlemez-meghajtón</span><span class="sxs-lookup"><span data-stu-id="b307e-151">The manifest file relative path on the hard drive</span></span>

    -   <span data-ttu-id="b307e-152">A Base16 kódolású jegyzékfájl MD5 kivonatoló</span><span class="sxs-lookup"><span data-stu-id="b307e-152">The Base16 encoded manifest file MD5 hash</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="b307e-153">A meghajtók szállítási</span><span class="sxs-lookup"><span data-stu-id="b307e-153">Shipping your drives</span></span>
<span data-ttu-id="b307e-154">Kell küldje el az előző lépésben beszerzett címre meghajtó, és a csomag követési számának meg kell adnia az Import/Export szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b307e-154">You must ship your drives to the address that you obtained from the previous step, and you must provide the Import/Export service with the tracking number of the package.</span></span>

> [!NOTE]
>  <span data-ttu-id="b307e-155">A meghajtók, amely előállít egy azonosítószám a csomag támogatott szolgáltatónként szolgáltatáson keresztül kell küldje el.</span><span class="sxs-lookup"><span data-stu-id="b307e-155">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-the-import-job-with-your-shipping-information"></a><span data-ttu-id="b307e-156">Az importálási feladat naplóküldése adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="b307e-156">Updating the import job with your shipping information</span></span>
<span data-ttu-id="b307e-157">Miután a nyilvántartási szám, hívja az [frissítése feladat tulajdonságai](/api/storageimportexport/jobs#Jobs_Update) a szállítási szolgáltatónként nevét, a projekt a nyomon követési száma és a vivőjel-számát visszatérési szállítási frissítési művelete.</span><span class="sxs-lookup"><span data-stu-id="b307e-157">After you have your tracking number, call the [Update Job Properties](/api/storageimportexport/jobs#Jobs_Update) operation to update the shipping carrier name, the tracking number for the job, and the carrier account number for return shipping.</span></span> <span data-ttu-id="b307e-158">Opcionálisan megadhat meghajtók és a szállítási dátumot is.</span><span class="sxs-lookup"><span data-stu-id="b307e-158">You can optionally specify the number of drives and the shipping date as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b307e-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b307e-159">Next steps</span></span>

* [<span data-ttu-id="b307e-160">Az Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="b307e-160">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
