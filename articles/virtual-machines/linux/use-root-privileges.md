---
title: "Használja a legfelső szintű jogosultságokat a Linux virtuális gépek |} Microsoft Docs"
description: "Megtudhatja, hogyan használja a legfelső szintű jogosultságokat egy Linux virtuális gép az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: a2c106a2-dceb-43a3-9dd1-50ed77685952
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: dc39db1f5fecffb60499a5420bfe72850e2fffd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="2e51a-103">Gyökér szintű jogosultság használata Azure-beli linuxos virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="2e51a-103">Using root privileges on Linux virtual machines in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="2e51a-104">Alapértelmezés szerint a `root` felhasználó le van tiltva, a Linux virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2e51a-104">By default, the `root` user is disabled on Linux virtual machines in Azure.</span></span> <span data-ttu-id="2e51a-105">Felhasználók futtathatók parancsokat emelt szintű jogosultságokkal a `sudo` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2e51a-105">Users can run commands with elevated privileges by using the `sudo` command.</span></span> <span data-ttu-id="2e51a-106">Azonban a felhasználói élmény eltérhet attól függően, hogy a rendszer lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="2e51a-106">However, the experience may vary depending on how the system was provisioned.</span></span>

1. <span data-ttu-id="2e51a-107">**SSH-kulcs és a jelszó csak vagy jelszó** – a virtuális gép vagy a tanúsítvánnyal lett kiépítve (`.CER` fájl) vagy SSH-kulcsot, valamint egy jelszót, vagy csak a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="2e51a-107">**SSH key and password OR password only** - the virtual machine was provisioned with either a certificate (`.CER` file) or SSH key as well as a password, or just a user name and password.</span></span> <span data-ttu-id="2e51a-108">Ebben az esetben `sudo` felszólítja a felhasználó jelszavát a parancs végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="2e51a-108">In this case `sudo` will prompt for the user's password before executing the command.</span></span>
2. <span data-ttu-id="2e51a-109">**Csak az SSH-kulcs** – a virtuális gép tanúsítvánnyal lett kiépítve (`.cer`, `.pem`, vagy `.pub` fájl) vagy az SSH-kulcsot, de nem kell jelszót.</span><span class="sxs-lookup"><span data-stu-id="2e51a-109">**SSH key only** - the virtual machine was provisioned with a certificate (`.cer`, `.pem`, or `.pub` file) or SSH key, but no password.</span></span>  <span data-ttu-id="2e51a-110">Ebben az esetben `sudo` **sem fog** Rákérdezés a felhasználó jelszavát a parancs végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="2e51a-110">In this case `sudo` **will not** prompt for the user's password before executing the command.</span></span>

## <a name="ssh-key-and-password-or-password-only"></a><span data-ttu-id="2e51a-111">SSH kulcs és a jelszó vagy a jelszó csak</span><span class="sxs-lookup"><span data-stu-id="2e51a-111">SSH Key and Password, or Password Only</span></span>
<span data-ttu-id="2e51a-112">Jelentkezzen be a Linux virtuális gép SSH-kulcs vagy jelszó-hitelesítést használ, majd futtassa a parancsokat a `sudo`, például:</span><span class="sxs-lookup"><span data-stu-id="2e51a-112">Log into the Linux virtual machine using SSH key or password authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>
    [sudo] password for azureuser:

<span data-ttu-id="2e51a-113">Ebben az esetben a felhasználó kéri a jelszót.</span><span class="sxs-lookup"><span data-stu-id="2e51a-113">In this case the user will be prompted for a password.</span></span> <span data-ttu-id="2e51a-114">A jelszó beírása után `sudo` fog futni a parancsának `root` jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="2e51a-114">After entering the password `sudo` will run the command with `root` privileges.</span></span>

<span data-ttu-id="2e51a-115">Engedélyezheti a passwordless sudo szerkesztésével a `/etc/sudoers.d/waagent` fájlba, például:</span><span class="sxs-lookup"><span data-stu-id="2e51a-115">You can also enable passwordless sudo by editing the `/etc/sudoers.d/waagent` file, for example:</span></span>

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

<span data-ttu-id="2e51a-116">Ez a változás a felhasználó "azureuser" passwordless sudo tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="2e51a-116">This change will allow for passwordless sudo by the user "azureuser".</span></span>

## <a name="ssh-key-only"></a><span data-ttu-id="2e51a-117">SSH kulcs csak</span><span class="sxs-lookup"><span data-stu-id="2e51a-117">SSH Key Only</span></span>
<span data-ttu-id="2e51a-118">Jelentkezzen be a Linux virtuális gép SSH-kulcs hitelesítést használ, majd futtassa a parancsokat a `sudo`, például:</span><span class="sxs-lookup"><span data-stu-id="2e51a-118">Log into the Linux virtual machine using SSH key authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>

<span data-ttu-id="2e51a-119">Ebben az esetben a felhasználó fog **nem** kell adni a jelszót.</span><span class="sxs-lookup"><span data-stu-id="2e51a-119">In this case the user will **not** be prompted for a password.</span></span> <span data-ttu-id="2e51a-120">Egyre erősebb után `<enter>`, `sudo` fog futni a parancsának `root` jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="2e51a-120">After pressing `<enter>`, `sudo` will run the command with `root` privileges.</span></span>

