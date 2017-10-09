---
title: "aaaAzure felügyelt alkalmazás CredentialsCombo felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Compute.CredentialsCombo felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: d44a3929ebb7a5ff78b72f9eaeb6e52b098e266f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo felhasználói felületi elem
A Windows és Linux jelszavak és a nyilvános SSH-kulcsok beépített érvényesítéssel vezérlők egy csoportja. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a>Séma
Ha `osPlatform` van **Windows**, majd hello következő sémát használja:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "hello password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

Ha `osPlatform` van **Linux**, majd hello következő sémát használja:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "hello password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- `osPlatform`meg kell adni, és lehet **Windows** vagy **Linux**.
- Ha `constraints.required` értéke túl**igaz**, majd hello jelszó vagy SSH nyilvános kulcs szövegmezőkben sikeresen értékek toovalidate kell tartalmaznia. hello alapértelmezett értéke **igaz**.
- Ha `options.hideConfirmation` értéke túl**igaz**, akkor hello második szövegmező hello jelszó megerősítése el van rejtve. hello alapértelmezett értéke **hamis**.
- Ha `options.hidePassword` értéke túl**igaz**, majd hello beállítás toouse jelszó-hitelesítés van-e rejtve. Használat csak akkor, ha `osPlatform` van **Linux**. Az alapértelmezett érték **hamis**.
- További korlátozza a hello engedélyezett jelszavak hello segítségével végrehajtható `customPasswordRegex` tulajdonság. a karakterlánc hello `customValidationMessage` akkor látható, ha a jelszó egyéni érvényesítése sikertelen. hello alapértelmezett mindkét tulajdonság értéke **null**.

## <a name="sample-output"></a>Példa kimenet
Ha `osPlatform` van **Windows**, vagy hello felhasználó által megadott helyett nyilvános SSH-kulcs jelszót, majd hello következő kimeneti várt:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

Ha hello felhasználó által megadott nyilvános SSH-kulcsot, majd hello következő kimeneti várt:
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).
