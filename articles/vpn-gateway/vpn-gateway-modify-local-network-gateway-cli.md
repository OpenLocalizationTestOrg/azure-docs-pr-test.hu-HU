---
title: "Hello helyi hálózati átjáró IP-cím előtagokat és hello VPN-átjáró IP-cím módosítása |} Azure |} PARANCSSORI FELÜLETTEL |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan módosítja a helyi hálózati átjáró hello Azure CLI használata IP-cím előtagokat."
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
ms.openlocfilehash: 4b8bbf3e9d3d42ac2d12f87360fa0a4134c57a21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-hello-azure-cli"></a><span data-ttu-id="31812-103">Hello Azure parancssori felület használatával a helyi hálózati átjáró beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="31812-103">Modify local network gateway settings using hello Azure CLI</span></span>

<span data-ttu-id="31812-104">Egyes esetekben a helyi hálózati átjáró címelőtagja vagy az átjáró IP-cím hello beállításait módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="31812-104">Sometimes hello settings for your local network gateway Address Prefix or Gateway IP Address change.</span></span> <span data-ttu-id="31812-105">Ez a cikk bemutatja, hogyan toomodify a helyi hálózati átjáró beállításokat.</span><span class="sxs-lookup"><span data-stu-id="31812-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="31812-106">Ezeket a beállításokat egy másik módszer a következő lista hello egy másik lehetőség kiválasztásával a is módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="31812-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="31812-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31812-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="31812-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31812-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="31812-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="31812-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="31812-110"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="31812-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="31812-111">Hello hello parancssori felület parancsait (2.0-s vagy újabb) legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="31812-111">Install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="31812-112">Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="31812-112">For information about installing hello CLI commands, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="31812-113"><a name="ipaddprefix"></a>IP-cím előtagokat módosítása</span><span class="sxs-lookup"><span data-stu-id="31812-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <span data-ttu-id="31812-114"><a name="gwip"></a>Hello átjáró IP-cím módosítása</span><span class="sxs-lookup"><span data-stu-id="31812-114"><a name="gwip"></a>Modify hello gateway IP address</span></span>

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="31812-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31812-115">Next steps</span></span>

<span data-ttu-id="31812-116">Ellenőrizheti a gateway-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="31812-116">You can verify your gateway connection.</span></span> <span data-ttu-id="31812-117">Lásd: [egy átjáró kapcsolat ellenőrzéséhez](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="31812-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

