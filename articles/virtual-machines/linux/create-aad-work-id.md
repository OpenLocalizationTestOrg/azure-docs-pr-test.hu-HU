---
title: "Az aad-ben egy munkahelyi vagy iskolai azonosító létrehozása Linux |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy munkahelyi vagy iskolai azonosító az Azure Active Directory használata a Linux virtuális gépek."
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
ms.openlocfilehash: 4fdb72e3eec41d66f8561baf1755aa425ac278af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a><span data-ttu-id="11674-103">Az Azure Active Directory használata a Linux virtuális gépek egy munkahelyi vagy iskolai azonosító létrehozása</span><span class="sxs-lookup"><span data-stu-id="11674-103">Creating a Work or School identity in Azure Active Directory to use with Linux VMs</span></span>
<span data-ttu-id="11674-104">Ha személyes Azure-fiók létrehozása vagy egy személyes MSDN-előfizetéssel rendelkezik, és előnyeit az MSDN Azure-kreditek--az Azure-fiók létrehozása egy *Microsoft-fiók* identitás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="11674-104">If you created a personal Azure account or have a personal MSDN subscription and created the Azure account to take advantage of the MSDN Azure credits -- you used a *Microsoft account* identity to create it.</span></span> <span data-ttu-id="11674-105">Számos nagy szolgáltatást Azure-- [erőforrás csoport sablonok](../../azure-resource-manager/resource-group-overview.md) – példa működéséhez (Azure Active Directory által kezelt identitás) egy munkahelyi vagy iskolai fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="11674-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) to work.</span></span> <span data-ttu-id="11674-106">Hajtsa végre az alábbi utasításokat hozható létre egy új munkahelyi vagy iskolai fiókot, mert Szerencsére az személyes Azure-fiókjával kapcsolatos legjobb dolog, hogy egy alapértelmezett Azure Active Directory-tartomány, amelyek segítségével hozzon létre egy új munkahelyi vagy iskolai fiókkal t származik a hat Azure-szolgáltatások azt igénylő használható.</span><span class="sxs-lookup"><span data-stu-id="11674-106">You can follow the instructions below to create a new work or school account because fortunately, one of the best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use to create a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="11674-107">Azonban a legutóbbi módosítások lehetővé teszik az előfizetés bármely más Azure-fiók használatával kezelheti a `azure login` interaktív bejelentkezési módszer az [Itt](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="11674-107">However, recent changes make it possible to manage your subscription with any type of Azure account using the `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="11674-108">Használja a mechanizmus, vagy amelyeket az alábbi utasításokat követve.</span><span class="sxs-lookup"><span data-stu-id="11674-108">You can either use that mechanism, or you can follow the instructions that follow.</span></span> <span data-ttu-id="11674-109">Emellett [nem hoz létre munkahelyi vagy iskolai Windows virtuális gépek használatához Azure Active Directoryban](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="11674-109">You can also [create a work or school identity in Azure Active Directory to use with Windows VMs](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

