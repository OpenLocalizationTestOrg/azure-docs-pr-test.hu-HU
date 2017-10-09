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
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-linux-vms"></a><span data-ttu-id="c5b0a-103">Az Azure Active Directory toouse Linux virtuális gépek egy munkahelyi vagy iskolai azonosító létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5b0a-103">Creating a Work or School identity in Azure Active Directory toouse with Linux VMs</span></span>
<span data-ttu-id="c5b0a-104">Ha személyes Azure-fiók létrehozása vagy egy személyes MSDN-előfizetéssel rendelkezik, és létre hello Azure-fiók tootake előnyeit hello MSDN Azure kreditek--egy *Microsoft-fiók* identitás toocreate azt.</span><span class="sxs-lookup"><span data-stu-id="c5b0a-104">If you created a personal Azure account or have a personal MSDN subscription and created hello Azure account tootake advantage of hello MSDN Azure credits -- you used a *Microsoft account* identity toocreate it.</span></span> <span data-ttu-id="c5b0a-105">Számos nagy szolgáltatást Azure-- [erőforrás csoport sablonok](../../azure-resource-manager/resource-group-overview.md) példa – szükséges a munkahelyi vagy iskolai fiókjával (Azure Active Directory által kezelt identitás) toowork.</span><span class="sxs-lookup"><span data-stu-id="c5b0a-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) toowork.</span></span> <span data-ttu-id="c5b0a-106">Hajtsa végre az hello utasításokat toocreate alatt egy új munkahelyi vagy iskolai fiókhoz, mert Szerencsére hello legjobb dolog kapcsolatos személyes Azure-fiókja, hogy az ismét toocreate használt alapértelmezett Azure Active Directory tartományban egy új munkahelyi vagy iskolai fiók, amely az Azure-szolgáltatások azt igénylő használható.</span><span class="sxs-lookup"><span data-stu-id="c5b0a-106">You can follow hello instructions below toocreate a new work or school account because fortunately, one of hello best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use toocreate a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="c5b0a-107">Azonban a legutóbbi módosítások révén lehetséges toomanage bármilyen típusú hello használata Azure-fiók előfizetés `azure login` interaktív bejelentkezési módszer az [Itt](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="c5b0a-107">However, recent changes make it possible toomanage your subscription with any type of Azure account using hello `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="c5b0a-108">Mechanizmus használhatja, vagy hajtsa végre az hello utasításokban.</span><span class="sxs-lookup"><span data-stu-id="c5b0a-108">You can either use that mechanism, or you can follow hello instructions that follow.</span></span> <span data-ttu-id="c5b0a-109">Emellett [nem hoz létre munkahelyi vagy iskolai az Azure Active Directory toouse Windows virtuális gépek](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c5b0a-109">You can also [create a work or school identity in Azure Active Directory toouse with Windows VMs](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

