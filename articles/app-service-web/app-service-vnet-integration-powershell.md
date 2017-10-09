---
title: "aaaConnect a app tooyour virtuális hálózat PowerShell használatával"
description: "Leírja, hogyan tooconnect tooand működnek virtuális hálózatokat a PowerShell használatával"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: a5c76e77-972a-431c-b14b-3611dae1631b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: ccompy
ms.openlocfilehash: c9d0fa99d02cab7b2c7211a1b2f7b7d0cd27ee8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-app-tooyour-virtual-network-by-using-powershell"></a><span data-ttu-id="b7a51-103">Az alkalmazás tooyour virtuális hálózati csatlakoztatása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b7a51-103">Connect your app tooyour virtual network by using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="b7a51-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b7a51-104">Overview</span></span>
<span data-ttu-id="b7a51-105">Az Azure App Service szolgáltatásban csatlakoztathatja az alkalmazás (webes, mobil, vagy API) tooan Azure virtuális hálózathoz (VNet) az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="b7a51-105">In Azure App Service, you can connect your app (web, mobile, or API) tooan Azure virtual network (VNet) in your subscription.</span></span> <span data-ttu-id="b7a51-106">A szolgáltatás virtuális integráció neve.</span><span class="sxs-lookup"><span data-stu-id="b7a51-106">This feature is called VNet Integration.</span></span> <span data-ttu-id="b7a51-107">hello VNet integrációs szolgáltatás számára nem keverendő kell hello App Service Environment-környezet szolgáltatással, amely lehetővé teszi a virtuális hálózat az Azure App Service példányának toorun.</span><span class="sxs-lookup"><span data-stu-id="b7a51-107">hello VNet Integration feature should not be confused with hello App Service Environment feature, which allows you toorun an instance of Azure App Service in your virtual network.</span></span>

<span data-ttu-id="b7a51-108">hello VNet integrációs szolgáltatás számára van a felhasználói felület (UI) az új portálon hello használható toointegrate hello klasszikus üzembe helyezési modellel vagy hello Azure Resource Manager üzembe helyezési modellben telepített virtuális hálózatokat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-108">hello VNet Integration feature has a user interface (UI) in hello new portal that you can use toointegrate with virtual networks that are deployed by using either hello classic deployment model or hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="b7a51-109">Ha azt szeretné, hogy hello funkcióval kapcsolatban további toolearn, [az alkalmazás integrálása az Azure virtuális hálózat](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="b7a51-109">If you want toolearn more about hello feature, see [Integrate your app with an Azure virtual network](web-sites-integrate-with-vnet.md).</span></span>

<span data-ttu-id="b7a51-110">Ez a cikk nem kapcsolatos hogyan toouse hello felhasználói felület, de ahelyett, hogy arról, hogyan van tooenable integrációs PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b7a51-110">This article is not about how toouse hello UI but rather about how tooenable integration by using PowerShell.</span></span> <span data-ttu-id="b7a51-111">Mivel minden üzembe helyezési modellel hello parancsai nem egyezik, akkor ez a cikk szakasza minden központi telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="b7a51-111">Because hello commands for each deployment model are different, this article has a section for each deployment model.</span></span>  

<span data-ttu-id="b7a51-112">Ez a cikk a folytatás előtt győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="b7a51-112">Before you continue with this article, ensure that you have:</span></span>

* <span data-ttu-id="b7a51-113">hello Azure PowerShell SDK legújabb telepítve.</span><span class="sxs-lookup"><span data-stu-id="b7a51-113">hello latest Azure PowerShell SDK installed.</span></span> <span data-ttu-id="b7a51-114">A hello Webplatform-telepítővel telepíthető.</span><span class="sxs-lookup"><span data-stu-id="b7a51-114">You can install this with hello Web Platform Installer.</span></span>
* <span data-ttu-id="b7a51-115">Az Azure App Service egy Standard vagy Premium Termékváltozat futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b7a51-115">An app in Azure App Service running in a Standard or Premium SKU.</span></span>

## <a name="classic-virtual-networks"></a><span data-ttu-id="b7a51-116">Klasszikus virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="b7a51-116">Classic virtual networks</span></span>
<span data-ttu-id="b7a51-117">Ez a szakasz ismerteti a három feladatok hello klasszikus üzembe helyezési modellt használó virtuális hálózatok:</span><span class="sxs-lookup"><span data-stu-id="b7a51-117">This section explains three tasks for virtual networks that use hello classic deployment model:</span></span>

1. <span data-ttu-id="b7a51-118">Csatlakoztassa a app tooa elérésű, korábban létező virtuális hálózati átjáró és a pont – hely kapcsolat van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b7a51-118">Connect your app tooa preexisting virtual network that has a gateway and is configured for point-to-site connectivity.</span></span>
2. <span data-ttu-id="b7a51-119">Az alkalmazás a virtuális hálózati integráció adatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="b7a51-119">Update your virtual network integration information for your app.</span></span>
3. <span data-ttu-id="b7a51-120">Az alkalmazás leválasztása a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-120">Disconnect your app from your virtual network.</span></span>

### <a name="connect-an-app-tooa-classic-vnet"></a><span data-ttu-id="b7a51-121">Csatlakozás egy alkalmazás tooa klasszikus virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="b7a51-121">Connect an app tooa classic VNet</span></span>
<span data-ttu-id="b7a51-122">egy alkalmazás tooa virtuális hálózathoz tooconnect kövesse az alábbi három lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b7a51-122">tooconnect an app tooa virtual network, follow these three steps:</span></span>

1. <span data-ttu-id="b7a51-123">Deklarálja, hogy az csatlakozni fog egy adott virtuális hálózati toohello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b7a51-123">Declare toohello web app that it will be joining a particular virtual network.</span></span> <span data-ttu-id="b7a51-124">hello app olyan tanúsítvány, amely a virtuális hálózati toohello kap a pont – hely kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b7a51-124">hello app will generate a certificate that will be given toohello virtual network for point-to-site connectivity.</span></span>
2. <span data-ttu-id="b7a51-125">Töltsön fel hello web app tanúsítvány toohello virtuális hálózatot, és majd lekérheti a hello pont-pont VPN csomag URI.</span><span class="sxs-lookup"><span data-stu-id="b7a51-125">Upload hello web app certificate toohello virtual network, and then retrieve hello point-to-site VPN package URI.</span></span>
3. <span data-ttu-id="b7a51-126">Frissítse a hello web app virtuális hálózati kapcsolat hello pont-pont csomag URI.</span><span class="sxs-lookup"><span data-stu-id="b7a51-126">Update hello web app's virtual network connection with hello point-to-site package URI.</span></span>

<span data-ttu-id="b7a51-127">hello első és harmadik lépéseket teljes parancsfájlos, de hello második lépés szükséges egy egyszeri, manuális művelet hello portál vagy a hozzáférés tooperform **PUT** vagy **javítás** műveletek hello virtuális hálózaton Azure Resource Manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="b7a51-127">hello first and third steps are fully scriptable, but hello second step requires a one-time, manual action through hello portal, or access tooperform **PUT** or **PATCH** actions on hello virtual network Azure Resource Manager endpoint.</span></span> <span data-ttu-id="b7a51-128">Lépjen kapcsolatba Azure támogatási toohave ez engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b7a51-128">Contact Azure Support toohave this enabled.</span></span> <span data-ttu-id="b7a51-129">Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e a klasszikus virtuális hálózatot, amelyen már engedélyezve van a pont – hely kapcsolat és a telepített átjárót.</span><span class="sxs-lookup"><span data-stu-id="b7a51-129">Before you start, make sure that you have a classic virtual network that has point-to-site connectivity already enabled and a deployed gateway.</span></span> <span data-ttu-id="b7a51-130">toocreate hello átjáró és az engedélyezés pont – hely kapcsolatot, kell toouse hello portal részben ismertetett módon [VPN-átjáró létrehozása][createvpngateway].</span><span class="sxs-lookup"><span data-stu-id="b7a51-130">toocreate hello gateway and enable point-to-site connectivity, you need toouse hello portal as described at [Creating a VPN gateway][createvpngateway].</span></span>

<span data-ttu-id="b7a51-131">hello klasszikus virtuális hálózatot kell toobe hello ugyanahhoz az előfizetéshez, az App Service tervet, amely integrációhoz tartás hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7a51-131">hello classic virtual network needs toobe in hello same subscription as your App Service plan that holds hello app that you are integrating with.</span></span>

##### <a name="set-up-azure-powershell-sdk"></a><span data-ttu-id="b7a51-132">Azure PowerShell SDK beállítása</span><span class="sxs-lookup"><span data-stu-id="b7a51-132">Set up Azure PowerShell SDK</span></span>
<span data-ttu-id="b7a51-133">Nyisson meg egy PowerShell-ablakot, és állítsa be az Azure-fiókja és -előfizetést használatával:</span><span class="sxs-lookup"><span data-stu-id="b7a51-133">Open a PowerShell window and set up your Azure account and subscription by using:</span></span>

    Login-AzureRmAccount

<span data-ttu-id="b7a51-134">Ez a parancs nyílik Rákérdezés tooget Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b7a51-134">That command will open a prompt tooget your Azure credentials.</span></span> <span data-ttu-id="b7a51-135">Miután bejelentkezik, használja a következő parancsok tooselect hello előfizetést, amelyet az toouse hello egyikét.</span><span class="sxs-lookup"><span data-stu-id="b7a51-135">After you sign in, use either of hello following commands tooselect hello subscription that you want toouse.</span></span> <span data-ttu-id="b7a51-136">Győződjön meg arról, hogy a rendszer a virtuális hálózat és az App Service-csomag hello-előfizetését használja.</span><span class="sxs-lookup"><span data-stu-id="b7a51-136">Make sure that you are using hello subscription that your virtual network and App Service plan are in.</span></span>

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

<span data-ttu-id="b7a51-137">vagy</span><span class="sxs-lookup"><span data-stu-id="b7a51-137">or</span></span>

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a><span data-ttu-id="b7a51-138">A cikk ezt használja változók</span><span class="sxs-lookup"><span data-stu-id="b7a51-138">Variables used in this article</span></span>
<span data-ttu-id="b7a51-139">toosimplify parancsok helyezünk egy **$Configuration** PowerShell változó hello adott konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="b7a51-139">toosimplify commands, we will set a **$Configuration** PowerShell variable with hello specific configuration.</span></span>

<span data-ttu-id="b7a51-140">Egy változót az alábbiak szerint állíthatja a PowerShellben a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="b7a51-140">Set a variable as follows in PowerShell with hello following parameters:</span></span>

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

<span data-ttu-id="b7a51-141">hello app helyen hello hely nélkül szóközt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b7a51-141">hello app location should be hello location without any spaces.</span></span> <span data-ttu-id="b7a51-142">USA nyugati régiója például westus.</span><span class="sxs-lookup"><span data-stu-id="b7a51-142">For example, West US is westus.</span></span>

    $Configuration.WebAppLocation = "[Your web app Location]"

<span data-ttu-id="b7a51-143">hello következő elem, ahol hello tanúsítványt kell írni.</span><span class="sxs-lookup"><span data-stu-id="b7a51-143">hello next item is where hello certificate should be written.</span></span> <span data-ttu-id="b7a51-144">Azt a helyi számítógépen írható elérési útnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b7a51-144">It should be a writable path on your local computer.</span></span> <span data-ttu-id="b7a51-145">Győződjön meg arról, hogy tooinclude .cer hello végén.</span><span class="sxs-lookup"><span data-stu-id="b7a51-145">Make sure tooinclude .cer at hello end.</span></span>

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

<span data-ttu-id="b7a51-146">toosee mi állítja, típus **$Configuration**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-146">toosee what you set, type **$Configuration**.</span></span>

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

<span data-ttu-id="b7a51-147">Ez a szakasz többi hello feltételezi, hogy rendelkezik-e csak leírtak változók.</span><span class="sxs-lookup"><span data-stu-id="b7a51-147">hello rest of this section assumes that you have a variable created as just described.</span></span>

##### <a name="declare-hello-virtual-network-toohello-app"></a><span data-ttu-id="b7a51-148">Hello virtuális hálózati toohello app deklarálható</span><span class="sxs-lookup"><span data-stu-id="b7a51-148">Declare hello virtual network toohello app</span></span>
<span data-ttu-id="b7a51-149">A következő parancs tootell hello alkalmazást, hogy azt fogja használni az adott virtuális hálózati hello használata.</span><span class="sxs-lookup"><span data-stu-id="b7a51-149">Use hello following command tootell hello app that it will be using this particular virtual network.</span></span> <span data-ttu-id="b7a51-150">Ennek hatására hello app toogenerate szükséges tanúsítványok:</span><span class="sxs-lookup"><span data-stu-id="b7a51-150">This will cause hello app toogenerate necessary certificates:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

<span data-ttu-id="b7a51-151">Ha ez a parancs végrehajtása sikeres, **$vnet** rendelkeznie kell egy **tulajdonságok** változó azt.</span><span class="sxs-lookup"><span data-stu-id="b7a51-151">If this command succeeds, **$vnet** should have a **Properties** variable in it.</span></span> <span data-ttu-id="b7a51-152">Hello **tulajdonságok** változó tartalmaznia kell mindkét a tanúsítvány ujjlenyomata és hello tanúsítványának adatait.</span><span class="sxs-lookup"><span data-stu-id="b7a51-152">hello **Properties** variable should contain both a certificate thumbprint and hello certificate data.</span></span>

##### <a name="upload-hello-web-app-certificate-toohello-virtual-network"></a><span data-ttu-id="b7a51-153">Hello web app tanúsítvány toohello virtuális hálózati feltöltése</span><span class="sxs-lookup"><span data-stu-id="b7a51-153">Upload hello web app certificate toohello virtual network</span></span>
<span data-ttu-id="b7a51-154">Kézi, egyszeri lépés akkor szükséges, az egyes előfizetés és a virtuális hálózati kombinációja.</span><span class="sxs-lookup"><span data-stu-id="b7a51-154">A manual, one-time step is required for each subscription and virtual network combination.</span></span> <span data-ttu-id="b7a51-155">Ez azt jelenti, hogy ha az előfizetés A tooVirtual hálózati alkalmazások kapcsolódik, szüksége lesz toodo ezt a lépést csak egyszer, függetlenül attól, hogy hány alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b7a51-155">That is, if you are connecting apps in Subscription A tooVirtual Network A, you will need toodo this step only once regardless of how many apps you configure.</span></span> <span data-ttu-id="b7a51-156">Ha ad hozzá egy új alkalmazás tooanother virtuális hálózatot, toodo többé lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="b7a51-156">If you are adding a new app tooanother virtual network, you'll need toodo this again.</span></span> <span data-ttu-id="b7a51-157">hello ennek oka, hogy a tanúsítványok készletét jön létre az Azure App Service előfizetési szinten, és hello beállítása után minden virtuális hálózathoz csatlakozó hello alkalmazások jön létre.</span><span class="sxs-lookup"><span data-stu-id="b7a51-157">hello reason for this is that a set of certificates is generated at a subscription level in Azure App Service, and hello set is generated once for each virtual network that hello apps will connect to.</span></span>

<span data-ttu-id="b7a51-158">hello tanúsítványokat fog rendelkezik már be van állítva, ha követte ezeket a lépéseket, vagy ha integrálva hello azonos virtuális hálózati hello portál használatával.</span><span class="sxs-lookup"><span data-stu-id="b7a51-158">hello certificates will have already been set if you followed these steps or if you integrated with hello same virtual network by using hello portal.</span></span>

<span data-ttu-id="b7a51-159">első lépés hello toogenerate hello .cer fájl.</span><span class="sxs-lookup"><span data-stu-id="b7a51-159">hello first step is toogenerate hello .cer file.</span></span> <span data-ttu-id="b7a51-160">hello második lépésben tooupload hello .cer fájl tooyour virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-160">hello second step is tooupload hello .cer file tooyour virtual network.</span></span> <span data-ttu-id="b7a51-161">toogenerate hello .cer fájl hello API-hívás a hello futtassa a következő parancsok hello a korábbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="b7a51-161">toogenerate hello .cer file from hello API call in hello earlier step, run hello following commands.</span></span>

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

<span data-ttu-id="b7a51-162">hello tanúsítvány hello helyen találhatók, amely **$Configuration.GeneratedCertificatePath** határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b7a51-162">hello certificate will be found in hello location that **$Configuration.GeneratedCertificatePath** specifies.</span></span>

<span data-ttu-id="b7a51-163">tooupload hello tanúsítvány manuálisan, használja a hello [Azure-portálon] [ azureportal] és **Tallózás virtuális hálózat (klasszikus)** > **VPN-kapcsolatok**  >  **Pont-pont** > **tanúsítványok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-163">tooupload hello certificate manually, use hello [Azure portal][azureportal] and **Browse Virtual Network (classic)** > **VPN connections** > **Point-to-site** > **Manage certificates**.</span></span> <span data-ttu-id="b7a51-164">Itt a tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="b7a51-164">From here, upload your certificate.</span></span>

##### <a name="get-hello-point-to-site-package"></a><span data-ttu-id="b7a51-165">Hello pont-pont csomag</span><span class="sxs-lookup"><span data-stu-id="b7a51-165">Get hello point-to-site package</span></span>
<span data-ttu-id="b7a51-166">hello következő lépése a virtuális hálózati kapcsolat a webalkalmazás beállítása tooget hello pont-pont csomag, és adja meg a webalkalmazás tooyour.</span><span class="sxs-lookup"><span data-stu-id="b7a51-166">hello next step in setting up a virtual network connection on a web app is tooget hello point-to-site package and provide it tooyour web app.</span></span>

<span data-ttu-id="b7a51-167">A következő nevű GetNetworkPackageUri.json valahol a számítógépre, például C:\Azure\Templates\GetNetworkPackageUri.json tooa sablonfájlt hello mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7a51-167">Save hello following template tooa file called GetNetworkPackageUri.json somewhere on your computer, for example, C:\Azure\Templates\GetNetworkPackageUri.json.</span></span>

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


<span data-ttu-id="b7a51-168">A megadott bemeneti paraméterek:</span><span class="sxs-lookup"><span data-stu-id="b7a51-168">Set input parameters:</span></span>

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

<span data-ttu-id="b7a51-169">Hívás hello parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="b7a51-169">Call hello script:</span></span>

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


<span data-ttu-id="b7a51-170">hello változó **$output. Outputs.packageUri** hello csomag URI toobe tooyour webalkalmazás megadott mostantól tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b7a51-170">hello variable **$output.Outputs.packageUri** will now contain hello package URI toobe given tooyour web app.</span></span>

##### <a name="upload-hello-point-to-site-package-tooyour-app"></a><span data-ttu-id="b7a51-171">Hello pont-pont csomag tooyour alkalmazás feltöltése</span><span class="sxs-lookup"><span data-stu-id="b7a51-171">Upload hello point-to-site package tooyour app</span></span>
<span data-ttu-id="b7a51-172">utolsó lépésként hello tooprovide hello app csomaggal.</span><span class="sxs-lookup"><span data-stu-id="b7a51-172">hello final step is tooprovide hello app with this package.</span></span> <span data-ttu-id="b7a51-173">Egyszerűen futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="b7a51-173">Simply run hello next command:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

<span data-ttu-id="b7a51-174">Ha a rendszer megkérdezi, hogy meg vannak felülírja a meglévő erőforrás tooconfirm, győződjön meg arról, hogy tooallow azt.</span><span class="sxs-lookup"><span data-stu-id="b7a51-174">If a message asks you tooconfirm that you are overwriting an existing resource, make sure tooallow it.</span></span>

<span data-ttu-id="b7a51-175">Után ez a parancs sikeres, az alkalmazás most már csatlakoztatott toohello virtuális hálózati kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b7a51-175">After this command succeeds, your app should now be connected toohello virtual network.</span></span> <span data-ttu-id="b7a51-176">tooconfirm sikeres, nyissa meg tooyour app konzolon, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b7a51-176">tooconfirm success, go tooyour app console, and type hello following:</span></span>

    SET WEBSITE_

<span data-ttu-id="b7a51-177">Ha egy értéket, amely megfelel a hello cél virtuális hálózat nevét hello WEBSITE_VNETNAME nevű környezeti változó, az összes konfiguráció sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="b7a51-177">If there is an environment variable called WEBSITE_VNETNAME that has a value that matches hello name of hello target virtual network, all configurations have succeeded.</span></span>

### <a name="update-classic-vnet-integration-information"></a><span data-ttu-id="b7a51-178">Klasszikus virtuális integráció frissítése</span><span class="sxs-lookup"><span data-stu-id="b7a51-178">Update classic VNet integration information</span></span>
<span data-ttu-id="b7a51-179">tooupdate vagy újraszinkronizálásra adatait, egyszerűen ismételje meg a hello hello integrációs hello első helyen létrehozásakor követett.</span><span class="sxs-lookup"><span data-stu-id="b7a51-179">tooupdate or resync your information, simply repeat hello steps that you followed when you created hello integration in hello first place.</span></span> <span data-ttu-id="b7a51-180">Ezek a lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="b7a51-180">Those steps are:</span></span>

1. <span data-ttu-id="b7a51-181">Adja meg a konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-181">Define your configuration information.</span></span>
2. <span data-ttu-id="b7a51-182">Hello virtuális hálózati toohello app deklarálható.</span><span class="sxs-lookup"><span data-stu-id="b7a51-182">Declare hello virtual network toohello app.</span></span>
3. <span data-ttu-id="b7a51-183">Hello pont-pont csomag beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b7a51-183">Get hello point-to-site package.</span></span>
4. <span data-ttu-id="b7a51-184">Hello pont-pont csomag tooyour alkalmazás feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7a51-184">Upload hello point-to-site package tooyour app.</span></span>

### <a name="disconnect-your-app-from-a-classic-vnet"></a><span data-ttu-id="b7a51-185">Válassza le az alkalmazást egy klasszikus virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="b7a51-185">Disconnect your app from a classic VNet</span></span>
<span data-ttu-id="b7a51-186">toodisconnect hello alkalmazást, és kell hello konfigurációs adatait, amely a virtuális hálózati integráció során lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="b7a51-186">toodisconnect hello app, you need hello configuration information that was set during virtual network integration.</span></span> <span data-ttu-id="b7a51-187">Az információk, nincs majd egy parancs toodisconnect az alkalmazás a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="b7a51-187">Using that information, there is then one command toodisconnect your app from your virtual network.</span></span>

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a><span data-ttu-id="b7a51-188">Erőforrás-kezelő virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="b7a51-188">Resource Manager virtual networks</span></span>
<span data-ttu-id="b7a51-189">Erőforrás-kezelő virtuális hálózat is létezik Azure Resource Manager API-k, amelyek egyszerűsítése bizonyos folyamatok klasszikus virtuális hálózatokat képest.</span><span class="sxs-lookup"><span data-stu-id="b7a51-189">Resource Manager virtual networks have Azure Resource Manager APIs, which simplify some processes when compared with classic virtual networks.</span></span> <span data-ttu-id="b7a51-190">Egy parancsfájl, amely segítséget hello a következő feladatok végrehajtásában vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="b7a51-190">We have a script that will help you complete hello following tasks:</span></span>

* <span data-ttu-id="b7a51-191">Hozzon létre egy erőforrás-kezelő virtuális hálózatot, és az alkalmazás integrálható.</span><span class="sxs-lookup"><span data-stu-id="b7a51-191">Create a Resource Manager virtual network and integrate your app with it.</span></span>
* <span data-ttu-id="b7a51-192">Hozzon létre egy pont – hely kapcsolat konfigurálása az erőforrás-kezelő már meglévő virtuális hálózat és az alkalmazás integrálható.</span><span class="sxs-lookup"><span data-stu-id="b7a51-192">Create a gateway, configure point-to-site connectivity in a preexisting Resource Manager virtual network, and then integrate your app with it.</span></span>
* <span data-ttu-id="b7a51-193">Az alkalmazás integrálja egy már létező olyan erőforrás-kezelő virtuális hálózattal, amelyen egy átjáró és a pont – hely kapcsolat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b7a51-193">Integrate your app with a preexisting Resource Manager virtual network that has a gateway and point-to-site connectivity enabled.</span></span>
* <span data-ttu-id="b7a51-194">Az alkalmazás leválasztása a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-194">Disconnect your app from your virtual network.</span></span>

### <a name="resource-manager-vnet-app-service-integration-script"></a><span data-ttu-id="b7a51-195">Erőforrás-kezelő virtuális hálózat App Service integrációs parancsfájlját</span><span class="sxs-lookup"><span data-stu-id="b7a51-195">Resource Manager VNet App Service integration script</span></span>
<span data-ttu-id="b7a51-196">Másolja az alábbi parancsfájlt, és mentse tooa fájl hello.</span><span class="sxs-lookup"><span data-stu-id="b7a51-196">Copy hello following script and save it tooa file.</span></span> <span data-ttu-id="b7a51-197">Ha nem szeretné toouse hello parancsfájl, érzi, hogy a szabad toolearn toosee hogyan tooset folyamatot egy erőforrás-kezelő virtuális hálózattal.</span><span class="sxs-lookup"><span data-stu-id="b7a51-197">If you don’t want toouse hello script, feel free toolearn from it toosee how tooset things up with a Resource Manager virtual network.</span></span>

    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate toothis VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up tooan hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with hello following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create hello virtual network and add it here. hello way this works is:
        # 1) Add hello VNET association toohello App. This allows hello App toogenerate certificates, etc. for hello VNET.
        # 2) Create hello VNET and VNET gateway, add hello certificates, create hello public IP, etc., required for hello gateway
        # 3) Get hello VPN package from hello gateway and pass it back toohello App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association tooVNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, hello gateway should be able toobe joined tooan App, but may require some minor tweaking. We will declare toohello App now toouse this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected tooVNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET toointegrate with" $vnets $vnetNames

        # We need toocheck if this VNET is able toobe joined tooa App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have hello right certificate, we will need tooadd it.
                # If it doesn't have a point-to-site range, we will need tooadd it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need toocreate one.
            Write-Host "This Virtual Network has no gateway. I will need toocreate one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in hello address space $($vnet.AddressSpace.AddressPrefixes), with hello following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with hello following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need toocreate one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of hello Vpn type. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need toocheck if hello certificate here exists in hello gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting hello VPN package and giving it toohello App
        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected tooVNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
            else
        {
            Write-Host "Not connected tooa VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound toothis account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter hello Resource Group of your App"

    $appName = Read-Host "Please enter hello Name of your App"

    $options = @("Add a NEW Virtual Network tooan App", "Add an EXISTING Virtual Network tooan App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want toodo?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

<span data-ttu-id="b7a51-198">Hello parancsfájl másolatának mentése.</span><span class="sxs-lookup"><span data-stu-id="b7a51-198">Save a copy of hello script.</span></span> <span data-ttu-id="b7a51-199">Ebben a cikkben V2VnetAllinOne.ps1 nevezik, de egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="b7a51-199">In this article, it is called V2VnetAllinOne.ps1, but you can use another name.</span></span> <span data-ttu-id="b7a51-200">Nincsenek ehhez a parancsprogramhoz argumentumok.</span><span class="sxs-lookup"><span data-stu-id="b7a51-200">There are no arguments for this script.</span></span> <span data-ttu-id="b7a51-201">Egyszerűen futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="b7a51-201">You simply run it.</span></span> <span data-ttu-id="b7a51-202">hello először thing hello parancsfájl hajt végre a rendszer felszólítja a toosign.</span><span class="sxs-lookup"><span data-stu-id="b7a51-202">hello first thing hello script will do is prompt you toosign in.</span></span> <span data-ttu-id="b7a51-203">Miután bejelentkezik, hello parancsfájl lekérdezi a fiók adatait, és előfizetések listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b7a51-203">After you sign in, hello script gets details about your account and returns a list of subscriptions.</span></span> <span data-ttu-id="b7a51-204">A hitelesítő adatok hello kérelem nem számítva, hello kezdeti parancsfájl végrehajtása néz ki:</span><span class="sxs-lookup"><span data-stu-id="b7a51-204">Not counting hello request for your credentials, hello initial script execution looks like this:</span></span>

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) <span data-ttu-id="b7a51-205">Bemutató előfizetés (af5358e1-acac-2c90-a9eb-722190abf47a)</span><span class="sxs-lookup"><span data-stu-id="b7a51-205">Demo Subscription (af5358e1-acac-2c90-a9eb-722190abf47a)</span></span>
    2) <span data-ttu-id="b7a51-206">MS-teszt (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span><span class="sxs-lookup"><span data-stu-id="b7a51-206">MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span></span>
    3) <span data-ttu-id="b7a51-207">Lila bemutató előfizetés (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span><span class="sxs-lookup"><span data-stu-id="b7a51-207">Purple Demo Subscription (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span></span>

    <span data-ttu-id="b7a51-208">Válasszon egy lehetőséget: 3</span><span class="sxs-lookup"><span data-stu-id="b7a51-208">Choose an option: 3</span></span>

    <span data-ttu-id="b7a51-209">Fiók: ccompy@microsoft.com környezet: AzureCloud előfizetés: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d bérlői: 722278f-fef1-499f-91ab-2323d011db47</span><span class="sxs-lookup"><span data-stu-id="b7a51-209">Account      : ccompy@microsoft.com Environment  : AzureCloud Subscription : 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Tenant       : 722278f-fef1-499f-91ab-2323d011db47</span></span>

    <span data-ttu-id="b7a51-210">Adja meg az alkalmazás erőforráscsoport hello: hcdemo-rg adja meg az alkalmazás neve hello: v2vnetpowershell miről szeretne toodo?</span><span class="sxs-lookup"><span data-stu-id="b7a51-210">Please enter hello Resource Group of your App: hcdemo-rg Please enter hello Name of your App: v2vnetpowershell What do you want toodo?</span></span>

    1) <span data-ttu-id="b7a51-211">Egy új virtuális hálózat tooan alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7a51-211">Add a NEW Virtual Network tooan App</span></span>
    2) <span data-ttu-id="b7a51-212">Egy meglévő virtuális hálózat tooan alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7a51-212">Add an EXISTING Virtual Network tooan App</span></span>
    3) <span data-ttu-id="b7a51-213">Távolítsa el a virtuális hálózat az alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="b7a51-213">Remove a Virtual Network from an App</span></span>

<span data-ttu-id="b7a51-214">Ez a szakasz többi hello ismerteti ezen három lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="b7a51-214">hello rest of this section explains each of those three options.</span></span>

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a><span data-ttu-id="b7a51-215">Erőforrás-kezelő VNet létrehozása, és azt integrálni</span><span class="sxs-lookup"><span data-stu-id="b7a51-215">Create a Resource Manager VNet and integrate with it</span></span>
<span data-ttu-id="b7a51-216">toocreate, hogy a használt hello Resource Manager üzembe helyezési modellben, és integrálhatja az alkalmazás a új virtuális hálózat kiválasztása **1) hozzáadása egy új virtuális hálózat tooan App**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-216">toocreate a new virtual network that uses hello Resource Manager deployment model and integrate it with your app, select **1) Add a NEW Virtual Network tooan App**.</span></span> <span data-ttu-id="b7a51-217">Ez kérni fogja az hello hello virtuális hálózat nevét.</span><span class="sxs-lookup"><span data-stu-id="b7a51-217">This will prompt you for hello name of hello virtual network.</span></span> <span data-ttu-id="b7a51-218">Abban az esetben, ha ahogyan azt láthatja a beállításokat, a következő hello használt hello nevét, v2pshell.</span><span class="sxs-lookup"><span data-stu-id="b7a51-218">In my case, as you can see in hello following settings, I used hello name, v2pshell.</span></span>

<span data-ttu-id="b7a51-219">hello parancsfájl tájékoztatást nyújt a hello hello virtuális hálózat létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="b7a51-219">hello script gives hello details about hello virtual network that's being created.</span></span> <span data-ttu-id="b7a51-220">Szeretném, ha bármelyik hello értékek módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="b7a51-220">If I want, I can change any of hello values.</span></span> <span data-ttu-id="b7a51-221">A példa végrehajtása a következő beállítások hello rendelkező virtuális hálózatban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="b7a51-221">In this example execution, I created a virtual network that has hello following settings:</span></span>

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

<span data-ttu-id="b7a51-222">Ha azt szeretné, toochange hello értékek bármelyikét, írja be a **Y** és hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="b7a51-222">If you want toochange any of hello values, type **Y** and make hello changes.</span></span> <span data-ttu-id="b7a51-223">Ha elégedett hello virtuális hálózati beállításait, írja be a **N** vagy egyszerűen csak nyomja le az Enter hello beállításainak módosításával kapcsolatos megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="b7a51-223">When you are happy with hello virtual network settings, type **N** or simply press Enter when prompted about changing hello settings.</span></span> <span data-ttu-id="b7a51-224">Ezekből a befejezéséig hello parancsfájl megtudhatja, amit a rendszergazda némelyike "i's végre, amíg nem toocreate hello virtuális hálózati átjáró kezdődik.</span><span class="sxs-lookup"><span data-stu-id="b7a51-224">From there on until completion, hello script will tell you some of what it' i's doing until it starts toocreate hello virtual network gateway.</span></span> <span data-ttu-id="b7a51-225">A lépés akár tooan órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b7a51-225">That step can take up tooan hour.</span></span> <span data-ttu-id="b7a51-226">Ebben a fázisban nem folyamatjelző van, de hello parancsfájl lehetővé teszi, hogy ismeri a hello átjáró létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b7a51-226">There is no progress indicator during this phase, but hello script will let you know when hello gateway has been created.</span></span>

<span data-ttu-id="b7a51-227">Hello parancsfájl befejezése után, akkor megtudhatja, hogy **befejezett**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-227">When hello script finishes, it will say **Finished**.</span></span> <span data-ttu-id="b7a51-228">Ezen a ponton fog hello neve erőforrás-kezelő virtuális hálózat és a kiválasztott beállításokat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-228">At this point, you will have a Resource Manager virtual network that has hello name and settings that you selected.</span></span> <span data-ttu-id="b7a51-229">Az új virtuális hálózat is integrálják az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7a51-229">This new virtual network will also be integrated with your app.</span></span>

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a><span data-ttu-id="b7a51-230">Az alkalmazás integrálja a már meglévő Resource Manager virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="b7a51-230">Integrate your app with a preexisting Resource Manager VNet</span></span>
<span data-ttu-id="b7a51-231">Ha egy már meglévő virtuális hálózatot, ha megadja a pont – hely kapcsolat vagy átjáróként működő nem rendelkező erőforrás-kezelő virtuális hálózat most integrálása, hello parancsprogram beállítja, hogy.</span><span class="sxs-lookup"><span data-stu-id="b7a51-231">When you're integrating with a preexisting virtual network, if you provide a Resource Manager virtual network that doesn’t have a gateway or point-to-site connectivity, hello script will set that up.</span></span> <span data-ttu-id="b7a51-232">Hello virtuális hálózat már rendelkezik állítsa be ezeket a beállításokat, ha hello parancsfájlt halad egyenes toohello alkalmazásintegráció.</span><span class="sxs-lookup"><span data-stu-id="b7a51-232">If hello VNET already has those things set up, hello script goes straight toohello app integration.</span></span> <span data-ttu-id="b7a51-233">toostart ezt a folyamatot, egyszerűen válassza **2) hozzáadása egy meglévő virtuális hálózat tooan App**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-233">toostart this process, simply select **2) Add an EXISTING Virtual Network tooan App**.</span></span>

<span data-ttu-id="b7a51-234">Ez a beállítás csak akkor, ha már létező erőforrás-kezelő virtuális hálózattal rendelkezik, amely hello működik az alkalmazás ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b7a51-234">This option works only if you have a preexisting Resource Manager virtual network that is in hello same subscription as your app.</span></span> <span data-ttu-id="b7a51-235">Miután hello lehetőséget választja, választhat a Resource Manager virtuális hálózatok listájával.</span><span class="sxs-lookup"><span data-stu-id="b7a51-235">After you select hello option, you will be presented with a list of your Resource Manager virtual networks.</span></span>   

    Select a VNET toointegrate with

    1) <span data-ttu-id="b7a51-236">v2demonetwork</span><span class="sxs-lookup"><span data-stu-id="b7a51-236">v2demonetwork</span></span>
    2) <span data-ttu-id="b7a51-237">v2pshell</span><span class="sxs-lookup"><span data-stu-id="b7a51-237">v2pshell</span></span>
    3) <span data-ttu-id="b7a51-238">v2vnetintdemo</span><span class="sxs-lookup"><span data-stu-id="b7a51-238">v2vnetintdemo</span></span>
    4) <span data-ttu-id="b7a51-239">v2asenetwork</span><span class="sxs-lookup"><span data-stu-id="b7a51-239">v2asenetwork</span></span>
    5) <span data-ttu-id="b7a51-240">v2pshell2</span><span class="sxs-lookup"><span data-stu-id="b7a51-240">v2pshell2</span></span>

    <span data-ttu-id="b7a51-241">Válasszon egy lehetőséget: 5</span><span class="sxs-lookup"><span data-stu-id="b7a51-241">Choose an option: 5</span></span>

<span data-ttu-id="b7a51-242">Hello virtuális hálózathoz, amelyet a toointegrate egyszerűen válassza.</span><span class="sxs-lookup"><span data-stu-id="b7a51-242">Simply select hello virtual network that you want toointegrate with.</span></span> <span data-ttu-id="b7a51-243">Ha már van egy átjáró, amely a pont – hely kapcsolat engedélyezve van, hello parancsfájl egyszerűen integrálható az alkalmazás a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-243">If you already have a gateway that has point-to-site connectivity enabled, hello script will simply integrate your app with your virtual network.</span></span> <span data-ttu-id="b7a51-244">Ha nem rendelkezik egy átjárót, szüksége lesz a toospecify hello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="b7a51-244">If you do not have a gateway, you will need toospecify hello gateway subnet.</span></span> <span data-ttu-id="b7a51-245">Az átjáró-alhálózatot a virtuális hálózat címtere kell lennie, és nem lehet a más alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b7a51-245">Your gateway subnet must be in your virtual network address space, and it cannot be in any other subnet.</span></span> <span data-ttu-id="b7a51-246">Ha egy virtuális hálózati átjáró nélkül, és futtassa ezt a lépést, akkor a dolgok néznek ki:</span><span class="sxs-lookup"><span data-stu-id="b7a51-246">If you have a virtual network without a gateway and run this step, things look like this:</span></span>

    This Virtual Network has no gateway. I will need toocreate one.
    Your VNET is in hello address space 172.16.0.0/16, with hello following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

<span data-ttu-id="b7a51-247">Ebben a példában a virtuális hálózati átjáró, amely rendelkezik a következő beállítások hello létrehozott:</span><span class="sxs-lookup"><span data-stu-id="b7a51-247">In this example, I created a virtual network gateway that has hello following settings:</span></span>

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association tooVNET

<span data-ttu-id="b7a51-248">Ha azt szeretné, toochange ezeket a beállításokat, megteheti.</span><span class="sxs-lookup"><span data-stu-id="b7a51-248">If you want toochange any of those settings, you can do so.</span></span> <span data-ttu-id="b7a51-249">Vagy nyomja le az ENTER billentyűt, és hello parancsfájl létrehoz az átjáró és az alkalmazás tooyour virtuális hálózat csatolása.</span><span class="sxs-lookup"><span data-stu-id="b7a51-249">Otherwise, press Enter and hello script will create your gateway and attach your app tooyour virtual network.</span></span> <span data-ttu-id="b7a51-250">hello átjáró létrehozásának ideje még nem állt le egy óra alatt, ha, ezért győződjön meg arról, hogy figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="b7a51-250">hello gateway creation time is still an hour, though, so make sure you keep that in mind.</span></span> <span data-ttu-id="b7a51-251">Ha minden befejeződött, hello parancsfájl megtudhatja, hogy **befejezett**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-251">When everything is finished, hello script will say **Finished**.</span></span>

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a><span data-ttu-id="b7a51-252">Válassza le az alkalmazást egy erőforrás-kezelő virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="b7a51-252">Disconnect your app from a Resource Manager VNet</span></span>
<span data-ttu-id="b7a51-253">Az alkalmazás leválasztása a virtuális hálózat nem hello átjáró le és tiltsa le a pont – hely kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-253">Disconnecting your app from your virtual network does not take down hello gateway or disable point-to-site connectivity.</span></span> <span data-ttu-id="b7a51-254">Előfordulhat, ha minden használni azt máshová.</span><span class="sxs-lookup"><span data-stu-id="b7a51-254">You might, after all, be using it for something else.</span></span> <span data-ttu-id="b7a51-255">Azt is leválasztása nem azt minden más alkalmazás nem hello egy megadott.</span><span class="sxs-lookup"><span data-stu-id="b7a51-255">It also does not disconnect it from any other apps other than hello one you provided.</span></span> <span data-ttu-id="b7a51-256">tooperform Ez a művelet, jelölje be **3) virtuális hálózat eltávolítása egy alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-256">tooperform this action, select **3) Remove a Virtual Network from an App**.</span></span> <span data-ttu-id="b7a51-257">Ha így tesz, látni fogja például ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="b7a51-257">When you do so, you will see something like this:</span></span>

    Currently connected tooVNET v2pshell

    Confirm
    Are you sure you want toodelete hello following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

<span data-ttu-id="b7a51-258">Bár hello parancsfájl feliratú törlés, nem törli hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b7a51-258">Although hello script says delete, it does not delete hello virtual network.</span></span> <span data-ttu-id="b7a51-259">Csak az éppen eltávolítja hello integráció.</span><span class="sxs-lookup"><span data-stu-id="b7a51-259">It’s just removing hello integration.</span></span> <span data-ttu-id="b7a51-260">Miután meggyőződött róla, hogy ez a választható toodo, hello parancs meglehetősen gyorsan dolgoz fel, és jelzi, hogy **igaz** amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="b7a51-260">After you confirm that this is what you want toodo, hello command is processed quite quickly and tells you **True** when it is done.</span></span>

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
