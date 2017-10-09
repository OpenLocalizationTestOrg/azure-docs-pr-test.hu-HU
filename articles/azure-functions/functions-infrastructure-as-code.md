---
title: "az Azure Functions függvény alkalmazások aaaAutomate erőforrások telepítése |} Microsoft Docs"
description: "Megtudhatja, hogyan toobuild egy Azure Resource Manager-sablon, amely a függvény alkalmazást telepíti."
services: Functions
documtationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure funkciók, a Funkciók, a kiszolgáló nélküli architektúra, a kódot, az azure erőforrás-kezelő infrastruktúra"
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: b0df0d4ef9fe93213f7b1cb1d1e6b4e14f8b3a30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Az Azure Functions függvény alkalmazás erőforrás-telepítés automatizálása

Az Azure Resource Manager sablon toodeploy egy függvény alkalmazást is használhatja. Ez a cikk ismerteti a hello szükséges erőforrások és a paraméterek úgy. Szükség lehet további források toodeploy, attól függően, hogy hello [eseményindítók és kötések](functions-triggers-bindings.md) az függvény alkalmazásban.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

A minta-sablonok lásd:
- [függvény app a fogyasztás terv]
- [függvény alkalmazást az Azure App Service-csomag]

## <a name="required-resources"></a>Szükséges erőforrások

A függvény az alkalmazás csak ezeket az erőforrásokat:

* Egy [Azure Storage](../storage/index.md) fiók
* Üzemeltetési terv (fogyasztás terv vagy App Service-csomag)
* Egy függvény alkalmazást 

### <a name="storage-account"></a>Tárfiók

Egy Azure storage-fiók egy függvény app szükség. Általános célú fiók, amely támogatja a BLOB, táblák, üzenetsorok és fájlok szükséges. További információkért lásd: [Azure Functions tárolási fiókra vonatkozó követelmények](functions-create-function-app-portal.md#storage-account-requirements).

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

Emellett a Tulajdonságok hello `AzureWebJobsStorage` és `AzureWebJobsDashboard` Alkalmazásbeállítások hello hely konfigurációban kell megadni. hello Azure Functions futtatókörnyezete használja hello `AzureWebJobsStorage` kapcsolati karakterlánc toocreate belső várólisták. kapcsolati karakterlánc hello `AzureWebJobsDashboard` használt toolog tooAzure Table storage és a tápkábelek hello van **figyelő** hello portálon lapon.

Ezeket a tulajdonságokat meg van határozva a hello `appSettings` hello gyűjtemény `siteConfig` objektum:

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
```    

### <a name="hosting-plan"></a>Üzemeltetési terv

hello üzemeltetési terv meghatározásának hello függ, hogy használ-e a felhasználási vagy App Service-csomag. Lásd: [hello fogyasztás terven függvény alkalmazás telepítése](#consumption) és [függvény alkalmazás az App Service-csomag hello telepítése](#app-service-plan).

### <a name="function-app"></a>Függvény alkalmazás

hello függvény app erőforrás van definiálva típusú erőforrások használatával **Microsoft.Web/Site** és fajta **functionapp**:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ]
```

<a name="consumption"></a>

## <a name="deploy-a-function-app-on-hello-consumption-plan"></a>A hello fogyasztás terv függvény alkalmazás telepítése

Egy függvény alkalmazás két különböző módban is futtathatja: hello felhasználás a terv és az App Service-csomag hello. a kód fut-e, méretezi kimenő szükséges toohandle terhelés, és majd arányosan csökken, amikor a kód nem fut a hello fogyasztás terv automatikusan számítási teljesítményt foglal le. Igen toopay tétlen virtuális gépek nem rendelkezik, és tooreserve kapacitás nincs előre. További információ az üzemeltető terveket, toolearn lásd: [Azure Functions használat és az App Service csomagokban](functions-scale.md).

Lásd: a minta Azure Resource Manager sablon [függvény app a fogyasztás terv].

### <a name="create-a-consumption-plan"></a>Felhasználás terv létrehozása

A felhasználási terv "kiszolgálófarm" erőforrás különleges típusú. Megadja azt a hello `Dynamic` hello értéke `computeMode` és `sku` tulajdonságok:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

### <a name="create-a-function-app"></a>Függvényalkalmazás létrehozása

Emellett a fogyasztás terv kell hello Helykonfiguráció két további beállítások: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` és `WEBSITE_CONTENTSHARE`. Ezek a tulajdonságok konfigurálása hello tárolási fiók és a fájlelérési út hello függvény alkalmazáskódot és konfigurációs tároló.

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsDashboard",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~1"
                }
            ]
        }
    }
}
```                    

<a name="app-service-plan"></a> 

## <a name="deploy-a-function-app-on-hello-app-service-plan"></a>Az App Service-csomag hello függvény alkalmazás telepítése

Az App Service-csomag hello a függvény alkalmazás fut, a Basic, Standard és Premium termékváltozat hasonló tooweb alkalmazásokra dedikált virtuális gépeken. App Service-csomag hello működésével kapcsolatos részletekért lásd: hello [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Lásd: a minta Azure Resource Manager sablon [függvény alkalmazást az Azure App Service-csomag].

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a>Függvényalkalmazás létrehozása 

Miután kijelölt egy beállítást, a függvény-alkalmazás létrehozása. hello app hello tároló, amely a függvények.

A függvény az alkalmazás rendelkezik sok gyermekszintű erőforrása, amely a központi telepítésre, beleértve az alkalmazásbeállítások és az adatforrás beállításokat is használhatja. Akkor is előfordulhat, hogy válasszon tooremove hello **sourcecontrols** gyermek-erőforrás és a, valamint egy másik [rendszerbe állítási beállításának](functions-continuous-deployment.md) helyette.

> [!IMPORTANT]
> toosuccessfully az alkalmazás telepítése Azure Resource Manager használatával, a fontos toounderstand hogyan vannak telepítve az erőforrások az Azure-ban. A következő példa hello, a legfelső szintű konfigurációk használatával alkalmazhatók **siteConfig**. Fontos tooset ezeket a konfigurációkat a felső szinten, mert azok átadja toohello funkciók futási és telepítési motor információkat is. Legfelső szintű adatokat hello gyermek előtt meg kell adni **sourcecontrols vagy webes** erőforrás vonatkozik. Bár lehetséges tooconfigure ezeket a beállításokat a hello gyermekszintű **config/appSettings** erőforrás, a függvény app telepítenie kell bizonyos esetekben *előtt* **config/appSettings**  vonatkozik. Például használata esetén a funkciók [Logic Apps](../logic-apps/index.md), a függvények egy másik erőforrás függősége.

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            { "name": "FUNCTIONS_EXTENSION_VERSION", "value": "~1" },
            { "name": "Project", "value": "src" }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> Ez a sablon által hello [projekt](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app beállítások értéket, amely beállítja a hello alapkönyvtárának mely hello a funkciók telepítési motor (Kudu) megkeresi telepíthető. A funkciók vannak a tárházban hello almappájában **src** mappát. Igen, a fenti példa hello, hivatott hello app beállítások érték túl`src`. Ha a funkciók hello a tárház gyökérkönyvtárában, vagy ha nem telepíti a verziókövetésből, eltávolíthatja az alkalmazás beállításainak érték.

## <a name="deploy-your-template"></a>A sablon telepítése

Használhatja a következő módokon toodeploy hello bármelyikét a sablont:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-tooazure-button"></a>TooAzure gomb telepítése

Cserélje le ```<url-encoded-path-to-azuredeploy-json>``` rendelkező egy [URL-kódolású](https://www.bing.com/search?q=url+encode) hello nyers elérési útja verzióját a `azuredeploy.json` fájlt a Githubon.

Íme egy példa a markdown használó:

```markdown
[![Deploy tooAzure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Íme egy példa a HTML formátumú:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Következő lépések

További tudnivalók toodevelop és az Azure Functions konfigurálása.

* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)
* [Hogyan tooconfigure Azure működni Alkalmazásbeállítások](functions-how-to-use-azure-function-app-settings.md)
* [Az első Azure-függvény létrehozása](functions-create-first-azure-function.md)

<!-- LINKS -->

[függvény app a fogyasztás terv]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[függvény alkalmazást az Azure App Service-csomag]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
