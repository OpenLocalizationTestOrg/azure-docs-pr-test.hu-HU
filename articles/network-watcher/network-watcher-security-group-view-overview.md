---
title: "az Azure hálózati figyelőt aaaIntroduction toosecurity csoport nézetében |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt biztonsági nézet képességek áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: c2f6dbbffd0aedbb9db4b69d1758f2e66dd7abb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonetwork-security-group-view-in-azure-network-watcher"></a><span data-ttu-id="d5280-103">Bevezetés toonetwork biztonsági csoport megtekintése az Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="d5280-103">Introduction toonetwork security group view in Azure Network Watcher</span></span>

<span data-ttu-id="d5280-104">Hálózati biztonsági csoportok társított alhálózati szinten, vagy egy hálózati adapter szintjén.</span><span class="sxs-lookup"><span data-stu-id="d5280-104">Network Security groups are associated at a subnet level or at a NIC level.</span></span> <span data-ttu-id="d5280-105">Ha tartozó alhálózati szinten, tooall hello Virtuálisgép-példány hello alhálózati érvényes.</span><span class="sxs-lookup"><span data-stu-id="d5280-105">When associated at a subnet level, it applies tooall hello VM instances in hello subnet.</span></span> <span data-ttu-id="d5280-106">Hálózati biztonsági csoport megtekintése összes hello beállított NSG-ket és egy virtuális gép hello konfigurációs betekintést biztosít a hálózati és alhálózati szinten társított szabályokat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d5280-106">Network Security Group view returns all hello configured NSGs and rules that are associated at a NIC and subnet level for a virtual machine providing insight into hello configuration.</span></span> <span data-ttu-id="d5280-107">Továbbá a hello hatékony biztonsági szabályok visszaadott minden hello hálózati adapterek virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="d5280-107">In addition, hello effective security rules are returned for each of hello NICs in a VM.</span></span> <span data-ttu-id="d5280-108">Hálózati biztonsági csoport használatával nézet felmérheti a virtuális gépek, a hálózati biztonsági réseket, például a megnyitott portok.</span><span class="sxs-lookup"><span data-stu-id="d5280-108">Using Network Security Group view, you can assess a VM for network vulnerabilities such as open ports.</span></span> <span data-ttu-id="d5280-109">Ha a hálózati biztonsági csoport a várt módon működik alapján is ellenőrzéséhez egy [hello összehasonlítása konfigurálva és hatékony biztonsági szabályok hello](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="d5280-109">You can also validate if your Network Security Group is working as expected based on a [comparison between hello configured and hello effective security rules](network-watcher-nsg-auditing-powershell.md).</span></span>

<span data-ttu-id="d5280-110">Több kiterjesztett használati eset biztonsági, megfelelőségi és naplózási van.</span><span class="sxs-lookup"><span data-stu-id="d5280-110">A more extended use case is in security compliance and auditing.</span></span> <span data-ttu-id="d5280-111">Biztonsági irányításhoz modellként előíró meghatározott biztonsági szabályok adhat meg a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="d5280-111">You can define a prescriptive set of security rules as a model for security governance in your organization.</span></span> <span data-ttu-id="d5280-112">Egy rendszeres megfelelőségi naplózási programozott módon valósítható hello hatékony szabályok hello előíró szabályokkal összehasonlítva az egyes hello virtuális gépeket a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="d5280-112">A periodic compliance audit can be implemented in a programmatic way by comparing hello prescriptive rules with hello effective rules for each of hello VMs in your network.</span></span>

<span data-ttu-id="d5280-113">Hello a hatályos, a alhálózat, a hálózati illesztő és az alapértelmezett portál szabályok vannak osztva.</span><span class="sxs-lookup"><span data-stu-id="d5280-113">In hello portal rules are divided by Effective, Subnet, Network Interface, and Default.</span></span> <span data-ttu-id="d5280-114">Ez hello szabálya tooa virtuális géppé egyszerű nézetét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d5280-114">This provides a simple view into hello rules applied tooa virtual machine.</span></span> <span data-ttu-id="d5280-115">A Letöltés gombra kerül a tooeasily hello lapon függetlenül minden hello biztonsági szabályok letöltése CSV-fájlba.</span><span class="sxs-lookup"><span data-stu-id="d5280-115">A download button is provided tooeasily download all hello security rules no matter hello tab into a CSV file.</span></span>

![Biztonsági csoport megtekintése][1]

<span data-ttu-id="d5280-117">Szabályok választhatók ki, és tooshow hello hálózati biztonsági csoport és a forrás és a cél-előtagok megnyílik egy új panelen.</span><span class="sxs-lookup"><span data-stu-id="d5280-117">Rules can be selected and a new blade opens up tooshow hello Network Security Group and source and destination prefixes.</span></span> <span data-ttu-id="d5280-118">Ezen a panelen navigálhat közvetlenül toohello hálózati biztonsági csoport erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d5280-118">From this blade you can navigate directly toohello Network Security Group resource.</span></span>

![Részletezési][2]

### <a name="next-steps"></a><span data-ttu-id="d5280-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5280-120">Next steps</span></span>

<span data-ttu-id="d5280-121">Megtudhatja, hogyan tooaudit a hálózati biztonsági csoport beállításainak ellátogatva [naplózási hálózati biztonsági csoport beállításait a PowerShell használatával](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="d5280-121">Learn how tooaudit your Network Security Group settings by visiting [Audit Network Security Group settings with PowerShell](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









