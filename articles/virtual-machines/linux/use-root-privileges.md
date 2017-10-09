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
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Gyökér szintű jogosultság használata Azure-beli linuxos virtuális gépeken
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Alapértelmezés szerint hello `root` felhasználó le van tiltva, a Linux virtuális gépek Azure-ban. Felhasználók futtathatják a parancsokat emelt szintű jogosultságokkal hello segítségével `sudo` parancsot. Azonban hello élmény eltérhet attól függően, hogy hogyan hello rendszer lett kiépítve.

1. **SSH-kulcs és a jelszó csak vagy jelszó** -hello virtuális gép vagy a tanúsítvánnyal lett kiépítve (`.CER` fájl) vagy SSH-kulcsot, valamint egy jelszót, vagy csak a felhasználónevet és jelszót. Ebben az esetben `sudo` felszólítja hello felhasználói jelszó hello parancs végrehajtása előtt.
2. **Csak az SSH-kulcs** -hello virtuális gép tanúsítvánnyal lett kiépítve (`.cer`, `.pem`, vagy `.pub` fájl) vagy az SSH-kulcsot, de nem kell jelszót.  Ebben az esetben `sudo` **sem fog** Rákérdezés a hello jelszó hello parancs végrehajtása előtt.

## <a name="ssh-key-and-password-or-password-only"></a>SSH kulcs és a jelszó vagy a jelszó csak
Jelentkezzen be hello Linux virtuális gép SSH-kulcs vagy jelszó-hitelesítést használ, majd futtassa a parancsokat a `sudo`, például:

    # sudo <command>
    [sudo] password for azureuser:

Ebben az esetben a hello felhasználó kéri a jelszót. Hello jelszó beírása után `sudo` hello parancsot fog futni `root` jogosultságokkal.

Engedélyezheti a passwordless sudo hello szerkesztésével `/etc/sudoers.d/waagent` fájlba, például:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Ez a változás hello felhasználó "azureuser" passwordless sudo tesz lehetővé.

## <a name="ssh-key-only"></a>SSH kulcs csak
Jelentkezzen be hello Linux virtuális gép SSH hitelesítés használatával, majd futtassa a parancsokat a `sudo`, például:

    # sudo <command>

Ebben az esetben hello felhasználó fog **nem** kell adni a jelszót. Egyre erősebb után `<enter>`, `sudo` hello parancsot fog futni `root` jogosultságokkal.

