---
title: "a munkahelyi vagy iskolai-identitás az aad-ben a Windows aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy munkahelyi vagy iskolai identitás, az Azure Active Directory toouse a Windows virtuális gépekkel."
services: virtual-machines-windows
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d07dca34-618a-48aa-9971-03d9c1210f4a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: dd6e2381fd0aa503483aa786b36232e557729c4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-windows-vms"></a>Az Azure Active Directory toouse Windows virtuális gépek egy munkahelyi vagy iskolai azonosító létrehozása
Ha személyes Azure-fiók létrehozása vagy egy személyes MSDN-előfizetéssel rendelkezik, és létre hello Azure-fiók tootake előnyeit hello MSDN Azure kreditek--egy *Microsoft-fiók* identitás toocreate azt. Számos nagy szolgáltatást Azure-- [erőforrás csoport sablonok](../../azure-resource-manager/resource-group-overview.md) példa – szükséges a munkahelyi vagy iskolai fiókjával (Azure Active Directory által kezelt identitás) toowork. Hajtsa végre az hello utasításokat toocreate alatt egy új munkahelyi vagy iskolai fiókhoz, mert Szerencsére hello legjobb dolog kapcsolatos személyes Azure-fiókja, hogy az ismét toocreate használt alapértelmezett Azure Active Directory tartományban egy új munkahelyi vagy iskolai fiók, amely az Azure-szolgáltatások azt igénylő használható.

Azonban a legutóbbi módosítások révén lehetséges toomanage bármilyen típusú hello használata Azure-fiók előfizetés `azure login` interaktív bejelentkezési módszer az [Itt](../../xplat-cli-connect.md). Mechanizmus használhatja, vagy hajtsa végre az hello utasításokban. Emellett [nem hoz létre munkahelyi vagy iskolai a Linux virtuális gépek az Azure Active Directory toouse](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

