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
# <a name="how-tooscale-a-cloud-service-in-powershell"></a>Hogyan tooscale egy felhőalapú szolgáltatás a PowerShellben

Használhatja a Windows PowerShell tooscale egy webes vagy feldolgozói szerepkör bejövő vagy kimenő hozzáadásával vagy eltávolításával példányok.  

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Az előfizetés, a PowerShell segítségével bármilyen műveletet elvégzése előtt be kell jelentkeznie:

```powershell
Add-AzureAccount
```

Ha a fiókjához társított több előfizetéssel rendelkezik, szükség lehet attól függően, hogy a felhőszolgáltatás tartalmazó toochange hello előfizetésben. toocheck hello előfizetésben, futtassa:

```powershell
Get-AzureSubscription -Current
```

Ha toochange hello előfizetésben kell futtatni:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-hello-current-instance-count-for-your-role"></a>Ellenőrizze a szerepkör hello aktuális példányok száma

toocheck hello jelenlegi állapotában a szerepkör futtatásához:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Kell visszakapnia hello szerepkör, beleértve az aktuális operációs rendszer verziója és a példány számoláshoz kapcsolatos információkat. Ebben az esetben a hello szerepkör egyetlen példányt rendelkezik.

![Hello szerepkör kapcsolatos információk](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-hello-role-by-adding-more-instances"></a>Szerepkör horizontális felskálázása hello ismételt felvételével

tooscale ki a szerepkör, pass hello szükségeskonfiguráció-példányok száma szerint hello **száma** paraméter toohello **Set-AzureRole** parancsmagot:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

hello parancsmag blokkok hello új példányok közben rövid ideig üzembe helyezve, és elindult. Ebben az időszakban, ha megnyit egy új PowerShell-ablakot, és hívás **Get-AzureRole** mint korábban, látni fogja a hello új cél példányok száma. És hello szerepkör állapota hello portálon vizsgálja meg, ha megjelenítheti hello új példány indítása:

![Virtuálisgép-példány a portál indítása](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Miután elindította hello új példányok, hello parancsmag sikeresen vissza:

![Szerepkör példány növekedése sikeres](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-hello-role-by-removing-instances"></a>Bővítse a hello szerepkörben példányok eltávolítása

Méretezheti szerepkör, amely eltávolítja példányok hello azonos módon. Set hello **száma** paraméter **Set-AzureRole** toohello szám példányok kívánt toohave hello méretezési művelet befejeződése után.

## <a name="next-steps"></a>Következő lépések

Már nem lehetséges tooconfigure automatikus méretezése a PowerShell-szolgáltatásokhoz. által látható toodo [hogyan tooauto a felhőalapú szolgáltatások méretezéséhez](cloud-services-how-to-scale-portal.md).
