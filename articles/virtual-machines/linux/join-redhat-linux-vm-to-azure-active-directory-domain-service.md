---
title: "egy RedHat Linux virtuális gép tooan Azure Active Directory Tartományi aaaJoin |} Microsoft Docs"
description: "Hogyan toojoin egy meglévő Azure Active Directory tartományi szolgáltatások RedHat Enterprise Linux 7 virtuális gép tooan."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2016
ms.author: v-livech
ms.openlocfilehash: f3ba3c764e253191753f1cc5fc8c3b85c53818af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-redhat-linux-vm-tooan-azure-active-directory-domain-service"></a><span data-ttu-id="28b08-103">Csatlakozás egy RedHat Linux virtuális gép tooan Azure Active Directory tartományi szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="28b08-103">Join a RedHat Linux VM tooan Azure Active Directory Domain Service</span></span>

<span data-ttu-id="28b08-104">Ez a cikk bemutatja, hogyan kezeli az toojoin a Red Hat Enterprise Linux (RHEL) 7 virtuális gép tooan Azure Active Directory tartományi szolgáltatások (AADDS) a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="28b08-104">This article shows you how toojoin a Red Hat Enterprise Linux (RHEL) 7 virtual machine tooan Azure Active Directory Domain Services (AADDS) managed domain.</span></span>  <span data-ttu-id="28b08-105">hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="28b08-105">hello requirements are:</span></span>

- [<span data-ttu-id="28b08-106">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="28b08-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)

- [<span data-ttu-id="28b08-107">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="28b08-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

- [<span data-ttu-id="28b08-108">egy Azure Active Directory tartományi szolgáltatások tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="28b08-108">an Azure Active Directory Domain Services DC</span></span>](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="28b08-109">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="28b08-109">Quick Commands</span></span>

<span data-ttu-id="28b08-110">_Cserélje le olyan saját beállításaival._</span><span class="sxs-lookup"><span data-stu-id="28b08-110">_Replace any examples with your own settings._</span></span>

### <a name="switch-hello-azure-cli-tooclassic-deployment-mode"></a><span data-ttu-id="28b08-111">Kapcsoló hello azure-cli tooclassic telepítési módban</span><span class="sxs-lookup"><span data-stu-id="28b08-111">Switch hello azure-cli tooclassic deployment mode</span></span>

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a><span data-ttu-id="28b08-112">Egy RHEL verziója és a lemezkép keresése</span><span class="sxs-lookup"><span data-stu-id="28b08-112">Search for a RHEL version and image</span></span>

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a><span data-ttu-id="28b08-113">Egy Redhat Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="28b08-113">Create a Redhat Linux VM</span></span>

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p myPassword \
-e 22 \
-t "~/.ssh/id_rsa.pub" \
-z "Small" \
-l "West US"
```

### <a name="ssh-toohello-vm"></a><span data-ttu-id="28b08-114">SSH toohello méretű VM</span><span class="sxs-lookup"><span data-stu-id="28b08-114">SSH toohello VM</span></span>

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a><span data-ttu-id="28b08-115">YUM-csomagok frissítése</span><span class="sxs-lookup"><span data-stu-id="28b08-115">Update YUM packages</span></span>

```bash
sudo yum update
```

### <a name="install-packages-needed"></a><span data-ttu-id="28b08-116">Szükséges csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="28b08-116">Install packages needed</span></span>

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

<span data-ttu-id="28b08-117">Most, hogy a szükséges hello csomagok hello Linux virtuális gépek vannak telepítve, hello következő feladata toojoin hello virtuális gép toohello által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="28b08-117">Now that hello required packages are installed on hello Linux virtual machine, hello next task is toojoin hello virtual machine toohello managed domain.</span></span>

### <a name="discover-hello-aad-domain-services-managed-domain"></a><span data-ttu-id="28b08-118">Hello AAD tartományi szolgáltatásokra által kezelt tartomány felderítése</span><span class="sxs-lookup"><span data-stu-id="28b08-118">Discover hello AAD Domain Services managed domain</span></span>

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a><span data-ttu-id="28b08-119">Kerberos inicializálása</span><span class="sxs-lookup"><span data-stu-id="28b08-119">Initialize kerberos</span></span>

<span data-ttu-id="28b08-120">Adjon meg egy felhasználót, aki toohello "AAD DC rendszergazdák" csoportba tartozik.</span><span class="sxs-lookup"><span data-stu-id="28b08-120">Ensure that you specify a user who belongs toohello 'AAD DC Administrators' group.</span></span> <span data-ttu-id="28b08-121">Csak ezek a felhasználók kapcsolódhatnak számítógépek toohello által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="28b08-121">Only these users can join computers toohello managed domain.</span></span>

```bash
kinit ahmet@mydomain.com
```

### <a name="join-hello-machine-toohello-domain"></a><span data-ttu-id="28b08-122">Hello gép toohello tartományhoz</span><span class="sxs-lookup"><span data-stu-id="28b08-122">Join hello machine toohello domain</span></span>

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-hello-machine-is-joined-toohello-domain"></a><span data-ttu-id="28b08-123">Győződjön meg arról hello gép illesztett toohello tartomány</span><span class="sxs-lookup"><span data-stu-id="28b08-123">Verify hello machine is joined toohello domain</span></span>

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="28b08-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28b08-124">Next Steps</span></span>

* [<span data-ttu-id="28b08-125">Red Hat frissítési infrastruktúra (RHUI) igény Red Hat Enterprise Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="28b08-125">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="28b08-126">A virtuális gépek az Azure Resource Manager Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="28b08-126">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="28b08-127">Központi telepítése és virtuális gépek kezelése az Azure Resource Manager-sablonok és hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="28b08-127">Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI</span></span>](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
