---
title: "aaaKey tároló titkos Resource Manager-sablonnal |} Microsoft Docs"
description: "Bemutatja, hogyan toopass egy titkos kulcsból érkező tároló paraméterként üzembe helyezése során."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 0bb7760c95b3b4ef34c9e5cc2e3421be56b5e5e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-key-vault-toopass-secure-parameter-value-during-deployment"></a><span data-ttu-id="ac186-103">Key Vault toopass biztonságos paraméter értékét használja a központi telepítése során</span><span class="sxs-lookup"><span data-stu-id="ac186-103">Use Key Vault toopass secure parameter value during deployment</span></span>

<span data-ttu-id="ac186-104">Központi telepítése során egy paraméterként kell toopass (például jelszót) egy biztonságos értékre, hello értéket le egy [Azure Key Vault](../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ac186-104">When you need toopass a secure value (like a password) as a parameter during deployment, you can retrieve hello value from an [Azure Key Vault](../key-vault/key-vault-whatis.md).</span></span> <span data-ttu-id="ac186-105">Hello érték hello kulcstartó és a paraméter-fájlban a titkos kulcs hivatkozó lekérni.</span><span class="sxs-lookup"><span data-stu-id="ac186-105">You retrieve hello value by referencing hello key vault and secret in your parameter file.</span></span> <span data-ttu-id="ac186-106">hello érték sosem hagyja, mivel csak hivatkoznak a kulcstartót azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ac186-106">hello value is never exposed because you only reference its key vault ID.</span></span> <span data-ttu-id="ac186-107">Nem kell toomanually meg hello hello titkos kulcsot minden egyes telepítéshez hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ac186-107">You do not need toomanually enter hello value for hello secret each time you deploy hello resources.</span></span> <span data-ttu-id="ac186-108">hello kulcstároló mint hello erőforráscsoport esetében helyez üzembe egy másik előfizetést is szerepel.</span><span class="sxs-lookup"><span data-stu-id="ac186-108">hello key vault can exist in a different subscription than hello resource group you are deploying to.</span></span> <span data-ttu-id="ac186-109">Való hivatkozáskor hello kulcstároló, tartalmaz-e hello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="ac186-109">When referencing hello key vault, you include hello subscription ID.</span></span>

<span data-ttu-id="ac186-110">Hello kulcstartó létrehozásakor állítsa hello *enabledForTemplateDeployment* tulajdonság túl*igaz*.</span><span class="sxs-lookup"><span data-stu-id="ac186-110">When creating hello key vault, set hello *enabledForTemplateDeployment* property too*true*.</span></span> <span data-ttu-id="ac186-111">Az érték tootrue beállításával, hozzáférést engedélyez a Resource Manager-sablonok központi telepítése során.</span><span class="sxs-lookup"><span data-stu-id="ac186-111">By setting this value tootrue, you permit access from Resource Manager templates during deployment.</span></span>  

## <a name="deploy-a-key-vault-and-secret"></a><span data-ttu-id="ac186-112">Telepítsen egy kulcstartót és a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="ac186-112">Deploy a key vault and secret</span></span>

<span data-ttu-id="ac186-113">Kulcstároló toocreate és a titkos kulcsot, használja az Azure parancssori felület vagy a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ac186-113">toocreate a key vault and secret, use either Azure CLI or PowerShell.</span></span> <span data-ttu-id="ac186-114">Figyelje meg, hogy hello kulcstároló sablon-üzembehelyezés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="ac186-114">Notice that hello key vault is enabled for template deployment.</span></span> 

<span data-ttu-id="ac186-115">Azure CLI esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="ac186-115">For Azure CLI, use:</span></span>

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

<span data-ttu-id="ac186-116">PowerShell esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="ac186-116">For PowerShell, use:</span></span>

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-toohello-secret"></a><span data-ttu-id="ac186-117">Engedélyezze a hozzáférést toohello titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="ac186-117">Enable access toohello secret</span></span>

<span data-ttu-id="ac186-118">Függetlenül attól, hogy egy új kulcstartó vagy egy meglévő, győződjön meg arról, hello sablon telepítése hello felhasználó hozzáférhet hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="ac186-118">Whether you are using a new key vault or an existing one, ensure that hello user deploying hello template can access hello secret.</span></span> <span data-ttu-id="ac186-119">a titkos kulcs hivatkozó sablonok telepítésével hello felhasználónak rendelkeznie kell hello `Microsoft.KeyVault/vaults/deploy/action` hello kulcstároló engedély.</span><span class="sxs-lookup"><span data-stu-id="ac186-119">hello user deploying a template that references a secret must have hello `Microsoft.KeyVault/vaults/deploy/action` permission for hello key vault.</span></span> <span data-ttu-id="ac186-120">Hello [tulajdonos](../active-directory/role-based-access-built-in-roles.md#owner) és [közreműködő](../active-directory/role-based-access-built-in-roles.md#contributor) szerepkörök is engedélyezik a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="ac186-120">hello [Owner](../active-directory/role-based-access-built-in-roles.md#owner) and [Contributor](../active-directory/role-based-access-built-in-roles.md#contributor) roles both grant this access.</span></span> <span data-ttu-id="ac186-121">Létrehozhat egy [egyéni szerepkör](../active-directory/role-based-access-control-custom-roles.md) , amely engedélyt ad a és hello felhasználói toothat szerepkört.</span><span class="sxs-lookup"><span data-stu-id="ac186-121">You can also create a [custom role](../active-directory/role-based-access-control-custom-roles.md) that grants this permission and add hello user toothat role.</span></span> <span data-ttu-id="ac186-122">További információ a felhasználói tooa szerepkör hozzáadása: [a felhasználó az Azure Active Directoryban tooadministrator szerepkörök hozzárendelése](../active-directory/active-directory-users-assign-role-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ac186-122">For information about adding a user tooa role, see [Assign a user tooadministrator roles in Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span></span>


## <a name="reference-a-secret-with-static-id"></a><span data-ttu-id="ac186-123">Hivatkozás statikus azonosítójú titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="ac186-123">Reference a secret with static ID</span></span>

<span data-ttu-id="ac186-124">hello sablont, amely megkapja a kulcstartót titkos kulcs olyan, mint a többi sablont.</span><span class="sxs-lookup"><span data-stu-id="ac186-124">hello template that receives a key vault secret is like any other template.</span></span> <span data-ttu-id="ac186-125">Mivel ez **hello kulcstároló hello paraméter fájlban, nem hello sablon hivatkozik.**</span><span class="sxs-lookup"><span data-stu-id="ac186-125">That's because **you reference hello key vault in hello parameter file, not hello template.**</span></span> <span data-ttu-id="ac186-126">Például a következő sablon hello telepíti egy rendszergazdai jelszót tartalmazó SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ac186-126">For example, hello following template deploys a SQL database that includes an administrator password.</span></span> <span data-ttu-id="ac186-127">hello a password paraméter értéke tooa biztonságos karakterláncot kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="ac186-127">hello password parameter is set tooa secure string.</span></span> <span data-ttu-id="ac186-128">De hello sablon nem adja meg, ha ezt az értéket származik.</span><span class="sxs-lookup"><span data-stu-id="ac186-128">But, hello template does not specify where that value comes from.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

<span data-ttu-id="ac186-129">Most létre sablon megelőző hello paraméter fájlt.</span><span class="sxs-lookup"><span data-stu-id="ac186-129">Now, create a parameter file for hello preceding template.</span></span> <span data-ttu-id="ac186-130">Hello paraméterfájl adja meg egy paraméter, amely megfelel a hello paraméter hello sablonban hello neve.</span><span class="sxs-lookup"><span data-stu-id="ac186-130">In hello parameter file, specify a parameter that matches hello name of hello parameter in hello template.</span></span> <span data-ttu-id="ac186-131">Hello paraméter értéke lásd a hello kulcstároló titkos hello.</span><span class="sxs-lookup"><span data-stu-id="ac186-131">For hello parameter value, reference hello secret from hello key vault.</span></span> <span data-ttu-id="ac186-132">Hello titkos hello kulcstároló hello erőforrás-azonosítója és hello neve hello titok átadásával hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ac186-132">You reference hello secret by passing hello resource identifier of hello key vault and hello name of hello secret.</span></span> <span data-ttu-id="ac186-133">A következő példa hello hello kulcstároló titkos kulcs már léteznie kell, és statikus értéket adja meg az erőforrás-azonosítóhoz.</span><span class="sxs-lookup"><span data-stu-id="ac186-133">In hello following example, hello key vault secret must already exist, and you provide a static value for its resource ID.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a><span data-ttu-id="ac186-134">Dinamikus azonosítójú titkos kulcs hivatkozik</span><span class="sxs-lookup"><span data-stu-id="ac186-134">Reference a secret with dynamic ID</span></span>

<span data-ttu-id="ac186-135">hello előző szakaszban bemutatta, hogyan toopass egy statikus erőforrás-azonosító hello kulcs tároló titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="ac186-135">hello previous section showed how toopass a static resource ID for hello key vault secret.</span></span> <span data-ttu-id="ac186-136">Azonban bizonyos esetekben kell tooreference kulcstároló titkos kulcs eltérő hello jelenlegi üzemelő példány alapján.</span><span class="sxs-lookup"><span data-stu-id="ac186-136">However, in some scenarios, you need tooreference a key vault secret that varies based on hello current deployment.</span></span> <span data-ttu-id="ac186-137">Ebben az esetben nem lehet hello paraméterfájl kódolnia hello erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="ac186-137">In that case, you cannot hard-code hello resource ID in hello parameters file.</span></span> <span data-ttu-id="ac186-138">Sajnos nem lehet dinamikusan létrehozhat hello erőforrás-azonosító hello paraméterek fájlban mert sablon kifejezésekben nem engedélyezettek a hello paraméterfájl.</span><span class="sxs-lookup"><span data-stu-id="ac186-138">Unfortunately, you cannot dynamically generate hello resource ID in hello parameters file because template expressions are not permitted in hello parameters file.</span></span>

<span data-ttu-id="ac186-139">toodynamically hello erőforrás-azonosító kulcstároló titkos kulcs létrehozása, hello erőforrás, amelyet a beágyazott sablonba hello titkos kulcsot kell áthelyeznie.</span><span class="sxs-lookup"><span data-stu-id="ac186-139">toodynamically generate hello resource ID for a key vault secret, you must move hello resource that needs hello secret into a nested template.</span></span> <span data-ttu-id="ac186-140">A fő sablonban hello beágyazott sablon hozzáadása, és adjon át egy paraméter, amely tartalmazza a dinamikusan generált hello erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="ac186-140">In your master template, you add hello nested template and pass in a parameter that contains hello dynamically generated resource ID.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a><span data-ttu-id="ac186-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac186-141">Next steps</span></span>
* <span data-ttu-id="ac186-142">Kulcstároló kapcsolatos általános információkért lásd: [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ac186-142">For general information about key vaults, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>
* <span data-ttu-id="ac186-143">A titkos kulcs hivatkozó teljes példákért lásd [Key Vault példák](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span><span class="sxs-lookup"><span data-stu-id="ac186-143">For complete examples of referencing key secrets, see [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span></span>

