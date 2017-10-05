---
title: "A VPN-átjáró kapcsolat ellenőrzése |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan ellenőrizheti egy virtuális hálózat VPN Gateway-kapcsolatot."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: b2d702ecdd5e1fca342e7c84c6e75339097f0bcd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="verify-a-vpn-gateway-connection"></a><span data-ttu-id="a16ae-103">A VPN-átjáró kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a16ae-103">Verify a VPN Gateway connection</span></span>

<span data-ttu-id="a16ae-104">Ez a cikk bemutatja, hogyan ellenőrizheti a klasszikus és Resource Manager üzembe helyezési modellel VPN gateway-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="a16ae-104">This article shows you how to verify a VPN gateway connection for both the classic and Resource Manager deployment models.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="a16ae-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a16ae-105">Azure portal</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a><span data-ttu-id="a16ae-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a16ae-106">PowerShell</span></span>

<span data-ttu-id="a16ae-107">Ha ellenőrizni szeretné a Resource Manager üzembe helyezési modellel PowerShell segítségével a VPN gateway-kapcsolattal, telepítse a legújabb verzióját a [Azure Resource Manager PowerShell-parancsmagok](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a16ae-107">To verify a VPN gateway connection for the Resource Manager deployment model using PowerShell, install the latest version of the [Azure Resource Manager PowerShell cmdlets](/powershell/azure/overview).</span></span>

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a><span data-ttu-id="a16ae-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a16ae-108">Azure CLI</span></span>

<span data-ttu-id="a16ae-109">Ellenőrizze a Resource Manager üzembe helyezési modellel, az Azure parancssori felület használatával a VPN gateway-kapcsolattal, telepítse a legújabb verzióját a [parancssori felület parancsait](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="a16ae-109">To verify a VPN gateway connection for the Resource Manager deployment model using Azure CLI, install the latest version of the [CLI commands](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 or later).</span></span>

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a><span data-ttu-id="a16ae-110">(Klasszikus) Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a16ae-110">Azure portal (classic)</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a><span data-ttu-id="a16ae-111">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="a16ae-111">PowerShell (classic)</span></span>

<span data-ttu-id="a16ae-112">Ellenőrizze a klasszikus üzembe helyezési modell PowerShell segítségével a VPN gateway-kapcsolatot, telepítse az Azure PowerShell-parancsmagok legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="a16ae-112">To verify your VPN gateway connection for the classic deployment model using PowerShell, install the latest versions of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="a16ae-113">Ügyeljen arra, hogy töltse le és telepítse a [szolgáltatásfelügyelet](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modul.</span><span class="sxs-lookup"><span data-stu-id="a16ae-113">Be sure to download and install the [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) module.</span></span> <span data-ttu-id="a16ae-114">Jelentkezzen be a klasszikus üzembe helyezési modellel a "Hozzáadás-AzureAccount" segítségével.</span><span class="sxs-lookup"><span data-stu-id="a16ae-114">Use 'Add-AzureAccount' to log in to the classic deployment model.</span></span>

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a><span data-ttu-id="a16ae-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a16ae-115">Next steps</span></span>

* <span data-ttu-id="a16ae-116">A virtuális hálózatokhoz hozzáadhat virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="a16ae-116">You can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="a16ae-117">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a16ae-117">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>