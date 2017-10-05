---
title: "Hozzon létre, és az Azure szolgáltatás katalógus által felügyelt alkalmazások közzététele |} Microsoft Docs"
description: "Bemutatja, hogyan hozzon létre egy Azure által felügyelt alkalmazás, amely tagja a szervezet számára készült."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 39b74984ec2f89ed39753963de7fe3ff79577c9e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>A belső felhasználásához kezelt alkalmazás közzététele

Létrehozhat és közzététele az Azure [kezelt alkalmazások](managed-application-overview.md) szolgálnak, hogy a szervezet tagjaira. Például hogy az informatikai részleg, amelyek biztosítják a vállalati szabványoknak való megfelelés felügyelt alkalmazások közzététele. A kezelt alkalmazások a szolgáltatáskatalógus, nem az Azure piactéren keresztül érhetők el.

A szolgáltatáskatalógus a kezelt alkalmazás közzététele a következőket kell tennie:

* Hozzon létre három sablon szükséges fájlokat tartalmazó .zip-csomagja.
* Döntse el, mely felhasználó, csoport vagy alkalmazás hozzá kell férnie az erőforráscsoport, a felhasználó az előfizetéshez.
* Hozzon létre a kezelt alkalmazás-definíciót, amely a .zip-csomagja mutat, és az identitás hozzáférést kér.

## <a name="create-a-managed-application-package"></a>Kezelt alkalmazás-csomag létrehozása

Az első lépés, ha a három kötelező sablonfájlokat importálni. Mindhárom fájlt csomagot .zip fájlba, majd töltse fel az elérhető helyen, például egy tárfiókot. A kezelt alkalmazás definíciójának létrehozásakor hivatkozás átadása a .zip fájl.

* **applianceMainTemplate.json**: ezt a fájlt a kezelt alkalmazás részeként határozza meg az Azure-erőforrások törlődnek. A sablon nem eltér a normál Resource Manager-sablon. Például egy tárfiókot, a kezelt alkalmazás létrehozásához applianceMainTemplate.json tartalmazza:

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

* **mainTemplate.json**: felhasználók telepíteni ezt a sablont a kezelt alkalmazás létrehozásakor. Meghatározza a kezelt alkalmazás erőforrás, amely Microsoft.Solutions/appliances erőforrástípus. Ez a fájl tartalmazza a paraméterek applianceMainTemplate.json erőforrásokra van szüksége.

  Ez a sablon két fontos tulajdonságok beállítása. Első, a **applianceDefinitionId** tulajdonság a kezelt alkalmazás-definíció azonosítója. A témakör későbbi részében hoz létre a definíciót. Ha az érték határozza meg, melyik előfizetésbe és erőforráscsoportba csoportot a kezelt alkalmazás-definíciók tárolására használandó. És meg kell határoznia egy nevet a definíciójának. Az azonosítója a következő formátumban:

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  Második, a **managedResourceGroupId** tulajdonság értéke a az erőforrás-csoport azonosítója, ahol az Azure-erőforrások jönnek létre. Ez az erőforráscsoport neve az értéket, vagy adjon meg egy nevet a felhasználó. Az azonosító formátuma:

  `/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.

  A következő példa bemutatja egy mainTemplate.json fájlt. Azt adja meg egy erőforráscsoportot a telepített erőforrások. A definíció azonosítója nevű definíció használatára van beállítva **storageApp** erőforráscsoportban nevű **managedApplicationGroup**. Ezek helyett különböző nevekkel. Adja meg a saját előfizetés-azonosító található a definíció azonosítóját.

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

* **applianceCreateUiDefinition.json**: az Azure portál használja ezt a fájlt létrehozni a felhasználói felület, a felhasználók számára a kezelt alkalmazás létrehozása. Azt határozza meg, hogyan felhasználói adatbevitelt mindegyik paraméterhez. Beállítások is használhat, például a legördülő listából válassza ki, szövegmezőben, jelszó mezőbe, és más beviteli eszközök. A felhasználói felület csomagdefiníciós fájl egy felügyelt alkalmazás létrehozásához, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).

  A következő példa bemutatja egy applianceCreateUiDefinition.json fájl, amely lehetővé teszi, hogy a felhasználók számára a tárolási fiók előtagja keresztül a szövegmezőben adja meg.

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
                "toolTip": "Provide a value that is used for the prefix of your storage account. Limit to 11 characters.",
                "constraints": {
                    "required": true,
                    "regex": "^[a-z0-9A-Z]{1,11}$",
                    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-11 characters long."
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

Után készen áll a szükséges fájlokat, becsomagolja .zip-fájlként. A három fájlt kell a gyökérszinten .zip fájl. Ha azokat egy mappába helyezett, hibaüzenet arról, hogy a szükséges fájlok hiányoznak a kezelt alkalmazás definíciójának létrehozásakor. Töltse fel a csomag elérhető helyen a ahol képes használni. Ez a cikk fennmaradó azt feltételezi, hogy a .zip fájl megtalálható-e nyilvánosan elérhető tárolóra.

## <a name="create-an-azure-active-directory-user-group-or-application"></a>Egy Azure Active Directory felhasználói csoport vagy az alkalmazás létrehozása

A második lépés a felhasználói csoport vagy az erőforrások kezelése az ügyfél nevében az alkalmazás kiválasztása. A felhasználói csoport vagy az alkalmazás engedélyekkel rendelkezzen a felügyelt erőforráscsoporthoz rendelt szerepkör alapján. A szerepkör lehet minden olyan beépített szerepköralapú hozzáférés-vezérlést (RBAC) szerepkört, például a tulajdonos vagy közreműködő szerepkörrel. Is egy adott felhasználó engedélyt adhat a erőforrások kezeléséhez, de általában ez az engedély hozzárendelése egy felhasználói csoportot. Hozzon létre egy új Active Directory-felhasználócsoportot, lásd: [hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](../active-directory/active-directory-groups-create-azure-portal.md).

Az Objektumazonosító, a felhasználói csoport számára az erőforrások kezelése van szüksége. A következő példa bemutatja, hogyan objektum lekérése a csoporthoz tartozó megjelenített név:

```azurecli-interactive
az ad group show --group exampleGroupName
```

A példában a parancs a következő eredményt adja vissza:

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

Az Objektumazonosító lekéréséhez használja:

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-the-role-definition-id"></a>A szerepkör-definíció azonosítója beolvasása

A következő lépésben azt szeretné, hogy hozzáférést biztosítson a felhasználó, a felhasználói csoport vagy az alkalmazás RBAC beépített szerepkör szerepkör-definíció azonosítója. Általában akkor használják a tulajdonos vagy közreműködő vagy olvasó szerepkört. A következő parancsot a szerepkör-definíció azonosítója lekérése a tulajdonosi szerepkört mutatja be:


```azurecli-interactive
az role definition list --name owner
```

Ez a parancs a következő kimeneti adja vissza:

```azurecli
{
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "properties": {
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything, including access to resources.",
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

A "name" tulajdonság az előző példából van szüksége. Most, hogy a tulajdonság le:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-the-managed-application-definition"></a>A kezelt alkalmazás-definíció létrehozása

Ha még nem rendelkezik egy erőforráscsoportot a kezelt alkalmazás definícióját tárolásához, hozzon létre egyet:

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

Most hozzon létre a kezelt alkalmazás definícióját erőforrást.

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

Az előző példában használt paraméterek a következők:

* **Erőforráscsoport**: a kezelt alkalmazás definícióját létrehozási helyének erőforráscsoport nevét.
* **zárolási szintű**: zárolási típusú helyezve a felügyelt erőforráscsoportot. Megakadályozza, hogy az ügyfél nemkívánatos műveleteket végez az erőforráscsoport. Csak olvasható jelenleg az egyetlen támogatott zárolási szint. Csak olvasható megadása esetén az ügyfél csak megtekintheti ezeket az erőforrásokat a felügyelt erőforráscsoportban található.
* **engedélyek**: a résztvevő-azonosító és a szerepkör-definíció azonosítója, amely engedélyt ad a felügyelt erőforráscsoport segítségével mutatja be. Formátumban van megadva `<principalId>:<roleDefinitionId>`. Több érték is is meg kell adni ehhez a tulajdonsághoz. Ha több érték van szükség, akkor meg kell határozni a képernyőn `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Több érték is szóközzel elválasztva.
* **csomag – fájl-uri**: a kezelt alkalmazás csomagot, amely tartalmazza a sablonfájlokat importálni, amely lehet egy Azure Storage-blobba helyét.

## <a name="next-steps"></a>Következő lépések

* Felügyelt alkalmazások bemutatása, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).
* A fájlok, tekintse meg a [felügyelt alkalmazás minták](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).
* Felügyelt alkalmazások közzétételéhez az Azure piactéren kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactéren](managed-application-author-marketplace.md).
* További információ a piactérről kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactéren](managed-application-consume-marketplace.md).
* A felhasználói felület csomagdefiníciós fájl egy felügyelt alkalmazás létrehozásához, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
