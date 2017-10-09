---
title: "aaaDeploy MSDeploy állomásnév és az ssl-tanúsítvány használatával webes alkalmazás"
description: "Webes alkalmazás MSDeploy használ, és az egyéni állomásnevet és egy SSL-tanúsítvány beállítása az Azure Resource Manager sablon toodeploy használata"
services: app-service\web
manager: erikre
documentationcenter: 
author: jodehavi
ms.assetid: 66366a72-cef7-4d75-8779-f4d32ed33cf7
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2016
ms.author: jodehavi
ms.openlocfilehash: ac13f4a7d14ae182e8e7ced5adff30491422d1e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>A webalkalmazás MSDeploy, az egyéni állomásnevet és az SSL-tanúsítvány telepítése
Ez az útmutató végigvezeti egy végpont telepítés létrehozása az Azure Web Apps, MSDeploy követését, valamint egy egyéni állomásnevet és egy SSL-tanúsítvány toohello ARM-sablon hozzáadása.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

### <a name="create-sample-application"></a>A minta-alkalmazás létrehozása
Lesz az ASP.NET webalkalmazások telepítését. első lépés hello toocreate egy egyszerű webalkalmazást (vagy kiválaszthatják toouse, hogy egy meglévő - ebben az esetben kihagyhatja ezt a lépést).

Nyissa meg a Visual Studio 2015-öt, és válassza a fájl > Új projekt. A megjelenő hello párbeszédpanelen válassza a webes > ASP.NET Webalkalmazásként való kezelése. Sablonok alatt válassza a webes, és válassza ki a hello MVC-sablont. Válassza ki *módosítsa a hitelesítési típus* túl*nem hitelesítési*. Ez az imént toomake hello mintaalkalmazás lehető legegyszerűbb.

Egy egyszerű ASP.Net webes alkalmazás készen áll a toouse lesz ezen a ponton a telepítési folyamat részeként.

### <a name="create-msdeploy-package"></a>MSDeploy-csomag létrehozása
Következő lépés toocreate hello csomag toodeploy hello web app tooAzure. toodo, mentse a projektet, és futtassa a hello következő hello parancssorból:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Ezzel létrehoz egy tömörített csomag hello PackageLocation mappában. hello alkalmazás most már készen áll a telepített, toobe, most már lefordíthatja az Azure Resource Manager sablon toodo ki, amely.

### <a name="create-arm-template"></a>ARM-sablon létrehozása
Első lépésként kezdjük egy alapszintű ARM-sablon, amely létrehoz egy webalkalmazást, valamint egy üzemeltetési terv (vegye figyelembe, hogy nem látható kivonatosan mutatja paraméterek és változók).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Ezt követően kell toomodify hello web app erőforrás tootake beágyazott MSDeploy erőforrás. Ez akkor tooreference hello csomag korábban létrehozott, és kérje meg az Azure Resource Manager toouse MSDeploy toodeploy hello csomag toohello Azure webalkalmazás. hello következő hello Microsoft.Web/sites erőforráshoz. a beágyazott hello MSDeploy erőforrás jeleníti meg:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Most láthatja, hogy hello MSDeploy erőforrást vesz igénybe egy **packageUri** tulajdonság, amely az alábbiak szerint definiáltuk:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Ez **packageUri** vesz hello tárfiók URI azonosítóját, amely mutat, ahol fel kell töltenie a csomag zip való toohello tárfiók. hello Azure Resource Manager fogja használni, [megosztott hozzáférési aláírásokkal](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello csomagot le helyileg hello tárfiók hello sablon telepítésekor. Ez a folyamat automatizált lesz egy PowerShell-parancsfájlt, amely hello csomag feltöltése és hello Azure felügyeleti API toocreate hello kulcsok szükséges hívja, és adja át azokat hello sablonba paraméterekként keresztül (*_artifactsLocation* és *_artifactsLocationSasToken*). Szüksége lesz toodefine paraméterek hello mappa és a fájlnév hello csomag feltöltött toounder hello tároló.

Ezután egy másik beágyazott erőforrás toosetup hello állomásnév kötések tooleverage egyéni tartományt a tooadd van szüksége. Első kell tooensure saját hello állomásnév, és állítsa be toobe ellenőrzése az Azure-ban, hogy a sajátja – tekintse meg fog [egyéni tartománynév beállítása az Azure App Service](app-service-web-tutorial-custom-domain.md). Ezt követően a következő tooyour sablon hello Microsoft.Web/sites erőforrás szakaszban hello is hozzáadhat:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Végül kell tooadd más legfelső szintű erőforrás, Microsoft.Web/certificates. Ehhez az erőforráshoz fogja tartalmazni az SSL-tanúsítvány, és lehet azonos a webes alkalmazás szintű és üzemeltetési terv hello fog létrehozni.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Szüksége lesz toohave rendelés tooset fel az erőforrás érvényes SSL-tanúsítványt. Ha elvégezte a tanúsítvány érvényes majd kell tooextract hello pfx bájt Base64 kódolású karakterlánc. Egy beállítás tooextract Ez a következő PowerShell-paranccsal toouse hello:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Sikerült majd adja meg a paraméter tooyour ARM központi telepítési sablont.

Ezen a ponton hello ARM-sablon készen áll.

### <a name="deploy-template"></a>Sablon üzembe helyezése
utolsó lépéseinek hello vannak toopiece ezzel együtt a teljes-végpont a központi. egyszerűbbé teheti az hello toomake telepítési **telepítés-AzureResourceGroup.ps1** hozzáadott egy Azure erőforráscsoport-projekt létrehozásakor, a Visual Studio toohelp bármely szükséges összetevők feltöltése a PowerShell-parancsfájl hello sablont. Szükségel toohave, a kívánt időben toouse storage-fiók létrehozása. Ehhez a példához létrehozott egy megosztott tárfiókot a hello package.zip toobe feltöltve. hello parancsfájl AzCopy tooupload hello csomag toohello tárfiókot fogja használni. Adja meg a a összetevő helye és hello parancsfájl automatikusan feltölti, hogy a tároló nevű directory toohello lévő összes fájlhoz. Telepítés-AzureResourceGroup.ps1 hívása után toothen frissítés hello SSL kötések toomap hello egyéni állomásnév az SSL-tanúsítvánnyal rendelkezik.

PowerShell mutat be a következő hello hello teljes telepítési hívó hello telepítés-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script toodeploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app toobind ssl certificate toohostname. This has toobe done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Ezen a ponton az alkalmazás kell van telepítve, és meg kell tudni toobrowse tooit https://www.yourcustomdomain.com keresztül

