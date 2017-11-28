---
title: "Telepítse és konfigurálja a Ansible Azure virtuális gépeken való használatra |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse és konfigurálja a Ansible Ubuntu, CentOS és SLES Azure-erőforrások kezeléséhez"
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
ms.openlocfilehash: 52b763274437961dccfc862c8a45fbd57ea9fc4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a><span data-ttu-id="137ac-103">Telepítse és konfigurálja az Azure virtuális gépek kezeléséhez Ansible</span><span class="sxs-lookup"><span data-stu-id="137ac-103">Install and configure Ansible to manage virtual machines in Azure</span></span>
<span data-ttu-id="137ac-104">Ez a cikk részletesen Ansible és a szükséges Azure Python SDK-modulok telepítése a leggyakrabban használt Linux disztribúciókkal részénél.</span><span class="sxs-lookup"><span data-stu-id="137ac-104">This article details how to install Ansible and required Azure Python SDK modules for some of the most common Linux distros.</span></span> <span data-ttu-id="137ac-105">Más disztribúciókkal Ansible telepíthető a telepített csomagok, hogy elférjen az adott platform beállításával.</span><span class="sxs-lookup"><span data-stu-id="137ac-105">You can install Ansible on other distros by adjusting the installed packages to fit your particular platform.</span></span> <span data-ttu-id="137ac-106">Szeretne létrehozni Azure-erőforrások biztonságos elérését, is megismerheti, hogyan hozhat létre és Ansible használandó hitelesítő adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="137ac-106">To create Azure resources in a secure manner, you also learn how to create and define credentials for Ansible to use.</span></span> 

<span data-ttu-id="137ac-107">További telepítési opciókon és a további platformok lépéseket, tekintse meg a [Ansible telepítése útmutató](https://docs.ansible.com/ansible/intro_installation.html).</span><span class="sxs-lookup"><span data-stu-id="137ac-107">For more installation options and steps for additional platforms, see the [Ansible install guide](https://docs.ansible.com/ansible/intro_installation.html).</span></span>


## <a name="install-ansible"></a><span data-ttu-id="137ac-108">Ansible telepítése</span><span class="sxs-lookup"><span data-stu-id="137ac-108">Install Ansible</span></span>
<span data-ttu-id="137ac-109">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="137ac-109">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="137ac-110">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAnsible* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="137ac-110">The following example creates a resource group named *myResourceGroupAnsible* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

<span data-ttu-id="137ac-111">Most hozzon létre egy virtuális Gépet, és telepítse a következő disztribúciókkal egyike Ansible:</span><span class="sxs-lookup"><span data-stu-id="137ac-111">Now create a VM and install Ansible for one of the following distros:</span></span>

- [<span data-ttu-id="137ac-112">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="137ac-112">Ubuntu 16.04 LTS</span></span>](#ubuntu1604-lts)
- [<span data-ttu-id="137ac-113">7.3 centOS</span><span class="sxs-lookup"><span data-stu-id="137ac-113">CentOS 7.3</span></span>](#centos-73)
- [<span data-ttu-id="137ac-114">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="137ac-114">SLES 12.2 SP2</span></span>](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="137ac-115">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="137ac-115">Ubuntu 16.04 LTS</span></span>
<span data-ttu-id="137ac-116">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="137ac-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="137ac-117">Az alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="137ac-117">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="137ac-118">SSH-kapcsolatot a virtuális gép használata a `publicIpAddress` feljegyzett kimenetét a virtuális Gépet a létrehozási művelet futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="137ac-118">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="137ac-119">A virtuális gépen, a szükséges csomagok telepítése az Azure Python SDK modulok és Ansible az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="137ac-119">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

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

<span data-ttu-id="137ac-120">Most helyezze át a [létrehozása Azure hitelesítő adatok](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="137ac-120">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="centos-73"></a><span data-ttu-id="137ac-121">7.3 centOS</span><span class="sxs-lookup"><span data-stu-id="137ac-121">CentOS 7.3</span></span>
<span data-ttu-id="137ac-122">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="137ac-122">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="137ac-123">Az alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="137ac-123">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="137ac-124">SSH-kapcsolatot a virtuális gép használata a `publicIpAddress` feljegyzett kimenetét a virtuális Gépet a létrehozási művelet futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="137ac-124">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="137ac-125">A virtuális gépen, a szükséges csomagok telepítése az Azure Python SDK modulok és Ansible az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="137ac-125">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

<span data-ttu-id="137ac-126">Most helyezze át a [létrehozása Azure hitelesítő adatok](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="137ac-126">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="sles-122-sp2"></a><span data-ttu-id="137ac-127">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="137ac-127">SLES 12.2 SP2</span></span>
<span data-ttu-id="137ac-128">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="137ac-128">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="137ac-129">Az alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="137ac-129">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="137ac-130">SSH-kapcsolatot a virtuális gép használata a `publicIpAddress` feljegyzett kimenetét a virtuális Gépet a létrehozási művelet futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="137ac-130">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="137ac-131">A virtuális gépen, a szükséges csomagok telepítése az Azure Python SDK modulok és Ansible az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="137ac-131">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

<span data-ttu-id="137ac-132">Most helyezze át a [létrehozása Azure hitelesítő adatok](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="137ac-132">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


## <a name="create-azure-credentials"></a><span data-ttu-id="137ac-133">Az Azure hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="137ac-133">Create Azure credentials</span></span>
<span data-ttu-id="137ac-134">Ansible kommunikál az Azure-ban, a felhasználónév és jelszó vagy egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="137ac-134">Ansible communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="137ac-135">Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a Ansible használható.</span><span class="sxs-lookup"><span data-stu-id="137ac-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Ansible.</span></span> <span data-ttu-id="137ac-136">Szabályozza, és adja meg az engedélyeket, hogy milyen műveletek a szolgáltatás egyszerű hajthat végre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="137ac-136">You control and define the permissions as to what operations the service principal can perform in Azure.</span></span> <span data-ttu-id="137ac-137">A biztonság növelése érdekében csak a felhasználónév és jelszó megadása keresztül alábbi példa létrehoz egy alapszintű service egyszerű.</span><span class="sxs-lookup"><span data-stu-id="137ac-137">To improve security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="137ac-138">Az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) és a hitelesítő adatait, amelyet a Ansible kimeneti:</span><span class="sxs-lookup"><span data-stu-id="137ac-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output the credentials that Ansible needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="137ac-139">A kimenet a fenti parancsok például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="137ac-139">An example of the output from the preceding commands is as follows:</span></span>

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

<span data-ttu-id="137ac-140">A hitelesítéshez az Azure-ba is be kell szereznie az Azure-előfizetése Azonosítóját [az fiók megjelenítése](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="137ac-140">To authenticate to Azure, you also need to obtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="137ac-141">Ez a két parancs kimenete a következő lépésben használhatja.</span><span class="sxs-lookup"><span data-stu-id="137ac-141">You use the output from these two commands in the next step.</span></span>


## <a name="create-ansible-credentials-file"></a><span data-ttu-id="137ac-142">Ansible hitelesítőadat-fájlt létrehozni</span><span class="sxs-lookup"><span data-stu-id="137ac-142">Create Ansible credentials file</span></span>
<span data-ttu-id="137ac-143">Adja meg a hitelesítő adatokat a Ansible, adja meg a környezeti változókat, vagy hozzon létre egy helyi hitelesítőadat-fájlt.</span><span class="sxs-lookup"><span data-stu-id="137ac-143">To provide credentials to Ansible, you define environment variables or create a local credentials file.</span></span> <span data-ttu-id="137ac-144">További információ a Ansible hitelesítő adatok megadását: [hitelesítő adatokat biztosító Azure-modulok](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span><span class="sxs-lookup"><span data-stu-id="137ac-144">For more information about how to define Ansible credentials, see [Providing Credentials to Azure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span></span> 

<span data-ttu-id="137ac-145">A fejlesztési környezet létrehozása egy *hitelesítő adatok* fájl Ansible a virtuális gép a gazdagépen az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="137ac-145">For a development environment, create a *credentials* file for Ansible on your host VM as follows:</span></span>

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

<span data-ttu-id="137ac-146">A *hitelesítő adatok* maga az előfizetés-azonosító egyesíti az egyszerű szolgáltatás létrehozása kimenetét.</span><span class="sxs-lookup"><span data-stu-id="137ac-146">The *credentials* file itself combines the subscription ID with the output of creating a service principal.</span></span> <span data-ttu-id="137ac-147">Az előző kimeneti [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) parancs a ugyanabban a sorrendben, igény szerint *client_id*, *titkos*, és *bérlői*.</span><span class="sxs-lookup"><span data-stu-id="137ac-147">Output from the previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is the same order as needed for *client_id*, *secret*, and *tenant*.</span></span> <span data-ttu-id="137ac-148">Az alábbi példa *hitelesítő adatok* fájl ezeket az értékeket az előző kimeneti megfelelő jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="137ac-148">The following example *credentials* file shows these values matching the previous output.</span></span> <span data-ttu-id="137ac-149">Adja meg a saját értékek a következők szerint:</span><span class="sxs-lookup"><span data-stu-id="137ac-149">Enter your own values as follows:</span></span>

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a><span data-ttu-id="137ac-150">Ansible környezeti változók használata</span><span class="sxs-lookup"><span data-stu-id="137ac-150">Use Ansible environment variables</span></span>
<span data-ttu-id="137ac-151">Ha az eszközöket, például a Ansible torony vagy Jenkins használni kívánja, megadhatja környezeti változók az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="137ac-151">If you are going to use tools such as Ansible Tower or Jenkins, you can define environment variables as follows.</span></span> <span data-ttu-id="137ac-152">Ezek a változók egyesíthető kimenetét a egyszerű szolgáltatás létrehozása az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="137ac-152">These variables combine the subscription ID with the output from creating a service principal.</span></span> <span data-ttu-id="137ac-153">Az előző kimeneti [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) parancs a ugyanabban a sorrendben, igény szerint *AZURE_CLIENT_ID*, *AZURE_SECRET*, és *AZURE_TENANT* .</span><span class="sxs-lookup"><span data-stu-id="137ac-153">Output from the previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is the same order as needed for *AZURE_CLIENT_ID*, *AZURE_SECRET*, and *AZURE_TENANT*.</span></span> 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a><span data-ttu-id="137ac-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="137ac-154">Next steps</span></span>
<span data-ttu-id="137ac-155">Most már rendelkezik Ansible és a szükséges Azure Python SDK modulok telepítve és Ansible megadott hitelesítő adatok használatához.</span><span class="sxs-lookup"><span data-stu-id="137ac-155">You now have Ansible and the required Azure Python SDK modules installed, and credentials defined for Ansible to use.</span></span> <span data-ttu-id="137ac-156">Megtudhatja, hogyan [hozzon létre egy virtuális gép Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="137ac-156">Learn how to [create a VM with Ansible](ansible-create-vm.md).</span></span> <span data-ttu-id="137ac-157">Azt is megtudhatja hogyan [teljes Azure virtuális gép létrehozása és az azt támogató erőforrások Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="137ac-157">You can also learn how to [create a complete Azure VM and supporting resources with Ansible](ansible-create-complete-vm.md).</span></span>