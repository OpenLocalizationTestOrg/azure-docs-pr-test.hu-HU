---
title: "aaaImport és exportálás a tartományzóna fájl használata az Azure CLI 1.0 DNS tooAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooimport és exportálás a DNS zóna fájl tooAzure DNS Azure CLI 1.0 használatával"
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
ms.openlocfilehash: 4c3163395e151e9934c730349b828c612491016f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-a-dns-zone-file-using-hello-azure-cli-10"></a><span data-ttu-id="6cbc3-103">Importálni és exportálni egy DNS-zónafájlját hello Azure CLI 1.0 használatával</span><span class="sxs-lookup"><span data-stu-id="6cbc3-103">Import and export a DNS zone file using hello Azure CLI 1.0</span></span> 

<span data-ttu-id="6cbc3-104">Ez a cikk bemutatja, hogyan hogyan tooimport és exportálási DNS zóna fájlok az Azure DNS használatával hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-104">This article walks you through how tooimport and export DNS zone files for Azure DNS using hello Azure CLI 1.0.</span></span>

## <a name="introduction-toodns-zone-migration"></a><span data-ttu-id="6cbc3-105">Bevezetés tooDNS zóna áttelepítése</span><span class="sxs-lookup"><span data-stu-id="6cbc3-105">Introduction tooDNS zone migration</span></span>

<span data-ttu-id="6cbc3-106">A DNS-zónafájlját egy szövegfájlt, amely az összes hello zónában tartománynévrendszer (DNS) rekord részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-106">A DNS zone file is a text file that contains details of every Domain Name System (DNS) record in hello zone.</span></span> <span data-ttu-id="6cbc3-107">Ez azt jelenti, szabványos formátumban, így a megfelelő DNS-rekordok átvitele a DNS-rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-107">It follows a standard format, making it suitable for transferring DNS records between DNS systems.</span></span> <span data-ttu-id="6cbc3-108">Egy zóna fájllal egy gyors, megbízható, és kényelmesen tootransfer egy DNS-zónát, vagy abból az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-108">Using a zone file is a quick, reliable, and convenient way tootransfer a DNS zone into or out of Azure DNS.</span></span>

<span data-ttu-id="6cbc3-109">Az Azure DNS támogatja, importálása és exportálása a zóna fájlok hello Azure parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-109">Azure DNS supports importing and exporting zone files by using hello Azure command-line interface (CLI).</span></span> <span data-ttu-id="6cbc3-110">Zóna fájl importálás **nem** jelenleg támogatott Azure PowerShell vagy hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-110">Zone file import is **not** currently supported via Azure PowerShell or hello Azure portal.</span></span>

<span data-ttu-id="6cbc3-111">hello Azure CLI 1.0 használt szolgáltatások kezeléséhez az Azure platformfüggetlen parancssori eszköz.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-111">hello Azure CLI 1.0 is a cross-platform command-line tool used for managing Azure services.</span></span> <span data-ttu-id="6cbc3-112">Használható a hello Windows, Mac és Linux platformokon a hello [Azure letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6cbc3-112">It is available for hello Windows, Mac, and Linux platforms from hello [Azure downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="6cbc3-113">Az importálás és exportálás a zóna fájlokat, mert hello leggyakoribb neve server szoftver, többplatformos támogatást fontos [kötési](https://www.isc.org/downloads/bind/), jellemzően futó Linux.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-113">Cross-platform support is important for importing and exporting zone files, because hello most common name server software, [BIND](https://www.isc.org/downloads/bind/), typically runs on Linux.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbc3-114">Jelenleg az Azure CLI hello két verziója.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-114">There are currently two versions of hello Azure CLI.</span></span> <span data-ttu-id="6cbc3-115">CLI1.0 Node.js alapul, és rendelkezik az "azure" kezdve parancsok.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-115">CLI1.0 is based on Node.js, and has commands beginning with "azure".</span></span>
> <span data-ttu-id="6cbc3-116">CLI2.0 "az" kezdve parancsok rendelkezik, és a Python alapul.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-116">CLI2.0 is based on Python and has commands beginning with "az".</span></span> <span data-ttu-id="6cbc3-117">Amíg a zóna fájl importálása verziójával is támogatott, azt javasoljuk, hello CLI1.0 parancsok, ezen az oldalon leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-117">While zone file import is supported in both versions, we recommend using hello CLI1.0 commands, as described in this page.</span></span>

## <a name="obtain-your-existing-dns-zone-file"></a><span data-ttu-id="6cbc3-118">A meglévő DNS-zónafájlját beszerzése</span><span class="sxs-lookup"><span data-stu-id="6cbc3-118">Obtain your existing DNS zone file</span></span>

<span data-ttu-id="6cbc3-119">A DNS-zónafájlját importál Azure DNS-ben, meg kell tooobtain hello zóna fájl egy példányát.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-119">Before you import a DNS zone file into Azure DNS, you need tooobtain a copy of hello zone file.</span></span> <span data-ttu-id="6cbc3-120">fájl forrásában hello attól függ, ahol hello DNS-zóna jelenleg üzemel.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-120">hello source of this file depends on where hello DNS zone is currently hosted.</span></span>

* <span data-ttu-id="6cbc3-121">Ha a DNS-zóna (például a tartományregisztráló, dedikált DNS-szolgáltató vagy alternatív felhőszolgáltatóként) egy partner szolgáltatás működteti, a service hello képességét toodownload hello DNS-zónafájlját kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-121">If your DNS zone is hosted by a partner service (such as a domain registrar, dedicated DNS hosting provider, or alternative cloud provider), that service should provide hello ability toodownload hello DNS zone file.</span></span>
* <span data-ttu-id="6cbc3-122">Ha a Windows DNS-üzemelteti a DNS-zónát, hello alapértelmezett mappa hello zóna fájlok: **%systemroot%\system32\dns**.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-122">If your DNS zone is hosted on Windows DNS, hello default folder for hello zone files is **%systemroot%\system32\dns**.</span></span> <span data-ttu-id="6cbc3-123">hello teljes elérési útja tooeach zóna fájl is látható hello **általános** hello DNS-konzol fülre.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-123">hello full path tooeach zone file also shows on hello **General** tab of hello DNS console.</span></span>
* <span data-ttu-id="6cbc3-124">Ha a DNS-zóna BIND használatával, hello hely hello zónafájl zónák meg van adva hello BIND konfigurációs fájl **named.conf**.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-124">If your DNS zone is hosted by using BIND, hello location of hello zone file for each zone is specified in hello BIND configuration file **named.conf**.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbc3-125">GoDaddy letöltésének zóna fájlok formátuma némileg nem szabványos.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-125">Zone files downloaded from GoDaddy have a slightly nonstandard format.</span></span> <span data-ttu-id="6cbc3-126">Toocorrect ez előtt kell a zóna fájlok importálása az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-126">You need toocorrect this before you import these zone files into Azure DNS.</span></span>
>
> <span data-ttu-id="6cbc3-127">Teljesen minősített nevek hello RDATA minden DNS-rekord a DNS-nevek vannak megadva, de nem rendelkeznek a záró "." Ez azt jelenti, hogy azok más relatív nevek DNS rendszer értelmezi.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-127">DNS names in hello RDATA of each DNS record are specified as fully qualified names, but they don't have a terminating "." This means they are interpreted by other DNS systems as relative names.</span></span> <span data-ttu-id="6cbc3-128">Szüksége tooedit hello zóna fájl tooappend hello leáll "." tootheir neve előtt az Azure DNS importálja azokat.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-128">You need tooedit hello zone file tooappend hello terminating "." tootheir names before you import them into Azure DNS.</span></span>
>
> <span data-ttu-id="6cbc3-129">Például hello CNAME rekordot a "www 3600 CNAME contoso.com" úgy kell módosítani túl "www 3600 IN CNAME contoso.com."</span><span class="sxs-lookup"><span data-stu-id="6cbc3-129">For example, hello CNAME record "www 3600 IN CNAME contoso.com" should be changed too"www 3600 IN CNAME contoso.com."</span></span>
> <span data-ttu-id="6cbc3-130">(az a záró ".").</span><span class="sxs-lookup"><span data-stu-id="6cbc3-130">(with a terminating ".").</span></span>

## <a name="import-a-dns-zone-file-into-azure-dns"></a><span data-ttu-id="6cbc3-131">Egy DNS-zóna fájlt importálja az Azure DNS</span><span class="sxs-lookup"><span data-stu-id="6cbc3-131">Import a DNS zone file into Azure DNS</span></span>

<span data-ttu-id="6cbc3-132">Új zóna az Azure DNS-zóna fájlok importálása hoz, ha egy nem létezik.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-132">Importing a zone file creates a new zone in Azure DNS if one does not already exist.</span></span> <span data-ttu-id="6cbc3-133">Ha hello zóna már létezik, hello rekordhalmazok hello zóna fájlban kell egyesíthető hello meglévő rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-133">If hello zone already exists, hello record sets in hello zone file must be merged with hello existing record sets.</span></span>

### <a name="merge-behavior"></a><span data-ttu-id="6cbc3-134">Viselkedés egyesítése</span><span class="sxs-lookup"><span data-stu-id="6cbc3-134">Merge behavior</span></span>

* <span data-ttu-id="6cbc3-135">Alapértelmezés szerint a meglévő és új rekordhalmazok egyesítődnek.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-135">By default, existing and new record sets are merged.</span></span> <span data-ttu-id="6cbc3-136">Egyesített rekordhalmaz belüli azonos rekordokat deszerializálni duplikált.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-136">Identical records within a merged record set are de-duplicated.</span></span>
* <span data-ttu-id="6cbc3-137">Másik lehetőségként hello megadásával `--force` lehetőségre, a meglévő rekordhalmazok új rekordhalmazok az importálási folyamat cserél hello.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-137">Alternatively, by specifying hello `--force` option, hello import process replaces existing record sets with new record sets.</span></span> <span data-ttu-id="6cbc3-138">Meglévő rekordhalmazok, amely nem rendelkezik a megfelelő rekordhalmazt hello importált zónafájl nem lesznek eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-138">Existing record sets that do not have a corresponding record set in hello imported zone file are not be removed.</span></span>
* <span data-ttu-id="6cbc3-139">Rekordhalmazok egyesítésekor hello idő toolive (TTL) elérésű, korábban létező rekordkészletek szerepel.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-139">When record sets are merged, hello time toolive (TTL) of preexisting record sets is used.</span></span> <span data-ttu-id="6cbc3-140">Ha `--force` van használva hello hello új rekordhalmaz Élettartammal szolgál.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-140">When `--force` is used, hello TTL of hello new record set is used.</span></span>
* <span data-ttu-id="6cbc3-141">Kezdő hatóság (SOA) paraméterek (kivéve `host`) fájlból hello importált zóna, függetlenül attól, hogy mindig készít `--force` szolgál.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-141">Start of Authority (SOA) parameters (except `host`) are always taken from hello imported zone file, regardless of whether `--force` is used.</span></span> <span data-ttu-id="6cbc3-142">Ehhez hasonlóan hello névkiszolgáló-rekord hello zóna felső pontja állítsa be, a hello TTL mindig használatban van hello importált zóna fájlból.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-142">Similarly, for hello name server record set at hello zone apex, hello TTL is always taken from hello imported zone file.</span></span>
* <span data-ttu-id="6cbc3-143">Az importált CNAME rekord nem váltják ki egy meglévő CNAME jegyezze fel az azonos név, kivéve, ha hello hello `--force` paraméter meg van adva.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-143">An imported CNAME record does not replace an existing CNAME record with hello same name unless hello `--force` parameter is specified.</span></span>
* <span data-ttu-id="6cbc3-144">Ha ütközés lép fel egy olyan CNAME rekordot, és egy másik rekord között azonos nevet, de különböző hello adja (függetlenül attól, amely meglévő vagy új), hello meglévő rekord marad.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-144">When a conflict arises between a CNAME record and another record of hello same name but different type (regardless of which is existing or new), hello existing record is retained.</span></span> <span data-ttu-id="6cbc3-145">Ez nem függ a hello használata `--force`.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-145">This is independent of hello use of `--force`.</span></span>

### <a name="additional-information-about-importing"></a><span data-ttu-id="6cbc3-146">További információt a importálása</span><span class="sxs-lookup"><span data-stu-id="6cbc3-146">Additional information about importing</span></span>

<span data-ttu-id="6cbc3-147">hello következő megjegyzések tartalmazzák hello zóna további technikai részleteiért importálási folyamat.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-147">hello following notes provide additional technical details about hello zone import process.</span></span>

* <span data-ttu-id="6cbc3-148">Hello `$TTL` direktíva nem kötelező, és támogatja azt.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-148">hello `$TTL` directive is optional, and it is supported.</span></span> <span data-ttu-id="6cbc3-149">Ha nem `$TTL` irányelv kap, egy explicit TTL nélkül importálásának tooa alapértelmezett élettartam 3600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-149">When no `$TTL` directive is given, records without an explicit TTL are imported set tooa default TTL of 3600 seconds.</span></span> <span data-ttu-id="6cbc3-150">Ha két rögzíti a hello azonos rekordhalmaz különböző TTLs megadásához hello alacsonyabb értéket használja.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-150">When two records in hello same record set specify different TTLs, hello lower value is used.</span></span>
* <span data-ttu-id="6cbc3-151">Hello `$ORIGIN` direktíva nem kötelező, és támogatja azt.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-151">hello `$ORIGIN` directive is optional, and it is supported.</span></span> <span data-ttu-id="6cbc3-152">Ha nem `$ORIGIN` van beállítva, alapértelmezett hello használt értéke hello zóna neve, ahogy hello parancssorban (plusz hello leáll ".").</span><span class="sxs-lookup"><span data-stu-id="6cbc3-152">When no `$ORIGIN` is set, hello default value used is hello zone name as specified on hello command line (plus hello terminating ".").</span></span>
* <span data-ttu-id="6cbc3-153">Hello `$INCLUDE` és `$GENERATE` nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-153">hello `$INCLUDE` and `$GENERATE` directives are not supported.</span></span>
* <span data-ttu-id="6cbc3-154">Ezek a rekordok típusok támogatottak: A, AAAA, CNAME, MX, NS, SOA, SRV és TXT.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-154">These record types are supported: A, AAAA, CNAME, MX, NS, SOA, SRV, and TXT.</span></span>
* <span data-ttu-id="6cbc3-155">hello SOA-rekord automatikusan létrejön Azure DNS-zóna létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-155">hello SOA record is created automatically by Azure DNS when a zone is created.</span></span> <span data-ttu-id="6cbc3-156">Egy zóna fájl importálásakor összes SOA típusú paramétert kell venni hello zónafájl *kivéve* hello `host` paraméter.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-156">When you import a zone file, all SOA parameters are taken from hello zone file *except* hello `host` parameter.</span></span> <span data-ttu-id="6cbc3-157">Ez a paraméter Azure DNS által nyújtott hello értéket használja.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-157">This parameter uses hello value provided by Azure DNS.</span></span> <span data-ttu-id="6cbc3-158">Ennek az az oka ennek a paraméternek toohello elsődleges névkiszolgáló Azure DNS által nyújtott kell hivatkoznia.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-158">This is because this parameter must refer toohello primary name server provided by Azure DNS.</span></span>
* <span data-ttu-id="6cbc3-159">hello neve server rekordhalmaz hello zóna csúcsán is automatikusan hozza létre Azure DNS hello zóna létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-159">hello name server record set at hello zone apex is also created automatically by Azure DNS when hello zone is created.</span></span> <span data-ttu-id="6cbc3-160">Csak hello a rekordhalmaz Élettartammal importálva van.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-160">Only hello TTL of this record set is imported.</span></span> <span data-ttu-id="6cbc3-161">Ezeket a rekordokat az Azure DNS által nyújtott hello névkiszolgálói neveket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-161">These records contain hello name server names provided by Azure DNS.</span></span> <span data-ttu-id="6cbc3-162">hello erőforrásrekord-adatokat nem írják hello értékek hello importált zóna fájlban találhatók.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-162">hello record data is not overwritten by hello values contained in hello imported zone file.</span></span>
* <span data-ttu-id="6cbc3-163">Nyilvános előzetes Azure DNS TXT rekordok csak egyetlen-karakterlánc támogatja.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-163">During Public Preview, Azure DNS supports only single-string TXT records.</span></span> <span data-ttu-id="6cbc3-164">Karakterláncsoros TXT-rekordok vannak összefűzött és csonkolt too255 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-164">Multistring TXT records are be concatenated and truncated too255 characters.</span></span>

### <a name="cli-format-and-values"></a><span data-ttu-id="6cbc3-165">Parancssori felület formátuma és értékei</span><span class="sxs-lookup"><span data-stu-id="6cbc3-165">CLI format and values</span></span>

<span data-ttu-id="6cbc3-166">hello hello Azure CLI parancs tooimport DNS-zóna formátuma:</span><span class="sxs-lookup"><span data-stu-id="6cbc3-166">hello format of hello Azure CLI command tooimport a DNS zone is:</span></span>

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="6cbc3-167">Értékek:</span><span class="sxs-lookup"><span data-stu-id="6cbc3-167">Values:</span></span>

* <span data-ttu-id="6cbc3-168">`<resource group>`van hello zóna Azure DNS-ben hello hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-168">`<resource group>` is hello name of hello resource group for hello zone in Azure DNS.</span></span>
* <span data-ttu-id="6cbc3-169">`<zone name>`hello zóna hello neve van.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-169">`<zone name>` is hello name of hello zone.</span></span>
* <span data-ttu-id="6cbc3-170">`<zone file name>`hello elérési útja vagy neve hello zóna fájl toobe importálva van.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-170">`<zone file name>` is hello path/name of hello zone file toobe imported.</span></span>

<span data-ttu-id="6cbc3-171">Ha egy ilyen nevű zónát hello erőforráscsoporthoz tartozik, nem létezik, az Ön létrejön.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-171">If a zone with this name does not exist in hello resource group, it is created for you.</span></span> <span data-ttu-id="6cbc3-172">Ha hello zóna már létezik, hello importált rekordhalmazok egyesítve lesznek az meglévő rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-172">If hello zone already exists, hello imported record sets are merged with existing record sets.</span></span> <span data-ttu-id="6cbc3-173">toooverwrite hello meglévő rekordhalmazok, használja a hello `--force` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-173">toooverwrite hello existing record sets, use hello `--force` option.</span></span>

<span data-ttu-id="6cbc3-174">a zóna fájl ténylegesen importálása, hello használata nélkül tooverify hello formátumát `--parse-only` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-174">tooverify hello format of a zone file without actually importing it, use hello `--parse-only` option.</span></span>

### <a name="step-1-import-a-zone-file"></a><span data-ttu-id="6cbc3-175">1. lépés</span><span class="sxs-lookup"><span data-stu-id="6cbc3-175">Step 1.</span></span> <span data-ttu-id="6cbc3-176">Zóna fájl importálása</span><span class="sxs-lookup"><span data-stu-id="6cbc3-176">Import a zone file</span></span>

<span data-ttu-id="6cbc3-177">egy zóna fájl hello zóna tooimport **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-177">tooimport a zone file for hello zone **contoso.com**.</span></span>

1. <span data-ttu-id="6cbc3-178">Jelentkezzen be tooyour Azure-előfizetés hello Azure CLI 1.0 használatával.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-178">Sign in tooyour Azure subscription by using hello Azure CLI 1.0.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="6cbc3-179">Válassza ki a kívánt toocreate az új DNS-zónát hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-179">Select hello subscription where you want toocreate your new DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="6cbc3-180">Az Azure DNS egy olyan Azure csak erőforrás-kezelő szolgáltatás, így hello Azure CLI kapcsolt tooResource kezelő módban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-180">Azure DNS is an Azure Resource Manager-only service, so hello Azure CLI must be switched tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="6cbc3-181">Hello Azure DNS-szolgáltatás használata előtt regisztrálnia kell az előfizetési toouse hello Microsoft.Network erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-181">Before you use hello Azure DNS service, you must register your subscription toouse hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="6cbc3-182">(Ez a műveletet egyszer kell elvégezni az egyes előfizetésekhez.)</span><span class="sxs-lookup"><span data-stu-id="6cbc3-182">(This is a one-time operation for each subscription.)</span></span>

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. <span data-ttu-id="6cbc3-183">Ha Ön nem rendelkezik ilyennel, szükség toocreate egy erőforrás-kezelő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-183">If you don't have one already, you also need toocreate a Resource Manager resource group.</span></span>

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. <span data-ttu-id="6cbc3-184">tooimport hello zóna **contoso.com** hello fájlból **contoso.com.txt** be egy új DNS-zóna hello erőforráscsoportban **myresourcegroup**, hello paranccsal `azure network dns zone import`.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-184">tooimport hello zone **contoso.com** from hello file **contoso.com.txt** into a new DNS zone in hello resource group **myresourcegroup**, run hello command `azure network dns zone import`.</span></span><BR><span data-ttu-id="6cbc3-185">Ez a parancs hello zóna fájlt, és elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-185">This command loads hello zone file and parse it.</span></span> <span data-ttu-id="6cbc3-186">hello parancs végrehajtása parancsokat a hello Azure DNS-szolgáltatás toocreate hello zónát, és minden hello rekordhalmazok hello zónában.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-186">hello command executes a series of commands on hello Azure DNS service toocreate hello zone and all hello record sets in hello zone.</span></span> <span data-ttu-id="6cbc3-187">hello parancs jelentések folyamatban hello konzolablakban, továbbá az esetleges hibákat vagy figyelmeztetéseket.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-187">hello command reports progress in hello console window, along with any errors or warnings.</span></span> <span data-ttu-id="6cbc3-188">Rekordhalmazok sorozat jönnek létre, mert szükség lehet néhány perc tooimport nagy zónafájl.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-188">Because record sets are created in series, it may take a few minutes tooimport a large zone file.</span></span>

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-hello-zone"></a><span data-ttu-id="6cbc3-189">2. lépés</span><span class="sxs-lookup"><span data-stu-id="6cbc3-189">Step 2.</span></span> <span data-ttu-id="6cbc3-190">Ellenőrizze a hello zóna</span><span class="sxs-lookup"><span data-stu-id="6cbc3-190">Verify hello zone</span></span>

<span data-ttu-id="6cbc3-191">tooverify hello DNS-zóna hello fájl importálása után használhatja hello a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="6cbc3-191">tooverify hello DNS zone after you import hello file, you can use any one of hello following methods:</span></span>

* <span data-ttu-id="6cbc3-192">Hello rögzíti az Azure parancssori felület parancs a következő hello segítségével jeleníthetők meg:</span><span class="sxs-lookup"><span data-stu-id="6cbc3-192">You can list hello records by using hello following Azure CLI command:</span></span>

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* <span data-ttu-id="6cbc3-193">Hello rekordok hello PowerShell parancsmag segítségével listázhatja `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-193">You can list hello records by using hello PowerShell cmdlet `Get-AzureRmDnsRecordSet`.</span></span>
* <span data-ttu-id="6cbc3-194">Használhat `nslookup` tooverify névfeloldás hello rögzíti.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-194">You can use `nslookup` tooverify name resolution for hello records.</span></span> <span data-ttu-id="6cbc3-195">Mivel hello zóna még nem delegált, explicit módon kell toospecify hello megfelelő Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-195">Because hello zone isn't delegated yet, you need toospecify hello correct Azure DNS name servers explicitly.</span></span> <span data-ttu-id="6cbc3-196">hello következő minta bemutatja, hogyan tooretrieve hello névkiszolgálói neveket hozzárendelve toohello zóna.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-196">hello following sample shows how tooretrieve hello name server names assigned toohello zone.</span></span> <span data-ttu-id="6cbc3-197">Informatikai is bemutatja, hogyan tooquery hello "www" történő rögzítéséhez `nslookup`.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-197">IT also shows how tooquery hello "www" record by using `nslookup`.</span></span>

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up hello DNS Record Set "@" of type "NS"
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

### <a name="step-3-update-dns-delegation"></a><span data-ttu-id="6cbc3-198">3. lépés</span><span class="sxs-lookup"><span data-stu-id="6cbc3-198">Step 3.</span></span> <span data-ttu-id="6cbc3-199">DNS-delegálás frissítése</span><span class="sxs-lookup"><span data-stu-id="6cbc3-199">Update DNS delegation</span></span>

<span data-ttu-id="6cbc3-200">Miután ellenőrizte, hogy helyesen lettek importálva hello zóna, tooupdate hello DNS delegálás toopoint toohello van szüksége az Azure DNS-névhez kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-200">After you have verified that hello zone has been imported correctly, you need tooupdate hello DNS delegation toopoint toohello Azure DNS name servers.</span></span> <span data-ttu-id="6cbc3-201">További információkért lásd: hello cikk [hello DNS-delegálás frissítése](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="6cbc3-201">For more information, see hello article [Update hello DNS delegation](dns-domain-delegation.md).</span></span>

## <a name="export-a-dns-zone-file-from-azure-dns"></a><span data-ttu-id="6cbc3-202">Exportálja egy DNS-zónafájlját az Azure DNS-ben</span><span class="sxs-lookup"><span data-stu-id="6cbc3-202">Export a DNS zone file from Azure DNS</span></span>

<span data-ttu-id="6cbc3-203">hello hello Azure CLI parancs tooimport DNS-zóna formátuma:</span><span class="sxs-lookup"><span data-stu-id="6cbc3-203">hello format of hello Azure CLI command tooimport a DNS zone is:</span></span>

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="6cbc3-204">Értékek:</span><span class="sxs-lookup"><span data-stu-id="6cbc3-204">Values:</span></span>

* <span data-ttu-id="6cbc3-205">`<resource group>`van hello zóna Azure DNS-ben hello hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-205">`<resource group>` is hello name of hello resource group for hello zone in Azure DNS.</span></span>
* <span data-ttu-id="6cbc3-206">`<zone name>`hello zóna hello neve van.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-206">`<zone name>` is hello name of hello zone.</span></span>
* <span data-ttu-id="6cbc3-207">`<zone file name>`hello zóna fájl toobe exportált hello/útvonalnév van.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-207">`<zone file name>` is hello path/name of hello zone file toobe exported.</span></span>

<span data-ttu-id="6cbc3-208">Hello zóna importálás, először a toosign, válassza ki az előfizetés, hello Azure CLI toouse Resource Manager módra konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-208">As with hello zone import, you first need toosign in, choose your subscription, and configure hello Azure CLI toouse Resource Manager mode.</span></span>

### <a name="tooexport-a-zone-file"></a><span data-ttu-id="6cbc3-209">egy zóna fájl tooexport</span><span class="sxs-lookup"><span data-stu-id="6cbc3-209">tooexport a zone file</span></span>

1. <span data-ttu-id="6cbc3-210">Jelentkezzen be tooyour Azure-előfizetés hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-210">Sign in tooyour Azure subscription by using hello Azure CLI.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="6cbc3-211">Válassza ki a kívánt toocreate a DNS-zóna hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-211">Select hello subscription where you want toocreate your DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="6cbc3-212">Az Azure DNS egy olyan Azure csak erőforrás-kezelő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-212">Azure DNS is an Azure Resource Manager-only service.</span></span> <span data-ttu-id="6cbc3-213">hello Azure CLI kapcsolt tooResource kezelő módban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-213">hello Azure CLI must be switched tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="6cbc3-214">tooexport hello Azure DNS-zónával **contoso.com** erőforráscsoportban **myresourcegroup** toohello fájl **contoso.com.txt** (hello aktuális mappában), futtassa a `azure network dns zone export`.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-214">tooexport hello existing Azure DNS zone **contoso.com** in resource group **myresourcegroup** toohello file **contoso.com.txt** (in hello current folder), run `azure network dns zone export`.</span></span> <span data-ttu-id="6cbc3-215">Ez a parancs hívások hello Azure DNS szolgáltatást tooenumerate rekordkészletek hello zónában, és hello eredmények tooa BIND-kompatibilis zóna-fájl exportálását.</span><span class="sxs-lookup"><span data-stu-id="6cbc3-215">This command  calls hello Azure DNS service tooenumerate record sets in hello zone and export hello results tooa BIND-compatible zone file.</span></span>

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
