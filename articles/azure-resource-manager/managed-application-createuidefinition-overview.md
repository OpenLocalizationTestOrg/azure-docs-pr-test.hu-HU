---
title: "az Azure által felügyelt alkalmazások felhasználói felületi definíciójának létrehozása aaaUnderstand |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate felhasználói felület definíciókat az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: d53ddf438c24d5a6cb8dd53ca0b4694ab0462515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-createuidefinition"></a>Ismerkedés a CreateUiDefinition
Ez a dokumentum egy CreateUiDefinition, hello Azure portál toogenerate hello felhasználói felület által kezelt alkalmazás létrehozásához használt alapvető fogalmait hello vezet be.

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
* paraméterek

Kezelt alkalmazások, mindig kell kezelő `Microsoft.Compute.MultiVm`, és a legújabb támogatott hello verziója `0.1.2-preview`.

hello paraméterek tulajdonság sémájának hello hello megadott kezelő és a verzió hello kombinációja függ. Kezelt alkalmazások hello támogatott tulajdonságok: `basics`, `steps`, és `outputs`. hello alapjait és lépéseket tulajdonságok tartalmaznak hello _elemek_ – például a szövegmezők és legördülő listák megnyílásának - toobe megjelenő hello Azure-portálon. hello kimenetek tulajdonság használt toomap hello kimeneti értékeit hello megadott elemek toohello paraméterei hello Azure Resource Manager központi telepítési sablont.

Beleértve `$schema` javasolt, de nem kötelező. Ha meg van adva, a következő hello `version` meg kell egyeznie a hello verzió belül hello `$schema` URI.

## <a name="basics"></a>Alapvető beállítások
hello alapjai lépés mindig hello jönnek létre, ha az Azure-portálon hello elemzi a CreateUiDefinition hello varázsló első lépése. Ezenkívül toodisplaying hello elemek megadott `basics`, hello portál esetében elemei felhasználók toochoose hello előfizetés, erőforráscsoport és hello telepítési helyét. Általában lekérdezése a központi telepítés kiterjedő paraméterek hello nevét egy fürt vagy a rendszergazdai hitelesítő adatokat, például elemeket kell nyissa meg az ebben a lépésben.

Ha egy elem viselkedés attól függ, hello felhasználói előfizetés, erőforráscsoporthoz vagy helyre, majd, hogy elem nem használható az alapokat. Például **Microsoft.Compute.SizeSelector** hello felhasználói előfizetésben és helyen toodetermine hello listájában elérhető méretek függ. Ezért **Microsoft.Compute.SizeSelector** csak akkor használható a lépéseket. Általában csak elemek hello **Microsoft.Common** névtér alapokat is használható. Bár bizonyos elemek más névtérben (például **Microsoft.Compute.Credentials**), amely nem függ a hello felhasználói környezet, továbbra is engedélyezett.

## <a name="steps"></a>Lépések
hello lépéseket tulajdonság nulla vagy több további lépést toodisplay után alapjai, egy vagy több elemet tartalmaz, amelyek mindegyike tartalmazhat. Fontolja meg egy szerepkör vagy a réteg hello alkalmazás telepítése lépést. Például hozzáadhat egy bemenetek hello fő csomóponthoz, és egy lépést hello ezen csomópontokhoz tartozó fürtben.

## <a name="outputs"></a>kimenetek
hello Azure-portált használja hello `outputs` tulajdonság toomap elemek `basics` és `steps` toohello paraméterei hello Azure Resource Manager központi telepítési sablont. a szótár hello kulcsok hello hello sablon paraméterek nevei, és hello értékei származó hivatkozott hello hello kimeneti objektumok tulajdonságai.

## <a name="functions"></a>Functions
Hasonló túl[sablonfüggvényei](resource-group-template-functions.md) az Azure Resource Manager (mind a szintaxist és a funkció), CreateUiDefinition elem bemenetekhez és kimenetekhez való munkához funkciókat biztosít, valamint szolgáltatások – pl. conditionals.

## <a name="next-steps"></a>Következő lépések
CreateUiDefinition önmagában egy egyszerű sémával rendelkezik. hello valós mélysége azt minden hello támogatott elemek és funkciók, mely a következő dokumentumok hello wondrous részletesen leírja származnak:

- [Elemek](managed-application-createuidefinition-elements.md)
- [Functions](managed-application-createuidefinition-functions.md)

A jelenlegi JSON-séma CreateUiDefinition érhető el itt: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json. 

Későbbi verzióiban lesz elérhető legyen a következőn hello ugyanazon a helyen. Cserélje le a hello `0.1.2-preview` hello URL-cím és hello része `version` érték hello verzió azonosítójú toouse szeretné. hello támogatott verzió azonosítók `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, és `0.1.2-preview`.
