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
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-windows-vms"></a><span data-ttu-id="b066b-103">Az Azure Active Directory toouse Windows virtuális gépek egy munkahelyi vagy iskolai azonosító létrehozása</span><span class="sxs-lookup"><span data-stu-id="b066b-103">Creating a Work or School identity in Azure Active Directory toouse with Windows VMs</span></span>
<span data-ttu-id="b066b-104">Ha személyes Azure-fiók létrehozása vagy egy személyes MSDN-előfizetéssel rendelkezik, és létre hello Azure-fiók tootake előnyeit hello MSDN Azure kreditek--egy *Microsoft-fiók* identitás toocreate azt.</span><span class="sxs-lookup"><span data-stu-id="b066b-104">If you created a personal Azure account or have a personal MSDN subscription and created hello Azure account tootake advantage of hello MSDN Azure credits -- you used a *Microsoft account* identity toocreate it.</span></span> <span data-ttu-id="b066b-105">Számos nagy szolgáltatást Azure-- [erőforrás csoport sablonok](../../azure-resource-manager/resource-group-overview.md) példa – szükséges a munkahelyi vagy iskolai fiókjával (Azure Active Directory által kezelt identitás) toowork.</span><span class="sxs-lookup"><span data-stu-id="b066b-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) toowork.</span></span> <span data-ttu-id="b066b-106">Hajtsa végre az hello utasításokat toocreate alatt egy új munkahelyi vagy iskolai fiókhoz, mert Szerencsére hello legjobb dolog kapcsolatos személyes Azure-fiókja, hogy az ismét toocreate használt alapértelmezett Azure Active Directory tartományban egy új munkahelyi vagy iskolai fiók, amely az Azure-szolgáltatások azt igénylő használható.</span><span class="sxs-lookup"><span data-stu-id="b066b-106">You can follow hello instructions below toocreate a new work or school account because fortunately, one of hello best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use toocreate a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="b066b-107">Azonban a legutóbbi módosítások révén lehetséges toomanage bármilyen típusú hello használata Azure-fiók előfizetés `azure login` interaktív bejelentkezési módszer az [Itt](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="b066b-107">However, recent changes make it possible toomanage your subscription with any type of Azure account using hello `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="b066b-108">Mechanizmus használhatja, vagy hajtsa végre az hello utasításokban.</span><span class="sxs-lookup"><span data-stu-id="b066b-108">You can either use that mechanism, or you can follow hello instructions that follow.</span></span> <span data-ttu-id="b066b-109">Emellett [nem hoz létre munkahelyi vagy iskolai a Linux virtuális gépek az Azure Active Directory toouse](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b066b-109">You can also [create a work or school identity in Azure Active Directory toouse with Linux VMs](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

