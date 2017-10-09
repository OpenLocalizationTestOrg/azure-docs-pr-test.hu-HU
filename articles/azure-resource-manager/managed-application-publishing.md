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
# <a name="publish-a-managed-application-for-internal-consumption"></a><span data-ttu-id="6e4db-103">A belső felhasználásához kezelt alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="6e4db-103">Publish a managed application for internal consumption</span></span>

<span data-ttu-id="6e4db-104">Létrehozhat és közzététele az Azure [kezelt alkalmazások](managed-application-overview.md) szolgálnak, hogy a szervezet tagjaira.</span><span class="sxs-lookup"><span data-stu-id="6e4db-104">You can create and publish Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="6e4db-105">Például hogy az informatikai részleg, amelyek biztosítják a vállalati szabványoknak való megfelelés felügyelt alkalmazások közzététele.</span><span class="sxs-lookup"><span data-stu-id="6e4db-105">For example, an IT department can publish managed applications that ensure compliance with organizational standards.</span></span> <span data-ttu-id="6e4db-106">A kezelt alkalmazások hello szolgáltatáskatalógus, nem hello Azure piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6e4db-106">These managed applications are available through hello service catalog, not hello Azure Marketplace.</span></span>

<span data-ttu-id="6e4db-107">toopublish hello szolgáltatási katalógusa kezelt alkalmazás, kell:</span><span class="sxs-lookup"><span data-stu-id="6e4db-107">toopublish a managed application for hello service catalog, you must:</span></span>

* <span data-ttu-id="6e4db-108">Hozzon létre egy hello három sablon szükséges fájlokat tartalmazó .zip-csomagja.</span><span class="sxs-lookup"><span data-stu-id="6e4db-108">Create a .zip package that contains hello three required template files.</span></span>
* <span data-ttu-id="6e4db-109">Döntse el, mely felhasználó, csoport vagy alkalmazás toohello erőforráscsoport hello felhasználó az előfizetéshez kell hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="6e4db-109">Decide which user, group, or application needs access toohello resource group in hello user's subscription.</span></span>
* <span data-ttu-id="6e4db-110">Hozzon létre felügyelt hello definíciót, amely toohello .zip-csomagja mutat, és hello identitás hozzáférést kér.</span><span class="sxs-lookup"><span data-stu-id="6e4db-110">Create hello managed application definition that points toohello .zip package and requests access for hello identity.</span></span>

## <a name="create-a-managed-application-package"></a><span data-ttu-id="6e4db-111">Kezelt alkalmazás-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e4db-111">Create a managed application package</span></span>

<span data-ttu-id="6e4db-112">hello első lépéseként toocreate hello három szükséges sablont fájl.</span><span class="sxs-lookup"><span data-stu-id="6e4db-112">hello first step is toocreate hello three required template files.</span></span> <span data-ttu-id="6e4db-113">Mindhárom fájlt csomagot .zip fájlba, majd töltse fel az tooan elérhető helyre, például egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="6e4db-113">Package all three files into a .zip file, and upload it tooan accessible location, such as a storage account.</span></span> <span data-ttu-id="6e4db-114">A hivatkozás toothis .zip fájl létrehozása hello kezelt definíciót adja át.</span><span class="sxs-lookup"><span data-stu-id="6e4db-114">You pass a link toothis .zip file when creating hello managed application definition.</span></span>

* <span data-ttu-id="6e4db-115">**applianceMainTemplate.json**: A fájl határozza meg a hello Azure által hello részeként telepített erőforrások felügyelt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6e4db-115">**applianceMainTemplate.json**: This file defines hello Azure resources that are provisioned as part of hello managed application.</span></span> <span data-ttu-id="6e4db-116">hello sablon ugyanolyan helyzetet teremt, mint egy normál Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="6e4db-116">hello template is no different than a regular Resource Manager template.</span></span> <span data-ttu-id="6e4db-117">Például egy tárfiókot, felügyelt alkalmazás segítségével toocreate, applianceMainTemplate.json tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6e4db-117">For example, toocreate a storage account through a managed application, applianceMainTemplate.json contains:</span></span>

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

* <span data-ttu-id="6e4db-118">**mainTemplate.json**: felhasználók telepíteni ezt a sablont hello létrehozása kezelt alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6e4db-118">**mainTemplate.json**: Users deploy this template when creating hello managed application.</span></span> <span data-ttu-id="6e4db-119">Azt határozza meg a felügyelt hello alkalmazás erőforrás Microsoft.Solutions/appliances erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="6e4db-119">It defines hello managed application resource, which is a Microsoft.Solutions/appliances resource type.</span></span> <span data-ttu-id="6e4db-120">Ez a fájl tartalmazza az összes hello paraméter applianceMainTemplate.json hello erőforrásokra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6e4db-120">This file contains all hello parameters you need for hello resources in applianceMainTemplate.json.</span></span>

  <span data-ttu-id="6e4db-121">Ez a sablon két fontos tulajdonságok beállítása.</span><span class="sxs-lookup"><span data-stu-id="6e4db-121">You set two important properties in this template.</span></span> <span data-ttu-id="6e4db-122">Első lépésként hello **applianceDefinitionId** tulajdonsága felügyelt hello definíciót hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6e4db-122">First, hello **applianceDefinitionId** property is hello ID of hello managed application definition.</span></span> <span data-ttu-id="6e4db-123">A témakör későbbi részében hello definition hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6e4db-123">You create hello definition later in this topic.</span></span> <span data-ttu-id="6e4db-124">Ha az érték, ha el kell döntenie, melyik előfizetés és az erőforrás csoport toouse tárolására hello kezelt alkalmazás-definíciók.</span><span class="sxs-lookup"><span data-stu-id="6e4db-124">When setting this value, you must decide which subscription and resource group toouse for storing hello managed application definitions.</span></span> <span data-ttu-id="6e4db-125">És el kell döntenie, a hello definíciójának neve.</span><span class="sxs-lookup"><span data-stu-id="6e4db-125">And, you must decide on a name for hello definition.</span></span> <span data-ttu-id="6e4db-126">hello azonosító hello formátumban van:</span><span class="sxs-lookup"><span data-stu-id="6e4db-126">hello ID is in hello format:</span></span>

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  <span data-ttu-id="6e4db-127">Második, hello **managedResourceGroupId** tulajdonsága hello hello erőforrás-csoport azonosítója, ahol hello Azure-erőforrások jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="6e4db-127">Second, hello **managedResourceGroupId** property is hello ID of hello resource group where hello Azure resources are created.</span></span> <span data-ttu-id="6e4db-128">Ez az erőforráscsoport neve az értéket, vagy adjon meg egy nevet hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6e4db-128">You can assign a value for this resource group name or let hello user provide a name.</span></span> <span data-ttu-id="6e4db-129">hello hello azonosító formátuma:</span><span class="sxs-lookup"><span data-stu-id="6e4db-129">hello format of hello ID is:</span></span>

  <span data-ttu-id="6e4db-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span><span class="sxs-lookup"><span data-stu-id="6e4db-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span></span>

  <span data-ttu-id="6e4db-131">a következő példa hello mainTemplate.json fájl jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6e4db-131">hello following example shows a mainTemplate.json file.</span></span> <span data-ttu-id="6e4db-132">Azt adja meg egy erőforráscsoportot hello telepített erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6e4db-132">It specifies a resource group for hello deployed resources.</span></span> <span data-ttu-id="6e4db-133">hello azonosító definíció nevű set toouse **storageApp** erőforráscsoportban nevű **managedApplicationGroup**.</span><span class="sxs-lookup"><span data-stu-id="6e4db-133">hello definition ID is set toouse a definition named **storageApp** in a resource group named **managedApplicationGroup**.</span></span> <span data-ttu-id="6e4db-134">Ezen értékek toouse különböző nevét módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6e4db-134">You can change these values toouse different names.</span></span> <span data-ttu-id="6e4db-135">Adja meg a saját előfizetés-azonosító a hello definíciójának azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6e4db-135">Provide your own subscription ID in hello definition ID.</span></span>

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

* <span data-ttu-id="6e4db-136">**applianceCreateUiDefinition.json**: hello Azure-portálon használ a fájl toogenerate hello felhasználói felület létrehozó felhasználók a felügyelt alkalmazási hello.</span><span class="sxs-lookup"><span data-stu-id="6e4db-136">**applianceCreateUiDefinition.json**: hello Azure portal uses this file toogenerate hello user interface for users who create hello managed application.</span></span> <span data-ttu-id="6e4db-137">Azt határozza meg, hogyan felhasználói adatbevitelt mindegyik paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="6e4db-137">You define how users provide input for each parameter.</span></span> <span data-ttu-id="6e4db-138">Beállítások is használhat, például a legördülő listából válassza ki, szövegmezőben, jelszó mezőbe, és más beviteli eszközök.</span><span class="sxs-lookup"><span data-stu-id="6e4db-138">You can use options like a drop-down list, text box, password box, and other input tools.</span></span> <span data-ttu-id="6e4db-139">Hogyan toocreate egy felhasználói felületi csomagdefiníciós fájl egy felügyelt alkalmazáshoz: toolearn [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6e4db-139">toolearn how toocreate a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>

  <span data-ttu-id="6e4db-140">hello következő példa bemutatja egy applianceCreateUiDefinition.json fájl, amely lehetővé teszi, hogy a felhasználók toospecify hello tárolási fiók előtagja keresztül a szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="6e4db-140">hello following example shows an applianceCreateUiDefinition.json file that enables users toospecify hello storage account name prefix through a textbox.</span></span>

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

<span data-ttu-id="6e4db-141">Miután az összes szükséges hello fájlok készen áll, becsomagolja .zip-fájlként.</span><span class="sxs-lookup"><span data-stu-id="6e4db-141">After all hello needed files are ready, package them as a .zip file.</span></span> <span data-ttu-id="6e4db-142">hello három fájlt kell gyökérszinten hello hello .zip fájl.</span><span class="sxs-lookup"><span data-stu-id="6e4db-142">hello three files must be at hello root level of hello .zip file.</span></span> <span data-ttu-id="6e4db-143">Ha azokat egy mappába helyezett, hibaüzenet hello létrehozása kezelt alkalmazás-definíciót, amely szerint az hello szükséges fájlok hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="6e4db-143">If you put them in a folder, you receive an error when creating hello managed application definition that states hello required files are not present.</span></span> <span data-ttu-id="6e4db-144">Töltse fel a hello csomag tooan hozzáférhető hely, ahol képes használni.</span><span class="sxs-lookup"><span data-stu-id="6e4db-144">Upload hello package tooan accessible location from where it can be consumed.</span></span> <span data-ttu-id="6e4db-145">hello a cikk hátralévő része azt feltételezi, hogy hello .zip fájl megtalálható-e nyilvánosan elérhető tárolóra.</span><span class="sxs-lookup"><span data-stu-id="6e4db-145">hello remainder of this article assumes hello .zip file exists in a publicly accessible storage blob container.</span></span>

## <a name="create-an-azure-active-directory-user-group-or-application"></a><span data-ttu-id="6e4db-146">Egy Azure Active Directory felhasználói csoport vagy az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e4db-146">Create an Azure Active Directory user group or application</span></span>

<span data-ttu-id="6e4db-147">hello második lépésben tooselect felhasználói csoport vagy az alkalmazás hello erőforrások kezelése hello ügyfél nevében.</span><span class="sxs-lookup"><span data-stu-id="6e4db-147">hello second step is tooselect a user group or application for managing hello resources on behalf of hello customer.</span></span> <span data-ttu-id="6e4db-148">A felhasználói csoport vagy az alkalmazás rendelkezik engedélyekkel hello felügyelt erőforrás csoport függően toohello szerepkör, amely hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="6e4db-148">This user group or application has permissions on hello managed resource group according toohello role that is assigned.</span></span> <span data-ttu-id="6e4db-149">hello szerepkör lehet minden olyan beépített szerepköralapú hozzáférés-vezérlést (RBAC) szerepkört, például a tulajdonos vagy közreműködő szerepkörrel.</span><span class="sxs-lookup"><span data-stu-id="6e4db-149">hello role can be any built-in Role-Based Access Control (RBAC) role like Owner or Contributor.</span></span> <span data-ttu-id="6e4db-150">Is adhat egy adott felhasználó engedély toomanage hello erőforrásokat, de általában rendelje hozzá a engedély tooa felhasználói csoport.</span><span class="sxs-lookup"><span data-stu-id="6e4db-150">You also can give an individual user permission toomanage hello resources, but typically you assign this permission tooa user group.</span></span> <span data-ttu-id="6e4db-151">toocreate egy új Active Directory-felhasználócsoportot, lásd: [hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](../active-directory/active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6e4db-151">toocreate a new Active Directory user group, see [Create a group and add members in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span></span>

<span data-ttu-id="6e4db-152">Hello felhasználói csoport toouse hello Objektumazonosító szükséges hello erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="6e4db-152">You need hello object ID of hello user group toouse for managing hello resources.</span></span> <span data-ttu-id="6e4db-153">hello a következő példa bemutatja, hogyan tooget hello Objektumazonosító hello csoport megjelenített neve:</span><span class="sxs-lookup"><span data-stu-id="6e4db-153">hello following example shows how tooget hello object ID from hello group's display name:</span></span>

```azurecli-interactive
az ad group show --group exampleGroupName
```

<span data-ttu-id="6e4db-154">hello példaparancs adja vissza a következő kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="6e4db-154">hello example command returns hello following output:</span></span>

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

<span data-ttu-id="6e4db-155">tooretrieve csak hello Objektumazonosító, használja:</span><span class="sxs-lookup"><span data-stu-id="6e4db-155">tooretrieve just hello object ID, use:</span></span>

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-hello-role-definition-id"></a><span data-ttu-id="6e4db-156">Szerepkör-definíció azonosítója hello beolvasása</span><span class="sxs-lookup"><span data-stu-id="6e4db-156">Get hello role definition ID</span></span>

<span data-ttu-id="6e4db-157">A következő lépésben az RBAC beépített szerepkör toogrant hozzáférés toohello felhasználó, a felhasználói csoport vagy az alkalmazás kívánt hello hello szerepkör-definíció azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6e4db-157">Next, you need hello role definition ID of hello RBAC built-in role you want toogrant access toohello user, user group, or application.</span></span> <span data-ttu-id="6e4db-158">Általában akkor használják, hello tulajdonos vagy közreműködő vagy olvasó szerepkört.</span><span class="sxs-lookup"><span data-stu-id="6e4db-158">Typically, you use hello Owner or Contributor or Reader role.</span></span> <span data-ttu-id="6e4db-159">a következő parancs hello bemutatja, hogyan tooget hello hello tulajdonosi szerepkör szerepkör-definíció azonosítója:</span><span class="sxs-lookup"><span data-stu-id="6e4db-159">hello following command shows how tooget hello role definition ID for hello Owner role:</span></span>


```azurecli-interactive
az role definition list --name owner
```

<span data-ttu-id="6e4db-160">Ez a parancs a következő kimeneti hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="6e4db-160">That command returns hello following output:</span></span>

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

<span data-ttu-id="6e4db-161">A fenti példa hello hello "name" tulajdonság értékének hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6e4db-161">You need hello value of hello "name" property from hello preceding example.</span></span> <span data-ttu-id="6e4db-162">Most, hogy a tulajdonság le:</span><span class="sxs-lookup"><span data-stu-id="6e4db-162">You can retrieve just that property with:</span></span>

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-hello-managed-application-definition"></a><span data-ttu-id="6e4db-163">Hello felügyelt alkalmazás definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e4db-163">Create hello managed application definition</span></span>

<span data-ttu-id="6e4db-164">Ha még nem rendelkezik egy erőforráscsoportot a kezelt alkalmazás definícióját tárolásához, hozzon létre egyet:</span><span class="sxs-lookup"><span data-stu-id="6e4db-164">If you do not already have a resource group for storing your managed application definition, create one now:</span></span>

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

<span data-ttu-id="6e4db-165">Ezután hozzon létre hello felügyelt alkalmazás definícióját erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6e4db-165">Now, create hello managed application definition resource.</span></span>

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

<span data-ttu-id="6e4db-166">a fenti példa hello használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="6e4db-166">hello parameters used in hello preceding example are:</span></span>

* <span data-ttu-id="6e4db-167">**Erőforráscsoport**: hello nevét, ahol hello felügyelt definíciót hello erőforráscsoport létrejön.</span><span class="sxs-lookup"><span data-stu-id="6e4db-167">**resource-group**: hello name of hello resource group where hello managed application definition is created.</span></span>
* <span data-ttu-id="6e4db-168">**zárolási szintű**: hello felügyelt erőforráscsoport hello típusú zárolást helyezni.</span><span class="sxs-lookup"><span data-stu-id="6e4db-168">**lock-level**: hello type of lock placed on hello managed resource group.</span></span> <span data-ttu-id="6e4db-169">Megakadályozza, hogy hello ügyfél nemkívánatos műveleteket végez az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="6e4db-169">It prevents hello customer from performing undesirable operations on this resource group.</span></span> <span data-ttu-id="6e4db-170">Jelenleg csak olvasható hello csak akkor támogatja a zárolási szint.</span><span class="sxs-lookup"><span data-stu-id="6e4db-170">Currently, ReadOnly is hello only supported lock level.</span></span> <span data-ttu-id="6e4db-171">Ha meg van határozva a csak olvasható, hello felhasználói csak olvashatók hello erőforrások hello felügyelt erőforráscsoportban található.</span><span class="sxs-lookup"><span data-stu-id="6e4db-171">When ReadOnly is specified, hello customer can only read hello resources present in hello managed resource group.</span></span>
* <span data-ttu-id="6e4db-172">**engedélyek**: hello résztvevő-azonosító és hello szerepkör-definíció azonosítója, amelyek a felügyelt erőforrások toohello használt toogrant engedélycsoport ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6e4db-172">**authorizations**: Describes hello principal ID and hello role definition ID that are used toogrant permission toohello managed resource group.</span></span> <span data-ttu-id="6e4db-173">Hello formátumban van megadva `<principalId>:<roleDefinitionId>`.</span><span class="sxs-lookup"><span data-stu-id="6e4db-173">It's specified in hello format of `<principalId>:<roleDefinitionId>`.</span></span> <span data-ttu-id="6e4db-174">Több érték is is meg kell adni ehhez a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="6e4db-174">Multiple values also can be specified for this property.</span></span> <span data-ttu-id="6e4db-175">Ha több érték van szükség, akkor meg kell határozni hello formában `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span><span class="sxs-lookup"><span data-stu-id="6e4db-175">If multiple values are needed, they should be specified in hello form `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span></span> <span data-ttu-id="6e4db-176">Több érték is szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="6e4db-176">Multiple values are separated by a space.</span></span>
* <span data-ttu-id="6e4db-177">**csomag – fájl-uri**: hello hello sablonfájlokat, amely lehet egy Azure Storage-blobot tartalmazó hello felügyelt alkalmazáscsomagot helyét.</span><span class="sxs-lookup"><span data-stu-id="6e4db-177">**package-file-uri**: hello location of hello managed application package that contains hello template files, which can be an Azure Storage blob.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e4db-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e4db-178">Next steps</span></span>

* <span data-ttu-id="6e4db-179">Egy bevezető toomanaged alkalmazások, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6e4db-179">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="6e4db-180">Hello fájlok, tekintse meg a [felügyelt alkalmazás minták](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="6e4db-180">For examples of hello files, see [Managed application samples](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span></span>
* <span data-ttu-id="6e4db-181">További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="6e4db-181">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
* <span data-ttu-id="6e4db-182">Közzétételi kezelt alkalmazások toohello Azure piactér kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactér hello](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="6e4db-182">For information about publishing managed applications toohello Azure Marketplace, see [Azure managed applications in hello Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="6e4db-183">További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="6e4db-183">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="6e4db-184">Hogyan toocreate egy felhasználói felületi csomagdefiníciós fájl egy felügyelt alkalmazáshoz: toolearn [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6e4db-184">toolearn how toocreate a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
