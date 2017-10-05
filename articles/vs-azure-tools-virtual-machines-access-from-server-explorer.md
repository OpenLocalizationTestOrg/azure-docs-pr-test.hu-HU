---
title: "Az Azure virtuális gépek használata a Server Explorer |} Microsoft Docs"
description: "Get megtekintése létrehozása és kezelése az Azure virtuális gépek (VM) a Visual Studio Server Explorer."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: fcbb00cc2f00691e25ea84333e8c418b08210a67
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a><span data-ttu-id="12845-103">A Server Explorer Azure virtuális gépek elérése</span><span class="sxs-lookup"><span data-stu-id="12845-103">Accessing Azure Virtual Machines from Server Explorer</span></span>
<span data-ttu-id="12845-104">A Visual Studio Server Explorer használatával jelenítheti meg információkat az Azure által futtatott virtuális gépek kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="12845-104">By using Server Explorer in Visual Studio, you can display information about your virtual machines hosted by Azure.</span></span>

## <a name="accessing-virtual-machines-in-server-explorer"></a><span data-ttu-id="12845-105">A Server Explorer virtuális gépek elérése</span><span class="sxs-lookup"><span data-stu-id="12845-105">Accessing virtual machines in Server Explorer</span></span>
<span data-ttu-id="12845-106">Ha az Azure-ban üzemeltetett virtuális gépeket, a Server Explorer is elérhet.</span><span class="sxs-lookup"><span data-stu-id="12845-106">If you have virtual machines hosted by Azure, you can access them in Server Explorer.</span></span> <span data-ttu-id="12845-107">Először be kell jelentkeznie Azure-előfizetése a mobile services megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="12845-107">You must first sign in to your Azure subscription to view your mobile services.</span></span> <span data-ttu-id="12845-108">Jelentkezzen be, a Server Explorer nyissa meg a helyi menü az Azure csomóponthoz, és válassza **kapcsolódás a Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="12845-108">To sign in, open the shortcut menu for the Azure node in Server Explorer, and choose **Connect to Microsoft Azure**.</span></span>

### <a name="to-get-information-about-your-virtual-machines"></a><span data-ttu-id="12845-109">A virtuális gépek kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="12845-109">To get information about your virtual machines</span></span>
1. <span data-ttu-id="12845-110">A Server Explorer eszközben válassza ki a virtuális gépet, és válassza a Tulajdonságok ablak megjelenítése F4 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="12845-110">In Server Explorer, choose a virtual machine, and then choose the F4 key to show its properties window.</span></span>
   
    <span data-ttu-id="12845-111">Az alábbi táblázat bemutatja, milyen tulajdonságok érhetőek el, de összes csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="12845-111">The following table shows what properties are available, but they are all read-only.</span></span> <span data-ttu-id="12845-112">Ha módosítani szeretné azokat, használja a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="12845-112">To change them, use the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>
   
   | <span data-ttu-id="12845-113">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="12845-113">Property</span></span> | <span data-ttu-id="12845-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="12845-114">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="12845-115">DNS-név</span><span class="sxs-lookup"><span data-stu-id="12845-115">DNS Name</span></span> |<span data-ttu-id="12845-116">A virtuális gép internetcímét URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="12845-116">The URL with the Internet address of the virtual machine.</span></span> |
   | <span data-ttu-id="12845-117">Környezet</span><span class="sxs-lookup"><span data-stu-id="12845-117">Environment</span></span> |<span data-ttu-id="12845-118">A virtuális gép esetén ez a tulajdonság értéke mindig éles.</span><span class="sxs-lookup"><span data-stu-id="12845-118">For a virtual machine, the value of this property is always Production.</span></span> |
   | <span data-ttu-id="12845-119">Név</span><span class="sxs-lookup"><span data-stu-id="12845-119">Name</span></span> |<span data-ttu-id="12845-120">A virtuális gép neve.</span><span class="sxs-lookup"><span data-stu-id="12845-120">The name of the virtual machine.</span></span> |
   | <span data-ttu-id="12845-121">Méret</span><span class="sxs-lookup"><span data-stu-id="12845-121">Size</span></span> |<span data-ttu-id="12845-122">A virtuális gép, amely tükrözi a rendelkezésre álló szabad memória és lemezterület mennyisége mérete.</span><span class="sxs-lookup"><span data-stu-id="12845-122">The size of the virtual machine, which reflects the amount of memory and disk space that’s available.</span></span> <span data-ttu-id="12845-123">További információkért lásd: Útmutató: konfigurálja a virtuális gépek méretét.</span><span class="sxs-lookup"><span data-stu-id="12845-123">For more information, see How To: Configure Virtual Machine Sizes.</span></span> |
   | <span data-ttu-id="12845-124">status</span><span class="sxs-lookup"><span data-stu-id="12845-124">Status</span></span> |<span data-ttu-id="12845-125">Értékek: indítása, elindítva, leállítása, leállítva vagy Állapot lekérése során.</span><span class="sxs-lookup"><span data-stu-id="12845-125">Values include Starting, Started, Stopping, Stopped, and Retrieving Status.</span></span> <span data-ttu-id="12845-126">Ha beolvasása állapot jelenik meg, a jelenlegi állapot: ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="12845-126">If Retrieving Status appears, the current status is unknown.</span></span> <span data-ttu-id="12845-127">Ez a tulajdonság értékek eltérnek az értékeket, amelyeket a rendszer a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="12845-127">The values for this property differ from the values that are used on the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> |
   | <span data-ttu-id="12845-128">Előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="12845-128">SubscriptionID</span></span> |<span data-ttu-id="12845-129">Az Azure-fiók előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="12845-129">The subscription ID for your Azure account.</span></span> <span data-ttu-id="12845-130">Ezt az információt is megjeleníthetők a a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885) egy előfizetés tulajdonságai között megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="12845-130">You can show this information on the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) by viewing the properties for a subscription.</span></span> |
2. <span data-ttu-id="12845-131">Válasszon egy végpont csomópont, és nézze meg a **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="12845-131">Choose an endpoint node, and then view the **Properties** window.</span></span>
3. <span data-ttu-id="12845-132">A következő táblázat ismerteti a rendelkezésre álló tulajdonságok végpontok, de csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="12845-132">The following table describes the available properties of endpoints, but they are read-only.</span></span> <span data-ttu-id="12845-133">Adja hozzá vagy szerkesztheti a végpontokat a virtuális gép használja a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="12845-133">To add or edit the endpoints for a virtual machine, use the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> 
   
   | <span data-ttu-id="12845-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="12845-134">Property</span></span> | <span data-ttu-id="12845-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="12845-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="12845-136">Név</span><span class="sxs-lookup"><span data-stu-id="12845-136">Name</span></span> |<span data-ttu-id="12845-137">A végpont azonosítója.</span><span class="sxs-lookup"><span data-stu-id="12845-137">An identifier for the endpoint.</span></span> |
   | <span data-ttu-id="12845-138">Magánhálózati Port</span><span class="sxs-lookup"><span data-stu-id="12845-138">Private Port</span></span> |<span data-ttu-id="12845-139">A port, az alkalmazás belső hálózati hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="12845-139">The port for network access internal to your application.</span></span> |
   | <span data-ttu-id="12845-140">Protokoll</span><span class="sxs-lookup"><span data-stu-id="12845-140">Protocol</span></span> |<span data-ttu-id="12845-141">A protokoll által használt a szállítási réteg ezen a végponton, TCP vagy UDP.</span><span class="sxs-lookup"><span data-stu-id="12845-141">The protocol that the transport layer for this endpoint uses, either TCP or UDP.</span></span> |
   | <span data-ttu-id="12845-142">Nyilvános port</span><span class="sxs-lookup"><span data-stu-id="12845-142">Public Port</span></span> |<span data-ttu-id="12845-143">Az alkalmazás nyilvánosan elérhető használt port.</span><span class="sxs-lookup"><span data-stu-id="12845-143">The port that’s used for public access to your application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="12845-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12845-144">Next steps</span></span>
<span data-ttu-id="12845-145">A Visual Studio Azure szerepkörök használatával kapcsolatos további tudnivalókért lásd: [a távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="12845-145">To learn more about using Azure roles in Visual Studio, see [Using Remote Desktop with Azure Roles](vs-azure-tools-remote-desktop-roles.md).</span></span>

