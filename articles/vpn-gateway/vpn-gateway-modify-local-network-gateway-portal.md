---
title: "Hello helyi hálózati átjáró IP-cím előtagokat és hello VPN-átjáró IP-cím módosítása |} Azure |} Portál |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan módosítja a hello Azure-portál használatával helyi hálózati átjáró IP-cím előtagokat."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 001df7b748ccc234d87aab3501a4f0e4ddfe60f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-hello-azure-portal"></a><span data-ttu-id="87a75-103">Hello Azure-portál használatával a helyi hálózati átjáró beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="87a75-103">Modify local network gateway settings using hello Azure portal</span></span>

<span data-ttu-id="87a75-104">Egyes esetekben a helyi hálózati átjáró címelőtagja vagy GatewayIPAddress hello-beállítások módosítása.</span><span class="sxs-lookup"><span data-stu-id="87a75-104">Sometimes hello settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="87a75-105">Ez a cikk bemutatja, hogyan toomodify a helyi hálózati átjáró beállításokat.</span><span class="sxs-lookup"><span data-stu-id="87a75-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="87a75-106">Ezeket a beállításokat egy másik módszer a következő lista hello egy másik lehetőség kiválasztásával a is módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="87a75-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="87a75-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="87a75-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="87a75-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="87a75-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="87a75-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="87a75-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <span data-ttu-id="87a75-110"><a name="ipaddprefix"></a>IP-cím előtagokat módosítása</span><span class="sxs-lookup"><span data-stu-id="87a75-110"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

<span data-ttu-id="87a75-111">IP-cím előtagokat módosításakor hello követi függ, hogy rendelkezik-e a helyi hálózati átjáró kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="87a75-111">When you modify IP address prefixes, hello steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <span data-ttu-id="87a75-112"><a name="gwip"></a>Hello átjáró IP-cím módosítása</span><span class="sxs-lookup"><span data-stu-id="87a75-112"><a name="gwip"></a>Modify hello gateway IP address</span></span>

<span data-ttu-id="87a75-113">Nyilvános IP-címének hello VPN-eszköz, amelyet az tooconnect toohas megváltozásakor toomodify hello helyi hálózati átjáró tooreflect módosító szüksége.</span><span class="sxs-lookup"><span data-stu-id="87a75-113">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="87a75-114">Hello nyilvános IP-cím módosítása esetén hello követi függ, hogy rendelkezik-e a helyi hálózati átjáró kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="87a75-114">When you change hello public IP address, hello steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a><span data-ttu-id="87a75-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87a75-115">Next steps</span></span>

<span data-ttu-id="87a75-116">Ellenőrizheti a gateway-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="87a75-116">You can verify your gateway connection.</span></span> <span data-ttu-id="87a75-117">Lásd: [egy átjáró kapcsolat ellenőrzéséhez](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="87a75-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>