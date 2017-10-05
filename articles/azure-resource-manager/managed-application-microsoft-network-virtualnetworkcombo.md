---
title: "Az Azure által felügyelt alkalmazás VirtualNetworkCombo felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 8bb255b76ac5c3de570fa569a1cfb3ee953f9687
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem
Egy új vagy meglévő virtuális hálózat kiválasztásához vezérlők egy csoportja. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- A felső Drótvázdiagram, az a felhasználó kiválasztotta egy új virtuális hálózat, a felhasználó minden alhálózat nevét és címét előtag szabhatja testre. Alhálózatok konfigurálása ebben az esetben nem kötelező megadni.
- Az alsó Drótvázdiagram a felhasználó kiválasztotta meglévő virtuális hálózat, így a felhasználó a központi telepítési sablont igényel minden egyes alhálózatot kell társítani egy létező alhálózatot. Alhálózatok konfigurálása ebben az esetben szükség.

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
- Ha meg van adva, az első mozaikként, átfedés nélkül cím előtag méretű `defaultValue.addressPrefixSize` a felhasználó az előfizetéshez létező virtuális hálózatok automatikusan alapján történik.
- Az alapértelmezett érték `defaultValue.name` és `defaultValue.addressPrefixSize` van **null**.
- `constraints.minAddressPrefixSize`meg kell adni. Meglévő virtuális hálózatok egy kisebb, mint a megadott érték címtartománnyal rendelkező kijelölésnél érhetők el.
- `subnets`meg kell adni, és `constraints.minAddressPrefixSize` meg kell adni az egyes alhálózatokon.
- Amikor új virtuális hálózat létrehozása minden alhálózati cím előtag alapján van kiszámítva automatikusan a virtuális hálózat címelőtagjához és a megfelelő `addressPrefixSize`.
- Ha egy meglévő virtuális hálózat, kisebb, mint a megfelelő alhálózatok `constraints.minAddressPrefixSize` kijelölés nem érhetők el. Továbbá ha meg van adva, alhálózatok, amelyek nem tartalmaznak legalább `minAddressCount` elérhető címei nem lehet kiválasztani.
Az alapértelmezett érték **0**. Győződjön meg arról, hogy a rendelkezésre álló címek összefüggő, adja meg a **igaz** a `requireContiguousAddresses`. Az alapértelmezett érték **igaz**.
- A meglévő virtuális hálózat alhálózatok létrehozása nem támogatott.
- Ha `options.hideExisting` van **igaz**, a felhasználó egy meglévő virtuális hálózat nem választható. Az alapértelmezett érték **hamis**.

## <a name="sample-output"></a>Minta kimenet
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
* Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).
