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
# <a name="publish-a-managed-application-for-internal-consumption"></a><span data-ttu-id="ad2b7-103">A belső felhasználásához kezelt alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="ad2b7-103">Publish a managed application for internal consumption</span></span>

<span data-ttu-id="ad2b7-104">Létrehozhat és közzététele az Azure [kezelt alkalmazások](managed-application-overview.md) szolgálnak, hogy a szervezet tagjaira.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-104">You can create and publish Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="ad2b7-105">Például hogy az informatikai részleg, amelyek biztosítják a vállalati szabványoknak való megfelelés felügyelt alkalmazások közzététele.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-105">For example, an IT department can publish managed applications that ensure compliance with organizational standards.</span></span> <span data-ttu-id="ad2b7-106">A kezelt alkalmazások a szolgáltatáskatalógus, nem az Azure piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-106">These managed applications are available through the service catalog, not the Azure Marketplace.</span></span>

<span data-ttu-id="ad2b7-107">A szolgáltatáskatalógus a kezelt alkalmazás közzététele a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-107">To publish a managed application for the service catalog, you must:</span></span>

* <span data-ttu-id="ad2b7-108">Hozzon létre három sablon szükséges fájlokat tartalmazó .zip-csomagja.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-108">Create a .zip package that contains the three required template files.</span></span>
* <span data-ttu-id="ad2b7-109">Döntse el, mely felhasználó, csoport vagy alkalmazás hozzá kell férnie az erőforráscsoport, a felhasználó az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-109">Decide which user, group, or application needs access to the resource group in the user's subscription.</span></span>
* <span data-ttu-id="ad2b7-110">Hozzon létre a kezelt alkalmazás-definíciót, amely a .zip-csomagja mutat, és az identitás hozzáférést kér.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-110">Create the managed application definition that points to the .zip package and requests access for the identity.</span></span>

## <a name="create-a-managed-application-package"></a><span data-ttu-id="ad2b7-111">Kezelt alkalmazás-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad2b7-111">Create a managed application package</span></span>

<span data-ttu-id="ad2b7-112">Az első lépés, ha a három kötelező sablonfájlokat importálni.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-112">The first step is to create the three required template files.</span></span> <span data-ttu-id="ad2b7-113">Mindhárom fájlt csomagot .zip fájlba, majd töltse fel az elérhető helyen, például egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-113">Package all three files into a .zip file, and upload it to an accessible location, such as a storage account.</span></span> <span data-ttu-id="ad2b7-114">A kezelt alkalmazás definíciójának létrehozásakor hivatkozás átadása a .zip fájl.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-114">You pass a link to this .zip file when creating the managed application definition.</span></span>

* <span data-ttu-id="ad2b7-115">**applianceMainTemplate.json**: ezt a fájlt a kezelt alkalmazás részeként határozza meg az Azure-erőforrások törlődnek.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-115">**applianceMainTemplate.json**: This file defines the Azure resources that are provisioned as part of the managed application.</span></span> <span data-ttu-id="ad2b7-116">A sablon nem eltér a normál Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-116">The template is no different than a regular Resource Manager template.</span></span> <span data-ttu-id="ad2b7-117">Például egy tárfiókot, a kezelt alkalmazás létrehozásához applianceMainTemplate.json tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-117">For example, to create a storage account through a managed application, applianceMainTemplate.json contains:</span></span>

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

* <span data-ttu-id="ad2b7-118">**mainTemplate.json**: felhasználók telepíteni ezt a sablont a kezelt alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-118">**mainTemplate.json**: Users deploy this template when creating the managed application.</span></span> <span data-ttu-id="ad2b7-119">Meghatározza a kezelt alkalmazás erőforrás, amely Microsoft.Solutions/appliances erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-119">It defines the managed application resource, which is a Microsoft.Solutions/appliances resource type.</span></span> <span data-ttu-id="ad2b7-120">Ez a fájl tartalmazza a paraméterek applianceMainTemplate.json erőforrásokra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-120">This file contains all the parameters you need for the resources in applianceMainTemplate.json.</span></span>

  <span data-ttu-id="ad2b7-121">Ez a sablon két fontos tulajdonságok beállítása.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-121">You set two important properties in this template.</span></span> <span data-ttu-id="ad2b7-122">Első, a **applianceDefinitionId** tulajdonság a kezelt alkalmazás-definíció azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-122">First, the **applianceDefinitionId** property is the ID of the managed application definition.</span></span> <span data-ttu-id="ad2b7-123">A témakör későbbi részében hoz létre a definíciót.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-123">You create the definition later in this topic.</span></span> <span data-ttu-id="ad2b7-124">Ha az érték határozza meg, melyik előfizetésbe és erőforráscsoportba csoportot a kezelt alkalmazás-definíciók tárolására használandó.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-124">When setting this value, you must decide which subscription and resource group to use for storing the managed application definitions.</span></span> <span data-ttu-id="ad2b7-125">És meg kell határoznia egy nevet a definíciójának.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-125">And, you must decide on a name for the definition.</span></span> <span data-ttu-id="ad2b7-126">Az azonosítója a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-126">The ID is in the format:</span></span>

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  <span data-ttu-id="ad2b7-127">Második, a **managedResourceGroupId** tulajdonság értéke a az erőforrás-csoport azonosítója, ahol az Azure-erőforrások jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-127">Second, the **managedResourceGroupId** property is the ID of the resource group where the Azure resources are created.</span></span> <span data-ttu-id="ad2b7-128">Ez az erőforráscsoport neve az értéket, vagy adjon meg egy nevet a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-128">You can assign a value for this resource group name or let the user provide a name.</span></span> <span data-ttu-id="ad2b7-129">Az azonosító formátuma:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-129">The format of the ID is:</span></span>

  <span data-ttu-id="ad2b7-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span></span>

  <span data-ttu-id="ad2b7-131">A következő példa bemutatja egy mainTemplate.json fájlt.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-131">The following example shows a mainTemplate.json file.</span></span> <span data-ttu-id="ad2b7-132">Azt adja meg egy erőforráscsoportot a telepített erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-132">It specifies a resource group for the deployed resources.</span></span> <span data-ttu-id="ad2b7-133">A definíció azonosítója nevű definíció használatára van beállítva **storageApp** erőforráscsoportban nevű **managedApplicationGroup**.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-133">The definition ID is set to use a definition named **storageApp** in a resource group named **managedApplicationGroup**.</span></span> <span data-ttu-id="ad2b7-134">Ezek helyett különböző nevekkel.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-134">You can change these values to use different names.</span></span> <span data-ttu-id="ad2b7-135">Adja meg a saját előfizetés-azonosító található a definíció azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-135">Provide your own subscription ID in the definition ID.</span></span>

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

* <span data-ttu-id="ad2b7-136">**applianceCreateUiDefinition.json**: az Azure portál használja ezt a fájlt létrehozni a felhasználói felület, a felhasználók számára a kezelt alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-136">**applianceCreateUiDefinition.json**: The Azure portal uses this file to generate the user interface for users who create the managed application.</span></span> <span data-ttu-id="ad2b7-137">Azt határozza meg, hogyan felhasználói adatbevitelt mindegyik paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-137">You define how users provide input for each parameter.</span></span> <span data-ttu-id="ad2b7-138">Beállítások is használhat, például a legördülő listából válassza ki, szövegmezőben, jelszó mezőbe, és más beviteli eszközök.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-138">You can use options like a drop-down list, text box, password box, and other input tools.</span></span> <span data-ttu-id="ad2b7-139">A felhasználói felület csomagdefiníciós fájl egy felügyelt alkalmazás létrehozásához, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-139">To learn how to create a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>

  <span data-ttu-id="ad2b7-140">A következő példa bemutatja egy applianceCreateUiDefinition.json fájl, amely lehetővé teszi, hogy a felhasználók számára a tárolási fiók előtagja keresztül a szövegmezőben adja meg.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-140">The following example shows an applianceCreateUiDefinition.json file that enables users to specify the storage account name prefix through a textbox.</span></span>

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

<span data-ttu-id="ad2b7-141">Után készen áll a szükséges fájlokat, becsomagolja .zip-fájlként.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-141">After all the needed files are ready, package them as a .zip file.</span></span> <span data-ttu-id="ad2b7-142">A három fájlt kell a gyökérszinten .zip fájl.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-142">The three files must be at the root level of the .zip file.</span></span> <span data-ttu-id="ad2b7-143">Ha azokat egy mappába helyezett, hibaüzenet arról, hogy a szükséges fájlok hiányoznak a kezelt alkalmazás definíciójának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-143">If you put them in a folder, you receive an error when creating the managed application definition that states the required files are not present.</span></span> <span data-ttu-id="ad2b7-144">Töltse fel a csomag elérhető helyen a ahol képes használni.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-144">Upload the package to an accessible location from where it can be consumed.</span></span> <span data-ttu-id="ad2b7-145">Ez a cikk fennmaradó azt feltételezi, hogy a .zip fájl megtalálható-e nyilvánosan elérhető tárolóra.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-145">The remainder of this article assumes the .zip file exists in a publicly accessible storage blob container.</span></span>

## <a name="create-an-azure-active-directory-user-group-or-application"></a><span data-ttu-id="ad2b7-146">Egy Azure Active Directory felhasználói csoport vagy az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad2b7-146">Create an Azure Active Directory user group or application</span></span>

<span data-ttu-id="ad2b7-147">A második lépés a felhasználói csoport vagy az erőforrások kezelése az ügyfél nevében az alkalmazás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-147">The second step is to select a user group or application for managing the resources on behalf of the customer.</span></span> <span data-ttu-id="ad2b7-148">A felhasználói csoport vagy az alkalmazás engedélyekkel rendelkezzen a felügyelt erőforráscsoporthoz rendelt szerepkör alapján.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-148">This user group or application has permissions on the managed resource group according to the role that is assigned.</span></span> <span data-ttu-id="ad2b7-149">A szerepkör lehet minden olyan beépített szerepköralapú hozzáférés-vezérlést (RBAC) szerepkört, például a tulajdonos vagy közreműködő szerepkörrel.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-149">The role can be any built-in Role-Based Access Control (RBAC) role like Owner or Contributor.</span></span> <span data-ttu-id="ad2b7-150">Is egy adott felhasználó engedélyt adhat a erőforrások kezeléséhez, de általában ez az engedély hozzárendelése egy felhasználói csoportot.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-150">You also can give an individual user permission to manage the resources, but typically you assign this permission to a user group.</span></span> <span data-ttu-id="ad2b7-151">Hozzon létre egy új Active Directory-felhasználócsoportot, lásd: [hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](../active-directory/active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-151">To create a new Active Directory user group, see [Create a group and add members in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span></span>

<span data-ttu-id="ad2b7-152">Az Objektumazonosító, a felhasználói csoport számára az erőforrások kezelése van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-152">You need the object ID of the user group to use for managing the resources.</span></span> <span data-ttu-id="ad2b7-153">A következő példa bemutatja, hogyan objektum lekérése a csoporthoz tartozó megjelenített név:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-153">The following example shows how to get the object ID from the group's display name:</span></span>

```azurecli-interactive
az ad group show --group exampleGroupName
```

<span data-ttu-id="ad2b7-154">A példában a parancs a következő eredményt adja vissza:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-154">The example command returns the following output:</span></span>

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

<span data-ttu-id="ad2b7-155">Az Objektumazonosító lekéréséhez használja:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-155">To retrieve just the object ID, use:</span></span>

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-the-role-definition-id"></a><span data-ttu-id="ad2b7-156">A szerepkör-definíció azonosítója beolvasása</span><span class="sxs-lookup"><span data-stu-id="ad2b7-156">Get the role definition ID</span></span>

<span data-ttu-id="ad2b7-157">A következő lépésben azt szeretné, hogy hozzáférést biztosítson a felhasználó, a felhasználói csoport vagy az alkalmazás RBAC beépített szerepkör szerepkör-definíció azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-157">Next, you need the role definition ID of the RBAC built-in role you want to grant access to the user, user group, or application.</span></span> <span data-ttu-id="ad2b7-158">Általában akkor használják a tulajdonos vagy közreműködő vagy olvasó szerepkört.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-158">Typically, you use the Owner or Contributor or Reader role.</span></span> <span data-ttu-id="ad2b7-159">A következő parancsot a szerepkör-definíció azonosítója lekérése a tulajdonosi szerepkört mutatja be:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-159">The following command shows how to get the role definition ID for the Owner role:</span></span>


```azurecli-interactive
az role definition list --name owner
```

<span data-ttu-id="ad2b7-160">Ez a parancs a következő kimeneti adja vissza:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-160">That command returns the following output:</span></span>

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

<span data-ttu-id="ad2b7-161">A "name" tulajdonság az előző példából van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-161">You need the value of the "name" property from the preceding example.</span></span> <span data-ttu-id="ad2b7-162">Most, hogy a tulajdonság le:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-162">You can retrieve just that property with:</span></span>

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-the-managed-application-definition"></a><span data-ttu-id="ad2b7-163">A kezelt alkalmazás-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad2b7-163">Create the managed application definition</span></span>

<span data-ttu-id="ad2b7-164">Ha még nem rendelkezik egy erőforráscsoportot a kezelt alkalmazás definícióját tárolásához, hozzon létre egyet:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-164">If you do not already have a resource group for storing your managed application definition, create one now:</span></span>

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

<span data-ttu-id="ad2b7-165">Most hozzon létre a kezelt alkalmazás definícióját erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-165">Now, create the managed application definition resource.</span></span>

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

<span data-ttu-id="ad2b7-166">Az előző példában használt paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="ad2b7-166">The parameters used in the preceding example are:</span></span>

* <span data-ttu-id="ad2b7-167">**Erőforráscsoport**: a kezelt alkalmazás definícióját létrehozási helyének erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-167">**resource-group**: The name of the resource group where the managed application definition is created.</span></span>
* <span data-ttu-id="ad2b7-168">**zárolási szintű**: zárolási típusú helyezve a felügyelt erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-168">**lock-level**: The type of lock placed on the managed resource group.</span></span> <span data-ttu-id="ad2b7-169">Megakadályozza, hogy az ügyfél nemkívánatos műveleteket végez az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-169">It prevents the customer from performing undesirable operations on this resource group.</span></span> <span data-ttu-id="ad2b7-170">Csak olvasható jelenleg az egyetlen támogatott zárolási szint.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-170">Currently, ReadOnly is the only supported lock level.</span></span> <span data-ttu-id="ad2b7-171">Csak olvasható megadása esetén az ügyfél csak megtekintheti ezeket az erőforrásokat a felügyelt erőforráscsoportban található.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-171">When ReadOnly is specified, the customer can only read the resources present in the managed resource group.</span></span>
* <span data-ttu-id="ad2b7-172">**engedélyek**: a résztvevő-azonosító és a szerepkör-definíció azonosítója, amely engedélyt ad a felügyelt erőforráscsoport segítségével mutatja be.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-172">**authorizations**: Describes the principal ID and the role definition ID that are used to grant permission to the managed resource group.</span></span> <span data-ttu-id="ad2b7-173">Formátumban van megadva `<principalId>:<roleDefinitionId>`.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-173">It's specified in the format of `<principalId>:<roleDefinitionId>`.</span></span> <span data-ttu-id="ad2b7-174">Több érték is is meg kell adni ehhez a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-174">Multiple values also can be specified for this property.</span></span> <span data-ttu-id="ad2b7-175">Ha több érték van szükség, akkor meg kell határozni a képernyőn `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-175">If multiple values are needed, they should be specified in the form `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span></span> <span data-ttu-id="ad2b7-176">Több érték is szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-176">Multiple values are separated by a space.</span></span>
* <span data-ttu-id="ad2b7-177">**csomag – fájl-uri**: a kezelt alkalmazás csomagot, amely tartalmazza a sablonfájlokat importálni, amely lehet egy Azure Storage-blobba helyét.</span><span class="sxs-lookup"><span data-stu-id="ad2b7-177">**package-file-uri**: The location of the managed application package that contains the template files, which can be an Azure Storage blob.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad2b7-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad2b7-178">Next steps</span></span>

* <span data-ttu-id="ad2b7-179">Felügyelt alkalmazások bemutatása, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-179">For an introduction to managed applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="ad2b7-180">A fájlok, tekintse meg a [felügyelt alkalmazás minták](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-180">For examples of the files, see [Managed application samples](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span></span>
* <span data-ttu-id="ad2b7-181">További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-181">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
* <span data-ttu-id="ad2b7-182">Felügyelt alkalmazások közzétételéhez az Azure piactéren kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactéren](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-182">For information about publishing managed applications to the Azure Marketplace, see [Azure managed applications in the Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="ad2b7-183">További információ a piactérről kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactéren](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-183">For information about consuming a managed application from the Marketplace, see [Consume Azure managed applications in the Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="ad2b7-184">A felhasználói felület csomagdefiníciós fájl egy felügyelt alkalmazás létrehozásához, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b7-184">To learn how to create a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
