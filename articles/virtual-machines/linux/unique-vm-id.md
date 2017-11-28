---
title: "egy Azure Linux virtuális gép azonosítója aaaGet |} Microsoft Docs"
description: "Ismerteti, hogyan tooget és használja az Azure Linux virtuális gép egyedi azonosítóját."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 136c5d28-ff6b-4466-b27f-7a29785b5d27
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/23/2017
ms.author: kmouss
ms.openlocfilehash: 4c8ddfc2e892824581e77649285ee8adbccd5def
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a><span data-ttu-id="fab59-103">Elérésére és használatára az Azure virtuális gép egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="fab59-103">Accessing and Using Azure VM Unique ID</span></span>
<span data-ttu-id="fab59-104">Az Azure virtuális gép egyedi azonosítója egy 128 azonosító kódolású, minden Azure IaaS virtuális gép SMBIOS tárolja, és jelenleg olvasható platform BIOS-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="fab59-104">Azure VM unique ID is a 128bits identifier encoded and stored in all Azure IaaS VM’s SMBIOS and can currently be read using platform BIOS commands.</span></span>

<span data-ttu-id="fab59-105">Az Azure virtuális gép egyedi azonosítója egy csak olvasható tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="fab59-105">Azure VM unique ID is a Read-only property.</span></span> <span data-ttu-id="fab59-106">Virtuális gép Azure egyedi azonosítója nem változik, újraindítás leállásakor (tervezett vagy nem tervezett), indítása/leállítása deszerializálni lefoglalni, szolgáltatás a javítás vagy a hely visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="fab59-106">Azure Unique VM ID won’t change upon reboot shutdown (either planned for unplanned), Start/Stop de-allocate, service healing or restore in place.</span></span> <span data-ttu-id="fab59-107">Azonban ha hello VM pillanatfelvételt és másolt toocreate egy új példányát, új Azure virtuális gép azonosítója van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="fab59-107">However, if hello VM is a snapshot and copied toocreate a new instance, new Azure VM ID is configured.</span></span>

> [!NOTE]
> <span data-ttu-id="fab59-108">Ha a régebbi virtuális gépek létrehozása és mivel ezen új szolgáltatás lett megkezdődött (2014. szeptember 18) fut, indítsa újra a virtuális gép tooautomatically első Azure egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fab59-108">If you have older VMs created and running since this new feature got rolled out (September 18, 2014), please restart your VM tooautomatically get an Azure unique ID.</span></span>
> 
> 

<span data-ttu-id="fab59-109">tooaccess Azure egyedi virtuális gép azonosítója a virtuális gép hello belül:</span><span class="sxs-lookup"><span data-stu-id="fab59-109">tooaccess Azure Unique VM ID from within hello VM:</span></span>

## <a name="create-a-vm"></a><span data-ttu-id="fab59-110">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="fab59-110">Create a VM</span></span>
<span data-ttu-id="fab59-111">További információkért lásd: [virtuális gép létrehozása](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="fab59-111">For more information, see [Create a Virtual Machine](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="connect-toohello-vm"></a><span data-ttu-id="fab59-112">Csatlakozás a virtuális gép toohello</span><span class="sxs-lookup"><span data-stu-id="fab59-112">Connect toohello VM</span></span>
<span data-ttu-id="fab59-113">További információkért lásd: [SSH a Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="fab59-113">For more information, see [SSH from Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="query-vm-unique-id"></a><span data-ttu-id="fab59-114">Lekérdezés virtuális gép egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="fab59-114">Query VM Unique ID</span></span>
<span data-ttu-id="fab59-115">A parancs (a példa **Ubuntu**):</span><span class="sxs-lookup"><span data-stu-id="fab59-115">Command (example uses **Ubuntu**):</span></span>

```bash
sudo dmidecode | grep UUID
```

<span data-ttu-id="fab59-116">Példa várt eredmény:</span><span class="sxs-lookup"><span data-stu-id="fab59-116">Example Expected Results:</span></span>

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

<span data-ttu-id="fab59-117">Lejáró tooBig Endian bit rendezés hello tényleges egyedi virtuális gép azonosítója ebben az esetben lesz:</span><span class="sxs-lookup"><span data-stu-id="fab59-117">Due tooBig Endian bit ordering, hello actual Unique VM ID in this case will be:</span></span>

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

<span data-ttu-id="fab59-118">Azure virtuális gép egyedi azonosítója használható különböző helyzetekben, hogy hello VM Azure-on futó vagy a helyszíni és segít az engedélyezési, jelentés vagy általános követési követelményeit, előfordulhat, hogy az Azure infrastruktúra-szolgáltatási üzemelő példányok esetében.</span><span class="sxs-lookup"><span data-stu-id="fab59-118">Azure VM unique ID can be used in different scenarios whether hello VM is running on Azure or on-premises and can help your licensing, reporting or general tracking requirements you may have on your Azure IaaS deployments.</span></span> <span data-ttu-id="fab59-119">Alkalmazások létrehozásához, és igazoló őket az Azure számos független szoftvergyártók tooidentify egy Azure virtuális Gépen egész annak életciklusa és tootell lehet szükség, ha hello virtuális gép fut az Azure, a helyszíni vagy más szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="fab59-119">Many independent software vendors building applications and certifying them on Azure may require tooidentify an Azure VM throughout its lifecycle and tootell if hello VM is running on Azure, on-Premises or on other cloud providers.</span></span> <span data-ttu-id="fab59-120">A platform-azonosító például ha hello megfelelően szoftvert vagy súgó toocorrelate bármely virtuális gép tooits adatforrás például tooassist hello jobb platform és tootrack jobb metrikáját hello beállításával, és segítséget összefüggéseket többek között a metrikák egyéb felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="fab59-120">This platform identifier can for example help detect if hello software is properly licensed or help toocorrelate any VM data tooits source such as tooassist on setting hello right metrics for hello right platform and tootrack and correlate these metrics amongst other uses.</span></span>

