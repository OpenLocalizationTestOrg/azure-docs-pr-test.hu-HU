---
title: "az aad-ben Linux egy munkahelyi vagy iskolai identitás aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy munkahelyi vagy iskolai identitás, az Azure Active Directory toouse a Linux virtuális gépekkel."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: b0f86d77-c669-4aa1-a095-c2aa4d9105fe
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: 54c3d0b0e89fe1b2d6a9b58a46776fe446ed72b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-linux-vms"></a>Az Azure Active Directory toouse Linux virtuális gépek egy munkahelyi vagy iskolai azonosító létrehozása
Ha személyes Azure-fiók létrehozása vagy egy személyes MSDN-előfizetéssel rendelkezik, és létre hello Azure-fiók tootake előnyeit hello MSDN Azure kreditek--egy *Microsoft-fiók* identitás toocreate azt. Számos nagy szolgáltatást Azure-- [erőforrás csoport sablonok](../../azure-resource-manager/resource-group-overview.md) példa – szükséges a munkahelyi vagy iskolai fiókjával (Azure Active Directory által kezelt identitás) toowork. Hajtsa végre az hello utasításokat toocreate alatt egy új munkahelyi vagy iskolai fiókhoz, mert Szerencsére hello legjobb dolog kapcsolatos személyes Azure-fiókja, hogy az ismét toocreate használt alapértelmezett Azure Active Directory tartományban egy új munkahelyi vagy iskolai fiók, amely az Azure-szolgáltatások azt igénylő használható.

Azonban a legutóbbi módosítások révén lehetséges toomanage bármilyen típusú hello használata Azure-fiók előfizetés `azure login` interaktív bejelentkezési módszer az [Itt](../../xplat-cli-connect.md). Mechanizmus használhatja, vagy hajtsa végre az hello utasításokban. Emellett [nem hoz létre munkahelyi vagy iskolai az Azure Active Directory toouse Windows virtuális gépek](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

