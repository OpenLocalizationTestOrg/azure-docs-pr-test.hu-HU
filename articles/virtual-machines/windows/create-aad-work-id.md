---
title: "Nem hoz létre munkahelyi vagy iskolai aad-ben a Windows rendszerben |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy munkahelyi vagy iskolai azonosító az Azure Active Directoryban, a Windows virtuális gépekkel szeretné használni."
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
ms.openlocfilehash: 7694b959a384aaed213adc31e02debca31b7c131
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a><span data-ttu-id="4718b-103">Windows virtuális gépek használatához Azure Active Directoryban egy munkahelyi vagy iskolai azonosító létrehozása</span><span class="sxs-lookup"><span data-stu-id="4718b-103">Creating a Work or School identity in Azure Active Directory to use with Windows VMs</span></span>
<span data-ttu-id="4718b-104">Ha személyes Azure-fiók létrehozása vagy egy személyes MSDN-előfizetéssel rendelkezik, és előnyeit az MSDN Azure-kreditek--az Azure-fiók létrehozása egy *Microsoft-fiók* identitás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4718b-104">If you created a personal Azure account or have a personal MSDN subscription and created the Azure account to take advantage of the MSDN Azure credits -- you used a *Microsoft account* identity to create it.</span></span> <span data-ttu-id="4718b-105">Számos nagy szolgáltatást Azure-- [erőforrás csoport sablonok](../../azure-resource-manager/resource-group-overview.md) – példa működéséhez (Azure Active Directory által kezelt identitás) egy munkahelyi vagy iskolai fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="4718b-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) to work.</span></span> <span data-ttu-id="4718b-106">Hajtsa végre az alábbi utasításokat hozható létre egy új munkahelyi vagy iskolai fiókot, mert Szerencsére az személyes Azure-fiókjával kapcsolatos legjobb dolog, hogy egy alapértelmezett Azure Active Directory-tartomány, amelyek segítségével hozzon létre egy új munkahelyi vagy iskolai fiókkal t származik a hat Azure-szolgáltatások azt igénylő használható.</span><span class="sxs-lookup"><span data-stu-id="4718b-106">You can follow the instructions below to create a new work or school account because fortunately, one of the best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use to create a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="4718b-107">Azonban a legutóbbi módosítások lehetővé teszik az előfizetés bármely más Azure-fiók használatával kezelheti a `azure login` interaktív bejelentkezési módszer az [Itt](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="4718b-107">However, recent changes make it possible to manage your subscription with any type of Azure account using the `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="4718b-108">Használja a mechanizmus, vagy amelyeket az alábbi utasításokat követve.</span><span class="sxs-lookup"><span data-stu-id="4718b-108">You can either use that mechanism, or you can follow the instructions that follow.</span></span> <span data-ttu-id="4718b-109">Emellett [nem hoz létre munkahelyi vagy iskolai az Azure Active Directory használata a Linux virtuális gépek](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4718b-109">You can also [create a work or school identity in Azure Active Directory to use with Linux VMs](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

