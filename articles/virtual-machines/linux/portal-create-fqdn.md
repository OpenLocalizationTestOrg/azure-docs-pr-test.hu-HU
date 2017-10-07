---
title: "hello Azure-portálon a Linux virtuális gép teljes Tartományneve aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy teljesen minősített tartománynév vagy a teljes tartománynév, az erőforrás-kezelő alapú virtuális gép hello Azure-portálon."
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
ms.openlocfilehash: 1494a0cb1caa62069c72096a739aee111ac8b383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-linux-vm"></a><span data-ttu-id="8a2cd-103">Hozzon létre egy teljesen minősített tartománynevét hello Azure-portálon a Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="8a2cd-103">Create a fully qualified domain name in hello Azure portal for a Linux VM</span></span>

<span data-ttu-id="8a2cd-104">Amikor egy virtuális gép (VM) hoz létre a hello [Azure-portálon](https://portal.azure.com), egy nyilvános IP-erőforrás hello virtuális gép automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-104">When you create a virtual machine (VM) in hello [Azure portal](https://portal.azure.com), a public IP resource for hello virtual machine is automatically created.</span></span> <span data-ttu-id="8a2cd-105">Az IP cím tooremotely hozzáférés hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-105">You use this IP address tooremotely access hello VM.</span></span> <span data-ttu-id="8a2cd-106">Bár a hello portál nem hoz létre egy [teljesen minősített tartománynevét](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), vagy teljesen minősített Tartományneve, felveheti egy hello virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-106">Although hello portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can add one once hello VM is created.</span></span> <span data-ttu-id="8a2cd-107">Ez a cikk bemutatja a hello lépéseket toocreate egy DNS-nevét vagy teljes Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-107">This article demonstrates hello steps toocreate a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="8a2cd-108">Hozzon létre egy teljesen minősített Tartományneve</span><span class="sxs-lookup"><span data-stu-id="8a2cd-108">Create a FQDN</span></span>
<span data-ttu-id="8a2cd-109">Ez a cikk feltételezi, hogy már létrehozta a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="8a2cd-110">Ha szükséges, akkor [hozzon létre egy virtuális gép hello portálon](quick-create-portal.md) vagy [hello Azure parancssori Felülettel rendelkező](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8a2cd-110">If needed, you can [create a VM in hello portal](quick-create-portal.md) or [with hello Azure CLI](quick-create-cli.md).</span></span> <span data-ttu-id="8a2cd-111">Miután a virtuális gép megfelelően működik, és kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8a2cd-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="8a2cd-112">Csatlakoztathatja távolról virtuális gép erre a DNS-nevével, mint toohello `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-112">You can now connect remotely toohello VM using this DNS name such as with `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a2cd-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a2cd-113">Next steps</span></span>
<span data-ttu-id="8a2cd-114">Most, hogy a virtuális gép nyilvános IP-cím és DNS névvel rendelkezik, akkor általános alkalmazás-keretrendszerbeli vagy szolgáltatásokat tudjanak telepíteni nginx, a MongoDB, a Docker, például stb.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as nginx, MongoDB, Docker, etc.</span></span>

<span data-ttu-id="8a2cd-115">Akkor is olvashat további tudnivalókat [erőforrás-kezelő használatával](../../azure-resource-manager/resource-group-overview.md) kapcsolatos tippek felépítése az Azure-környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="8a2cd-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

