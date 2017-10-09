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
# <a name="install-and-configure-ansible-toomanage-virtual-machines-in-azure"></a>Telepítse és konfigurálja a Ansible toomanage virtuális gépek Azure-ban
Ez a cikk részletesen, hogyan tooinstall Ansible és néhány szükséges Azure Python SDK modulokat hello leggyakoribb Linux disztribúciókkal. Telepíthető Ansible más disztribúciókkal telepítve hello csomagok toofit továbbítania az adott platformon. toocreate Azure erőforrások biztonságos elérését, azt is megtudhatja, hogyan toocreate és Ansible toouse hitelesítő adatainak megadása. 

További telepítési beállítások és a további platformok lépéseivel kapcsolatban lásd: hello [Ansible telepítése útmutató](https://docs.ansible.com/ansible/intro_installation.html).


## <a name="install-ansible"></a>Ansible telepítése
Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAnsible* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

Most hozzon létre egy virtuális Gépet, és telepítse az alábbi disztribúciókkal hello egyik Ansible:

- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [7.3 centOS](#centos-73)
- [SLES 12.2 SP2](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS
Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. hello alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour VM használatával hello `publicIpAddress` hello feljegyzett kimenetét a hello Virtuálisgép-létrehozási művelet:

```bash
ssh azureuser@<publicIpAddress>
```

A virtuális Gépet telepítse a szükséges hello csomagok hello Azure Python SDK modulok és Ansible az alábbiak szerint:

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

Most helyezze át, amely túl[létrehozása Azure hitelesítő adatok](#create-azure-credentials).


### <a name="centos-73"></a>7.3 centOS
Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. hello alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour VM használatával hello `publicIpAddress` hello feljegyzett kimenetét a hello Virtuálisgép-létrehozási művelet:

```bash
ssh azureuser@<publicIpAddress>
```

A virtuális Gépet telepítse a szükséges hello csomagok hello Azure Python SDK modulok és Ansible az alábbiak szerint:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

Most helyezze át, amely túl[létrehozása Azure hitelesítő adatok](#create-azure-credentials).


### <a name="sles-122-sp2"></a>SLES 12.2 SP2
Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. hello alábbi példakód létrehozza a virtuális gépek nevű *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour VM használatával hello `publicIpAddress` hello feljegyzett kimenetét a hello Virtuálisgép-létrehozási művelet:

```bash
ssh azureuser@<publicIpAddress>
```

A virtuális Gépet telepítse a szükséges hello csomagok hello Azure Python SDK modulok és Ansible az alábbiak szerint:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

Most helyezze át, amely túl[létrehozása Azure hitelesítő adatok](#create-azure-credentials).


## <a name="create-azure-credentials"></a>Az Azure hitelesítő adatok létrehozása
Ansible kommunikál az Azure-ban, a felhasználónév és jelszó vagy egy egyszerű szolgáltatást. Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a Ansible használható. Szabályozza, és adja meg hello engedélyek, mivel toowhat műveletek hello szolgáltatás egyszerű hajthat végre az Azure-ban. tooimprove biztonsági keresztül csak a felhasználónév és jelszó megadása, ez a példa létrehoz egy alapszintű service egyszerű.

Az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) és kimeneti hello hitelesítő adatait, amelyet a Ansible:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

A parancsok megelőző hello hello kimeneti példát a következőképpen történik:

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

tooauthenticate tooAzure is szükség van az azonosító az Azure-előfizetéshez tooobtain [az fiók megjelenítése](/cli/azure/account#show):

```azurecli
az account show --query [id] --output tsv
```

Ez a két parancs kimenetét hello hello következő lépésben használhatja.


## <a name="create-ansible-credentials-file"></a>Ansible hitelesítőadat-fájlt létrehozni
tooprovide tooAnsible hitelesítő adatait, adja meg a környezeti változókat, vagy hozzon létre egy helyi hitelesítőadat-fájlt. További információ toodefine Ansible hitelesítő adatokat, lásd: [biztosító hitelesítő adatok tooAzure modulok](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules). 

A fejlesztési környezet létrehozása egy *hitelesítő adatok* fájl Ansible a virtuális gép a gazdagépen az alábbiak szerint:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Hello *hitelesítő adatok* maga egyesíti az előfizetés-azonosító hello hello kimenete egy egyszerű szolgáltatás létrehozása. Előző hello kimenetét [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) parancs: hello azonos rendezése igény szerint *client_id*, *titkos*, és *bérlői* . hello a következő példa *hitelesítő adatok* fájl ezeket az értékeket hello előző kimeneti megfelelő jeleníti meg. Adja meg a saját értékek a következők szerint:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a>Ansible környezeti változók használata
Ha például Ansible torony vagy Jenkins toouse eszközök, az alábbiak szerint megadhatja az környezeti változók. Ezek a változók egyesíthető egyszerű szolgáltatás létrehozása hello kimenete hello előfizetés-azonosító. Előző hello kimenetét [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) parancs: hello azonos rendezése igény szerint *AZURE_CLIENT_ID*, *AZURE_SECRET*, és *AZURE_ BÉRLŐI*. 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Következő lépések
Most már rendelkezik Ansible és hello Azure Python SDK modulok telepítve és Ansible toouse hitelesítő adatokhoz kötelező. Ismerje meg, hogyan túl[hozzon létre egy virtuális gép Ansible](ansible-create-vm.md). Itt olvashat hogyan túl[teljes Azure virtuális gép létrehozása és az azt támogató erőforrások Ansible](ansible-create-complete-vm.md).
