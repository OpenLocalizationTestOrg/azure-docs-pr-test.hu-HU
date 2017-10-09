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
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a><span data-ttu-id="d738a-103">A webalkalmazás MSDeploy, az egyéni állomásnevet és az SSL-tanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="d738a-103">Deploy a web app with MSDeploy, custom hostname and SSL certificate</span></span>
<span data-ttu-id="d738a-104">Ez az útmutató végigvezeti egy végpont telepítés létrehozása az Azure Web Apps, MSDeploy követését, valamint egy egyéni állomásnevet és egy SSL-tanúsítvány toohello ARM-sablon hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d738a-104">This guide walks through creating an end-to-end deployment for an Azure Web App, leveraging MSDeploy as well as adding a custom hostname and an SSL certificate toohello ARM template.</span></span>

<span data-ttu-id="d738a-105">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d738a-105">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

### <a name="create-sample-application"></a><span data-ttu-id="d738a-106">A minta-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d738a-106">Create Sample Application</span></span>
<span data-ttu-id="d738a-107">Lesz az ASP.NET webalkalmazások telepítését.</span><span class="sxs-lookup"><span data-stu-id="d738a-107">You will be deploying an ASP.NET web application.</span></span> <span data-ttu-id="d738a-108">első lépés hello toocreate egy egyszerű webalkalmazást (vagy kiválaszthatják toouse, hogy egy meglévő - ebben az esetben kihagyhatja ezt a lépést).</span><span class="sxs-lookup"><span data-stu-id="d738a-108">hello first step is toocreate a simple web application (or you could choose toouse an existing one - in which case you can skip this step).</span></span>

<span data-ttu-id="d738a-109">Nyissa meg a Visual Studio 2015-öt, és válassza a fájl > Új projekt.</span><span class="sxs-lookup"><span data-stu-id="d738a-109">Open Visual Studio 2015 and choose File > New Project.</span></span> <span data-ttu-id="d738a-110">A megjelenő hello párbeszédpanelen válassza a webes > ASP.NET Webalkalmazásként való kezelése.</span><span class="sxs-lookup"><span data-stu-id="d738a-110">On hello dialog that appears choose Web > ASP.NET Web Application.</span></span> <span data-ttu-id="d738a-111">Sablonok alatt válassza a webes, és válassza ki a hello MVC-sablont.</span><span class="sxs-lookup"><span data-stu-id="d738a-111">Under Templates choose Web and choose hello MVC template.</span></span> <span data-ttu-id="d738a-112">Válassza ki *módosítsa a hitelesítési típus* túl*nem hitelesítési*.</span><span class="sxs-lookup"><span data-stu-id="d738a-112">Select *Change authentication type* too*No Authentication*.</span></span> <span data-ttu-id="d738a-113">Ez az imént toomake hello mintaalkalmazás lehető legegyszerűbb.</span><span class="sxs-lookup"><span data-stu-id="d738a-113">This is just toomake hello sample application as simple as possible.</span></span>

<span data-ttu-id="d738a-114">Egy egyszerű ASP.Net webes alkalmazás készen áll a toouse lesz ezen a ponton a telepítési folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="d738a-114">At this point you will have a basic ASP.Net web app ready toouse as part of your deployment process.</span></span>

### <a name="create-msdeploy-package"></a><span data-ttu-id="d738a-115">MSDeploy-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="d738a-115">Create MSDeploy package</span></span>
<span data-ttu-id="d738a-116">Következő lépés toocreate hello csomag toodeploy hello web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d738a-116">Next step is toocreate hello package toodeploy hello web app tooAzure.</span></span> <span data-ttu-id="d738a-117">toodo, mentse a projektet, és futtassa a hello következő hello parancssorból:</span><span class="sxs-lookup"><span data-stu-id="d738a-117">toodo this, save your project and then run hello following from hello command line:</span></span>

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

<span data-ttu-id="d738a-118">Ezzel létrehoz egy tömörített csomag hello PackageLocation mappában.</span><span class="sxs-lookup"><span data-stu-id="d738a-118">This will create a zipped package under hello PackageLocation folder.</span></span> <span data-ttu-id="d738a-119">hello alkalmazás most már készen áll a telepített, toobe, most már lefordíthatja az Azure Resource Manager sablon toodo ki, amely.</span><span class="sxs-lookup"><span data-stu-id="d738a-119">hello application is now ready toobe deployed, which you can now build out an Azure Resource Manager template toodo that.</span></span>

### <a name="create-arm-template"></a><span data-ttu-id="d738a-120">ARM-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="d738a-120">Create ARM Template</span></span>
<span data-ttu-id="d738a-121">Első lépésként kezdjük egy alapszintű ARM-sablon, amely létrehoz egy webalkalmazást, valamint egy üzemeltetési terv (vegye figyelembe, hogy nem látható kivonatosan mutatja paraméterek és változók).</span><span class="sxs-lookup"><span data-stu-id="d738a-121">First, let's start with a basic ARM template that will create a web application and a hosting plan (note that parameters and variables are not shown for brevity).</span></span>

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

<span data-ttu-id="d738a-122">Ezt követően kell toomodify hello web app erőforrás tootake beágyazott MSDeploy erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d738a-122">Next, you will need toomodify hello web app resource tootake a nested MSDeploy resource.</span></span> <span data-ttu-id="d738a-123">Ez akkor tooreference hello csomag korábban létrehozott, és kérje meg az Azure Resource Manager toouse MSDeploy toodeploy hello csomag toohello Azure webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d738a-123">This will allow you tooreference hello package created earlier and tell Azure Resource Manager toouse MSDeploy toodeploy hello package toohello Azure WebApp.</span></span> <span data-ttu-id="d738a-124">hello következő hello Microsoft.Web/sites erőforráshoz. a beágyazott hello MSDeploy erőforrás jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="d738a-124">hello following shows hello Microsoft.Web/sites resource with hello nested MSDeploy resource:</span></span>

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

<span data-ttu-id="d738a-125">Most láthatja, hogy hello MSDeploy erőforrást vesz igénybe egy **packageUri** tulajdonság, amely az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="d738a-125">Now you will notice that hello MSDeploy resource takes a **packageUri** property which is defined as follows:</span></span>

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

<span data-ttu-id="d738a-126">Ez **packageUri** vesz hello tárfiók URI azonosítóját, amely mutat, ahol fel kell töltenie a csomag zip való toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d738a-126">This **packageUri** takes hello storage account uri which points toohello storage account where you will upload your package zip to.</span></span> <span data-ttu-id="d738a-127">hello Azure Resource Manager fogja használni, [megosztott hozzáférési aláírásokkal](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello csomagot le helyileg hello tárfiók hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d738a-127">hello Azure Resource Manager will leverage [Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello package down locally from hello storage account when you deploy hello template.</span></span> <span data-ttu-id="d738a-128">Ez a folyamat automatizált lesz egy PowerShell-parancsfájlt, amely hello csomag feltöltése és hello Azure felügyeleti API toocreate hello kulcsok szükséges hívja, és adja át azokat hello sablonba paraméterekként keresztül (*_artifactsLocation* és *_artifactsLocationSasToken*).</span><span class="sxs-lookup"><span data-stu-id="d738a-128">This process will be automated via a PowerShell script that will upload hello package and call hello Azure Management API toocreate hello keys required and pass those into hello template as parameters (*_artifactsLocation* and *_artifactsLocationSasToken*).</span></span> <span data-ttu-id="d738a-129">Szüksége lesz toodefine paraméterek hello mappa és a fájlnév hello csomag feltöltött toounder hello tároló.</span><span class="sxs-lookup"><span data-stu-id="d738a-129">You will need toodefine parameters for hello folder and filename hello package is uploaded toounder hello storage container.</span></span>

<span data-ttu-id="d738a-130">Ezután egy másik beágyazott erőforrás toosetup hello állomásnév kötések tooleverage egyéni tartományt a tooadd van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d738a-130">Next you need tooadd in another nested resource toosetup hello hostname bindings tooleverage a custom domain.</span></span> <span data-ttu-id="d738a-131">Első kell tooensure saját hello állomásnév, és állítsa be toobe ellenőrzése az Azure-ban, hogy a sajátja – tekintse meg fog [egyéni tartománynév beállítása az Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d738a-131">You will first need tooensure that you own hello hostname and set it up toobe verified by Azure that you own it - see [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span> <span data-ttu-id="d738a-132">Ezt követően a következő tooyour sablon hello Microsoft.Web/sites erőforrás szakaszban hello is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="d738a-132">Once that is done you can add hello following tooyour template under hello Microsoft.Web/sites resource section:</span></span>

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

<span data-ttu-id="d738a-133">Végül kell tooadd más legfelső szintű erőforrás, Microsoft.Web/certificates.</span><span class="sxs-lookup"><span data-stu-id="d738a-133">Finally you need tooadd another top level resource, Microsoft.Web/certificates.</span></span> <span data-ttu-id="d738a-134">Ehhez az erőforráshoz fogja tartalmazni az SSL-tanúsítvány, és lehet azonos a webes alkalmazás szintű és üzemeltetési terv hello fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d738a-134">This resource will contain your SSL certificate and will exist at hello same level as your web app and hosting plan.</span></span>

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

<span data-ttu-id="d738a-135">Szüksége lesz toohave rendelés tooset fel az erőforrás érvényes SSL-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d738a-135">You will need toohave a valid SSL certificate in order tooset up this resource.</span></span> <span data-ttu-id="d738a-136">Ha elvégezte a tanúsítvány érvényes majd kell tooextract hello pfx bájt Base64 kódolású karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d738a-136">Once you have that valid certificate then you need tooextract hello pfx bytes as a base64 string.</span></span> <span data-ttu-id="d738a-137">Egy beállítás tooextract Ez a következő PowerShell-paranccsal toouse hello:</span><span class="sxs-lookup"><span data-stu-id="d738a-137">One option tooextract this is toouse hello following PowerShell command:</span></span>

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

<span data-ttu-id="d738a-138">Sikerült majd adja meg a paraméter tooyour ARM központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="d738a-138">You could then pass this as a parameter tooyour ARM deployment template.</span></span>

<span data-ttu-id="d738a-139">Ezen a ponton hello ARM-sablon készen áll.</span><span class="sxs-lookup"><span data-stu-id="d738a-139">At this point hello ARM template is ready.</span></span>

### <a name="deploy-template"></a><span data-ttu-id="d738a-140">Sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d738a-140">Deploy Template</span></span>
<span data-ttu-id="d738a-141">utolsó lépéseinek hello vannak toopiece ezzel együtt a teljes-végpont a központi.</span><span class="sxs-lookup"><span data-stu-id="d738a-141">hello final steps are toopiece this all together into a full end-to-end deployment.</span></span> <span data-ttu-id="d738a-142">egyszerűbbé teheti az hello toomake telepítési **telepítés-AzureResourceGroup.ps1** hozzáadott egy Azure erőforráscsoport-projekt létrehozásakor, a Visual Studio toohelp bármely szükséges összetevők feltöltése a PowerShell-parancsfájl hello sablont.</span><span class="sxs-lookup"><span data-stu-id="d738a-142">toomake deployment easier you can leverage hello **Deploy-AzureResourceGroup.ps1** PowerShell script that is added when you create an Azure Resource Group project in Visual Studio toohelp with uploading of any artifacts required in hello template.</span></span> <span data-ttu-id="d738a-143">Szükségel toohave, a kívánt időben toouse storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d738a-143">It requires you toohave created a storage account you want toouse ahead of time.</span></span> <span data-ttu-id="d738a-144">Ehhez a példához létrehozott egy megosztott tárfiókot a hello package.zip toobe feltöltve.</span><span class="sxs-lookup"><span data-stu-id="d738a-144">For this example, I created a shared storage account for hello package.zip toobe uploaded.</span></span> <span data-ttu-id="d738a-145">hello parancsfájl AzCopy tooupload hello csomag toohello tárfiókot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d738a-145">hello script will leverage AzCopy tooupload hello package toohello storage account.</span></span> <span data-ttu-id="d738a-146">Adja meg a a összetevő helye és hello parancsfájl automatikusan feltölti, hogy a tároló nevű directory toohello lévő összes fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="d738a-146">You pass in your artifact folder location and hello script will automatically upload all files within that directory toohello named storage container.</span></span> <span data-ttu-id="d738a-147">Telepítés-AzureResourceGroup.ps1 hívása után toothen frissítés hello SSL kötések toomap hello egyéni állomásnév az SSL-tanúsítvánnyal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d738a-147">After calling Deploy-AzureResourceGroup.ps1 you have toothen update hello SSL bindings toomap hello custom hostname with your SSL certificate.</span></span>

<span data-ttu-id="d738a-148">PowerShell mutat be a következő hello hello teljes telepítési hívó hello telepítés-AzureResourceGroup.ps1:</span><span class="sxs-lookup"><span data-stu-id="d738a-148">hello following PowerShell shows hello complete deployment calling hello Deploy-AzureResourceGroup.ps1:</span></span>

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

<span data-ttu-id="d738a-149">Ezen a ponton az alkalmazás kell van telepítve, és meg kell tudni toobrowse tooit https://www.yourcustomdomain.com keresztül</span><span class="sxs-lookup"><span data-stu-id="d738a-149">At this point your application should have been deployed and you should be able toobrowse tooit via https://www.yourcustomdomain.com</span></span>

