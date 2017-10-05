---
title: "Az Azure által felügyelt alkalmazás PublicIpAddressCombo felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Network.PublicIpAddressCombo felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 2eb773f5f0cf389fc39bc3a0f5fbf9ac726d1949
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a><span data-ttu-id="d5286-103">Microsoft.Network.PublicIpAddressCombo felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="d5286-103">Microsoft.Network.PublicIpAddressCombo UI element</span></span>
<span data-ttu-id="d5286-104">Egy új vagy meglévő nyilvános IP-cím kiválasztásához vezérlők egy csoportja.</span><span class="sxs-lookup"><span data-stu-id="d5286-104">A group of controls for selecting a new or existing public IP address.</span></span> <span data-ttu-id="d5286-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d5286-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d5286-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="d5286-106">UI sample</span></span>
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- <span data-ttu-id="d5286-108">Ha a felhasználó kiválasztja a "Nincs" nyilvános IP-cím, a tartomány címke szövegmezőt rejtett.</span><span class="sxs-lookup"><span data-stu-id="d5286-108">If the user selects 'None' for public IP address, the domain name label text box is hidden.</span></span>
- <span data-ttu-id="d5286-109">Ha a felhasználó kiválaszt egy meglévő nyilvános IP-cím, a tartomány címke szövegmezőt le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="d5286-109">If the user selects an existing public IP address, the domain name label text box is disabled.</span></span> <span data-ttu-id="d5286-110">A kijelölt IP-cím tartománynévcímkéje érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="d5286-110">Its value is the domain name label of the selected IP address.</span></span>
- <span data-ttu-id="d5286-111">A tartomány neve utótagot (például westus.cloudapp.azure.com) frissítéseket automatikusan a kijelölt hely alapján.</span><span class="sxs-lookup"><span data-stu-id="d5286-111">The domain name suffix (for example, westus.cloudapp.azure.com) updates automatically based on the selected location.</span></span>

## <a name="schema"></a><span data-ttu-id="d5286-112">Séma</span><span class="sxs-lookup"><span data-stu-id="d5286-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="d5286-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d5286-113">Remarks</span></span>
- <span data-ttu-id="d5286-114">Ha `constraints.required.domainNameLabel` értéke **igaz**, a felhasználónak a tartománynév-címke meg kell adnia egy új nyilvános IP-cím létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="d5286-114">If `constraints.required.domainNameLabel` is set to **true**, the user must provide a domain name label when creating a new public IP address.</span></span> <span data-ttu-id="d5286-115">Meglévő nyilvános IP-címek, egy címke nélküli nem lehet választani.</span><span class="sxs-lookup"><span data-stu-id="d5286-115">Existing public IP addresses without a label are not available for selection.</span></span>
- <span data-ttu-id="d5286-116">Ha `options.hideNone` értéke **igaz**, majd jelölje be a beállítás **nincs** a nyilvános IP-cím van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="d5286-116">If `options.hideNone` is set to **true**, then the option to select **None** for the public IP address is hidden.</span></span> <span data-ttu-id="d5286-117">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d5286-117">The default value is **false**.</span></span>
- <span data-ttu-id="d5286-118">Ha `options.hideDomainNameLabel` értéke **igaz**, akkor a tartománynév-címke a szövegmezőbe írja be van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="d5286-118">If `options.hideDomainNameLabel` is set to **true**, then the text box for domain name label is hidden.</span></span> <span data-ttu-id="d5286-119">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d5286-119">The default value is **false**.</span></span>
- <span data-ttu-id="d5286-120">Ha `options.hideExisting` értéke igaz, akkor a felhasználó nem választhat egy meglévő nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="d5286-120">If `options.hideExisting` is true, then the user is not able to choose an existing public IP address.</span></span> <span data-ttu-id="d5286-121">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d5286-121">The default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="d5286-122">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="d5286-122">Sample output</span></span>
<span data-ttu-id="d5286-123">Ha a felhasználó kijelöli a nyilvános IP-cím, a következő kimeneti várt:</span><span class="sxs-lookup"><span data-stu-id="d5286-123">If the user selects no public IP address, the following output is expected:</span></span>
```json
{
  "newOrExistingOrNone": "none"
}
```

<span data-ttu-id="d5286-124">Ha a felhasználó kiválaszt egy új vagy meglévő IP-címet, a következő kimeneti várt:</span><span class="sxs-lookup"><span data-stu-id="d5286-124">If the user selects a new or existing IP address, the following output is expected:</span></span>
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- <span data-ttu-id="d5286-125">Ha `options.hideNone` meg van adva, `newOrExistingOrNone` mindig vissza **nincs**.</span><span class="sxs-lookup"><span data-stu-id="d5286-125">When `options.hideNone` is specified, `newOrExistingOrNone` always returns **none**.</span></span>
- <span data-ttu-id="d5286-126">Ha `options.hideDomainNameLabel` meg van adva, `domainNameLabel` nincs deklarálva.</span><span class="sxs-lookup"><span data-stu-id="d5286-126">When `options.hideDomainNameLabel` is specified, `domainNameLabel` is undeclared.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5286-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5286-127">Next steps</span></span>
* <span data-ttu-id="d5286-128">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5286-128">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d5286-129">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5286-129">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d5286-130">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d5286-130">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
