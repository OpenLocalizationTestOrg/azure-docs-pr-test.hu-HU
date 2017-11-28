---
title: "Importálni és exportálni egy tartományi zóna fájlt az Azure DNS-Azure CLI 1.0 használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan importálhat és exportálhat egy DNS-zónafájlját az Azure DNS-Azure CLI 1.0 használatával"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: d6d3fa7aa0e8b2462b3a6b4b66d3d87ab5535314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli-10"></a><span data-ttu-id="baca5-103">Importálni és exportálni egy DNS-zónafájlját az Azure CLI 1.0 használatával</span><span class="sxs-lookup"><span data-stu-id="baca5-103">Import and export a DNS zone file using the Azure CLI 1.0</span></span> 

<span data-ttu-id="baca5-104">Ez a cikk bemutatja, hogyan importálása és exportálása a DNS-zóna fájlok az Azure DNS az Azure CLI 1.0 használatával.</span><span class="sxs-lookup"><span data-stu-id="baca5-104">This article walks you through how to import and export DNS zone files for Azure DNS using the Azure CLI 1.0.</span></span>

## <a name="introduction-to-dns-zone-migration"></a><span data-ttu-id="baca5-105">DNS-zóna áttelepítés</span><span class="sxs-lookup"><span data-stu-id="baca5-105">Introduction to DNS zone migration</span></span>

<span data-ttu-id="baca5-106">A DNS-zónafájlját egy szövegfájlt, amely minden tartománynévrendszer (DNS) rekordot a zónában részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="baca5-106">A DNS zone file is a text file that contains details of every Domain Name System (DNS) record in the zone.</span></span> <span data-ttu-id="baca5-107">Ez azt jelenti, szabványos formátumban, így a megfelelő DNS-rekordok átvitele a DNS-rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="baca5-107">It follows a standard format, making it suitable for transferring DNS records between DNS systems.</span></span> <span data-ttu-id="baca5-108">Zóna fájllal módja a gyors, megbízható és kényelmes átvitele egy DNS-zónát, vagy abból az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="baca5-108">Using a zone file is a quick, reliable, and convenient way to transfer a DNS zone into or out of Azure DNS.</span></span>

<span data-ttu-id="baca5-109">Az Azure DNS támogatja, importálása és exportálása a zóna fájlok az Azure parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="baca5-109">Azure DNS supports importing and exporting zone files by using the Azure command-line interface (CLI).</span></span> <span data-ttu-id="baca5-110">Zóna fájl importálás **nem** jelenleg támogatott Azure PowerShell vagy az Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="baca5-110">Zone file import is **not** currently supported via Azure PowerShell or the Azure portal.</span></span>

<span data-ttu-id="baca5-111">Az Azure CLI 1.0 Azure-szolgáltatások kezelésére használt platformfüggetlen parancssori eszközzel.</span><span class="sxs-lookup"><span data-stu-id="baca5-111">The Azure CLI 1.0 is a cross-platform command-line tool used for managing Azure services.</span></span> <span data-ttu-id="baca5-112">A Windows, Mac és Linux rendszerek érhető el a [Azure letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="baca5-112">It is available for the Windows, Mac, and Linux platforms from the [Azure downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="baca5-113">Többplatformos támogatást fontos történő importálására és exportálására zóna fájlokat, mert a leggyakrabban használt név kiszolgálószoftver [kötési](https://www.isc.org/downloads/bind/), jellemzően futó Linux.</span><span class="sxs-lookup"><span data-stu-id="baca5-113">Cross-platform support is important for importing and exporting zone files, because the most common name server software, [BIND](https://www.isc.org/downloads/bind/), typically runs on Linux.</span></span>

> [!NOTE]
> <span data-ttu-id="baca5-114">Jelenleg az Azure parancssori felület két verziója.</span><span class="sxs-lookup"><span data-stu-id="baca5-114">There are currently two versions of the Azure CLI.</span></span> <span data-ttu-id="baca5-115">CLI1.0 Node.js alapul, és rendelkezik az "azure" kezdve parancsok.</span><span class="sxs-lookup"><span data-stu-id="baca5-115">CLI1.0 is based on Node.js, and has commands beginning with "azure".</span></span>
> <span data-ttu-id="baca5-116">CLI2.0 "az" kezdve parancsok rendelkezik, és a Python alapul.</span><span class="sxs-lookup"><span data-stu-id="baca5-116">CLI2.0 is based on Python and has commands beginning with "az".</span></span> <span data-ttu-id="baca5-117">Zóna fájl importálása verziójával is támogatott, amíg azt javasoljuk, CLI1.0 parancsok ezen a lapon ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="baca5-117">While zone file import is supported in both versions, we recommend using the CLI1.0 commands, as described in this page.</span></span>

## <a name="obtain-your-existing-dns-zone-file"></a><span data-ttu-id="baca5-118">A meglévő DNS-zónafájlját beszerzése</span><span class="sxs-lookup"><span data-stu-id="baca5-118">Obtain your existing DNS zone file</span></span>

<span data-ttu-id="baca5-119">Mielőtt egy DNS-zónafájlját az Azure DNS importálja, kell hozzájutni a zóna fájlt.</span><span class="sxs-lookup"><span data-stu-id="baca5-119">Before you import a DNS zone file into Azure DNS, you need to obtain a copy of the zone file.</span></span> <span data-ttu-id="baca5-120">A fájl forrásában attól függ, amelyben a DNS-zóna jelenleg üzemel.</span><span class="sxs-lookup"><span data-stu-id="baca5-120">The source of this file depends on where the DNS zone is currently hosted.</span></span>

* <span data-ttu-id="baca5-121">Ha a DNS-zóna (például a tartományregisztráló, dedikált DNS-szolgáltató vagy alternatív felhőszolgáltatóként) egy partner szolgáltatás működteti, a service töltse le a DNS-zónafájlját lehetőséget kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="baca5-121">If your DNS zone is hosted by a partner service (such as a domain registrar, dedicated DNS hosting provider, or alternative cloud provider), that service should provide the ability to download the DNS zone file.</span></span>
* <span data-ttu-id="baca5-122">Ha a DNS-zónát a Windows DNS-üzemelteti, az alapértelmezett mappa, a zóna fájlok: **%systemroot%\system32\dns**.</span><span class="sxs-lookup"><span data-stu-id="baca5-122">If your DNS zone is hosted on Windows DNS, the default folder for the zone files is **%systemroot%\system32\dns**.</span></span> <span data-ttu-id="baca5-123">A fájl teljes elérési útját minden zóna is jeleníti meg a **általános** lapon, a DNS-konzol.</span><span class="sxs-lookup"><span data-stu-id="baca5-123">The full path to each zone file also shows on the **General** tab of the DNS console.</span></span>
* <span data-ttu-id="baca5-124">Ha a DNS-zóna BIND használatával, a hely a zónák zónafájl meg van adva a BIND konfigurációs fájl **named.conf**.</span><span class="sxs-lookup"><span data-stu-id="baca5-124">If your DNS zone is hosted by using BIND, the location of the zone file for each zone is specified in the BIND configuration file **named.conf**.</span></span>

> [!NOTE]
> <span data-ttu-id="baca5-125">GoDaddy letöltésének zóna fájlok formátuma némileg nem szabványos.</span><span class="sxs-lookup"><span data-stu-id="baca5-125">Zone files downloaded from GoDaddy have a slightly nonstandard format.</span></span> <span data-ttu-id="baca5-126">A probléma megoldására ezeket a fájlokat zóna Azure DNS-ben való importálása előtt kell.</span><span class="sxs-lookup"><span data-stu-id="baca5-126">You need to correct this before you import these zone files into Azure DNS.</span></span>
>
> <span data-ttu-id="baca5-127">Az RDATA minden DNS-rekord a DNS-nevek teljesen minősített nevek vannak megadva, de nem rendelkeznek a záró "." Ez azt jelenti, hogy azok más relatív nevek DNS rendszer értelmezi.</span><span class="sxs-lookup"><span data-stu-id="baca5-127">DNS names in the RDATA of each DNS record are specified as fully qualified names, but they don't have a terminating "." This means they are interpreted by other DNS systems as relative names.</span></span> <span data-ttu-id="baca5-128">Módosítania kell a zóna fájl hozzáfűzése a záró "." nevüket előtt az Azure DNS importálja azokat.</span><span class="sxs-lookup"><span data-stu-id="baca5-128">You need to edit the zone file to append the terminating "." to their names before you import them into Azure DNS.</span></span>
>
> <span data-ttu-id="baca5-129">Például a CNAME rekord "www 3600 CNAME contoso.com" módosítani kell a "www 3600 CNAME contoso.com."</span><span class="sxs-lookup"><span data-stu-id="baca5-129">For example, the CNAME record "www 3600 IN CNAME contoso.com" should be changed to "www 3600 IN CNAME contoso.com."</span></span>
> <span data-ttu-id="baca5-130">(az a záró ".").</span><span class="sxs-lookup"><span data-stu-id="baca5-130">(with a terminating ".").</span></span>

## <a name="import-a-dns-zone-file-into-azure-dns"></a><span data-ttu-id="baca5-131">Egy DNS-zóna fájlt importálja az Azure DNS</span><span class="sxs-lookup"><span data-stu-id="baca5-131">Import a DNS zone file into Azure DNS</span></span>

<span data-ttu-id="baca5-132">Új zóna az Azure DNS-zóna fájlok importálása hoz, ha egy nem létezik.</span><span class="sxs-lookup"><span data-stu-id="baca5-132">Importing a zone file creates a new zone in Azure DNS if one does not already exist.</span></span> <span data-ttu-id="baca5-133">Ha a zóna már létezik, a rekordhalmazok a(z) egyesíthető kell a meglévő rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="baca5-133">If the zone already exists, the record sets in the zone file must be merged with the existing record sets.</span></span>

### <a name="merge-behavior"></a><span data-ttu-id="baca5-134">Viselkedés egyesítése</span><span class="sxs-lookup"><span data-stu-id="baca5-134">Merge behavior</span></span>

* <span data-ttu-id="baca5-135">Alapértelmezés szerint a meglévő és új rekordhalmazok egyesítődnek.</span><span class="sxs-lookup"><span data-stu-id="baca5-135">By default, existing and new record sets are merged.</span></span> <span data-ttu-id="baca5-136">Egyesített rekordhalmaz belüli azonos rekordokat deszerializálni duplikált.</span><span class="sxs-lookup"><span data-stu-id="baca5-136">Identical records within a merged record set are de-duplicated.</span></span>
* <span data-ttu-id="baca5-137">Másik lehetőségként megadásával a `--force` beállítás, az importálási folyamat cserél meglévő rekordhalmazok az új rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="baca5-137">Alternatively, by specifying the `--force` option, the import process replaces existing record sets with new record sets.</span></span> <span data-ttu-id="baca5-138">Meglévő rekordhalmazok, amely nem rendelkezik egy megfelelő rekordot a zóna importált fájl nem lesznek eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="baca5-138">Existing record sets that do not have a corresponding record set in the imported zone file are not be removed.</span></span>
* <span data-ttu-id="baca5-139">Rekordhalmazok egyesítésekor a rendszer mennyi ideig élettartam (TTL) elérésű, korábban létező rekordkészletek szolgál.</span><span class="sxs-lookup"><span data-stu-id="baca5-139">When record sets are merged, the time to live (TTL) of preexisting record sets is used.</span></span> <span data-ttu-id="baca5-140">Ha `--force` van használt, az az új rekord készlet élettartam.</span><span class="sxs-lookup"><span data-stu-id="baca5-140">When `--force` is used, the TTL of the new record set is used.</span></span>
* <span data-ttu-id="baca5-141">Kezdő hatóság (SOA) paraméterek (kivéve `host`) az importált zóna fájlból, függetlenül attól, hogy mindig készít `--force` szolgál.</span><span class="sxs-lookup"><span data-stu-id="baca5-141">Start of Authority (SOA) parameters (except `host`) are always taken from the imported zone file, regardless of whether `--force` is used.</span></span> <span data-ttu-id="baca5-142">Ehhez hasonlóan a névkiszolgáló-rekord a zóna felső pontja beállított, az a TTL-t mindig használatban van az importált zóna fájlból.</span><span class="sxs-lookup"><span data-stu-id="baca5-142">Similarly, for the name server record set at the zone apex, the TTL is always taken from the imported zone file.</span></span>
* <span data-ttu-id="baca5-143">Az importált CNAME rekord nem helyettesíti az meglévő CNAME rekord ugyanazzal a névvel kivéve, ha a `--force` paraméter meg van adva.</span><span class="sxs-lookup"><span data-stu-id="baca5-143">An imported CNAME record does not replace an existing CNAME record with the same name unless the `--force` parameter is specified.</span></span>
* <span data-ttu-id="baca5-144">Ha egy olyan CNAME rekordot, és egy másik rekord azonos, de eltérő típusú (függetlenül attól, amely meglévő vagy új) közötti ütközés lép fel, a meglévő rekord marad.</span><span class="sxs-lookup"><span data-stu-id="baca5-144">When a conflict arises between a CNAME record and another record of the same name but different type (regardless of which is existing or new), the existing record is retained.</span></span> <span data-ttu-id="baca5-145">Ez az független alkalmazásának `--force`.</span><span class="sxs-lookup"><span data-stu-id="baca5-145">This is independent of the use of `--force`.</span></span>

### <a name="additional-information-about-importing"></a><span data-ttu-id="baca5-146">További információt a importálása</span><span class="sxs-lookup"><span data-stu-id="baca5-146">Additional information about importing</span></span>

<span data-ttu-id="baca5-147">Az alábbi megjegyzések adja meg, a zóna további technikai részleteiért importálási folyamat.</span><span class="sxs-lookup"><span data-stu-id="baca5-147">The following notes provide additional technical details about the zone import process.</span></span>

* <span data-ttu-id="baca5-148">A `$TTL` direktíva nem kötelező, és támogatja azt.</span><span class="sxs-lookup"><span data-stu-id="baca5-148">The `$TTL` directive is optional, and it is supported.</span></span> <span data-ttu-id="baca5-149">Ha nem `$TTL` irányelv kap, a rekordok egy explicit TTL nélkül olyan importált értékre állítva egy alapértelmezett élettartam 3600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="baca5-149">When no `$TTL` directive is given, records without an explicit TTL are imported set to a default TTL of 3600 seconds.</span></span> <span data-ttu-id="baca5-150">Ha két rekordok rekord ugyanazokat a különböző TTLs adja meg, az alacsonyabb értéket használja.</span><span class="sxs-lookup"><span data-stu-id="baca5-150">When two records in the same record set specify different TTLs, the lower value is used.</span></span>
* <span data-ttu-id="baca5-151">A `$ORIGIN` direktíva nem kötelező, és támogatja azt.</span><span class="sxs-lookup"><span data-stu-id="baca5-151">The `$ORIGIN` directive is optional, and it is supported.</span></span> <span data-ttu-id="baca5-152">Ha nem `$ORIGIN` beállítva, a használt alapértelmezett érték megadva a parancssorban a zóna nevét (és a záró ".").</span><span class="sxs-lookup"><span data-stu-id="baca5-152">When no `$ORIGIN` is set, the default value used is the zone name as specified on the command line (plus the terminating ".").</span></span>
* <span data-ttu-id="baca5-153">A `$INCLUDE` és `$GENERATE` nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="baca5-153">The `$INCLUDE` and `$GENERATE` directives are not supported.</span></span>
* <span data-ttu-id="baca5-154">Ezek a rekordok típusok támogatottak: A, AAAA, CNAME, MX, NS, SOA, SRV és TXT.</span><span class="sxs-lookup"><span data-stu-id="baca5-154">These record types are supported: A, AAAA, CNAME, MX, NS, SOA, SRV, and TXT.</span></span>
* <span data-ttu-id="baca5-155">A SOA-rekord automatikusan létrejön Azure DNS-zóna létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="baca5-155">The SOA record is created automatically by Azure DNS when a zone is created.</span></span> <span data-ttu-id="baca5-156">Egy zóna fájl importálásakor összes SOA paraméter készít a zóna fájlból *kivéve* a `host` paraméter.</span><span class="sxs-lookup"><span data-stu-id="baca5-156">When you import a zone file, all SOA parameters are taken from the zone file *except* the `host` parameter.</span></span> <span data-ttu-id="baca5-157">Ez a paraméter az Azure DNS által nyújtott értéket használja.</span><span class="sxs-lookup"><span data-stu-id="baca5-157">This parameter uses the value provided by Azure DNS.</span></span> <span data-ttu-id="baca5-158">Ennek az az oka ennek a paraméternek az Azure DNS által nyújtott elsődleges névkiszolgálóra kell hivatkoznia.</span><span class="sxs-lookup"><span data-stu-id="baca5-158">This is because this parameter must refer to the primary name server provided by Azure DNS.</span></span>
* <span data-ttu-id="baca5-159">A névkiszolgáló-rekord a zóna felső pontja beállított is automatikusan hozza létre Azure DNS-ben a zóna létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="baca5-159">The name server record set at the zone apex is also created automatically by Azure DNS when the zone is created.</span></span> <span data-ttu-id="baca5-160">A rekordhalmaz csak a Élettartammal importálva van.</span><span class="sxs-lookup"><span data-stu-id="baca5-160">Only the TTL of this record set is imported.</span></span> <span data-ttu-id="baca5-161">Ezeket a rekordokat az Azure DNS által nyújtott névkiszolgálói neveket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="baca5-161">These records contain the name server names provided by Azure DNS.</span></span> <span data-ttu-id="baca5-162">A rekord adatait nem írják az importált zónafájl szereplő értékeket.</span><span class="sxs-lookup"><span data-stu-id="baca5-162">The record data is not overwritten by the values contained in the imported zone file.</span></span>
* <span data-ttu-id="baca5-163">Nyilvános előzetes Azure DNS TXT rekordok csak egyetlen-karakterlánc támogatja.</span><span class="sxs-lookup"><span data-stu-id="baca5-163">During Public Preview, Azure DNS supports only single-string TXT records.</span></span> <span data-ttu-id="baca5-164">Karakterláncsoros TXT rekord összefűzendő és csonkolja a 255 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="baca5-164">Multistring TXT records are be concatenated and truncated to 255 characters.</span></span>

### <a name="cli-format-and-values"></a><span data-ttu-id="baca5-165">Parancssori felület formátuma és értékei</span><span class="sxs-lookup"><span data-stu-id="baca5-165">CLI format and values</span></span>

<span data-ttu-id="baca5-166">A DNS-zónák importálására szolgáló Azure CLI parancs formátuma:</span><span class="sxs-lookup"><span data-stu-id="baca5-166">The format of the Azure CLI command to import a DNS zone is:</span></span>

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="baca5-167">Értékek:</span><span class="sxs-lookup"><span data-stu-id="baca5-167">Values:</span></span>

* <span data-ttu-id="baca5-168">`<resource group>`az Azure DNS-zóna az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="baca5-168">`<resource group>` is the name of the resource group for the zone in Azure DNS.</span></span>
* <span data-ttu-id="baca5-169">`<zone name>`a zóna neve van.</span><span class="sxs-lookup"><span data-stu-id="baca5-169">`<zone name>` is the name of the zone.</span></span>
* <span data-ttu-id="baca5-170">`<zone file name>`a zóna fájl, importálandók/útvonalnév van.</span><span class="sxs-lookup"><span data-stu-id="baca5-170">`<zone file name>` is the path/name of the zone file to be imported.</span></span>

<span data-ttu-id="baca5-171">Ha egy zónát a név nem létezik az erőforráscsoporthoz tartozik, az Ön létrejön.</span><span class="sxs-lookup"><span data-stu-id="baca5-171">If a zone with this name does not exist in the resource group, it is created for you.</span></span> <span data-ttu-id="baca5-172">Ha a zóna már létezik, az importált rekordhalmazok egyesítve lesznek az meglévő rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="baca5-172">If the zone already exists, the imported record sets are merged with existing record sets.</span></span> <span data-ttu-id="baca5-173">Felülírja a meglévő rekordkészleteket, használja a `--force` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="baca5-173">To overwrite the existing record sets, use the `--force` option.</span></span>

<span data-ttu-id="baca5-174">A zóna formátuma ténylegesen importálása nélkül, amellyel a `--parse-only` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="baca5-174">To verify the format of a zone file without actually importing it, use the `--parse-only` option.</span></span>

### <a name="step-1-import-a-zone-file"></a><span data-ttu-id="baca5-175">1. lépés</span><span class="sxs-lookup"><span data-stu-id="baca5-175">Step 1.</span></span> <span data-ttu-id="baca5-176">Zóna fájl importálása</span><span class="sxs-lookup"><span data-stu-id="baca5-176">Import a zone file</span></span>

<span data-ttu-id="baca5-177">A zóna zóna fájl importálásához **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="baca5-177">To import a zone file for the zone **contoso.com**.</span></span>

1. <span data-ttu-id="baca5-178">Jelentkezzen be az Azure-előfizetéshez az Azure CLI 1.0 használatával.</span><span class="sxs-lookup"><span data-stu-id="baca5-178">Sign in to your Azure subscription by using the Azure CLI 1.0.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="baca5-179">Válassza ki az előfizetést, ha szeretne létrehozni az új DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="baca5-179">Select the subscription where you want to create your new DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="baca5-180">Az Azure DNS egy olyan Azure csak erőforrás-kezelő szolgáltatás, így a Resource Manager módra kell állítani az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="baca5-180">Azure DNS is an Azure Resource Manager-only service, so the Azure CLI must be switched to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="baca5-181">Az Azure DNS-szolgáltatás használata előtt regisztrálnia kell az előfizetése használja a Microsoft.Network erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="baca5-181">Before you use the Azure DNS service, you must register your subscription to use the Microsoft.Network resource provider.</span></span> <span data-ttu-id="baca5-182">(Ez a műveletet egyszer kell elvégezni az egyes előfizetésekhez.)</span><span class="sxs-lookup"><span data-stu-id="baca5-182">(This is a one-time operation for each subscription.)</span></span>

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. <span data-ttu-id="baca5-183">Ha egy már nem rendelkezik, akkor is meg kell erőforrás-kezelő erőforráscsoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="baca5-183">If you don't have one already, you also need to create a Resource Manager resource group.</span></span>

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. <span data-ttu-id="baca5-184">A zóna importálandó **contoso.com** fájlból **contoso.com.txt** be egy új DNS-zóna erőforráscsoportban **myresourcegroup**, futtassa a parancsot `azure network dns zone import`.</span><span class="sxs-lookup"><span data-stu-id="baca5-184">To import the zone **contoso.com** from the file **contoso.com.txt** into a new DNS zone in the resource group **myresourcegroup**, run the command `azure network dns zone import`.</span></span><BR><span data-ttu-id="baca5-185">Ez a parancs a zóna fájlt, és elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="baca5-185">This command loads the zone file and parse it.</span></span> <span data-ttu-id="baca5-186">A parancs a zóna létrehozása az Azure DNS szolgáltatásban parancsokat végrehajtása során, és minden rekordot a zónában állítja be.</span><span class="sxs-lookup"><span data-stu-id="baca5-186">The command executes a series of commands on the Azure DNS service to create the zone and all the record sets in the zone.</span></span> <span data-ttu-id="baca5-187">A parancs jelentések folyamatban van a konzolablakban, továbbá az esetleges hibákat vagy figyelmeztetéseket.</span><span class="sxs-lookup"><span data-stu-id="baca5-187">The command reports progress in the console window, along with any errors or warnings.</span></span> <span data-ttu-id="baca5-188">Rekordhalmazok sorozat jönnek létre, mert nagy zóna fájl importálása néhány percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="baca5-188">Because record sets are created in series, it may take a few minutes to import a large zone file.</span></span>

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a><span data-ttu-id="baca5-189">2. lépés</span><span class="sxs-lookup"><span data-stu-id="baca5-189">Step 2.</span></span> <span data-ttu-id="baca5-190">Ellenőrizze a zóna</span><span class="sxs-lookup"><span data-stu-id="baca5-190">Verify the zone</span></span>

<span data-ttu-id="baca5-191">A fájl importálása után a DNS-zóna ellenőrzéséhez használhatja a következő módszerek egyikét sem:</span><span class="sxs-lookup"><span data-stu-id="baca5-191">To verify the DNS zone after you import the file, you can use any one of the following methods:</span></span>

* <span data-ttu-id="baca5-192">A rekord a következő Azure CLI-parancs segítségével jeleníthetők meg:</span><span class="sxs-lookup"><span data-stu-id="baca5-192">You can list the records by using the following Azure CLI command:</span></span>

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* <span data-ttu-id="baca5-193">A PowerShell-parancsmag segítségével listázhatja a rekordok `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="baca5-193">You can list the records by using the PowerShell cmdlet `Get-AzureRmDnsRecordSet`.</span></span>
* <span data-ttu-id="baca5-194">Használhat `nslookup` névfeloldás a rekordok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="baca5-194">You can use `nslookup` to verify name resolution for the records.</span></span> <span data-ttu-id="baca5-195">Mivel a zóna még nincs delegálása után kell explicit módon adja meg a megfelelő Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="baca5-195">Because the zone isn't delegated yet, you need to specify the correct Azure DNS name servers explicitly.</span></span> <span data-ttu-id="baca5-196">A következő minta bemutatja, hogyan lehet lekérni a zóna névkiszolgálóinak neveit.</span><span class="sxs-lookup"><span data-stu-id="baca5-196">The following sample shows how to retrieve the name server names assigned to the zone.</span></span> <span data-ttu-id="baca5-197">Informatikai is bemutatja, hogyan lekérdezni a "www" rekordhalmazhoz használatával `nslookup`.</span><span class="sxs-lookup"><span data-stu-id="baca5-197">IT also shows how to query the "www" record by using `nslookup`.</span></span>

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a><span data-ttu-id="baca5-198">3. lépés</span><span class="sxs-lookup"><span data-stu-id="baca5-198">Step 3.</span></span> <span data-ttu-id="baca5-199">DNS-delegálás frissítése</span><span class="sxs-lookup"><span data-stu-id="baca5-199">Update DNS delegation</span></span>

<span data-ttu-id="baca5-200">Miután ellenőrizte, hogy a zóna megfelelően importálták-e, kell, hogy az Azure DNS névkiszolgálóit mutasson a DNS-delegálás frissítése.</span><span class="sxs-lookup"><span data-stu-id="baca5-200">After you have verified that the zone has been imported correctly, you need to update the DNS delegation to point to the Azure DNS name servers.</span></span> <span data-ttu-id="baca5-201">További információkért lásd: a cikk [a DNS-delegálás frissítése](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="baca5-201">For more information, see the article [Update the DNS delegation](dns-domain-delegation.md).</span></span>

## <a name="export-a-dns-zone-file-from-azure-dns"></a><span data-ttu-id="baca5-202">Exportálja egy DNS-zónafájlját az Azure DNS-ben</span><span class="sxs-lookup"><span data-stu-id="baca5-202">Export a DNS zone file from Azure DNS</span></span>

<span data-ttu-id="baca5-203">A DNS-zónák importálására szolgáló Azure CLI parancs formátuma:</span><span class="sxs-lookup"><span data-stu-id="baca5-203">The format of the Azure CLI command to import a DNS zone is:</span></span>

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="baca5-204">Értékek:</span><span class="sxs-lookup"><span data-stu-id="baca5-204">Values:</span></span>

* <span data-ttu-id="baca5-205">`<resource group>`az Azure DNS-zóna az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="baca5-205">`<resource group>` is the name of the resource group for the zone in Azure DNS.</span></span>
* <span data-ttu-id="baca5-206">`<zone name>`a zóna neve van.</span><span class="sxs-lookup"><span data-stu-id="baca5-206">`<zone name>` is the name of the zone.</span></span>
* <span data-ttu-id="baca5-207">`<zone file name>`a zóna fájl exportálását/útvonalnév van.</span><span class="sxs-lookup"><span data-stu-id="baca5-207">`<zone file name>` is the path/name of the zone file to be exported.</span></span>

<span data-ttu-id="baca5-208">A zóna az importáláshoz, akkor először kell bejelentkezni, válassza ki az előfizetés, és konfigurálása az Azure parancssori felület használata a Resource Manager módra.</span><span class="sxs-lookup"><span data-stu-id="baca5-208">As with the zone import, you first need to sign in, choose your subscription, and configure the Azure CLI to use Resource Manager mode.</span></span>

### <a name="to-export-a-zone-file"></a><span data-ttu-id="baca5-209">Egy zóna fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="baca5-209">To export a zone file</span></span>

1. <span data-ttu-id="baca5-210">Jelentkezzen be az Azure-előfizetéshez az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="baca5-210">Sign in to your Azure subscription by using the Azure CLI.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="baca5-211">Válassza ki az előfizetést, ha szeretne létrehozni a DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="baca5-211">Select the subscription where you want to create your DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="baca5-212">Az Azure DNS egy olyan Azure csak erőforrás-kezelő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="baca5-212">Azure DNS is an Azure Resource Manager-only service.</span></span> <span data-ttu-id="baca5-213">Az Azure parancssori felület Resource Manager módra kell állítani.</span><span class="sxs-lookup"><span data-stu-id="baca5-213">The Azure CLI must be switched to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="baca5-214">A meglévő Azure DNS-zóna exportálása **contoso.com** erőforráscsoportban **myresourcegroup** fájl **contoso.com.txt** (az aktuális mappában), futtassa `azure network dns zone export`.</span><span class="sxs-lookup"><span data-stu-id="baca5-214">To export the existing Azure DNS zone **contoso.com** in resource group **myresourcegroup** to the file **contoso.com.txt** (in the current folder), run `azure network dns zone export`.</span></span> <span data-ttu-id="baca5-215">Ez a parancs meghívja az Azure DNS szolgáltatást a zónában rekordhalmazok enumerálása, és a BIND-kompatibilis zóna fájlba exportálhatja az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="baca5-215">This command  calls the Azure DNS service to enumerate record sets in the zone and export the results to a BIND-compatible zone file.</span></span>

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
