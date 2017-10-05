---
title: "FQDN létrehozni a Linux virtuális gépek az Azure portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy teljesen minősített tartománynevét, vagy teljes tartománynév, az erőforrás-kezelő alapú virtuális gépet az Azure portálon."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49bfec791fcca3feabc4eb280cefd7faada0ea31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a><span data-ttu-id="381d8-103">Hozzon létre egy teljesen minősített tartománynevét a Linux virtuális gépek Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="381d8-103">Create a fully qualified domain name in the Azure portal for a Linux VM</span></span>

<span data-ttu-id="381d8-104">Amikor a virtuális gép (VM) hoz létre a [Azure-portálon](https://portal.azure.com), egy nyilvános IP-erőforrás a virtuális gép automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="381d8-104">When you create a virtual machine (VM) in the [Azure portal](https://portal.azure.com), a public IP resource for the virtual machine is automatically created.</span></span> <span data-ttu-id="381d8-105">Az IP-cím segítségével érheti el távolról a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="381d8-105">You use this IP address to remotely access the VM.</span></span> <span data-ttu-id="381d8-106">Bár a portál nem hoz létre egy [teljesen minősített tartománynevét](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), vagy teljes Tartománynevét, adhat hozzá egy virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="381d8-106">Although the portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can add one once the VM is created.</span></span> <span data-ttu-id="381d8-107">Ez a cikk azt mutatja be, a DNS-nevét vagy teljes tartománynév létrehozásához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="381d8-107">This article demonstrates the steps to create a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="381d8-108">Hozzon létre egy teljesen minősített Tartományneve</span><span class="sxs-lookup"><span data-stu-id="381d8-108">Create a FQDN</span></span>
<span data-ttu-id="381d8-109">Ez a cikk feltételezi, hogy már létrehozta a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="381d8-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="381d8-110">Ha szükséges, akkor [hozzon létre egy virtuális Gépet a portálon](quick-create-portal.md) vagy [az Azure parancssori felülettel](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="381d8-110">If needed, you can [create a VM in the portal](quick-create-portal.md) or [with the Azure CLI](quick-create-cli.md).</span></span> <span data-ttu-id="381d8-111">Miután a virtuális gép megfelelően működik, és kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="381d8-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="381d8-112">Most csatlakozhat távolról a virtuális gép használata a DNS-név, többek között a `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="381d8-112">You can now connect remotely to the VM using this DNS name such as with `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="381d8-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="381d8-113">Next steps</span></span>
<span data-ttu-id="381d8-114">Most, hogy a virtuális gép nyilvános IP-cím és DNS névvel rendelkezik, akkor általános alkalmazás-keretrendszerbeli vagy szolgáltatásokat tudjanak telepíteni nginx, a MongoDB, a Docker, például stb.</span><span class="sxs-lookup"><span data-stu-id="381d8-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as nginx, MongoDB, Docker, etc.</span></span>

<span data-ttu-id="381d8-115">Akkor is olvashat további tudnivalókat [erőforrás-kezelő használatával](../../azure-resource-manager/resource-group-overview.md) kapcsolatos tippek felépítése az Azure-környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="381d8-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

