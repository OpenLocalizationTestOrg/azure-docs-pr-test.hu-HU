---
title: "Számítási aaaAzure - Linux diagnosztikai bővítmény |} Microsoft Docs"
description: "Hogyan tooconfigure hello Azure Linux diagnosztikai bővítmény (LAD) toocollect metrikák eseményeit és Azure-ban futó Linux virtuális gépek."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 2b27ec00a876ded359a75170b407e28c40d8445d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-linux-diagnostic-extension-toomonitor-metrics-and-logs"></a><span data-ttu-id="853b2-103">Linux diagnosztikai bővítmény toomonitor metrikák és a naplókat</span><span class="sxs-lookup"><span data-stu-id="853b2-103">Use Linux Diagnostic Extension toomonitor metrics and logs</span></span>

<span data-ttu-id="853b2-104">Ez a dokumentum ismerteti a 3.0-s vagy újabb a hello Linux diagnosztikai bővítmény verzió.</span><span class="sxs-lookup"><span data-stu-id="853b2-104">This document describes version 3.0 and newer of hello Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="853b2-105">2.3-as és a régebbi verziójával kapcsolatos információkért lásd: [Ez a dokumentum](./classic/diagnostic-extension-v2.md).</span><span class="sxs-lookup"><span data-stu-id="853b2-105">For information about version 2.3 and older, see [this document](./classic/diagnostic-extension-v2.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="853b2-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="853b2-106">Introduction</span></span>

<span data-ttu-id="853b2-107">hello Linux diagnosztikai bővítmény segít a felhasználó a figyelő hello a Microsoft Azure-on futó Linux virtuális gépek állapotát.</span><span class="sxs-lookup"><span data-stu-id="853b2-107">hello Linux Diagnostic Extension helps a user monitor hello health of a Linux VM running on Microsoft Azure.</span></span> <span data-ttu-id="853b2-108">Hello a következő képességekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="853b2-108">It has hello following capabilities:</span></span>

* <span data-ttu-id="853b2-109">A virtuális gép hello rendszer teljesítménymutatók gyűjt, és a megadott tábla kijelölt tárfiókban tárolja azokat.</span><span class="sxs-lookup"><span data-stu-id="853b2-109">Collects system performance metrics from hello VM and stores them in a specific table in a designated storage account.</span></span>
* <span data-ttu-id="853b2-110">Alkalmazásnapló-események lekéri a syslog, és tárolja őket a megadott tábla kijelölt tárfiókot hello.</span><span class="sxs-lookup"><span data-stu-id="853b2-110">Retrieves log events from syslog and stores them in a specific table in hello designated storage account.</span></span>
* <span data-ttu-id="853b2-111">Lehetővé teszi, hogy a felhasználók toocustomize hello adatok metrikák gyűjtött és a feltöltése.</span><span class="sxs-lookup"><span data-stu-id="853b2-111">Enables users toocustomize hello data metrics that are collected and uploaded.</span></span>
* <span data-ttu-id="853b2-112">Lehetővé teszi a felhasználók toocustomize hello syslog létesítményekben és súlyossági szintek az eseményeket, amelyek gyűjtött és a feltöltése.</span><span class="sxs-lookup"><span data-stu-id="853b2-112">Enables users toocustomize hello syslog facilities and severity levels of events that are collected and uploaded.</span></span>
* <span data-ttu-id="853b2-113">Lehetővé teszi, hogy a felhasználók tooupload megadott napló fájlok tooa kijelölt tároló tábla.</span><span class="sxs-lookup"><span data-stu-id="853b2-113">Enables users tooupload specified log files tooa designated storage table.</span></span>
* <span data-ttu-id="853b2-114">Támogatja a küldés, metrikákat és naplózási események tooarbitrary EventHub végpontok és blobok JSON-formátumú hello kijelölt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="853b2-114">Supports sending metrics and log events tooarbitrary EventHub endpoints and JSON-formatted blobs in hello designated storage account.</span></span>

<span data-ttu-id="853b2-115">A bővítmény mindkét Azure üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="853b2-115">This extension works with both Azure deployment models.</span></span>

## <a name="installing-hello-extension-in-your-vm"></a><span data-ttu-id="853b2-116">A virtuális gép hello-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="853b2-116">Installing hello extension in your VM</span></span>

<span data-ttu-id="853b2-117">A bővítmény hello Azure PowerShell-parancsmagok, az Azure parancssori felület parancsfájlok vagy Azure központi telepítési sablonok használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="853b2-117">You can enable this extension by using hello Azure PowerShell cmdlets, Azure CLI scripts, or Azure deployment templates.</span></span> <span data-ttu-id="853b2-118">További információkért lásd: [bővítményeit biztosító funkciókat](./extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="853b2-118">For more information, see [Extensions Features](./extensions-features.md).</span></span>

<span data-ttu-id="853b2-119">hello Azure-portál nem használt tooenable vagy LAD 3.0 konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="853b2-119">hello Azure portal cannot be used tooenable or configure LAD 3.0.</span></span> <span data-ttu-id="853b2-120">Ehelyett azt telepíti, és konfigurálja a 2.3 verziója.</span><span class="sxs-lookup"><span data-stu-id="853b2-120">Instead, it installs and configures version 2.3.</span></span> <span data-ttu-id="853b2-121">Az Azure portál diagramok és értesítések működéséhez hello bővítmény verzióját is adataival.</span><span class="sxs-lookup"><span data-stu-id="853b2-121">Azure portal graphs and alerts work with data from both versions of hello extension.</span></span>

<span data-ttu-id="853b2-122">A telepítési utasításokat és egy [letölthető mintakonfiguráció](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0-s konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="853b2-122">These installation instructions and a [downloadable sample configuration](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) configure LAD 3.0 to:</span></span>

* <span data-ttu-id="853b2-123">rögzítési és tároló hello metrikák LAD 2.3; által biztosított volt</span><span class="sxs-lookup"><span data-stu-id="853b2-123">capture and store hello same metrics as were provided by LAD 2.3;</span></span>
* <span data-ttu-id="853b2-124">fájl rendszer metrika, új tooLAD 3.0; hasznos készlete rögzítése</span><span class="sxs-lookup"><span data-stu-id="853b2-124">capture a useful set of file system metrics, new tooLAD 3.0;</span></span>
* <span data-ttu-id="853b2-125">hello alapértelmezett syslog gyűjtemény LAD 2.3; által engedélyezett rögzítése</span><span class="sxs-lookup"><span data-stu-id="853b2-125">capture hello default syslog collection enabled by LAD 2.3;</span></span>
* <span data-ttu-id="853b2-126">Engedélyezze a hello Azure portál felhasználói diagramkészítési, és a virtuális gép metrikák riasztást küld.</span><span class="sxs-lookup"><span data-stu-id="853b2-126">enable hello Azure portal experience for charting and alerting on VM metrics.</span></span>

<span data-ttu-id="853b2-127">hello letölthető beállítás csak egy példa; Módosítsa ezt a toosuit saját igényeinek.</span><span class="sxs-lookup"><span data-stu-id="853b2-127">hello downloadable configuration is just an example; modify it toosuit your own needs.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="853b2-128">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="853b2-128">Prerequisites</span></span>

* <span data-ttu-id="853b2-129">**Az Azure Linux ügynök 2.2.0 verzió vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="853b2-129">**Azure Linux Agent version 2.2.0 or later**.</span></span> <span data-ttu-id="853b2-130">A legtöbb Azure virtuális gép Linux gyűjtemény lemezképei 2.2.7 verzióját tartalmazzák, vagy később.</span><span class="sxs-lookup"><span data-stu-id="853b2-130">Most Azure VM Linux gallery images include version 2.2.7 or later.</span></span> <span data-ttu-id="853b2-131">Futtatás `/usr/sbin/waagent -version` tooconfirm hello verziója van telepítve, a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="853b2-131">Run `/usr/sbin/waagent -version` tooconfirm hello version installed on hello VM.</span></span> <span data-ttu-id="853b2-132">Virtuális gép hello hello Vendég ügynök egy régebbi verziója fut, hajtsa végre a [ezeket az utasításokat](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate azt.</span><span class="sxs-lookup"><span data-stu-id="853b2-132">If hello VM is running an older version of hello guest agent, follow [these instructions](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate it.</span></span>
* <span data-ttu-id="853b2-133">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="853b2-133">**Azure CLI**.</span></span> <span data-ttu-id="853b2-134">[Azure CLI 2.0 hello beállítása](https://docs.microsoft.com/cli/azure/install-azure-cli) környezet a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="853b2-134">[Set up hello Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) environment on your machine.</span></span>
* <span data-ttu-id="853b2-135">hello wget parancs, ha már nincs: futtatása `sudo apt-get install wget`.</span><span class="sxs-lookup"><span data-stu-id="853b2-135">hello wget command, if you don't already have it: Run `sudo apt-get install wget`.</span></span>
* <span data-ttu-id="853b2-136">Meglévő Azure-előfizetése és a meglévő tárolási fiók belül toostore hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="853b2-136">An existing Azure subscription and an existing storage account within it toostore hello data.</span></span>

### <a name="sample-installation"></a><span data-ttu-id="853b2-137">A minta telepítése</span><span class="sxs-lookup"><span data-stu-id="853b2-137">Sample installation</span></span>

<span data-ttu-id="853b2-138">Adja meg helyes paraméterek hello hello első három sort, majd hajtsa végre ezt a parancsfájlt a legfelső szintű:</span><span class="sxs-lookup"><span data-stu-id="853b2-138">Fill in hello correct parameters on hello first three lines, then execute this script as root:</span></span>

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login tooAzure first before anything else
az login

# Select hello subscription containing hello storage account
az account set --subscription <your_azure_subscription_id>

# Download hello sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build hello VM resource ID. Replace storage account name and resource ID in hello public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build hello protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure tooinstall and enable hello extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

<span data-ttu-id="853b2-139">hello mintakonfiguráció hello URL-címet, és annak tartalmát, amelyek tulajdonos toochange.</span><span class="sxs-lookup"><span data-stu-id="853b2-139">hello URL for hello sample configuration, and its contents, are subject toochange.</span></span> <span data-ttu-id="853b2-140">Töltse le a hello portálbeállítások JSON-fájl egy példányát, és az igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="853b2-140">Download a copy of hello portal settings JSON file and customize it for your needs.</span></span> <span data-ttu-id="853b2-141">A sablonok vagy automation, hozhat létre egy saját másolat ahelyett, hogy letöltése URL-címet minden alkalommal, amikor használja.</span><span class="sxs-lookup"><span data-stu-id="853b2-141">Any templates or automation you construct should use your own copy, rather than downloading that URL each time.</span></span>

### <a name="updating-hello-extension-settings"></a><span data-ttu-id="853b2-142">Hello bővítmény beállításainak frissítése folyamatban</span><span class="sxs-lookup"><span data-stu-id="853b2-142">Updating hello extension settings</span></span>

<span data-ttu-id="853b2-143">Miután megváltoztatta a védett vagy nyilvános beállításai, telepíteni őket toohello VM futtatásával hello ugyanezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="853b2-143">After you've changed your Protected or Public settings, deploy them toohello VM by running hello same command.</span></span> <span data-ttu-id="853b2-144">Változás a hello-beállítások, ha frissített hello beállítások toohello bővítmény küldése.</span><span class="sxs-lookup"><span data-stu-id="853b2-144">If anything changed in hello settings, hello updated settings are sent toohello extension.</span></span> <span data-ttu-id="853b2-145">LAD hello konfiguráció újratöltése, és újraindítja a saját magát.</span><span class="sxs-lookup"><span data-stu-id="853b2-145">LAD reloads hello configuration and restarts itself.</span></span>

### <a name="migration-from-previous-versions-of-hello-extension"></a><span data-ttu-id="853b2-146">Hello bővítmény korábbi verzióiról való áttelepítés</span><span class="sxs-lookup"><span data-stu-id="853b2-146">Migration from previous versions of hello extension</span></span>

<span data-ttu-id="853b2-147">hello hello bővítmény legújabb verziója van **3.0**.</span><span class="sxs-lookup"><span data-stu-id="853b2-147">hello latest version of hello extension is **3.0**.</span></span> <span data-ttu-id="853b2-148">**A régi verziókat (2.x) elavultak, ezért lehet, hogy közzé nem tett, vagy azt követően 2018 július 31**.</span><span class="sxs-lookup"><span data-stu-id="853b2-148">**Any old versions (2.x) are deprecated and may be unpublished on or after July 31, 2018**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="853b2-149">A bővítmény legfrissebb változtatásokat toohello konfigurációs hello bővítmény vezet be.</span><span class="sxs-lookup"><span data-stu-id="853b2-149">This extension introduces breaking changes toohello configuration of hello extension.</span></span> <span data-ttu-id="853b2-150">Egy ilyen módosítás tooimprove hello biztonsági hello bővítmény; Ennek eredményeképpen visszamenőleges 2.x kompatibilitást sikerült nem kell tartani.</span><span class="sxs-lookup"><span data-stu-id="853b2-150">One such change was made tooimprove hello security of hello extension; as a result, backwards compatibility with 2.x could not be maintained.</span></span> <span data-ttu-id="853b2-151">Hello bővítmény Publisher ehhez a kiterjesztéshez is hello közzétevője hello 2.x verziója eltérő.</span><span class="sxs-lookup"><span data-stu-id="853b2-151">Also, hello Extension Publisher for this extension is different than hello publisher for hello 2.x versions.</span></span>
>
> <span data-ttu-id="853b2-152">a 2.x toothis új hello-kiterjesztés verziószámának toomigrate, el kell távolítania hello régi bővítmény (a hello régi közzétevő neve), majd a telepítés 3 hello bővítmény verziója.</span><span class="sxs-lookup"><span data-stu-id="853b2-152">toomigrate from 2.x toothis new version of hello extension, you must uninstall hello old extension (under hello old publisher name), then install version 3 of hello extension.</span></span>

<span data-ttu-id="853b2-153">Javaslatok:</span><span class="sxs-lookup"><span data-stu-id="853b2-153">Recommendations:</span></span>

* <span data-ttu-id="853b2-154">Telepítse a hello bővítmény engedélyezve van az automatikus alverzió frissítését.</span><span class="sxs-lookup"><span data-stu-id="853b2-154">Install hello extension with automatic minor version upgrade enabled.</span></span>
  * <span data-ttu-id="853b2-155">A klasszikus üzembe helyezési modellt virtuális gépeket adja meg a "3.*" hello verzió telepítésekor hello bővítmény Azure XPLAT parancssori felületen vagy a Powershellen keresztül.</span><span class="sxs-lookup"><span data-stu-id="853b2-155">On classic deployment model VMs, specify '3.*' as hello version if you are installing hello extension through Azure XPLAT CLI or Powershell.</span></span>
  * <span data-ttu-id="853b2-156">Az Azure Resource Manager telepítési modellhez tartozó virtuális gépek esetén tartalmazhat ""autoUpgradeMinorVersion": true" hello VM központi telepítési sablon.</span><span class="sxs-lookup"><span data-stu-id="853b2-156">On Azure Resource Manager deployment model VMs, include '"autoUpgradeMinorVersion": true' in hello VM deployment template.</span></span>
* <span data-ttu-id="853b2-157">A LAD 3.0 új/eltérőek a tárolási fiók használata.</span><span class="sxs-lookup"><span data-stu-id="853b2-157">Use a new/different storage account for LAD 3.0.</span></span> <span data-ttu-id="853b2-158">Van több kis incompatibilities LAD 2.3 és LAD 3.0 között, amelyek megosztása a problémás fiók:</span><span class="sxs-lookup"><span data-stu-id="853b2-158">There are several small incompatibilities between LAD 2.3 and LAD 3.0 that make sharing an account troublesome:</span></span>
  * <span data-ttu-id="853b2-159">LAD 3.0 tárolja a syslog-események egy táblát, amely egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="853b2-159">LAD 3.0 stores syslog events in a table with a different name.</span></span>
  * <span data-ttu-id="853b2-160">a karakterláncok hello counterSpecifier `builtin` metrikák LAD 3.0 különböznek.</span><span class="sxs-lookup"><span data-stu-id="853b2-160">hello counterSpecifier strings for `builtin` metrics differ in LAD 3.0.</span></span>

## <a name="protected-settings"></a><span data-ttu-id="853b2-161">Védett beállításai</span><span class="sxs-lookup"><span data-stu-id="853b2-161">Protected settings</span></span>

<span data-ttu-id="853b2-162">Ez a konfigurációs adatokat bizalmas adatokat, amelyeket védeni kell a nyilvános nézet, például a tároló hitelesítő adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="853b2-162">This set of configuration information contains sensitive information that should be protected from public view, for example, storage credentials.</span></span> <span data-ttu-id="853b2-163">Ezek a beállítások esetén az átvitt tooand hello bővítmény titkosított formában tárolja.</span><span class="sxs-lookup"><span data-stu-id="853b2-163">These settings are transmitted tooand stored by hello extension in encrypted form.</span></span>

```json
{
    "storageAccountName" : "hello storage account tooreceive data",
    "storageAccountEndPoint": "hello hostname suffix for hello cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

<span data-ttu-id="853b2-164">Név</span><span class="sxs-lookup"><span data-stu-id="853b2-164">Name</span></span> | <span data-ttu-id="853b2-165">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-165">Value</span></span>
---- | -----
<span data-ttu-id="853b2-166">storageAccountName</span><span class="sxs-lookup"><span data-stu-id="853b2-166">storageAccountName</span></span> | <span data-ttu-id="853b2-167">hello neve, amelyben hello bővítmény adatot ír hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="853b2-167">hello name of hello storage account in which data is written by hello extension.</span></span>
<span data-ttu-id="853b2-168">storageAccountEndPoint</span><span class="sxs-lookup"><span data-stu-id="853b2-168">storageAccountEndPoint</span></span> | <span data-ttu-id="853b2-169">(választható) hello végpont melyik hello storage-fiókban található hello felhő azonosítása.</span><span class="sxs-lookup"><span data-stu-id="853b2-169">(optional) hello endpoint identifying hello cloud in which hello storage account exists.</span></span> <span data-ttu-id="853b2-170">Ha ez a beállítás hiányzik, LAD alapértelmezés szerint használt érték toohello Azure nyilvános felhőjében `https://core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="853b2-170">If this setting is absent, LAD defaults toohello Azure public cloud, `https://core.windows.net`.</span></span> <span data-ttu-id="853b2-171">a tárfiók a Németországi Azure, Azure Government vagy Azure Kína toouse megfelelően állítsa ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="853b2-171">toouse a storage account in Azure Germany, Azure Government, or Azure China, set this value accordingly.</span></span>
<span data-ttu-id="853b2-172">storageAccountSasToken</span><span class="sxs-lookup"><span data-stu-id="853b2-172">storageAccountSasToken</span></span> | <span data-ttu-id="853b2-173">Egy [fiók SAS-jogkivonat](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) a Blob és Table szolgáltatásait (`ss='bt'`), megfelelő toocontainers és objektumok (`srt='co'`), amely hozzáadásához létrehozása, listában frissítése, és írási engedélyekkel (`sp='acluw'`).</span><span class="sxs-lookup"><span data-stu-id="853b2-173">An [Account SAS token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) for Blob and Table services (`ss='bt'`), applicable toocontainers and objects (`srt='co'`), which grants add, create, list, update, and write permissions (`sp='acluw'`).</span></span> <span data-ttu-id="853b2-174">Tegye *nem* hello bevezető kérdőjel (?) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="853b2-174">Do *not* include hello leading question-mark (?).</span></span>
<span data-ttu-id="853b2-175">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="853b2-175">mdsdHttpProxy</span></span> | <span data-ttu-id="853b2-176">(választható) HTTP proxy szükséges információkat tooenable hello bővítmény tooconnect toohello megadott tárfiók és végpontot.</span><span class="sxs-lookup"><span data-stu-id="853b2-176">(optional) HTTP proxy information needed tooenable hello extension tooconnect toohello specified storage account and endpoint.</span></span>
<span data-ttu-id="853b2-177">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="853b2-177">sinksConfig</span></span> | <span data-ttu-id="853b2-178">(választható) Alternatív célok toowhich metrikákkal és eseményekkel részleteit kézbesítése.</span><span class="sxs-lookup"><span data-stu-id="853b2-178">(optional) Details of alternative destinations toowhich metrics and events can be delivered.</span></span> <span data-ttu-id="853b2-179">egyes adatokat fogadó hello bővítmény által támogatott hello részleteit az alábbi hello szakaszok ismertetnek.</span><span class="sxs-lookup"><span data-stu-id="853b2-179">hello specific details of each data sink supported by hello extension are covered in hello sections that follow.</span></span>

<span data-ttu-id="853b2-180">Könnyen összeállíthat szükséges hello SAS-jogkivonat hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="853b2-180">You can easily construct hello required SAS token through hello Azure portal.</span></span>

1. <span data-ttu-id="853b2-181">Válassza ki a kívánt hello bővítmény toowrite hello általános célú tárfiókok fiók toowhich</span><span class="sxs-lookup"><span data-stu-id="853b2-181">Select hello general-purpose storage account toowhich you want hello extension toowrite</span></span>
1. <span data-ttu-id="853b2-182">Válassza a "Megosztás hozzáférési aláírás" hello hello bal oldali menü részéből beállításai</span><span class="sxs-lookup"><span data-stu-id="853b2-182">Select "Shared access signature" from hello Settings part of hello left menu</span></span>
1. <span data-ttu-id="853b2-183">Ellenőrizze az előzőekben leírt hello megfelelő szakaszait</span><span class="sxs-lookup"><span data-stu-id="853b2-183">Make hello appropriate sections as previously described</span></span>
1. <span data-ttu-id="853b2-184">Hello "Készítése SAS" gombra.</span><span class="sxs-lookup"><span data-stu-id="853b2-184">Click hello "Generate SAS" button.</span></span>

![Kép](./media/diagnostic-extension/make_sas.png)

<span data-ttu-id="853b2-186">Másolás hello SAS létre hello storageAccountSasToken mezőbe; Távolítsa el a hello bevezető kérdőjel ("?").</span><span class="sxs-lookup"><span data-stu-id="853b2-186">Copy hello generated SAS into hello storageAccountSasToken field; remove hello leading question-mark ("?").</span></span>

### <a name="sinksconfig"></a><span data-ttu-id="853b2-187">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="853b2-187">sinksConfig</span></span>

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

<span data-ttu-id="853b2-188">Ez a szakasz választható toowhich hello bővítmény összegyűjti az hello információkat küld a további célok meghatározása.</span><span class="sxs-lookup"><span data-stu-id="853b2-188">This optional section defines additional destinations toowhich hello extension sends hello information it collects.</span></span> <span data-ttu-id="853b2-189">hello "fogadó" tömb minden további adatokat fogadó egy objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="853b2-189">hello "sink" array contains an object for each additional data sink.</span></span> <span data-ttu-id="853b2-190">"típus" attribútum meghatározza, hogy hello hello hello objektum a többi attribútumával.</span><span class="sxs-lookup"><span data-stu-id="853b2-190">hello "type" attribute determines hello other attributes in hello object.</span></span>

<span data-ttu-id="853b2-191">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-191">Element</span></span> | <span data-ttu-id="853b2-192">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-192">Value</span></span>
------- | -----
<span data-ttu-id="853b2-193">név</span><span class="sxs-lookup"><span data-stu-id="853b2-193">name</span></span> | <span data-ttu-id="853b2-194">Egy karakterláncot toorefer toothis fogadó részeiben hello bővítménykonfiguráció használják.</span><span class="sxs-lookup"><span data-stu-id="853b2-194">A string used toorefer toothis sink elsewhere in hello extension configuration.</span></span>
<span data-ttu-id="853b2-195">type</span><span class="sxs-lookup"><span data-stu-id="853b2-195">type</span></span> | <span data-ttu-id="853b2-196">a fogadó múltbeli hello típusa.</span><span class="sxs-lookup"><span data-stu-id="853b2-196">hello type of sink being defined.</span></span> <span data-ttu-id="853b2-197">Határozza meg, más értékek hello (ha vannak) az ilyen típusú példányok.</span><span class="sxs-lookup"><span data-stu-id="853b2-197">Determines hello other values (if any) in instances of this type.</span></span>

<span data-ttu-id="853b2-198">Hello Linux diagnosztikai bővítmény 3.0-s verziója két fogadó-típusokat támogatja: az EventHub és JsonBlob.</span><span class="sxs-lookup"><span data-stu-id="853b2-198">Version 3.0 of hello Linux Diagnostic Extension supports two sink types: EventHub, and JsonBlob.</span></span>

#### <a name="hello-eventhub-sink"></a><span data-ttu-id="853b2-199">hello EventHub fogadó</span><span class="sxs-lookup"><span data-stu-id="853b2-199">hello EventHub sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

<span data-ttu-id="853b2-200">hello "sasURL" bejegyzés hello közzé kell tenni a teljes URL-címet, beleértve a SAS-jogkivonat, hello Eseményközpont toowhich adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="853b2-200">hello "sasURL" entry contains hello full URL, including SAS token, for hello Event Hub toowhich data should be published.</span></span> <span data-ttu-id="853b2-201">LAD egy házirendet, amely lehetővé teszi, hogy a hello küldési jogcím elnevezési Aláírást igényel.</span><span class="sxs-lookup"><span data-stu-id="853b2-201">LAD requires a SAS naming a policy that enables hello Send claim.</span></span> <span data-ttu-id="853b2-202">Példa:</span><span class="sxs-lookup"><span data-stu-id="853b2-202">An example:</span></span>

* <span data-ttu-id="853b2-203">Hozzon létre egy Event Hubs-névtér neve`contosohub`</span><span class="sxs-lookup"><span data-stu-id="853b2-203">Create an Event Hubs namespace called `contosohub`</span></span>
* <span data-ttu-id="853b2-204">Létrehoz egy Eseményközpontot hello névtér neve`syslogmsgs`</span><span class="sxs-lookup"><span data-stu-id="853b2-204">Create an Event Hub in hello namespace called `syslogmsgs`</span></span>
* <span data-ttu-id="853b2-205">Megosztott hozzáférési házirend létrehozása a hello nevű Eseményközpont `writer` , hogy lehetővé teszi, hogy hello küldési jogcím</span><span class="sxs-lookup"><span data-stu-id="853b2-205">Create a Shared access policy on hello Event Hub named `writer` that enables hello Send claim</span></span>

<span data-ttu-id="853b2-206">Ha létrehozott egy SAS jó 2018. január 1. az UTC éjfél hello sasURL érték lehet:</span><span class="sxs-lookup"><span data-stu-id="853b2-206">If you created a SAS good until midnight UTC on January 1, 2018, hello sasURL value might be:</span></span>

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

<span data-ttu-id="853b2-207">További információ a SAS-jogkivonatokat előállító az Event Hubs: [Ez a weblap](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span><span class="sxs-lookup"><span data-stu-id="853b2-207">For more information about generating SAS tokens for Event Hubs, see [this web page](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span></span>

#### <a name="hello-jsonblob-sink"></a><span data-ttu-id="853b2-208">hello JsonBlob fogadó</span><span class="sxs-lookup"><span data-stu-id="853b2-208">hello JsonBlob sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

<span data-ttu-id="853b2-209">Adatok irányítja a rendszer az Azure-tárfiókba blobok tooa JsonBlob fogadó tárolja.</span><span class="sxs-lookup"><span data-stu-id="853b2-209">Data directed tooa JsonBlob sink is stored in blobs in Azure storage.</span></span> <span data-ttu-id="853b2-210">Minden példánya LAD blob minden fogadó név óránként hoz létre.</span><span class="sxs-lookup"><span data-stu-id="853b2-210">Each instance of LAD creates a blob every hour for each sink name.</span></span> <span data-ttu-id="853b2-211">Minden egyes blob mindig tartalmaz egy szintaktikailag érvényes JSON-tömb objektum.</span><span class="sxs-lookup"><span data-stu-id="853b2-211">Each blob always contains a syntactically valid JSON array of object.</span></span> <span data-ttu-id="853b2-212">Új bejegyzések i kerülnek toohello tömb.</span><span class="sxs-lookup"><span data-stu-id="853b2-212">New entries are atomically added toohello array.</span></span> <span data-ttu-id="853b2-213">Blobok egy azonos nevet, amint hello fogadó hello-tárolóban vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="853b2-213">Blobs are stored in a container with hello same name as hello sink.</span></span> <span data-ttu-id="853b2-214">az Azure storage szabályok hello blob tároló nevének alkalmazni JsonBlob nyelő toohello nevek: 3 és 63 kisbetűs alfanumerikus ASCII-karaktereket, és kötőjelek között.</span><span class="sxs-lookup"><span data-stu-id="853b2-214">hello Azure storage rules for blob container names apply toohello names of JsonBlob sinks: between 3 and 63 lower-case alphanumeric ASCII characters or dashes.</span></span>

## <a name="public-settings"></a><span data-ttu-id="853b2-215">Nyilvános beállításai</span><span class="sxs-lookup"><span data-stu-id="853b2-215">Public settings</span></span>

<span data-ttu-id="853b2-216">Ez a struktúra hello bővítmény által összegyűjtött hello adatokat szabályozó beállítások több blokkot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="853b2-216">This structure contains various blocks of settings that control hello information collected by hello extension.</span></span> <span data-ttu-id="853b2-217">Minden beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="853b2-217">Each setting is optional.</span></span> <span data-ttu-id="853b2-218">Ha megad `ladCfg`, meg kell adni `StorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="853b2-218">If you specify `ladCfg`, you must also specify `StorageAccount`.</span></span>

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "hello storage account tooreceive data",
    "mdsdHttpProxy" : ""
}
```

<span data-ttu-id="853b2-219">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-219">Element</span></span> | <span data-ttu-id="853b2-220">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-220">Value</span></span>
------- | -----
<span data-ttu-id="853b2-221">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="853b2-221">StorageAccount</span></span> | <span data-ttu-id="853b2-222">hello neve, amelyben hello bővítmény adatot ír hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="853b2-222">hello name of hello storage account in which data is written by hello extension.</span></span> <span data-ttu-id="853b2-223">Kell azonos hello megadott nevet hello [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="853b2-223">Must be hello same name as is specified in hello [Protected settings](#protected-settings).</span></span>
<span data-ttu-id="853b2-224">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="853b2-224">mdsdHttpProxy</span></span> | <span data-ttu-id="853b2-225">(választható) Ugyanaz, mint hello [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="853b2-225">(optional) Same as in hello [Protected settings](#protected-settings).</span></span> <span data-ttu-id="853b2-226">hello nyilvános érték felülbírálja hello titkos érték, ha beállítása.</span><span class="sxs-lookup"><span data-stu-id="853b2-226">hello public value is overridden by hello private value, if set.</span></span> <span data-ttu-id="853b2-227">Helyezze el, amely tartalmazza a titkos kulcs, például a jelszó, hello proxybeállítások [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="853b2-227">Place proxy settings that contain a secret, such as a password, in hello [Protected settings](#protected-settings).</span></span>

<span data-ttu-id="853b2-228">hello maradék összetevőit részletei a következő részekben hello.</span><span class="sxs-lookup"><span data-stu-id="853b2-228">hello remaining elements are described in detail in hello following sections.</span></span>

### <a name="ladcfg"></a><span data-ttu-id="853b2-229">ladCfg</span><span class="sxs-lookup"><span data-stu-id="853b2-229">ladCfg</span></span>

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

<span data-ttu-id="853b2-230">A választható struktúra vezérlők hello gyűjtése metrikák és a naplókat a kézbesítési toohello Azure metrikák szolgáltatás és tooother adatokat fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="853b2-230">This optional structure controls hello gathering of metrics and logs for delivery toohello Azure Metrics service and tooother data sinks.</span></span> <span data-ttu-id="853b2-231">Meg kell adnia vagy `performanceCounters` vagy `syslogEvents` vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="853b2-231">You must specify either `performanceCounters` or `syslogEvents` or both.</span></span> <span data-ttu-id="853b2-232">Meg kell adnia a hello `metrics` struktúra.</span><span class="sxs-lookup"><span data-stu-id="853b2-232">You must specify hello `metrics` structure.</span></span>

<span data-ttu-id="853b2-233">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-233">Element</span></span> | <span data-ttu-id="853b2-234">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-234">Value</span></span>
------- | -----
<span data-ttu-id="853b2-235">eventVolume</span><span class="sxs-lookup"><span data-stu-id="853b2-235">eventVolume</span></span> | <span data-ttu-id="853b2-236">(választható) Vezérlők hello hello tárolási tábla belül létrehozott partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="853b2-236">(optional) Controls hello number of partitions created within hello storage table.</span></span> <span data-ttu-id="853b2-237">Egyikének kell lennie `"Large"`, `"Medium"`, vagy `"Small"`.</span><span class="sxs-lookup"><span data-stu-id="853b2-237">Must be one of `"Large"`, `"Medium"`, or `"Small"`.</span></span> <span data-ttu-id="853b2-238">Ha nincs megadva, hello alapértelmezett értéke `"Medium"`.</span><span class="sxs-lookup"><span data-stu-id="853b2-238">If not specified, hello default value is `"Medium"`.</span></span>
<span data-ttu-id="853b2-239">sampleRateInSeconds</span><span class="sxs-lookup"><span data-stu-id="853b2-239">sampleRateInSeconds</span></span> | <span data-ttu-id="853b2-240">(választható) hello alapértelmezett időközétől nyers (unaggregated) mérőszámok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="853b2-240">(optional) hello default interval between collection of raw (unaggregated) metrics.</span></span> <span data-ttu-id="853b2-241">legkisebb támogatott hello mintavételi gyakoriság: 15 másodperc.</span><span class="sxs-lookup"><span data-stu-id="853b2-241">hello smallest supported sample rate is 15 seconds.</span></span> <span data-ttu-id="853b2-242">Ha nincs megadva, hello alapértelmezett értéke `15`.</span><span class="sxs-lookup"><span data-stu-id="853b2-242">If not specified, hello default value is `15`.</span></span>

#### <a name="metrics"></a><span data-ttu-id="853b2-243">metrics</span><span class="sxs-lookup"><span data-stu-id="853b2-243">metrics</span></span>

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

<span data-ttu-id="853b2-244">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-244">Element</span></span> | <span data-ttu-id="853b2-245">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-245">Value</span></span>
------- | -----
<span data-ttu-id="853b2-246">resourceId</span><span class="sxs-lookup"><span data-stu-id="853b2-246">resourceId</span></span> | <span data-ttu-id="853b2-247">hello Azure Resource Manager erőforrás-azonosító hello virtuális gép vagy virtuálisgép-méretezési hello beállítása toowhich hello VM tartozik.</span><span class="sxs-lookup"><span data-stu-id="853b2-247">hello Azure Resource Manager resource ID of hello VM or of hello virtual machine scale set toowhich hello VM belongs.</span></span> <span data-ttu-id="853b2-248">Ezzel a beállítással lehet is adni, ha bármely JsonBlob fogadó hello konfigurációban szerepel.</span><span class="sxs-lookup"><span data-stu-id="853b2-248">This setting must be also specified if any JsonBlob sink is used in hello configuration.</span></span>
<span data-ttu-id="853b2-249">scheduledTransferPeriod</span><span class="sxs-lookup"><span data-stu-id="853b2-249">scheduledTransferPeriod</span></span> | <span data-ttu-id="853b2-250">hello gyakoriság, amellyel összesített adatok gyűjtése le toobe számított, és át tooAzure metrikák van 8601 időt időközönkénti kifejezve.</span><span class="sxs-lookup"><span data-stu-id="853b2-250">hello frequency at which aggregate metrics are toobe computed and transferred tooAzure Metrics, expressed as an IS 8601 time interval.</span></span> <span data-ttu-id="853b2-251">hello legkisebb átviteli időtartam 60 másodperc, ez azt jelenti, hogy PT1M.</span><span class="sxs-lookup"><span data-stu-id="853b2-251">hello smallest transfer period is 60 seconds, that is, PT1M.</span></span> <span data-ttu-id="853b2-252">Meg kell adnia legalább egy scheduledTransferPeriod.</span><span class="sxs-lookup"><span data-stu-id="853b2-252">You must specify at least one scheduledTransferPeriod.</span></span>

<span data-ttu-id="853b2-253">Minták hello performanceCounters szakaszában megadott metrikák gyűjtött 15 másodpercenként hello vagy hello mintavételi gyakoriság hello számláló explicit módon definiálva.</span><span class="sxs-lookup"><span data-stu-id="853b2-253">Samples of hello metrics specified in hello performanceCounters section are collected every 15 seconds or at hello sample rate explicitly defined for hello counter.</span></span> <span data-ttu-id="853b2-254">Ha több scheduledTransferPeriod gyakoriságot (mint pl. a hello) jelenik meg, minden Összesítés kiszámítása egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="853b2-254">If multiple scheduledTransferPeriod frequencies appear (as in hello example), each aggregation is computed independently.</span></span>

#### <a name="performancecounters"></a><span data-ttu-id="853b2-255">performanceCounters</span><span class="sxs-lookup"><span data-stu-id="853b2-255">performanceCounters</span></span>

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

<span data-ttu-id="853b2-256">Ez a szakasz választható metrikák hello gyűjteményét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="853b2-256">This optional section controls hello collection of metrics.</span></span> <span data-ttu-id="853b2-257">Nyers minták összesítik az egyes [scheduledTransferPeriod](#metrics) tooproduce ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="853b2-257">Raw samples are aggregated for each [scheduledTransferPeriod](#metrics) tooproduce these values:</span></span>

* <span data-ttu-id="853b2-258">témakörök</span><span class="sxs-lookup"><span data-stu-id="853b2-258">mean</span></span>
* <span data-ttu-id="853b2-259">minimális</span><span class="sxs-lookup"><span data-stu-id="853b2-259">minimum</span></span>
* <span data-ttu-id="853b2-260">Maximális</span><span class="sxs-lookup"><span data-stu-id="853b2-260">maximum</span></span>
* <span data-ttu-id="853b2-261">utolsó összegyűjtött érték</span><span class="sxs-lookup"><span data-stu-id="853b2-261">last-collected value</span></span>
* <span data-ttu-id="853b2-262">nyers minták száma használt toocompute hello összesítés</span><span class="sxs-lookup"><span data-stu-id="853b2-262">count of raw samples used toocompute hello aggregate</span></span>

<span data-ttu-id="853b2-263">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-263">Element</span></span> | <span data-ttu-id="853b2-264">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-264">Value</span></span>
------- | -----
<span data-ttu-id="853b2-265">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="853b2-265">sinks</span></span> | <span data-ttu-id="853b2-266">(választható) A neveket vesszővel tagolt listája LAD küld összesített metrika eredmények toowhich fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="853b2-266">(optional) A comma-separated list of names of sinks toowhich LAD sends aggregated metric results.</span></span> <span data-ttu-id="853b2-267">Az összes összesített adatok gyűjtése le felsorolva közzétett tooeach fogadó.</span><span class="sxs-lookup"><span data-stu-id="853b2-267">All aggregated metrics are published tooeach listed sink.</span></span> <span data-ttu-id="853b2-268">Lásd: [sinksConfig](#sinksconfig).</span><span class="sxs-lookup"><span data-stu-id="853b2-268">See [sinksConfig](#sinksconfig).</span></span> <span data-ttu-id="853b2-269">Példa: `"EHsink1, myjsonsink"`.</span><span class="sxs-lookup"><span data-stu-id="853b2-269">Example: `"EHsink1, myjsonsink"`.</span></span>
<span data-ttu-id="853b2-270">type</span><span class="sxs-lookup"><span data-stu-id="853b2-270">type</span></span> | <span data-ttu-id="853b2-271">Hello tényleges szolgáltató hello mérőszám azonosítja.</span><span class="sxs-lookup"><span data-stu-id="853b2-271">Identifies hello actual provider of hello metric.</span></span>
<span data-ttu-id="853b2-272">Osztály</span><span class="sxs-lookup"><span data-stu-id="853b2-272">class</span></span> | <span data-ttu-id="853b2-273">"Számláló", és azonosítja az adott metrika hello hello szolgáltató névtéren belül.</span><span class="sxs-lookup"><span data-stu-id="853b2-273">Together with "counter", identifies hello specific metric within hello provider's namespace.</span></span>
<span data-ttu-id="853b2-274">A számláló</span><span class="sxs-lookup"><span data-stu-id="853b2-274">counter</span></span> | <span data-ttu-id="853b2-275">"Class" együtt azonosítja az adott metrika hello hello szolgáltató névtéren belül.</span><span class="sxs-lookup"><span data-stu-id="853b2-275">Together with "class", identifies hello specific metric within hello provider's namespace.</span></span>
<span data-ttu-id="853b2-276">counterSpecifier</span><span class="sxs-lookup"><span data-stu-id="853b2-276">counterSpecifier</span></span> | <span data-ttu-id="853b2-277">Azonosítja az adott metrika hello hello Azure metrikák névtéren belül.</span><span class="sxs-lookup"><span data-stu-id="853b2-277">Identifies hello specific metric within hello Azure Metrics namespace.</span></span>
<span data-ttu-id="853b2-278">Az állapot</span><span class="sxs-lookup"><span data-stu-id="853b2-278">condition</span></span> | <span data-ttu-id="853b2-279">(választható) Választja ki egy adott példányához hello objektum toowhich hello metrika vonatkozik, vagy választják ki hello összesítési, hogy az objektum összes példánya között.</span><span class="sxs-lookup"><span data-stu-id="853b2-279">(optional) Selects a specific instance of hello object toowhich hello metric applies or selects hello aggregation across all instances of that object.</span></span> <span data-ttu-id="853b2-280">További információkért lásd: hello [ `builtin` metrikai meghatározásainak](#metrics-supported-by-builtin).</span><span class="sxs-lookup"><span data-stu-id="853b2-280">For more information, see hello [`builtin` metric definitions](#metrics-supported-by-builtin).</span></span>
<span data-ttu-id="853b2-281">sampleRate</span><span class="sxs-lookup"><span data-stu-id="853b2-281">sampleRate</span></span> | <span data-ttu-id="853b2-282">AZ, hogy beállítja hello gyakoriság, amellyel ez a mérőszám a nyers minták gyűjtik 8601 időközönként.</span><span class="sxs-lookup"><span data-stu-id="853b2-282">IS 8601 interval that sets hello rate at which raw samples for this metric are collected.</span></span> <span data-ttu-id="853b2-283">Ha nincs megadva, hello adatgyűjtési időköz értéke hello értékének [sampleRateInSeconds](#ladcfg).</span><span class="sxs-lookup"><span data-stu-id="853b2-283">If not set, hello collection interval is set by hello value of [sampleRateInSeconds](#ladcfg).</span></span> <span data-ttu-id="853b2-284">hello legrövidebb támogatott mintavételi gyakoriság: 15 másodperc (PT15S).</span><span class="sxs-lookup"><span data-stu-id="853b2-284">hello shortest supported sample rate is 15 seconds (PT15S).</span></span>
<span data-ttu-id="853b2-285">egység</span><span class="sxs-lookup"><span data-stu-id="853b2-285">unit</span></span> | <span data-ttu-id="853b2-286">Ezek a karakterláncok egyike lehet: "Count", "Memória", "S", "Százaléka", "CountPerSecond", "BytesPerSecond", "Ezredmásodperces".</span><span class="sxs-lookup"><span data-stu-id="853b2-286">Should be one of these strings: "Count", "Bytes", "Seconds", "Percent", "CountPerSecond", "BytesPerSecond", "Millisecond".</span></span> <span data-ttu-id="853b2-287">Hello egység hello mérőszám határozza meg.</span><span class="sxs-lookup"><span data-stu-id="853b2-287">Defines hello unit for hello metric.</span></span> <span data-ttu-id="853b2-288">Hello gyűjtött adatok fogyasztók várt hello gyűjtött adatok értékek toomatch a egység.</span><span class="sxs-lookup"><span data-stu-id="853b2-288">Consumers of hello collected data expect hello collected data values toomatch this unit.</span></span> <span data-ttu-id="853b2-289">LAD figyelmen kívül hagyja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="853b2-289">LAD ignores this field.</span></span>
<span data-ttu-id="853b2-290">displayName</span><span class="sxs-lookup"><span data-stu-id="853b2-290">displayName</span></span> | <span data-ttu-id="853b2-291">hello (nyelven hello hello társított területi beállítást által megadott) címke toobe csatolt Azure metrikák toothis adatokat.</span><span class="sxs-lookup"><span data-stu-id="853b2-291">hello label (in hello language specified by hello associated locale setting) toobe attached toothis data in Azure Metrics.</span></span> <span data-ttu-id="853b2-292">LAD figyelmen kívül hagyja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="853b2-292">LAD ignores this field.</span></span>

<span data-ttu-id="853b2-293">hello counterSpecifier tetszőleges azonosító, amely.</span><span class="sxs-lookup"><span data-stu-id="853b2-293">hello counterSpecifier is an arbitrary identifier.</span></span> <span data-ttu-id="853b2-294">A mérőszámok, például az Azure portál diagramkészítési, és a szolgáltatás, riasztás hello fogyasztók hello "kulcsot" azonosító a metrika vagy metrika példányának counterSpecifier használja.</span><span class="sxs-lookup"><span data-stu-id="853b2-294">Consumers of metrics, like hello Azure portal charting and alerting feature, use counterSpecifier as hello "key" that identifies a metric or an instance of a metric.</span></span> <span data-ttu-id="853b2-295">A `builtin` metrika, ajánlott counterSpecifier értékek kezdődő `/builtin/`.</span><span class="sxs-lookup"><span data-stu-id="853b2-295">For `builtin` metrics, we recommend you use counterSpecifier values that begin with `/builtin/`.</span></span> <span data-ttu-id="853b2-296">Gyűjti a metrika egy adott példányához, azt javasoljuk csatlakoztatása hello toohello counterSpecifier értékének hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="853b2-296">If you are collecting a specific instance of a metric, we recommend you attach hello identifier of hello instance toohello counterSpecifier value.</span></span> <span data-ttu-id="853b2-297">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="853b2-297">Some examples:</span></span>

* <span data-ttu-id="853b2-298">`/builtin/Processor/PercentIdleTime`-Az összes mag között átlagosan üresjárati idő</span><span class="sxs-lookup"><span data-stu-id="853b2-298">`/builtin/Processor/PercentIdleTime` - Idle time averaged across all cores</span></span>
* <span data-ttu-id="853b2-299">`/builtin/Disk/FreeSpace(/mnt)`– Hello /mnt FileSystem szabad terület</span><span class="sxs-lookup"><span data-stu-id="853b2-299">`/builtin/Disk/FreeSpace(/mnt)` - Free space for hello /mnt filesystem</span></span>
* <span data-ttu-id="853b2-300">`/builtin/Disk/FreeSpace`– Az összes csatlakoztatott fájlrendszerek között átlagosan szabad terület</span><span class="sxs-lookup"><span data-stu-id="853b2-300">`/builtin/Disk/FreeSpace` - Free space averaged across all mounted filesystems</span></span>

<span data-ttu-id="853b2-301">LAD sem hello Azure-portálon vár hello counterSpecifier érték toomatch bármely mintát.</span><span class="sxs-lookup"><span data-stu-id="853b2-301">Neither LAD nor hello Azure portal expects hello counterSpecifier value toomatch any pattern.</span></span> <span data-ttu-id="853b2-302">Hogyan hozható létre counterSpecifier értékek a kell.</span><span class="sxs-lookup"><span data-stu-id="853b2-302">Be consistent in how you construct counterSpecifier values.</span></span>

<span data-ttu-id="853b2-303">Ha a `performanceCounters`, LAD tooa adattábla mindig ír az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="853b2-303">When you specify `performanceCounters`, LAD always writes data tooa table in Azure storage.</span></span> <span data-ttu-id="853b2-304">Akkor is rendelkezik hello azonos tooJSON blobokat és/vagy az Event Hubs írt adatokat, de tárolni tooa adattábla nem tiltható le.</span><span class="sxs-lookup"><span data-stu-id="853b2-304">You can have hello same data written tooJSON blobs and/or Event Hubs, but you cannot disable storing data tooa table.</span></span> <span data-ttu-id="853b2-305">Hello konfigurált diagnosztikai bővítmény toouse összes példánya ugyanazt a tárfiókot, nevét és a végpont hozzáadása a metrikák és a naplók toohello hello ugyanabban a táblában.</span><span class="sxs-lookup"><span data-stu-id="853b2-305">All instances of hello diagnostic extension configured toouse hello same storage account name and endpoint add their metrics and logs toohello same table.</span></span> <span data-ttu-id="853b2-306">Ha túl sok virtuális gép írás képes szabályozni a tábla partícióra, Azure toohello ír toothat partíció.</span><span class="sxs-lookup"><span data-stu-id="853b2-306">If too many VMs are writing toohello same table partition, Azure can throttle writes toothat partition.</span></span> <span data-ttu-id="853b2-307">hello eventVolume okok bejegyzések toobe beállítása 1 (kicsi), 10 (közepes) vagy 100 (nagy) különböző partíciók elosztva.</span><span class="sxs-lookup"><span data-stu-id="853b2-307">hello eventVolume setting causes entries toobe spread across 1 (Small), 10 (Medium), or 100 (Large) different partitions.</span></span> <span data-ttu-id="853b2-308">Általában, "Közepes" is elegendő tooensure forgalom nem folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="853b2-308">Usually, "Medium" is sufficient tooensure traffic is not throttled.</span></span> <span data-ttu-id="853b2-309">hello Azure metrikák szolgáltatása hello Azure-portálon a tábla tooproduce diagramjait vagy tootrigger riasztások hello adatait használja.</span><span class="sxs-lookup"><span data-stu-id="853b2-309">hello Azure Metrics feature of hello Azure portal uses hello data in this table tooproduce graphs or tootrigger alerts.</span></span> <span data-ttu-id="853b2-310">Ezek a karakterláncok hello összefűzése kell hello nevét:</span><span class="sxs-lookup"><span data-stu-id="853b2-310">hello table name is hello concatenation of these strings:</span></span>

* `WADMetrics`
* <span data-ttu-id="853b2-311">hello "scheduledTransferPeriod" hello összesítve hello táblában tárolt értékek</span><span class="sxs-lookup"><span data-stu-id="853b2-311">hello "scheduledTransferPeriod" for hello aggregated values stored in hello table</span></span>
* `P10DV2S`
* <span data-ttu-id="853b2-312">Hello űrlap "ÉÉÉÉHHNN", amely megváltoztatja minden 10 nap, dátum</span><span class="sxs-lookup"><span data-stu-id="853b2-312">A date, in hello form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="853b2-313">Példák `WADMetricsPT1HP10DV2S20170410` és `WADMetricsPT1MP10DV2S20170609`.</span><span class="sxs-lookup"><span data-stu-id="853b2-313">Examples include `WADMetricsPT1HP10DV2S20170410` and `WADMetricsPT1MP10DV2S20170609`.</span></span>

#### <a name="syslogevents"></a><span data-ttu-id="853b2-314">syslogEvents</span><span class="sxs-lookup"><span data-stu-id="853b2-314">syslogEvents</span></span>

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

<span data-ttu-id="853b2-315">Ez a szakasz választható határozza meg a syslog naplóeseményeket hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="853b2-315">This optional section controls hello collection of log events from syslog.</span></span> <span data-ttu-id="853b2-316">Hello szakasz elhagyása esetén syslog-események nem minden rögzíti.</span><span class="sxs-lookup"><span data-stu-id="853b2-316">If hello section is omitted, syslog events are not captured at all.</span></span>

<span data-ttu-id="853b2-317">hello syslogEventConfiguration gyűjteményben szerepel egy bejegyzés minden egyes csomópontjára syslog-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="853b2-317">hello syslogEventConfiguration collection has one entry for each syslog facility of interest.</span></span> <span data-ttu-id="853b2-318">Ha minSeverity "NONE" az egy adott létesítményt a személyes, vagy az, hogy a létesítmény nem jelenik meg hello elem minden, az, hogy a létesítmény események nem lesznek rögzítve.</span><span class="sxs-lookup"><span data-stu-id="853b2-318">If minSeverity is "NONE" for a particular facility, or if that facility does not appear in hello element at all, no events from that facility are captured.</span></span>

<span data-ttu-id="853b2-319">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-319">Element</span></span> | <span data-ttu-id="853b2-320">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-320">Value</span></span>
------- | -----
<span data-ttu-id="853b2-321">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="853b2-321">sinks</span></span> | <span data-ttu-id="853b2-322">A neveket vesszővel tagolt listája toowhich egyedi aktiválási naplót esemény közzé lesz téve fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="853b2-322">A comma-separated list of names of sinks toowhich individual log events are published.</span></span> <span data-ttu-id="853b2-323">Hello korlátozások syslogEventConfiguration a megfelelő összes naplózási eseményt felsorolt közzétett tooeach fogadó.</span><span class="sxs-lookup"><span data-stu-id="853b2-323">All log events matching hello restrictions in syslogEventConfiguration are published tooeach listed sink.</span></span> <span data-ttu-id="853b2-324">Példa: "EHforsyslog"</span><span class="sxs-lookup"><span data-stu-id="853b2-324">Example: "EHforsyslog"</span></span>
<span data-ttu-id="853b2-325">facilityName</span><span class="sxs-lookup"><span data-stu-id="853b2-325">facilityName</span></span> | <span data-ttu-id="853b2-326">A syslog létesítmény nevét (például a "napló\_felhasználói" vagy "napló\_LOCAL0").</span><span class="sxs-lookup"><span data-stu-id="853b2-326">A syslog facility name (such as "LOG\_USER" or "LOG\_LOCAL0").</span></span> <span data-ttu-id="853b2-327">Című rész hello "létesítmény" hello [syslog man lap](http://man7.org/linux/man-pages/man3/syslog.3.html) hello teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="853b2-327">See hello "facility" section of hello [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for hello full list.</span></span>
<span data-ttu-id="853b2-328">minSeverity</span><span class="sxs-lookup"><span data-stu-id="853b2-328">minSeverity</span></span> | <span data-ttu-id="853b2-329">A syslog súlyossági szint (például a "napló\_hiba" vagy "napló\_INFO").</span><span class="sxs-lookup"><span data-stu-id="853b2-329">A syslog severity level (such as "LOG\_ERR" or "LOG\_INFO").</span></span> <span data-ttu-id="853b2-330">Című rész hello "szint" hello [syslog man lap](http://man7.org/linux/man-pages/man3/syslog.3.html) hello teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="853b2-330">See hello "level" section of hello [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for hello full list.</span></span> <span data-ttu-id="853b2-331">hello kiterjesztés küldött események toohello intézmény rögzíti, vagy a fenti hello megadott szint.</span><span class="sxs-lookup"><span data-stu-id="853b2-331">hello extension captures events sent toohello facility at or above hello specified level.</span></span>

<span data-ttu-id="853b2-332">Ha a `syslogEvents`, LAD tooa adattábla mindig ír az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="853b2-332">When you specify `syslogEvents`, LAD always writes data tooa table in Azure storage.</span></span> <span data-ttu-id="853b2-333">Akkor is rendelkezik hello azonos tooJSON blobokat és/vagy az Event Hubs írt adatokat, de tárolni tooa adattábla nem tiltható le.</span><span class="sxs-lookup"><span data-stu-id="853b2-333">You can have hello same data written tooJSON blobs and/or Event Hubs, but you cannot disable storing data tooa table.</span></span> <span data-ttu-id="853b2-334">Ez a táblázat viselkedése particionálás hello van hello megegyeznek a `performanceCounters`.</span><span class="sxs-lookup"><span data-stu-id="853b2-334">hello partitioning behavior for this table is hello same as described for `performanceCounters`.</span></span> <span data-ttu-id="853b2-335">Ezek a karakterláncok hello összefűzése kell hello nevét:</span><span class="sxs-lookup"><span data-stu-id="853b2-335">hello table name is hello concatenation of these strings:</span></span>

* `LinuxSyslog`
* <span data-ttu-id="853b2-336">Hello űrlap "ÉÉÉÉHHNN", amely megváltoztatja minden 10 nap, dátum</span><span class="sxs-lookup"><span data-stu-id="853b2-336">A date, in hello form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="853b2-337">Példák `LinuxSyslog20170410` és `LinuxSyslog20170609`.</span><span class="sxs-lookup"><span data-stu-id="853b2-337">Examples include `LinuxSyslog20170410` and `LinuxSyslog20170609`.</span></span>

### <a name="perfcfg"></a><span data-ttu-id="853b2-338">perfCfg</span><span class="sxs-lookup"><span data-stu-id="853b2-338">perfCfg</span></span>

<span data-ttu-id="853b2-339">Ez a szakasz választható szabályozza tetszőleges végrehajtásának [OMI](https://github.com/Microsoft/omi) lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="853b2-339">This optional section controls execution of arbitrary [OMI](https://github.com/Microsoft/omi) queries.</span></span>

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

<span data-ttu-id="853b2-340">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-340">Element</span></span> | <span data-ttu-id="853b2-341">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-341">Value</span></span>
------- | -----
<span data-ttu-id="853b2-342">Namespace</span><span class="sxs-lookup"><span data-stu-id="853b2-342">namespace</span></span> | <span data-ttu-id="853b2-343">(választható) hello OMI névtér melyik hello belül lekérdezés kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="853b2-343">(optional) hello OMI namespace within which hello query should be executed.</span></span> <span data-ttu-id="853b2-344">Ha nincs megadva, hello alapértelmezett értéke "legfelső szintű/scx", hello által megvalósított [a System Center platformfüggetlen szolgáltatók](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span><span class="sxs-lookup"><span data-stu-id="853b2-344">If unspecified, hello default value is "root/scx", implemented by hello [System Center Cross-platform Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span></span>
<span data-ttu-id="853b2-345">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="853b2-345">query</span></span> | <span data-ttu-id="853b2-346">hello OMI lekérdezés toobe végre.</span><span class="sxs-lookup"><span data-stu-id="853b2-346">hello OMI query toobe executed.</span></span>
<span data-ttu-id="853b2-347">Tábla</span><span class="sxs-lookup"><span data-stu-id="853b2-347">table</span></span> | <span data-ttu-id="853b2-348">(választható) hello az Azure storage tábla, a kijelölt tárfiókkal hello (lásd: [beállítások védett](#protected-settings)).</span><span class="sxs-lookup"><span data-stu-id="853b2-348">(optional) hello Azure storage table, in hello designated storage account (see [Protected settings](#protected-settings)).</span></span>
<span data-ttu-id="853b2-349">frequency</span><span class="sxs-lookup"><span data-stu-id="853b2-349">frequency</span></span> | <span data-ttu-id="853b2-350">hello lekérdezés végrehajtása között eltelt másodpercek számát (nem kötelező) hello.</span><span class="sxs-lookup"><span data-stu-id="853b2-350">(optional) hello number of seconds between execution of hello query.</span></span> <span data-ttu-id="853b2-351">Alapértelmezett értéke 300 (5 percig); minimális érték 15 másodpercre.</span><span class="sxs-lookup"><span data-stu-id="853b2-351">Default value is 300 (5 minutes); minimum value is 15 seconds.</span></span>
<span data-ttu-id="853b2-352">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="853b2-352">sinks</span></span> | <span data-ttu-id="853b2-353">(választható) További mosdók toowhich nyers minta metrika eredmények neveket vesszővel tagolt listája közzé kell tenni.</span><span class="sxs-lookup"><span data-stu-id="853b2-353">(optional) A comma-separated list of names of additional sinks toowhich raw sample metric results should be published.</span></span> <span data-ttu-id="853b2-354">Nincs összesítési e nyers minták számított hello bővítmény vagy Azure metrikákat.</span><span class="sxs-lookup"><span data-stu-id="853b2-354">No aggregation of these raw samples is computed by hello extension or by Azure Metrics.</span></span>

<span data-ttu-id="853b2-355">"Table" vagy "fogadók esetében", vagy mindkettőt, meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="853b2-355">Either "table" or "sinks", or both, must be specified.</span></span>

### <a name="filelogs"></a><span data-ttu-id="853b2-356">fileLogs</span><span class="sxs-lookup"><span data-stu-id="853b2-356">fileLogs</span></span>

<span data-ttu-id="853b2-357">Vezérlők hello rögzítése a naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="853b2-357">Controls hello capture of log files.</span></span> <span data-ttu-id="853b2-358">LAD új szöveges sort rögzíti, az oktatóprogram toohello fájl, és írja őket az tootable sorok és/vagy a megadott mosdók (JsonBlob vagy EventHub).</span><span class="sxs-lookup"><span data-stu-id="853b2-358">LAD captures new text lines as they are written toohello file and writes them tootable rows and/or any specified sinks (JsonBlob or EventHub).</span></span>

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

<span data-ttu-id="853b2-359">Elem</span><span class="sxs-lookup"><span data-stu-id="853b2-359">Element</span></span> | <span data-ttu-id="853b2-360">Érték</span><span class="sxs-lookup"><span data-stu-id="853b2-360">Value</span></span>
------- | -----
<span data-ttu-id="853b2-361">Fájl</span><span class="sxs-lookup"><span data-stu-id="853b2-361">file</span></span> | <span data-ttu-id="853b2-362">hello teljes elérési útja hello napló fájl toobe figyelése, és rögzített.</span><span class="sxs-lookup"><span data-stu-id="853b2-362">hello full pathname of hello log file toobe watched and captured.</span></span> <span data-ttu-id="853b2-363">hello pathname nevet egyetlen fájl; nem egy könyvtár nevet és nem tartalmazhat helyettesítő karaktereket.</span><span class="sxs-lookup"><span data-stu-id="853b2-363">hello pathname must name a single file; it cannot name a directory or contain wildcards.</span></span>
<span data-ttu-id="853b2-364">Tábla</span><span class="sxs-lookup"><span data-stu-id="853b2-364">table</span></span> | <span data-ttu-id="853b2-365">(választható) hello az Azure storage tábla kijelölt hello Storage (ahogy a védett hello configuration), az új sorok hello "kisebb" hello fájl íródtak a fiókot.</span><span class="sxs-lookup"><span data-stu-id="853b2-365">(optional) hello Azure storage table, in hello designated storage account (as specified in hello protected configuration), into which new lines from hello "tail" of hello file are written.</span></span>
<span data-ttu-id="853b2-366">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="853b2-366">sinks</span></span> | <span data-ttu-id="853b2-367">(választható) Küldött neve további mosdók toowhich napló vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="853b2-367">(optional) A comma-separated list of names of additional sinks toowhich log lines sent.</span></span>

<span data-ttu-id="853b2-368">"Table" vagy "fogadók esetében", vagy mindkettőt, meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="853b2-368">Either "table" or "sinks", or both, must be specified.</span></span>

## <a name="metrics-supported-by-hello-builtin-provider"></a><span data-ttu-id="853b2-369">Hello beépített szolgáltató támogatja a mérőszámok</span><span class="sxs-lookup"><span data-stu-id="853b2-369">Metrics supported by hello builtin provider</span></span>

<span data-ttu-id="853b2-370">hello beépített metrika szolgáltató metrikákat a legérdekesebb tooa széles körét felhasználók forrásaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="853b2-370">hello builtin metric provider is a source of metrics most interesting tooa broad set of users.</span></span> <span data-ttu-id="853b2-371">A metrikák öt széleskörű osztályok sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="853b2-371">These metrics fall into five broad classes:</span></span>

* <span data-ttu-id="853b2-372">Processzor</span><span class="sxs-lookup"><span data-stu-id="853b2-372">Processor</span></span>
* <span data-ttu-id="853b2-373">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="853b2-373">Memory</span></span>
* <span data-ttu-id="853b2-374">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="853b2-374">Network</span></span>
* <span data-ttu-id="853b2-375">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="853b2-375">Filesystem</span></span>
* <span data-ttu-id="853b2-376">Lemez</span><span class="sxs-lookup"><span data-stu-id="853b2-376">Disk</span></span>

### <a name="builtin-metrics-for-hello-processor-class"></a><span data-ttu-id="853b2-377">a beépített metrikáját hello processzor osztály</span><span class="sxs-lookup"><span data-stu-id="853b2-377">builtin metrics for hello Processor class</span></span>

<span data-ttu-id="853b2-378">hello processzor osztály a mérőszámok tájékoztatást ad azokról a virtuális gép hello processzor kihasználtsága.</span><span class="sxs-lookup"><span data-stu-id="853b2-378">hello Processor class of metrics provides information about processor usage in hello VM.</span></span> <span data-ttu-id="853b2-379">Százalékos összesítésekor hello eredménye hello átlagos összes processzorok között.</span><span class="sxs-lookup"><span data-stu-id="853b2-379">When aggregating percentages, hello result is hello average across all CPUs.</span></span> <span data-ttu-id="853b2-380">A két fő virtuális gép egy alapvető 100 %-os elfoglalt volt, és más hello 100 %-os üresjárati, hello jelez-e PercentIdleTime pedig 50.</span><span class="sxs-lookup"><span data-stu-id="853b2-380">In a two core VM, if one core was 100% busy and hello other was 100% idle, hello reported PercentIdleTime would be 50.</span></span> <span data-ttu-id="853b2-381">Ha minden core 50 % foglalt volt hello azonos az az időszak hello jelentett eredmény is pedig 50.</span><span class="sxs-lookup"><span data-stu-id="853b2-381">If each core was 50% busy for hello same period, hello reported result would also be 50.</span></span> <span data-ttu-id="853b2-382">Egy négy alapvető virtuális gép, egy alapvető 100 % foglalt és hello üresjárati, mások hello jelentett PercentIdleTime 75 lenne.</span><span class="sxs-lookup"><span data-stu-id="853b2-382">In a four core VM, with one core 100% busy and hello others idle, hello reported PercentIdleTime would be 75.</span></span>

<span data-ttu-id="853b2-383">A számláló</span><span class="sxs-lookup"><span data-stu-id="853b2-383">counter</span></span> | <span data-ttu-id="853b2-384">Jelentése</span><span class="sxs-lookup"><span data-stu-id="853b2-384">Meaning</span></span>
------- | -------
<span data-ttu-id="853b2-385">PercentIdleTime</span><span class="sxs-lookup"><span data-stu-id="853b2-385">PercentIdleTime</span></span> | <span data-ttu-id="853b2-386">Hogy a processzorok volt végrehajtása hello kernel üresjárati hurok hello összesítési időszakban idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-386">Percentage of time during hello aggregation window that processors were executing hello kernel idle loop</span></span>
<span data-ttu-id="853b2-387">PercentProcessorTime</span><span class="sxs-lookup"><span data-stu-id="853b2-387">PercentProcessorTime</span></span> | <span data-ttu-id="853b2-388">Nem üresjárati szálat idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-388">Percentage of time executing a non-idle thread</span></span>
<span data-ttu-id="853b2-389">PercentIOWaitTime</span><span class="sxs-lookup"><span data-stu-id="853b2-389">PercentIOWaitTime</span></span> | <span data-ttu-id="853b2-390">Várakozás az I/O műveletek toocomplete idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-390">Percentage of time waiting for IO operations toocomplete</span></span>
<span data-ttu-id="853b2-391">PercentInterruptTime</span><span class="sxs-lookup"><span data-stu-id="853b2-391">PercentInterruptTime</span></span> | <span data-ttu-id="853b2-392">Hardver vagy szoftver megszakítások, DPC-k (késleltetett eljáráshívások) végrehajtása idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-392">Percentage of time executing hardware/software interrupts and DPCs (deferred procedure calls)</span></span>
<span data-ttu-id="853b2-393">PercentUserTime</span><span class="sxs-lookup"><span data-stu-id="853b2-393">PercentUserTime</span></span> | <span data-ttu-id="853b2-394">Hello összesítési időszak alatt nem üresjárati idő hello fordított idő százalékos aránya a felhasználó több normál prioritással</span><span class="sxs-lookup"><span data-stu-id="853b2-394">Of non-idle time during hello aggregation window, hello percentage of time spent in user more at normal priority</span></span>
<span data-ttu-id="853b2-395">PercentNiceTime</span><span class="sxs-lookup"><span data-stu-id="853b2-395">PercentNiceTime</span></span> | <span data-ttu-id="853b2-396">Nem üresjárati idő hello süllyesztett (jó) prioritással töltött százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-396">Of non-idle time, hello percentage spent at lowered (nice) priority</span></span>
<span data-ttu-id="853b2-397">PercentPrivilegedTime</span><span class="sxs-lookup"><span data-stu-id="853b2-397">PercentPrivilegedTime</span></span> | <span data-ttu-id="853b2-398">Nem üresjárati idő hello védett (kernel) módban töltött százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-398">Of non-idle time, hello percentage spent in privileged (kernel) mode</span></span>

<span data-ttu-id="853b2-399">hello első négy számlálók kell összeg too100 %.</span><span class="sxs-lookup"><span data-stu-id="853b2-399">hello first four counters should sum too100%.</span></span> <span data-ttu-id="853b2-400">hello utolsó három teljesítményszámlálók is sum too100 %; Ezek tovább PercentProcessorTime PercentIOWaitTime és PercentInterruptTime hello összege.</span><span class="sxs-lookup"><span data-stu-id="853b2-400">hello last three counters also sum too100%; they subdivide hello sum of PercentProcessorTime, PercentIOWaitTime, and PercentInterruptTime.</span></span>

<span data-ttu-id="853b2-401">egyetlen mérőszám az összes processzor gyűjtődnek tooobtain beállítása `"condition": "IsAggregate=TRUE"`.</span><span class="sxs-lookup"><span data-stu-id="853b2-401">tooobtain a single metric aggregated across all processors, set `"condition": "IsAggregate=TRUE"`.</span></span> <span data-ttu-id="853b2-402">megadott processzorsebességgel rendelkező metrika tooobtain hello második logikai processzorral egy négy alapvető VM, mint például beállítása `"condition": "Name=\\"1\\""`.</span><span class="sxs-lookup"><span data-stu-id="853b2-402">tooobtain a metric for a specific processor, such as hello second logical processor of a four core VM, set `"condition": "Name=\\"1\\""`.</span></span> <span data-ttu-id="853b2-403">Logikai processzor számok hello tartományban vannak `[0..n-1]`.</span><span class="sxs-lookup"><span data-stu-id="853b2-403">Logical processor numbers are in hello range `[0..n-1]`.</span></span>

### <a name="builtin-metrics-for-hello-memory-class"></a><span data-ttu-id="853b2-404">a beépített metrikáját hello memória osztály</span><span class="sxs-lookup"><span data-stu-id="853b2-404">builtin metrics for hello Memory class</span></span>

<span data-ttu-id="853b2-405">hello memória osztály a mérőszámok memóriafelhasználás a lapozást, és áttelepíteni a forráskörnyezetból információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="853b2-405">hello Memory class of metrics provides information about memory utilization, paging, and swapping.</span></span>

<span data-ttu-id="853b2-406">A számláló</span><span class="sxs-lookup"><span data-stu-id="853b2-406">counter</span></span> | <span data-ttu-id="853b2-407">Jelentése</span><span class="sxs-lookup"><span data-stu-id="853b2-407">Meaning</span></span>
------- | -------
<span data-ttu-id="853b2-408">AvailableMemory</span><span class="sxs-lookup"><span data-stu-id="853b2-408">AvailableMemory</span></span> | <span data-ttu-id="853b2-409">Rendelkezésre álló fizikai memória MIB</span><span class="sxs-lookup"><span data-stu-id="853b2-409">Available physical memory in MiB</span></span>
<span data-ttu-id="853b2-410">PercentAvailableMemory</span><span class="sxs-lookup"><span data-stu-id="853b2-410">PercentAvailableMemory</span></span> | <span data-ttu-id="853b2-411">A teljes memória százalékos rendelkezésre álló fizikai memória</span><span class="sxs-lookup"><span data-stu-id="853b2-411">Available physical memory as a percent of total memory</span></span>
<span data-ttu-id="853b2-412">UsedMemory</span><span class="sxs-lookup"><span data-stu-id="853b2-412">UsedMemory</span></span> | <span data-ttu-id="853b2-413">Használatban lévő fizikai memória (MiB)</span><span class="sxs-lookup"><span data-stu-id="853b2-413">In-use physical memory (MiB)</span></span>
<span data-ttu-id="853b2-414">PercentUsedMemory</span><span class="sxs-lookup"><span data-stu-id="853b2-414">PercentUsedMemory</span></span> | <span data-ttu-id="853b2-415">A teljes memória százalékos a használatban lévő fizikai memória</span><span class="sxs-lookup"><span data-stu-id="853b2-415">In-use physical memory as a percent of total memory</span></span>
<span data-ttu-id="853b2-416">PagesPerSec</span><span class="sxs-lookup"><span data-stu-id="853b2-416">PagesPerSec</span></span> | <span data-ttu-id="853b2-417">Teljes lapozófájl (olvasás/írás)</span><span class="sxs-lookup"><span data-stu-id="853b2-417">Total paging (read/write)</span></span>
<span data-ttu-id="853b2-418">PagesReadPerSec</span><span class="sxs-lookup"><span data-stu-id="853b2-418">PagesReadPerSec</span></span> | <span data-ttu-id="853b2-419">Lapok olvasni háttértár (lapozófájl programfájlt, leképezett fájlt, stb.)</span><span class="sxs-lookup"><span data-stu-id="853b2-419">Pages read from backing store (swap file, program file, mapped file, etc.)</span></span>
<span data-ttu-id="853b2-420">PagesWrittenPerSec</span><span class="sxs-lookup"><span data-stu-id="853b2-420">PagesWrittenPerSec</span></span> | <span data-ttu-id="853b2-421">Toobacking írt lapok tárolja (lapozófájl, leképezett fájlt, stb.)</span><span class="sxs-lookup"><span data-stu-id="853b2-421">Pages written toobacking store (swap file, mapped file, etc.)</span></span>
<span data-ttu-id="853b2-422">AvailableSwap</span><span class="sxs-lookup"><span data-stu-id="853b2-422">AvailableSwap</span></span> | <span data-ttu-id="853b2-423">Nem használt lapozóterület (MiB)</span><span class="sxs-lookup"><span data-stu-id="853b2-423">Unused swap space (MiB)</span></span>
<span data-ttu-id="853b2-424">PercentAvailableSwap</span><span class="sxs-lookup"><span data-stu-id="853b2-424">PercentAvailableSwap</span></span> | <span data-ttu-id="853b2-425">Nem használt lapozóterület teljes lapozófájl-kapacitás százalékában</span><span class="sxs-lookup"><span data-stu-id="853b2-425">Unused swap space as a percentage of total swap</span></span>
<span data-ttu-id="853b2-426">UsedSwap</span><span class="sxs-lookup"><span data-stu-id="853b2-426">UsedSwap</span></span> | <span data-ttu-id="853b2-427">Használatban lévő lapozóterület (MiB)</span><span class="sxs-lookup"><span data-stu-id="853b2-427">In-use swap space (MiB)</span></span>
<span data-ttu-id="853b2-428">PercentUsedSwap</span><span class="sxs-lookup"><span data-stu-id="853b2-428">PercentUsedSwap</span></span> | <span data-ttu-id="853b2-429">Használatban lévő lapozóterület teljes lapozófájl-kapacitás százalékában</span><span class="sxs-lookup"><span data-stu-id="853b2-429">In-use swap space as a percentage of total swap</span></span>

<span data-ttu-id="853b2-430">Ez az osztály a mérőszámok csak egyetlen példány van.</span><span class="sxs-lookup"><span data-stu-id="853b2-430">This class of metrics has only a single instance.</span></span> <span data-ttu-id="853b2-431">hello "feltétel" attribútum nem hasznos beállításokkal rendelkezik, és megadni.</span><span class="sxs-lookup"><span data-stu-id="853b2-431">hello "condition" attribute has no useful settings and should be omitted.</span></span>

### <a name="builtin-metrics-for-hello-network-class"></a><span data-ttu-id="853b2-432">a beépített metrikáját hello hálózati osztály</span><span class="sxs-lookup"><span data-stu-id="853b2-432">builtin metrics for hello Network class</span></span>

<span data-ttu-id="853b2-433">hello hálózati osztály a mérőszámok tevékenységről szolgáltat információkat hálózati az egyes hálózati adaptereken indítása óta.</span><span class="sxs-lookup"><span data-stu-id="853b2-433">hello Network class of metrics provides information about network activity on an individual network interfaces since boot.</span></span> <span data-ttu-id="853b2-434">LAD nem biztosít sávszélesség metrikákat, amelyek a gazdagép-metrikák kérhető.</span><span class="sxs-lookup"><span data-stu-id="853b2-434">LAD does not expose bandwidth metrics, which can be retrieved from host metrics.</span></span>

<span data-ttu-id="853b2-435">A számláló</span><span class="sxs-lookup"><span data-stu-id="853b2-435">counter</span></span> | <span data-ttu-id="853b2-436">Jelentése</span><span class="sxs-lookup"><span data-stu-id="853b2-436">Meaning</span></span>
------- | -------
<span data-ttu-id="853b2-437">BytesTransmitted</span><span class="sxs-lookup"><span data-stu-id="853b2-437">BytesTransmitted</span></span> | <span data-ttu-id="853b2-438">Rendszerindítás óta küldött bájtok száma összesen</span><span class="sxs-lookup"><span data-stu-id="853b2-438">Total bytes sent since boot</span></span>
<span data-ttu-id="853b2-439">BytesReceived</span><span class="sxs-lookup"><span data-stu-id="853b2-439">BytesReceived</span></span> | <span data-ttu-id="853b2-440">Rendszerindítás óta fogadott összes bájt</span><span class="sxs-lookup"><span data-stu-id="853b2-440">Total bytes received since boot</span></span>
<span data-ttu-id="853b2-441">BytesTotal</span><span class="sxs-lookup"><span data-stu-id="853b2-441">BytesTotal</span></span> | <span data-ttu-id="853b2-442">Küldött vagy indítása óta fogadott bájtok teljes száma</span><span class="sxs-lookup"><span data-stu-id="853b2-442">Total bytes sent or received since boot</span></span>
<span data-ttu-id="853b2-443">PacketsTransmitted</span><span class="sxs-lookup"><span data-stu-id="853b2-443">PacketsTransmitted</span></span> | <span data-ttu-id="853b2-444">Rendszerindítás óta küldött csomagok száma összesen</span><span class="sxs-lookup"><span data-stu-id="853b2-444">Total packets sent since boot</span></span>
<span data-ttu-id="853b2-445">PacketsReceived</span><span class="sxs-lookup"><span data-stu-id="853b2-445">PacketsReceived</span></span> | <span data-ttu-id="853b2-446">Rendszerindítás óta fogadott csomagok száma összesen</span><span class="sxs-lookup"><span data-stu-id="853b2-446">Total packets received since boot</span></span>
<span data-ttu-id="853b2-447">TotalRxErrors</span><span class="sxs-lookup"><span data-stu-id="853b2-447">TotalRxErrors</span></span> | <span data-ttu-id="853b2-448">A fogadási hibák száma indítása óta</span><span class="sxs-lookup"><span data-stu-id="853b2-448">Number of receive errors since boot</span></span>
<span data-ttu-id="853b2-449">TotalTxErrors</span><span class="sxs-lookup"><span data-stu-id="853b2-449">TotalTxErrors</span></span> | <span data-ttu-id="853b2-450">-Küldési hibák száma indítása óta</span><span class="sxs-lookup"><span data-stu-id="853b2-450">Number of transmit errors since boot</span></span>
<span data-ttu-id="853b2-451">TotalCollisions</span><span class="sxs-lookup"><span data-stu-id="853b2-451">TotalCollisions</span></span> | <span data-ttu-id="853b2-452">Rendszerindítás óta hello hálózati portok által jelentett ütközések száma</span><span class="sxs-lookup"><span data-stu-id="853b2-452">Number of collisions reported by hello network ports since boot</span></span>

 <span data-ttu-id="853b2-453">Bár ez az osztály van instanced, LAD nem támogatja az összes hálózati eszköz gyűjtődnek rögzítésével hálózati metrikákat.</span><span class="sxs-lookup"><span data-stu-id="853b2-453">Although this class is instanced, LAD does not support capturing Network metrics aggregated across all network devices.</span></span> <span data-ttu-id="853b2-454">egy adott illesztő eth0, például metrikáját tooobtain hello beállítása `"condition": "InstanceID=\\"eth0\\""`.</span><span class="sxs-lookup"><span data-stu-id="853b2-454">tooobtain hello metrics for a specific interface, such as eth0, set `"condition": "InstanceID=\\"eth0\\""`.</span></span>

### <a name="builtin-metrics-for-hello-filesystem-class"></a><span data-ttu-id="853b2-455">a beépített metrikáját hello Filesystem osztály</span><span class="sxs-lookup"><span data-stu-id="853b2-455">builtin metrics for hello Filesystem class</span></span>

<span data-ttu-id="853b2-456">hello Filesystem osztály a mérőszámok filesystem használati információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="853b2-456">hello Filesystem class of metrics provides information about filesystem usage.</span></span> <span data-ttu-id="853b2-457">Abszolút és százalékos értéket jelentett, a megjelenő tooan a szokványos felhasználói (nem a legfelső szintű) lesznek.</span><span class="sxs-lookup"><span data-stu-id="853b2-457">Absolute and percentage values are reported as they'd be displayed tooan ordinary user (not root).</span></span>

<span data-ttu-id="853b2-458">A számláló</span><span class="sxs-lookup"><span data-stu-id="853b2-458">counter</span></span> | <span data-ttu-id="853b2-459">Jelentése</span><span class="sxs-lookup"><span data-stu-id="853b2-459">Meaning</span></span>
------- | -------
<span data-ttu-id="853b2-460">FreeSpace</span><span class="sxs-lookup"><span data-stu-id="853b2-460">FreeSpace</span></span> | <span data-ttu-id="853b2-461">Szabad lemezterület (bájt)</span><span class="sxs-lookup"><span data-stu-id="853b2-461">Available disk space in bytes</span></span>
<span data-ttu-id="853b2-462">UsedSpace</span><span class="sxs-lookup"><span data-stu-id="853b2-462">UsedSpace</span></span> | <span data-ttu-id="853b2-463">A felhasznált lemezterület (bájt)</span><span class="sxs-lookup"><span data-stu-id="853b2-463">Used disk space in bytes</span></span>
<span data-ttu-id="853b2-464">PercentFreeSpace</span><span class="sxs-lookup"><span data-stu-id="853b2-464">PercentFreeSpace</span></span> | <span data-ttu-id="853b2-465">Szabad terület százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-465">Percentage free space</span></span>
<span data-ttu-id="853b2-466">PercentUsedSpace</span><span class="sxs-lookup"><span data-stu-id="853b2-466">PercentUsedSpace</span></span> | <span data-ttu-id="853b2-467">A felhasznált terület százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="853b2-467">Percentage used space</span></span>
<span data-ttu-id="853b2-468">PercentFreeInodes</span><span class="sxs-lookup"><span data-stu-id="853b2-468">PercentFreeInodes</span></span> | <span data-ttu-id="853b2-469">Nem használt Inode-OK százaléka</span><span class="sxs-lookup"><span data-stu-id="853b2-469">Percentage of unused inodes</span></span>
<span data-ttu-id="853b2-470">PercentUsedInodes</span><span class="sxs-lookup"><span data-stu-id="853b2-470">PercentUsedInodes</span></span> | <span data-ttu-id="853b2-471">Összesítve közötti összes fájlrendszerek lefoglalt (használatban) Inode-OK százaléka</span><span class="sxs-lookup"><span data-stu-id="853b2-471">Percentage of allocated (in use) inodes summed across all filesystems</span></span>
<span data-ttu-id="853b2-472">BytesReadPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-472">BytesReadPerSecond</span></span> | <span data-ttu-id="853b2-473">Másodpercenként olvasott bájtok száma</span><span class="sxs-lookup"><span data-stu-id="853b2-473">Bytes read per second</span></span>
<span data-ttu-id="853b2-474">BytesWrittenPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-474">BytesWrittenPerSecond</span></span> | <span data-ttu-id="853b2-475">Másodpercenként írt bájtok</span><span class="sxs-lookup"><span data-stu-id="853b2-475">Bytes written per second</span></span>
<span data-ttu-id="853b2-476">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-476">BytesPerSecond</span></span> | <span data-ttu-id="853b2-477">Bájt nem írható és olvasható / másodperc</span><span class="sxs-lookup"><span data-stu-id="853b2-477">Bytes read or written per second</span></span>
<span data-ttu-id="853b2-478">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-478">ReadsPerSecond</span></span> | <span data-ttu-id="853b2-479">Olvasási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="853b2-479">Read operations per second</span></span>
<span data-ttu-id="853b2-480">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-480">WritesPerSecond</span></span> | <span data-ttu-id="853b2-481">Írási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="853b2-481">Write operations per second</span></span>
<span data-ttu-id="853b2-482">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-482">TransfersPerSecond</span></span> | <span data-ttu-id="853b2-483">Olvasási vagy írási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="853b2-483">Read or write operations per second</span></span>

<span data-ttu-id="853b2-484">Összesített értékeket fájlrendszerek keresztül érhető el úgy, hogy `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="853b2-484">Aggregated values across all file systems can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="853b2-485">Például egy adott csatlakoztatott fájlrendszer értékei "/ mnt", úgy, hogy szerezhetők `"condition": 'Name="/mnt"'`.</span><span class="sxs-lookup"><span data-stu-id="853b2-485">Values for a specific mounted file system, such as "/mnt", can be obtained by setting `"condition": 'Name="/mnt"'`.</span></span>

### <a name="builtin-metrics-for-hello-disk-class"></a><span data-ttu-id="853b2-486">a beépített metrikáját hello lemez osztály</span><span class="sxs-lookup"><span data-stu-id="853b2-486">builtin metrics for hello Disk class</span></span>

<span data-ttu-id="853b2-487">hello lemez osztály a mérőszámok eszköz lemezhasználati információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="853b2-487">hello Disk class of metrics provides information about disk device usage.</span></span> <span data-ttu-id="853b2-488">A statisztikai információk toohello teljes meghajtó alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="853b2-488">These statistics apply toohello entire drive.</span></span> <span data-ttu-id="853b2-489">Ha egy eszközön több fájlrendszereket, hello számlálók az eszköznek vannak, gyakorlatilag az összes gyűjtődnek.</span><span class="sxs-lookup"><span data-stu-id="853b2-489">If there are multiple file systems on a device, hello counters for that device are, effectively, aggregated across all of them.</span></span>

<span data-ttu-id="853b2-490">A számláló</span><span class="sxs-lookup"><span data-stu-id="853b2-490">counter</span></span> | <span data-ttu-id="853b2-491">Jelentése</span><span class="sxs-lookup"><span data-stu-id="853b2-491">Meaning</span></span>
------- | -------
<span data-ttu-id="853b2-492">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-492">ReadsPerSecond</span></span> | <span data-ttu-id="853b2-493">Olvasási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="853b2-493">Read operations per second</span></span>
<span data-ttu-id="853b2-494">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-494">WritesPerSecond</span></span> | <span data-ttu-id="853b2-495">Írási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="853b2-495">Write operations per second</span></span>
<span data-ttu-id="853b2-496">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-496">TransfersPerSecond</span></span> | <span data-ttu-id="853b2-497">Teljes műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="853b2-497">Total operations per second</span></span>
<span data-ttu-id="853b2-498">AverageReadTime</span><span class="sxs-lookup"><span data-stu-id="853b2-498">AverageReadTime</span></span> | <span data-ttu-id="853b2-499">Olvasási művelet átlagos másodpercben</span><span class="sxs-lookup"><span data-stu-id="853b2-499">Average seconds per read operation</span></span>
<span data-ttu-id="853b2-500">AverageWriteTime</span><span class="sxs-lookup"><span data-stu-id="853b2-500">AverageWriteTime</span></span> | <span data-ttu-id="853b2-501">Írási művelet átlagos másodpercben</span><span class="sxs-lookup"><span data-stu-id="853b2-501">Average seconds per write operation</span></span>
<span data-ttu-id="853b2-502">AverageTransferTime</span><span class="sxs-lookup"><span data-stu-id="853b2-502">AverageTransferTime</span></span> | <span data-ttu-id="853b2-503">Művelet átlagos másodpercben</span><span class="sxs-lookup"><span data-stu-id="853b2-503">Average seconds per operation</span></span>
<span data-ttu-id="853b2-504">AverageDiskQueueLength</span><span class="sxs-lookup"><span data-stu-id="853b2-504">AverageDiskQueueLength</span></span> | <span data-ttu-id="853b2-505">Várólistára helyezett lemezen műveletek átlagos száma</span><span class="sxs-lookup"><span data-stu-id="853b2-505">Average number of queued disk operations</span></span>
<span data-ttu-id="853b2-506">ReadBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-506">ReadBytesPerSecond</span></span> | <span data-ttu-id="853b2-507">A másodpercenként beolvasott bájtok száma</span><span class="sxs-lookup"><span data-stu-id="853b2-507">Number of bytes read per second</span></span>
<span data-ttu-id="853b2-508">WriteBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-508">WriteBytesPerSecond</span></span> | <span data-ttu-id="853b2-509">Másodpercenként írt bájtok száma</span><span class="sxs-lookup"><span data-stu-id="853b2-509">Number of bytes written per second</span></span>
<span data-ttu-id="853b2-510">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="853b2-510">BytesPerSecond</span></span> | <span data-ttu-id="853b2-511">Olvassa el és másodpercenként írt bájtok száma</span><span class="sxs-lookup"><span data-stu-id="853b2-511">Number of bytes read or written per second</span></span>

<span data-ttu-id="853b2-512">Minden lemezeken összesített értékeket szerezhető be úgy, hogy `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="853b2-512">Aggregated values across all disks can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="853b2-513">beállítás tooget adatokat egy adott eszköz számára (például/dev/sdf1), `"condition": "Name=\\"/dev/sdf1\\""`.</span><span class="sxs-lookup"><span data-stu-id="853b2-513">tooget information for a specific device (for example, /dev/sdf1), set `"condition": "Name=\\"/dev/sdf1\\""`.</span></span>

## <a name="installing-and-configuring-lad-30-via-cli"></a><span data-ttu-id="853b2-514">Telepítése és konfigurálása LAD 3.0 parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="853b2-514">Installing and configuring LAD 3.0 via CLI</span></span>

<span data-ttu-id="853b2-515">Ha a védett beállítások hello fájlban PrivateConfig.json és a nyilvános konfigurációs adatokat PublicConfig.json, futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="853b2-515">Assuming your protected settings are in hello file PrivateConfig.json and your public configuration information is in PublicConfig.json, run this command:</span></span>

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

<span data-ttu-id="853b2-516">hello parancs feltételezi, hogy a hello Azure Resource Manager módot (arm) a hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="853b2-516">hello command assumes you are using hello Azure Resource Management mode (arm) of hello Azure CLI.</span></span> <span data-ttu-id="853b2-517">tooconfigure LAD a klasszikus üzembe helyezési modell (ASM) virtuális gépeket, váltson túl "asm" mód (`azure config mode asm`), és hagyja ki a hello erőforráscsoport-név hello parancsban.</span><span class="sxs-lookup"><span data-stu-id="853b2-517">tooconfigure LAD for classic deployment model (ASM) VMs, switch too"asm" mode (`azure config mode asm`) and omit hello resource group name in hello command.</span></span> <span data-ttu-id="853b2-518">További információkért lásd: hello [platformfüggetlen parancssori felület dokumentáció](https://docs.microsoft.com/azure/xplat-cli-connect).</span><span class="sxs-lookup"><span data-stu-id="853b2-518">For more information, see hello [cross-platform CLI documentation](https://docs.microsoft.com/azure/xplat-cli-connect).</span></span>

## <a name="an-example-lad-30-configuration"></a><span data-ttu-id="853b2-519">Egy példa LAD 3.0 konfiguráció</span><span class="sxs-lookup"><span data-stu-id="853b2-519">An example LAD 3.0 configuration</span></span>

<span data-ttu-id="853b2-520">A definíciók, itt meg egy minta LAD 3.0 bővítménykonfiguráció rövid megelőző hello alapján.</span><span class="sxs-lookup"><span data-stu-id="853b2-520">Based on hello preceding definitions, here's a sample LAD 3.0 extension configuration with some explanation.</span></span> <span data-ttu-id="853b2-521">tooapply ez jelen példában tooyour kell használni a saját tárfióknév, SAS-jogkivonat fiókot, és EventHubs SAS-jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="853b2-521">tooapply this sample tooyour case, you should use your own storage account name, account SAS token, and EventHubs SAS tokens.</span></span>

### <a name="privateconfigjson"></a><span data-ttu-id="853b2-522">PrivateConfig.json</span><span class="sxs-lookup"><span data-stu-id="853b2-522">PrivateConfig.json</span></span>

<span data-ttu-id="853b2-523">Ezek a személyes beállítások konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="853b2-523">These private settings configure:</span></span>

* <span data-ttu-id="853b2-524">a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="853b2-524">a storage account</span></span>
* <span data-ttu-id="853b2-525">a megfelelő fiók SAS-jogkivonat</span><span class="sxs-lookup"><span data-stu-id="853b2-525">a matching account SAS token</span></span>
* <span data-ttu-id="853b2-526">több fogadók esetében (JsonBlob vagy az SAS-tokenje EventHubs)</span><span class="sxs-lookup"><span data-stu-id="853b2-526">several sinks (JsonBlob or EventHubs with SAS tokens)</span></span>

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a><span data-ttu-id="853b2-527">PublicConfig.json</span><span class="sxs-lookup"><span data-stu-id="853b2-527">PublicConfig.json</span></span>

<span data-ttu-id="853b2-528">A nyilvános beállítások okozhat a LAD:</span><span class="sxs-lookup"><span data-stu-id="853b2-528">These public settings cause LAD to:</span></span>

* <span data-ttu-id="853b2-529">Töltse fel a százalékos processzoridő és használt-terület toohello `WADMetrics*` tábla</span><span class="sxs-lookup"><span data-stu-id="853b2-529">Upload percent-processor-time and used-disk-space metrics toohello `WADMetrics*` table</span></span>
* <span data-ttu-id="853b2-530">Töltse fel a syslog létesítmény "user" és a súlyosság "Infó" toohello üzeneteit `LinuxSyslog*` tábla</span><span class="sxs-lookup"><span data-stu-id="853b2-530">Upload messages from syslog facility "user" and severity "info" toohello `LinuxSyslog*` table</span></span>
* <span data-ttu-id="853b2-531">Töltse fel a nyers OMI lekérdezési eredmények (PercentProcessorTime és PercentIdleTime) toohello nevű `LinuxCPU` tábla</span><span class="sxs-lookup"><span data-stu-id="853b2-531">Upload raw OMI query results (PercentProcessorTime and PercentIdleTime) toohello named `LinuxCPU` table</span></span>
* <span data-ttu-id="853b2-532">Töltse fel a fájl sorainak hozzáfűzött `/var/log/myladtestlog` toohello `MyLadTestLog` tábla</span><span class="sxs-lookup"><span data-stu-id="853b2-532">Upload appended lines in file `/var/log/myladtestlog` toohello `MyLadTestLog` table</span></span>

<span data-ttu-id="853b2-533">Minden esetben adatokat is feltöltött:</span><span class="sxs-lookup"><span data-stu-id="853b2-533">In each case, data is also uploaded to:</span></span>

* <span data-ttu-id="853b2-534">Az Azure Blob storage (a tároló neve: hello JsonBlob fogadó meghatározottak szerint)</span><span class="sxs-lookup"><span data-stu-id="853b2-534">Azure Blob storage (container name is as defined in hello JsonBlob sink)</span></span>
* <span data-ttu-id="853b2-535">EventHubs végpont (ahogy a hello EventHubs fogadó)</span><span class="sxs-lookup"><span data-stu-id="853b2-535">EventHubs endpoint (as specified in hello EventHubs sink)</span></span>

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

<span data-ttu-id="853b2-536">Hello `resourceId` hello konfigurációs meg kell egyeznie, hogy hello VM vagy hello virtuálisgép-méretezési állítva.</span><span class="sxs-lookup"><span data-stu-id="853b2-536">hello `resourceId` in hello configuration must match that of hello VM or hello virtual machine scale set.</span></span>

* <span data-ttu-id="853b2-537">Az Azure platform metrikák diagramkészítési és riasztás tudja hello dolgozunk a virtuális gép hello erőforrás-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="853b2-537">Azure platform metrics charting and alerting knows hello resourceId of hello VM you're working on.</span></span> <span data-ttu-id="853b2-538">A virtuális gép hello resourceId hello keresési kulcs használatával toofind hello adatok azt vár.</span><span class="sxs-lookup"><span data-stu-id="853b2-538">It expects toofind hello data for your VM using hello resourceId hello lookup key.</span></span>
* <span data-ttu-id="853b2-539">Az Azure automatikus skálázás használatakor hello resourceId hello automatikus skálázás konfigurációban meg kell egyeznie hello resourceId LAD használják.</span><span class="sxs-lookup"><span data-stu-id="853b2-539">If you use Azure autoscale, hello resourceId in hello autoscale configuration must match hello resourceId used by LAD.</span></span>
* <span data-ttu-id="853b2-540">hello resourceId beépített LAD által írt JsonBlobs hello nevét.</span><span class="sxs-lookup"><span data-stu-id="853b2-540">hello resourceId is built into hello names of JsonBlobs written by LAD.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="853b2-541">Az adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="853b2-541">View your data</span></span>

<span data-ttu-id="853b2-542">Hello Azure portál tooview teljesítményadatok, vagy állítson be riasztásokat:</span><span class="sxs-lookup"><span data-stu-id="853b2-542">Use hello Azure portal tooview performance data or set alerts:</span></span>

![Kép](./media/diagnostic-extension/graph_metrics.png)

<span data-ttu-id="853b2-544">Hello `performanceCounters` adatok mindig egy Azure Storage táblázatban vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="853b2-544">hello `performanceCounters` data are always stored in an Azure Storage table.</span></span> <span data-ttu-id="853b2-545">Az Azure Storage API-k sok nyelvekhez és platformokhoz érhetők el.</span><span class="sxs-lookup"><span data-stu-id="853b2-545">Azure Storage APIs are available for many languages and platforms.</span></span>

<span data-ttu-id="853b2-546">TooJsonBlob mosdók küldött adatok blobot, amely a hello nevű hello tárfiók tárolja [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="853b2-546">Data sent tooJsonBlob sinks is stored in blobs in hello storage account named in hello [Protected settings](#protected-settings).</span></span> <span data-ttu-id="853b2-547">Hello Blobadatok bármely Azure Blob Storage API-k használatával is felhasználhatnak.</span><span class="sxs-lookup"><span data-stu-id="853b2-547">You can consume hello blob data using any Azure Blob Storage APIs.</span></span>

<span data-ttu-id="853b2-548">Ezenkívül használhatja a felhasználói felület eszközök tooaccess hello adatokat az Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="853b2-548">In addition, you can use these UI tools tooaccess hello data in Azure Storage:</span></span>

* <span data-ttu-id="853b2-549">A Visual Studio Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="853b2-549">Visual Studio Server Explorer.</span></span>
* <span data-ttu-id="853b2-550">[A Microsoft Azure Tártallózó](https://azurestorageexplorer.codeplex.com/ "Azure Tártallózó").</span><span class="sxs-lookup"><span data-stu-id="853b2-550">[Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

<span data-ttu-id="853b2-551">A Microsoft Azure Tártallózó munkamenet pillanatképe látható hello az Azure Storage-táblákat és a tárolók jön létre a teszteléshez használt virtuális gép egy megfelelően konfigurált LAD 3.0 bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="853b2-551">This snapshot of a Microsoft Azure Storage Explorer session shows hello generated Azure Storage tables and containers from a correctly configured LAD 3.0 extension on a test VM.</span></span> <span data-ttu-id="853b2-552">hello kép nem egyezik pontosan hello [LAD 3.0 mintakonfiguráció](#an-example-lad-30-configuration).</span><span class="sxs-lookup"><span data-stu-id="853b2-552">hello image doesn't match exactly with hello [sample LAD 3.0 configuration](#an-example-lad-30-configuration).</span></span>

![Kép](./media/diagnostic-extension/stg_explorer.png)

<span data-ttu-id="853b2-554">Tekintse meg a megfelelő hello [EventHubs dokumentáció](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn hogyan tooconsume üzenetek közzétett tooan EventHubs végpont.</span><span class="sxs-lookup"><span data-stu-id="853b2-554">See hello relevant [EventHubs documentation](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn how tooconsume messages published tooan EventHubs endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="853b2-555">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="853b2-555">Next steps</span></span>

* <span data-ttu-id="853b2-556">A metrika értesítések [Azure figyelő](../../monitoring-and-diagnostics/insights-alerts-portal.md) hello metrikáihoz összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="853b2-556">Create metric alerts in [Azure Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) for hello metrics you collect.</span></span>
* <span data-ttu-id="853b2-557">Hozzon létre [diagramok figyelési](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) a metrikáihoz.</span><span class="sxs-lookup"><span data-stu-id="853b2-557">Create [monitoring charts](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) for your metrics.</span></span>
* <span data-ttu-id="853b2-558">Ismerje meg, hogyan túl[hozzon létre egy virtuálisgép-méretezési csoport](/azure/virtual-machines/linux/tutorial-create-vmss) használ a metrikák toocontrol automatikus skálázást.</span><span class="sxs-lookup"><span data-stu-id="853b2-558">Learn how too[create a virtual machine scale set](/azure/virtual-machines/linux/tutorial-create-vmss) using your metrics toocontrol autoscaling.</span></span>
