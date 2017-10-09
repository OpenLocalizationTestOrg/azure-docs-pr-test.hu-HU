---
title: "aaaAzure felügyelt alkalmazás PublicIpAddressCombo felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Network.PublicIpAddressCombo felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 8ba689005c0eccda0a57bf628de4b5197886a950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo felhasználói felületi elem
Egy új vagy meglévő nyilvános IP-cím kiválasztásához vezérlők egy csoportja. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Ha hello felhasználó kiválasztja a "Nincs" nyilvános IP-cím, hello tartomány címke szövegmezőt rejtett.
- Ha hello felhasználó kiválaszt egy meglévő nyilvános IP-cím, hello tartomány címke szövegmezőt le van tiltva. Hello tartománynév-címke hello kiválasztott IP-cím érvénytelen.
- hello tartomány neve (például westus.cloudapp.azure.com) utótag frissítések automatikusan kijelölt hello helye alapján.

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "foobar"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- Ha `constraints.required.domainNameLabel` értéke túl**igaz**, hello felhasználónak a tartománynév-címke meg kell adnia egy új nyilvános IP-cím létrehozása során. Meglévő nyilvános IP-címek, egy címke nélküli nem lehet választani.
- Ha `options.hideNone` értéke túl**igaz**, majd a beállítás tooselect hello **nincs** hello nyilvános IP-cím van-e rejtve. hello alapértelmezett értéke **hamis**.
- Ha `options.hideDomainNameLabel` értéke túl**igaz**, majd a tartománynév-címke hello szövegmezőbe írja be van-e rejtve. hello alapértelmezett értéke **hamis**.
- Ha `options.hideExisting` értéke igaz, akkor hello felhasználó nem tud toochoose egy meglévő nyilvános IP-cím. hello alapértelmezett értéke **hamis**.

## <a name="sample-output"></a>Példa kimenet
Ha nincs nyilvános IP-cím a hello felhasználó választ, hello következő kimeneti várt:
```json
{
  "newOrExistingOrNone": "none"
}
```

Ha hello felhasználó kiválaszt egy új vagy meglévő IP-címet, hello következő kimeneti várt:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- Ha `options.hideNone` meg van adva, `newOrExistingOrNone` mindig vissza **nincs**.
- Ha `options.hideDomainNameLabel` meg van adva, `domainNameLabel` nincs deklarálva.

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).
