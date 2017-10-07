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
# <a name="modify-local-network-gateway-settings-using-powershell"></a>Helyi hálózati átjáró beállításainak módosítása a PowerShell-lel

Egyes esetekben a helyi hálózati átjáró címelőtagja vagy GatewayIPAddress hello-beállítások módosítása. Ez a cikk bemutatja, hogyan toomodify a helyi hálózati átjáró beállításokat. Ezeket a beállításokat egy másik módszer a következő lista hello egy másik lehetőség kiválasztásával a is módosíthatja:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Előkészületek

Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) hello PowerShell-parancsmagok telepítéséről további információt.

## <a name="ipaddprefix"></a>IP-cím előtagokat módosítása

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="gwip"></a>Hello átjáró IP-cím módosítása

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Következő lépések

Ellenőrizheti a gateway-kapcsolatot. Lásd: [egy átjáró kapcsolat ellenőrzéséhez](vpn-gateway-verify-connection-resource-manager.md).