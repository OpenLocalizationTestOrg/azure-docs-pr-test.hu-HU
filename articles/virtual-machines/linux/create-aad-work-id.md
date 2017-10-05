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
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Az Azure Active Directory használata a Linux virtuális gépek egy munkahelyi vagy iskolai azonosító létrehozása
Ha személyes Azure-fiók létrehozása vagy egy személyes MSDN-előfizetéssel rendelkezik, és előnyeit az MSDN Azure-kreditek--az Azure-fiók létrehozása egy *Microsoft-fiók* identitás létrehozásához. Számos nagy szolgáltatást Azure-- [erőforrás csoport sablonok](../../azure-resource-manager/resource-group-overview.md) – példa működéséhez (Azure Active Directory által kezelt identitás) egy munkahelyi vagy iskolai fiók szükséges. Hajtsa végre az alábbi utasításokat hozható létre egy új munkahelyi vagy iskolai fiókot, mert Szerencsére az személyes Azure-fiókjával kapcsolatos legjobb dolog, hogy egy alapértelmezett Azure Active Directory-tartomány, amelyek segítségével hozzon létre egy új munkahelyi vagy iskolai fiókkal t származik a hat Azure-szolgáltatások azt igénylő használható.

Azonban a legutóbbi módosítások lehetővé teszik az előfizetés bármely más Azure-fiók használatával kezelheti a `azure login` interaktív bejelentkezési módszer az [Itt](../../xplat-cli-connect.md). Használja a mechanizmus, vagy amelyeket az alábbi utasításokat követve. Emellett [nem hoz létre munkahelyi vagy iskolai Windows virtuális gépek használatához Azure Active Directoryban](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

