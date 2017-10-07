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
# <a name="verify-a-vpn-gateway-connection"></a>A VPN-átjáró kapcsolat ellenőrzése

Ez a cikk bemutatja, hogyan tooverify hello klasszikus és Resource Manager üzembe helyezési modellel VPN gateway-kapcsolattal.

## <a name="azure-portal"></a>Azure Portal

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

a VPN gateway-kapcsolatot az erőforrás-kezelő telepítési hello tooverify modell a PowerShell használatával, hello hello legújabb verziójának telepítéséhez [Azure Resource Manager PowerShell-parancsmagok](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

a VPN gateway-kapcsolatot az erőforrás-kezelő telepítési hello tooverify modellhez tartozó Azure parancssori felület használatával, hello hello legújabb verziójának telepítéséhez [parancssori felület parancsait](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0-s vagy újabb).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>(Klasszikus) Azure-portálon

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (klasszikus)

a VPN gateway-kapcsolatot a hello klasszikus telepítési modell powershellel, tooverify hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Lehet, hogy toodownload és a telepítés hello [szolgáltatásfelügyelet](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modul. Használja a "Hozzáadás-AzureAccount" toolog toohello klasszikus üzembe helyezési modellben.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Következő lépések

* Virtuális gépek tooyour virtuális hálózatokat adhat hozzá. A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
