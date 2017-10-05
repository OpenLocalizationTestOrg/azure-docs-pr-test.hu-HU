---
title: "A Linux virtuális gépek SAP használatával |} Microsoft Docs"
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
ms.openlocfilehash: 66eb53f99ce4b30ec283243deb3649c9ca897a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="798ca-103">SAP használata a Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="798ca-103">Using SAP on Linux virtual machines in Azure</span></span>
<span data-ttu-id="798ca-104">A felhőalapú számítástechnika széles körben használt fogalom, amely egyre fontosabbá válik az informatikai iparágban, a kisvállalatok esetében ugyanúgy, mint a nagy, nemzetközi vállalatok számára.</span><span class="sxs-lookup"><span data-stu-id="798ca-104">Cloud Computing is a widely used term which is gaining more and more importance within the IT industry, from small companies up to large and multinational corporations.</span></span> <span data-ttu-id="798ca-105">A Microsoft Azure a Microsoft új lehetőségek széles skáláját nyújtó felhőszolgáltatás-platformja.</span><span class="sxs-lookup"><span data-stu-id="798ca-105">Microsoft Azure is the Cloud Services Platform from Microsoft which offers a wide spectrum of new possibilities.</span></span> <span data-ttu-id="798ca-106">Az ügyfelek most már gyorsan építhetnek ki és vonhatnak ki az üzemből felhőszolgáltatásként rendelkezésre bocsájtott alkalmazásokat, ami azt jelenti, hogy többé nem érvényesülnek a technikai vagy költségvetési korlátozások.</span><span class="sxs-lookup"><span data-stu-id="798ca-106">Now customers are able to rapidly provision and de-provision applications as Cloud-Services, so they are not limited to technical or budgeting restrictions.</span></span> <span data-ttu-id="798ca-107">A vállalatoknak nem kell időt és forrásokat befektetni a hardverinfrastruktúrába: ehelyett az alkalmazásra, az üzleti folyamatokra és az ügyfeleknek és felhasználóknak nyújtott értékre koncentrálhatnak.</span><span class="sxs-lookup"><span data-stu-id="798ca-107">Instead of investing time and budget into hardware infrastructure, companies can focus on the application, business processes and its benefits for customers and users.</span></span>

<span data-ttu-id="798ca-108">Microsoft Azure virtuális gépekkel Microsoft biztosít az átfogó infrastruktúra (IaaS) szolgáltatás platformként.</span><span class="sxs-lookup"><span data-stu-id="798ca-108">With Microsoft Azure virtual machines, Microsoft offers a comprehensive Infrastructure as a Service (IaaS) platform.</span></span> <span data-ttu-id="798ca-109">Az Azure virtuális gépek támogatják az SAP NetWeaver-alapú alkalmazásokat (IaaS).</span><span class="sxs-lookup"><span data-stu-id="798ca-109">SAP NetWeaver based applications are supported on Azure Virtual Machines (IaaS).</span></span> <span data-ttu-id="798ca-110">Az alábbi tanulmányok azt ismertetik, hogyan tervezésével és megvalósításával SAP NetWeaver-alapú alkalmazások a Windows virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="798ca-110">The whitepapers below describe how to plan and implement SAP NetWeaver based applications on Windows virtual machines in Azure.</span></span> <span data-ttu-id="798ca-111">SAP NetWeaver-alapú alkalmazások valósítja meg a [Windows virtuális gépek](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="798ca-111">You can also implement SAP NetWeaver based applications on [Windows virtual machines](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a><span data-ttu-id="798ca-112">SAP NetWeaver Azure SUSE Linux virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="798ca-112">SAP NetWeaver on Azure SUSE Linux Virtual Machines</span></span>
<span data-ttu-id="798ca-113">Cím: A Microsoft Azure SUSE Linux virtuális gépeken SAP NetWeaver tesztelése</span><span class="sxs-lookup"><span data-stu-id="798ca-113">Title: Testing SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span>

<span data-ttu-id="798ca-114">Összefoglalás: Nincs hivatalos SAP támogatás a SAP NetWeaver ezen a ponton a időben történő futtatásához Azure Linux virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="798ca-114">Summary: There is no official SAP support for running SAP NetWeaver on Azure Linux VMs at this point in time.</span></span> <span data-ttu-id="798ca-115">Azonban érdemes lehet megtenni, néhány tesztelése, vagy vegye fontolóra SAP bemutató vagy futtatásához képzési rendszerek Azure Linux virtuális gépek mindaddig, amíg nincs szükség az SAP támogatási szolgálatától.</span><span class="sxs-lookup"><span data-stu-id="798ca-115">Nevertheless customers might want to do some testing or might consider to run SAP demo or training systems on Azure Linux VMs as long as there is no need for contacting SAP support.</span></span> <span data-ttu-id="798ca-116">Ez a cikk segítségül szolgálhat Azure SUSE Linux virtuális gépek telepítése a futó SAP, és néhány alapvető mutatókat biztosít közös nehézségek elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="798ca-116">This article should help setting up Azure SUSE Linux VMs for running SAP and gives some basic hints in order to avoid common potential pitfalls.</span></span>

<span data-ttu-id="798ca-117">Frissített: 2015. decemberi</span><span class="sxs-lookup"><span data-stu-id="798ca-117">Updated: December 2015</span></span>

[<span data-ttu-id="798ca-118">Ez a cikk itt található</span><span class="sxs-lookup"><span data-stu-id="798ca-118">This article can be found here</span></span>](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

