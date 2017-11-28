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
# <a name="how-tooconnect-and-log-on-tooan-azure-virtual-machine-running-windows"></a><span data-ttu-id="b8a99-103">Hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows</span><span class="sxs-lookup"><span data-stu-id="b8a99-103">How tooconnect and log on tooan Azure virtual machine running Windows</span></span>
<span data-ttu-id="b8a99-104">Hello fogjuk **Connect** hello Azure portál toostart egy távoli asztal (RDP) munkamenet Windows asztali gombjára.</span><span class="sxs-lookup"><span data-stu-id="b8a99-104">You'll use hello **Connect** button in hello Azure portal toostart a Remote Desktop (RDP) session from a Windows desktop.</span></span> <span data-ttu-id="b8a99-105">Először toohello virtuálisgép kapcsolódik-e, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="b8a99-105">First you connect toohello virtual machine, then you log on.</span></span>

<span data-ttu-id="b8a99-106">Windows virtuális gép Mac tooconnect tooa próbált meg, ha szüksége van-e RDP-ügyfelet Mac számítógépen, például tooinstall [Microsoft távoli asztal](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span><span class="sxs-lookup"><span data-stu-id="b8a99-106">If you are trying tooconnect tooa Windows VM from a Mac, you need tooinstall an RDP client for Mac like [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span></span>

## <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="b8a99-107">Csatlakoztassa toohello virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="b8a99-107">Connect toohello virtual machine</span></span>
1. <span data-ttu-id="b8a99-108">Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b8a99-108">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b8a99-109">Hello központ menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="b8a99-109">On hello Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="b8a99-110">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="b8a99-110">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="b8a99-111">Virtuális gép hello hello paneljén kattintson **Connect**.</span><span class="sxs-lookup"><span data-stu-id="b8a99-111">On hello blade for hello virtual machine, click **Connect**.</span></span>
   
    ![Képernyőfelvétel a hello Azure portál ábrázoló hogyan tooconnect tooyour virtuális gép.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > <span data-ttu-id="b8a99-113">Ha hello **Connect** hello portálon gomb szürkén jelenik meg, és nem csatlakoztatott tooAzure keresztül egy [Express Route](../../expressroute/expressroute-introduction.md) vagy [telephelyek közötti VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) kapcsolat, toocreate van szüksége, és rendelje hozzá a virtuális Gépet egy nyilvános IP-cím RDP használata előtt.</span><span class="sxs-lookup"><span data-stu-id="b8a99-113">If hello **Connect** button in hello portal is greyed out and you are not connected tooAzure via an [Express Route](../../expressroute/expressroute-introduction.md) or [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) connection, you need toocreate and assign your VM a public IP address before you can use RDP.</span></span> <span data-ttu-id="b8a99-114">További információ: [nyilvános IP-címek az Azure-ban](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b8a99-114">You can read more about [public IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>
   > 
   > 

## <a name="log-on-toohello-virtual-machine"></a><span data-ttu-id="b8a99-115">Jelentkezzen be a virtuális gép toohello</span><span class="sxs-lookup"><span data-stu-id="b8a99-115">Log on toohello virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="b8a99-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8a99-116">Next steps</span></span>
<span data-ttu-id="b8a99-117">Ha hiba történt a tooconnect meg, lásd: [távoli asztali kapcsolatainak hibaelhárítása](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8a99-117">If you run into trouble when you try tooconnect, see [Troubleshoot Remote Desktop connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="b8a99-118">Ez a cikk útmutatást nyújt a gyakori problémák diagnosztizálásához és elhárításához.</span><span class="sxs-lookup"><span data-stu-id="b8a99-118">This article walks you through diagnosing and resolving common problems.</span></span>

