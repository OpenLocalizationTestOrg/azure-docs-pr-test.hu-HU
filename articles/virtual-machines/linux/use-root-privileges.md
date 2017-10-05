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
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Gyökér szintű jogosultság használata Azure-beli linuxos virtuális gépeken
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Alapértelmezés szerint a `root` felhasználó le van tiltva, a Linux virtuális gépek Azure-ban. Felhasználók futtathatók parancsokat emelt szintű jogosultságokkal a `sudo` parancsot. Azonban a felhasználói élmény eltérhet attól függően, hogy a rendszer lett kiépítve.

1. **SSH-kulcs és a jelszó csak vagy jelszó** – a virtuális gép vagy a tanúsítvánnyal lett kiépítve (`.CER` fájl) vagy SSH-kulcsot, valamint egy jelszót, vagy csak a felhasználónevet és jelszót. Ebben az esetben `sudo` felszólítja a felhasználó jelszavát a parancs végrehajtása előtt.
2. **Csak az SSH-kulcs** – a virtuális gép tanúsítvánnyal lett kiépítve (`.cer`, `.pem`, vagy `.pub` fájl) vagy az SSH-kulcsot, de nem kell jelszót.  Ebben az esetben `sudo` **sem fog** Rákérdezés a felhasználó jelszavát a parancs végrehajtása előtt.

## <a name="ssh-key-and-password-or-password-only"></a>SSH kulcs és a jelszó vagy a jelszó csak
Jelentkezzen be a Linux virtuális gép SSH-kulcs vagy jelszó-hitelesítést használ, majd futtassa a parancsokat a `sudo`, például:

    # sudo <command>
    [sudo] password for azureuser:

Ebben az esetben a felhasználó kéri a jelszót. A jelszó beírása után `sudo` fog futni a parancsának `root` jogosultságokkal.

Engedélyezheti a passwordless sudo szerkesztésével a `/etc/sudoers.d/waagent` fájlba, például:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Ez a változás a felhasználó "azureuser" passwordless sudo tesz lehetővé.

## <a name="ssh-key-only"></a>SSH kulcs csak
Jelentkezzen be a Linux virtuális gép SSH-kulcs hitelesítést használ, majd futtassa a parancsokat a `sudo`, például:

    # sudo <command>

Ebben az esetben a felhasználó fog **nem** kell adni a jelszót. Egyre erősebb után `<enter>`, `sudo` fog futni a parancsának `root` jogosultságokkal.

