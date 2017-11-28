---
title: "aaaCreate és az SSH használata a Linux virtuális gépek Azure-ban pár kulcs |} Microsoft Docs"
description: "Hogyan toocreate és az SSH nyilvános és titkos kulcsból álló kulcspárt. használja az Azure tooimprove Linux virtuális gépek hello biztonsági hello hitelesítési folyamatot."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: iainfou
ms.openlocfilehash: 7fb94841d34d5bc006f3134adf91102ddce5f174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a><span data-ttu-id="e06c7-103">Hogyan toocreate és az SSH nyilvános és titkos kulcs használata párosítsa Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e06c7-103">How toocreate and use an SSH public and private key pair for Linux VMs in Azure</span></span>
<span data-ttu-id="e06c7-104">A secure shell (SSH) kulcspár Azure-ban SSH-kulcsok használata a hitelesítéshez, így nem kell hello jelszavak toolog a virtuális gépek (VM) hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="e06c7-104">With a secure shell (SSH) key pair, you can create virtual machines (VMs) in Azure that use SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span> <span data-ttu-id="e06c7-105">Ez a cikk bemutatja, hogyan a tooquickly hozhat létre, és az SSH protokoll 2-es RSA nyilvános és titkos kulcsot tartalmazó fájlt pár használhat Linux virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="e06c7-105">This article shows you how tooquickly generate and use an SSH protocol version 2 RSA public and private key file pair for Linux VMs.</span></span> <span data-ttu-id="e06c7-106">További lépések és további példákat: [részletes lépéseket toocreate SSH-kulcspár és tanúsítványok](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="e06c7-106">For more detailed steps and additional examples, see [detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

## <a name="create-an-ssh-key-pair"></a><span data-ttu-id="e06c7-107">SSH-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="e06c7-107">Create an SSH key pair</span></span>
<span data-ttu-id="e06c7-108">Használjon hello `ssh-keygen` parancs toocreate SSH nyilvános és titkos kulcs fájlok hello létrehozott alapértelmezés szerint `~/.ssh` könyvtár, de megadhat egy másik helyet és további jelszót (jelszó tooaccess hello titkos kulcsfájlt) során a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="e06c7-108">Use hello `ssh-keygen` command toocreate SSH public and private key files that are by default created in hello `~/.ssh` directory, but you can specify a different location and additional passphrase (a password tooaccess hello private key file) when prompted.</span></span> <span data-ttu-id="e06c7-109">Futtassa a parancsot a rendszerhéjakba követően, a saját hello kérdések megválaszolásával hello.</span><span class="sxs-lookup"><span data-stu-id="e06c7-109">Run hello following command from a Bash shell, answering hello prompts with your own information.</span></span>

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-hello-ssh-key-pair"></a><span data-ttu-id="e06c7-110">Hello SSH-kulcspárral használata</span><span class="sxs-lookup"><span data-stu-id="e06c7-110">Use hello SSH key pair</span></span>
<span data-ttu-id="e06c7-111">hello nyilvános kulcsot, amelyet a Linux virtuális Gépet az Azure-ban tárolt alapértelmezés szerint ki van `~/.ssh/id_rsa.pub`, kivéve, ha módosította hello hely létrehozása után azokat.</span><span class="sxs-lookup"><span data-stu-id="e06c7-111">hello public key that you place on your Linux VM in Azure is by default stored in `~/.ssh/id_rsa.pub`, unless you changed hello location when you created them.</span></span> <span data-ttu-id="e06c7-112">Hello használatakor [Azure CLI 2.0](/cli/azure) toocreate a virtuális gép hello használata esetén a nyilvános kulcs hello hely megadása [az virtuális gép létrehozása](/cli/azure/vm#create) a hello `--ssh-key-path` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e06c7-112">If you use hello [Azure CLI 2.0](/cli/azure) toocreate your VM, specify hello location of this public key when you use hello [az vm create](/cli/azure/vm#create) with hello `--ssh-key-path` option.</span></span> <span data-ttu-id="e06c7-113">Ha másolja és illessze be a nyilvános kulcsot tartalmazó fájlt toouse hello hello tartalmát hello Azure-portálon vagy a Resource Manager-sablon, ügyeljen arra, hogy nem minden további szóköz.</span><span class="sxs-lookup"><span data-stu-id="e06c7-113">If you copy and paste hello contents of hello public key file toouse in hello Azure portal or a Resource Manager template, make sure you don't copy any additional whitespace.</span></span> <span data-ttu-id="e06c7-114">Például az operációs rendszer X használatakor átadható hello nyilvános kulcsot tartalmazó fájlt (alapértelmezés szerint **~/.ssh/id_rsa.pub**) túl**pbcopy** (más Linux olyan programok érhetők el, amely ugyanaz, mint például a hellotoocopyhellotartalma`xclip`).</span><span class="sxs-lookup"><span data-stu-id="e06c7-114">For example, if you use OS X, you can pipe hello public key file (by default, **~/.ssh/id_rsa.pub**) too**pbcopy** toocopy hello contents (there are other Linux programs that do hello same thing, such as `xclip`).</span></span>

<span data-ttu-id="e06c7-115">Ha nem ismeri a nyilvános SSH-kulcsokat, megnézheti a saját nyilvános kulcsát a `cat` parancs futtatásával az alábbiak szerint. A `~/.ssh/id_rsa.pub` helyet cserélje le a saját nyilvános kulcsának helyére:</span><span class="sxs-lookup"><span data-stu-id="e06c7-115">If you're not familiar with SSH public keys, you can see your public key by running `cat` as follows, replacing `~/.ssh/id_rsa.pub` with your own public key file location:</span></span>

```bash
cat ~/.ssh/id_rsa.pub
```

<span data-ttu-id="e06c7-116">Hello nyilvános kulcsot az Azure virtuális gép, használó SSH tooyour VM használatával hello IP-cím vagy a virtuális Gépet DNS-nevét (ne feledje tooreplace `azureuser` és `myvm.westus.cloudapp.azure.com` alatt a hello rendszergazda felhasználónevét és a hello teljesen minősített tartománynév – vagy az IP-cím):</span><span class="sxs-lookup"><span data-stu-id="e06c7-116">With hello public key on your Azure VM, SSH tooyour VM using hello IP address or DNS name of your VM (remember tooreplace `azureuser` and `myvm.westus.cloudapp.azure.com` below with hello admin username and hello fully qualified domain name -- or IP address):</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="e06c7-117">Ha egy hozzáférési kódot adta meg a kulcspár létrehozásakor, írja be a hello jelszót hello bejelentkezési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="e06c7-117">If you provided a passphrase when you created your key pair, enter hello passphrase when prompted during hello login process.</span></span> <span data-ttu-id="e06c7-118">(hello kiszolgáló hozzá van adva tooyour `~/.ssh/known_hosts` mappa, és nem kell adnia tooconnect újra amíg hello nyilvános kulcsot az Azure virtuális gép változásairól vagy hello kiszolgáló nevét a rendszer eltávolítja a `~/.ssh/known_hosts`.)</span><span class="sxs-lookup"><span data-stu-id="e06c7-118">(hello server is added tooyour `~/.ssh/known_hosts` folder, and you won't be asked tooconnect again until hello public key on your Azure VM changes or hello server name is removed from `~/.ssh/known_hosts`.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e06c7-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e06c7-119">Next steps</span></span>

<span data-ttu-id="e06c7-120">SSH-kulcsok használatával létrehozott virtuális gépek alapértelmezés szerint úgy konfigurálja a jelszavak le van tiltva, jelentősen drágább, és ezért nehezen toomake találgatásos módszeren alapuló találgatás kísérletek.</span><span class="sxs-lookup"><span data-stu-id="e06c7-120">VMs created using SSH keys are by default configured with passwords disabled, toomake brute-forced guessing attempts vastly more expensive and therefore difficult.</span></span> <span data-ttu-id="e06c7-121">Ez a témakör a gyors használatra készített egyszerű SSH-kulcspár létrehozását tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="e06c7-121">This topic describes creating a simple SSH key pair for quick usage.</span></span> <span data-ttu-id="e06c7-122">Ha az SSH-kulcspárral létrehozásához további segítségre van szüksége, vagy szükség további tanúsítványokra, lásd: [részletes lépéseket toocreate SSH-kulcspár és tanúsítványok](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="e06c7-122">If you need more assistance in creating your SSH key pair or require additional certificates, see [Detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

<span data-ttu-id="e06c7-123">Virtuális gépek, amelyek az SSH-kulcspárral, hello Azure-portálon, a parancssori felület és a sablonok segítségével hozhatja létre:</span><span class="sxs-lookup"><span data-stu-id="e06c7-123">You can create VMs that use your SSH key pair using hello Azure portal, CLI, and templates:</span></span>

* [<span data-ttu-id="e06c7-124">Biztonságos Linux virtuális gép létrehozása hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e06c7-124">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e06c7-125">Biztonságos Linux virtuális gép létrehozása hello Azure CLI 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="e06c7-125">Create a secure Linux VM using hello Azure CLI 2.0)</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e06c7-126">Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="e06c7-126">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
