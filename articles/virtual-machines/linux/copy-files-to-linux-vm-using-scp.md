---
title: "aaaMove fájlok, a szolgáltatáskapcsolódási pont Azure Linux virtuális gépekről érkező tooand |} Microsoft Docs"
description: "Fájlok tooand biztonságosan áthelyezése az Azure-ban a szolgáltatáskapcsolódási pont és egy SSH-kulcspárral Linux virtuális gép."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 9e4dce667aa581df74aec3d45a6ba5e52f777d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-files-tooand-from-a-linux-vm-using-scp"></a><span data-ttu-id="48e3e-103">Linux virtuális gépet a szolgáltatáskapcsolódási pont fájlok tooand áthelyezése</span><span class="sxs-lookup"><span data-stu-id="48e3e-103">Move files tooand from a Linux VM using SCP</span></span>

<span data-ttu-id="48e3e-104">Ez a cikk bemutatja, hogyan toomove fájlok a munkaállomáson tooan Azure Linux virtuális gép mentése, vagy egy Azure Linux virtuális gép tooyour munkaállomásra, leállt biztonságos másolása (SCP).</span><span class="sxs-lookup"><span data-stu-id="48e3e-104">This article shows how toomove files from your workstation up tooan Azure Linux VM, or from an Azure Linux VM down tooyour workstation, using Secure Copy (SCP).</span></span> <span data-ttu-id="48e3e-105">Fájlok a munkaállomáson és a Linux virtuális gép közötti áthelyezése, gyorsan és biztonságosan, fontos az Azure-infrastruktúra felügyeletéhez.</span><span class="sxs-lookup"><span data-stu-id="48e3e-105">Moving files between your workstation and a Linux VM, quickly and securely, is critical for managing your Azure infrastructure.</span></span> 

<span data-ttu-id="48e3e-106">Ez a cikk a Linux virtuális gép üzembe helyezett Azure használatával kell [SSH nyilvános és titkos kulcs fájlok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48e3e-106">For this article, you need a Linux VM deployed in Azure using [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="48e3e-107">Szükség egy SCP-ügyfél a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="48e3e-107">You also need an SCP client for your local computer.</span></span> <span data-ttu-id="48e3e-108">SSH épül, és alapértelmezett rendszerhéjakba hello legtöbb Linux és Mac számítógépek és az egyes Windows ismertetése szerepel.</span><span class="sxs-lookup"><span data-stu-id="48e3e-108">It is built on top of SSH and included in hello default Bash shell of most Linux and Mac computers and some Windows shells.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="48e3e-109">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="48e3e-109">Quick commands</span></span>

<span data-ttu-id="48e3e-110">Másolja át a fájlt toohello Linux virtuális gép mentése</span><span class="sxs-lookup"><span data-stu-id="48e3e-110">Copy a file up toohello Linux VM</span></span>

```bash
scp file azureuser@azurehost:directory/targetfile
```

<span data-ttu-id="48e3e-111">Másolja át a fájlt a Linux virtuális gép hello le</span><span class="sxs-lookup"><span data-stu-id="48e3e-111">Copy a file down from hello Linux VM</span></span>

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="48e3e-112">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="48e3e-112">Detailed walkthrough</span></span>

<span data-ttu-id="48e3e-113">Példaként, azt egy Azure-alapú konfigurációs fájl mentése tooa Linux virtuális gép áthelyezése, és húzza a naplófájl könyvtárának, le is Szolgáltatáskapcsolódási pontjait és SSH kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="48e3e-113">As examples, we move an Azure configuration file up tooa Linux VM and pull down a log file directory, both using SCP and SSH keys.</span></span>   

## <a name="ssh-key-pair-authentication"></a><span data-ttu-id="48e3e-114">SSH-kulcspár hitelesítés</span><span class="sxs-lookup"><span data-stu-id="48e3e-114">SSH key pair authentication</span></span>

<span data-ttu-id="48e3e-115">Szolgáltatáskapcsolódási pont hello átviteli réteg SSH használ.</span><span class="sxs-lookup"><span data-stu-id="48e3e-115">SCP uses SSH for hello transport layer.</span></span> <span data-ttu-id="48e3e-116">SSH leírók hello hitelesítési hello cél gazdagépen, és az SSH alapértelmezés szerint egy titkosított csatornán hello fájl áthelyezi azt.</span><span class="sxs-lookup"><span data-stu-id="48e3e-116">SSH handles hello authentication on hello destination host, and it moves hello file in an encrypted tunnel provided by default with SSH.</span></span> <span data-ttu-id="48e3e-117">SSH hitelesítés a felhasználónevek és jelszavak használható.</span><span class="sxs-lookup"><span data-stu-id="48e3e-117">For SSH authentication, usernames and passwords can be used.</span></span> <span data-ttu-id="48e3e-118">Ajánlott biztonsági eljárásként azonban SSH nyilvános és titkos kulcsos hitelesítés használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="48e3e-118">However, SSH public and private key authentication are recommended as a security best practice.</span></span> <span data-ttu-id="48e3e-119">SSH hitelesítette hello kapcsolat, ha a szolgáltatáskapcsolódási pont majd megkezdi hello fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="48e3e-119">Once SSH has authenticated hello connection, SCP then begins copying hello file.</span></span> <span data-ttu-id="48e3e-120">Használja a megfelelően konfigurált `~/.ssh/config` és az SSH nyilvános és titkos kulcsokat, hello SCP-kapcsolat csak a kiszolgáló nevét (vagy IP-cím) segítségével hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="48e3e-120">Using a properly configured `~/.ssh/config` and SSH public and private keys, hello SCP connection can be established by just using a server name (or IP address).</span></span> <span data-ttu-id="48e3e-121">Ha csak egy SSH-kulcsot, SCP megkeresi azt hello `~/.ssh/` könyvtár, és használja ezt a szolgáltatást a virtuális gép toohello alapértelmezett toolog által.</span><span class="sxs-lookup"><span data-stu-id="48e3e-121">If you only have one SSH key, SCP looks for it in hello `~/.ssh/` directory, and uses it by default toolog in toohello VM.</span></span>

<span data-ttu-id="48e3e-122">Konfigurálásáról további információt a `~/.ssh/config` és az SSH nyilvános és titkos kulcsok, lásd: [hozzon létre SSH-kulcsok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48e3e-122">For more information on configuring your `~/.ssh/config` and SSH public and private keys, see [Create SSH keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="scp-a-file-tooa-linux-vm"></a><span data-ttu-id="48e3e-123">Szolgáltatáskapcsolódási pont egy fájl tooa Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="48e3e-123">SCP a file tooa Linux VM</span></span>

<span data-ttu-id="48e3e-124">Például hello első azt tooa Linux virtuális gép által használt toodeploy automation be Azure-alapú konfigurációs fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="48e3e-124">For hello first example, we copy an Azure configuration file up tooa Linux VM that is used toodeploy automation.</span></span> <span data-ttu-id="48e3e-125">Ez a fájl tartalmazza az Azure API hitelesítő adatait, amely tartalmazza a titkos kulcsok, mert a biztonsági fontos.</span><span class="sxs-lookup"><span data-stu-id="48e3e-125">Because this file contains Azure API credentials, which include secrets, security is important.</span></span> <span data-ttu-id="48e3e-126">hello titkosított alagút SSH által biztosított védelmet nyújt a hello hello fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="48e3e-126">hello encrypted tunnel provided by SSH protects hello contents of hello file.</span></span>

<span data-ttu-id="48e3e-127">parancs másolatok hello helyi következő hello *.azure/config* tooan Azure virtuális gép teljes tartománynévvel fájl *myserver.eastus.cloudapp.azure.com*. hello Azure virtuális gép hello rendszergazdai felhasználónév *azureuser* .</span><span class="sxs-lookup"><span data-stu-id="48e3e-127">hello following command copies hello local *.azure/config* file tooan Azure VM with FQDN *myserver.eastus.cloudapp.azure.com*. hello admin user name on hello Azure VM is *azureuser*.</span></span> <span data-ttu-id="48e3e-128">hello fájl megcélzott toohello */home/azureuser/* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="48e3e-128">hello file is targeted toohello */home/azureuser/* directory.</span></span> <span data-ttu-id="48e3e-129">A parancs a saját értékeit helyettesítse.</span><span class="sxs-lookup"><span data-stu-id="48e3e-129">Substitute your own values in this command.</span></span>

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a><span data-ttu-id="48e3e-130">Szolgáltatáskapcsolódási pont a könyvtárat, a Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="48e3e-130">SCP a directory from a Linux VM</span></span>

<span data-ttu-id="48e3e-131">Ebben a példában a Linux virtuális gép hello tooyour munkaállomás le egy könyvtárat a naplófájlok másolja azt.</span><span class="sxs-lookup"><span data-stu-id="48e3e-131">For this example, we copy a directory of log files from hello Linux VM down tooyour workstation.</span></span> <span data-ttu-id="48e3e-132">Egy naplófájlt is, vagy nem tartalmazhatnak bizalmas vagy titkos adatok.</span><span class="sxs-lookup"><span data-stu-id="48e3e-132">A log file may or may not contain sensitive or secret data.</span></span> <span data-ttu-id="48e3e-133">Azonban a szolgáltatáskapcsolódási pont biztosítja a Titkosított hello naplófájlok hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="48e3e-133">However, using SCP ensures hello contents of hello log files are encrypted.</span></span> <span data-ttu-id="48e3e-134">Szolgáltatáskapcsolódási pont tootransfer hello fájlok használata hello legegyszerűbb módja tooget naplókönyvtár és tooyour munkaállomás le fájlok hello miközben lehetőség van a biztonságos.</span><span class="sxs-lookup"><span data-stu-id="48e3e-134">Using SCP tootransfer hello files is hello easiest way tooget hello log directory and files down tooyour workstation while also being secure.</span></span>

<span data-ttu-id="48e3e-135">hello alábbi a parancs átmásolja a terjesztendő fájlok hello */otthoni/azureuser/logs/* könyvtárába hello Azure virtuális gép toohello helyi könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="48e3e-135">hello following command copies files in hello */home/azureuser/logs/* directory on hello Azure VM toohello local /tmp directory:</span></span>

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

<span data-ttu-id="48e3e-136">Hello `-r` cli jelző arra utasítja a szolgáltatáskapcsolódási pont toorecursively másolási hello fájlok és könyvtárak hello pontról hello könyvtár hello parancs szerepel.</span><span class="sxs-lookup"><span data-stu-id="48e3e-136">hello `-r` cli flag instructs SCP toorecursively copy hello files and directories from hello point of hello directory listed in hello command.</span></span>  <span data-ttu-id="48e3e-137">Figyelje meg, hogy a parancssori szintaxist hello a rendszer hasonló tooa `cp` -parancs másolásával.</span><span class="sxs-lookup"><span data-stu-id="48e3e-137">Also notice that hello command-line syntax is similar tooa `cp` copy command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48e3e-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48e3e-138">Next steps</span></span>

* [<span data-ttu-id="48e3e-139">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="48e3e-139">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
