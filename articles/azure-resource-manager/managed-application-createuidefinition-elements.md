---
title: "aaaAzure kezelt alkalmazás, hozzon létre felhasználói felület definition funkciók |} Microsoft Docs"
description: "Hello funkciók toouse ismerteti az Azure által felügyelt alkalmazások felhasználói felületi meghatározások létrehozása"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: a34c6202372168cda769c471b1c9fdd539dd0f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition elemek
Ez a cikk ismerteti a hello séma- és egy CreateUiDefinition minden támogatott elemei tulajdonságait. Ezek az elemek használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md). hello séma legtöbb elemek a következőképpen történik:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Keep calm and visit hello [Azure Portal](portal.azure.com).",
  "constraints": {},
  "options": {},
  "visible": true
}
```
| Tulajdonság | Szükséges | Leírás |
| -------- | -------- | ----------- |
| név | Igen | Belső azonosító tooreference egy elem előfordulását. hello hello elemnév leggyakoribb használata van `outputs`, amennyiben hello hello kimeneti értékeit leképezett toohello paraméterek hello sablon elemei a következők. Is használhatja toobind hello kimeneti értéke egy elem toohello `defaultValue` másik elem. |
| type | Igen | hello felhasználói felület vezérlő toorender hello elemhez. A támogatott típusainak listáját lásd: [elemek](#elements). |
| Címke | Igen | hello hello elem szöveg megjelenítése. Néhány elemtípus több címke tartalmaz, így hello érték lehet egy több karakterláncokat tartalmazó objektum. |
| DefaultValue érték | Nem | hello hello elem alapértelmezett értéke. Néhány elemtípus támogatja összetett alapértelmezett értékeket, így hello érték lehet egy objektumot. |
| Elemleírás | Nem | hello szöveg toodisplay hello elemleírás hello elem található. Hasonló túl`label`, bizonyos elemek támogatja a több eszköz tipp karakterláncokat. Beágyazott hivatkozásokat tartalmaz a Markdown-szintaxis használatával lehet beágyazott.
| Megkötések | Nem | Egy vagy több tulajdonságokhoz hello elem használt toocustomize hello ellenőrzési viselkedését. elemtípus megkötések hello támogatott tulajdonságai eltérők. Néhány elemtípus nem támogatják a hello ellenőrzési viselkedése testreszabása, és így vannak megkötések tulajdonságot. |
| beállítások | Nem | További tulajdonságok testre szabják hello elem hello jellemzőit. Hasonló túl`constraints`, hello támogatott tulajdonságok elemtípus változhat. |
| Látható | Nem | Azt jelzi, hogy megjelenik-e hello elem. Ha `true`, hello elem és a megfelelő gyermekelemek jelennek meg. hello alapértelmezett értéke `true`. Használjon [logikai funkciók](managed-application-createuidefinition-functions.md#logical-functions) toodynamically szabályozhatja a tulajdonság értékét.

## <a name="elements"></a>Elemek

dokumentáció hello egyes elemei a felhasználói felület mintát tartalmaz, séma, megjegyzések hello elem (általában vonatkozó érvényesítése és a támogatott Testreszabás), és a minta kimenet hello viselkedését.

- [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
- [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
