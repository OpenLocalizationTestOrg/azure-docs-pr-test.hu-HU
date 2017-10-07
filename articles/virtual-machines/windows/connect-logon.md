---
title: "Windows Server virtuális gép aaaConnect tooa |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconnect és bejelentkezés tooa Windows virtuális gép használatával hello Azure portál és hello Resource Manager telepítési modell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: cynthn
ms.openlocfilehash: dc397f435ef165ae5af09f1d037ad3d520bb7ac3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-and-log-on-tooan-azure-virtual-machine-running-windows"></a>Hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows
Hello fogjuk **Connect** hello Azure portál toostart egy távoli asztal (RDP) munkamenet Windows asztali gombjára. Először toohello virtuálisgép kapcsolódik-e, majd jelentkezzen be.

Windows virtuális gép Mac tooconnect tooa próbált meg, ha szüksége van-e RDP-ügyfelet Mac számítógépen, például tooinstall [Microsoft távoli asztal](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-toohello-virtual-machine"></a>Csatlakoztassa toohello virtuális gépet
1. Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello központ menüben kattintson a **virtuális gépek**.
3. Válassza ki a virtuális gép hello hello listából.
4. Virtuális gép hello hello paneljén kattintson **Connect**.
   
    ![Képernyőfelvétel a hello Azure portál ábrázoló hogyan tooconnect tooyour virtuális gép.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > Ha hello **Connect** hello portálon gomb szürkén jelenik meg, és nem csatlakoztatott tooAzure keresztül egy [Express Route](../../expressroute/expressroute-introduction.md) vagy [telephelyek közötti VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) kapcsolat, toocreate van szüksége, és rendelje hozzá a virtuális Gépet egy nyilvános IP-cím RDP használata előtt. További információ: [nyilvános IP-címek az Azure-ban](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="log-on-toohello-virtual-machine"></a>Jelentkezzen be a virtuális gép toohello
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Következő lépések
Ha hiba történt a tooconnect meg, lásd: [távoli asztali kapcsolatainak hibaelhárítása](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ez a cikk útmutatást nyújt a gyakori problémák diagnosztizálásához és elhárításához.

