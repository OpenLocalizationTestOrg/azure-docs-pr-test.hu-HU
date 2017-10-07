---
title: "aaaAzure felügyelt alkalmazás FileUpload felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Common.FileUpload felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 7af5bec992e3f120afb1bdf56d8b4c19a8e5e834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonfileupload-ui-element"></a><span data-ttu-id="e11e5-103">Microsoft.Common.FileUpload felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="e11e5-103">Microsoft.Common.FileUpload UI element</span></span>
<span data-ttu-id="e11e5-104">A vezérlő, amely lehetővé teszi, hogy egy felhasználó toospecify egy vagy több tooupload fájlok.</span><span class="sxs-lookup"><span data-stu-id="e11e5-104">A control that allows a user toospecify one or more files tooupload.</span></span> <span data-ttu-id="e11e5-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="e11e5-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="e11e5-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="e11e5-106">UI sample</span></span>
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a><span data-ttu-id="e11e5-108">Séma</span><span class="sxs-lookup"><span data-stu-id="e11e5-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="e11e5-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e11e5-109">Remarks</span></span>
- <span data-ttu-id="e11e5-110">`constraints.accept`Megadja a hello fájltípusok hello böngésző fájl párbeszédpanelen látható.</span><span class="sxs-lookup"><span data-stu-id="e11e5-110">`constraints.accept` specifies hello types of files that are shown in hello browser's file dialog.</span></span> <span data-ttu-id="e11e5-111">Lásd: hello [HTML5-specifikációt](http://www.w3.org/TR/html5/forms.html#attr-input-accept) az engedélyezett értékek.</span><span class="sxs-lookup"><span data-stu-id="e11e5-111">See hello [HTML5 specification](http://www.w3.org/TR/html5/forms.html#attr-input-accept) for allowed values.</span></span> <span data-ttu-id="e11e5-112">hello alapértelmezett értéke **null**.</span><span class="sxs-lookup"><span data-stu-id="e11e5-112">hello default value is **null**.</span></span>
- <span data-ttu-id="e11e5-113">Ha `options.multiple` értéke túl**igaz**, hello használhatók tooselect több mint egy fájl hello böngészővel fájl párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="e11e5-113">If `options.multiple` is set too**true**, hello user is allowed tooselect more than one file in hello browser's file dialog.</span></span> <span data-ttu-id="e11e5-114">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e11e5-114">hello default value is **false**.</span></span>
- <span data-ttu-id="e11e5-115">Ez az elem feltöltése fájlokat támogatja hello értéke alapján két módban `options.uploadMode`.</span><span class="sxs-lookup"><span data-stu-id="e11e5-115">This element supports uploading files in two modes based on hello value of `options.uploadMode`.</span></span> <span data-ttu-id="e11e5-116">Ha **fájl** van megadva, hello kimenet hello fájlt egy blobba hello tartalmát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e11e5-116">If **file** is specified, hello output contains hello contents of hello file as a blob.</span></span> <span data-ttu-id="e11e5-117">Ha **URL-cím** van megadva, akkor hello fájl feltöltött tooa ideiglenes helyre, és hello kimenet hello hello blob URL-CÍMÉT tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e11e5-117">If **url** is specified, then hello file is uploaded tooa temporary location, and hello output contains hello URL of hello blob.</span></span> <span data-ttu-id="e11e5-118">Ideiglenes blobok kiürítendő 24 óra múlva.</span><span class="sxs-lookup"><span data-stu-id="e11e5-118">Temporary blobs will be purged after 24 hours.</span></span> <span data-ttu-id="e11e5-119">hello alapértelmezett értéke **fájl**.</span><span class="sxs-lookup"><span data-stu-id="e11e5-119">hello default value is **file**.</span></span>
- <span data-ttu-id="e11e5-120">hello értékének `options.openMode` határozza meg, hogyan hello fájl olvasható.</span><span class="sxs-lookup"><span data-stu-id="e11e5-120">hello value of `options.openMode` determines how hello file is read.</span></span> <span data-ttu-id="e11e5-121">Ha hello fájl várt toobe egyszerű szövegként, adja meg a **szöveg**; más, adja meg **bináris**.</span><span class="sxs-lookup"><span data-stu-id="e11e5-121">If hello file is expected toobe plain text, specify **text**; else, specify **binary**.</span></span> <span data-ttu-id="e11e5-122">hello alapértelmezett értéke **szöveg**.</span><span class="sxs-lookup"><span data-stu-id="e11e5-122">hello default value is **text**.</span></span>
- <span data-ttu-id="e11e5-123">Ha `options.uploadMode` értéke túl**fájl** és `options.openMode` értéke túl**bináris**, hello eredménye a base64 kódolású.</span><span class="sxs-lookup"><span data-stu-id="e11e5-123">If `options.uploadMode` is set too**file** and `options.openMode` is set too**binary**, hello output is base64-encoded.</span></span>
- <span data-ttu-id="e11e5-124">`options.encoding`Adja meg a hello kódolási toouse hello fájl olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="e11e5-124">`options.encoding` specifies hello encoding toouse when reading hello file.</span></span> <span data-ttu-id="e11e5-125">hello alapértelmezett értéke **UTF-8**, és csak akkor, ha `options.openMode` értéke túl**szöveg**.</span><span class="sxs-lookup"><span data-stu-id="e11e5-125">hello default value is **UTF-8**, and is used only when `options.openMode` is set too**text**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="e11e5-126">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="e11e5-126">Sample output</span></span>
<span data-ttu-id="e11e5-127">Ha options.multiple értéke false, és options.uploadMode fájlt, majd a kimenet tartalmazza hello hello fájl tartalmát egy JSON-karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="e11e5-127">If options.multiple is false and options.uploadMode is file, then the output contains hello contents of hello file as a JSON string:</span></span>

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

<span data-ttu-id="e11e5-128">Ha igaz options.multiple and'options.uploadMode fájl, akkor a kimenet tartalmazza a hello fájlok hello tartalmát egy JSON-tömb:</span><span class="sxs-lookup"><span data-stu-id="e11e5-128">If options.multiple is true and\`options.uploadMode is file, then the output contains hello contents of hello files as a JSON array:</span></span>

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

<span data-ttu-id="e11e5-129">Ha options.multiple értéke false, és options.uploadMode URL-címet, majd a kimeneti egy URL-címet tartalmaz JSON karakterláncként:</span><span class="sxs-lookup"><span data-stu-id="e11e5-129">If options.multiple is false and options.uploadMode is url, then the output contains a URL as a JSON string:</span></span>

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

<span data-ttu-id="e11e5-130">Ha options.multiple értéke true, és options.uploadMode URL-címet, majd a kimenet tartalmazza URL-címek listáját egy JSON-tömb:</span><span class="sxs-lookup"><span data-stu-id="e11e5-130">If options.multiple is true and options.uploadMode is url, then the output contains a list of URLs as a JSON array:</span></span>
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

<span data-ttu-id="e11e5-131">Egy CreateUiDefinition tesztelésekor (például a Google Chrome) egyes böngészők csonkolása hello böngészőkonzolt hello Microsoft.Common.FileUpload eleme által generált URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="e11e5-131">When testing a CreateUiDefinition, some browsers (like Google Chrome) truncate URLs generated by hello Microsoft.Common.FileUpload element in hello browser console.</span></span> <span data-ttu-id="e11e5-132">Szükség lehet tooright kattintással egyes hivatkozások toocopy hello teljes URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="e11e5-132">You may need tooright-click individual links toocopy hello full URLs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e11e5-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e11e5-133">Next steps</span></span>
* <span data-ttu-id="e11e5-134">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e11e5-134">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="e11e5-135">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e11e5-135">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="e11e5-136">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="e11e5-136">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
