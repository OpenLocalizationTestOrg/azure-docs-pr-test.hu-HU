---
title: "Telepítse és konfigurálja a virtuális gépek és más Azure-infrastruktúra kiépítése Terraform |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse és konfigurálja az Azure-erőforrások létrehozásához Terraform"
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
ms.openlocfilehash: da567097be38ac649c6bf1de1508de24d21cb877
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a><span data-ttu-id="9127e-103">Telepítse és konfigurálja az Azure virtuális gépek és egyéb infrastruktúra kiépítéséhez Terraform</span><span class="sxs-lookup"><span data-stu-id="9127e-103">Install and configure Terraform to provision VMs and other infrastructure into Azure</span></span> 
<span data-ttu-id="9127e-104">Ez a cikk ismerteti a telepítése és konfigurálása a Terraform rendelkezés erőforrások, például virtuális gépek az Azure szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9127e-104">This article describes the necessary steps to install and configure Terraform to provision resources such as virtual machines into Azure.</span></span> <span data-ttu-id="9127e-105">Megtudhatja, hogyan létrehozása és engedélyezése a felhőalapú erőforrások kiépítése biztonságos módon Terraform Azure hitelesítő adatait használja.</span><span class="sxs-lookup"><span data-stu-id="9127e-105">You will learn how to create and use Azure credentials to enable Terraform to provision cloud resources in a secure manner.</span></span>

<span data-ttu-id="9127e-106">HashiCorp Terraform könnyedén definiálására és központi telepítésére a felhőalapú infrastruktúra egy HashiCorp konfigurációs nyelvi (Hardverkompatibilitási) nevű egyéni templating nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="9127e-106">HashiCorp Terraform provides an easy way to define and deploy cloud infrastructure by using a custom templating language called HashiCorp configuration language (HCL).</span></span> <span data-ttu-id="9127e-107">Az egyéni nyelv [könnyen írási és megértse](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="9127e-107">This custom language is [easy to write and easy to understand](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="9127e-108">Továbbá használatával a `terraform plan` paranccsal jelenítheti meg a módosításokat a infrastruktúra előtt véglegesítse azokat.</span><span class="sxs-lookup"><span data-stu-id="9127e-108">Additionally, by using the `terraform plan` command, you can visualize the changes to your infrastructure before you commit them.</span></span> <span data-ttu-id="9127e-109">Kövesse az alábbi lépéseket az Azure-ral Terraform használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="9127e-109">Follow these steps to start using Terraform with Azure.</span></span>

## <a name="install-terraform"></a><span data-ttu-id="9127e-110">Terraform telepítése</span><span class="sxs-lookup"><span data-stu-id="9127e-110">Install Terraform</span></span>
<span data-ttu-id="9127e-111">Terraform, telepítendő [letöltése](https://www.terraform.io/downloads.html) egy különálló, az operációs rendszerhez megfelelő csomag telepítési könyvtár.</span><span class="sxs-lookup"><span data-stu-id="9127e-111">To install Terraform, [download](https://www.terraform.io/downloads.html) the package appropriate for your operating system into a separate install directory.</span></span> <span data-ttu-id="9127e-112">A letöltés egy egyetlen végrehajtható fájlt, amelynek meg kell is meg a globális elérési utat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9127e-112">The download contains a single executable file, for which you should also define a global path.</span></span> <span data-ttu-id="9127e-113">Hogyan kell beállítani az elérési utat a Linux és Mac útmutatásra van szüksége, látogasson el [a weblap](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span><span class="sxs-lookup"><span data-stu-id="9127e-113">For instructions on how to set the path on Linux and Mac, go to [this webpage](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span></span> <span data-ttu-id="9127e-114">Hogyan lehet beállítani az elérési út a Windows útmutatásra van szüksége, látogasson el [a weblap](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span><span class="sxs-lookup"><span data-stu-id="9127e-114">For instructions on how to set the path on Windows, go to [this webpage](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span></span> <span data-ttu-id="9127e-115">Ellenőrizze a telepítést, futtassa a `terraform` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9127e-115">To verify your installation, run the `terraform` command.</span></span> <span data-ttu-id="9127e-116">Az elérhető Terraform listáját output típusúként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="9127e-116">You should see a list of available Terraform options as output.</span></span>

<span data-ttu-id="9127e-117">Ezt követően kell az Azure-előfizetéshez infrastruktúra létesítéséhez Terraform eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9127e-117">Next, you need to allow Terraform access to your Azure subscription to perform infrastructure provisioning.</span></span>

## <a name="set-up-terraform-access-to-azure"></a><span data-ttu-id="9127e-118">Az Azure-bA Terraform hozzáférés beállítása</span><span class="sxs-lookup"><span data-stu-id="9127e-118">Set up Terraform access to Azure</span></span>
<span data-ttu-id="9127e-119">Az Azure Active Directory (Azure AD) két olyan entitásra létrehozásához szükséges engedélyezéséhez Terraform rendelkezés erőforrások az Azure: egy Azure AD-alkalmazást és az Azure AD szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="9127e-119">To enable Terraform to provision resources into Azure, you need to create two entities in Azure Active Directory (Azure AD): an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="9127e-120">Ezt követően ezeket az entitásokat azonosítók használhatja a Terraform parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="9127e-120">Then, you use these entities' identifiers in your Terraform scripts.</span></span> <span data-ttu-id="9127e-121">Egy egyszerű egy helyi példányát egy globális Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9127e-121">A service principal is a local instance of a global Azure AD application.</span></span> <span data-ttu-id="9127e-122">Egy egyszerű szolgáltatás lehetővé teszi, hogy részletes helyi hozzáférés-vezérlést az globális erőforrások.</span><span class="sxs-lookup"><span data-stu-id="9127e-122">A service principal allows granular local access control to global resources.</span></span>

<span data-ttu-id="9127e-123">Több módon is hozzon létre egy Azure AD-alkalmazást és az Azure AD szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="9127e-123">There are several ways to create an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="9127e-124">A legegyszerűbb leggyorsabb módon ma pedig úgy, hogy használja az Azure CLI 2.0, amely [, töltse le és telepítse a Windows, Linux és Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9127e-124">The easiest and fastest way today is to use Azure CLI 2.0, which [you can download and install on Windows, Linux, or a Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="9127e-125">Is használhatja PowerShell vagy Azure CLI 1.0 a szükséges biztonsági infrastruktúra létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9127e-125">You also can use PowerShell or Azure CLI 1.0 to create the necessary security infrastructure.</span></span> <span data-ttu-id="9127e-126">Az alábbi utasításokat konfigurálása az Azure-Terraform összes ezek a módszerek használatával megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="9127e-126">The instructions that follow show you how to configure Terraform for Azure by using all of these approaches.</span></span>

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a><span data-ttu-id="9127e-127">Azure CLI 2.0 használja (a Windows, Linux és Mac-felhasználók számára)</span><span class="sxs-lookup"><span data-stu-id="9127e-127">Use Azure CLI 2.0 (for Windows, Linux, or Mac users)</span></span> 
<span data-ttu-id="9127e-128">Követően töltse le és telepítse a [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), jelentkezzen be az Azure-előfizetéshez felügyeletéhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="9127e-128">After you download and install the [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), sign in to administer your Azure subscription by issuing the following command:</span></span>

```
az login
```

>[!NOTE]
><span data-ttu-id="9127e-129">Ha a kínai, a Németországi Azure vagy az Azure Government felhők használja, először konfiguráljon az adott felhő használható az Azure parancssori felület szeretné.</span><span class="sxs-lookup"><span data-stu-id="9127e-129">If you use the China, Azure Germany, or Azure Government clouds, you need to first configure the Azure CLI to work with that cloud.</span></span> <span data-ttu-id="9127e-130">Ehhez futtassa a következő:</span><span class="sxs-lookup"><span data-stu-id="9127e-130">You can do this by running the following:</span></span>

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

<span data-ttu-id="9127e-131">Ha több Azure-előfizetéssel rendelkezik, azok adatai által visszaadott a `az login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9127e-131">If you have multiple Azure subscriptions, their details are returned by the `az login` command.</span></span> <span data-ttu-id="9127e-132">Állítsa be a `SUBSCRIPTION_ID` ahhoz, hogy a visszaadott érték környezeti változó `id` mező szeretné használni az előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="9127e-132">Set the `SUBSCRIPTION_ID` environment variable to hold the value of the returned `id` field from the subscription you want to use.</span></span> 

<span data-ttu-id="9127e-133">Állítsa be az ehhez a munkamenethez használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="9127e-133">Set the subscription that you want to use for this session.</span></span>

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

<span data-ttu-id="9127e-134">A lekérdezés a fiókot az előfizetés-azonosító és a bérlői azonosító értékek lekérése.</span><span class="sxs-lookup"><span data-stu-id="9127e-134">Query the account to get the subscription ID and tenant ID values.</span></span>

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

<span data-ttu-id="9127e-135">Következő lépésként hozzon létre külön hitelesítő adatokat Terraform.</span><span class="sxs-lookup"><span data-stu-id="9127e-135">Next, create separate credentials for Terraform.</span></span>

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

<span data-ttu-id="9127e-136">Visszaadja a appId, a jelszót, a sp_name és a bérlői.</span><span class="sxs-lookup"><span data-stu-id="9127e-136">Your appId, password, sp_name, and tenant are returned.</span></span> <span data-ttu-id="9127e-137">Jegyezze fel az appId és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="9127e-137">Make a note of the appId and password.</span></span>

<span data-ttu-id="9127e-138">Erősítse meg a hitelesítő adatait (egyszerű szolgáltatásnév), nyissa meg az új rendszerhéjat, és futtassa a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="9127e-138">To confirm your credentials (service principal), open a new shell and run the following commands.</span></span> <span data-ttu-id="9127e-139">Helyettesítse be sp_name, jelszót és bérlői által visszaadott érték:</span><span class="sxs-lookup"><span data-stu-id="9127e-139">Substitute the returned values for sp_name, password, and tenant:</span></span>

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a><span data-ttu-id="9127e-140">Használja a Powershellt (Windows-felhasználó)</span><span class="sxs-lookup"><span data-stu-id="9127e-140">Use PowerShell (for Windows users)</span></span> 
<span data-ttu-id="9127e-141">Használata Windows írási és végrehajtási a Terraform parancsfájlok és a konfigurációs feladatok PowerShell használatával, konfigurálja a gép a megfelelő PowerShell-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="9127e-141">To use a Windows machine to write and execute your Terraform scripts and to use PowerShell for configuration tasks, configure your machine with the right PowerShell tools.</span></span> 

1. <span data-ttu-id="9127e-142">PowerShell-eszközök lépéseit követve telepítse [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="9127e-142">Install PowerShell tools by following the steps in [Install and configure Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> 

2. <span data-ttu-id="9127e-143">Letöltés és végrehajtás a [azure-setup.ps1 parancsfájl](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) a PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="9127e-143">Download and execute the [azure-setup.ps1 script](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) from the PowerShell console.</span></span>

3. <span data-ttu-id="9127e-144">Az azure-setup.ps1 parancsfájl futtatásához, töltse le, és hajtsa végre a `./azure-setup.ps1 setup` parancsot a PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="9127e-144">To run the azure-setup.ps1 script, download it and execute the `./azure-setup.ps1 setup` command from the PowerShell console.</span></span> <span data-ttu-id="9127e-145">Majd jelentkezzen be rendszergazdai jogosultságokkal rendelkező Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="9127e-145">Then sign in to your Azure subscription with administrative privileges.</span></span>

4. <span data-ttu-id="9127e-146">Adjon meg egy alkalmazásnevet (tetszőleges karakterlánc, kötelező) Ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="9127e-146">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="9127e-147">Ha szükséges adjon meg egy erős jelszót, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="9127e-147">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="9127e-148">Ha nem ad meg egy jelszót, egy erős jelszót hozza létre az Ön .NET biztonsági könyvtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="9127e-148">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a><span data-ttu-id="9127e-149">Azure CLI 1.0 használata (a Linux- vagy Mac-felhasználók számára)</span><span class="sxs-lookup"><span data-stu-id="9127e-149">Use Azure CLI 1.0 (for Linux or Mac users)</span></span>
<span data-ttu-id="9127e-150">Megkezdéséhez Terraform Linux-gépek vagy az Azure CLI 1.0 Mac számítógépekhez, telepítse a megfelelő könyvtárak a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9127e-150">To get started with Terraform on Linux machines or Macs with Azure CLI 1.0, install the proper libraries on your machine.</span></span>  

1. <span data-ttu-id="9127e-151">Azure xPlat parancssori felület teszttelepítése lépéseit követve telepítse [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9127e-151">Install Azure xPlat CLI tools by following the steps in [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> 

2. <span data-ttu-id="9127e-152">Töltse le és telepítse a JSON processzor utasításait követve [jq letöltése](https://stedolan.github.io/jq/download/).</span><span class="sxs-lookup"><span data-stu-id="9127e-152">Download and install a JSON processor by following the instructions in [Download jq](https://stedolan.github.io/jq/download/).</span></span>

3. <span data-ttu-id="9127e-153">Letöltés és végrehajtás a [azure-setup.sh parancsfájl](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash parancsfájl a konzolról.</span><span class="sxs-lookup"><span data-stu-id="9127e-153">Download and execute the [azure-setup.sh script](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash script from the console.</span></span>

4. <span data-ttu-id="9127e-154">Az azure-setup.sh parancsfájl futtatásához, töltse le, és hajtsa végre a `./azure-setup setup` parancsot a konzolról.</span><span class="sxs-lookup"><span data-stu-id="9127e-154">To run the azure-setup.sh script, download it and execute the `./azure-setup setup` command from the console.</span></span> <span data-ttu-id="9127e-155">Majd jelentkezzen be rendszergazdai jogosultságokkal rendelkező Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="9127e-155">Then sign in to your Azure subscription with administrative privileges.</span></span>
 
5. <span data-ttu-id="9127e-156">Adjon meg egy alkalmazásnevet (tetszőleges karakterlánc, kötelező) Ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="9127e-156">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="9127e-157">Ha szükséges adjon meg egy erős jelszót, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="9127e-157">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="9127e-158">Ha nem ad meg egy jelszót, egy erős jelszót hozza létre az Ön .NET biztonsági könyvtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="9127e-158">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

<span data-ttu-id="9127e-159">A korábbi parancsfájlok hozzon létre egy Azure AD-alkalmazást és a szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="9127e-159">All the previous scripts create an Azure AD application and service principal.</span></span> <span data-ttu-id="9127e-160">Az egyszerű szolgáltatás lekérdezi a közreműködői vagy a tulajdonos szintű hozzáférés az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="9127e-160">The service principal gets a contributor or owner-level access on the subscription.</span></span> <span data-ttu-id="9127e-161">Hozzáférést biztosít a magas szintű miatt mindig a parancsfájlok által létrehozott biztonsági információ védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="9127e-161">Because of the high level of access granted, you should always protect the security information generated by those scripts.</span></span> <span data-ttu-id="9127e-162">Jegyezze fel az összes négy adatra biztonsági a parancsfájlok által biztosított: appId, jelszó ELŐFIZETÉS_AZONOSÍTÓJA és tenant_id.</span><span class="sxs-lookup"><span data-stu-id="9127e-162">Make a note of all four pieces of security information provided by those scripts: appId, password, subscription_id, and tenant_id.</span></span>

## <a name="set-environment-variables"></a><span data-ttu-id="9127e-163">Környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="9127e-163">Set environment variables</span></span>
<span data-ttu-id="9127e-164">Létrehozott, és konfigurálja az Azure AD szolgáltatás egyszerű, kell ahhoz, hogy ismeri a bérlő azonosítója, előfizetés-azonosító, ügyfél-Azonosítót és titkos ügyfélkulcsot használandó Terraform.</span><span class="sxs-lookup"><span data-stu-id="9127e-164">After you create and configure an Azure AD service principal, you need to let Terraform know the tenant ID, subscription ID, client ID, and client secret to use.</span></span> <span data-ttu-id="9127e-165">Ezt megteheti a Terraform parancsfájlokban ágyaz be ezeket az értékeket a [létrehozása alapvető infrastruktúra Terraform használatával](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="9127e-165">You can do it by embedding those values in your Terraform scripts, as described in [Create basic infrastructure by using Terraform](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="9127e-166">Másik lehetőségként beállíthatja a következő környezeti változók (és így elkerülhető a véletlenül ellenőrzés, vagy a hitelesítő adatok megosztása):</span><span class="sxs-lookup"><span data-stu-id="9127e-166">Alternately, you can set the following environment variables (and thus avoid accidentally checking in or sharing your credentials):</span></span>

- <span data-ttu-id="9127e-167">ARM_SUBSCRIPTION_ID</span><span class="sxs-lookup"><span data-stu-id="9127e-167">ARM_SUBSCRIPTION_ID</span></span>
- <span data-ttu-id="9127e-168">ARM_CLIENT_ID</span><span class="sxs-lookup"><span data-stu-id="9127e-168">ARM_CLIENT_ID</span></span>
- <span data-ttu-id="9127e-169">ARM_CLIENT_SECRET</span><span class="sxs-lookup"><span data-stu-id="9127e-169">ARM_CLIENT_SECRET</span></span>
- <span data-ttu-id="9127e-170">ARM_TENANT_ID</span><span class="sxs-lookup"><span data-stu-id="9127e-170">ARM_TENANT_ID</span></span>

<span data-ttu-id="9127e-171">Ez a parancsfájlpélda rendszerhéj segítségével állítsa be ezeket a változókat:</span><span class="sxs-lookup"><span data-stu-id="9127e-171">You can use this sample shell script to set those variables:</span></span>

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

<span data-ttu-id="9127e-172">Emellett ha Terraform vagy kínai, vagy Azure Government az Azure vagy a Németországi Azure használ, be kell a környezeti változó megfelelően.</span><span class="sxs-lookup"><span data-stu-id="9127e-172">Additionally, if you use Terraform with Azure in China or either Azure Government or Azure Germany, you need to set the ENVIRONMENT variable appropriately.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9127e-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9127e-173">Next steps</span></span>
<span data-ttu-id="9127e-174">Ezzel Terraform telepített és konfigurált Azure hitelesítő adataival, hogy elindíthatja az Azure-előfizetéséhez olyan infrastruktúra üzembe.</span><span class="sxs-lookup"><span data-stu-id="9127e-174">You have now installed Terraform and configured Azure credentials so that you can start deploying infrastructure into your Azure subscription.</span></span> <span data-ttu-id="9127e-175">A következő megtudhatja, hogyan [hozzon létre infrastruktúra Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="9127e-175">Next, learn how to [create infrastructure with Terraform](terraform-create-complete-vm.md).</span></span>
