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
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a><span data-ttu-id="1002a-103">Microsoft.Network.PublicIpAddressCombo felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="1002a-103">Microsoft.Network.PublicIpAddressCombo UI element</span></span>
<span data-ttu-id="1002a-104">Egy új vagy meglévő nyilvános IP-cím kiválasztásához vezérlők egy csoportja.</span><span class="sxs-lookup"><span data-stu-id="1002a-104">A group of controls for selecting a new or existing public IP address.</span></span> <span data-ttu-id="1002a-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="1002a-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="1002a-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="1002a-106">UI sample</span></span>
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- <span data-ttu-id="1002a-108">Ha hello felhasználó kiválasztja a "Nincs" nyilvános IP-cím, hello tartomány címke szövegmezőt rejtett.</span><span class="sxs-lookup"><span data-stu-id="1002a-108">If hello user selects 'None' for public IP address, hello domain name label text box is hidden.</span></span>
- <span data-ttu-id="1002a-109">Ha hello felhasználó kiválaszt egy meglévő nyilvános IP-cím, hello tartomány címke szövegmezőt le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="1002a-109">If hello user selects an existing public IP address, hello domain name label text box is disabled.</span></span> <span data-ttu-id="1002a-110">Hello tartománynév-címke hello kiválasztott IP-cím érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="1002a-110">Its value is hello domain name label of hello selected IP address.</span></span>
- <span data-ttu-id="1002a-111">hello tartomány neve (például westus.cloudapp.azure.com) utótag frissítések automatikusan kijelölt hello helye alapján.</span><span class="sxs-lookup"><span data-stu-id="1002a-111">hello domain name suffix (for example, westus.cloudapp.azure.com) updates automatically based on hello selected location.</span></span>

## <a name="schema"></a><span data-ttu-id="1002a-112">Séma</span><span class="sxs-lookup"><span data-stu-id="1002a-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="1002a-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1002a-113">Remarks</span></span>
- <span data-ttu-id="1002a-114">Ha `constraints.required.domainNameLabel` értéke túl**igaz**, hello felhasználónak a tartománynév-címke meg kell adnia egy új nyilvános IP-cím létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="1002a-114">If `constraints.required.domainNameLabel` is set too**true**, hello user must provide a domain name label when creating a new public IP address.</span></span> <span data-ttu-id="1002a-115">Meglévő nyilvános IP-címek, egy címke nélküli nem lehet választani.</span><span class="sxs-lookup"><span data-stu-id="1002a-115">Existing public IP addresses without a label are not available for selection.</span></span>
- <span data-ttu-id="1002a-116">Ha `options.hideNone` értéke túl**igaz**, majd a beállítás tooselect hello **nincs** hello nyilvános IP-cím van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="1002a-116">If `options.hideNone` is set too**true**, then hello option tooselect **None** for hello public IP address is hidden.</span></span> <span data-ttu-id="1002a-117">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="1002a-117">hello default value is **false**.</span></span>
- <span data-ttu-id="1002a-118">Ha `options.hideDomainNameLabel` értéke túl**igaz**, majd a tartománynév-címke hello szövegmezőbe írja be van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="1002a-118">If `options.hideDomainNameLabel` is set too**true**, then hello text box for domain name label is hidden.</span></span> <span data-ttu-id="1002a-119">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="1002a-119">hello default value is **false**.</span></span>
- <span data-ttu-id="1002a-120">Ha `options.hideExisting` értéke igaz, akkor hello felhasználó nem tud toochoose egy meglévő nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="1002a-120">If `options.hideExisting` is true, then hello user is not able toochoose an existing public IP address.</span></span> <span data-ttu-id="1002a-121">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="1002a-121">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="1002a-122">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="1002a-122">Sample output</span></span>
<span data-ttu-id="1002a-123">Ha nincs nyilvános IP-cím a hello felhasználó választ, hello következő kimeneti várt:</span><span class="sxs-lookup"><span data-stu-id="1002a-123">If hello user selects no public IP address, hello following output is expected:</span></span>
```json
{
  "newOrExistingOrNone": "none"
}
```

<span data-ttu-id="1002a-124">Ha hello felhasználó kiválaszt egy új vagy meglévő IP-címet, hello következő kimeneti várt:</span><span class="sxs-lookup"><span data-stu-id="1002a-124">If hello user selects a new or existing IP address, hello following output is expected:</span></span>
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- <span data-ttu-id="1002a-125">Ha `options.hideNone` meg van adva, `newOrExistingOrNone` mindig vissza **nincs**.</span><span class="sxs-lookup"><span data-stu-id="1002a-125">When `options.hideNone` is specified, `newOrExistingOrNone` always returns **none**.</span></span>
- <span data-ttu-id="1002a-126">Ha `options.hideDomainNameLabel` meg van adva, `domainNameLabel` nincs deklarálva.</span><span class="sxs-lookup"><span data-stu-id="1002a-126">When `options.hideDomainNameLabel` is specified, `domainNameLabel` is undeclared.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1002a-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1002a-127">Next steps</span></span>
* <span data-ttu-id="1002a-128">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1002a-128">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="1002a-129">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1002a-129">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="1002a-130">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="1002a-130">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
