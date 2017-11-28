---
title: a VPN Gateway-kapcsolatot aaaVerify |} Microsoft Docs
description: "Ez a cikk bemutatja, hogyan tooverify a virtuális hálózat VPN Gateway-kapcsolatot."
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
ms.openlocfilehash: 0d3da94a76b36251d629f82b1575328c7ac10b26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="verify-a-vpn-gateway-connection"></a><span data-ttu-id="29688-103">A VPN-átjáró kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="29688-103">Verify a VPN Gateway connection</span></span>

<span data-ttu-id="29688-104">Ez a cikk bemutatja, hogyan tooverify hello klasszikus és Resource Manager üzembe helyezési modellel VPN gateway-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="29688-104">This article shows you how tooverify a VPN gateway connection for both hello classic and Resource Manager deployment models.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="29688-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="29688-105">Azure portal</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a><span data-ttu-id="29688-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29688-106">PowerShell</span></span>

<span data-ttu-id="29688-107">a VPN gateway-kapcsolatot az erőforrás-kezelő telepítési hello tooverify modell a PowerShell használatával, hello hello legújabb verziójának telepítéséhez [Azure Resource Manager PowerShell-parancsmagok](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="29688-107">tooverify a VPN gateway connection for hello Resource Manager deployment model using PowerShell, install hello latest version of hello [Azure Resource Manager PowerShell cmdlets](/powershell/azure/overview).</span></span>

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a><span data-ttu-id="29688-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="29688-108">Azure CLI</span></span>

<span data-ttu-id="29688-109">a VPN gateway-kapcsolatot az erőforrás-kezelő telepítési hello tooverify modellhez tartozó Azure parancssori felület használatával, hello hello legújabb verziójának telepítéséhez [parancssori felület parancsait](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="29688-109">tooverify a VPN gateway connection for hello Resource Manager deployment model using Azure CLI, install hello latest version of hello [CLI commands](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 or later).</span></span>

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a><span data-ttu-id="29688-110">(Klasszikus) Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="29688-110">Azure portal (classic)</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a><span data-ttu-id="29688-111">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="29688-111">PowerShell (classic)</span></span>

<span data-ttu-id="29688-112">a VPN gateway-kapcsolatot a hello klasszikus telepítési modell powershellel, tooverify hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="29688-112">tooverify your VPN gateway connection for hello classic deployment model using PowerShell, install hello latest versions of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="29688-113">Lehet, hogy toodownload és a telepítés hello [szolgáltatásfelügyelet](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modul.</span><span class="sxs-lookup"><span data-stu-id="29688-113">Be sure toodownload and install hello [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) module.</span></span> <span data-ttu-id="29688-114">Használja a "Hozzáadás-AzureAccount" toolog toohello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="29688-114">Use 'Add-AzureAccount' toolog in toohello classic deployment model.</span></span>

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a><span data-ttu-id="29688-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29688-115">Next steps</span></span>

* <span data-ttu-id="29688-116">Virtuális gépek tooyour virtuális hálózatokat adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="29688-116">You can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="29688-117">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29688-117">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
