---
title: "egy Azure aaaDelete fürt és az erőforrások |} Microsoft Docs"
description: "Ismerje meg, hogyan toocompletely delete a Service Fabric-fürt vagy törlése hello tartalmazó csoport hello fürt, vagy szelektív módon törli az erőforrásokat."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 5c15a4184644da715cd69397f2150de86ab433ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-hello-resources-it-uses"></a>Az Azure és a hello erőforrás használja a Service Fabric-fürt törlése
A Service Fabric-fürt épül fel sok más Azure-erőforrások továbbá toohello fürterőforrás magát. Így toocompletely a Service Fabric-fürt törlése szükség toodelete összes hello történik, az erőforrásokat.
Két lehetőség közül választhat: vagy delete hello erőforráscsoport, amely a fürt hello van (amely törli a hello fürterőforrás és más erőforrások hello erőforráscsoportban) vagy a kifejezetten a hello fürterőforrás törlése és a hozzá társított erőforrásokat (de nem más erőforrások az erőforráscsoportban hello).

> [!NOTE]
> Hello fürterőforrás törlése **nem** összes hello más erőforrások, amelyek tevődnek össze a Service Fabric-fürt törlése.
> 
> 

## <a name="delete-hello-entire-resource-group-rg-that-hello-service-fabric-cluster-is-in"></a>Hello teljes erőforráscsoport (RG), amely a Service Fabric lévő hello törlése
Ez a hello, hogy törli a fürtöt, vele együtt hello erőforráscsoporthoz társított összes hello erőforrás legegyszerűbb módja tooensure. Hello erőforráscsoport PowerShell használatával törölheti vagy hello Azure-portálon keresztül. Ha az erőforráscsoport erőforráshoz kapcsolódó tooService fabric-fürt, majd törölheti egy adott erőforráshoz.

### <a name="delete-hello-resource-group-using-azure-powershell"></a>Az Azure PowerShell hello erőforráscsoport törlése
Hello erőforráscsoport hello Azure PowerShell-parancsmagok a következő futtatásával is törli. Győződjön meg arról, hogy Azure PowerShell 1.0 vagy nagyobb telepítve van a számítógépen. Ha nem ezt megelőzően, lépésekkel hello leírt [hogyan tooinstall és az Azure PowerShell konfigurálása.](/powershell/azure/overview)

Nyisson meg egy PowerShell-ablakot, és futtassa a következő PS parancsmagok hello:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

A kérdés tooconfirm hello törlés fog kapni, ha nem használja hello *-Force* lehetőséget. A megerősítő hello RG, és a benne található összes hello erőforrások törlése.

### <a name="delete-a-resource-group-in-hello-azure-portal"></a>Az Azure-portálon hello erőforrás csoport törlése
1. Bejelentkezési toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a kívánt toodelete toohello Service Fabric-fürt.
3. Kattintson a hello hello fürt essentials oldalon erőforráscsoport neve.
4. Ekkor megjelenik a hello **erőforrás csoport Essentials** lap.
5. Kattintson a **Törlés** gombra.
6. Hello utasításokat kövesse, hogy hello erőforráscsoport lap toocomplete hello törlése.

![Erőforrás-csoport törlése][ResourceGroupDelete]

## <a name="delete-hello-cluster-resource-and-hello-resources-it-uses-but-not-other-resources-in-hello-resource-group"></a>Hello fürterőforrás és hello erőforrásokat használ, de nem hello erőforráscsoport egyéb erőforrások törlése
Ha az erőforráscsoport csak olyan erőforrásokat, amelyek a Service Fabric-fürt kapcsolódó toohello toodelete szeretne, majd könnyebb toodelete hello teljes erőforráscsoport. Ha tooselectively delete hello erőforrások egyenként az erőforráscsoportban, majd kövesse az alábbi lépéseket.

Ha telepítette a fürt hello portál használatával, vagy valamelyik hello Service Fabric Resource Manager-sablonok segítségével hello sablon gyűjteményből, fürt által használt hello hello erőforrások címkéjű vannak a következő két címkék hello. Használhatja őket erőforrások kívánt toodecide toodelete.

***1. tag:*** kulcs = fürtnév, érték = "hello fürt neve"

***2. tag:*** kulcsot resourceName, érték = = ServiceFabric

### <a name="delete-specific-resources-in-hello-azure-portal"></a>Egy adott erőforráshoz, az Azure-portálon hello törlése
1. Bejelentkezési toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a kívánt toodelete toohello Service Fabric-fürt.
3. Nyissa meg túl**összes beállítás** hello essentials panelen.
4. Kattintson a **címkék** alatt **erőforrás-kezelés** hello-beállítások panelen.
5. Kattintson az egyik hello **címkék** a hello Címkék panel tooget az adott címkével ellátott összes hello erőforrások listáját.
   
    ![Erőforráscímkék][ResourceTags]
6. Miután címkézett erőforrások hello listája, kattintson a hello erőforrások mindegyikének, és törölje őket.
   
    ![Címkézett erőforrások][TaggedResources]

### <a name="delete-hello-resources-using-azure-powershell"></a>Az Azure PowerShell hello erőforrások törlése
Hello erőforrások egyenként-hello Azure PowerShell-parancsmagok a következő futtatásával törölheti. Győződjön meg arról, hogy Azure PowerShell 1.0 vagy nagyobb telepítve van a számítógépen. Ha nem ezt megelőzően, lépésekkel hello leírt [hogyan tooinstall és az Azure PowerShell konfigurálása.](/powershell/azure/overview)

Nyisson meg egy PowerShell-ablakot, és futtassa a következő PS parancsmagok hello:

```powershell
Login-AzureRmAccount
```
Az egyes hello erőforrások szeretné, hogy toodelete, futtassa a következő hello:

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of hello resource group>" -Force
```

toodelete hello fürterőforrás, futtassa a következő hello:

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of hello resource group>" -Force
```

## <a name="next-steps"></a>Következő lépések
A következő tooalso olvasási hello fürt frissítése és a szolgáltatások particionálás megismerése:

* [További tudnivalók a fürt frissítése](service-fabric-cluster-upgrade.md)
* [További tudnivalók a maximális méret állapotalapú szolgáltatások particionálás](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
