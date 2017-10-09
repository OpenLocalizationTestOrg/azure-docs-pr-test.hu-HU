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
# <a name="use-key-vault-toopass-secure-parameter-value-during-deployment"></a>Key Vault toopass biztonságos paraméter értékét használja a központi telepítése során

Központi telepítése során egy paraméterként kell toopass (például jelszót) egy biztonságos értékre, hello értéket le egy [Azure Key Vault](../key-vault/key-vault-whatis.md). Hello érték hello kulcstartó és a paraméter-fájlban a titkos kulcs hivatkozó lekérni. hello érték sosem hagyja, mivel csak hivatkoznak a kulcstartót azonosítóját. Nem kell toomanually meg hello hello titkos kulcsot minden egyes telepítéshez hello erőforrásokat. hello kulcstároló mint hello erőforráscsoport esetében helyez üzembe egy másik előfizetést is szerepel. Való hivatkozáskor hello kulcstároló, tartalmaz-e hello előfizetés-azonosító.

Hello kulcstartó létrehozásakor állítsa hello *enabledForTemplateDeployment* tulajdonság túl*igaz*. Az érték tootrue beállításával, hozzáférést engedélyez a Resource Manager-sablonok központi telepítése során.  

## <a name="deploy-a-key-vault-and-secret"></a>Telepítsen egy kulcstartót és a titkos kulcs

Kulcstároló toocreate és a titkos kulcsot, használja az Azure parancssori felület vagy a PowerShell. Figyelje meg, hogy hello kulcstároló sablon-üzembehelyezés engedélyezve van. 

Azure CLI esetén használja az alábbi parancsot:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

PowerShell esetén használja az alábbi parancsot:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-toohello-secret"></a>Engedélyezze a hozzáférést toohello titkos kulcs

Függetlenül attól, hogy egy új kulcstartó vagy egy meglévő, győződjön meg arról, hello sablon telepítése hello felhasználó hozzáférhet hello titkos kulcsot. a titkos kulcs hivatkozó sablonok telepítésével hello felhasználónak rendelkeznie kell hello `Microsoft.KeyVault/vaults/deploy/action` hello kulcstároló engedély. Hello [tulajdonos](../active-directory/role-based-access-built-in-roles.md#owner) és [közreműködő](../active-directory/role-based-access-built-in-roles.md#contributor) szerepkörök is engedélyezik a hozzáférést. Létrehozhat egy [egyéni szerepkör](../active-directory/role-based-access-control-custom-roles.md) , amely engedélyt ad a és hello felhasználói toothat szerepkört. További információ a felhasználói tooa szerepkör hozzáadása: [a felhasználó az Azure Active Directoryban tooadministrator szerepkörök hozzárendelése](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Hivatkozás statikus azonosítójú titkos kulcs

hello sablont, amely megkapja a kulcstartót titkos kulcs olyan, mint a többi sablont. Mivel ez **hello kulcstároló hello paraméter fájlban, nem hello sablon hivatkozik.** Például a következő sablon hello telepíti egy rendszergazdai jelszót tartalmazó SQL-adatbázis. hello a password paraméter értéke tooa biztonságos karakterláncot kell megadnia. De hello sablon nem adja meg, ha ezt az értéket származik.

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

Most létre sablon megelőző hello paraméter fájlt. Hello paraméterfájl adja meg egy paraméter, amely megfelel a hello paraméter hello sablonban hello neve. Hello paraméter értéke lásd a hello kulcstároló titkos hello. Hello titkos hello kulcstároló hello erőforrás-azonosítója és hello neve hello titok átadásával hivatkozik. A következő példa hello hello kulcstároló titkos kulcs már léteznie kell, és statikus értéket adja meg az erőforrás-azonosítóhoz.

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

## <a name="reference-a-secret-with-dynamic-id"></a>Dinamikus azonosítójú titkos kulcs hivatkozik

hello előző szakaszban bemutatta, hogyan toopass egy statikus erőforrás-azonosító hello kulcs tároló titkos kulcsot. Azonban bizonyos esetekben kell tooreference kulcstároló titkos kulcs eltérő hello jelenlegi üzemelő példány alapján. Ebben az esetben nem lehet hello paraméterfájl kódolnia hello erőforrás-azonosító. Sajnos nem lehet dinamikusan létrehozhat hello erőforrás-azonosító hello paraméterek fájlban mert sablon kifejezésekben nem engedélyezettek a hello paraméterfájl.

toodynamically hello erőforrás-azonosító kulcstároló titkos kulcs létrehozása, hello erőforrás, amelyet a beágyazott sablonba hello titkos kulcsot kell áthelyeznie. A fő sablonban hello beágyazott sablon hozzáadása, és adjon át egy paraméter, amely tartalmazza a dinamikusan generált hello erőforrás-azonosító.

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

## <a name="next-steps"></a>Következő lépések
* Kulcstároló kapcsolatos általános információkért lásd: [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).
* A titkos kulcs hivatkozó teljes példákért lásd [Key Vault példák](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

