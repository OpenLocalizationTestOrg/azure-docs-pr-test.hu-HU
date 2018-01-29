---
title: "Az Azure szülő erőforrás hibák |} Microsoft Docs"
description: "Javítsa a hibákat, az a szülő erőforrás használatakor ismerteti."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: e59147c4ac18f730f27b9d4aa9c008f219881065
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-parent-resources"></a>Javítsa a hibákat a szülő erőforrások

Ez a cikk ismerteti a erőforrástól függ a szülő erőforrás telepítése során felmerülő hibákat.

## <a name="symptom"></a>Jelenség

A erőforrása, amely egy másik erőforrásra való gyermeke való telepítésekor a következő hibaüzenet jelenhet meg:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>Ok

Amikor egy erőforrást egy másik erőforrásra való gyermek, a szülő erőforrás léteznie kell a gyermek-erőforrás létrehozása előtt. A gyermek-erőforrás nevét tartalmazza a szülő nevének. Például egy SQL-adatbázis előfordulhat, hogy meghatározása:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

De ha egy függőséget meg a kiszolgálón, az adatbázis telepítése előfordulhat, hogy indítsa el a kiszolgáló rendelkezik központi telepítése előtt.

## <a name="solution"></a>Megoldás

Ez a hiba elhárításához függőség tartalmazza.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

További információkért lásd: [határozza meg a ahhoz, hogy az Azure Resource Manager sablonokban erőforrásokat üzembe helyezi](resource-group-define-dependencies.md).