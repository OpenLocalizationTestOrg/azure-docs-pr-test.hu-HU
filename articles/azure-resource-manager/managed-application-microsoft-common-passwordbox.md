---
title: "Az Azure által felügyelt alkalmazás PasswordBox felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Common.PasswordBox felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 196a4b8f77145f83e46b4b23e148bb3a9dffc1b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonpasswordbox-ui-element"></a><span data-ttu-id="e6add-103">Microsoft.Common.PasswordBox felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="e6add-103">Microsoft.Common.PasswordBox UI element</span></span>
<span data-ttu-id="e6add-104">Ez a vezérlő segítségével adja meg, és erősítse meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="e6add-104">A control that can be used to provide and confirm a password.</span></span> <span data-ttu-id="e6add-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="e6add-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="e6add-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="e6add-106">UI sample</span></span>
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a><span data-ttu-id="e6add-108">Séma</span><span class="sxs-lookup"><span data-stu-id="e6add-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="e6add-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e6add-109">Remarks</span></span>
- <span data-ttu-id="e6add-110">Ez az elem nem támogatja a `defaultValue` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e6add-110">This element doesn't support the `defaultValue` property.</span></span>
- <span data-ttu-id="e6add-111">A megvalósítás részletei `constraints`, lásd: [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span><span class="sxs-lookup"><span data-stu-id="e6add-111">For implementation details of `constraints`, see [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span></span>
- <span data-ttu-id="e6add-112">Ha `options.hideConfirmation` értéke **igaz**, erősítse meg a jelszót a második szöveges jelölőnégyzet be van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="e6add-112">If `options.hideConfirmation` is set to **true**, the second text box for confirming the user's password is hidden.</span></span> <span data-ttu-id="e6add-113">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e6add-113">The default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="e6add-114">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="e6add-114">Sample output</span></span>
```json
"p4ssw0rd"
```

## <a name="next-steps"></a><span data-ttu-id="e6add-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6add-115">Next steps</span></span>
* <span data-ttu-id="e6add-116">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6add-116">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="e6add-117">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6add-117">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="e6add-118">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="e6add-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
