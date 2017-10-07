---
title: "a Windows PowerShell egy Azure felhőalapú szolgáltatásából aaaScale |} Microsoft Docs"
description: "(klasszikus) Megtudhatja, hogyan toouse PowerShell tooscale egy webes vagy feldolgozói szerepkör vagy az Azure-ban."
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
ms.openlocfilehash: cfac6660e84f8ae24e4e9bdd5bf2016fb9cd7045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-a-cloud-service-in-powershell"></a><span data-ttu-id="6adb9-103">Hogyan tooscale egy felhőalapú szolgáltatás a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="6adb9-103">How tooscale a cloud service in PowerShell</span></span>

<span data-ttu-id="6adb9-104">Használhatja a Windows PowerShell tooscale egy webes vagy feldolgozói szerepkör bejövő vagy kimenő hozzáadásával vagy eltávolításával példányok.</span><span class="sxs-lookup"><span data-stu-id="6adb9-104">You can use Windows PowerShell tooscale a web role or worker role in or out by adding or removing instances.</span></span>  

## <a name="log-in-tooazure"></a><span data-ttu-id="6adb9-105">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="6adb9-105">Log in tooAzure</span></span>

<span data-ttu-id="6adb9-106">Az előfizetés, a PowerShell segítségével bármilyen műveletet elvégzése előtt be kell jelentkeznie:</span><span class="sxs-lookup"><span data-stu-id="6adb9-106">Before you can perform any operations on your subscription through PowerShell, you must log in:</span></span>

```powershell
Add-AzureAccount
```

<span data-ttu-id="6adb9-107">Ha a fiókjához társított több előfizetéssel rendelkezik, szükség lehet attól függően, hogy a felhőszolgáltatás tartalmazó toochange hello előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="6adb9-107">If you have multiple subscriptions associated with your account, you may need toochange hello current subscription depending on where your cloud service resides.</span></span> <span data-ttu-id="6adb9-108">toocheck hello előfizetésben, futtassa:</span><span class="sxs-lookup"><span data-stu-id="6adb9-108">toocheck hello current subscription, run:</span></span>

```powershell
Get-AzureSubscription -Current
```

<span data-ttu-id="6adb9-109">Ha toochange hello előfizetésben kell futtatni:</span><span class="sxs-lookup"><span data-stu-id="6adb9-109">If you need toochange hello current subscription, run:</span></span>

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-hello-current-instance-count-for-your-role"></a><span data-ttu-id="6adb9-110">Ellenőrizze a szerepkör hello aktuális példányok száma</span><span class="sxs-lookup"><span data-stu-id="6adb9-110">Check hello current instance count for your role</span></span>

<span data-ttu-id="6adb9-111">toocheck hello jelenlegi állapotában a szerepkör futtatásához:</span><span class="sxs-lookup"><span data-stu-id="6adb9-111">toocheck hello current state of your role, run:</span></span>

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

<span data-ttu-id="6adb9-112">Kell visszakapnia hello szerepkör, beleértve az aktuális operációs rendszer verziója és a példány számoláshoz kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="6adb9-112">You should get back information about hello role, including its current OS version and instance count.</span></span> <span data-ttu-id="6adb9-113">Ebben az esetben a hello szerepkör egyetlen példányt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6adb9-113">In this case, hello role has a single instance.</span></span>

![Hello szerepkör kapcsolatos információk](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-hello-role-by-adding-more-instances"></a><span data-ttu-id="6adb9-115">Szerepkör horizontális felskálázása hello ismételt felvételével</span><span class="sxs-lookup"><span data-stu-id="6adb9-115">Scale out hello role by adding more instances</span></span>

<span data-ttu-id="6adb9-116">tooscale ki a szerepkör, pass hello szükségeskonfiguráció-példányok száma szerint hello **száma** paraméter toohello **Set-AzureRole** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="6adb9-116">tooscale out your role, pass hello desired number of instances as hello **Count** parameter toohello **Set-AzureRole** cmdlet:</span></span>

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

<span data-ttu-id="6adb9-117">hello parancsmag blokkok hello új példányok közben rövid ideig üzembe helyezve, és elindult.</span><span class="sxs-lookup"><span data-stu-id="6adb9-117">hello cmdlet blocks momentarily while hello new instances are provisioned and started.</span></span> <span data-ttu-id="6adb9-118">Ebben az időszakban, ha megnyit egy új PowerShell-ablakot, és hívás **Get-AzureRole** mint korábban, látni fogja a hello új cél példányok száma.</span><span class="sxs-lookup"><span data-stu-id="6adb9-118">During this time, if you open a new PowerShell window and call **Get-AzureRole** as shown earlier, you will see hello new target instance count.</span></span> <span data-ttu-id="6adb9-119">És hello szerepkör állapota hello portálon vizsgálja meg, ha megjelenítheti hello új példány indítása:</span><span class="sxs-lookup"><span data-stu-id="6adb9-119">And if you inspect hello role status in hello portal, you should see hello new instance starting up:</span></span>

![Virtuálisgép-példány a portál indítása](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

<span data-ttu-id="6adb9-121">Miután elindította hello új példányok, hello parancsmag sikeresen vissza:</span><span class="sxs-lookup"><span data-stu-id="6adb9-121">Once hello new instances have started, hello cmdlet will return successfully:</span></span>

![Szerepkör példány növekedése sikeres](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-hello-role-by-removing-instances"></a><span data-ttu-id="6adb9-123">Bővítse a hello szerepkörben példányok eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6adb9-123">Scale in hello role by removing instances</span></span>

<span data-ttu-id="6adb9-124">Méretezheti szerepkör, amely eltávolítja példányok hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="6adb9-124">You can scale in a role by removing instances in hello same way.</span></span> <span data-ttu-id="6adb9-125">Set hello **száma** paraméter **Set-AzureRole** toohello szám példányok kívánt toohave hello méretezési művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="6adb9-125">Set hello **Count** parameter on **Set-AzureRole** toohello number of instances you want toohave after hello scale in operation is complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6adb9-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6adb9-126">Next steps</span></span>

<span data-ttu-id="6adb9-127">Már nem lehetséges tooconfigure automatikus méretezése a PowerShell-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6adb9-127">It is not possible tooconfigure auto-scale for cloud services from PowerShell.</span></span> <span data-ttu-id="6adb9-128">által látható toodo [hogyan tooauto a felhőalapú szolgáltatások méretezéséhez](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6adb9-128">toodo that, see [How tooauto scale a cloud service](cloud-services-how-to-scale-portal.md).</span></span>
