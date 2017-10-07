---
title: "Hello helyi hálózati átjáró IP-cím előtagokat és hello VPN-átjáró IP-cím módosítása |} Azure |} PowerShell |} Microsoft Docs"
description: "Ez a cikk végigvezeti a PowerShell használatával helyi hálózati átjáró IP-cím előtagokat módosítása"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 1353598b39a97fae9bdb424505a5ae2560482654
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a><span data-ttu-id="f676d-103">Helyi hálózati átjáró beállításainak módosítása a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="f676d-103">Modify local network gateway settings using PowerShell</span></span>

<span data-ttu-id="f676d-104">Egyes esetekben a helyi hálózati átjáró címelőtagja vagy GatewayIPAddress hello-beállítások módosítása.</span><span class="sxs-lookup"><span data-stu-id="f676d-104">Sometimes hello settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="f676d-105">Ez a cikk bemutatja, hogyan toomodify a helyi hálózati átjáró beállításokat.</span><span class="sxs-lookup"><span data-stu-id="f676d-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="f676d-106">Ezeket a beállításokat egy másik módszer a következő lista hello egy másik lehetőség kiválasztásával a is módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="f676d-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f676d-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f676d-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="f676d-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f676d-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="f676d-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f676d-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="f676d-110"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f676d-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="f676d-111">Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f676d-111">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="f676d-112">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) hello PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="f676d-112">See [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for more information about installing hello PowerShell cmdlets.</span></span>

## <span data-ttu-id="f676d-113"><a name="ipaddprefix"></a>IP-cím előtagokat módosítása</span><span class="sxs-lookup"><span data-stu-id="f676d-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="f676d-114"><a name="gwip"></a>Hello átjáró IP-cím módosítása</span><span class="sxs-lookup"><span data-stu-id="f676d-114"><a name="gwip"></a>Modify hello gateway IP address</span></span>

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="f676d-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f676d-115">Next steps</span></span>

<span data-ttu-id="f676d-116">Ellenőrizheti a gateway-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f676d-116">You can verify your gateway connection.</span></span> <span data-ttu-id="f676d-117">Lásd: [egy átjáró kapcsolat ellenőrzéséhez](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="f676d-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>