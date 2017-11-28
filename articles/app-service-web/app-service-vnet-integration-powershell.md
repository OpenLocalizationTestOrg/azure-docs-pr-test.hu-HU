---
title: "Csatlakozás egy alkalmazással a virtuális hálózatra a PowerShell használatával"
description: "Leírja, hogyan kell csatlakozni, és a virtuális hálózatok használata a PowerShell használatával"
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
ms.openlocfilehash: 6fae6a6c162fa326161d2b47a259b3151d6e3dd0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a><span data-ttu-id="45f3a-103">Csatlakozás egy alkalmazással a virtuális hálózatra a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="45f3a-103">Connect your app to your virtual network by using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="45f3a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="45f3a-104">Overview</span></span>
<span data-ttu-id="45f3a-105">Az Azure App Service-ben kapcsolódhat az alkalmazás (webes, mobil, vagy API) egy Azure virtuális hálózatot (VNet) az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="45f3a-105">In Azure App Service, you can connect your app (web, mobile, or API) to an Azure virtual network (VNet) in your subscription.</span></span> <span data-ttu-id="45f3a-106">A szolgáltatás virtuális integráció neve.</span><span class="sxs-lookup"><span data-stu-id="45f3a-106">This feature is called VNet Integration.</span></span> <span data-ttu-id="45f3a-107">A virtuális hálózat integrációs szolgáltatás számára nem kell összetéveszthető az App Service Environment-környezet funkció, amely lehetővé teszi a virtuális hálózat egy példányát az Azure App Service futtatását.</span><span class="sxs-lookup"><span data-stu-id="45f3a-107">The VNet Integration feature should not be confused with the App Service Environment feature, which allows you to run an instance of Azure App Service in your virtual network.</span></span>

<span data-ttu-id="45f3a-108">A virtuális hálózat integrációs szolgáltatás számára az új portálon, hogy a virtuális hálózatok, a klasszikus üzembe helyezési modellt vagy az Azure Resource Manager telepítési modell segítségével telepített integrálása használhatja a felhasználói felület (UI) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="45f3a-108">The VNet Integration feature has a user interface (UI) in the new portal that you can use to integrate with virtual networks that are deployed by using either the classic deployment model or the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="45f3a-109">Ha azt szeretné, további információt a szolgáltatást, lásd: [az alkalmazás integrálása az Azure virtuális hálózat](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="45f3a-109">If you want to learn more about the feature, see [Integrate your app with an Azure virtual network](web-sites-integrate-with-vnet.md).</span></span>

<span data-ttu-id="45f3a-110">Ez a cikk van, nem a felhasználói felület használatával kapcsolatban, de ahelyett, hogy engedélyezésével kapcsolatos integrációs PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="45f3a-110">This article is not about how to use the UI but rather about how to enable integration by using PowerShell.</span></span> <span data-ttu-id="45f3a-111">Mivel minden üzembe helyezési modellben a parancsokat, akkor ez a cikk szakasza minden központi telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="45f3a-111">Because the commands for each deployment model are different, this article has a section for each deployment model.</span></span>  

<span data-ttu-id="45f3a-112">Ez a cikk a folytatás előtt győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="45f3a-112">Before you continue with this article, ensure that you have:</span></span>

* <span data-ttu-id="45f3a-113">A legújabb Azure PowerShell SDK telepítve.</span><span class="sxs-lookup"><span data-stu-id="45f3a-113">The latest Azure PowerShell SDK installed.</span></span> <span data-ttu-id="45f3a-114">Az a Webplatform-telepítővel telepíthető.</span><span class="sxs-lookup"><span data-stu-id="45f3a-114">You can install this with the Web Platform Installer.</span></span>
* <span data-ttu-id="45f3a-115">Az Azure App Service egy Standard vagy Premium Termékváltozat futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="45f3a-115">An app in Azure App Service running in a Standard or Premium SKU.</span></span>

## <a name="classic-virtual-networks"></a><span data-ttu-id="45f3a-116">Klasszikus virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="45f3a-116">Classic virtual networks</span></span>
<span data-ttu-id="45f3a-117">Ez a szakasz ismerteti a klasszikus üzembe helyezési modellt használó virtuális hálózatok három feladatok:</span><span class="sxs-lookup"><span data-stu-id="45f3a-117">This section explains three tasks for virtual networks that use the classic deployment model:</span></span>

1. <span data-ttu-id="45f3a-118">Az alkalmazás kapcsolódni tudjon egy már meglévő virtuális hálózati átjáró és a pont – hely kapcsolat van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="45f3a-118">Connect your app to a preexisting virtual network that has a gateway and is configured for point-to-site connectivity.</span></span>
2. <span data-ttu-id="45f3a-119">Az alkalmazás a virtuális hálózati integráció adatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="45f3a-119">Update your virtual network integration information for your app.</span></span>
3. <span data-ttu-id="45f3a-120">Az alkalmazás leválasztása a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-120">Disconnect your app from your virtual network.</span></span>

### <a name="connect-an-app-to-a-classic-vnet"></a><span data-ttu-id="45f3a-121">Az alkalmazás kapcsolódni tudjon egy klasszikus virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="45f3a-121">Connect an app to a classic VNet</span></span>
<span data-ttu-id="45f3a-122">Egy alkalmazás egy virtuális hálózathoz való kapcsolódáshoz, kövesse a fenti három lépést:</span><span class="sxs-lookup"><span data-stu-id="45f3a-122">To connect an app to a virtual network, follow these three steps:</span></span>

1. <span data-ttu-id="45f3a-123">Deklarálja azt a webalkalmazást, hogy az csatlakozni fog egy adott virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="45f3a-123">Declare to the web app that it will be joining a particular virtual network.</span></span> <span data-ttu-id="45f3a-124">Az alkalmazás egy pont – hely kapcsolat a virtuális hálózathoz megadott tanúsítványt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="45f3a-124">The app will generate a certificate that will be given to the virtual network for point-to-site connectivity.</span></span>
2. <span data-ttu-id="45f3a-125">A webes alkalmazás-tanúsítvány feltöltése a virtuális hálózathoz, és majd lekérheti a pont-pont VPN csomag URI.</span><span class="sxs-lookup"><span data-stu-id="45f3a-125">Upload the web app certificate to the virtual network, and then retrieve the point-to-site VPN package URI.</span></span>
3. <span data-ttu-id="45f3a-126">A web app virtuális hálózati kapcsolat frissítése a pont-pont csomag URI.</span><span class="sxs-lookup"><span data-stu-id="45f3a-126">Update the web app's virtual network connection with the point-to-site package URI.</span></span>

<span data-ttu-id="45f3a-127">Az első és harmadik lépéseket teljes parancsfájlos, de a második lépés szükséges a portálon, vagy hajtsa végre a hozzáférést egy egyszeri, manuális művelet **PUT** vagy **javítás** műveletek a virtuális hálózati Azure Resource Manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="45f3a-127">The first and third steps are fully scriptable, but the second step requires a one-time, manual action through the portal, or access to perform **PUT** or **PATCH** actions on the virtual network Azure Resource Manager endpoint.</span></span> <span data-ttu-id="45f3a-128">Kérje az Azure támogatási ez engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="45f3a-128">Contact Azure Support to have this enabled.</span></span> <span data-ttu-id="45f3a-129">Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e a klasszikus virtuális hálózatot, amelyen már engedélyezve van a pont – hely kapcsolat és a telepített átjárót.</span><span class="sxs-lookup"><span data-stu-id="45f3a-129">Before you start, make sure that you have a classic virtual network that has point-to-site connectivity already enabled and a deployed gateway.</span></span> <span data-ttu-id="45f3a-130">Az átjáró létrehozásához, és engedélyezze a pont – hely kapcsolatot, akkor portált kell használniuk a részben ismertetett módon [VPN-átjáró létrehozása][createvpngateway].</span><span class="sxs-lookup"><span data-stu-id="45f3a-130">To create the gateway and enable point-to-site connectivity, you need to use the portal as described at [Creating a VPN gateway][createvpngateway].</span></span>

<span data-ttu-id="45f3a-131">A klasszikus virtuális hálózaton kell lennie az App Service-csomag, amely tárolja az alkalmazást, amely integrációhoz tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="45f3a-131">The classic virtual network needs to be in the same subscription as your App Service plan that holds the app that you are integrating with.</span></span>

##### <a name="set-up-azure-powershell-sdk"></a><span data-ttu-id="45f3a-132">Azure PowerShell SDK beállítása</span><span class="sxs-lookup"><span data-stu-id="45f3a-132">Set up Azure PowerShell SDK</span></span>
<span data-ttu-id="45f3a-133">Nyisson meg egy PowerShell-ablakot, és állítsa be az Azure-fiókja és -előfizetést használatával:</span><span class="sxs-lookup"><span data-stu-id="45f3a-133">Open a PowerShell window and set up your Azure account and subscription by using:</span></span>

    Login-AzureRmAccount

<span data-ttu-id="45f3a-134">Ez a parancs jelenít meg az Azure hitelesítő adatainak lekérése nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="45f3a-134">That command will open a prompt to get your Azure credentials.</span></span> <span data-ttu-id="45f3a-135">Miután bejelentkezik, az alábbi parancsok egyikét segítségével válassza ki a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="45f3a-135">After you sign in, use either of the following commands to select the subscription that you want to use.</span></span> <span data-ttu-id="45f3a-136">Győződjön meg arról, hogy az előfizetés, amelyek a virtuális hálózat és az App Service-csomagot használ-e.</span><span class="sxs-lookup"><span data-stu-id="45f3a-136">Make sure that you are using the subscription that your virtual network and App Service plan are in.</span></span>

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

<span data-ttu-id="45f3a-137">vagy</span><span class="sxs-lookup"><span data-stu-id="45f3a-137">or</span></span>

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a><span data-ttu-id="45f3a-138">A cikk ezt használja változók</span><span class="sxs-lookup"><span data-stu-id="45f3a-138">Variables used in this article</span></span>
<span data-ttu-id="45f3a-139">Egyszerűbbé teheti a parancsok, helyezünk egy **$Configuration** PowerShell változó az adott konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="45f3a-139">To simplify commands, we will set a **$Configuration** PowerShell variable with the specific configuration.</span></span>

<span data-ttu-id="45f3a-140">Egy változót az alábbiak szerint állíthatja a PowerShellben a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="45f3a-140">Set a variable as follows in PowerShell with the following parameters:</span></span>

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

<span data-ttu-id="45f3a-141">Az alkalmazás bármely szóközök nélkül a helyen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="45f3a-141">The app location should be the location without any spaces.</span></span> <span data-ttu-id="45f3a-142">USA nyugati régiója például westus.</span><span class="sxs-lookup"><span data-stu-id="45f3a-142">For example, West US is westus.</span></span>

    $Configuration.WebAppLocation = "[Your web app Location]"

<span data-ttu-id="45f3a-143">A következő elem, ahol lehet írni a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="45f3a-143">The next item is where the certificate should be written.</span></span> <span data-ttu-id="45f3a-144">Azt a helyi számítógépen írható elérési útnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="45f3a-144">It should be a writable path on your local computer.</span></span> <span data-ttu-id="45f3a-145">Ügyeljen arra, hogy tartalmazzák a .cer végén.</span><span class="sxs-lookup"><span data-stu-id="45f3a-145">Make sure to include .cer at the end.</span></span>

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

<span data-ttu-id="45f3a-146">Akkor állítsa be parancsot kell beírnia **$Configuration**.</span><span class="sxs-lookup"><span data-stu-id="45f3a-146">To see what you set, type **$Configuration**.</span></span>

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

<span data-ttu-id="45f3a-147">Ez a szakasz a többi feltételezi, hogy rendelkezik-e csak leírtak változók.</span><span class="sxs-lookup"><span data-stu-id="45f3a-147">The rest of this section assumes that you have a variable created as just described.</span></span>

##### <a name="declare-the-virtual-network-to-the-app"></a><span data-ttu-id="45f3a-148">Deklarálja az alkalmazás a virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="45f3a-148">Declare the virtual network to the app</span></span>
<span data-ttu-id="45f3a-149">A következő paranccsal kérje meg az alkalmazást, hogy azt fogja használni az adott virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="45f3a-149">Use the following command to tell the app that it will be using this particular virtual network.</span></span> <span data-ttu-id="45f3a-150">Ez azt eredményezi, az alkalmazásnak, hogy a szükséges tanúsítványok létrehozása:</span><span class="sxs-lookup"><span data-stu-id="45f3a-150">This will cause the app to generate necessary certificates:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

<span data-ttu-id="45f3a-151">Ha ez a parancs végrehajtása sikeres, **$vnet** rendelkeznie kell egy **tulajdonságok** változó azt.</span><span class="sxs-lookup"><span data-stu-id="45f3a-151">If this command succeeds, **$vnet** should have a **Properties** variable in it.</span></span> <span data-ttu-id="45f3a-152">A **tulajdonságok** változót kell tartalmaznia, a tanúsítvány-ujjlenyomat, mind a tanúsítványának adatait.</span><span class="sxs-lookup"><span data-stu-id="45f3a-152">The **Properties** variable should contain both a certificate thumbprint and the certificate data.</span></span>

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a><span data-ttu-id="45f3a-153">A webes alkalmazás-tanúsítvány feltöltése a virtuális hálózathoz</span><span class="sxs-lookup"><span data-stu-id="45f3a-153">Upload the web app certificate to the virtual network</span></span>
<span data-ttu-id="45f3a-154">Kézi, egyszeri lépés akkor szükséges, az egyes előfizetés és a virtuális hálózati kombinációja.</span><span class="sxs-lookup"><span data-stu-id="45f3a-154">A manual, one-time step is required for each subscription and virtual network combination.</span></span> <span data-ttu-id="45f3a-155">Ez azt jelenti, hogy ha a virtuális hálózati alkalmazások az előfizetést A kapcsolódik, akkor csak egyszer, függetlenül attól, hogy hány alkalmazások konfigurálása ehhez a lépéshez.</span><span class="sxs-lookup"><span data-stu-id="45f3a-155">That is, if you are connecting apps in Subscription A to Virtual Network A, you will need to do this step only once regardless of how many apps you configure.</span></span> <span data-ttu-id="45f3a-156">Ha egy másik virtuális hálózathoz ad hozzá egy új alkalmazást, szüksége újra.</span><span class="sxs-lookup"><span data-stu-id="45f3a-156">If you are adding a new app to another virtual network, you'll need to do this again.</span></span> <span data-ttu-id="45f3a-157">Ennek oka, hogy a tanúsítványok készletét jön létre az Azure App Service előfizetési szinten, és a készletben jön létre egyszer minden virtuális hálózathoz, amelyek csatlakozni fognak az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="45f3a-157">The reason for this is that a set of certificates is generated at a subscription level in Azure App Service, and the set is generated once for each virtual network that the apps will connect to.</span></span>

<span data-ttu-id="45f3a-158">A tanúsítványok lesznek rendelkezik már be van állítva Ha követte ezeket a lépéseket, vagy ha a portál használatával integrálva van az azonos virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="45f3a-158">The certificates will have already been set if you followed these steps or if you integrated with the same virtual network by using the portal.</span></span>

<span data-ttu-id="45f3a-159">Az első lépés a .cer-fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="45f3a-159">The first step is to generate the .cer file.</span></span> <span data-ttu-id="45f3a-160">A második lépésben fel kell töltenie a .cer fájlt a virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="45f3a-160">The second step is to upload the .cer file to your virtual network.</span></span> <span data-ttu-id="45f3a-161">Az API-hívás az előző lépésben a .cer-fájl létrehozásához futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-161">To generate the .cer file from the API call in the earlier step, run the following commands.</span></span>

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

<span data-ttu-id="45f3a-162">A tanúsítvány a helyen találhatók, amely **$Configuration.GeneratedCertificatePath** határozza meg.</span><span class="sxs-lookup"><span data-stu-id="45f3a-162">The certificate will be found in the location that **$Configuration.GeneratedCertificatePath** specifies.</span></span>

<span data-ttu-id="45f3a-163">A tanúsítvány manuális feltöltése, használja a [Azure-portálon] [ azureportal] és **Tallózás virtuális hálózat (klasszikus)** > **VPN-kapcsolatok** > **pont-pont** > **tanúsítványok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="45f3a-163">To upload the certificate manually, use the [Azure portal][azureportal] and **Browse Virtual Network (classic)** > **VPN connections** > **Point-to-site** > **Manage certificates**.</span></span> <span data-ttu-id="45f3a-164">Itt a tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="45f3a-164">From here, upload your certificate.</span></span>

##### <a name="get-the-point-to-site-package"></a><span data-ttu-id="45f3a-165">A pont-pont csomag</span><span class="sxs-lookup"><span data-stu-id="45f3a-165">Get the point-to-site package</span></span>
<span data-ttu-id="45f3a-166">A következő lépése a virtuális hálózati kapcsolat a webalkalmazás beállítása, hogy a pont-pont csomagjának és adja meg a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="45f3a-166">The next step in setting up a virtual network connection on a web app is to get the point-to-site package and provide it to your web app.</span></span>

<span data-ttu-id="45f3a-167">A következő sablon mentése nevű GetNetworkPackageUri.json valahol a számítógépre, például C:\Azure\Templates\GetNetworkPackageUri.json fájlba.</span><span class="sxs-lookup"><span data-stu-id="45f3a-167">Save the following template to a file called GetNetworkPackageUri.json somewhere on your computer, for example, C:\Azure\Templates\GetNetworkPackageUri.json.</span></span>

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


<span data-ttu-id="45f3a-168">A megadott bemeneti paraméterek:</span><span class="sxs-lookup"><span data-stu-id="45f3a-168">Set input parameters:</span></span>

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

<span data-ttu-id="45f3a-169">A parancsprogram hívása:</span><span class="sxs-lookup"><span data-stu-id="45f3a-169">Call the script:</span></span>

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


<span data-ttu-id="45f3a-170">A változó **$output. Outputs.packageUri** mostantól tartalmazza a csomag URI-t kell megadni a webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="45f3a-170">The variable **$output.Outputs.packageUri** will now contain the package URI to be given to your web app.</span></span>

##### <a name="upload-the-point-to-site-package-to-your-app"></a><span data-ttu-id="45f3a-171">A pont-pont csomag feltölteni az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="45f3a-171">Upload the point-to-site package to your app</span></span>
<span data-ttu-id="45f3a-172">Az utolsó lépés, hogy adja meg az alkalmazás ezzel a csomaggal.</span><span class="sxs-lookup"><span data-stu-id="45f3a-172">The final step is to provide the app with this package.</span></span> <span data-ttu-id="45f3a-173">Egyszerűen futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="45f3a-173">Simply run the next command:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

<span data-ttu-id="45f3a-174">Ha a rendszer megkérdezi, hogy ellenőrizze, hogy valamelyik meglévő erőforrására másolattal, győződjön meg arról, hogy engedélyezi-e.</span><span class="sxs-lookup"><span data-stu-id="45f3a-174">If a message asks you to confirm that you are overwriting an existing resource, make sure to allow it.</span></span>

<span data-ttu-id="45f3a-175">Után ez a parancs sikeres, az alkalmazás most a virtuális hálózathoz kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="45f3a-175">After this command succeeds, your app should now be connected to the virtual network.</span></span> <span data-ttu-id="45f3a-176">Erősítse meg a sikeres, az alkalmazás-konzolon, és írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="45f3a-176">To confirm success, go to your app console, and type the following:</span></span>

    SET WEBSITE_

<span data-ttu-id="45f3a-177">Ha egy környezeti változó neve, amely rendelkezik egy érték, amely megegyezik-e a cél virtuális hálózat WEBSITE_VNETNAME, az összes konfiguráció sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="45f3a-177">If there is an environment variable called WEBSITE_VNETNAME that has a value that matches the name of the target virtual network, all configurations have succeeded.</span></span>

### <a name="update-classic-vnet-integration-information"></a><span data-ttu-id="45f3a-178">Klasszikus virtuális integráció frissítése</span><span class="sxs-lookup"><span data-stu-id="45f3a-178">Update classic VNet integration information</span></span>
<span data-ttu-id="45f3a-179">Frissítéséhez, vagy el az újraszinkronizálást, az adatait, egyszerűen ismételje meg a integrálását az elsőként létrehozott követett.</span><span class="sxs-lookup"><span data-stu-id="45f3a-179">To update or resync your information, simply repeat the steps that you followed when you created the integration in the first place.</span></span> <span data-ttu-id="45f3a-180">Ezek a lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="45f3a-180">Those steps are:</span></span>

1. <span data-ttu-id="45f3a-181">Adja meg a konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-181">Define your configuration information.</span></span>
2. <span data-ttu-id="45f3a-182">Deklarálja az alkalmazás a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-182">Declare the virtual network to the app.</span></span>
3. <span data-ttu-id="45f3a-183">A pont-pont csomag beolvasása.</span><span class="sxs-lookup"><span data-stu-id="45f3a-183">Get the point-to-site package.</span></span>
4. <span data-ttu-id="45f3a-184">A pont-pont csomag feltölteni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="45f3a-184">Upload the point-to-site package to your app.</span></span>

### <a name="disconnect-your-app-from-a-classic-vnet"></a><span data-ttu-id="45f3a-185">Válassza le az alkalmazást egy klasszikus virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="45f3a-185">Disconnect your app from a classic VNet</span></span>
<span data-ttu-id="45f3a-186">Válassza le az alkalmazást, a virtuális hálózati integráció során megadott konfigurációs információkat kell.</span><span class="sxs-lookup"><span data-stu-id="45f3a-186">To disconnect the app, you need the configuration information that was set during virtual network integration.</span></span> <span data-ttu-id="45f3a-187">Az információk, nincs majd az alkalmazás bontja a virtuális hálózat egy parancs.</span><span class="sxs-lookup"><span data-stu-id="45f3a-187">Using that information, there is then one command to disconnect your app from your virtual network.</span></span>

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a><span data-ttu-id="45f3a-188">Erőforrás-kezelő virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="45f3a-188">Resource Manager virtual networks</span></span>
<span data-ttu-id="45f3a-189">Erőforrás-kezelő virtuális hálózat is létezik Azure Resource Manager API-k, amelyek egyszerűsítése bizonyos folyamatok klasszikus virtuális hálózatokat képest.</span><span class="sxs-lookup"><span data-stu-id="45f3a-189">Resource Manager virtual networks have Azure Resource Manager APIs, which simplify some processes when compared with classic virtual networks.</span></span> <span data-ttu-id="45f3a-190">Van olyan parancsfájlt, amely segít a hajtsa végre a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="45f3a-190">We have a script that will help you complete the following tasks:</span></span>

* <span data-ttu-id="45f3a-191">Hozzon létre egy erőforrás-kezelő virtuális hálózatot, és az alkalmazás integrálható.</span><span class="sxs-lookup"><span data-stu-id="45f3a-191">Create a Resource Manager virtual network and integrate your app with it.</span></span>
* <span data-ttu-id="45f3a-192">Hozzon létre egy pont – hely kapcsolat konfigurálása az erőforrás-kezelő már meglévő virtuális hálózat és az alkalmazás integrálható.</span><span class="sxs-lookup"><span data-stu-id="45f3a-192">Create a gateway, configure point-to-site connectivity in a preexisting Resource Manager virtual network, and then integrate your app with it.</span></span>
* <span data-ttu-id="45f3a-193">Az alkalmazás integrálja egy már létező olyan erőforrás-kezelő virtuális hálózattal, amelyen egy átjáró és a pont – hely kapcsolat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="45f3a-193">Integrate your app with a preexisting Resource Manager virtual network that has a gateway and point-to-site connectivity enabled.</span></span>
* <span data-ttu-id="45f3a-194">Az alkalmazás leválasztása a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-194">Disconnect your app from your virtual network.</span></span>

### <a name="resource-manager-vnet-app-service-integration-script"></a><span data-ttu-id="45f3a-195">Erőforrás-kezelő virtuális hálózat App Service integrációs parancsfájlját</span><span class="sxs-lookup"><span data-stu-id="45f3a-195">Resource Manager VNet App Service integration script</span></span>
<span data-ttu-id="45f3a-196">Másolja a következő parancsfájlt, és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="45f3a-196">Copy the following script and save it to a file.</span></span> <span data-ttu-id="45f3a-197">Ha nem szeretné a parancsfájlt, nyugodtan megtudjuk módjával dolgot beállítása egy erőforrás-kezelő virtuális hálózattal.</span><span class="sxs-lookup"><span data-stu-id="45f3a-197">If you don’t want to use the script, feel free to learn from it to see how to set things up with a Resource Manager virtual network.</span></span>

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

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
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
            Write-Host "Currently, I will create a VNET with the following settings:"
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
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

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

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at the start and the end of the URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
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
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

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

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
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
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
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

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at the start and the end of the URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put the VPN client configuration package onto the App
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
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
            else
        {
            Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
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

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

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

<span data-ttu-id="45f3a-198">A parancsfájl másolatának mentése.</span><span class="sxs-lookup"><span data-stu-id="45f3a-198">Save a copy of the script.</span></span> <span data-ttu-id="45f3a-199">Ebben a cikkben V2VnetAllinOne.ps1 nevezik, de egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="45f3a-199">In this article, it is called V2VnetAllinOne.ps1, but you can use another name.</span></span> <span data-ttu-id="45f3a-200">Nincsenek ehhez a parancsprogramhoz argumentumok.</span><span class="sxs-lookup"><span data-stu-id="45f3a-200">There are no arguments for this script.</span></span> <span data-ttu-id="45f3a-201">Egyszerűen futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="45f3a-201">You simply run it.</span></span> <span data-ttu-id="45f3a-202">A parancsfájl fog tenni elsőként felszólítja, hogy jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="45f3a-202">The first thing the script will do is prompt you to sign in.</span></span> <span data-ttu-id="45f3a-203">Miután bejelentkezik, a parancsfájl lekérdezi a fiók adatait, és előfizetések listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="45f3a-203">After you sign in, the script gets details about your account and returns a list of subscriptions.</span></span> <span data-ttu-id="45f3a-204">A kérelem a hitelesítő adatok nem számítva, a kezdeti parancsfájl végrehajtása néz ki:</span><span class="sxs-lookup"><span data-stu-id="45f3a-204">Not counting the request for your credentials, the initial script execution looks like this:</span></span>

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) <span data-ttu-id="45f3a-205">Bemutató előfizetés (af5358e1-acac-2c90-a9eb-722190abf47a)</span><span class="sxs-lookup"><span data-stu-id="45f3a-205">Demo Subscription (af5358e1-acac-2c90-a9eb-722190abf47a)</span></span>
    2) <span data-ttu-id="45f3a-206">MS-teszt (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span><span class="sxs-lookup"><span data-stu-id="45f3a-206">MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span></span>
    3) <span data-ttu-id="45f3a-207">Lila bemutató előfizetés (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span><span class="sxs-lookup"><span data-stu-id="45f3a-207">Purple Demo Subscription (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span></span>

    <span data-ttu-id="45f3a-208">Válasszon egy lehetőséget: 3</span><span class="sxs-lookup"><span data-stu-id="45f3a-208">Choose an option: 3</span></span>

    <span data-ttu-id="45f3a-209">Fiók: ccompy@microsoft.com környezet: AzureCloud előfizetés: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d bérlői: 722278f-fef1-499f-91ab-2323d011db47</span><span class="sxs-lookup"><span data-stu-id="45f3a-209">Account      : ccompy@microsoft.com Environment  : AzureCloud Subscription : 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Tenant       : 722278f-fef1-499f-91ab-2323d011db47</span></span>

    <span data-ttu-id="45f3a-210">Adja meg az alkalmazás az erőforráscsoport: hcdemo-rg adja meg az alkalmazás nevét: v2vnetpowershell mi történjen a teendő?</span><span class="sxs-lookup"><span data-stu-id="45f3a-210">Please enter the Resource Group of your App: hcdemo-rg Please enter the Name of your App: v2vnetpowershell What do you want to do?</span></span>

    1) <span data-ttu-id="45f3a-211">ÚJ virtuális hálózat hozzáadása egy alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="45f3a-211">Add a NEW Virtual Network to an App</span></span>
    2) <span data-ttu-id="45f3a-212">Egy meglévő virtuális hálózat hozzáadása egy alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="45f3a-212">Add an EXISTING Virtual Network to an App</span></span>
    3) <span data-ttu-id="45f3a-213">Távolítsa el a virtuális hálózat az alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="45f3a-213">Remove a Virtual Network from an App</span></span>

<span data-ttu-id="45f3a-214">Ez a szakasz a többi ismerteti ezen három lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="45f3a-214">The rest of this section explains each of those three options.</span></span>

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a><span data-ttu-id="45f3a-215">Erőforrás-kezelő VNet létrehozása, és azt integrálni</span><span class="sxs-lookup"><span data-stu-id="45f3a-215">Create a Resource Manager VNet and integrate with it</span></span>
<span data-ttu-id="45f3a-216">A Resource Manager üzembe helyezési modellel használó új virtuális hálózat létrehozásához, és integrálhatja a az alkalmazás, válassza ki a **1) egy új virtuális hálózat hozzáadása egy alkalmazáshoz**.</span><span class="sxs-lookup"><span data-stu-id="45f3a-216">To create a new virtual network that uses the Resource Manager deployment model and integrate it with your app, select **1) Add a NEW Virtual Network to an App**.</span></span> <span data-ttu-id="45f3a-217">Ez kérni fogja a virtuális hálózat nevét.</span><span class="sxs-lookup"><span data-stu-id="45f3a-217">This will prompt you for the name of the virtual network.</span></span> <span data-ttu-id="45f3a-218">Abban az esetben, ha ahogy látja, a következő beállításokat, a használt a név, v2pshell.</span><span class="sxs-lookup"><span data-stu-id="45f3a-218">In my case, as you can see in the following settings, I used the name, v2pshell.</span></span>

<span data-ttu-id="45f3a-219">A parancsfájl tájékoztatást nyújt a a kiválasztott virtuális hálózat létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="45f3a-219">The script gives the details about the virtual network that's being created.</span></span> <span data-ttu-id="45f3a-220">Ha szeretné, módosíthatók valamely értékét.</span><span class="sxs-lookup"><span data-stu-id="45f3a-220">If I want, I can change any of the values.</span></span> <span data-ttu-id="45f3a-221">A példa végrehajtása a létrehozott egy virtuális hálózatot, a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="45f3a-221">In this example execution, I created a virtual network that has the following settings:</span></span>

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

<span data-ttu-id="45f3a-222">Ha valamely értékét módosítani kívánja, írja be a **Y** , és hajtsa végre a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-222">If you want to change any of the values, type **Y** and make the changes.</span></span> <span data-ttu-id="45f3a-223">Ha elégedett a virtuális hálózati beállításait, írja be a **N** vagy egyszerűen csak nyomja le az ENTER billentyűt, amikor a rendszer kéri, a beállítások módosításával.</span><span class="sxs-lookup"><span data-stu-id="45f3a-223">When you are happy with the virtual network settings, type **N** or simply press Enter when prompted about changing the settings.</span></span> <span data-ttu-id="45f3a-224">Innen a befejezéséig, a parancsfájl megtudhatja, amit a rendszergazda némelyike "i's egészen a virtuális hálózati átjáró létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="45f3a-224">From there on until completion, the script will tell you some of what it' i's doing until it starts to create the virtual network gateway.</span></span> <span data-ttu-id="45f3a-225">A lépés egy óráig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-225">That step can take up to an hour.</span></span> <span data-ttu-id="45f3a-226">Ebben a fázisban nem folyamatjelző van, de a parancsfájl lehetővé teszi, hogy tudja, hogy az átjáró létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="45f3a-226">There is no progress indicator during this phase, but the script will let you know when the gateway has been created.</span></span>

<span data-ttu-id="45f3a-227">A parancsfájl befejezése után, akkor megtudhatja, hogy **befejezett**.</span><span class="sxs-lookup"><span data-stu-id="45f3a-227">When the script finishes, it will say **Finished**.</span></span> <span data-ttu-id="45f3a-228">Ezen a ponton, amelyen a nevét, és a megadott beállításokat, erőforrás-kezelő virtuális hálózat fog.</span><span class="sxs-lookup"><span data-stu-id="45f3a-228">At this point, you will have a Resource Manager virtual network that has the name and settings that you selected.</span></span> <span data-ttu-id="45f3a-229">Az új virtuális hálózat is integrálják az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="45f3a-229">This new virtual network will also be integrated with your app.</span></span>

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a><span data-ttu-id="45f3a-230">Az alkalmazás integrálja a már meglévő Resource Manager virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="45f3a-230">Integrate your app with a preexisting Resource Manager VNet</span></span>
<span data-ttu-id="45f3a-231">Ha egy már meglévő virtuális hálózatot, ha megadja a pont – hely kapcsolat vagy átjáróként működő nem rendelkező erőforrás-kezelő virtuális hálózat most integrálása, a parancsfájl beállításához nyújt útmutatást, amelyek.</span><span class="sxs-lookup"><span data-stu-id="45f3a-231">When you're integrating with a preexisting virtual network, if you provide a Resource Manager virtual network that doesn’t have a gateway or point-to-site connectivity, the script will set that up.</span></span> <span data-ttu-id="45f3a-232">Ha a virtuális hálózat már rendelkezik állítsa be ezeket a beállításokat, a parancsfájl ugrik rögtön az alkalmazásintegráció.</span><span class="sxs-lookup"><span data-stu-id="45f3a-232">If the VNET already has those things set up, the script goes straight to the app integration.</span></span> <span data-ttu-id="45f3a-233">Ez a folyamat indításához egyszerűen válassza **2) egy meglévő virtuális hálózat hozzáadása egy alkalmazáshoz**.</span><span class="sxs-lookup"><span data-stu-id="45f3a-233">To start this process, simply select **2) Add an EXISTING Virtual Network to an App**.</span></span>

<span data-ttu-id="45f3a-234">Ez a beállítás csak akkor, ha már létező erőforrás-kezelő virtuális hálózattal rendelkezik, amely ugyanazt az előfizetést, az alkalmazás működik.</span><span class="sxs-lookup"><span data-stu-id="45f3a-234">This option works only if you have a preexisting Resource Manager virtual network that is in the same subscription as your app.</span></span> <span data-ttu-id="45f3a-235">A beállítást, akkor megjelenik a Resource Manager virtuális hálózatokból álló listát.</span><span class="sxs-lookup"><span data-stu-id="45f3a-235">After you select the option, you will be presented with a list of your Resource Manager virtual networks.</span></span>   

    Select a VNET to integrate with

    1) <span data-ttu-id="45f3a-236">v2demonetwork</span><span class="sxs-lookup"><span data-stu-id="45f3a-236">v2demonetwork</span></span>
    2) <span data-ttu-id="45f3a-237">v2pshell</span><span class="sxs-lookup"><span data-stu-id="45f3a-237">v2pshell</span></span>
    3) <span data-ttu-id="45f3a-238">v2vnetintdemo</span><span class="sxs-lookup"><span data-stu-id="45f3a-238">v2vnetintdemo</span></span>
    4) <span data-ttu-id="45f3a-239">v2asenetwork</span><span class="sxs-lookup"><span data-stu-id="45f3a-239">v2asenetwork</span></span>
    5) <span data-ttu-id="45f3a-240">v2pshell2</span><span class="sxs-lookup"><span data-stu-id="45f3a-240">v2pshell2</span></span>

    <span data-ttu-id="45f3a-241">Válasszon egy lehetőséget: 5</span><span class="sxs-lookup"><span data-stu-id="45f3a-241">Choose an option: 5</span></span>

<span data-ttu-id="45f3a-242">Egyszerűen jelölje ki a virtuális hálózat, amelyet integrálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="45f3a-242">Simply select the virtual network that you want to integrate with.</span></span> <span data-ttu-id="45f3a-243">Ha már van egy átjáró, amely a pont – hely kapcsolat engedélyezve van, a parancsfájl egyszerűen integrálható az alkalmazás a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-243">If you already have a gateway that has point-to-site connectivity enabled, the script will simply integrate your app with your virtual network.</span></span> <span data-ttu-id="45f3a-244">Ha nem rendelkezik egy átjárót, akkor adja meg az átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="45f3a-244">If you do not have a gateway, you will need to specify the gateway subnet.</span></span> <span data-ttu-id="45f3a-245">Az átjáró-alhálózatot a virtuális hálózat címtere kell lennie, és nem lehet a más alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="45f3a-245">Your gateway subnet must be in your virtual network address space, and it cannot be in any other subnet.</span></span> <span data-ttu-id="45f3a-246">Ha egy virtuális hálózati átjáró nélkül, és futtassa ezt a lépést, akkor a dolgok néznek ki:</span><span class="sxs-lookup"><span data-stu-id="45f3a-246">If you have a virtual network without a gateway and run this step, things look like this:</span></span>

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

<span data-ttu-id="45f3a-247">Ebben a példában létrehozott egy virtuális hálózati átjáró a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="45f3a-247">In this example, I created a virtual network gateway that has the following settings:</span></span>

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

<span data-ttu-id="45f3a-248">Ha szeretné módosítani ezeket a beállításokat, megteheti.</span><span class="sxs-lookup"><span data-stu-id="45f3a-248">If you want to change any of those settings, you can do so.</span></span> <span data-ttu-id="45f3a-249">Vagy nyomja le az ENTER billentyűt, és a parancsfájl létrehoz az átjáró és az alkalmazás csatlakoztatása a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-249">Otherwise, press Enter and the script will create your gateway and attach your app to your virtual network.</span></span> <span data-ttu-id="45f3a-250">Az átjáró létrehozása idő továbbra is egy óra alatt, ha, ezért győződjön meg arról, hogy figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="45f3a-250">The gateway creation time is still an hour, though, so make sure you keep that in mind.</span></span> <span data-ttu-id="45f3a-251">Ha minden befejeződött, a parancsfájl megtudhatja, hogy **befejezett**.</span><span class="sxs-lookup"><span data-stu-id="45f3a-251">When everything is finished, the script will say **Finished**.</span></span>

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a><span data-ttu-id="45f3a-252">Válassza le az alkalmazást egy erőforrás-kezelő virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="45f3a-252">Disconnect your app from a Resource Manager VNet</span></span>
<span data-ttu-id="45f3a-253">Az alkalmazás leválasztása a virtuális hálózat nem az átjáró le és tiltsa le a pont – hely kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-253">Disconnecting your app from your virtual network does not take down the gateway or disable point-to-site connectivity.</span></span> <span data-ttu-id="45f3a-254">Előfordulhat, ha minden használni azt máshová.</span><span class="sxs-lookup"><span data-stu-id="45f3a-254">You might, after all, be using it for something else.</span></span> <span data-ttu-id="45f3a-255">Azt is nem válassza le azt minden más alkalmazás nem a megadott.</span><span class="sxs-lookup"><span data-stu-id="45f3a-255">It also does not disconnect it from any other apps other than the one you provided.</span></span> <span data-ttu-id="45f3a-256">Ez a művelet elvégzéséhez válasszon **3) virtuális hálózat eltávolítása egy alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="45f3a-256">To perform this action, select **3) Remove a Virtual Network from an App**.</span></span> <span data-ttu-id="45f3a-257">Ha így tesz, látni fogja például ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="45f3a-257">When you do so, you will see something like this:</span></span>

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

<span data-ttu-id="45f3a-258">A parancsfájl törlése szerint, de nem törli a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="45f3a-258">Although the script says delete, it does not delete the virtual network.</span></span> <span data-ttu-id="45f3a-259">Csak az éppen eltávolítja az integráció.</span><span class="sxs-lookup"><span data-stu-id="45f3a-259">It’s just removing the integration.</span></span> <span data-ttu-id="45f3a-260">Miután meggyőződött róla, hogy ez mit kíván tenni a, a parancs meglehetősen gyorsan dolgoz fel, és jelzi, hogy **igaz** amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="45f3a-260">After you confirm that this is what you want to do, the command is processed quite quickly and tells you **True** when it is done.</span></span>

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
