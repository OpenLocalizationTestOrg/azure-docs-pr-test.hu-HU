---
title: "aaaCreate és az Azure szolgáltatás katalógus által felügyelt alkalmazások közzététele |} Microsoft Docs"
description: "Bemutatja, hogyan toocreate az Azure által felügyelt alkalmazás, amelyet a szervezet tagjaira."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 31f2f9e3b50f57dae7f4dcf2edefa7366bfff25c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>A belső felhasználásához kezelt alkalmazás közzététele

Létrehozhat és közzététele az Azure [kezelt alkalmazások](managed-application-overview.md) szolgálnak, hogy a szervezet tagjaira. Például hogy az informatikai részleg, amelyek biztosítják a vállalati szabványoknak való megfelelés felügyelt alkalmazások közzététele. A kezelt alkalmazások hello szolgáltatáskatalógus, nem hello Azure piactéren keresztül érhetők el.

toopublish hello szolgáltatási katalógusa kezelt alkalmazás, kell:

* Hozzon létre egy hello három sablon szükséges fájlokat tartalmazó .zip-csomagja.
* Döntse el, mely felhasználó, csoport vagy alkalmazás toohello erőforráscsoport hello felhasználó az előfizetéshez kell hozzáférni.
* Hozzon létre felügyelt hello definíciót, amely toohello .zip-csomagja mutat, és hello identitás hozzáférést kér.

## <a name="create-a-managed-application-package"></a>Kezelt alkalmazás-csomag létrehozása

hello első lépéseként toocreate hello három szükséges sablont fájl. Mindhárom fájlt csomagot .zip fájlba, majd töltse fel az tooan elérhető helyre, például egy tárfiókot. A hivatkozás toothis .zip fájl létrehozása hello kezelt definíciót adja át.

* **applianceMainTemplate.json**: A fájl határozza meg a hello Azure által hello részeként telepített erőforrások felügyelt alkalmazást. hello sablon ugyanolyan helyzetet teremt, mint egy normál Resource Manager-sablon. Például egy tárfiókot, felügyelt alkalmazás segítségével toocreate, applianceMainTemplate.json tartalmazza:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]",
            "apiVersion": "2016-01-01",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {}
  }
  ```

* **mainTemplate.json**: felhasználók telepíteni ezt a sablont hello létrehozása kezelt alkalmazás. Azt határozza meg a felügyelt hello alkalmazás erőforrás Microsoft.Solutions/appliances erőforrástípus. Ez a fájl tartalmazza az összes hello paraméter applianceMainTemplate.json hello erőforrásokra van szüksége.

  Ez a sablon két fontos tulajdonságok beállítása. Első lépésként hello **applianceDefinitionId** tulajdonsága felügyelt hello definíciót hello azonosítója. A témakör későbbi részében hello definition hoz létre. Ha az érték, ha el kell döntenie, melyik előfizetés és az erőforrás csoport toouse tárolására hello kezelt alkalmazás-definíciók. És el kell döntenie, a hello definíciójának neve. hello azonosító hello formátumban van:

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  Második, hello **managedResourceGroupId** tulajdonsága hello hello erőforrás-csoport azonosítója, ahol hello Azure-erőforrások jönnek létre. Ez az erőforráscsoport neve az értéket, vagy adjon meg egy nevet hello felhasználó. hello hello azonosító formátuma:

  `/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.

  a következő példa hello mainTemplate.json fájl jeleníti meg. Azt adja meg egy erőforráscsoportot hello telepített erőforrásokhoz. hello azonosító definíció nevű set toouse **storageApp** erőforráscsoportban nevű **managedApplicationGroup**. Ezen értékek toouse különböző nevét módosíthatja. Adja meg a saját előfizetés-azonosító a hello definíciójának azonosítója.

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "variables": {
        "managedRGId": "[concat(resourceGroup().id,'-application-resources')]",
        "managedAppName": "[concat('managedStorage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/appliances",
            "name": "[variables('managedAppName')]",
            "apiVersion": "2016-09-01-preview",
            "location": "[resourceGroup().location]",
            "kind": "ServiceCatalog",
            "properties": {
                "managedResourceGroupId": "[variables('managedRGId')]",
                "applianceDefinitionId": "/subscriptions/<subscription-id>/resourceGroups/managedApplicationGroup/providers/Microsoft.Solutions/applianceDefinitions/storageApp",
                "parameters": {
                    "storageAccountNamePrefix": {
                        "value": "[parameters('storageAccountNamePrefix')]"
                    }
                }
            }
        }
    ]
  }
  ```

* **applianceCreateUiDefinition.json**: hello Azure-portálon használ a fájl toogenerate hello felhasználói felület létrehozó felhasználók a felügyelt alkalmazási hello. Azt határozza meg, hogyan felhasználói adatbevitelt mindegyik paraméterhez. Beállítások is használhat, például a legördülő listából válassza ki, szövegmezőben, jelszó mezőbe, és más beviteli eszközök. Hogyan toocreate egy felhasználói felületi csomagdefiníciós fájl egy felügyelt alkalmazáshoz: toolearn [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).

  hello következő példa bemutatja egy applianceCreateUiDefinition.json fájl, amely lehetővé teszi, hogy a felhasználók toospecify hello tárolási fiók előtagja keresztül a szövegmezőben.

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {
                "name": "storageAccounts",
                "type": "Microsoft.Common.TextBox",
                "label": "Storage account name prefix",
                "defaultValue": "storage",
                "toolTip": "Provide a value that is used for hello prefix of your storage account. Limit too11 characters.",
                "constraints": {
                    "required": true,
                    "regex": "^[a-z0-9A-Z]{1,11}$",
                    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-11 characters long."
                },
                "visible": true
            }
        ],
        "steps": [],
        "outputs": {
            "storageAccountNamePrefix": "[basics('storageAccounts')]"
        }
    }
  }
  ```

Miután az összes szükséges hello fájlok készen áll, becsomagolja .zip-fájlként. hello három fájlt kell gyökérszinten hello hello .zip fájl. Ha azokat egy mappába helyezett, hibaüzenet hello létrehozása kezelt alkalmazás-definíciót, amely szerint az hello szükséges fájlok hiányoznak. Töltse fel a hello csomag tooan hozzáférhető hely, ahol képes használni. hello a cikk hátralévő része azt feltételezi, hogy hello .zip fájl megtalálható-e nyilvánosan elérhető tárolóra.

## <a name="create-an-azure-active-directory-user-group-or-application"></a>Egy Azure Active Directory felhasználói csoport vagy az alkalmazás létrehozása

hello második lépésben tooselect felhasználói csoport vagy az alkalmazás hello erőforrások kezelése hello ügyfél nevében. A felhasználói csoport vagy az alkalmazás rendelkezik engedélyekkel hello felügyelt erőforrás csoport függően toohello szerepkör, amely hozzá van rendelve. hello szerepkör lehet minden olyan beépített szerepköralapú hozzáférés-vezérlést (RBAC) szerepkört, például a tulajdonos vagy közreműködő szerepkörrel. Is adhat egy adott felhasználó engedély toomanage hello erőforrásokat, de általában rendelje hozzá a engedély tooa felhasználói csoport. toocreate egy új Active Directory-felhasználócsoportot, lásd: [hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](../active-directory/active-directory-groups-create-azure-portal.md).

Hello felhasználói csoport toouse hello Objektumazonosító szükséges hello erőforrások kezelése. hello a következő példa bemutatja, hogyan tooget hello Objektumazonosító hello csoport megjelenített neve:

```azurecli-interactive
az ad group show --group exampleGroupName
```

hello példaparancs adja vissza a következő kimeneti hello:

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

tooretrieve csak hello Objektumazonosító, használja:

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-hello-role-definition-id"></a>Szerepkör-definíció azonosítója hello beolvasása

A következő lépésben az RBAC beépített szerepkör toogrant hozzáférés toohello felhasználó, a felhasználói csoport vagy az alkalmazás kívánt hello hello szerepkör-definíció azonosítója. Általában akkor használják, hello tulajdonos vagy közreműködő vagy olvasó szerepkört. a következő parancs hello bemutatja, hogyan tooget hello hello tulajdonosi szerepkör szerepkör-definíció azonosítója:


```azurecli-interactive
az role definition list --name owner
```

Ez a parancs a következő kimeneti hello adja vissza:

```azurecli
{
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "properties": {
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything, including access tooresources.",
      "permissions": [
        {
          "actions": [
            "*"
         ],
         "notActions": []
        }
      ],
      "roleName": "Owner",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
}
```

A fenti példa hello hello "name" tulajdonság értékének hello van szüksége. Most, hogy a tulajdonság le:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-hello-managed-application-definition"></a>Hello felügyelt alkalmazás definíció létrehozása

Ha még nem rendelkezik egy erőforráscsoportot a kezelt alkalmazás definícióját tárolásához, hozzon létre egyet:

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

Ezután hozzon létre hello felügyelt alkalmazás definícióját erőforrás.

```azurecli-interactive
az managedapp definition create \
  --name storageApp \
  --location "westcentralus" \
  --resource-group managedApplicationGroup \
  --lock-level ReadOnly \
  --display-name myteststorageapp \
  --description storageapp \
  --authorizations "$groupid:$roleid" \
  --package-file-uri <uri-path-to-zip-file>
```

a fenti példa hello használt hello paraméterek a következők:

* **Erőforráscsoport**: hello nevét, ahol hello felügyelt definíciót hello erőforráscsoport létrejön.
* **zárolási szintű**: hello felügyelt erőforráscsoport hello típusú zárolást helyezni. Megakadályozza, hogy hello ügyfél nemkívánatos műveleteket végez az erőforráscsoport. Jelenleg csak olvasható hello csak akkor támogatja a zárolási szint. Ha meg van határozva a csak olvasható, hello felhasználói csak olvashatók hello erőforrások hello felügyelt erőforráscsoportban található.
* **engedélyek**: hello résztvevő-azonosító és hello szerepkör-definíció azonosítója, amelyek a felügyelt erőforrások toohello használt toogrant engedélycsoport ismerteti. Hello formátumban van megadva `<principalId>:<roleDefinitionId>`. Több érték is is meg kell adni ehhez a tulajdonsághoz. Ha több érték van szükség, akkor meg kell határozni hello formában `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Több érték is szóközzel elválasztva.
* **csomag – fájl-uri**: hello hello sablonfájlokat, amely lehet egy Azure Storage-blobot tartalmazó hello felügyelt alkalmazáscsomagot helyét.

## <a name="next-steps"></a>Következő lépések

* Egy bevezető toomanaged alkalmazások, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).
* Hello fájlok, tekintse meg a [felügyelt alkalmazás minták](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).
* Közzétételi kezelt alkalmazások toohello Azure piactér kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactér hello](managed-application-author-marketplace.md).
* További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).
* Hogyan toocreate egy felhasználói felületi csomagdefiníciós fájl egy felügyelt alkalmazáshoz: toolearn [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
