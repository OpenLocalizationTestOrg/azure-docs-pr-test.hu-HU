---
title: "Az Azure által felügyelt alkalmazás FileUpload felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Common.FileUpload felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 217e9e63eb7cd198f70cee42b418867df9f1f993
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonfileupload-ui-element"></a><span data-ttu-id="d439d-103">Microsoft.Common.FileUpload felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="d439d-103">Microsoft.Common.FileUpload UI element</span></span>
<span data-ttu-id="d439d-104">Lehetővé teszi a felhasználóknak adjon meg egy vagy több fájl feltöltése vezérlő.</span><span class="sxs-lookup"><span data-stu-id="d439d-104">A control that allows a user to specify one or more files to upload.</span></span> <span data-ttu-id="d439d-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d439d-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d439d-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="d439d-106">UI sample</span></span>
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a><span data-ttu-id="d439d-108">Séma</span><span class="sxs-lookup"><span data-stu-id="d439d-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.FileUpload",
  "label": "Some file upload",
  "toolTip": "",
  "constraints": {
    "required": true,
    "accept": ".doc,.docx,.xml,application/msword"
  },
  "options": {
    "multiple": false,
    "uploadMode": "file",
    "openMode": "text",
    "encoding": "UTF-8"
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="d439d-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d439d-109">Remarks</span></span>
- <span data-ttu-id="d439d-110">`constraints.accept`Adja meg a fájlokat, amelyek megjelennek a böngésző fájlpárbeszédpanelt típusú.</span><span class="sxs-lookup"><span data-stu-id="d439d-110">`constraints.accept` specifies the types of files that are shown in the browser's file dialog.</span></span> <span data-ttu-id="d439d-111">Tekintse meg a [HTML5-specifikációt](http://www.w3.org/TR/html5/forms.html#attr-input-accept) az engedélyezett értékek.</span><span class="sxs-lookup"><span data-stu-id="d439d-111">See the [HTML5 specification](http://www.w3.org/TR/html5/forms.html#attr-input-accept) for allowed values.</span></span> <span data-ttu-id="d439d-112">Az alapértelmezett érték **null**.</span><span class="sxs-lookup"><span data-stu-id="d439d-112">The default value is **null**.</span></span>
- <span data-ttu-id="d439d-113">Ha `options.multiple` értéke **igaz**, a felhasználó által megadható egynél több fájl a böngésző fájl párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="d439d-113">If `options.multiple` is set to **true**, the user is allowed to select more than one file in the browser's file dialog.</span></span> <span data-ttu-id="d439d-114">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d439d-114">The default value is **false**.</span></span>
- <span data-ttu-id="d439d-115">Ez az elem feltöltése fájlokat támogatja a két mód értéke alapján `options.uploadMode`.</span><span class="sxs-lookup"><span data-stu-id="d439d-115">This element supports uploading files in two modes based on the value of `options.uploadMode`.</span></span> <span data-ttu-id="d439d-116">Ha **fájl** van megadva, a kimenet tartalmazza a fájlt egy blobba tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d439d-116">If **file** is specified, the output contains the contents of the file as a blob.</span></span> <span data-ttu-id="d439d-117">Ha **URL-cím** van megadva, akkor a fájl feltöltése egy ideiglenes helyre, és a kimenet tartalmazza a blob URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d439d-117">If **url** is specified, then the file is uploaded to a temporary location, and the output contains the URL of the blob.</span></span> <span data-ttu-id="d439d-118">Ideiglenes blobok kiürítendő 24 óra múlva.</span><span class="sxs-lookup"><span data-stu-id="d439d-118">Temporary blobs will be purged after 24 hours.</span></span> <span data-ttu-id="d439d-119">Az alapértelmezett érték **fájl**.</span><span class="sxs-lookup"><span data-stu-id="d439d-119">The default value is **file**.</span></span>
- <span data-ttu-id="d439d-120">Értékének `options.openMode` határozza meg, hogy a fájl írásvédett.</span><span class="sxs-lookup"><span data-stu-id="d439d-120">The value of `options.openMode` determines how the file is read.</span></span> <span data-ttu-id="d439d-121">Ha a fájl várt egyszerű szövegként, adja meg **szöveg**; más, adja meg **bináris**.</span><span class="sxs-lookup"><span data-stu-id="d439d-121">If the file is expected to be plain text, specify **text**; else, specify **binary**.</span></span> <span data-ttu-id="d439d-122">Az alapértelmezett érték **szöveg**.</span><span class="sxs-lookup"><span data-stu-id="d439d-122">The default value is **text**.</span></span>
- <span data-ttu-id="d439d-123">Ha `options.uploadMode` értéke **fájl** és `options.openMode` értéke **bináris**, a base64-kódolású kimenete.</span><span class="sxs-lookup"><span data-stu-id="d439d-123">If `options.uploadMode` is set to **file** and `options.openMode` is set to **binary**, the output is base64-encoded.</span></span>
- <span data-ttu-id="d439d-124">`options.encoding`Meghatározza, hogy a fájl olvasásánál használandó kódolás.</span><span class="sxs-lookup"><span data-stu-id="d439d-124">`options.encoding` specifies the encoding to use when reading the file.</span></span> <span data-ttu-id="d439d-125">Az alapértelmezett érték **UTF-8**, és csak akkor, ha `options.openMode` értéke **szöveg**.</span><span class="sxs-lookup"><span data-stu-id="d439d-125">The default value is **UTF-8**, and is used only when `options.openMode` is set to **text**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="d439d-126">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="d439d-126">Sample output</span></span>
<span data-ttu-id="d439d-127">Ha options.multiple értéke false, és options.uploadMode fájlt, majd a kimenet tartalmazza a fájl tartalmát egy JSON-karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="d439d-127">If options.multiple is false and options.uploadMode is file, then the output contains the contents of the file as a JSON string:</span></span>

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

<span data-ttu-id="d439d-128">Ha igaz options.multiple and'options.uploadMode fájl, akkor a kimenet tartalmazza a fájlok tartalmát egy JSON-tömb:</span><span class="sxs-lookup"><span data-stu-id="d439d-128">If options.multiple is true and\`options.uploadMode is file, then the output contains the contents of the files as a JSON array:</span></span>

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

<span data-ttu-id="d439d-129">Ha options.multiple értéke false, és options.uploadMode URL-címet, majd a kimeneti egy URL-címet tartalmaz JSON karakterláncként:</span><span class="sxs-lookup"><span data-stu-id="d439d-129">If options.multiple is false and options.uploadMode is url, then the output contains a URL as a JSON string:</span></span>

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

<span data-ttu-id="d439d-130">Ha options.multiple értéke true, és options.uploadMode URL-címet, majd a kimenet tartalmazza URL-címek listáját egy JSON-tömb:</span><span class="sxs-lookup"><span data-stu-id="d439d-130">If options.multiple is true and options.uploadMode is url, then the output contains a list of URLs as a JSON array:</span></span>
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

<span data-ttu-id="d439d-131">Egy CreateUiDefinition tesztelésekor (például a Google Chrome) egyes böngészők csonkolja a böngészőbeli konzolon a Microsoft.Common.FileUpload eleme által generált URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="d439d-131">When testing a CreateUiDefinition, some browsers (like Google Chrome) truncate URLs generated by the Microsoft.Common.FileUpload element in the browser console.</span></span> <span data-ttu-id="d439d-132">Kattintson a jobb gombbal az egyes hivatkozások másolása a teljes URL-címeket szeretne.</span><span class="sxs-lookup"><span data-stu-id="d439d-132">You may need to right-click individual links to copy the full URLs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d439d-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d439d-133">Next steps</span></span>
* <span data-ttu-id="d439d-134">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d439d-134">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d439d-135">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d439d-135">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d439d-136">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d439d-136">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
