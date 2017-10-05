---
title: "Létrehozása felhasználói felületének meghatározása az Azure által felügyelt alkalmazások megismerése |} Microsoft Docs"
description: "Ismerteti, hogyan lehet az Azure által felügyelt alkalmazások felhasználói felületi-meghatározások létrehozása"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 176b891538f85c5638a2321561c3d8bd377d245b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-createuidefinition"></a>Ismerkedés a CreateUiDefinition
Ez a dokumentum bemutatja a core CreateUiDefinition, amely az Azure-portálon kezelt alkalmazás létrehozásához a felhasználói felület létrehozásához használja.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

Egy CreateUiDefinition mindig három tulajdonságot tartalmazza: 

* eseménykezelő
* Verzió
* Paraméterek

Kezelt alkalmazások, mindig kell kezelő `Microsoft.Compute.MultiVm`, és a legfrissebb támogatott verziót `0.1.2-preview`.

A paraméterek tulajdonság sémájának attól függ, hogy a megadott kezelő és verziót. Kezelt alkalmazások, a támogatott tulajdonságok: `basics`, `steps`, és `outputs`. A alapjait és lépéseket tulajdonságok tartalmaznak a _elemek_ - például a szövegmezőből és a legördülő lista - az Azure portálon megjelenítendő. A kimenetek tulajdonság a megadott elemek kimeneti értékeit hozzárendelése az Azure Resource Manager központi telepítési sablon paramétereinek szolgál.

Beleértve `$schema` javasolt, de nem kötelező. Ha meg van adva, a következő `version` meg kell egyeznie a verzió belül a `$schema` URI.

## <a name="basics"></a>Alapvető beállítások
Az alapok lépés mindig az első lépés a varázsló jönnek létre, ha az Azure-portálon elemzi a CreateUiDefinition. A megadott elemek megjelenítése mellett `basics`, a portálon a felhasználók számára előfizetés, erőforráscsoport és a telepítési hely kiválasztása elemek esetében. Általában lekérdezése a központi telepítés kiterjedő paraméterek, például egy fürt vagy a rendszergazdai hitelesítő adatokat, nevét elemeket kell nyissa meg az ebben a lépésben.

Ha egy elem viselkedés attól függ, a felhasználó előfizetés, erőforráscsoportból vagy helyét, majd, hogy elem nem használható az alapokat. Például **Microsoft.Compute.SizeSelector** attól függ, a felhasználó előfizetésben és helyen meghatározni az elérhető méretek listáját. Ezért **Microsoft.Compute.SizeSelector** csak akkor használható a lépéseket. Általában csak elemeinek a **Microsoft.Common** névtér alapokat is használható. Bár bizonyos elemek más névtérben (például **Microsoft.Compute.Credentials**), amely nem függ a felhasználói környezet, továbbra is engedélyezett.

## <a name="steps"></a>Lépések
A lépések tulajdonság nulla vagy több további lépést is végre megjelenítése után alapjai, egy vagy több elemet tartalmaz, amelyek mindegyike tartalmazhat. Fontolja meg egy szerepkör vagy a réteg az alkalmazás telepítése lépést. Például hozzáadhat egy bemenetek a fő csomóponthoz, és egy lépést az ezen csomópontokhoz tartozó fürtben.

## <a name="outputs"></a>kimenetek
Az Azure-portálon használja a `outputs` tulajdonság elemeket a `basics` és `steps` paramétereinek az Azure Resource Manager központi telepítési sablont. Ehhez a szótárhoz kulcsai a sablon-paraméterek nevei, és a kimeneti objektumokat a hivatkozott elemeket tulajdonságainak értékei.

## <a name="functions"></a>Functions
Hasonló [sablonfüggvényei](resource-group-template-functions.md) az Azure Resource Manager (mind a szintaxist és a funkció), CreateUiDefinition elem bemenetekhez és kimenetekhez való munkához funkciókat biztosít, valamint szolgáltatások – pl. conditionals.

## <a name="next-steps"></a>Következő lépések
CreateUiDefinition önmagában egy egyszerű sémával rendelkezik. A valódi mélysége származik támogatott elemeket és függvények, amelyek wondrous részletesen leírja a következő dokumentumokat:

- [Elemek](managed-application-createuidefinition-elements.md)
- [Functions](managed-application-createuidefinition-functions.md)

A jelenlegi JSON-séma CreateUiDefinition érhető el itt: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json. 

Ugyanazon a helyen későbbi verzióiban lesz elérhető. Cserélje le a `0.1.2-preview` az URL-cím része, és a `version` a használni kívánt azonosító értéket. A jelenleg támogatott verziójú azonosítók `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, és `0.1.2-preview`.