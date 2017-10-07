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
# <a name="microsoftcommonfileupload-ui-element"></a>Microsoft.Common.FileUpload felhasználói felületi elem
A vezérlő, amely lehetővé teszi, hogy egy felhasználó toospecify egy vagy több tooupload fájlok. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a>Séma
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

## <a name="remarks"></a>Megjegyzések
- `constraints.accept`Megadja a hello fájltípusok hello böngésző fájl párbeszédpanelen látható. Lásd: hello [HTML5-specifikációt](http://www.w3.org/TR/html5/forms.html#attr-input-accept) az engedélyezett értékek. hello alapértelmezett értéke **null**.
- Ha `options.multiple` értéke túl**igaz**, hello használhatók tooselect több mint egy fájl hello böngészővel fájl párbeszédpanelt. hello alapértelmezett értéke **hamis**.
- Ez az elem feltöltése fájlokat támogatja hello értéke alapján két módban `options.uploadMode`. Ha **fájl** van megadva, hello kimenet hello fájlt egy blobba hello tartalmát tartalmazza. Ha **URL-cím** van megadva, akkor hello fájl feltöltött tooa ideiglenes helyre, és hello kimenet hello hello blob URL-CÍMÉT tartalmazza. Ideiglenes blobok kiürítendő 24 óra múlva. hello alapértelmezett értéke **fájl**.
- hello értékének `options.openMode` határozza meg, hogyan hello fájl olvasható. Ha hello fájl várt toobe egyszerű szövegként, adja meg a **szöveg**; más, adja meg **bináris**. hello alapértelmezett értéke **szöveg**.
- Ha `options.uploadMode` értéke túl**fájl** és `options.openMode` értéke túl**bináris**, hello eredménye a base64 kódolású.
- `options.encoding`Adja meg a hello kódolási toouse hello fájl olvasásakor. hello alapértelmezett értéke **UTF-8**, és csak akkor, ha `options.openMode` értéke túl**szöveg**.

## <a name="sample-output"></a>Példa kimenet
Ha options.multiple értéke false, és options.uploadMode fájlt, majd a kimenet tartalmazza hello hello fájl tartalmát egy JSON-karakterlánc:

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

Ha igaz options.multiple and'options.uploadMode fájl, akkor a kimenet tartalmazza a hello fájlok hello tartalmát egy JSON-tömb:

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

Ha options.multiple értéke false, és options.uploadMode URL-címet, majd a kimeneti egy URL-címet tartalmaz JSON karakterláncként:

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

Ha options.multiple értéke true, és options.uploadMode URL-címet, majd a kimenet tartalmazza URL-címek listáját egy JSON-tömb:
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

Egy CreateUiDefinition tesztelésekor (például a Google Chrome) egyes böngészők csonkolása hello böngészőkonzolt hello Microsoft.Common.FileUpload eleme által generált URL-címeket. Szükség lehet tooright kattintással egyes hivatkozások toocopy hello teljes URL-címeket.


## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).
