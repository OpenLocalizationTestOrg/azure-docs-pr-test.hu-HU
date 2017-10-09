---
title: "aaaAzure felügyelt alkalmazás VirtualNetworkCombo felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 1b0fa5360d93306f7a814723f77e42540bdaaa9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem
Egy új vagy meglévő virtuális hálózat kiválasztásához vezérlők egy csoportja. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- Hello felső Drótvázdiagram, a hello felhasználói kiválasztotta az új virtuális hálózat, így hello felhasználói testre szabhatja az egyes alhálózatok nevét és címét előtag. Alhálózatok konfigurálása ebben az esetben nem kötelező megadni.
- A hello alsó Drótvázdiagram hello felhasználói kiválasztotta a meglévő virtuális hálózat, a hello felhasználói kell társítani, minden alhálózati hello központi telepítési sablont megköveteli-e a tooan létező alhálózatot. Alhálózatok konfigurálása ebben az esetben szükség.

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- Ha meg van adva, hello első mozaikként, átfedés nélkül címelőtag méretű `defaultValue.addressPrefixSize` hello felhasználó az előfizetéshez létező virtuális hálózatok automatikusan alapján történik.
- az alapértelmezett érték hello `defaultValue.name` és `defaultValue.addressPrefixSize` van **null**.
- `constraints.minAddressPrefixSize`meg kell adni. Az-címtér kisebb, mint hello megadott érték nem lehet kiválasztani a meglévő virtuális hálózatok.
- `subnets`meg kell adni, és `constraints.minAddressPrefixSize` meg kell adni az egyes alhálózatokon.
- Amikor új virtuális hálózat létrehozása minden alhálózati cím előtag alapján van kiszámítva automatikusan hello virtuális hálózat címelőtagjához és a megfelelő `addressPrefixSize`.
- Ha egy meglévő virtuális hálózat, kisebb, mint a megfelelő alhálózatok `constraints.minAddressPrefixSize` kijelölés nem érhetők el. Továbbá ha meg van adva, alhálózatok, amelyek nem tartalmaznak legalább `minAddressCount` elérhető címei nem lehet kiválasztani.
hello alapértelmezett értéke **0**. amely a rendelkezésre álló címek hello tooensure összefüggő, adja meg **igaz** a `requireContiguousAddresses`. hello alapértelmezett értéke **igaz**.
- A meglévő virtuális hálózat alhálózatok létrehozása nem támogatott.
- Ha `options.hideExisting` van **igaz**, hello felhasználói egy meglévő virtuális hálózat nem választható. hello alapértelmezett értéke **hamis**.

## <a name="sample-output"></a>Példa kimenet
```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).
