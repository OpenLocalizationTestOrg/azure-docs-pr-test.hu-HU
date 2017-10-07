---
title: "Azure hálózati figyelő IP folyamat aaaVerify forgalom ellenőrizze - Azure-portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: abf639f36d32f3416dd927e66b635267b746e62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="89f1b-103">Ha a forgalom engedélyezett vagy megtagadott a tooor IP Flow ellenőrizze a virtuális gép alapján Azure hálózati figyelőt összetevője egy ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89f1b-103">Check if traffic is allowed or denied tooor from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="89f1b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="89f1b-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="89f1b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="89f1b-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="89f1b-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="89f1b-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="89f1b-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="89f1b-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="89f1b-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="89f1b-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="89f1b-109">IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="89f1b-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="89f1b-110">hello érvényesítési futtathatja a bejövő vagy kimenő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="89f1b-110">hello validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="89f1b-111">Ebben a forgatókönyvben hasznos tooget, hogy a virtuális gép működik tooan külső erőforrás vagy egy másik erőforrás a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="89f1b-111">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or another resource.</span></span> <span data-ttu-id="89f1b-112">IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="89f1b-112">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="89f1b-113">Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.</span><span class="sxs-lookup"><span data-stu-id="89f1b-113">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="89f1b-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="89f1b-114">Before you begin</span></span>

<span data-ttu-id="89f1b-115">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt, vagy hálózati figyelőt meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="89f1b-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="89f1b-116">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="89f1b-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="89f1b-117">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="89f1b-117">Scenario</span></span>

<span data-ttu-id="89f1b-118">Ez a forgatókönyv használ tooverify IP Flow győződjön meg arról, ha egy virtuális gép működik tooanother gép 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="89f1b-118">This scenario uses IP Flow Verify tooverify if a virtual machine can talk tooanother machine over port 443.</span></span> <span data-ttu-id="89f1b-119">Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza.</span><span class="sxs-lookup"><span data-stu-id="89f1b-119">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="89f1b-120">toolearn IP Flow ellenőrzéséhez kapcsolatos további információkért látogasson el [IP-adatfolyam ellenőrizze áttekintése](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="89f1b-120">toolearn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

### <a name="run-ip-flow-verify"></a><span data-ttu-id="89f1b-121">Futtatási IP-adatfolyam ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89f1b-121">Run IP flow verify</span></span>

<span data-ttu-id="89f1b-122">Nyissa meg a tooyour hálózati figyelőt, és kattintson a **IP folyamata ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="89f1b-122">Navigate tooyour Network Watcher and click **IP flow verify**.</span></span> <span data-ttu-id="89f1b-123">Válassza ki a hello virtuális gép és a hálózati illesztő kívánt tooverify forgalmát.</span><span class="sxs-lookup"><span data-stu-id="89f1b-123">Select hello virtual machine and network interface you want tooverify traffic from.</span></span> <span data-ttu-id="89f1b-124">Adja meg a szűrési további adatokat, és kattintson a **ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="89f1b-124">Enter any additional filtering information and click **Check**.</span></span>

<span data-ttu-id="89f1b-125">Miután rákattintott **ellenőrizze**, hello folyamat a megadott hello feltételeknek megfelelő jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="89f1b-125">Once you click **Check**, hello flow based on hello criteria you provided is checked.</span></span> <span data-ttu-id="89f1b-126">hello eredménye vagy **engedélyezett hozzáférési** vagy **hozzáférés megtagadva**.</span><span class="sxs-lookup"><span data-stu-id="89f1b-126">hello result is either **Access allowed** or **Access denied**.</span></span> <span data-ttu-id="89f1b-127">Ha a hozzáférés megtagadva hello hálózati biztonsági csoport (NSG), és biztonsági szabály, amely a forgalom blokkolása valósul meg.</span><span class="sxs-lookup"><span data-stu-id="89f1b-127">If access is denied, hello Network Security Group (NSG) and security rule that block traffic is provided.</span></span> <span data-ttu-id="89f1b-128">Ha forgalom hello szolgáltatásmegtagadásos normális működés, majd hello szabály sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="89f1b-128">If hello denial of traffic is expected behavior, then hello rule was successful.</span></span>

> [!NOTE]
> <span data-ttu-id="89f1b-129">IP-adatfolyam ellenőrizze, hogy megköveteli, hogy a virtuális gép erőforrásához hello le van foglalva.</span><span class="sxs-lookup"><span data-stu-id="89f1b-129">IP flow verify requires that hello VM resource is allocated.</span></span>

<span data-ttu-id="89f1b-130">Ahogy látja, a kép a következő hello, hello kimenő HTTPS-forgalom engedélyezve volt.</span><span class="sxs-lookup"><span data-stu-id="89f1b-130">As you can see from hello following image, hello outbound HTTPS traffic was allowed.</span></span>

![IP-adatfolyam ellenőrzése – áttekintés][1]

<span data-ttu-id="89f1b-132">Látható kép a következő hello, megváltozott-e a forgalom tooinbound és hello bejövő megváltozott port too123.</span><span class="sxs-lookup"><span data-stu-id="89f1b-132">As seen in hello following image, traffic is changed tooinbound and hello inbound port changed too123.</span></span> <span data-ttu-id="89f1b-133">Forgalom most megtagadva, "Hozzáférés megtagadva" üdvözlőüzenetére valósul meg hello hálózati biztonsági csoport és a biztonsági szabályt, amely megtagadja a hello forgalom együtt.</span><span class="sxs-lookup"><span data-stu-id="89f1b-133">Traffic is now denied, hello message "Access denied" is provided along with hello network security group and security rule that deny hello traffic.</span></span>

![IP-adatfolyam eredmények][2]

## <a name="next-steps"></a><span data-ttu-id="89f1b-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89f1b-135">Next steps</span></span>

<span data-ttu-id="89f1b-136">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello hálózati biztonsági csoport és a biztonsági szabályok definiált.</span><span class="sxs-lookup"><span data-stu-id="89f1b-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













