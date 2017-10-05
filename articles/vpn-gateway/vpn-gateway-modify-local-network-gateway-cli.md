---
title: "Módosítsa a helyi hálózati átjáró IP-cím előtagokat és a VPN-átjáró IP-címe |} Azure |} PARANCSSORI FELÜLETTEL |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan módosítása a helyi hálózati átjáró, az Azure parancssori felület használatával IP-cím előtagokat."
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
ms.openlocfilehash: 7db1ad970ebb93d46d5a861f9a9b27bf121531a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a><span data-ttu-id="5f9ba-103">Az Azure parancssori felület használatával a helyi hálózati átjáró beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="5f9ba-103">Modify local network gateway settings using the Azure CLI</span></span>

<span data-ttu-id="5f9ba-104">Egyes esetekben a helyi hálózati átjáró címelőtagja vagy az átjáró IP-cím beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="5f9ba-104">Sometimes the settings for your local network gateway Address Prefix or Gateway IP Address change.</span></span> <span data-ttu-id="5f9ba-105">Ez a cikk bemutatja, hogyan módosíthatja a helyi hálózati átjáró beállításokat.</span><span class="sxs-lookup"><span data-stu-id="5f9ba-105">This article shows you how to modify your local network gateway settings.</span></span> <span data-ttu-id="5f9ba-106">Emellett módosíthatja ezeket a beállításokat egy másik módszer a egy másik lehetőség kiválasztásával az alábbi listából:</span><span class="sxs-lookup"><span data-stu-id="5f9ba-106">You can also modify these settings using a different method by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5f9ba-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5f9ba-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="5f9ba-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5f9ba-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="5f9ba-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5f9ba-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="5f9ba-110"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5f9ba-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="5f9ba-111">Telepítse a legújabb verzióját a parancssori felület parancsait (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="5f9ba-111">Install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="5f9ba-112">Információk a CLI-parancsok telepítéséről: [Az Azure CLI 2.0-s verziójának telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5f9ba-112">For information about installing the CLI commands, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="5f9ba-113"><a name="ipaddprefix"></a>IP-cím előtagokat módosítása</span><span class="sxs-lookup"><span data-stu-id="5f9ba-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <span data-ttu-id="5f9ba-114"><a name="gwip"></a>Az átjáró IP-cím módosítása</span><span class="sxs-lookup"><span data-stu-id="5f9ba-114"><a name="gwip"></a>Modify the gateway IP address</span></span>

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="5f9ba-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f9ba-115">Next steps</span></span>

<span data-ttu-id="5f9ba-116">Ellenőrizheti a gateway-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5f9ba-116">You can verify your gateway connection.</span></span> <span data-ttu-id="5f9ba-117">Lásd: [egy átjáró kapcsolat ellenőrzéséhez](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="5f9ba-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

