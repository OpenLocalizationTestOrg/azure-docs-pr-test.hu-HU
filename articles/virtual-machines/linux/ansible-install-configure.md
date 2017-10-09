---
title: "aaaInstall és Ansible konfigurálása Azure virtuális gépeken való használatra |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és Ansible konfigurálása az Azure-erőforrások Ubuntu, CentOS és SLES kezelése"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2017
ms.author: iainfou
ms.openlocfilehash: b33d1893909b6134a5474617c9af2d6e4f627c05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-ansible-toomanage-virtual-machines-in-azure"></a><span data-ttu-id="34224-103">Telepítse és konfigurálja a Ansible toomanage virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="34224-103">Install and configure Ansible toomanage virtual machines in Azure</span></span>
<span data-ttu-id="34224-104">Ez a cikk részletesen, hogyan tooinstall Ansible és néhány szükséges Azure Python SDK modulokat hello leggyakoribb Linux disztribúciókkal.</span><span class="sxs-lookup"><span data-stu-id="34224-104">This article details how tooinstall Ansible and required Azure Python SDK modules for some of hello most common Linux distros.</span></span> <span data-ttu-id="34224-105">Telepíthető Ansible más disztribúciókkal telepítve hello csomagok toofit továbbítania az adott platformon.</span><span class="sxs-lookup"><span data-stu-id="34224-105">You can install Ansible on other distros by adjusting hello installed packages toofit your particular platform.</span></span> <span data-ttu-id="34224-106">toocreate Azure erőforrások biztonságos elérését, azt is megtudhatja, hogyan toocreate és Ansible toouse hitelesítő adatainak megadása.</span><span class="sxs-lookup"><span data-stu-id="34224-106">toocreate Azure resources in a secure manner, you also learn how toocreate and define credentials for Ansible toouse.</span></span> 

<span data-ttu-id="34224-107">További telepítési beállítások és a további platformok lépéseivel kapcsolatban lásd: hello [Ansible telepítése útmutató](https://docs.ansible.com/ansible/intro_installation.html).</span><span class="sxs-lookup"><span data-stu-id="34224-107">For more installation options and steps for additional platforms, see hello [Ansible install guide](https://docs.ansible.com/ansible/intro_installation.html).</span></span>


## <a name="install-ansible"></a><span data-ttu-id="34224-108">Ansible telepítése</span><span class="sxs-lookup"><span data-stu-id="34224-108">Install Ansible</span></span>
<span data-ttu-id="34224-109">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="34224-109">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="34224-110">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAnsible* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="34224-110">hello following example creates a resource group named *myResourceGroupAnsible* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

<span data-ttu-id="34224-111">Most hozzon létre egy virtuális Gépet, és telepítse az alábbi disztribúciókkal hello egyik Ansible:</span><span class="sxs-lookup"><span data-stu-id="34224-111">Now create a VM and install Ansible for one of hello following distros:</span></span>

- [<span data-ttu-id="34224-112">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="34224-112">Ubuntu 16.04 LTS</span></span>](#ubuntu1604-lts)
- [<span data-ttu-id="34224-113">7.3 centOS</span><span class="sxs-lookup"><span data-stu-id="34224-113">CentOS 7.3</span></span>](#centos-73)
- [<span data-ttu-id="34224-114">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="34224-114">SLES 12.2 SP2</span></span>](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="34224-115">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="34224-115">Ubuntu 16.04 LTS</span></span>
<span data-ttu-id="34224-116">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="34224-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="34224-117">hello alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="34224-117">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="34224-118">SSH tooyour VM használatával hello `publicIpAddress` hello feljegyzett kimenetét a hello Virtuálisgép-létrehozási művelet:</span><span class="sxs-lookup"><span data-stu-id="34224-118">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="34224-119">A virtuális Gépet telepítse a szükséges hello csomagok hello Azure Python SDK modulok és Ansible az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="34224-119">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Azure SDKs via pip
pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via apt
sudo apt-get install -y software-properties-common
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update && sudo apt-get install -y ansible
```

<span data-ttu-id="34224-120">Most helyezze át, amely túl[létrehozása Azure hitelesítő adatok](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="34224-120">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="centos-73"></a><span data-ttu-id="34224-121">7.3 centOS</span><span class="sxs-lookup"><span data-stu-id="34224-121">CentOS 7.3</span></span>
<span data-ttu-id="34224-122">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="34224-122">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="34224-123">hello alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="34224-123">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="34224-124">SSH tooyour VM használatával hello `publicIpAddress` hello feljegyzett kimenetét a hello Virtuálisgép-létrehozási művelet:</span><span class="sxs-lookup"><span data-stu-id="34224-124">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="34224-125">A virtuális Gépet telepítse a szükséges hello csomagok hello Azure Python SDK modulok és Ansible az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="34224-125">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

<span data-ttu-id="34224-126">Most helyezze át, amely túl[létrehozása Azure hitelesítő adatok](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="34224-126">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="sles-122-sp2"></a><span data-ttu-id="34224-127">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="34224-127">SLES 12.2 SP2</span></span>
<span data-ttu-id="34224-128">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="34224-128">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="34224-129">hello alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="34224-129">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="34224-130">SSH tooyour VM használatával hello `publicIpAddress` hello feljegyzett kimenetét a hello Virtuálisgép-létrehozási művelet:</span><span class="sxs-lookup"><span data-stu-id="34224-130">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="34224-131">A virtuális Gépet telepítse a szükséges hello csomagok hello Azure Python SDK modulok és Ansible az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="34224-131">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

<span data-ttu-id="34224-132">Most helyezze át, amely túl[létrehozása Azure hitelesítő adatok](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="34224-132">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


## <a name="create-azure-credentials"></a><span data-ttu-id="34224-133">Az Azure hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="34224-133">Create Azure credentials</span></span>
<span data-ttu-id="34224-134">Ansible kommunikál az Azure-ban, a felhasználónév és jelszó vagy egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="34224-134">Ansible communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="34224-135">Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a Ansible használható.</span><span class="sxs-lookup"><span data-stu-id="34224-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Ansible.</span></span> <span data-ttu-id="34224-136">Szabályozza, és adja meg hello engedélyek, mivel toowhat műveletek hello szolgáltatás egyszerű hajthat végre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="34224-136">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span> <span data-ttu-id="34224-137">tooimprove biztonsági keresztül csak a felhasználónév és jelszó megadása, ez a példa létrehoz egy alapszintű service egyszerű.</span><span class="sxs-lookup"><span data-stu-id="34224-137">tooimprove security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="34224-138">Az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) és kimeneti hello hitelesítő adatait, amelyet a Ansible:</span><span class="sxs-lookup"><span data-stu-id="34224-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that Ansible needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="34224-139">A parancsok megelőző hello hello kimeneti példát a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="34224-139">An example of hello output from hello preceding commands is as follows:</span></span>

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

<span data-ttu-id="34224-140">tooauthenticate tooAzure is szükség van az azonosító az Azure-előfizetéshez tooobtain [az fiók megjelenítése](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="34224-140">tooauthenticate tooAzure, you also need tooobtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="34224-141">Ez a két parancs kimenetét hello hello következő lépésben használhatja.</span><span class="sxs-lookup"><span data-stu-id="34224-141">You use hello output from these two commands in hello next step.</span></span>


## <a name="create-ansible-credentials-file"></a><span data-ttu-id="34224-142">Ansible hitelesítőadat-fájlt létrehozni</span><span class="sxs-lookup"><span data-stu-id="34224-142">Create Ansible credentials file</span></span>
<span data-ttu-id="34224-143">tooprovide tooAnsible hitelesítő adatait, adja meg a környezeti változókat, vagy hozzon létre egy helyi hitelesítőadat-fájlt.</span><span class="sxs-lookup"><span data-stu-id="34224-143">tooprovide credentials tooAnsible, you define environment variables or create a local credentials file.</span></span> <span data-ttu-id="34224-144">További információ toodefine Ansible hitelesítő adatokat, lásd: [biztosító hitelesítő adatok tooAzure modulok](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span><span class="sxs-lookup"><span data-stu-id="34224-144">For more information about how toodefine Ansible credentials, see [Providing Credentials tooAzure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span></span> 

<span data-ttu-id="34224-145">A fejlesztési környezet létrehozása egy *hitelesítő adatok* fájl Ansible a virtuális gép a gazdagépen az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="34224-145">For a development environment, create a *credentials* file for Ansible on your host VM as follows:</span></span>

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

<span data-ttu-id="34224-146">Hello *hitelesítő adatok* maga egyesíti az előfizetés-azonosító hello hello kimenete egy egyszerű szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="34224-146">hello *credentials* file itself combines hello subscription ID with hello output of creating a service principal.</span></span> <span data-ttu-id="34224-147">Előző hello kimenetét [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) parancs: hello azonos rendezése igény szerint *client_id*, *titkos*, és *bérlői* .</span><span class="sxs-lookup"><span data-stu-id="34224-147">Output from hello previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is hello same order as needed for *client_id*, *secret*, and *tenant*.</span></span> <span data-ttu-id="34224-148">hello a következő példa *hitelesítő adatok* fájl ezeket az értékeket hello előző kimeneti megfelelő jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="34224-148">hello following example *credentials* file shows these values matching hello previous output.</span></span> <span data-ttu-id="34224-149">Adja meg a saját értékek a következők szerint:</span><span class="sxs-lookup"><span data-stu-id="34224-149">Enter your own values as follows:</span></span>

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a><span data-ttu-id="34224-150">Ansible környezeti változók használata</span><span class="sxs-lookup"><span data-stu-id="34224-150">Use Ansible environment variables</span></span>
<span data-ttu-id="34224-151">Ha például Ansible torony vagy Jenkins toouse eszközök, az alábbiak szerint megadhatja az környezeti változók.</span><span class="sxs-lookup"><span data-stu-id="34224-151">If you are going toouse tools such as Ansible Tower or Jenkins, you can define environment variables as follows.</span></span> <span data-ttu-id="34224-152">Ezek a változók egyesíthető egyszerű szolgáltatás létrehozása hello kimenete hello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="34224-152">These variables combine hello subscription ID with hello output from creating a service principal.</span></span> <span data-ttu-id="34224-153">Előző hello kimenetét [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) parancs: hello azonos rendezése igény szerint *AZURE_CLIENT_ID*, *AZURE_SECRET*, és *AZURE_ BÉRLŐI*.</span><span class="sxs-lookup"><span data-stu-id="34224-153">Output from hello previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is hello same order as needed for *AZURE_CLIENT_ID*, *AZURE_SECRET*, and *AZURE_TENANT*.</span></span> 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a><span data-ttu-id="34224-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34224-154">Next steps</span></span>
<span data-ttu-id="34224-155">Most már rendelkezik Ansible és hello Azure Python SDK modulok telepítve és Ansible toouse hitelesítő adatokhoz kötelező.</span><span class="sxs-lookup"><span data-stu-id="34224-155">You now have Ansible and hello required Azure Python SDK modules installed, and credentials defined for Ansible toouse.</span></span> <span data-ttu-id="34224-156">Ismerje meg, hogyan túl[hozzon létre egy virtuális gép Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="34224-156">Learn how too[create a VM with Ansible](ansible-create-vm.md).</span></span> <span data-ttu-id="34224-157">Itt olvashat hogyan túl[teljes Azure virtuális gép létrehozása és az azt támogató erőforrások Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="34224-157">You can also learn how too[create a complete Azure VM and supporting resources with Ansible](ansible-create-complete-vm.md).</span></span>
