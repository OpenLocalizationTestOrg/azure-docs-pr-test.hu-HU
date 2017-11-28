---
title: Key Vault titkos Resource Manager-sablonnal |} Microsoft Docs
description: "Bemutatja, hogyan adhatók át a titkos kulcs kulcstároló paraméterként üzembe helyezése során."
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
ms.openlocfilehash: 1ca72599e67e79d42a3d430dbb13e89ea7265334
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a><span data-ttu-id="e6a91-103">Továbbítsa biztonságos paraméter értékét a telepítés során a Key Vault használatával</span><span class="sxs-lookup"><span data-stu-id="e6a91-103">Use Key Vault to pass secure parameter value during deployment</span></span>

<span data-ttu-id="e6a91-104">Adjon át egy biztonságos értéket (például jelszót) paraméterként központi telepítése során kell, ha az érték le egy [Azure Key Vault](../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e6a91-104">When you need to pass a secure value (like a password) as a parameter during deployment, you can retrieve the value from an [Azure Key Vault](../key-vault/key-vault-whatis.md).</span></span> <span data-ttu-id="e6a91-105">A kulcstartó és a paraméter-fájlban a titkos kulcs Vezérlőpultjának kikeresheti az értéket.</span><span class="sxs-lookup"><span data-stu-id="e6a91-105">You retrieve the value by referencing the key vault and secret in your parameter file.</span></span> <span data-ttu-id="e6a91-106">Az érték sosem hagyja, mert csak hivatkoznak a kulcstartót azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="e6a91-106">The value is never exposed because you only reference its key vault ID.</span></span> <span data-ttu-id="e6a91-107">Nem kell manuálisan adja meg az értékét a titkos kulcsot minden alkalommal, amikor az erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="e6a91-107">You do not need to manually enter the value for the secret each time you deploy the resources.</span></span> <span data-ttu-id="e6a91-108">A key vault létezhet, mint az erőforráscsoportot, hogy telepít egy másik előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="e6a91-108">The key vault can exist in a different subscription than the resource group you are deploying to.</span></span> <span data-ttu-id="e6a91-109">A key vault hivatkozik, akkor adja meg az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="e6a91-109">When referencing the key vault, you include the subscription ID.</span></span>

<span data-ttu-id="e6a91-110">A kulcstartó létrehozásakor állítsa be a *enabledForTemplateDeployment* tulajdonságot *igaz*.</span><span class="sxs-lookup"><span data-stu-id="e6a91-110">When creating the key vault, set the *enabledForTemplateDeployment* property to *true*.</span></span> <span data-ttu-id="e6a91-111">Ez az érték true értékre állításával lehetővé hozzáférési Resource Manager-sablonok alapján a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="e6a91-111">By setting this value to true, you permit access from Resource Manager templates during deployment.</span></span>  

## <a name="deploy-a-key-vault-and-secret"></a><span data-ttu-id="e6a91-112">Telepítsen egy kulcstartót és a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="e6a91-112">Deploy a key vault and secret</span></span>

<span data-ttu-id="e6a91-113">A kulcstartó és a titkos kulcs létrehozásához használja az Azure parancssori felület vagy a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6a91-113">To create a key vault and secret, use either Azure CLI or PowerShell.</span></span> <span data-ttu-id="e6a91-114">Figyelje meg, hogy a key vault sablon-üzembehelyezés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="e6a91-114">Notice that the key vault is enabled for template deployment.</span></span> 

<span data-ttu-id="e6a91-115">Azure CLI esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="e6a91-115">For Azure CLI, use:</span></span>

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

<span data-ttu-id="e6a91-116">PowerShell esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="e6a91-116">For PowerShell, use:</span></span>

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a><span data-ttu-id="e6a91-117">A titkos kulcs hozzáférésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e6a91-117">Enable access to the secret</span></span>

<span data-ttu-id="e6a91-118">Függetlenül attól, hogy egy új kulcstartó vagy egy meglévő, győződjön meg arról, hogy a sablon telepítése a felhasználó hozzáférhet-e a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e6a91-118">Whether you are using a new key vault or an existing one, ensure that the user deploying the template can access the secret.</span></span> <span data-ttu-id="e6a91-119">A felhasználónak, a titkos kulcs hivatkozó sablonok telepítésével kell rendelkeznie a `Microsoft.KeyVault/vaults/deploy/action` a key vault engedély.</span><span class="sxs-lookup"><span data-stu-id="e6a91-119">The user deploying a template that references a secret must have the `Microsoft.KeyVault/vaults/deploy/action` permission for the key vault.</span></span> <span data-ttu-id="e6a91-120">A [tulajdonos](../active-directory/role-based-access-built-in-roles.md#owner) és [közreműködő](../active-directory/role-based-access-built-in-roles.md#contributor) szerepkörök is engedélyezik a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="e6a91-120">The [Owner](../active-directory/role-based-access-built-in-roles.md#owner) and [Contributor](../active-directory/role-based-access-built-in-roles.md#contributor) roles both grant this access.</span></span> <span data-ttu-id="e6a91-121">Létrehozhat egy [egyéni szerepkör](../active-directory/role-based-access-control-custom-roles.md) , amely engedélyt ad a, és adja hozzá a felhasználót a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="e6a91-121">You can also create a [custom role](../active-directory/role-based-access-control-custom-roles.md) that grants this permission and add the user to that role.</span></span> <span data-ttu-id="e6a91-122">A felhasználó egy szerepkörhöz történő hozzáadásával kapcsolatos további információkért lásd: [felhasználó hozzárendelése az Azure Active Directory rendszergazdai szerepkörök](../active-directory/active-directory-users-assign-role-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e6a91-122">For information about adding a user to a role, see [Assign a user to administrator roles in Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span></span>


## <a name="reference-a-secret-with-static-id"></a><span data-ttu-id="e6a91-123">Hivatkozás statikus azonosítójú titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="e6a91-123">Reference a secret with static ID</span></span>

<span data-ttu-id="e6a91-124">A sablont, amely megkapja a kulcstartót titkos kulcs olyan, mint a többi sablont.</span><span class="sxs-lookup"><span data-stu-id="e6a91-124">The template that receives a key vault secret is like any other template.</span></span> <span data-ttu-id="e6a91-125">Mivel ez **a paraméter fájlban, nem a sablon a key vault hivatkozik.**</span><span class="sxs-lookup"><span data-stu-id="e6a91-125">That's because **you reference the key vault in the parameter file, not the template.**</span></span> <span data-ttu-id="e6a91-126">Például a következő sablon telepít egy SQL-adatbázis, amely egy rendszergazdai jelszót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e6a91-126">For example, the following template deploys a SQL database that includes an administrator password.</span></span> <span data-ttu-id="e6a91-127">A password paraméter értéke egy biztonságos karakterláncot kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="e6a91-127">The password parameter is set to a secure string.</span></span> <span data-ttu-id="e6a91-128">De a sablon nem adja meg, ha ezt az értéket származik.</span><span class="sxs-lookup"><span data-stu-id="e6a91-128">But, the template does not specify where that value comes from.</span></span>

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

<span data-ttu-id="e6a91-129">Most a fenti sablon paraméter fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e6a91-129">Now, create a parameter file for the preceding template.</span></span> <span data-ttu-id="e6a91-130">A paraméterfájl adja meg a sablon a paraméter neve megegyezik-e paraméter.</span><span class="sxs-lookup"><span data-stu-id="e6a91-130">In the parameter file, specify a parameter that matches the name of the parameter in the template.</span></span> <span data-ttu-id="e6a91-131">A paraméterérték használatával hivatkozik a key vault a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e6a91-131">For the parameter value, reference the secret from the key vault.</span></span> <span data-ttu-id="e6a91-132">A titkos kulcsot úgy, hogy a key vault erőforrás-azonosítója és a titkos kulcs neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e6a91-132">You reference the secret by passing the resource identifier of the key vault and the name of the secret.</span></span> <span data-ttu-id="e6a91-133">A következő példában a kulcstartót titkos kulcs már léteznie kell, és adjon meg statikus értéket az erőforrás-azonosítóhoz.</span><span class="sxs-lookup"><span data-stu-id="e6a91-133">In the following example, the key vault secret must already exist, and you provide a static value for its resource ID.</span></span>

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

## <a name="reference-a-secret-with-dynamic-id"></a><span data-ttu-id="e6a91-134">Dinamikus azonosítójú titkos kulcs hivatkozik</span><span class="sxs-lookup"><span data-stu-id="e6a91-134">Reference a secret with dynamic ID</span></span>

<span data-ttu-id="e6a91-135">Az előző szakaszban bemutatta, hogyan felelt meg a kulcstartót titkos kulcsot egy statikus erőforrás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e6a91-135">The previous section showed how to pass a static resource ID for the key vault secret.</span></span> <span data-ttu-id="e6a91-136">Azonban bizonyos esetekben kell kulcstároló titkos kulcs eltérő az aktuális telepítés alapján hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e6a91-136">However, in some scenarios, you need to reference a key vault secret that varies based on the current deployment.</span></span> <span data-ttu-id="e6a91-137">Ebben az esetben nem lehet szigorú kódot az erőforrás-azonosítója a paraméterfájlban.</span><span class="sxs-lookup"><span data-stu-id="e6a91-137">In that case, you cannot hard-code the resource ID in the parameters file.</span></span> <span data-ttu-id="e6a91-138">Sajnos nem lehet dinamikusan létrehozhat az erőforrás-azonosítója a paraméterfájlban mert sablon kifejezések nem megengedettek a paraméterfájlban.</span><span class="sxs-lookup"><span data-stu-id="e6a91-138">Unfortunately, you cannot dynamically generate the resource ID in the parameters file because template expressions are not permitted in the parameters file.</span></span>

<span data-ttu-id="e6a91-139">Lehet dinamikusan létrehozni a kulcstartó titkos kulcs az erőforrás-azonosító, át kell helyezni az erőforrás, amelyet a titkos kulcsot egy beágyazott sablonba.</span><span class="sxs-lookup"><span data-stu-id="e6a91-139">To dynamically generate the resource ID for a key vault secret, you must move the resource that needs the secret into a nested template.</span></span> <span data-ttu-id="e6a91-140">A fő sablonban a beágyazott sablon hozzáadása, és adjon át egy paraméter, amely tartalmazza a dinamikusan generált erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="e6a91-140">In your master template, you add the nested template and pass in a parameter that contains the dynamically generated resource ID.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e6a91-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6a91-141">Next steps</span></span>
* <span data-ttu-id="e6a91-142">Kulcstároló kapcsolatos általános információkért lásd: [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e6a91-142">For general information about key vaults, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>
* <span data-ttu-id="e6a91-143">A titkos kulcs hivatkozó teljes példákért lásd [Key Vault példák](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span><span class="sxs-lookup"><span data-stu-id="e6a91-143">For complete examples of referencing key secrets, see [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span></span>

