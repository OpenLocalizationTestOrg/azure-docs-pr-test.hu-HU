---
title: "aaaIntroduction tooIP folyamata győződjön meg az Azure hálózati figyelőt |} Microsoft Docs"
description: "Ezen a lapon áttekintést nyújt a hálózati figyelő IP hello folyamata ellenőrzési funkciók"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b648a4816a7ffdc6ca54462944b574e2395e8298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooip-flow-verify-in-azure-network-watcher"></a><span data-ttu-id="32add-103">Bevezetés tooIP folyamata győződjön meg az Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="32add-103">Introduction tooIP flow verify in Azure Network Watcher</span></span>

<span data-ttu-id="32add-104">IP-adatfolyam ellenőrizze ellenőrzéseket, ha egy csomag engedélyezett vagy megtagadott tooor 5 rekordos információk alapján virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="32add-104">IP flow verify checks if a packet is allowed or denied tooor from a virtual machine based on 5-tuple information.</span></span> <span data-ttu-id="32add-105">Ez az információ irányát, protokoll, helyi IP, távoli IP, helyi port és távoli port áll.</span><span class="sxs-lookup"><span data-stu-id="32add-105">This information consists of direction, protocol, local IP, remote IP, local port, and remote port.</span></span> <span data-ttu-id="32add-106">Ha hello csomagok megtagadta a biztonsági csoport, hello csomagok megtagadva hello szabály hello nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="32add-106">If hello packet is denied by a security group, hello name of hello rule that denied hello packet is returned.</span></span> <span data-ttu-id="32add-107">Minden cél- és IP-választható ki, miközben segít gyorsan diagnosztizálhatja a kapcsolat hibái vagy a toohello rendszergazdák internet pedig vagy toohello helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="32add-107">While any source or destination IP can be chosen, this feature helps administrators quickly diagnose connectivity issues from or toohello internet and from or toohello on-premises environment.</span></span>

<span data-ttu-id="32add-108">IP-adatfolyam ellenőrizze a hálózati adaptert egy virtuális gép célozza.</span><span class="sxs-lookup"><span data-stu-id="32add-108">IP flow verify targets a network interface of a virtual machine.</span></span> <span data-ttu-id="32add-109">Forgalom majd ellenőrizve hello konfigurált beállítások tooor az, hogy a hálózati illesztő alapján.</span><span class="sxs-lookup"><span data-stu-id="32add-109">Traffic flow is then verified based on hello configured settings tooor from that network interface.</span></span> <span data-ttu-id="32add-110">Ezzel a képességgel olyan hasznos, erősítse meg, ha egy szabály a hálózati biztonsági csoport blokkolja a belépési vagy kilépési forgalom tooor virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="32add-110">This capability is useful in confirming if a rule in a Network Security Group is blocking ingress or egress traffic tooor from a virtual machine.</span></span>

<span data-ttu-id="32add-111">Győződjön meg arról, hogy tervezi-e toorun IP folyamat minden régióban létrehozott hálózati figyelőt kell toobe példánya.</span><span class="sxs-lookup"><span data-stu-id="32add-111">An instance of Network Watcher needs toobe created in all regions that you plan toorun IP flow verify.</span></span> <span data-ttu-id="32add-112">Hálózati figyelőt egy regionális szolgáltatás, és csak a hello erőforrások lefutott is ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="32add-112">Network Watcher is a regional service and can only be ran against resources in hello same region.</span></span> <span data-ttu-id="32add-113">Ez nem befolyásolja a hello IP folyamat eredményeinek ellenőrzése társított hálózati adapter még mindig visszatér hello hello útvonal szerint.</span><span class="sxs-lookup"><span data-stu-id="32add-113">This does not affect hello results of IP flow verify as hello route associated with hello NIC will still be returned.</span></span>

![1][1]

## <a name="next-steps"></a><span data-ttu-id="32add-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32add-115">Next steps</span></span>

<span data-ttu-id="32add-116">Látogasson el a következő cikk toolearn, ha egy csomag engedélyezett vagy megtagadott egy adott virtuális gép hello portálon keresztül hello.</span><span class="sxs-lookup"><span data-stu-id="32add-116">Visit hello following article toolearn if a packet is allowed or denied for a specific virtual machine through hello portal.</span></span> [<span data-ttu-id="32add-117">Ellenőrizze, hogy ha a forgalom engedélyezve van a virtuális gép IP Flow ellenőrizze és hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="32add-117">Check if traffic is allowed on a VM with IP Flow Verify using hello portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












