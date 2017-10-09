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
# <a name="microsoftcomputecredentialscombo-ui-element"></a><span data-ttu-id="b39eb-103">Microsoft.Compute.CredentialsCombo felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="b39eb-103">Microsoft.Compute.CredentialsCombo UI element</span></span>
<span data-ttu-id="b39eb-104">A Windows és Linux jelszavak és a nyilvános SSH-kulcsok beépített érvényesítéssel vezérlők egy csoportja.</span><span class="sxs-lookup"><span data-stu-id="b39eb-104">A group of controls with built-in validation for Windows and Linux passwords and SSH public keys.</span></span> <span data-ttu-id="b39eb-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="b39eb-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="b39eb-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="b39eb-106">UI sample</span></span>
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a><span data-ttu-id="b39eb-108">Séma</span><span class="sxs-lookup"><span data-stu-id="b39eb-108">Schema</span></span>
<span data-ttu-id="b39eb-109">Ha `osPlatform` van **Windows**, majd hello következő sémát használja:</span><span class="sxs-lookup"><span data-stu-id="b39eb-109">If `osPlatform` is **Windows**, then hello following schema is used:</span></span>
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

<span data-ttu-id="b39eb-110">Ha `osPlatform` van **Linux**, majd hello következő sémát használja:</span><span class="sxs-lookup"><span data-stu-id="b39eb-110">If `osPlatform` is **Linux**, then hello following schema is used:</span></span>
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

## <a name="remarks"></a><span data-ttu-id="b39eb-111">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b39eb-111">Remarks</span></span>
- <span data-ttu-id="b39eb-112">`osPlatform`meg kell adni, és lehet **Windows** vagy **Linux**.</span><span class="sxs-lookup"><span data-stu-id="b39eb-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="b39eb-113">Ha `constraints.required` értéke túl**igaz**, majd hello jelszó vagy SSH nyilvános kulcs szövegmezőkben sikeresen értékek toovalidate kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="b39eb-113">If `constraints.required` is set too**true**, then hello password or SSH public key text boxes must contain values toovalidate successfully.</span></span> <span data-ttu-id="b39eb-114">hello alapértelmezett értéke **igaz**.</span><span class="sxs-lookup"><span data-stu-id="b39eb-114">hello default value is **true**.</span></span>
- <span data-ttu-id="b39eb-115">Ha `options.hideConfirmation` értéke túl**igaz**, akkor hello második szövegmező hello jelszó megerősítése el van rejtve.</span><span class="sxs-lookup"><span data-stu-id="b39eb-115">If `options.hideConfirmation` is set too**true**, then hello second text box for confirming hello user's password is hidden.</span></span> <span data-ttu-id="b39eb-116">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="b39eb-116">hello default value is **false**.</span></span>
- <span data-ttu-id="b39eb-117">Ha `options.hidePassword` értéke túl**igaz**, majd hello beállítás toouse jelszó-hitelesítés van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="b39eb-117">If `options.hidePassword` is set too**true**, then hello option toouse password authentication is hidden.</span></span> <span data-ttu-id="b39eb-118">Használat csak akkor, ha `osPlatform` van **Linux**.</span><span class="sxs-lookup"><span data-stu-id="b39eb-118">It can be used only when `osPlatform` is **Linux**.</span></span> <span data-ttu-id="b39eb-119">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="b39eb-119">The default value is **false**.</span></span>
- <span data-ttu-id="b39eb-120">További korlátozza a hello engedélyezett jelszavak hello segítségével végrehajtható `customPasswordRegex` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b39eb-120">Additional constraints on hello allowed passwords can be implemented by using hello `customPasswordRegex` property.</span></span> <span data-ttu-id="b39eb-121">a karakterlánc hello `customValidationMessage` akkor látható, ha a jelszó egyéni érvényesítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="b39eb-121">hello string in `customValidationMessage` is displayed when a password fails custom validation.</span></span> <span data-ttu-id="b39eb-122">hello alapértelmezett mindkét tulajdonság értéke **null**.</span><span class="sxs-lookup"><span data-stu-id="b39eb-122">hello default value for both properties is **null**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="b39eb-123">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="b39eb-123">Sample output</span></span>
<span data-ttu-id="b39eb-124">Ha `osPlatform` van **Windows**, vagy hello felhasználó által megadott helyett nyilvános SSH-kulcs jelszót, majd hello következő kimeneti várt:</span><span class="sxs-lookup"><span data-stu-id="b39eb-124">If `osPlatform` is **Windows**, or hello user provided a password instead of an SSH public key, then hello following output is expected:</span></span>

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

<span data-ttu-id="b39eb-125">Ha hello felhasználó által megadott nyilvános SSH-kulcsot, majd hello következő kimeneti várt:</span><span class="sxs-lookup"><span data-stu-id="b39eb-125">If hello user provided an SSH public key, then hello following output is expected:</span></span>
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a><span data-ttu-id="b39eb-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b39eb-126">Next steps</span></span>
* <span data-ttu-id="b39eb-127">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b39eb-127">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="b39eb-128">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b39eb-128">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="b39eb-129">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="b39eb-129">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
