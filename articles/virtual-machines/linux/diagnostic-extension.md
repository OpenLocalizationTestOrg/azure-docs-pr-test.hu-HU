---
title: "Az Azure Compute - Linux diagnosztikai bővítmény |} Microsoft Docs"
description: "Hogyan konfigurálható az Azure Linux diagnosztikai bővítmény (LAD) gyűjtéséhez és az Azure-ban futó Linux virtuális gépek események naplózása."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 525d706bd709ae72f2dca1c21e06db533ccf32b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-linux-diagnostic-extension-to-monitor-metrics-and-logs"></a><span data-ttu-id="faa40-103">Linux diagnosztikai kiterjesztésének használatával figyelheti a metrikák és a naplókat</span><span class="sxs-lookup"><span data-stu-id="faa40-103">Use Linux Diagnostic Extension to monitor metrics and logs</span></span>

<span data-ttu-id="faa40-104">Ez a dokumentum ismerteti a 3.0-s és a Linux-diagnosztikai bővítmény újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="faa40-104">This document describes version 3.0 and newer of the Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="faa40-105">2.3-as és a régebbi verziójával kapcsolatos információkért lásd: [Ez a dokumentum](./classic/diagnostic-extension-v2.md).</span><span class="sxs-lookup"><span data-stu-id="faa40-105">For information about version 2.3 and older, see [this document](./classic/diagnostic-extension-v2.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="faa40-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="faa40-106">Introduction</span></span>

<span data-ttu-id="faa40-107">A Linux diagnosztikai bővítmény segít a felhasználói figyelése a Microsoft Azure-on futó Linux virtuális gép állapotát.</span><span class="sxs-lookup"><span data-stu-id="faa40-107">The Linux Diagnostic Extension helps a user monitor the health of a Linux VM running on Microsoft Azure.</span></span> <span data-ttu-id="faa40-108">A következő képességekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="faa40-108">It has the following capabilities:</span></span>

* <span data-ttu-id="faa40-109">Rendszer teljesítménymutatók gyűjti össze a virtuális Gépet, és a megadott tábla kijelölt tárfiókban tárolja azokat.</span><span class="sxs-lookup"><span data-stu-id="faa40-109">Collects system performance metrics from the VM and stores them in a specific table in a designated storage account.</span></span>
* <span data-ttu-id="faa40-110">Alkalmazásnapló-események lekéri a syslog, és a megadott tábla kijelölt tárfiókban tárolja azokat.</span><span class="sxs-lookup"><span data-stu-id="faa40-110">Retrieves log events from syslog and stores them in a specific table in the designated storage account.</span></span>
* <span data-ttu-id="faa40-111">Lehetővé teszi a felhasználók a gyűjtött és a feltöltött adatok metrikák testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="faa40-111">Enables users to customize the data metrics that are collected and uploaded.</span></span>
* <span data-ttu-id="faa40-112">Lehetővé teszi a felhasználók gyűjtött és a feltöltése események súlyossági szintek és syslog létesítményekben testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="faa40-112">Enables users to customize the syslog facilities and severity levels of events that are collected and uploaded.</span></span>
* <span data-ttu-id="faa40-113">Lehetővé teszi a felhasználók egy kijelölt tároló táblához megadott naplófájlok feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="faa40-113">Enables users to upload specified log files to a designated storage table.</span></span>
* <span data-ttu-id="faa40-114">Támogatja a metrikákat és naplózási események küldése tetszőleges EventHub-végpontokat és a JSON-formátumú blobot, amely a kijelölt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="faa40-114">Supports sending metrics and log events to arbitrary EventHub endpoints and JSON-formatted blobs in the designated storage account.</span></span>

<span data-ttu-id="faa40-115">A bővítmény mindkét Azure üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="faa40-115">This extension works with both Azure deployment models.</span></span>

## <a name="installing-the-extension-in-your-vm"></a><span data-ttu-id="faa40-116">A bővítmény telepítése a virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="faa40-116">Installing the extension in your VM</span></span>

<span data-ttu-id="faa40-117">A bővítmény engedélyezéséhez az Azure PowerShell-parancsmagok, az Azure parancssori felület parancsfájlok vagy Azure központi telepítési sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="faa40-117">You can enable this extension by using the Azure PowerShell cmdlets, Azure CLI scripts, or Azure deployment templates.</span></span> <span data-ttu-id="faa40-118">További információkért lásd: [bővítményeit biztosító funkciókat](./extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="faa40-118">For more information, see [Extensions Features](./extensions-features.md).</span></span>

<span data-ttu-id="faa40-119">Az Azure-portálon engedélyezése és konfigurálása LAD 3.0 nem használható.</span><span class="sxs-lookup"><span data-stu-id="faa40-119">The Azure portal cannot be used to enable or configure LAD 3.0.</span></span> <span data-ttu-id="faa40-120">Ehelyett azt telepíti, és konfigurálja a 2.3 verziója.</span><span class="sxs-lookup"><span data-stu-id="faa40-120">Instead, it installs and configures version 2.3.</span></span> <span data-ttu-id="faa40-121">Az Azure portál diagramok és értesítések működéséhez a bővítmény verzióját is adataival.</span><span class="sxs-lookup"><span data-stu-id="faa40-121">Azure portal graphs and alerts work with data from both versions of the extension.</span></span>

<span data-ttu-id="faa40-122">A telepítési utasításokat és egy [letölthető mintakonfiguráció](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0-s konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="faa40-122">These installation instructions and a [downloadable sample configuration](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) configure LAD 3.0 to:</span></span>

* <span data-ttu-id="faa40-123">lépéssel rögzítheti és tárolhatja a metrikák LAD 2.3; által biztosított volt</span><span class="sxs-lookup"><span data-stu-id="faa40-123">capture and store the same metrics as were provided by LAD 2.3;</span></span>
* <span data-ttu-id="faa40-124">fájl rendszer metrikákat, új, 3.0-s LAD; hasznos készlete rögzítése</span><span class="sxs-lookup"><span data-stu-id="faa40-124">capture a useful set of file system metrics, new to LAD 3.0;</span></span>
* <span data-ttu-id="faa40-125">az alapértelmezett syslog gyűjtemény LAD 2.3; által engedélyezett rögzítése</span><span class="sxs-lookup"><span data-stu-id="faa40-125">capture the default syslog collection enabled by LAD 2.3;</span></span>
* <span data-ttu-id="faa40-126">Engedélyezze az Azure portál élményét diagramkészítési, és a virtuális gép metrikák riasztást küld.</span><span class="sxs-lookup"><span data-stu-id="faa40-126">enable the Azure portal experience for charting and alerting on VM metrics.</span></span>

<span data-ttu-id="faa40-127">A letölthető konfigurációja, csak egy példa; Módosítsa a saját igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="faa40-127">The downloadable configuration is just an example; modify it to suit your own needs.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="faa40-128">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="faa40-128">Prerequisites</span></span>

* <span data-ttu-id="faa40-129">**Az Azure Linux ügynök 2.2.0 verzió vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="faa40-129">**Azure Linux Agent version 2.2.0 or later**.</span></span> <span data-ttu-id="faa40-130">A legtöbb Azure virtuális gép Linux gyűjtemény lemezképei 2.2.7 verzióját tartalmazzák, vagy később.</span><span class="sxs-lookup"><span data-stu-id="faa40-130">Most Azure VM Linux gallery images include version 2.2.7 or later.</span></span> <span data-ttu-id="faa40-131">Futtatás `/usr/sbin/waagent -version` megerősítéséhez, hogy a virtuális Gépen telepített verzióval.</span><span class="sxs-lookup"><span data-stu-id="faa40-131">Run `/usr/sbin/waagent -version` to confirm the version installed on the VM.</span></span> <span data-ttu-id="faa40-132">Ha a vendégügynök egy régebbi verzióját a virtuális gép fut, hajtsa végre a [ezeket az utasításokat](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) frissíti.</span><span class="sxs-lookup"><span data-stu-id="faa40-132">If the VM is running an older version of the guest agent, follow [these instructions](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) to update it.</span></span>
* <span data-ttu-id="faa40-133">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="faa40-133">**Azure CLI**.</span></span> <span data-ttu-id="faa40-134">[Állítsa be az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) környezet a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="faa40-134">[Set up the Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) environment on your machine.</span></span>
* <span data-ttu-id="faa40-135">A wget parancs, ha már nincs: futtatása `sudo apt-get install wget`.</span><span class="sxs-lookup"><span data-stu-id="faa40-135">The wget command, if you don't already have it: Run `sudo apt-get install wget`.</span></span>
* <span data-ttu-id="faa40-136">Meglévő Azure-előfizetése és a meglévő tárfiók belül úgy, hogy az adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="faa40-136">An existing Azure subscription and an existing storage account within it to store the data.</span></span>

### <a name="sample-installation"></a><span data-ttu-id="faa40-137">A minta telepítése</span><span class="sxs-lookup"><span data-stu-id="faa40-137">Sample installation</span></span>

<span data-ttu-id="faa40-138">Adja meg az első három sorokban a megfelelő paramétereket, majd hajtsa végre ezt a parancsfájlt a legfelső szintű:</span><span class="sxs-lookup"><span data-stu-id="faa40-138">Fill in the correct parameters on the first three lines, then execute this script as root:</span></span>

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login to Azure first before anything else
az login

# Select the subscription containing the storage account
az account set --subscription <your_azure_subscription_id>

# Download the sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build the VM resource ID. Replace storage account name and resource ID in the public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build the protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure to install and enable the extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

<span data-ttu-id="faa40-139">A minta konfigurációját, és annak tartalmát, az URL-címe van változhatnak.</span><span class="sxs-lookup"><span data-stu-id="faa40-139">The URL for the sample configuration, and its contents, are subject to change.</span></span> <span data-ttu-id="faa40-140">Töltse le a portálbeállítások JSON-fájlt, és az igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="faa40-140">Download a copy of the portal settings JSON file and customize it for your needs.</span></span> <span data-ttu-id="faa40-141">A sablonok vagy automation, hozhat létre egy saját másolat ahelyett, hogy letöltése URL-címet minden alkalommal, amikor használja.</span><span class="sxs-lookup"><span data-stu-id="faa40-141">Any templates or automation you construct should use your own copy, rather than downloading that URL each time.</span></span>

### <a name="updating-the-extension-settings"></a><span data-ttu-id="faa40-142">A bővítmény beállításainak frissítése folyamatban</span><span class="sxs-lookup"><span data-stu-id="faa40-142">Updating the extension settings</span></span>

<span data-ttu-id="faa40-143">Miután megváltoztatta a védett vagy nyilvános beállításokat, azok a virtuális gép ugyanaz a parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="faa40-143">After you've changed your Protected or Public settings, deploy them to the VM by running the same command.</span></span> <span data-ttu-id="faa40-144">Ha a változás a a beállításokat, a bővítmény kapja meg a frissített beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="faa40-144">If anything changed in the settings, the updated settings are sent to the extension.</span></span> <span data-ttu-id="faa40-145">LAD betölti a konfigurációt, és újraindul saját magát.</span><span class="sxs-lookup"><span data-stu-id="faa40-145">LAD reloads the configuration and restarts itself.</span></span>

### <a name="migration-from-previous-versions-of-the-extension"></a><span data-ttu-id="faa40-146">A bővítmény korábbi verzióiról való áttelepítés</span><span class="sxs-lookup"><span data-stu-id="faa40-146">Migration from previous versions of the extension</span></span>

<span data-ttu-id="faa40-147">A bővítmény legújabb verziója **3.0**.</span><span class="sxs-lookup"><span data-stu-id="faa40-147">The latest version of the extension is **3.0**.</span></span> <span data-ttu-id="faa40-148">**A régi verziókat (2.x) elavultak, ezért lehet, hogy közzé nem tett, vagy azt követően 2018 július 31**.</span><span class="sxs-lookup"><span data-stu-id="faa40-148">**Any old versions (2.x) are deprecated and may be unpublished on or after July 31, 2018**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="faa40-149">A bővítmény jelentős változásokat a bővítmény konfigurációja vezet be.</span><span class="sxs-lookup"><span data-stu-id="faa40-149">This extension introduces breaking changes to the configuration of the extension.</span></span> <span data-ttu-id="faa40-150">Egy ilyen módosítás, melyekkel biztonságosabbá teheti a bővítmény; Ennek eredményeképpen visszamenőleges 2.x kompatibilitást sikerült nem kell tartani.</span><span class="sxs-lookup"><span data-stu-id="faa40-150">One such change was made to improve the security of the extension; as a result, backwards compatibility with 2.x could not be maintained.</span></span> <span data-ttu-id="faa40-151">A bővítmény Publisher ehhez a kiterjesztéshez is eltér a közzétevő a 2.x verziójához.</span><span class="sxs-lookup"><span data-stu-id="faa40-151">Also, the Extension Publisher for this extension is different than the publisher for the 2.x versions.</span></span>
>
> <span data-ttu-id="faa40-152">2.x át ezt a bővítmény új verzióját, távolítsa el a régi bővítmény (alatt a régi közzétevő neve), majd telepítse a bővítmény 3 verzióját.</span><span class="sxs-lookup"><span data-stu-id="faa40-152">To migrate from 2.x to this new version of the extension, you must uninstall the old extension (under the old publisher name), then install version 3 of the extension.</span></span>

<span data-ttu-id="faa40-153">Javaslatok:</span><span class="sxs-lookup"><span data-stu-id="faa40-153">Recommendations:</span></span>

* <span data-ttu-id="faa40-154">Telepítse a bővítmény engedélyezve van az automatikus alverzió frissítését.</span><span class="sxs-lookup"><span data-stu-id="faa40-154">Install the extension with automatic minor version upgrade enabled.</span></span>
  * <span data-ttu-id="faa40-155">A klasszikus telepítési modell virtuális gépek adja meg "3.*" verzióval Azure XPLAT parancssori felületen vagy a Powershellen keresztül bővítmény telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="faa40-155">On classic deployment model VMs, specify '3.*' as the version if you are installing the extension through Azure XPLAT CLI or Powershell.</span></span>
  * <span data-ttu-id="faa40-156">Az Azure Resource Manager telepítési modellhez tartozó virtuális gépek esetén tartalmazhat ""autoUpgradeMinorVersion": true" az a virtuális gép központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="faa40-156">On Azure Resource Manager deployment model VMs, include '"autoUpgradeMinorVersion": true' in the VM deployment template.</span></span>
* <span data-ttu-id="faa40-157">A LAD 3.0 új/eltérőek a tárolási fiók használata.</span><span class="sxs-lookup"><span data-stu-id="faa40-157">Use a new/different storage account for LAD 3.0.</span></span> <span data-ttu-id="faa40-158">Van több kis incompatibilities LAD 2.3 és LAD 3.0 között, amelyek megosztása a problémás fiók:</span><span class="sxs-lookup"><span data-stu-id="faa40-158">There are several small incompatibilities between LAD 2.3 and LAD 3.0 that make sharing an account troublesome:</span></span>
  * <span data-ttu-id="faa40-159">LAD 3.0 tárolja a syslog-események egy táblát, amely egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="faa40-159">LAD 3.0 stores syslog events in a table with a different name.</span></span>
  * <span data-ttu-id="faa40-160">A karakterláncok a counterSpecifier `builtin` metrikák LAD 3.0 különböznek.</span><span class="sxs-lookup"><span data-stu-id="faa40-160">The counterSpecifier strings for `builtin` metrics differ in LAD 3.0.</span></span>

## <a name="protected-settings"></a><span data-ttu-id="faa40-161">Védett beállításai</span><span class="sxs-lookup"><span data-stu-id="faa40-161">Protected settings</span></span>

<span data-ttu-id="faa40-162">Ez a konfigurációs adatokat bizalmas adatokat, amelyeket védeni kell a nyilvános nézet, például a tároló hitelesítő adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="faa40-162">This set of configuration information contains sensitive information that should be protected from public view, for example, storage credentials.</span></span> <span data-ttu-id="faa40-163">Ezek a beállítások továbbítva, és a bővítmény titkosított formában tárolja.</span><span class="sxs-lookup"><span data-stu-id="faa40-163">These settings are transmitted to and stored by the extension in encrypted form.</span></span>

```json
{
    "storageAccountName" : "the storage account to receive data",
    "storageAccountEndPoint": "the hostname suffix for the cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

<span data-ttu-id="faa40-164">Név</span><span class="sxs-lookup"><span data-stu-id="faa40-164">Name</span></span> | <span data-ttu-id="faa40-165">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-165">Value</span></span>
---- | -----
<span data-ttu-id="faa40-166">storageAccountName</span><span class="sxs-lookup"><span data-stu-id="faa40-166">storageAccountName</span></span> | <span data-ttu-id="faa40-167">A tárfiók, amelyben adatot ír a kiegészítő mező neve.</span><span class="sxs-lookup"><span data-stu-id="faa40-167">The name of the storage account in which data is written by the extension.</span></span>
<span data-ttu-id="faa40-168">storageAccountEndPoint</span><span class="sxs-lookup"><span data-stu-id="faa40-168">storageAccountEndPoint</span></span> | <span data-ttu-id="faa40-169">(választható) A végpont a felhőben, amelyben a tárfiók található azonosítása.</span><span class="sxs-lookup"><span data-stu-id="faa40-169">(optional) The endpoint identifying the cloud in which the storage account exists.</span></span> <span data-ttu-id="faa40-170">Ha ez a beállítás hiányzik, LAD az Azure nyilvános felhőjében alapértelmezett `https://core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="faa40-170">If this setting is absent, LAD defaults to the Azure public cloud, `https://core.windows.net`.</span></span> <span data-ttu-id="faa40-171">A Németországi Azure storage-fiók használatához Azure Government vagy Azure Kína, állítsa ezt az értéket ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="faa40-171">To use a storage account in Azure Germany, Azure Government, or Azure China, set this value accordingly.</span></span>
<span data-ttu-id="faa40-172">storageAccountSasToken</span><span class="sxs-lookup"><span data-stu-id="faa40-172">storageAccountSasToken</span></span> | <span data-ttu-id="faa40-173">Egy [fiók SAS-jogkivonat](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) a Blob és Table szolgáltatásait (`ss='bt'`), tárolók és objektumok alkalmazandó (`srt='co'`), amely hozzáadásához létrehozása, listában frissítése, és írási engedélyekkel (`sp='acluw'`).</span><span class="sxs-lookup"><span data-stu-id="faa40-173">An [Account SAS token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) for Blob and Table services (`ss='bt'`), applicable to containers and objects (`srt='co'`), which grants add, create, list, update, and write permissions (`sp='acluw'`).</span></span> <span data-ttu-id="faa40-174">Tegye *nem* közé tartozik a bevezető kérdőjel (?).</span><span class="sxs-lookup"><span data-stu-id="faa40-174">Do *not* include the leading question-mark (?).</span></span>
<span data-ttu-id="faa40-175">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="faa40-175">mdsdHttpProxy</span></span> | <span data-ttu-id="faa40-176">(választható) HTTP-proxyadatok csatlakozni a megadott tárfiók és a végpont a bővítmény engedélyezéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="faa40-176">(optional) HTTP proxy information needed to enable the extension to connect to the specified storage account and endpoint.</span></span>
<span data-ttu-id="faa40-177">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="faa40-177">sinksConfig</span></span> | <span data-ttu-id="faa40-178">(választható) Alternatív célhoz, amelyhez metrikákkal és eseményekkel kézbesítése részleteit.</span><span class="sxs-lookup"><span data-stu-id="faa40-178">(optional) Details of alternative destinations to which metrics and events can be delivered.</span></span> <span data-ttu-id="faa40-179">A bővítmény által támogatott minden egyes adatokat a fogadó részleteit a következő szakaszok ismertetnek.</span><span class="sxs-lookup"><span data-stu-id="faa40-179">The specific details of each data sink supported by the extension are covered in the sections that follow.</span></span>

<span data-ttu-id="faa40-180">A szükséges SAS-jogkivonatot az Azure portálon keresztül egyszerűen állíthat össze.</span><span class="sxs-lookup"><span data-stu-id="faa40-180">You can easily construct the required SAS token through the Azure portal.</span></span>

1. <span data-ttu-id="faa40-181">Válassza ki az általános célú tárfiók, amelybe írni a bővítmény</span><span class="sxs-lookup"><span data-stu-id="faa40-181">Select the general-purpose storage account to which you want the extension to write</span></span>
1. <span data-ttu-id="faa40-182">Válassza a "Megosztott hozzáférési aláírást" a bal oldali menü Beállítások része</span><span class="sxs-lookup"><span data-stu-id="faa40-182">Select "Shared access signature" from the Settings part of the left menu</span></span>
1. <span data-ttu-id="faa40-183">Ellenőrizze a megfelelő szakaszait, mint korábban leírt</span><span class="sxs-lookup"><span data-stu-id="faa40-183">Make the appropriate sections as previously described</span></span>
1. <span data-ttu-id="faa40-184">A "Készítése SAS" gombra.</span><span class="sxs-lookup"><span data-stu-id="faa40-184">Click the "Generate SAS" button.</span></span>

![Kép](./media/diagnostic-extension/make_sas.png)

<span data-ttu-id="faa40-186">A generált SAS másolja az storageAccountSasToken mezőjébe; Távolítsa el a bevezető kérdőjel ("?").</span><span class="sxs-lookup"><span data-stu-id="faa40-186">Copy the generated SAS into the storageAccountSasToken field; remove the leading question-mark ("?").</span></span>

### <a name="sinksconfig"></a><span data-ttu-id="faa40-187">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="faa40-187">sinksConfig</span></span>

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

<span data-ttu-id="faa40-188">Ez a szakasz választható, amelyhez a bővítményt a összegyűjti az adatokat elküldi a további célok meghatározása.</span><span class="sxs-lookup"><span data-stu-id="faa40-188">This optional section defines additional destinations to which the extension sends the information it collects.</span></span> <span data-ttu-id="faa40-189">A "fogadó" tömb minden további adatokat fogadó egy objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="faa40-189">The "sink" array contains an object for each additional data sink.</span></span> <span data-ttu-id="faa40-190">A "type" attribútum meghatározza, hogy az objektum más attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="faa40-190">The "type" attribute determines the other attributes in the object.</span></span>

<span data-ttu-id="faa40-191">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-191">Element</span></span> | <span data-ttu-id="faa40-192">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-192">Value</span></span>
------- | -----
<span data-ttu-id="faa40-193">név</span><span class="sxs-lookup"><span data-stu-id="faa40-193">name</span></span> | <span data-ttu-id="faa40-194">Ez a gyűjtő részeiben bővítménykonfiguráció hivatkozik karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="faa40-194">A string used to refer to this sink elsewhere in the extension configuration.</span></span>
<span data-ttu-id="faa40-195">type</span><span class="sxs-lookup"><span data-stu-id="faa40-195">type</span></span> | <span data-ttu-id="faa40-196">A múltbeli fogadó típusa.</span><span class="sxs-lookup"><span data-stu-id="faa40-196">The type of sink being defined.</span></span> <span data-ttu-id="faa40-197">Meghatározza, hogy a többi érték (ha vannak) az ilyen típusú példányok.</span><span class="sxs-lookup"><span data-stu-id="faa40-197">Determines the other values (if any) in instances of this type.</span></span>

<span data-ttu-id="faa40-198">A Linux diagnosztikai bővítmény 3.0-s verziója két fogadó-típusokat támogatja: az EventHub és JsonBlob.</span><span class="sxs-lookup"><span data-stu-id="faa40-198">Version 3.0 of the Linux Diagnostic Extension supports two sink types: EventHub, and JsonBlob.</span></span>

#### <a name="the-eventhub-sink"></a><span data-ttu-id="faa40-199">Az EventHub fogadó</span><span class="sxs-lookup"><span data-stu-id="faa40-199">The EventHub sink</span></span>

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

<span data-ttu-id="faa40-200">A "sasURL" bejegyzés tartalmazza a teljes URL-címet, beleértve a SAS-jogkivonat, az Event Hubs, amelyhez adatokat közzé kell tenni.</span><span class="sxs-lookup"><span data-stu-id="faa40-200">The "sasURL" entry contains the full URL, including SAS token, for the Event Hub to which data should be published.</span></span> <span data-ttu-id="faa40-201">LAD van szükség, egy olyan házirendet, amely lehetővé teszi, hogy a küldési elnevezési SAS jogcímek.</span><span class="sxs-lookup"><span data-stu-id="faa40-201">LAD requires a SAS naming a policy that enables the Send claim.</span></span> <span data-ttu-id="faa40-202">Példa:</span><span class="sxs-lookup"><span data-stu-id="faa40-202">An example:</span></span>

* <span data-ttu-id="faa40-203">Hozzon létre egy Event Hubs-névtér neve`contosohub`</span><span class="sxs-lookup"><span data-stu-id="faa40-203">Create an Event Hubs namespace called `contosohub`</span></span>
* <span data-ttu-id="faa40-204">Létrehoz egy Eseményközpontot, a névtér neve`syslogmsgs`</span><span class="sxs-lookup"><span data-stu-id="faa40-204">Create an Event Hub in the namespace called `syslogmsgs`</span></span>
* <span data-ttu-id="faa40-205">Az Event Hubs nevű meg megosztott hozzáférési házirend létrehozása `writer` , amely lehetővé teszi, hogy a küldési jogcím</span><span class="sxs-lookup"><span data-stu-id="faa40-205">Create a Shared access policy on the Event Hub named `writer` that enables the Send claim</span></span>

<span data-ttu-id="faa40-206">Ha létrehozott egy SAS jó 2018. január 1. az UTC éjfél sasURL érték lehet:</span><span class="sxs-lookup"><span data-stu-id="faa40-206">If you created a SAS good until midnight UTC on January 1, 2018, the sasURL value might be:</span></span>

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

<span data-ttu-id="faa40-207">További információ a SAS-jogkivonatokat előállító az Event Hubs: [Ez a weblap](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span><span class="sxs-lookup"><span data-stu-id="faa40-207">For more information about generating SAS tokens for Event Hubs, see [this web page](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span></span>

#### <a name="the-jsonblob-sink"></a><span data-ttu-id="faa40-208">A JsonBlob fogadó</span><span class="sxs-lookup"><span data-stu-id="faa40-208">The JsonBlob sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

<span data-ttu-id="faa40-209">Az Azure-tárfiókba blobok egy JsonBlob fogadó irányítva adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="faa40-209">Data directed to a JsonBlob sink is stored in blobs in Azure storage.</span></span> <span data-ttu-id="faa40-210">Minden példánya LAD blob minden fogadó név óránként hoz létre.</span><span class="sxs-lookup"><span data-stu-id="faa40-210">Each instance of LAD creates a blob every hour for each sink name.</span></span> <span data-ttu-id="faa40-211">Minden egyes blob mindig tartalmaz egy szintaktikailag érvényes JSON-tömb objektum.</span><span class="sxs-lookup"><span data-stu-id="faa40-211">Each blob always contains a syntactically valid JSON array of object.</span></span> <span data-ttu-id="faa40-212">Új bejegyzések i hozzáadódnak a tömb.</span><span class="sxs-lookup"><span data-stu-id="faa40-212">New entries are atomically added to the array.</span></span> <span data-ttu-id="faa40-213">A tároló neve megegyezik a gyűjtő a blobok tárolja.</span><span class="sxs-lookup"><span data-stu-id="faa40-213">Blobs are stored in a container with the same name as the sink.</span></span> <span data-ttu-id="faa40-214">A blob-tároló neve az Azure storage szabályok vonatkoznak a nevek JsonBlob nyelő: 3 és 63 kisbetűs alfanumerikus ASCII-karaktereket, és kötőjelek között.</span><span class="sxs-lookup"><span data-stu-id="faa40-214">The Azure storage rules for blob container names apply to the names of JsonBlob sinks: between 3 and 63 lower-case alphanumeric ASCII characters or dashes.</span></span>

## <a name="public-settings"></a><span data-ttu-id="faa40-215">Nyilvános beállításai</span><span class="sxs-lookup"><span data-stu-id="faa40-215">Public settings</span></span>

<span data-ttu-id="faa40-216">Ez a struktúra a bővítmény által gyűjtött információk szabályozó beállítások több blokkot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="faa40-216">This structure contains various blocks of settings that control the information collected by the extension.</span></span> <span data-ttu-id="faa40-217">Minden beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="faa40-217">Each setting is optional.</span></span> <span data-ttu-id="faa40-218">Ha megad `ladCfg`, meg kell adni `StorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="faa40-218">If you specify `ladCfg`, you must also specify `StorageAccount`.</span></span>

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "the storage account to receive data",
    "mdsdHttpProxy" : ""
}
```

<span data-ttu-id="faa40-219">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-219">Element</span></span> | <span data-ttu-id="faa40-220">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-220">Value</span></span>
------- | -----
<span data-ttu-id="faa40-221">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="faa40-221">StorageAccount</span></span> | <span data-ttu-id="faa40-222">A tárfiók, amelyben adatot ír a kiegészítő mező neve.</span><span class="sxs-lookup"><span data-stu-id="faa40-222">The name of the storage account in which data is written by the extension.</span></span> <span data-ttu-id="faa40-223">A névvel kell lennie, mint a megadott a [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="faa40-223">Must be the same name as is specified in the [Protected settings](#protected-settings).</span></span>
<span data-ttu-id="faa40-224">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="faa40-224">mdsdHttpProxy</span></span> | <span data-ttu-id="faa40-225">(választható) Ugyanaz, mint a a [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="faa40-225">(optional) Same as in the [Protected settings](#protected-settings).</span></span> <span data-ttu-id="faa40-226">A nyilvános érték felülbírálja a saját értéket, ha beállítása.</span><span class="sxs-lookup"><span data-stu-id="faa40-226">The public value is overridden by the private value, if set.</span></span> <span data-ttu-id="faa40-227">Helyezze el, amely tartalmazza a titkos kulcs, például a jelszó, a proxybeállításokat a [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="faa40-227">Place proxy settings that contain a secret, such as a password, in the [Protected settings](#protected-settings).</span></span>

<span data-ttu-id="faa40-228">A fennmaradó összetevőit az alábbi szakaszok részletesen.</span><span class="sxs-lookup"><span data-stu-id="faa40-228">The remaining elements are described in detail in the following sections.</span></span>

### <a name="ladcfg"></a><span data-ttu-id="faa40-229">ladCfg</span><span class="sxs-lookup"><span data-stu-id="faa40-229">ladCfg</span></span>

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

<span data-ttu-id="faa40-230">A választható struktúra vezérlők metrikák és a naplókat a kézbesítési az Azure metrikaszolgáltatás és egyéb adatok összegyűjtése fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="faa40-230">This optional structure controls the gathering of metrics and logs for delivery to the Azure Metrics service and to other data sinks.</span></span> <span data-ttu-id="faa40-231">Meg kell adnia vagy `performanceCounters` vagy `syslogEvents` vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="faa40-231">You must specify either `performanceCounters` or `syslogEvents` or both.</span></span> <span data-ttu-id="faa40-232">Meg kell adnia a `metrics` struktúra.</span><span class="sxs-lookup"><span data-stu-id="faa40-232">You must specify the `metrics` structure.</span></span>

<span data-ttu-id="faa40-233">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-233">Element</span></span> | <span data-ttu-id="faa40-234">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-234">Value</span></span>
------- | -----
<span data-ttu-id="faa40-235">eventVolume</span><span class="sxs-lookup"><span data-stu-id="faa40-235">eventVolume</span></span> | <span data-ttu-id="faa40-236">(választható) A tárolási tábla belül létrehozott partíciók számát szabályozza.</span><span class="sxs-lookup"><span data-stu-id="faa40-236">(optional) Controls the number of partitions created within the storage table.</span></span> <span data-ttu-id="faa40-237">Egyikének kell lennie `"Large"`, `"Medium"`, vagy `"Small"`.</span><span class="sxs-lookup"><span data-stu-id="faa40-237">Must be one of `"Large"`, `"Medium"`, or `"Small"`.</span></span> <span data-ttu-id="faa40-238">Ha nincs megadva, az alapértelmezett érték: `"Medium"`.</span><span class="sxs-lookup"><span data-stu-id="faa40-238">If not specified, the default value is `"Medium"`.</span></span>
<span data-ttu-id="faa40-239">sampleRateInSeconds</span><span class="sxs-lookup"><span data-stu-id="faa40-239">sampleRateInSeconds</span></span> | <span data-ttu-id="faa40-240">(választható) Az alapértelmezett időköz közötti nyers (unaggregated) mérőszámok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="faa40-240">(optional) The default interval between collection of raw (unaggregated) metrics.</span></span> <span data-ttu-id="faa40-241">A legkisebb támogatott mintavételi gyakoriság: 15 másodperc.</span><span class="sxs-lookup"><span data-stu-id="faa40-241">The smallest supported sample rate is 15 seconds.</span></span> <span data-ttu-id="faa40-242">Ha nincs megadva, az alapértelmezett érték: `15`.</span><span class="sxs-lookup"><span data-stu-id="faa40-242">If not specified, the default value is `15`.</span></span>

#### <a name="metrics"></a><span data-ttu-id="faa40-243">metrics</span><span class="sxs-lookup"><span data-stu-id="faa40-243">metrics</span></span>

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

<span data-ttu-id="faa40-244">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-244">Element</span></span> | <span data-ttu-id="faa40-245">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-245">Value</span></span>
------- | -----
<span data-ttu-id="faa40-246">resourceId</span><span class="sxs-lookup"><span data-stu-id="faa40-246">resourceId</span></span> | <span data-ttu-id="faa40-247">A virtuális gép vagy virtuálisgép-méretezési az Azure Resource Manager erőforrás-azonosítója, amelyhez tartozik a virtuális gép beállítása.</span><span class="sxs-lookup"><span data-stu-id="faa40-247">The Azure Resource Manager resource ID of the VM or of the virtual machine scale set to which the VM belongs.</span></span> <span data-ttu-id="faa40-248">Ezzel a beállítással lehet is megadott, ha bármely JsonBlob fogadó szerepel-e a konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="faa40-248">This setting must be also specified if any JsonBlob sink is used in the configuration.</span></span>
<span data-ttu-id="faa40-249">scheduledTransferPeriod</span><span class="sxs-lookup"><span data-stu-id="faa40-249">scheduledTransferPeriod</span></span> | <span data-ttu-id="faa40-250">A gyakoriság, amellyel összesített adatok gyűjtése le van számított és Azure metrika, százalékban kifejezve van 8601 időt időközönkénti továbbítja.</span><span class="sxs-lookup"><span data-stu-id="faa40-250">The frequency at which aggregate metrics are to be computed and transferred to Azure Metrics, expressed as an IS 8601 time interval.</span></span> <span data-ttu-id="faa40-251">A legkisebb átviteli időtartam 60 másodperc, ez azt jelenti, hogy PT1M.</span><span class="sxs-lookup"><span data-stu-id="faa40-251">The smallest transfer period is 60 seconds, that is, PT1M.</span></span> <span data-ttu-id="faa40-252">Meg kell adnia legalább egy scheduledTransferPeriod.</span><span class="sxs-lookup"><span data-stu-id="faa40-252">You must specify at least one scheduledTransferPeriod.</span></span>

<span data-ttu-id="faa40-253">A metrikák performanceCounters szakaszában megadott mintáit összegyűjtött 15 másodpercenként vagy: a minta értékelje az explicit módon definiálva számláló.</span><span class="sxs-lookup"><span data-stu-id="faa40-253">Samples of the metrics specified in the performanceCounters section are collected every 15 seconds or at the sample rate explicitly defined for the counter.</span></span> <span data-ttu-id="faa40-254">Ha több scheduledTransferPeriod gyakoriságot jelenik meg (ahogy a példában), minden Összesítés kiszámítása egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="faa40-254">If multiple scheduledTransferPeriod frequencies appear (as in the example), each aggregation is computed independently.</span></span>

#### <a name="performancecounters"></a><span data-ttu-id="faa40-255">performanceCounters</span><span class="sxs-lookup"><span data-stu-id="faa40-255">performanceCounters</span></span>

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

<span data-ttu-id="faa40-256">Ez a szakasz választható metrikák gyűjteményét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="faa40-256">This optional section controls the collection of metrics.</span></span> <span data-ttu-id="faa40-257">Nyers minták összesítik az egyes [scheduledTransferPeriod](#metrics) ezeket az értékeket létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="faa40-257">Raw samples are aggregated for each [scheduledTransferPeriod](#metrics) to produce these values:</span></span>

* <span data-ttu-id="faa40-258">témakörök</span><span class="sxs-lookup"><span data-stu-id="faa40-258">mean</span></span>
* <span data-ttu-id="faa40-259">minimális</span><span class="sxs-lookup"><span data-stu-id="faa40-259">minimum</span></span>
* <span data-ttu-id="faa40-260">Maximális</span><span class="sxs-lookup"><span data-stu-id="faa40-260">maximum</span></span>
* <span data-ttu-id="faa40-261">utolsó összegyűjtött érték</span><span class="sxs-lookup"><span data-stu-id="faa40-261">last-collected value</span></span>
* <span data-ttu-id="faa40-262">a nyers, összesített kiszámítására használt minták száma</span><span class="sxs-lookup"><span data-stu-id="faa40-262">count of raw samples used to compute the aggregate</span></span>

<span data-ttu-id="faa40-263">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-263">Element</span></span> | <span data-ttu-id="faa40-264">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-264">Value</span></span>
------- | -----
<span data-ttu-id="faa40-265">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="faa40-265">sinks</span></span> | <span data-ttu-id="faa40-266">(választható) Egy vesszővel tagolt listája nyelő mely LAD való küld mérték eredményeit összesíti.</span><span class="sxs-lookup"><span data-stu-id="faa40-266">(optional) A comma-separated list of names of sinks to which LAD sends aggregated metric results.</span></span> <span data-ttu-id="faa40-267">Minden felsorolt fogadó összes összesített metrikát kerülnek közzétételre.</span><span class="sxs-lookup"><span data-stu-id="faa40-267">All aggregated metrics are published to each listed sink.</span></span> <span data-ttu-id="faa40-268">Lásd: [sinksConfig](#sinksconfig).</span><span class="sxs-lookup"><span data-stu-id="faa40-268">See [sinksConfig](#sinksconfig).</span></span> <span data-ttu-id="faa40-269">Példa: `"EHsink1, myjsonsink"`.</span><span class="sxs-lookup"><span data-stu-id="faa40-269">Example: `"EHsink1, myjsonsink"`.</span></span>
<span data-ttu-id="faa40-270">type</span><span class="sxs-lookup"><span data-stu-id="faa40-270">type</span></span> | <span data-ttu-id="faa40-271">A metrika a tényleges szolgáltató azonosítja.</span><span class="sxs-lookup"><span data-stu-id="faa40-271">Identifies the actual provider of the metric.</span></span>
<span data-ttu-id="faa40-272">Osztály</span><span class="sxs-lookup"><span data-stu-id="faa40-272">class</span></span> | <span data-ttu-id="faa40-273">"Számláló", és azonosítja az adott metrika a szolgáltató névtéren belül.</span><span class="sxs-lookup"><span data-stu-id="faa40-273">Together with "counter", identifies the specific metric within the provider's namespace.</span></span>
<span data-ttu-id="faa40-274">A számláló</span><span class="sxs-lookup"><span data-stu-id="faa40-274">counter</span></span> | <span data-ttu-id="faa40-275">"Class", és azonosítja az adott metrika a szolgáltató névtéren belül.</span><span class="sxs-lookup"><span data-stu-id="faa40-275">Together with "class", identifies the specific metric within the provider's namespace.</span></span>
<span data-ttu-id="faa40-276">counterSpecifier</span><span class="sxs-lookup"><span data-stu-id="faa40-276">counterSpecifier</span></span> | <span data-ttu-id="faa40-277">Az Azure metrikák névtérben adott metrika azonosítja.</span><span class="sxs-lookup"><span data-stu-id="faa40-277">Identifies the specific metric within the Azure Metrics namespace.</span></span>
<span data-ttu-id="faa40-278">Az állapot</span><span class="sxs-lookup"><span data-stu-id="faa40-278">condition</span></span> | <span data-ttu-id="faa40-279">(választható) Kiválasztja az objektum, amelyhez a metrika vonatkozik, vagy az összesítés kiválasztja, hogy az objektum összes példánya között egy adott példányához.</span><span class="sxs-lookup"><span data-stu-id="faa40-279">(optional) Selects a specific instance of the object to which the metric applies or selects the aggregation across all instances of that object.</span></span> <span data-ttu-id="faa40-280">További információkért lásd: a [ `builtin` metrikai meghatározásainak](#metrics-supported-by-builtin).</span><span class="sxs-lookup"><span data-stu-id="faa40-280">For more information, see the [`builtin` metric definitions](#metrics-supported-by-builtin).</span></span>
<span data-ttu-id="faa40-281">sampleRate</span><span class="sxs-lookup"><span data-stu-id="faa40-281">sampleRate</span></span> | <span data-ttu-id="faa40-282">Beállítja a változási gyakoriság, amellyel ez a mérőszám a nyers minták gyűjtik 8601 intervallum van.</span><span class="sxs-lookup"><span data-stu-id="faa40-282">IS 8601 interval that sets the rate at which raw samples for this metric are collected.</span></span> <span data-ttu-id="faa40-283">Ha nincs megadva, az adatgyűjtési időköz értéke értékével [sampleRateInSeconds](#ladcfg).</span><span class="sxs-lookup"><span data-stu-id="faa40-283">If not set, the collection interval is set by the value of [sampleRateInSeconds](#ladcfg).</span></span> <span data-ttu-id="faa40-284">A legrövidebb támogatott mintavételi gyakoriság: 15 másodperc (PT15S).</span><span class="sxs-lookup"><span data-stu-id="faa40-284">The shortest supported sample rate is 15 seconds (PT15S).</span></span>
<span data-ttu-id="faa40-285">egység</span><span class="sxs-lookup"><span data-stu-id="faa40-285">unit</span></span> | <span data-ttu-id="faa40-286">Ezek a karakterláncok egyike lehet: "Count", "Memória", "S", "Százaléka", "CountPerSecond", "BytesPerSecond", "Ezredmásodperces".</span><span class="sxs-lookup"><span data-stu-id="faa40-286">Should be one of these strings: "Count", "Bytes", "Seconds", "Percent", "CountPerSecond", "BytesPerSecond", "Millisecond".</span></span> <span data-ttu-id="faa40-287">Határozza meg a metrika egység.</span><span class="sxs-lookup"><span data-stu-id="faa40-287">Defines the unit for the metric.</span></span> <span data-ttu-id="faa40-288">Az összegyűjtött adatok fogyasztóinak várhatóan az összegyűjtött adatok értékeket a egység.</span><span class="sxs-lookup"><span data-stu-id="faa40-288">Consumers of the collected data expect the collected data values to match this unit.</span></span> <span data-ttu-id="faa40-289">LAD figyelmen kívül hagyja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="faa40-289">LAD ignores this field.</span></span>
<span data-ttu-id="faa40-290">displayName</span><span class="sxs-lookup"><span data-stu-id="faa40-290">displayName</span></span> | <span data-ttu-id="faa40-291">A címke (a kapcsolódó területi beállításban megadott nyelven) csatolni kell ezeket az adatokat az Azure metrikákat.</span><span class="sxs-lookup"><span data-stu-id="faa40-291">The label (in the language specified by the associated locale setting) to be attached to this data in Azure Metrics.</span></span> <span data-ttu-id="faa40-292">LAD figyelmen kívül hagyja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="faa40-292">LAD ignores this field.</span></span>

<span data-ttu-id="faa40-293">A counterSpecifier tetszőleges azonosító, amely.</span><span class="sxs-lookup"><span data-stu-id="faa40-293">The counterSpecifier is an arbitrary identifier.</span></span> <span data-ttu-id="faa40-294">Metrikák fogyasztóinak, például az Azure portál diagramkészítési, és counterSpecifier riasztási szolgáltatás, használja a "key" azonosító a metrika példányának vagy egy mértéket.</span><span class="sxs-lookup"><span data-stu-id="faa40-294">Consumers of metrics, like the Azure portal charting and alerting feature, use counterSpecifier as the "key" that identifies a metric or an instance of a metric.</span></span> <span data-ttu-id="faa40-295">A `builtin` metrika, ajánlott counterSpecifier értékek kezdődő `/builtin/`.</span><span class="sxs-lookup"><span data-stu-id="faa40-295">For `builtin` metrics, we recommend you use counterSpecifier values that begin with `/builtin/`.</span></span> <span data-ttu-id="faa40-296">Gyűjti a metrika egy adott példányához, azt javasoljuk a példány azonosítója csatolása counterSpecifier értékét.</span><span class="sxs-lookup"><span data-stu-id="faa40-296">If you are collecting a specific instance of a metric, we recommend you attach the identifier of the instance to the counterSpecifier value.</span></span> <span data-ttu-id="faa40-297">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="faa40-297">Some examples:</span></span>

* <span data-ttu-id="faa40-298">`/builtin/Processor/PercentIdleTime`-Az összes mag között átlagosan üresjárati idő</span><span class="sxs-lookup"><span data-stu-id="faa40-298">`/builtin/Processor/PercentIdleTime` - Idle time averaged across all cores</span></span>
* <span data-ttu-id="faa40-299">`/builtin/Disk/FreeSpace(/mnt)`– Szabad terület a /mnt fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="faa40-299">`/builtin/Disk/FreeSpace(/mnt)` - Free space for the /mnt filesystem</span></span>
* <span data-ttu-id="faa40-300">`/builtin/Disk/FreeSpace`– Az összes csatlakoztatott fájlrendszerek között átlagosan szabad terület</span><span class="sxs-lookup"><span data-stu-id="faa40-300">`/builtin/Disk/FreeSpace` - Free space averaged across all mounted filesystems</span></span>

<span data-ttu-id="faa40-301">LAD és az Azure-portál nem vár a counterSpecifier értéket megfelel a mintának.</span><span class="sxs-lookup"><span data-stu-id="faa40-301">Neither LAD nor the Azure portal expects the counterSpecifier value to match any pattern.</span></span> <span data-ttu-id="faa40-302">Hogyan hozható létre counterSpecifier értékek a kell.</span><span class="sxs-lookup"><span data-stu-id="faa40-302">Be consistent in how you construct counterSpecifier values.</span></span>

<span data-ttu-id="faa40-303">Ha a `performanceCounters`, LAD mindig írja az adatokat az Azure storage-táblázathoz.</span><span class="sxs-lookup"><span data-stu-id="faa40-303">When you specify `performanceCounters`, LAD always writes data to a table in Azure storage.</span></span> <span data-ttu-id="faa40-304">Lehet írni a JSON-blobok és/vagy az Event Hubs ugyanazokat az adatokat, de nem tiltható le, a tábla adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="faa40-304">You can have the same data written to JSON blobs and/or Event Hubs, but you cannot disable storing data to a table.</span></span> <span data-ttu-id="faa40-305">A diagnosztikai bővítmény összes példánya azonos tárfióknév használatára konfigurált, és adja hozzá a metrikák és a naplók végpont ugyanahhoz a táblához.</span><span class="sxs-lookup"><span data-stu-id="faa40-305">All instances of the diagnostic extension configured to use the same storage account name and endpoint add their metrics and logs to the same table.</span></span> <span data-ttu-id="faa40-306">Túl sok virtuális gép tábla partícióra ír, az Azure képes szabályozni a írások ehhez a partícióhoz.</span><span class="sxs-lookup"><span data-stu-id="faa40-306">If too many VMs are writing to the same table partition, Azure can throttle writes to that partition.</span></span> <span data-ttu-id="faa40-307">A eventVolume beállítás azt eredményezi, 1 (kisméretű), 10 (közepes) keresztül terjedésének vagy 100 (nagy) különböző partíciók bejegyzések.</span><span class="sxs-lookup"><span data-stu-id="faa40-307">The eventVolume setting causes entries to be spread across 1 (Small), 10 (Medium), or 100 (Large) different partitions.</span></span> <span data-ttu-id="faa40-308">"Közepes" általában elegendő biztosítására a forgalom nem folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="faa40-308">Usually, "Medium" is sufficient to ensure traffic is not throttled.</span></span> <span data-ttu-id="faa40-309">Az Azure portál Azure metrikák jellemzője diagramok létrehozására vagy riasztásokat kiváltó használja ebben a táblázatban az adatokat.</span><span class="sxs-lookup"><span data-stu-id="faa40-309">The Azure Metrics feature of the Azure portal uses the data in this table to produce graphs or to trigger alerts.</span></span> <span data-ttu-id="faa40-310">A táblanév: ezek a karakterláncok a kapott:</span><span class="sxs-lookup"><span data-stu-id="faa40-310">The table name is the concatenation of these strings:</span></span>

* `WADMetrics`
* <span data-ttu-id="faa40-311">Az összesített értékek a táblában tárolt "scheduledTransferPeriod"</span><span class="sxs-lookup"><span data-stu-id="faa40-311">The "scheduledTransferPeriod" for the aggregated values stored in the table</span></span>
* `P10DV2S`
* <span data-ttu-id="faa40-312">Az űrlap "ÉÉÉÉHHNN", amely megváltoztatja minden 10 nap, dátum</span><span class="sxs-lookup"><span data-stu-id="faa40-312">A date, in the form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="faa40-313">Példák `WADMetricsPT1HP10DV2S20170410` és `WADMetricsPT1MP10DV2S20170609`.</span><span class="sxs-lookup"><span data-stu-id="faa40-313">Examples include `WADMetricsPT1HP10DV2S20170410` and `WADMetricsPT1MP10DV2S20170609`.</span></span>

#### <a name="syslogevents"></a><span data-ttu-id="faa40-314">syslogEvents</span><span class="sxs-lookup"><span data-stu-id="faa40-314">syslogEvents</span></span>

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

<span data-ttu-id="faa40-315">Ez a szakasz választható határozza meg a syslog alkalmazásnapló-események gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="faa40-315">This optional section controls the collection of log events from syslog.</span></span> <span data-ttu-id="faa40-316">A szakasz elhagyása esetén syslog-események nem minden rögzíti.</span><span class="sxs-lookup"><span data-stu-id="faa40-316">If the section is omitted, syslog events are not captured at all.</span></span>

<span data-ttu-id="faa40-317">A syslogEventConfiguration gyűjtemény szerepel egy bejegyzés minden egyes csomópontjára syslog-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="faa40-317">The syslogEventConfiguration collection has one entry for each syslog facility of interest.</span></span> <span data-ttu-id="faa40-318">Ha minSeverity "NONE" egy adott helyen, vagy az, hogy a létesítmény nem jelenik meg az elem minden, a rendszer rögzíti, hogy a létesítmény származó események.</span><span class="sxs-lookup"><span data-stu-id="faa40-318">If minSeverity is "NONE" for a particular facility, or if that facility does not appear in the element at all, no events from that facility are captured.</span></span>

<span data-ttu-id="faa40-319">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-319">Element</span></span> | <span data-ttu-id="faa40-320">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-320">Value</span></span>
------- | -----
<span data-ttu-id="faa40-321">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="faa40-321">sinks</span></span> | <span data-ttu-id="faa40-322">Egy vesszővel tagolt listája, amelyhez egyedi aktiválási naplót esemény közzé lesz téve nyelő.</span><span class="sxs-lookup"><span data-stu-id="faa40-322">A comma-separated list of names of sinks to which individual log events are published.</span></span> <span data-ttu-id="faa40-323">Minden felsorolt fogadó syslogEventConfiguration korlátozásai megfelelő összes naplózási események kerülnek közzétételre.</span><span class="sxs-lookup"><span data-stu-id="faa40-323">All log events matching the restrictions in syslogEventConfiguration are published to each listed sink.</span></span> <span data-ttu-id="faa40-324">Példa: "EHforsyslog"</span><span class="sxs-lookup"><span data-stu-id="faa40-324">Example: "EHforsyslog"</span></span>
<span data-ttu-id="faa40-325">facilityName</span><span class="sxs-lookup"><span data-stu-id="faa40-325">facilityName</span></span> | <span data-ttu-id="faa40-326">A syslog létesítmény nevét (például a "napló\_felhasználói" vagy "napló\_LOCAL0").</span><span class="sxs-lookup"><span data-stu-id="faa40-326">A syslog facility name (such as "LOG\_USER" or "LOG\_LOCAL0").</span></span> <span data-ttu-id="faa40-327">Az "létesítmény" című a [syslog man lap](http://man7.org/linux/man-pages/man3/syslog.3.html) teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="faa40-327">See the "facility" section of the [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for the full list.</span></span>
<span data-ttu-id="faa40-328">minSeverity</span><span class="sxs-lookup"><span data-stu-id="faa40-328">minSeverity</span></span> | <span data-ttu-id="faa40-329">A syslog súlyossági szint (például a "napló\_hiba" vagy "napló\_INFO").</span><span class="sxs-lookup"><span data-stu-id="faa40-329">A syslog severity level (such as "LOG\_ERR" or "LOG\_INFO").</span></span> <span data-ttu-id="faa40-330">Az "szint" című a [syslog man lap](http://man7.org/linux/man-pages/man3/syslog.3.html) teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="faa40-330">See the "level" section of the [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for the full list.</span></span> <span data-ttu-id="faa40-331">A bővítmény a létesítmény elérte vagy meghaladta a megadott szint küldött eseményeket rögzíti.</span><span class="sxs-lookup"><span data-stu-id="faa40-331">The extension captures events sent to the facility at or above the specified level.</span></span>

<span data-ttu-id="faa40-332">Ha a `syslogEvents`, LAD mindig írja az adatokat az Azure storage-táblázathoz.</span><span class="sxs-lookup"><span data-stu-id="faa40-332">When you specify `syslogEvents`, LAD always writes data to a table in Azure storage.</span></span> <span data-ttu-id="faa40-333">Lehet írni a JSON-blobok és/vagy az Event Hubs ugyanazokat az adatokat, de nem tiltható le, a tábla adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="faa40-333">You can have the same data written to JSON blobs and/or Event Hubs, but you cannot disable storing data to a table.</span></span> <span data-ttu-id="faa40-334">Ez a tábla particionáló működése megegyeznek a `performanceCounters`.</span><span class="sxs-lookup"><span data-stu-id="faa40-334">The partitioning behavior for this table is the same as described for `performanceCounters`.</span></span> <span data-ttu-id="faa40-335">A táblanév: ezek a karakterláncok a kapott:</span><span class="sxs-lookup"><span data-stu-id="faa40-335">The table name is the concatenation of these strings:</span></span>

* `LinuxSyslog`
* <span data-ttu-id="faa40-336">Az űrlap "ÉÉÉÉHHNN", amely megváltoztatja minden 10 nap, dátum</span><span class="sxs-lookup"><span data-stu-id="faa40-336">A date, in the form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="faa40-337">Példák `LinuxSyslog20170410` és `LinuxSyslog20170609`.</span><span class="sxs-lookup"><span data-stu-id="faa40-337">Examples include `LinuxSyslog20170410` and `LinuxSyslog20170609`.</span></span>

### <a name="perfcfg"></a><span data-ttu-id="faa40-338">perfCfg</span><span class="sxs-lookup"><span data-stu-id="faa40-338">perfCfg</span></span>

<span data-ttu-id="faa40-339">Ez a szakasz választható szabályozza tetszőleges végrehajtásának [OMI](https://github.com/Microsoft/omi) lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="faa40-339">This optional section controls execution of arbitrary [OMI](https://github.com/Microsoft/omi) queries.</span></span>

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

<span data-ttu-id="faa40-340">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-340">Element</span></span> | <span data-ttu-id="faa40-341">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-341">Value</span></span>
------- | -----
<span data-ttu-id="faa40-342">Namespace</span><span class="sxs-lookup"><span data-stu-id="faa40-342">namespace</span></span> | <span data-ttu-id="faa40-343">(választható) Az OMI névtér belül, amely hajtható végre a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="faa40-343">(optional) The OMI namespace within which the query should be executed.</span></span> <span data-ttu-id="faa40-344">Ha nincs megadva, az alapértelmezett érték: "legfelső szintű/scx", által megvalósított a [a System Center platformfüggetlen szolgáltatók](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span><span class="sxs-lookup"><span data-stu-id="faa40-344">If unspecified, the default value is "root/scx", implemented by the [System Center Cross-platform Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span></span>
<span data-ttu-id="faa40-345">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="faa40-345">query</span></span> | <span data-ttu-id="faa40-346">Az OMI lekérdezés végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="faa40-346">The OMI query to be executed.</span></span>
<span data-ttu-id="faa40-347">Tábla</span><span class="sxs-lookup"><span data-stu-id="faa40-347">table</span></span> | <span data-ttu-id="faa40-348">(választható) Az Azure storage tábla, a kijelölt tárfiókban lévő (lásd: [beállítások védett](#protected-settings)).</span><span class="sxs-lookup"><span data-stu-id="faa40-348">(optional) The Azure storage table, in the designated storage account (see [Protected settings](#protected-settings)).</span></span>
<span data-ttu-id="faa40-349">gyakoriság</span><span class="sxs-lookup"><span data-stu-id="faa40-349">frequency</span></span> | <span data-ttu-id="faa40-350">(választható) A lekérdezés végrehajtása között eltelt másodpercek száma.</span><span class="sxs-lookup"><span data-stu-id="faa40-350">(optional) The number of seconds between execution of the query.</span></span> <span data-ttu-id="faa40-351">Alapértelmezett értéke 300 (5 percig); minimális érték 15 másodpercre.</span><span class="sxs-lookup"><span data-stu-id="faa40-351">Default value is 300 (5 minutes); minimum value is 15 seconds.</span></span>
<span data-ttu-id="faa40-352">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="faa40-352">sinks</span></span> | <span data-ttu-id="faa40-353">(választható) További nyelő, amelyhez metrika eredmények nyers minta közzé kell tenni a neveket vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="faa40-353">(optional) A comma-separated list of names of additional sinks to which raw sample metric results should be published.</span></span> <span data-ttu-id="faa40-354">Nincs összesítési e nyers minták számított a bővítmény vagy Azure metrikákat.</span><span class="sxs-lookup"><span data-stu-id="faa40-354">No aggregation of these raw samples is computed by the extension or by Azure Metrics.</span></span>

<span data-ttu-id="faa40-355">"Table" vagy "fogadók esetében", vagy mindkettőt, meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="faa40-355">Either "table" or "sinks", or both, must be specified.</span></span>

### <a name="filelogs"></a><span data-ttu-id="faa40-356">fileLogs</span><span class="sxs-lookup"><span data-stu-id="faa40-356">fileLogs</span></span>

<span data-ttu-id="faa40-357">A rögzítés a naplófájlok szabályozza.</span><span class="sxs-lookup"><span data-stu-id="faa40-357">Controls the capture of log files.</span></span> <span data-ttu-id="faa40-358">LAD új szöveges sort rögzíti, mert a fájlt írás, és írja a táblázat sorainak és/vagy a megadott mosdók (JsonBlob vagy EventHub).</span><span class="sxs-lookup"><span data-stu-id="faa40-358">LAD captures new text lines as they are written to the file and writes them to table rows and/or any specified sinks (JsonBlob or EventHub).</span></span>

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

<span data-ttu-id="faa40-359">Elem</span><span class="sxs-lookup"><span data-stu-id="faa40-359">Element</span></span> | <span data-ttu-id="faa40-360">Érték</span><span class="sxs-lookup"><span data-stu-id="faa40-360">Value</span></span>
------- | -----
<span data-ttu-id="faa40-361">Fájl</span><span class="sxs-lookup"><span data-stu-id="faa40-361">file</span></span> | <span data-ttu-id="faa40-362">A teljes elérési útja a naplófájl figyelése és rögzítése.</span><span class="sxs-lookup"><span data-stu-id="faa40-362">The full pathname of the log file to be watched and captured.</span></span> <span data-ttu-id="faa40-363">A pathname nevet egyetlen fájl; nem egy könyvtár nevet és nem tartalmazhat helyettesítő karaktereket.</span><span class="sxs-lookup"><span data-stu-id="faa40-363">The pathname must name a single file; it cannot name a directory or contain wildcards.</span></span>
<span data-ttu-id="faa40-364">Tábla</span><span class="sxs-lookup"><span data-stu-id="faa40-364">table</span></span> | <span data-ttu-id="faa40-365">(választható) Az Azure storage tábla, a kijelölt tárfiókban (meghatározottak szerint a védett configuration), amelybe a "végéről" a fájl új sorok készültek.</span><span class="sxs-lookup"><span data-stu-id="faa40-365">(optional) The Azure storage table, in the designated storage account (as specified in the protected configuration), into which new lines from the "tail" of the file are written.</span></span>
<span data-ttu-id="faa40-366">fogadók esetében</span><span class="sxs-lookup"><span data-stu-id="faa40-366">sinks</span></span> | <span data-ttu-id="faa40-367">(választható) További nyelő küldött napló sorok a neveket vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="faa40-367">(optional) A comma-separated list of names of additional sinks to which log lines sent.</span></span>

<span data-ttu-id="faa40-368">"Table" vagy "fogadók esetében", vagy mindkettőt, meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="faa40-368">Either "table" or "sinks", or both, must be specified.</span></span>

## <a name="metrics-supported-by-the-builtin-provider"></a><span data-ttu-id="faa40-369">A beépített szolgáltató támogatja a mérőszámok</span><span class="sxs-lookup"><span data-stu-id="faa40-369">Metrics supported by the builtin provider</span></span>

<span data-ttu-id="faa40-370">A beépített metrika szolgáltató a forrása a metrikák a legérdekesebb a felhasználók széles körét.</span><span class="sxs-lookup"><span data-stu-id="faa40-370">The builtin metric provider is a source of metrics most interesting to a broad set of users.</span></span> <span data-ttu-id="faa40-371">A metrikák öt széleskörű osztályok sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="faa40-371">These metrics fall into five broad classes:</span></span>

* <span data-ttu-id="faa40-372">Processzor</span><span class="sxs-lookup"><span data-stu-id="faa40-372">Processor</span></span>
* <span data-ttu-id="faa40-373">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="faa40-373">Memory</span></span>
* <span data-ttu-id="faa40-374">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="faa40-374">Network</span></span>
* <span data-ttu-id="faa40-375">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="faa40-375">Filesystem</span></span>
* <span data-ttu-id="faa40-376">Lemez</span><span class="sxs-lookup"><span data-stu-id="faa40-376">Disk</span></span>

### <a name="builtin-metrics-for-the-processor-class"></a><span data-ttu-id="faa40-377">a processzor osztály beépített metrikák</span><span class="sxs-lookup"><span data-stu-id="faa40-377">builtin metrics for the Processor class</span></span>

<span data-ttu-id="faa40-378">A processzor osztály a mérőszámok tájékoztatást ad azokról a virtuális gép processzor kihasználtsága.</span><span class="sxs-lookup"><span data-stu-id="faa40-378">The Processor class of metrics provides information about processor usage in the VM.</span></span> <span data-ttu-id="faa40-379">Százalékos összesítésekor eredménye átlagos összes processzorok között.</span><span class="sxs-lookup"><span data-stu-id="faa40-379">When aggregating percentages, the result is the average across all CPUs.</span></span> <span data-ttu-id="faa40-380">A virtuális gép két, alapszintű egy alapvető 100 %-os elfoglalt volt, és a másik 100 %-os üresjárati, ha a jelentésben szereplő PercentIdleTime pedig 50.</span><span class="sxs-lookup"><span data-stu-id="faa40-380">In a two core VM, if one core was 100% busy and the other was 100% idle, the reported PercentIdleTime would be 50.</span></span> <span data-ttu-id="faa40-381">Ha minden core 50 % azonos időszakára vonatkozó elfoglalt volt, a jelentésben szereplő eredmény is pedig 50.</span><span class="sxs-lookup"><span data-stu-id="faa40-381">If each core was 50% busy for the same period, the reported result would also be 50.</span></span> <span data-ttu-id="faa40-382">A jelentett PercentIdleTime négy alapvető virtuális gép, egy alapvető 100 % foglalt, és a többi üresjárati, 75 lenne.</span><span class="sxs-lookup"><span data-stu-id="faa40-382">In a four core VM, with one core 100% busy and the others idle, the reported PercentIdleTime would be 75.</span></span>

<span data-ttu-id="faa40-383">A számláló</span><span class="sxs-lookup"><span data-stu-id="faa40-383">counter</span></span> | <span data-ttu-id="faa40-384">Jelentése</span><span class="sxs-lookup"><span data-stu-id="faa40-384">Meaning</span></span>
------- | -------
<span data-ttu-id="faa40-385">PercentIdleTime</span><span class="sxs-lookup"><span data-stu-id="faa40-385">PercentIdleTime</span></span> | <span data-ttu-id="faa40-386">A összesítési időszakban, hogy a processzorok volt végrehajtása a kernel üresjárati hurok idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="faa40-386">Percentage of time during the aggregation window that processors were executing the kernel idle loop</span></span>
<span data-ttu-id="faa40-387">PercentProcessorTime</span><span class="sxs-lookup"><span data-stu-id="faa40-387">PercentProcessorTime</span></span> | <span data-ttu-id="faa40-388">Nem üresjárati szálat idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="faa40-388">Percentage of time executing a non-idle thread</span></span>
<span data-ttu-id="faa40-389">PercentIOWaitTime</span><span class="sxs-lookup"><span data-stu-id="faa40-389">PercentIOWaitTime</span></span> | <span data-ttu-id="faa40-390">Várakozás az I/O műveletek elvégzéséhez idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="faa40-390">Percentage of time waiting for IO operations to complete</span></span>
<span data-ttu-id="faa40-391">PercentInterruptTime</span><span class="sxs-lookup"><span data-stu-id="faa40-391">PercentInterruptTime</span></span> | <span data-ttu-id="faa40-392">Hardver vagy szoftver megszakítások, DPC-k (késleltetett eljáráshívások) végrehajtása idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="faa40-392">Percentage of time executing hardware/software interrupts and DPCs (deferred procedure calls)</span></span>
<span data-ttu-id="faa40-393">PercentUserTime</span><span class="sxs-lookup"><span data-stu-id="faa40-393">PercentUserTime</span></span> | <span data-ttu-id="faa40-394">Az összesítési időszak alatt nem üresjárati idő a fordított idő százalékos aránya a felhasználó több normál prioritással</span><span class="sxs-lookup"><span data-stu-id="faa40-394">Of non-idle time during the aggregation window, the percentage of time spent in user more at normal priority</span></span>
<span data-ttu-id="faa40-395">PercentNiceTime</span><span class="sxs-lookup"><span data-stu-id="faa40-395">PercentNiceTime</span></span> | <span data-ttu-id="faa40-396">Nem üresjárati időt százalékos süllyesztett (jó) prioritással töltött</span><span class="sxs-lookup"><span data-stu-id="faa40-396">Of non-idle time, the percentage spent at lowered (nice) priority</span></span>
<span data-ttu-id="faa40-397">PercentPrivilegedTime</span><span class="sxs-lookup"><span data-stu-id="faa40-397">PercentPrivilegedTime</span></span> | <span data-ttu-id="faa40-398">Nem üresjárati időt százalékos töltött védett (kernel) módban</span><span class="sxs-lookup"><span data-stu-id="faa40-398">Of non-idle time, the percentage spent in privileged (kernel) mode</span></span>

<span data-ttu-id="faa40-399">Az első négy számlálók kell összeg 100 %.</span><span class="sxs-lookup"><span data-stu-id="faa40-399">The first four counters should sum to 100%.</span></span> <span data-ttu-id="faa40-400">Az utolsó három is számlálók összege 100 %; a PercentProcessorTime, PercentIOWaitTime és PercentInterruptTime azok tovább.</span><span class="sxs-lookup"><span data-stu-id="faa40-400">The last three counters also sum to 100%; they subdivide the sum of PercentProcessorTime, PercentIOWaitTime, and PercentInterruptTime.</span></span>

<span data-ttu-id="faa40-401">Az összes processzor gyűjtődnek egyetlen mérőszám beszerzéséhez beállítása `"condition": "IsAggregate=TRUE"`.</span><span class="sxs-lookup"><span data-stu-id="faa40-401">To obtain a single metric aggregated across all processors, set `"condition": "IsAggregate=TRUE"`.</span></span> <span data-ttu-id="faa40-402">Megadott processzorsebességgel rendelkező metrika beszerzése, például egy négy, a második logikai processzor virtuális gép központi, állítsa be `"condition": "Name=\\"1\\""`.</span><span class="sxs-lookup"><span data-stu-id="faa40-402">To obtain a metric for a specific processor, such as the second logical processor of a four core VM, set `"condition": "Name=\\"1\\""`.</span></span> <span data-ttu-id="faa40-403">A tartományban vannak logikai processzor számok `[0..n-1]`.</span><span class="sxs-lookup"><span data-stu-id="faa40-403">Logical processor numbers are in the range `[0..n-1]`.</span></span>

### <a name="builtin-metrics-for-the-memory-class"></a><span data-ttu-id="faa40-404">a beépített metrikákat a memória-osztály</span><span class="sxs-lookup"><span data-stu-id="faa40-404">builtin metrics for the Memory class</span></span>

<span data-ttu-id="faa40-405">A memória az osztály a mérőszámok memóriafelhasználás a lapozást, és áttelepíteni a forráskörnyezetból információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="faa40-405">The Memory class of metrics provides information about memory utilization, paging, and swapping.</span></span>

<span data-ttu-id="faa40-406">A számláló</span><span class="sxs-lookup"><span data-stu-id="faa40-406">counter</span></span> | <span data-ttu-id="faa40-407">Jelentése</span><span class="sxs-lookup"><span data-stu-id="faa40-407">Meaning</span></span>
------- | -------
<span data-ttu-id="faa40-408">AvailableMemory</span><span class="sxs-lookup"><span data-stu-id="faa40-408">AvailableMemory</span></span> | <span data-ttu-id="faa40-409">Rendelkezésre álló fizikai memória MIB</span><span class="sxs-lookup"><span data-stu-id="faa40-409">Available physical memory in MiB</span></span>
<span data-ttu-id="faa40-410">PercentAvailableMemory</span><span class="sxs-lookup"><span data-stu-id="faa40-410">PercentAvailableMemory</span></span> | <span data-ttu-id="faa40-411">A teljes memória százalékos rendelkezésre álló fizikai memória</span><span class="sxs-lookup"><span data-stu-id="faa40-411">Available physical memory as a percent of total memory</span></span>
<span data-ttu-id="faa40-412">UsedMemory</span><span class="sxs-lookup"><span data-stu-id="faa40-412">UsedMemory</span></span> | <span data-ttu-id="faa40-413">Használatban lévő fizikai memória (MiB)</span><span class="sxs-lookup"><span data-stu-id="faa40-413">In-use physical memory (MiB)</span></span>
<span data-ttu-id="faa40-414">PercentUsedMemory</span><span class="sxs-lookup"><span data-stu-id="faa40-414">PercentUsedMemory</span></span> | <span data-ttu-id="faa40-415">A teljes memória százalékos a használatban lévő fizikai memória</span><span class="sxs-lookup"><span data-stu-id="faa40-415">In-use physical memory as a percent of total memory</span></span>
<span data-ttu-id="faa40-416">PagesPerSec</span><span class="sxs-lookup"><span data-stu-id="faa40-416">PagesPerSec</span></span> | <span data-ttu-id="faa40-417">Teljes lapozófájl (olvasás/írás)</span><span class="sxs-lookup"><span data-stu-id="faa40-417">Total paging (read/write)</span></span>
<span data-ttu-id="faa40-418">PagesReadPerSec</span><span class="sxs-lookup"><span data-stu-id="faa40-418">PagesReadPerSec</span></span> | <span data-ttu-id="faa40-419">Lapok olvasni háttértár (lapozófájl programfájlt, leképezett fájlt, stb.)</span><span class="sxs-lookup"><span data-stu-id="faa40-419">Pages read from backing store (swap file, program file, mapped file, etc.)</span></span>
<span data-ttu-id="faa40-420">PagesWrittenPerSec</span><span class="sxs-lookup"><span data-stu-id="faa40-420">PagesWrittenPerSec</span></span> | <span data-ttu-id="faa40-421">Lapok írni a biztonsági tár (lapozófájl, leképezett fájlt, stb.)</span><span class="sxs-lookup"><span data-stu-id="faa40-421">Pages written to backing store (swap file, mapped file, etc.)</span></span>
<span data-ttu-id="faa40-422">AvailableSwap</span><span class="sxs-lookup"><span data-stu-id="faa40-422">AvailableSwap</span></span> | <span data-ttu-id="faa40-423">Nem használt lapozóterület (MiB)</span><span class="sxs-lookup"><span data-stu-id="faa40-423">Unused swap space (MiB)</span></span>
<span data-ttu-id="faa40-424">PercentAvailableSwap</span><span class="sxs-lookup"><span data-stu-id="faa40-424">PercentAvailableSwap</span></span> | <span data-ttu-id="faa40-425">Nem használt lapozóterület teljes lapozófájl-kapacitás százalékában</span><span class="sxs-lookup"><span data-stu-id="faa40-425">Unused swap space as a percentage of total swap</span></span>
<span data-ttu-id="faa40-426">UsedSwap</span><span class="sxs-lookup"><span data-stu-id="faa40-426">UsedSwap</span></span> | <span data-ttu-id="faa40-427">Használatban lévő lapozóterület (MiB)</span><span class="sxs-lookup"><span data-stu-id="faa40-427">In-use swap space (MiB)</span></span>
<span data-ttu-id="faa40-428">PercentUsedSwap</span><span class="sxs-lookup"><span data-stu-id="faa40-428">PercentUsedSwap</span></span> | <span data-ttu-id="faa40-429">Használatban lévő lapozóterület teljes lapozófájl-kapacitás százalékában</span><span class="sxs-lookup"><span data-stu-id="faa40-429">In-use swap space as a percentage of total swap</span></span>

<span data-ttu-id="faa40-430">Ez az osztály a mérőszámok csak egyetlen példány van.</span><span class="sxs-lookup"><span data-stu-id="faa40-430">This class of metrics has only a single instance.</span></span> <span data-ttu-id="faa40-431">A "feltétel" attribútum nem hasznos beállításokkal rendelkezik, és megadni.</span><span class="sxs-lookup"><span data-stu-id="faa40-431">The "condition" attribute has no useful settings and should be omitted.</span></span>

### <a name="builtin-metrics-for-the-network-class"></a><span data-ttu-id="faa40-432">a hálózati osztály beépített metrikák</span><span class="sxs-lookup"><span data-stu-id="faa40-432">builtin metrics for the Network class</span></span>

<span data-ttu-id="faa40-433">A hálózati osztály a mérőszámok tevékenységről szolgáltat információkat hálózati az egyes hálózati adaptereken indítása óta.</span><span class="sxs-lookup"><span data-stu-id="faa40-433">The Network class of metrics provides information about network activity on an individual network interfaces since boot.</span></span> <span data-ttu-id="faa40-434">LAD nem biztosít sávszélesség metrikákat, amelyek a gazdagép-metrikák kérhető.</span><span class="sxs-lookup"><span data-stu-id="faa40-434">LAD does not expose bandwidth metrics, which can be retrieved from host metrics.</span></span>

<span data-ttu-id="faa40-435">A számláló</span><span class="sxs-lookup"><span data-stu-id="faa40-435">counter</span></span> | <span data-ttu-id="faa40-436">Jelentése</span><span class="sxs-lookup"><span data-stu-id="faa40-436">Meaning</span></span>
------- | -------
<span data-ttu-id="faa40-437">BytesTransmitted</span><span class="sxs-lookup"><span data-stu-id="faa40-437">BytesTransmitted</span></span> | <span data-ttu-id="faa40-438">Rendszerindítás óta küldött bájtok száma összesen</span><span class="sxs-lookup"><span data-stu-id="faa40-438">Total bytes sent since boot</span></span>
<span data-ttu-id="faa40-439">BytesReceived</span><span class="sxs-lookup"><span data-stu-id="faa40-439">BytesReceived</span></span> | <span data-ttu-id="faa40-440">Rendszerindítás óta fogadott összes bájt</span><span class="sxs-lookup"><span data-stu-id="faa40-440">Total bytes received since boot</span></span>
<span data-ttu-id="faa40-441">BytesTotal</span><span class="sxs-lookup"><span data-stu-id="faa40-441">BytesTotal</span></span> | <span data-ttu-id="faa40-442">Küldött vagy indítása óta fogadott bájtok teljes száma</span><span class="sxs-lookup"><span data-stu-id="faa40-442">Total bytes sent or received since boot</span></span>
<span data-ttu-id="faa40-443">PacketsTransmitted</span><span class="sxs-lookup"><span data-stu-id="faa40-443">PacketsTransmitted</span></span> | <span data-ttu-id="faa40-444">Rendszerindítás óta küldött csomagok száma összesen</span><span class="sxs-lookup"><span data-stu-id="faa40-444">Total packets sent since boot</span></span>
<span data-ttu-id="faa40-445">PacketsReceived</span><span class="sxs-lookup"><span data-stu-id="faa40-445">PacketsReceived</span></span> | <span data-ttu-id="faa40-446">Rendszerindítás óta fogadott csomagok száma összesen</span><span class="sxs-lookup"><span data-stu-id="faa40-446">Total packets received since boot</span></span>
<span data-ttu-id="faa40-447">TotalRxErrors</span><span class="sxs-lookup"><span data-stu-id="faa40-447">TotalRxErrors</span></span> | <span data-ttu-id="faa40-448">A fogadási hibák száma indítása óta</span><span class="sxs-lookup"><span data-stu-id="faa40-448">Number of receive errors since boot</span></span>
<span data-ttu-id="faa40-449">TotalTxErrors</span><span class="sxs-lookup"><span data-stu-id="faa40-449">TotalTxErrors</span></span> | <span data-ttu-id="faa40-450">-Küldési hibák száma indítása óta</span><span class="sxs-lookup"><span data-stu-id="faa40-450">Number of transmit errors since boot</span></span>
<span data-ttu-id="faa40-451">TotalCollisions</span><span class="sxs-lookup"><span data-stu-id="faa40-451">TotalCollisions</span></span> | <span data-ttu-id="faa40-452">Rendszerindítás óta a hálózati portok által jelentett ütközések száma</span><span class="sxs-lookup"><span data-stu-id="faa40-452">Number of collisions reported by the network ports since boot</span></span>

 <span data-ttu-id="faa40-453">Bár ez az osztály van instanced, LAD nem támogatja az összes hálózati eszköz gyűjtődnek rögzítésével hálózati metrikákat.</span><span class="sxs-lookup"><span data-stu-id="faa40-453">Although this class is instanced, LAD does not support capturing Network metrics aggregated across all network devices.</span></span> <span data-ttu-id="faa40-454">Állítsa be a metrika egy adott felület esetében eth0, például az beszerzése `"condition": "InstanceID=\\"eth0\\""`.</span><span class="sxs-lookup"><span data-stu-id="faa40-454">To obtain the metrics for a specific interface, such as eth0, set `"condition": "InstanceID=\\"eth0\\""`.</span></span>

### <a name="builtin-metrics-for-the-filesystem-class"></a><span data-ttu-id="faa40-455">a fájlrendszer osztály beépített metrikák</span><span class="sxs-lookup"><span data-stu-id="faa40-455">builtin metrics for the Filesystem class</span></span>

<span data-ttu-id="faa40-456">A fájlrendszer osztály a mérőszámok filesystem használati információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="faa40-456">The Filesystem class of metrics provides information about filesystem usage.</span></span> <span data-ttu-id="faa40-457">Abszolút és százalékos értéket jelentett, akkor szokásos felhasználóhoz (nem a legfelső szintű) megjelenik.</span><span class="sxs-lookup"><span data-stu-id="faa40-457">Absolute and percentage values are reported as they'd be displayed to an ordinary user (not root).</span></span>

<span data-ttu-id="faa40-458">A számláló</span><span class="sxs-lookup"><span data-stu-id="faa40-458">counter</span></span> | <span data-ttu-id="faa40-459">Jelentése</span><span class="sxs-lookup"><span data-stu-id="faa40-459">Meaning</span></span>
------- | -------
<span data-ttu-id="faa40-460">FreeSpace</span><span class="sxs-lookup"><span data-stu-id="faa40-460">FreeSpace</span></span> | <span data-ttu-id="faa40-461">Szabad lemezterület (bájt)</span><span class="sxs-lookup"><span data-stu-id="faa40-461">Available disk space in bytes</span></span>
<span data-ttu-id="faa40-462">UsedSpace</span><span class="sxs-lookup"><span data-stu-id="faa40-462">UsedSpace</span></span> | <span data-ttu-id="faa40-463">A felhasznált lemezterület (bájt)</span><span class="sxs-lookup"><span data-stu-id="faa40-463">Used disk space in bytes</span></span>
<span data-ttu-id="faa40-464">PercentFreeSpace</span><span class="sxs-lookup"><span data-stu-id="faa40-464">PercentFreeSpace</span></span> | <span data-ttu-id="faa40-465">Szabad terület százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="faa40-465">Percentage free space</span></span>
<span data-ttu-id="faa40-466">PercentUsedSpace</span><span class="sxs-lookup"><span data-stu-id="faa40-466">PercentUsedSpace</span></span> | <span data-ttu-id="faa40-467">A felhasznált terület százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="faa40-467">Percentage used space</span></span>
<span data-ttu-id="faa40-468">PercentFreeInodes</span><span class="sxs-lookup"><span data-stu-id="faa40-468">PercentFreeInodes</span></span> | <span data-ttu-id="faa40-469">Nem használt Inode-OK százaléka</span><span class="sxs-lookup"><span data-stu-id="faa40-469">Percentage of unused inodes</span></span>
<span data-ttu-id="faa40-470">PercentUsedInodes</span><span class="sxs-lookup"><span data-stu-id="faa40-470">PercentUsedInodes</span></span> | <span data-ttu-id="faa40-471">Összesítve közötti összes fájlrendszerek lefoglalt (használatban) Inode-OK százaléka</span><span class="sxs-lookup"><span data-stu-id="faa40-471">Percentage of allocated (in use) inodes summed across all filesystems</span></span>
<span data-ttu-id="faa40-472">BytesReadPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-472">BytesReadPerSecond</span></span> | <span data-ttu-id="faa40-473">Másodpercenként olvasott bájtok száma</span><span class="sxs-lookup"><span data-stu-id="faa40-473">Bytes read per second</span></span>
<span data-ttu-id="faa40-474">BytesWrittenPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-474">BytesWrittenPerSecond</span></span> | <span data-ttu-id="faa40-475">Másodpercenként írt bájtok</span><span class="sxs-lookup"><span data-stu-id="faa40-475">Bytes written per second</span></span>
<span data-ttu-id="faa40-476">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-476">BytesPerSecond</span></span> | <span data-ttu-id="faa40-477">Bájt nem írható és olvasható / másodperc</span><span class="sxs-lookup"><span data-stu-id="faa40-477">Bytes read or written per second</span></span>
<span data-ttu-id="faa40-478">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-478">ReadsPerSecond</span></span> | <span data-ttu-id="faa40-479">Olvasási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="faa40-479">Read operations per second</span></span>
<span data-ttu-id="faa40-480">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-480">WritesPerSecond</span></span> | <span data-ttu-id="faa40-481">Írási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="faa40-481">Write operations per second</span></span>
<span data-ttu-id="faa40-482">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-482">TransfersPerSecond</span></span> | <span data-ttu-id="faa40-483">Olvasási vagy írási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="faa40-483">Read or write operations per second</span></span>

<span data-ttu-id="faa40-484">Összesített értékeket fájlrendszerek keresztül érhető el úgy, hogy `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="faa40-484">Aggregated values across all file systems can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="faa40-485">Például egy adott csatlakoztatott fájlrendszer értékei "/ mnt", úgy, hogy szerezhetők `"condition": 'Name="/mnt"'`.</span><span class="sxs-lookup"><span data-stu-id="faa40-485">Values for a specific mounted file system, such as "/mnt", can be obtained by setting `"condition": 'Name="/mnt"'`.</span></span>

### <a name="builtin-metrics-for-the-disk-class"></a><span data-ttu-id="faa40-486">a lemez osztály beépített metrikák</span><span class="sxs-lookup"><span data-stu-id="faa40-486">builtin metrics for the Disk class</span></span>

<span data-ttu-id="faa40-487">A lemez osztály a mérőszámok eszköz lemezhasználati információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="faa40-487">The Disk class of metrics provides information about disk device usage.</span></span> <span data-ttu-id="faa40-488">A statisztikai információk a teljes meghajtót vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="faa40-488">These statistics apply to the entire drive.</span></span> <span data-ttu-id="faa40-489">Ha egy eszközön több fájlrendszereket, az eszköznek a számlálók vannak, gyakorlatilag az összes gyűjtődnek.</span><span class="sxs-lookup"><span data-stu-id="faa40-489">If there are multiple file systems on a device, the counters for that device are, effectively, aggregated across all of them.</span></span>

<span data-ttu-id="faa40-490">A számláló</span><span class="sxs-lookup"><span data-stu-id="faa40-490">counter</span></span> | <span data-ttu-id="faa40-491">Jelentése</span><span class="sxs-lookup"><span data-stu-id="faa40-491">Meaning</span></span>
------- | -------
<span data-ttu-id="faa40-492">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-492">ReadsPerSecond</span></span> | <span data-ttu-id="faa40-493">Olvasási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="faa40-493">Read operations per second</span></span>
<span data-ttu-id="faa40-494">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-494">WritesPerSecond</span></span> | <span data-ttu-id="faa40-495">Írási műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="faa40-495">Write operations per second</span></span>
<span data-ttu-id="faa40-496">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-496">TransfersPerSecond</span></span> | <span data-ttu-id="faa40-497">Teljes műveletek másodpercenkénti száma</span><span class="sxs-lookup"><span data-stu-id="faa40-497">Total operations per second</span></span>
<span data-ttu-id="faa40-498">AverageReadTime</span><span class="sxs-lookup"><span data-stu-id="faa40-498">AverageReadTime</span></span> | <span data-ttu-id="faa40-499">Olvasási művelet átlagos másodpercben</span><span class="sxs-lookup"><span data-stu-id="faa40-499">Average seconds per read operation</span></span>
<span data-ttu-id="faa40-500">AverageWriteTime</span><span class="sxs-lookup"><span data-stu-id="faa40-500">AverageWriteTime</span></span> | <span data-ttu-id="faa40-501">Írási művelet átlagos másodpercben</span><span class="sxs-lookup"><span data-stu-id="faa40-501">Average seconds per write operation</span></span>
<span data-ttu-id="faa40-502">AverageTransferTime</span><span class="sxs-lookup"><span data-stu-id="faa40-502">AverageTransferTime</span></span> | <span data-ttu-id="faa40-503">Művelet átlagos másodpercben</span><span class="sxs-lookup"><span data-stu-id="faa40-503">Average seconds per operation</span></span>
<span data-ttu-id="faa40-504">AverageDiskQueueLength</span><span class="sxs-lookup"><span data-stu-id="faa40-504">AverageDiskQueueLength</span></span> | <span data-ttu-id="faa40-505">Várólistára helyezett lemezen műveletek átlagos száma</span><span class="sxs-lookup"><span data-stu-id="faa40-505">Average number of queued disk operations</span></span>
<span data-ttu-id="faa40-506">ReadBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-506">ReadBytesPerSecond</span></span> | <span data-ttu-id="faa40-507">A másodpercenként beolvasott bájtok száma</span><span class="sxs-lookup"><span data-stu-id="faa40-507">Number of bytes read per second</span></span>
<span data-ttu-id="faa40-508">WriteBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-508">WriteBytesPerSecond</span></span> | <span data-ttu-id="faa40-509">Másodpercenként írt bájtok száma</span><span class="sxs-lookup"><span data-stu-id="faa40-509">Number of bytes written per second</span></span>
<span data-ttu-id="faa40-510">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="faa40-510">BytesPerSecond</span></span> | <span data-ttu-id="faa40-511">Olvassa el és másodpercenként írt bájtok száma</span><span class="sxs-lookup"><span data-stu-id="faa40-511">Number of bytes read or written per second</span></span>

<span data-ttu-id="faa40-512">Minden lemezeken összesített értékeket szerezhető be úgy, hogy `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="faa40-512">Aggregated values across all disks can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="faa40-513">Ahhoz, hogy egy adott eszköz (például/dev/sdf1) adatait, állítsa be `"condition": "Name=\\"/dev/sdf1\\""`.</span><span class="sxs-lookup"><span data-stu-id="faa40-513">To get information for a specific device (for example, /dev/sdf1), set `"condition": "Name=\\"/dev/sdf1\\""`.</span></span>

## <a name="installing-and-configuring-lad-30-via-cli"></a><span data-ttu-id="faa40-514">Telepítése és konfigurálása LAD 3.0 parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="faa40-514">Installing and configuring LAD 3.0 via CLI</span></span>

<span data-ttu-id="faa40-515">Ha a védett beállítások PrivateConfig.json fájlban, és a nyilvános konfigurációs adatait a PublicConfig.json, futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="faa40-515">Assuming your protected settings are in the file PrivateConfig.json and your public configuration information is in PublicConfig.json, run this command:</span></span>

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

<span data-ttu-id="faa40-516">A parancs feltételezi, hogy használja az Azure CLI Azure Resource Manager (arm) módját.</span><span class="sxs-lookup"><span data-stu-id="faa40-516">The command assumes you are using the Azure Resource Management mode (arm) of the Azure CLI.</span></span> <span data-ttu-id="faa40-517">LAD konfigurálása a klasszikus üzembe helyezési modell (ASM) virtuális gépeket, váltson "asm" módra (`azure config mode asm`), és hagyja ki ezt a parancsot az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="faa40-517">To configure LAD for classic deployment model (ASM) VMs, switch to "asm" mode (`azure config mode asm`) and omit the resource group name in the command.</span></span> <span data-ttu-id="faa40-518">További információkért lásd: a [platformfüggetlen parancssori felület dokumentáció](https://docs.microsoft.com/azure/xplat-cli-connect).</span><span class="sxs-lookup"><span data-stu-id="faa40-518">For more information, see the [cross-platform CLI documentation](https://docs.microsoft.com/azure/xplat-cli-connect).</span></span>

## <a name="an-example-lad-30-configuration"></a><span data-ttu-id="faa40-519">Egy példa LAD 3.0 konfiguráció</span><span class="sxs-lookup"><span data-stu-id="faa40-519">An example LAD 3.0 configuration</span></span>

<span data-ttu-id="faa40-520">A fenti definíciók alapján, ez a minta LAD 3.0 bővítménykonfiguráció rövid.</span><span class="sxs-lookup"><span data-stu-id="faa40-520">Based on the preceding definitions, here's a sample LAD 3.0 extension configuration with some explanation.</span></span> <span data-ttu-id="faa40-521">Szeretné alkalmazni ezt a mintát a helyzet, akkor érdemes használni a saját tárfiók neve, a fiók SAS-jogkivonat és a EventHubs SAS-tokenje.</span><span class="sxs-lookup"><span data-stu-id="faa40-521">To apply this sample to your case, you should use your own storage account name, account SAS token, and EventHubs SAS tokens.</span></span>

### <a name="privateconfigjson"></a><span data-ttu-id="faa40-522">PrivateConfig.json</span><span class="sxs-lookup"><span data-stu-id="faa40-522">PrivateConfig.json</span></span>

<span data-ttu-id="faa40-523">Ezek a személyes beállítások konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="faa40-523">These private settings configure:</span></span>

* <span data-ttu-id="faa40-524">a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="faa40-524">a storage account</span></span>
* <span data-ttu-id="faa40-525">a megfelelő fiók SAS-jogkivonat</span><span class="sxs-lookup"><span data-stu-id="faa40-525">a matching account SAS token</span></span>
* <span data-ttu-id="faa40-526">több fogadók esetében (JsonBlob vagy az SAS-tokenje EventHubs)</span><span class="sxs-lookup"><span data-stu-id="faa40-526">several sinks (JsonBlob or EventHubs with SAS tokens)</span></span>

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

### <a name="publicconfigjson"></a><span data-ttu-id="faa40-527">PublicConfig.json</span><span class="sxs-lookup"><span data-stu-id="faa40-527">PublicConfig.json</span></span>

<span data-ttu-id="faa40-528">A nyilvános beállítások okozhat a LAD:</span><span class="sxs-lookup"><span data-stu-id="faa40-528">These public settings cause LAD to:</span></span>

* <span data-ttu-id="faa40-529">A százalékos processzoridő és használt-terület metrikák feltöltése a `WADMetrics*` tábla</span><span class="sxs-lookup"><span data-stu-id="faa40-529">Upload percent-processor-time and used-disk-space metrics to the `WADMetrics*` table</span></span>
* <span data-ttu-id="faa40-530">Töltse fel üzenetek a syslog létesítmény "user" és a súlyosság "Infó" a `LinuxSyslog*` tábla</span><span class="sxs-lookup"><span data-stu-id="faa40-530">Upload messages from syslog facility "user" and severity "info" to the `LinuxSyslog*` table</span></span>
* <span data-ttu-id="faa40-531">Töltse fel az elnevezett nyers OMI lekérdezési eredmények (PercentProcessorTime és PercentIdleTime) `LinuxCPU` tábla</span><span class="sxs-lookup"><span data-stu-id="faa40-531">Upload raw OMI query results (PercentProcessorTime and PercentIdleTime) to the named `LinuxCPU` table</span></span>
* <span data-ttu-id="faa40-532">Töltse fel a fájl sorainak hozzáfűzött `/var/log/myladtestlog` számára a `MyLadTestLog` tábla</span><span class="sxs-lookup"><span data-stu-id="faa40-532">Upload appended lines in file `/var/log/myladtestlog` to the `MyLadTestLog` table</span></span>

<span data-ttu-id="faa40-533">Minden esetben adatokat is feltöltött:</span><span class="sxs-lookup"><span data-stu-id="faa40-533">In each case, data is also uploaded to:</span></span>

* <span data-ttu-id="faa40-534">Az Azure Blob storage (a tároló neve: a JsonBlob fogadó meghatározottak szerint)</span><span class="sxs-lookup"><span data-stu-id="faa40-534">Azure Blob storage (container name is as defined in the JsonBlob sink)</span></span>
* <span data-ttu-id="faa40-535">EventHubs végpont (meghatározottak szerint a EventHubs fogadó)</span><span class="sxs-lookup"><span data-stu-id="faa40-535">EventHubs endpoint (as specified in the EventHubs sink)</span></span>

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

<span data-ttu-id="faa40-536">A `resourceId` konfigurációjában egyeznie kell, hogy a virtuális gép vagy virtuálisgép-méretezési állítsa be.</span><span class="sxs-lookup"><span data-stu-id="faa40-536">The `resourceId` in the configuration must match that of the VM or the virtual machine scale set.</span></span>

* <span data-ttu-id="faa40-537">Az Azure platform metrikák diagramkészítési és riasztás tudja az erőforrás-azonosítója a virtuális gép dolgozik.</span><span class="sxs-lookup"><span data-stu-id="faa40-537">Azure platform metrics charting and alerting knows the resourceId of the VM you're working on.</span></span> <span data-ttu-id="faa40-538">Várhatóan a keresési kulcs megtalálta az adatokat a virtuális gép az erőforrás-azonosítója használatával.</span><span class="sxs-lookup"><span data-stu-id="faa40-538">It expects to find the data for your VM using the resourceId the lookup key.</span></span>
* <span data-ttu-id="faa40-539">Azure automatikus skálázás használatakor, az erőforrás-azonosítója az automatikus skálázás konfigurációban meg kell egyeznie az erőforrás-azonosítója LAD használják.</span><span class="sxs-lookup"><span data-stu-id="faa40-539">If you use Azure autoscale, the resourceId in the autoscale configuration must match the resourceId used by LAD.</span></span>
* <span data-ttu-id="faa40-540">Az erőforrás-azonosítója beépített LAD által írt JsonBlobs nevét.</span><span class="sxs-lookup"><span data-stu-id="faa40-540">The resourceId is built into the names of JsonBlobs written by LAD.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="faa40-541">Az adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="faa40-541">View your data</span></span>

<span data-ttu-id="faa40-542">Az Azure portál segítségével teljesítményadatainak megjelenítéséhez, vagy állítson be riasztásokat:</span><span class="sxs-lookup"><span data-stu-id="faa40-542">Use the Azure portal to view performance data or set alerts:</span></span>

![Kép](./media/diagnostic-extension/graph_metrics.png)

<span data-ttu-id="faa40-544">A `performanceCounters` adatok mindig egy Azure Storage táblázatban vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="faa40-544">The `performanceCounters` data are always stored in an Azure Storage table.</span></span> <span data-ttu-id="faa40-545">Az Azure Storage API-k sok nyelvekhez és platformokhoz érhetők el.</span><span class="sxs-lookup"><span data-stu-id="faa40-545">Azure Storage APIs are available for many languages and platforms.</span></span>

<span data-ttu-id="faa40-546">JsonBlob mosdók küldött adatok blobot, amely a nevű tárfiók tárolja a [beállítások védett](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="faa40-546">Data sent to JsonBlob sinks is stored in blobs in the storage account named in the [Protected settings](#protected-settings).</span></span> <span data-ttu-id="faa40-547">A tárfiókban tárolt adatok bármely Azure Blob Storage API-k használatával is felhasználhatnak.</span><span class="sxs-lookup"><span data-stu-id="faa40-547">You can consume the blob data using any Azure Blob Storage APIs.</span></span>

<span data-ttu-id="faa40-548">Emellett a felhasználói felület eszközök segítségével érik el az adatokat az Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="faa40-548">In addition, you can use these UI tools to access the data in Azure Storage:</span></span>

* <span data-ttu-id="faa40-549">A Visual Studio Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="faa40-549">Visual Studio Server Explorer.</span></span>
* <span data-ttu-id="faa40-550">[A Microsoft Azure Tártallózó](https://azurestorageexplorer.codeplex.com/ "Azure Tártallózó").</span><span class="sxs-lookup"><span data-stu-id="faa40-550">[Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

<span data-ttu-id="faa40-551">A Microsoft Azure Tártallózó munkamenet pillanatképe jeleníti meg a létrehozott Azure Storage-táblákat és a tárolók egy megfelelően konfigurált LAD 3.0 bővítmény a teszteléshez használt virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="faa40-551">This snapshot of a Microsoft Azure Storage Explorer session shows the generated Azure Storage tables and containers from a correctly configured LAD 3.0 extension on a test VM.</span></span> <span data-ttu-id="faa40-552">A kép nem felel meg pontosan a [LAD 3.0 mintakonfiguráció](#an-example-lad-30-configuration).</span><span class="sxs-lookup"><span data-stu-id="faa40-552">The image doesn't match exactly with the [sample LAD 3.0 configuration](#an-example-lad-30-configuration).</span></span>

![Kép](./media/diagnostic-extension/stg_explorer.png)

<span data-ttu-id="faa40-554">Tekintse meg a megfelelő [EventHubs dokumentáció](../../event-hubs/event-hubs-what-is-event-hubs.md) megtudhatja, hogyan EventHubs végpont közzétett üzenetek felhasználását.</span><span class="sxs-lookup"><span data-stu-id="faa40-554">See the relevant [EventHubs documentation](../../event-hubs/event-hubs-what-is-event-hubs.md) to learn how to consume messages published to an EventHubs endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="faa40-555">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="faa40-555">Next steps</span></span>

* <span data-ttu-id="faa40-556">A metrika értesítések [Azure figyelő](../../monitoring-and-diagnostics/insights-alerts-portal.md) a gyűjtött metrikáihoz.</span><span class="sxs-lookup"><span data-stu-id="faa40-556">Create metric alerts in [Azure Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) for the metrics you collect.</span></span>
* <span data-ttu-id="faa40-557">Hozzon létre [diagramok figyelési](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) a metrikáihoz.</span><span class="sxs-lookup"><span data-stu-id="faa40-557">Create [monitoring charts](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) for your metrics.</span></span>
* <span data-ttu-id="faa40-558">Megtudhatja, hogyan [hozzon létre egy virtuálisgép-méretezési csoport](/azure/virtual-machines/linux/tutorial-create-vmss) a mérőszámok segítségével vezérelheti, automatikus skálázást.</span><span class="sxs-lookup"><span data-stu-id="faa40-558">Learn how to [create a virtual machine scale set](/azure/virtual-machines/linux/tutorial-create-vmss) using your metrics to control autoscaling.</span></span>
