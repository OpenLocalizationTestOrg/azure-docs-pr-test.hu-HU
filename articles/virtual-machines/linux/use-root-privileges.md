---
title: "a Linux virtuális gépek aaaUse root jogosultságot |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse legfelső szintű jogosultságot egy Linux virtuális gép az Azure-ban."
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
ms.openlocfilehash: 9411588c5fd0c86c4c73b3e44fbb56ab150013d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="a00db-103">Gyökér szintű jogosultság használata Azure-beli linuxos virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="a00db-103">Using root privileges on Linux virtual machines in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="a00db-104">Alapértelmezés szerint hello `root` felhasználó le van tiltva, a Linux virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a00db-104">By default, hello `root` user is disabled on Linux virtual machines in Azure.</span></span> <span data-ttu-id="a00db-105">Felhasználók futtathatják a parancsokat emelt szintű jogosultságokkal hello segítségével `sudo` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a00db-105">Users can run commands with elevated privileges by using hello `sudo` command.</span></span> <span data-ttu-id="a00db-106">Azonban hello élmény eltérhet attól függően, hogy hogyan hello rendszer lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="a00db-106">However, hello experience may vary depending on how hello system was provisioned.</span></span>

1. <span data-ttu-id="a00db-107">**SSH-kulcs és a jelszó csak vagy jelszó** -hello virtuális gép vagy a tanúsítvánnyal lett kiépítve (`.CER` fájl) vagy SSH-kulcsot, valamint egy jelszót, vagy csak a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="a00db-107">**SSH key and password OR password only** - hello virtual machine was provisioned with either a certificate (`.CER` file) or SSH key as well as a password, or just a user name and password.</span></span> <span data-ttu-id="a00db-108">Ebben az esetben `sudo` felszólítja hello felhasználói jelszó hello parancs végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="a00db-108">In this case `sudo` will prompt for hello user's password before executing hello command.</span></span>
2. <span data-ttu-id="a00db-109">**Csak az SSH-kulcs** -hello virtuális gép tanúsítvánnyal lett kiépítve (`.cer`, `.pem`, vagy `.pub` fájl) vagy az SSH-kulcsot, de nem kell jelszót.</span><span class="sxs-lookup"><span data-stu-id="a00db-109">**SSH key only** - hello virtual machine was provisioned with a certificate (`.cer`, `.pem`, or `.pub` file) or SSH key, but no password.</span></span>  <span data-ttu-id="a00db-110">Ebben az esetben `sudo` **sem fog** Rákérdezés a hello jelszó hello parancs végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="a00db-110">In this case `sudo` **will not** prompt for hello user's password before executing hello command.</span></span>

## <a name="ssh-key-and-password-or-password-only"></a><span data-ttu-id="a00db-111">SSH kulcs és a jelszó vagy a jelszó csak</span><span class="sxs-lookup"><span data-stu-id="a00db-111">SSH Key and Password, or Password Only</span></span>
<span data-ttu-id="a00db-112">Jelentkezzen be hello Linux virtuális gép SSH-kulcs vagy jelszó-hitelesítést használ, majd futtassa a parancsokat a `sudo`, például:</span><span class="sxs-lookup"><span data-stu-id="a00db-112">Log into hello Linux virtual machine using SSH key or password authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>
    [sudo] password for azureuser:

<span data-ttu-id="a00db-113">Ebben az esetben a hello felhasználó kéri a jelszót.</span><span class="sxs-lookup"><span data-stu-id="a00db-113">In this case hello user will be prompted for a password.</span></span> <span data-ttu-id="a00db-114">Hello jelszó beírása után `sudo` hello parancsot fog futni `root` jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="a00db-114">After entering hello password `sudo` will run hello command with `root` privileges.</span></span>

<span data-ttu-id="a00db-115">Engedélyezheti a passwordless sudo hello szerkesztésével `/etc/sudoers.d/waagent` fájlba, például:</span><span class="sxs-lookup"><span data-stu-id="a00db-115">You can also enable passwordless sudo by editing hello `/etc/sudoers.d/waagent` file, for example:</span></span>

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

<span data-ttu-id="a00db-116">Ez a változás hello felhasználó "azureuser" passwordless sudo tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="a00db-116">This change will allow for passwordless sudo by hello user "azureuser".</span></span>

## <a name="ssh-key-only"></a><span data-ttu-id="a00db-117">SSH kulcs csak</span><span class="sxs-lookup"><span data-stu-id="a00db-117">SSH Key Only</span></span>
<span data-ttu-id="a00db-118">Jelentkezzen be hello Linux virtuális gép SSH hitelesítés használatával, majd futtassa a parancsokat a `sudo`, például:</span><span class="sxs-lookup"><span data-stu-id="a00db-118">Log into hello Linux virtual machine using SSH key authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>

<span data-ttu-id="a00db-119">Ebben az esetben hello felhasználó fog **nem** kell adni a jelszót.</span><span class="sxs-lookup"><span data-stu-id="a00db-119">In this case hello user will **not** be prompted for a password.</span></span> <span data-ttu-id="a00db-120">Egyre erősebb után `<enter>`, `sudo` hello parancsot fog futni `root` jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="a00db-120">After pressing `<enter>`, `sudo` will run hello command with `root` privileges.</span></span>

