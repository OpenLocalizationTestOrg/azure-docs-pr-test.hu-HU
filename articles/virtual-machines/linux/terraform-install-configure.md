---
title: "aaaInstall és Terraform tooprovision virtuális gépek és egyéb infrastruktúra konfigurálása az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall Terraform toocreate Azure konfigurálása és erőforrások"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: b6706f53b21347442def05a592c30a2d22718984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-terraform-tooprovision-vms-and-other-infrastructure-into-azure"></a><span data-ttu-id="2902e-103">Telepítse és konfigurálja a Terraform tooprovision virtuális gépek és egyéb infrastruktúrával az Azure</span><span class="sxs-lookup"><span data-stu-id="2902e-103">Install and configure Terraform tooprovision VMs and other infrastructure into Azure</span></span> 
<span data-ttu-id="2902e-104">Ez a cikk ismerteti a hello szükséges lépéseket tooinstall, és Terraform tooprovision erőforrások, például a virtuális gépek konfigurálása az Azure.</span><span class="sxs-lookup"><span data-stu-id="2902e-104">This article describes hello necessary steps tooinstall and configure Terraform tooprovision resources such as virtual machines into Azure.</span></span> <span data-ttu-id="2902e-105">Megtudhatja, hogyan toocreate és -felhasználási Azure hitelesítő adatok tooenable Terraform tooprovision felhőalapú erőforrások biztonságos elérését.</span><span class="sxs-lookup"><span data-stu-id="2902e-105">You will learn how toocreate and use Azure credentials tooenable Terraform tooprovision cloud resources in a secure manner.</span></span>

<span data-ttu-id="2902e-106">HashiCorp Terraform egy egyszerűen toodefine biztosít, és telepítheti a felhőalapú infrastruktúra HashiCorp konfigurációs nyelvi (Hardverkompatibilitási) nevű egyéni templating nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="2902e-106">HashiCorp Terraform provides an easy way toodefine and deploy cloud infrastructure by using a custom templating language called HashiCorp configuration language (HCL).</span></span> <span data-ttu-id="2902e-107">Az egyéni nyelv [toowrite egyszerűen és könnyen toounderstand](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="2902e-107">This custom language is [easy toowrite and easy toounderstand](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="2902e-108">Továbbá használatával hello `terraform plan` parancs hello módosítások tooyour infrastruktúra jelenítheti meg előtt véglegesítse azokat.</span><span class="sxs-lookup"><span data-stu-id="2902e-108">Additionally, by using hello `terraform plan` command, you can visualize hello changes tooyour infrastructure before you commit them.</span></span> <span data-ttu-id="2902e-109">Kövesse a lépéseket toostart Terraform használata Azure.</span><span class="sxs-lookup"><span data-stu-id="2902e-109">Follow these steps toostart using Terraform with Azure.</span></span>

## <a name="install-terraform"></a><span data-ttu-id="2902e-110">Terraform telepítése</span><span class="sxs-lookup"><span data-stu-id="2902e-110">Install Terraform</span></span>
<span data-ttu-id="2902e-111">tooinstall Terraform, [letöltése](https://www.terraform.io/downloads.html) hello csomag megfelelő, az operációs rendszer egy önálló telepítés könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="2902e-111">tooinstall Terraform, [download](https://www.terraform.io/downloads.html) hello package appropriate for your operating system into a separate install directory.</span></span> <span data-ttu-id="2902e-112">hello egy egyetlen végrehajtható fájlt, amelynek meg kell is meg a globális elérési utat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="2902e-112">hello download contains a single executable file, for which you should also define a global path.</span></span> <span data-ttu-id="2902e-113">Az utasításokat hogyan tooset hello Linux és Mac, elérési út túl lépjen[a weblap](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span><span class="sxs-lookup"><span data-stu-id="2902e-113">For instructions on how tooset hello path on Linux and Mac, go too[this webpage](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span></span> <span data-ttu-id="2902e-114">Az utasítások hogyan tooset hello elérési út a Windows, a go túl[a weblap](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span><span class="sxs-lookup"><span data-stu-id="2902e-114">For instructions on how tooset hello path on Windows, go too[this webpage](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span></span> <span data-ttu-id="2902e-115">tooverify a telepítés, hello `terraform` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2902e-115">tooverify your installation, run hello `terraform` command.</span></span> <span data-ttu-id="2902e-116">Az elérhető Terraform listáját output típusúként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="2902e-116">You should see a list of available Terraform options as output.</span></span>

<span data-ttu-id="2902e-117">A következő lépésben tooallow Terraform hozzáférés tooyour Azure-előfizetés tooperform infrastruktúra kiépítése.</span><span class="sxs-lookup"><span data-stu-id="2902e-117">Next, you need tooallow Terraform access tooyour Azure subscription tooperform infrastructure provisioning.</span></span>

## <a name="set-up-terraform-access-tooazure"></a><span data-ttu-id="2902e-118">Terraform hozzáférés tooAzure beállítása</span><span class="sxs-lookup"><span data-stu-id="2902e-118">Set up Terraform access tooAzure</span></span>
<span data-ttu-id="2902e-119">Azure Active Directory (Azure AD) toocreate két entitása szüksége tooenable Terraform tooprovision erőforrások az Azure: egy Azure AD-alkalmazást és az Azure AD szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="2902e-119">tooenable Terraform tooprovision resources into Azure, you need toocreate two entities in Azure Active Directory (Azure AD): an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="2902e-120">Ezt követően ezeket az entitásokat azonosítók használhatja a Terraform parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="2902e-120">Then, you use these entities' identifiers in your Terraform scripts.</span></span> <span data-ttu-id="2902e-121">Egy egyszerű egy helyi példányát egy globális Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2902e-121">A service principal is a local instance of a global Azure AD application.</span></span> <span data-ttu-id="2902e-122">Egy egyszerű szolgáltatás lehetővé teszi, hogy a vezérlő tooglobal erőforrások részletes helyi eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2902e-122">A service principal allows granular local access control tooglobal resources.</span></span>

<span data-ttu-id="2902e-123">Számos módon toocreate van egy Azure AD-alkalmazást és az Azure AD szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="2902e-123">There are several ways toocreate an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="2902e-124">hello legegyszerűbben és leggyorsabban ma eltérő módon toouse Azure CLI 2.0, amely [, töltse le és telepítse a Windows, Linux és Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2902e-124">hello easiest and fastest way today is toouse Azure CLI 2.0, which [you can download and install on Windows, Linux, or a Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="2902e-125">PowerShell vagy Azure CLI 1.0 toocreate hello szükséges biztonsági infrastruktúra is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2902e-125">You also can use PowerShell or Azure CLI 1.0 toocreate hello necessary security infrastructure.</span></span> <span data-ttu-id="2902e-126">hello utasításokban mutatja be az Azure-Terraform tooconfigure összes ezek a módszerek használatával.</span><span class="sxs-lookup"><span data-stu-id="2902e-126">hello instructions that follow show you how tooconfigure Terraform for Azure by using all of these approaches.</span></span>

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a><span data-ttu-id="2902e-127">Azure CLI 2.0 használja (a Windows, Linux és Mac-felhasználók számára)</span><span class="sxs-lookup"><span data-stu-id="2902e-127">Use Azure CLI 2.0 (for Windows, Linux, or Mac users)</span></span> 
<span data-ttu-id="2902e-128">Töltse le és telepítse hello után [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), jelentkezzen be tooadminister az Azure-előfizetéshez hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="2902e-128">After you download and install hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), sign in tooadminister your Azure subscription by issuing hello following command:</span></span>

```
az login
```

>[!NOTE]
><span data-ttu-id="2902e-129">Hello Kína, a Németországi Azure vagy az Azure Government felhők használja, ha szüksége van-e toofirst hello Azure CLI toowork konfigurálja, hogy a felhő.</span><span class="sxs-lookup"><span data-stu-id="2902e-129">If you use hello China, Azure Germany, or Azure Government clouds, you need toofirst configure hello Azure CLI toowork with that cloud.</span></span> <span data-ttu-id="2902e-130">Ehhez futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2902e-130">You can do this by running hello following:</span></span>

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

<span data-ttu-id="2902e-131">Ha több Azure-előfizetéssel rendelkezik, azok adatai hello által visszaadott `az login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2902e-131">If you have multiple Azure subscriptions, their details are returned by hello `az login` command.</span></span> <span data-ttu-id="2902e-132">Set hello `SUBSCRIPTION_ID` környezeti változó toohold hello értékének hello visszaadott `id` hello előfizetésből mező kívánt toouse.</span><span class="sxs-lookup"><span data-stu-id="2902e-132">Set hello `SUBSCRIPTION_ID` environment variable toohold hello value of hello returned `id` field from hello subscription you want toouse.</span></span> 

<span data-ttu-id="2902e-133">Állítsa be, amelyet az toouse ehhez a munkamenethez hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2902e-133">Set hello subscription that you want toouse for this session.</span></span>

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

<span data-ttu-id="2902e-134">Hello fiók tooget hello előfizetés-azonosító lekérdezési, és a bérlői azonosító érték.</span><span class="sxs-lookup"><span data-stu-id="2902e-134">Query hello account tooget hello subscription ID and tenant ID values.</span></span>

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

<span data-ttu-id="2902e-135">Következő lépésként hozzon létre külön hitelesítő adatokat Terraform.</span><span class="sxs-lookup"><span data-stu-id="2902e-135">Next, create separate credentials for Terraform.</span></span>

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

<span data-ttu-id="2902e-136">Visszaadja a appId, a jelszót, a sp_name és a bérlői.</span><span class="sxs-lookup"><span data-stu-id="2902e-136">Your appId, password, sp_name, and tenant are returned.</span></span> <span data-ttu-id="2902e-137">Jegyezze fel a hello appId és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="2902e-137">Make a note of hello appId and password.</span></span>

<span data-ttu-id="2902e-138">tooconfirm a hitelesítő adatait (egyszerű szolgáltatásneve), nyissa meg az új rendszerhéjat, és futtassa a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="2902e-138">tooconfirm your credentials (service principal), open a new shell and run hello following commands.</span></span> <span data-ttu-id="2902e-139">Helyettesítő hello visszaadott sp_name, jelszót és bérlői értékeit:</span><span class="sxs-lookup"><span data-stu-id="2902e-139">Substitute hello returned values for sp_name, password, and tenant:</span></span>

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a><span data-ttu-id="2902e-140">Használja a Powershellt (Windows-felhasználó)</span><span class="sxs-lookup"><span data-stu-id="2902e-140">Use PowerShell (for Windows users)</span></span> 
<span data-ttu-id="2902e-141">egy Windows toouse toowrite számítógéphez, és hajtsa végre a Terraform parancsfájlok és toouse PowerShell konfigurációs feladatok, a gép beállítása hello megfelelő PowerShell-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="2902e-141">toouse a Windows machine toowrite and execute your Terraform scripts and toouse PowerShell for configuration tasks, configure your machine with hello right PowerShell tools.</span></span> 

1. <span data-ttu-id="2902e-142">PowerShell-eszközök hello lépéseit követve telepítse [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="2902e-142">Install PowerShell tools by following hello steps in [Install and configure Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> 

2. <span data-ttu-id="2902e-143">Letöltés és végrehajtás hello [azure-setup.ps1 parancsfájl](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) hello PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="2902e-143">Download and execute hello [azure-setup.ps1 script](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) from hello PowerShell console.</span></span>

3. <span data-ttu-id="2902e-144">toorun hello azure-setup.ps1 parancsfájlban, töltse le, és hello végrehajtható `./azure-setup.ps1 setup` parancs hello PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="2902e-144">toorun hello azure-setup.ps1 script, download it and execute hello `./azure-setup.ps1 setup` command from hello PowerShell console.</span></span> <span data-ttu-id="2902e-145">Majd jelentkezzen be rendszergazdai jogosultságokkal rendelkező Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="2902e-145">Then sign in tooyour Azure subscription with administrative privileges.</span></span>

4. <span data-ttu-id="2902e-146">Adjon meg egy alkalmazásnevet (tetszőleges karakterlánc, kötelező) Ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="2902e-146">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="2902e-147">Ha szükséges adjon meg egy erős jelszót, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="2902e-147">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="2902e-148">Ha nem ad meg egy jelszót, egy erős jelszót hozza létre az Ön .NET biztonsági könyvtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="2902e-148">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a><span data-ttu-id="2902e-149">Azure CLI 1.0 használata (a Linux- vagy Mac-felhasználók számára)</span><span class="sxs-lookup"><span data-stu-id="2902e-149">Use Azure CLI 1.0 (for Linux or Mac users)</span></span>
<span data-ttu-id="2902e-150">tooget használatába Terraform Linux-gépek vagy a Mac számítógépekhez az Azure CLI 1.0-s telepítése hello megfelelő szalagtárak a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2902e-150">tooget started with Terraform on Linux machines or Macs with Azure CLI 1.0, install hello proper libraries on your machine.</span></span>  

1. <span data-ttu-id="2902e-151">Azure xPlat parancssori felület teszttelepítése hello lépéseit követve telepítse [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2902e-151">Install Azure xPlat CLI tools by following hello steps in [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> 

2. <span data-ttu-id="2902e-152">Töltse le és telepítse a JSON processzor hello utasításait követve [jq letöltése](https://stedolan.github.io/jq/download/).</span><span class="sxs-lookup"><span data-stu-id="2902e-152">Download and install a JSON processor by following hello instructions in [Download jq](https://stedolan.github.io/jq/download/).</span></span>

3. <span data-ttu-id="2902e-153">Letöltés és végrehajtás hello [azure-setup.sh parancsfájl](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash parancsfájl hello konzolról.</span><span class="sxs-lookup"><span data-stu-id="2902e-153">Download and execute hello [azure-setup.sh script](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash script from hello console.</span></span>

4. <span data-ttu-id="2902e-154">toorun hello azure-setup.sh parancsfájlban, töltse le, és hello végrehajtható `./azure-setup setup` hello konzol parancsot.</span><span class="sxs-lookup"><span data-stu-id="2902e-154">toorun hello azure-setup.sh script, download it and execute hello `./azure-setup setup` command from hello console.</span></span> <span data-ttu-id="2902e-155">Majd jelentkezzen be rendszergazdai jogosultságokkal rendelkező Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="2902e-155">Then sign in tooyour Azure subscription with administrative privileges.</span></span>
 
5. <span data-ttu-id="2902e-156">Adjon meg egy alkalmazásnevet (tetszőleges karakterlánc, kötelező) Ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="2902e-156">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="2902e-157">Ha szükséges adjon meg egy erős jelszót, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="2902e-157">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="2902e-158">Ha nem ad meg egy jelszót, egy erős jelszót hozza létre az Ön .NET biztonsági könyvtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="2902e-158">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

<span data-ttu-id="2902e-159">Minden hello korábbi parancsfájlok hozzon létre egy Azure AD-alkalmazást és a szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="2902e-159">All hello previous scripts create an Azure AD application and service principal.</span></span> <span data-ttu-id="2902e-160">egyszerű hello szolgáltatás lekérdezi a közreműködői vagy a tulajdonos szintű hozzáférés hello az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="2902e-160">hello service principal gets a contributor or owner-level access on hello subscription.</span></span> <span data-ttu-id="2902e-161">Hozzáférést biztosít magas szintű hello, mert mindig a parancsfájlok által generált hello biztonsági információk védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="2902e-161">Because of hello high level of access granted, you should always protect hello security information generated by those scripts.</span></span> <span data-ttu-id="2902e-162">Jegyezze fel az összes négy adatra biztonsági a parancsfájlok által biztosított: appId, jelszó ELŐFIZETÉS_AZONOSÍTÓJA és tenant_id.</span><span class="sxs-lookup"><span data-stu-id="2902e-162">Make a note of all four pieces of security information provided by those scripts: appId, password, subscription_id, and tenant_id.</span></span>

## <a name="set-environment-variables"></a><span data-ttu-id="2902e-163">Környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="2902e-163">Set environment variables</span></span>
<span data-ttu-id="2902e-164">Létrehozott, és konfigurálja az Azure AD szolgáltatás egyszerű, toolet kell Terraform tudja hello Bérlőazonosító, előfizetés-azonosító, ügyfél-Azonosítót és titkos toouse ügyfél.</span><span class="sxs-lookup"><span data-stu-id="2902e-164">After you create and configure an Azure AD service principal, you need toolet Terraform know hello tenant ID, subscription ID, client ID, and client secret toouse.</span></span> <span data-ttu-id="2902e-165">Ezt megteheti a Terraform parancsfájlokban ágyaz be ezeket az értékeket a [létrehozása alapvető infrastruktúra Terraform használatával](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="2902e-165">You can do it by embedding those values in your Terraform scripts, as described in [Create basic infrastructure by using Terraform](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="2902e-166">Másik lehetőségként beállíthatja a következő környezeti változók hello (és így elkerülhető a véletlenül ellenőrzés, vagy a hitelesítő adatok megosztása):</span><span class="sxs-lookup"><span data-stu-id="2902e-166">Alternately, you can set hello following environment variables (and thus avoid accidentally checking in or sharing your credentials):</span></span>

- <span data-ttu-id="2902e-167">ARM_SUBSCRIPTION_ID</span><span class="sxs-lookup"><span data-stu-id="2902e-167">ARM_SUBSCRIPTION_ID</span></span>
- <span data-ttu-id="2902e-168">ARM_CLIENT_ID</span><span class="sxs-lookup"><span data-stu-id="2902e-168">ARM_CLIENT_ID</span></span>
- <span data-ttu-id="2902e-169">ARM_CLIENT_SECRET</span><span class="sxs-lookup"><span data-stu-id="2902e-169">ARM_CLIENT_SECRET</span></span>
- <span data-ttu-id="2902e-170">ARM_TENANT_ID</span><span class="sxs-lookup"><span data-stu-id="2902e-170">ARM_TENANT_ID</span></span>

<span data-ttu-id="2902e-171">Ez a minta rendszerhéj parancsfájl tooset ezeket a változókat használhatja:</span><span class="sxs-lookup"><span data-stu-id="2902e-171">You can use this sample shell script tooset those variables:</span></span>

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

<span data-ttu-id="2902e-172">Emellett Terraform vagy kínai, vagy Azure Government az Azure vagy a Németországi Azure használja, ha szüksége tooset hello környezeti változó megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2902e-172">Additionally, if you use Terraform with Azure in China or either Azure Government or Azure Germany, you need tooset hello ENVIRONMENT variable appropriately.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2902e-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2902e-173">Next steps</span></span>
<span data-ttu-id="2902e-174">Ezzel Terraform telepített és konfigurált Azure hitelesítő adataival, hogy elindíthatja az Azure-előfizetéséhez olyan infrastruktúra üzembe.</span><span class="sxs-lookup"><span data-stu-id="2902e-174">You have now installed Terraform and configured Azure credentials so that you can start deploying infrastructure into your Azure subscription.</span></span> <span data-ttu-id="2902e-175">A következő megtudhatja, hogyan túl[hozzon létre infrastruktúra Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="2902e-175">Next, learn how too[create infrastructure with Terraform](terraform-create-complete-vm.md).</span></span>
