---
title: "aaaDefine gyermek erőforrás az Azure-sablon alapján |} Microsoft Docs"
description: "Bemutatja, hogyan tooset hello erőforrástípus és az Azure Resource Manager-sablon a gyermek-erőforrás nevét"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: c502c589100d7ae864d7fb01b5ba10ddfaf92592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>A Resource Manager-sablon nevét és típusát gyermek erőforrás beállítása
Sablon létrehozásakor gyakran kell tooinclude kapcsolódó tooa szülő erőforrás gyermek erőforrásra. A sablon tartalmazhat például egy SQL server és adatbázis. hello SQL server hello szülő erőforrás, hello adatbázis pedig hello gyermek-erőforrás. 

hello hello gyermek erőforrástípus formátuma:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

hello hello gyermek erőforrás nevének formátuma:`{parent-resource-name}/{child-resource-name}`

Azonban adnia hello típusa és neve a sablon a menübeállításoktól függően e beágyazott hello szülő erőforrás belül, vagy a saját hello felső szintjén. Ez a témakör bemutatja, hogyan mindkét toohandle közelít.

Egy teljesen minősített hivatkozás tooa erőforrás térített hello rendelés toocombine szegmensek hello típusból név pedig nem egyszerűen hello két összefűzése.  Hello névtér után inkább sorozatát *típusnév/* párok a legkevésbé egyedi toomost adott:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Példa:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`megfelelő `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` helytelen

## <a name="nested-child-resource"></a>Beágyazott gyermektevékenységének erőforrás
hello legegyszerűbb módja toodefine gyermek erőforrás toonest rá hello szülő erőforrás. a következő példa azt mutatja meg, egy SQL Server ágyazva egy SQL-adatbázis hello.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Hello gyermek erőforrás, akkor hello típusának beállítása túl`databases` , de a teljes erőforrás típusa `Microsoft.Sql/servers/databases`. Nincs megadva `Microsoft.Sql/servers/` mivel feltételezzük hello szülő erőforrás típusból. hello gyermek-erőforrás neve túl van-e állítva`exampledatabase` , de hello teljes nevét hello szülő nevének tartalmazza. Nincs megadva `exampleserver` mivel feltételezzük hello szülő erőforrás.

## <a name="top-level-child-resource"></a>Legfelső szintű gyermek-erőforrás
Gyermek-erőforrás hello hello felső szintjén adhat meg. Előfordulhat, hogy ezt a módszert használja, ha az hello szülő erőforrás nem hello azonos sablont, vagy ha szeretné, hogy toouse `copy` toocreate több gyermek-erőforrás. Ezt a módszert hello teljes erőforrástípust biztosít, és tartalmazza a hello szülő erőforrás nevét az hello gyermek-erőforrás neve.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

hello adatbázisa egy gyermek-erőforrás toohello kiszolgáló annak ellenére, hogy az azonos szinten hello sablonban hello vannak definiálva.

## <a name="next-steps"></a>Következő lépések
* A fenyegetés elhárítására vonatkozó javaslatokról toocreate sablonjainak használatáról [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](resource-manager-template-best-practices.md).
* Például több gyermek erőforrások létrehozásának, [erőforrások az Azure Resource Manager sablonokban több példányának telepítése](resource-group-create-multiple.md).
