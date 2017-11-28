---
title: "aaaAzure felügyelt alkalmazás PasswordBox felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Common.PasswordBox felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: bcb1f54c0bee464075ed732ead9aa3f88697f49e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonpasswordbox-ui-element"></a><span data-ttu-id="211d2-103">Microsoft.Common.PasswordBox felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="211d2-103">Microsoft.Common.PasswordBox UI element</span></span>
<span data-ttu-id="211d2-104">A vezérlő használt tooprovide és erősítse meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="211d2-104">A control that can be used tooprovide and confirm a password.</span></span> <span data-ttu-id="211d2-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="211d2-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="211d2-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="211d2-106">UI sample</span></span>
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a><span data-ttu-id="211d2-108">Séma</span><span class="sxs-lookup"><span data-stu-id="211d2-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="211d2-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="211d2-109">Remarks</span></span>
- <span data-ttu-id="211d2-110">Ez az elem nem támogatja a hello `defaultValue` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="211d2-110">This element doesn't support hello `defaultValue` property.</span></span>
- <span data-ttu-id="211d2-111">A megvalósítás részletei `constraints`, lásd: [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span><span class="sxs-lookup"><span data-stu-id="211d2-111">For implementation details of `constraints`, see [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span></span>
- <span data-ttu-id="211d2-112">Ha `options.hideConfirmation` értéke túl**igaz**, hello második szövegmező hello jelszó megerősítése van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="211d2-112">If `options.hideConfirmation` is set too**true**, hello second text box for confirming hello user's password is hidden.</span></span> <span data-ttu-id="211d2-113">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="211d2-113">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="211d2-114">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="211d2-114">Sample output</span></span>
```json
"p4ssw0rd"
```

## <a name="next-steps"></a><span data-ttu-id="211d2-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="211d2-115">Next steps</span></span>
* <span data-ttu-id="211d2-116">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="211d2-116">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="211d2-117">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="211d2-117">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="211d2-118">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="211d2-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
