---
title: "a Linux virtuális gépek SAP aaaUsing |} Microsoft Docs"
description: "Ismerje meg az SAP használatát Linux virtuális gépeken (VM) a Microsoft Azure megoldásban"
services: virtual-machines-linux,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: f9cd93dc-71ad-48a4-8778-4e48aec484a6
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: fd4aad83d99ef5286488aaab6552fd67a5e35a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="d82a6-103">SAP használata a Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="d82a6-103">Using SAP on Linux virtual machines in Azure</span></span>
<span data-ttu-id="d82a6-104">A felhőalapú informatika egy olyan széles körben használt kifejezés, amely egyre több fontos hello informatikai iparági belül van való kis vállalatok toolarge és multinacionális cég vállalatok fel.</span><span class="sxs-lookup"><span data-stu-id="d82a6-104">Cloud Computing is a widely used term which is gaining more and more importance within hello IT industry, from small companies up toolarge and multinational corporations.</span></span> <span data-ttu-id="d82a6-105">A Microsoft Azure Cloud Services Platform széles skálája új lehetőségeket kínál, amelyek Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="d82a6-105">Microsoft Azure is hello Cloud Services Platform from Microsoft which offers a wide spectrum of new possibilities.</span></span> <span data-ttu-id="d82a6-106">Most ügyfelek rendszer képes toorapidly kiépítése és deaktiválás rendelkezés alkalmazás Felhőszolgáltatások, mint, hogy ne korlátozott tootechnical vagy költségvetési korlátozások.</span><span class="sxs-lookup"><span data-stu-id="d82a6-106">Now customers are able toorapidly provision and de-provision applications as Cloud-Services, so they are not limited tootechnical or budgeting restrictions.</span></span> <span data-ttu-id="d82a6-107">Helyett befektetés időt és a hardver infrastruktúra, a vállalatok összpontosíthat hello alkalmazás, az üzleti folyamatokat és az előnye az ügyfelek és felhasználók.</span><span class="sxs-lookup"><span data-stu-id="d82a6-107">Instead of investing time and budget into hardware infrastructure, companies can focus on hello application, business processes and its benefits for customers and users.</span></span>

<span data-ttu-id="d82a6-108">Microsoft Azure virtuális gépekkel Microsoft biztosít az átfogó infrastruktúra (IaaS) szolgáltatás platformként.</span><span class="sxs-lookup"><span data-stu-id="d82a6-108">With Microsoft Azure virtual machines, Microsoft offers a comprehensive Infrastructure as a Service (IaaS) platform.</span></span> <span data-ttu-id="d82a6-109">Az Azure virtuális gépek támogatják az SAP NetWeaver-alapú alkalmazásokat (IaaS).</span><span class="sxs-lookup"><span data-stu-id="d82a6-109">SAP NetWeaver based applications are supported on Azure Virtual Machines (IaaS).</span></span> <span data-ttu-id="d82a6-110">az alábbi hello tanulmányok ismertetik, hogyan tooplan és megvalósítása SAP NetWeaver alapú alkalmazások a Windows virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d82a6-110">hello whitepapers below describe how tooplan and implement SAP NetWeaver based applications on Windows virtual machines in Azure.</span></span> <span data-ttu-id="d82a6-111">SAP NetWeaver-alapú alkalmazások valósítja meg a [Windows virtuális gépek](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d82a6-111">You can also implement SAP NetWeaver based applications on [Windows virtual machines](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a><span data-ttu-id="d82a6-112">SAP NetWeaver Azure SUSE Linux virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="d82a6-112">SAP NetWeaver on Azure SUSE Linux Virtual Machines</span></span>
<span data-ttu-id="d82a6-113">cím: Azure virtuális gépeken Microsoft SUSE Linux SAP NetWeaver aaaTesting</span><span class="sxs-lookup"><span data-stu-id="d82a6-113">title: aaaTesting SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span>

<span data-ttu-id="d82a6-114">Összefoglalás: Nincs hivatalos SAP támogatás a SAP NetWeaver ezen a ponton a időben történő futtatásához Azure Linux virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="d82a6-114">Summary: There is no official SAP support for running SAP NetWeaver on Azure Linux VMs at this point in time.</span></span> <span data-ttu-id="d82a6-115">Az ügyfelek azonban célszerű toodo néhány tesztelése, vagy vegye fontolóra toorun SAP bemutató vagy képzési rendszerek Azure Linux virtuális gépek mindaddig, amíg nincs szükség az SAP támogatási szolgálatától.</span><span class="sxs-lookup"><span data-stu-id="d82a6-115">Nevertheless customers might want toodo some testing or might consider toorun SAP demo or training systems on Azure Linux VMs as long as there is no need for contacting SAP support.</span></span> <span data-ttu-id="d82a6-116">Ez a cikk segítségül szolgálhat Azure SUSE Linux virtuális gépek telepítése a futó SAP, valamint módot adnak számukra az alapvető megpróbálkozna sorrendben, tooavoid közös nehézségek.</span><span class="sxs-lookup"><span data-stu-id="d82a6-116">This article should help setting up Azure SUSE Linux VMs for running SAP and gives some basic hints in order tooavoid common potential pitfalls.</span></span>

<span data-ttu-id="d82a6-117">Frissített: 2015. decemberi</span><span class="sxs-lookup"><span data-stu-id="d82a6-117">Updated: December 2015</span></span>

[<span data-ttu-id="d82a6-118">Ez a cikk itt található</span><span class="sxs-lookup"><span data-stu-id="d82a6-118">This article can be found here</span></span>](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

