---
title: "Azure virtuális gépek a Server Explorer aaaAccessing |} Microsoft Docs"
description: "Hogyan tooview létrehozása és kezelése az Azure virtuális gépek (VM) a Visual Studio Server Explorer áttekintést kaphat."
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
ms.openlocfilehash: f8326aed105a64ca558f766d712cc68701f82c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a><span data-ttu-id="e07fe-103">A Server Explorer Azure virtuális gépek elérése</span><span class="sxs-lookup"><span data-stu-id="e07fe-103">Accessing Azure Virtual Machines from Server Explorer</span></span>
<span data-ttu-id="e07fe-104">A Visual Studio Server Explorer használatával jelenítheti meg információkat az Azure által futtatott virtuális gépek kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e07fe-104">By using Server Explorer in Visual Studio, you can display information about your virtual machines hosted by Azure.</span></span>

## <a name="accessing-virtual-machines-in-server-explorer"></a><span data-ttu-id="e07fe-105">A Server Explorer virtuális gépek elérése</span><span class="sxs-lookup"><span data-stu-id="e07fe-105">Accessing virtual machines in Server Explorer</span></span>
<span data-ttu-id="e07fe-106">Ha az Azure-ban üzemeltetett virtuális gépeket, a Server Explorer is elérhet.</span><span class="sxs-lookup"><span data-stu-id="e07fe-106">If you have virtual machines hosted by Azure, you can access them in Server Explorer.</span></span> <span data-ttu-id="e07fe-107">Ön először be kell jelentkeznie az Azure-előfizetés tooview tooyour a mobile services.</span><span class="sxs-lookup"><span data-stu-id="e07fe-107">You must first sign in tooyour Azure subscription tooview your mobile services.</span></span> <span data-ttu-id="e07fe-108">toosign, nyissa meg a helyi menüje hello hello Azure csomópont a Server Explorer, és válassza a **tooMicrosoft Azure csatlakozás**.</span><span class="sxs-lookup"><span data-stu-id="e07fe-108">toosign in, open hello shortcut menu for hello Azure node in Server Explorer, and choose **Connect tooMicrosoft Azure**.</span></span>

### <a name="tooget-information-about-your-virtual-machines"></a><span data-ttu-id="e07fe-109">a virtuális gépek tooget információ</span><span class="sxs-lookup"><span data-stu-id="e07fe-109">tooget information about your virtual machines</span></span>
1. <span data-ttu-id="e07fe-110">A Server Explorer eszközben válassza ki a virtuális gépet, és a Tulajdonságok ablak hello F4 kulcs tooshow válassza.</span><span class="sxs-lookup"><span data-stu-id="e07fe-110">In Server Explorer, choose a virtual machine, and then choose hello F4 key tooshow its properties window.</span></span>
   
    <span data-ttu-id="e07fe-111">hello a következő táblázat bemutatja, milyen tulajdonságok érhetőek el, de összes csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="e07fe-111">hello following table shows what properties are available, but they are all read-only.</span></span> <span data-ttu-id="e07fe-112">toochange, használja a hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="e07fe-112">toochange them, use hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>
   
   | <span data-ttu-id="e07fe-113">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e07fe-113">Property</span></span> | <span data-ttu-id="e07fe-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="e07fe-114">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="e07fe-115">DNS-név</span><span class="sxs-lookup"><span data-stu-id="e07fe-115">DNS Name</span></span> |<span data-ttu-id="e07fe-116">hello URL-cím elé hello internetcím hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="e07fe-116">hello URL with hello Internet address of hello virtual machine.</span></span> |
   | <span data-ttu-id="e07fe-117">Környezet</span><span class="sxs-lookup"><span data-stu-id="e07fe-117">Environment</span></span> |<span data-ttu-id="e07fe-118">A virtuális gép hello Ez a tulajdonság értéke mindig éles.</span><span class="sxs-lookup"><span data-stu-id="e07fe-118">For a virtual machine, hello value of this property is always Production.</span></span> |
   | <span data-ttu-id="e07fe-119">Név</span><span class="sxs-lookup"><span data-stu-id="e07fe-119">Name</span></span> |<span data-ttu-id="e07fe-120">hello hello virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="e07fe-120">hello name of hello virtual machine.</span></span> |
   | <span data-ttu-id="e07fe-121">Méret</span><span class="sxs-lookup"><span data-stu-id="e07fe-121">Size</span></span> |<span data-ttu-id="e07fe-122">hello virtuális gép, amely tükrözi a rendelkezésre álló szabad memória és lemezterület mennyisége hello hello mérete.</span><span class="sxs-lookup"><span data-stu-id="e07fe-122">hello size of hello virtual machine, which reflects hello amount of memory and disk space that’s available.</span></span> <span data-ttu-id="e07fe-123">További információkért lásd: Útmutató: konfigurálja a virtuális gépek méretét.</span><span class="sxs-lookup"><span data-stu-id="e07fe-123">For more information, see How To: Configure Virtual Machine Sizes.</span></span> |
   | <span data-ttu-id="e07fe-124">status</span><span class="sxs-lookup"><span data-stu-id="e07fe-124">Status</span></span> |<span data-ttu-id="e07fe-125">Értékek: indítása, elindítva, leállítása, leállítva vagy Állapot lekérése során.</span><span class="sxs-lookup"><span data-stu-id="e07fe-125">Values include Starting, Started, Stopping, Stopped, and Retrieving Status.</span></span> <span data-ttu-id="e07fe-126">Ha beolvasása állapot jelenik meg, hello aktuális állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="e07fe-126">If Retrieving Status appears, hello current status is unknown.</span></span> <span data-ttu-id="e07fe-127">hello értékek ehhez a tulajdonsághoz eltérnek a hello használt hello értékek [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="e07fe-127">hello values for this property differ from hello values that are used on hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> |
   | <span data-ttu-id="e07fe-128">Előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="e07fe-128">SubscriptionID</span></span> |<span data-ttu-id="e07fe-129">hello előfizetési Azonosítóját az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="e07fe-129">hello subscription ID for your Azure account.</span></span> <span data-ttu-id="e07fe-130">Ezek az információk megjelenítése hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885) hello előfizetés tulajdonságai között egy megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="e07fe-130">You can show this information on hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) by viewing hello properties for a subscription.</span></span> |
2. <span data-ttu-id="e07fe-131">Válasszon egy végpont csomópont, és nézze meg hello **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="e07fe-131">Choose an endpoint node, and then view hello **Properties** window.</span></span>
3. <span data-ttu-id="e07fe-132">hello következő táblázat ismerteti hello rendelkezésre álló tulajdonságok végpontok, de csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="e07fe-132">hello following table describes hello available properties of endpoints, but they are read-only.</span></span> <span data-ttu-id="e07fe-133">a virtuális gép tooadd vagy Szerkesztés hello végpontok használata hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="e07fe-133">tooadd or edit hello endpoints for a virtual machine, use hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> 
   
   | <span data-ttu-id="e07fe-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e07fe-134">Property</span></span> | <span data-ttu-id="e07fe-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="e07fe-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="e07fe-136">Név</span><span class="sxs-lookup"><span data-stu-id="e07fe-136">Name</span></span> |<span data-ttu-id="e07fe-137">Hello végpont azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e07fe-137">An identifier for hello endpoint.</span></span> |
   | <span data-ttu-id="e07fe-138">Magánhálózati Port</span><span class="sxs-lookup"><span data-stu-id="e07fe-138">Private Port</span></span> |<span data-ttu-id="e07fe-139">hálózati hozzáférés belső tooyour alkalmazás hello port.</span><span class="sxs-lookup"><span data-stu-id="e07fe-139">hello port for network access internal tooyour application.</span></span> |
   | <span data-ttu-id="e07fe-140">Protokoll</span><span class="sxs-lookup"><span data-stu-id="e07fe-140">Protocol</span></span> |<span data-ttu-id="e07fe-141">hello protokoll, amely a szállítási réteg végpont hello használ, TCP vagy UDP.</span><span class="sxs-lookup"><span data-stu-id="e07fe-141">hello protocol that hello transport layer for this endpoint uses, either TCP or UDP.</span></span> |
   | <span data-ttu-id="e07fe-142">Nyilvános port</span><span class="sxs-lookup"><span data-stu-id="e07fe-142">Public Port</span></span> |<span data-ttu-id="e07fe-143">hello nyilvános hozzáférés tooyour alkalmazásához használt port.</span><span class="sxs-lookup"><span data-stu-id="e07fe-143">hello port that’s used for public access tooyour application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e07fe-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e07fe-144">Next steps</span></span>
<span data-ttu-id="e07fe-145">További információ az Azure szerepkörök a Visual Studióban, toolearn lásd [a távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="e07fe-145">toolearn more about using Azure roles in Visual Studio, see [Using Remote Desktop with Azure Roles](vs-azure-tools-remote-desktop-roles.md).</span></span>

