---
title: "Azure-felhőszolgáltatás méretezése a Windows PowerShell |} Microsoft Docs"
description: "(klasszikus) Megtudhatja, hogyan lehet egy webes vagy feldolgozói szerepkör bejövő vagy kimenő skálázása az Azure PowerShell használatával."
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: mmccrory
ms.openlocfilehash: a7ae8ff202d403dff19b8c9a6a09492235db27ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a><span data-ttu-id="89c04-103">Egy felhőalapú szolgáltatás méretezése a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="89c04-103">How to scale a cloud service in PowerShell</span></span>

<span data-ttu-id="89c04-104">A Windows PowerShell segítségével egy webes vagy feldolgozói szerepkör bejövő vagy kimenő hozzáadásával vagy eltávolításával a példány méretezése.</span><span class="sxs-lookup"><span data-stu-id="89c04-104">You can use Windows PowerShell to scale a web role or worker role in or out by adding or removing instances.</span></span>  

## <a name="log-in-to-azure"></a><span data-ttu-id="89c04-105">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="89c04-105">Log in to Azure</span></span>

<span data-ttu-id="89c04-106">Az előfizetés, a PowerShell segítségével bármilyen műveletet elvégzése előtt be kell jelentkeznie:</span><span class="sxs-lookup"><span data-stu-id="89c04-106">Before you can perform any operations on your subscription through PowerShell, you must log in:</span></span>

```powershell
Add-AzureAccount
```

<span data-ttu-id="89c04-107">Ha a fiókjához társított több előfizetéssel rendelkezik, szükség lehet attól függően, hogy hol helyezkedik el a felhőalapú szolgáltatás jelenlegi módosítása.</span><span class="sxs-lookup"><span data-stu-id="89c04-107">If you have multiple subscriptions associated with your account, you may need to change the current subscription depending on where your cloud service resides.</span></span> <span data-ttu-id="89c04-108">A jelenlegi előfizetés ellenőrzéséhez futtassa:</span><span class="sxs-lookup"><span data-stu-id="89c04-108">To check the current subscription, run:</span></span>

```powershell
Get-AzureSubscription -Current
```

<span data-ttu-id="89c04-109">Ha módosítania kell a jelenlegi előfizetés, futtassa:</span><span class="sxs-lookup"><span data-stu-id="89c04-109">If you need to change the current subscription, run:</span></span>

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a><span data-ttu-id="89c04-110">Ellenőrizze a szerepkör az aktuális példányszám</span><span class="sxs-lookup"><span data-stu-id="89c04-110">Check the current instance count for your role</span></span>

<span data-ttu-id="89c04-111">A szerepkör aktuális állapotának ellenőrzéséhez futtassa:</span><span class="sxs-lookup"><span data-stu-id="89c04-111">To check the current state of your role, run:</span></span>

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

<span data-ttu-id="89c04-112">Kell visszakapnia információt a szerepkör, beleértve az aktuális operációs rendszer verziója és a példány száma.</span><span class="sxs-lookup"><span data-stu-id="89c04-112">You should get back information about the role, including its current OS version and instance count.</span></span> <span data-ttu-id="89c04-113">Ebben az esetben a szerepkörhöz egyetlen példányt.</span><span class="sxs-lookup"><span data-stu-id="89c04-113">In this case, the role has a single instance.</span></span>

![A szerepkör kapcsolatos információk](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a><span data-ttu-id="89c04-115">A szerepkör horizontális felskálázása ismételt felvételével</span><span class="sxs-lookup"><span data-stu-id="89c04-115">Scale out the role by adding more instances</span></span>

<span data-ttu-id="89c04-116">A szerepkör horizontális, továbbítja a példányok, a kívánt számát a **száma** paramétert a **Set-AzureRole** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="89c04-116">To scale out your role, pass the desired number of instances as the **Count** parameter to the **Set-AzureRole** cmdlet:</span></span>

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

<span data-ttu-id="89c04-117">A parancsmag blokkolja az új példányok közben rövid ideig üzembe helyezve, és elindult.</span><span class="sxs-lookup"><span data-stu-id="89c04-117">The cmdlet blocks momentarily while the new instances are provisioned and started.</span></span> <span data-ttu-id="89c04-118">Ebben az időszakban, ha megnyit egy új PowerShell-ablakot, és hívás **Get-AzureRole** mint korábban, megjelenik az új cél-példányok száma.</span><span class="sxs-lookup"><span data-stu-id="89c04-118">During this time, if you open a new PowerShell window and call **Get-AzureRole** as shown earlier, you will see the new target instance count.</span></span> <span data-ttu-id="89c04-119">És a szerepkör állapota a portálon vizsgálja meg, ha az új példány elindítása láthatja:</span><span class="sxs-lookup"><span data-stu-id="89c04-119">And if you inspect the role status in the portal, you should see the new instance starting up:</span></span>

![Virtuálisgép-példány a portál indítása](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

<span data-ttu-id="89c04-121">Miután az új példány indult el, a parancsmag sikeresen vissza:</span><span class="sxs-lookup"><span data-stu-id="89c04-121">Once the new instances have started, the cmdlet will return successfully:</span></span>

![Szerepkör példány növekedése sikeres](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a><span data-ttu-id="89c04-123">Bővítse a szerepkör a példányok eltávolítása</span><span class="sxs-lookup"><span data-stu-id="89c04-123">Scale in the role by removing instances</span></span>

<span data-ttu-id="89c04-124">Példányok ugyanúgy eltávolításával szerepkör méretezheti.</span><span class="sxs-lookup"><span data-stu-id="89c04-124">You can scale in a role by removing instances in the same way.</span></span> <span data-ttu-id="89c04-125">Állítsa be a **száma** paraméter **Set-AzureRole** kíván használni a méretezési művelet befejeződése után példányok száma.</span><span class="sxs-lookup"><span data-stu-id="89c04-125">Set the **Count** parameter on **Set-AzureRole** to the number of instances you want to have after the scale in operation is complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89c04-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89c04-126">Next steps</span></span>

<span data-ttu-id="89c04-127">Nincs automatikus méretezése a powershellből felhőszolgáltatások konfigurálása lehetséges.</span><span class="sxs-lookup"><span data-stu-id="89c04-127">It is not possible to configure auto-scale for cloud services from PowerShell.</span></span> <span data-ttu-id="89c04-128">Ehhez tekintse meg a [automatikus méretezési egy felhőalapú szolgáltatás hogyan](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="89c04-128">To do that, see [How to auto scale a cloud service](cloud-services-how-to-scale-portal.md).</span></span>
