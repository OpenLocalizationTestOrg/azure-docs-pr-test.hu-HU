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
# <a name="connect-your-app-tooyour-virtual-network-by-using-powershell"></a>Az alkalmazás tooyour virtuális hálózati csatlakoztatása a PowerShell használatával
## <a name="overview"></a>Áttekintés
Az Azure App Service szolgáltatásban csatlakoztathatja az alkalmazás (webes, mobil, vagy API) tooan Azure virtuális hálózathoz (VNet) az előfizetésben. A szolgáltatás virtuális integráció neve. hello VNet integrációs szolgáltatás számára nem keverendő kell hello App Service Environment-környezet szolgáltatással, amely lehetővé teszi a virtuális hálózat az Azure App Service példányának toorun.

hello VNet integrációs szolgáltatás számára van a felhasználói felület (UI) az új portálon hello használható toointegrate hello klasszikus üzembe helyezési modellel vagy hello Azure Resource Manager üzembe helyezési modellben telepített virtuális hálózatokat. Ha azt szeretné, hogy hello funkcióval kapcsolatban további toolearn, [az alkalmazás integrálása az Azure virtuális hálózat](web-sites-integrate-with-vnet.md).

Ez a cikk nem kapcsolatos hogyan toouse hello felhasználói felület, de ahelyett, hogy arról, hogyan van tooenable integrációs PowerShell használatával. Mivel minden üzembe helyezési modellel hello parancsai nem egyezik, akkor ez a cikk szakasza minden központi telepítési modell.  

Ez a cikk a folytatás előtt győződjön meg arról, hogy:

* hello Azure PowerShell SDK legújabb telepítve. A hello Webplatform-telepítővel telepíthető.
* Az Azure App Service egy Standard vagy Premium Termékváltozat futó alkalmazások.

## <a name="classic-virtual-networks"></a>Klasszikus virtuális hálózatok
Ez a szakasz ismerteti a három feladatok hello klasszikus üzembe helyezési modellt használó virtuális hálózatok:

1. Csatlakoztassa a app tooa elérésű, korábban létező virtuális hálózati átjáró és a pont – hely kapcsolat van konfigurálva.
2. Az alkalmazás a virtuális hálózati integráció adatok frissítése.
3. Az alkalmazás leválasztása a virtuális hálózat.

### <a name="connect-an-app-tooa-classic-vnet"></a>Csatlakozás egy alkalmazás tooa klasszikus virtuális hálózaton
egy alkalmazás tooa virtuális hálózathoz tooconnect kövesse az alábbi három lépéseket:

1. Deklarálja, hogy az csatlakozni fog egy adott virtuális hálózati toohello webalkalmazás. hello app olyan tanúsítvány, amely a virtuális hálózati toohello kap a pont – hely kapcsolatot hoz létre.
2. Töltsön fel hello web app tanúsítvány toohello virtuális hálózatot, és majd lekérheti a hello pont-pont VPN csomag URI.
3. Frissítse a hello web app virtuális hálózati kapcsolat hello pont-pont csomag URI.

hello első és harmadik lépéseket teljes parancsfájlos, de hello második lépés szükséges egy egyszeri, manuális művelet hello portál vagy a hozzáférés tooperform **PUT** vagy **javítás** műveletek hello virtuális hálózaton Azure Resource Manager-végpont. Lépjen kapcsolatba Azure támogatási toohave ez engedélyezve van. Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e a klasszikus virtuális hálózatot, amelyen már engedélyezve van a pont – hely kapcsolat és a telepített átjárót. toocreate hello átjáró és az engedélyezés pont – hely kapcsolatot, kell toouse hello portal részben ismertetett módon [VPN-átjáró létrehozása][createvpngateway].

hello klasszikus virtuális hálózatot kell toobe hello ugyanahhoz az előfizetéshez, az App Service tervet, amely integrációhoz tartás hello alkalmazást.

##### <a name="set-up-azure-powershell-sdk"></a>Azure PowerShell SDK beállítása
Nyisson meg egy PowerShell-ablakot, és állítsa be az Azure-fiókja és -előfizetést használatával:

    Login-AzureRmAccount

Ez a parancs nyílik Rákérdezés tooget Azure hitelesítő adatait. Miután bejelentkezik, használja a következő parancsok tooselect hello előfizetést, amelyet az toouse hello egyikét. Győződjön meg arról, hogy a rendszer a virtuális hálózat és az App Service-csomag hello-előfizetését használja.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

vagy

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>A cikk ezt használja változók
toosimplify parancsok helyezünk egy **$Configuration** PowerShell változó hello adott konfigurációval.

Egy változót az alábbiak szerint állíthatja a PowerShellben a következő paraméterek hello:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

hello app helyen hello hely nélkül szóközt kell lennie. USA nyugati régiója például westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

hello következő elem, ahol hello tanúsítványt kell írni. Azt a helyi számítógépen írható elérési útnak kell lennie. Győződjön meg arról, hogy tooinclude .cer hello végén.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

toosee mi állítja, típus **$Configuration**.

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

Ez a szakasz többi hello feltételezi, hogy rendelkezik-e csak leírtak változók.

##### <a name="declare-hello-virtual-network-toohello-app"></a>Hello virtuális hálózati toohello app deklarálható
A következő parancs tootell hello alkalmazást, hogy azt fogja használni az adott virtuális hálózati hello használata. Ennek hatására hello app toogenerate szükséges tanúsítványok:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Ha ez a parancs végrehajtása sikeres, **$vnet** rendelkeznie kell egy **tulajdonságok** változó azt. Hello **tulajdonságok** változó tartalmaznia kell mindkét a tanúsítvány ujjlenyomata és hello tanúsítványának adatait.

##### <a name="upload-hello-web-app-certificate-toohello-virtual-network"></a>Hello web app tanúsítvány toohello virtuális hálózati feltöltése
Kézi, egyszeri lépés akkor szükséges, az egyes előfizetés és a virtuális hálózati kombinációja. Ez azt jelenti, hogy ha az előfizetés A tooVirtual hálózati alkalmazások kapcsolódik, szüksége lesz toodo ezt a lépést csak egyszer, függetlenül attól, hogy hány alkalmazások konfigurálása. Ha ad hozzá egy új alkalmazás tooanother virtuális hálózatot, toodo többé lesz szüksége. hello ennek oka, hogy a tanúsítványok készletét jön létre az Azure App Service előfizetési szinten, és hello beállítása után minden virtuális hálózathoz csatlakozó hello alkalmazások jön létre.

hello tanúsítványokat fog rendelkezik már be van állítva, ha követte ezeket a lépéseket, vagy ha integrálva hello azonos virtuális hálózati hello portál használatával.

első lépés hello toogenerate hello .cer fájl. hello második lépésben tooupload hello .cer fájl tooyour virtuális hálózat. toogenerate hello .cer fájl hello API-hívás a hello futtassa a következő parancsok hello a korábbi lépésben.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

hello tanúsítvány hello helyen találhatók, amely **$Configuration.GeneratedCertificatePath** határozza meg.

tooupload hello tanúsítvány manuálisan, használja a hello [Azure-portálon] [ azureportal] és **Tallózás virtuális hálózat (klasszikus)** > **VPN-kapcsolatok**  >  **Pont-pont** > **tanúsítványok kezelése**. Itt a tanúsítvány feltöltése.

##### <a name="get-hello-point-to-site-package"></a>Hello pont-pont csomag
hello következő lépése a virtuális hálózati kapcsolat a webalkalmazás beállítása tooget hello pont-pont csomag, és adja meg a webalkalmazás tooyour.

A következő nevű GetNetworkPackageUri.json valahol a számítógépre, például C:\Azure\Templates\GetNetworkPackageUri.json tooa sablonfájlt hello mentéséhez.

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


A megadott bemeneti paraméterek:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Hívás hello parancsfájlt:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


hello változó **$output. Outputs.packageUri** hello csomag URI toobe tooyour webalkalmazás megadott mostantól tartalmazza.

##### <a name="upload-hello-point-to-site-package-tooyour-app"></a>Hello pont-pont csomag tooyour alkalmazás feltöltése
utolsó lépésként hello tooprovide hello app csomaggal. Egyszerűen futtassa a következő parancs hello:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Ha a rendszer megkérdezi, hogy meg vannak felülírja a meglévő erőforrás tooconfirm, győződjön meg arról, hogy tooallow azt.

Után ez a parancs sikeres, az alkalmazás most már csatlakoztatott toohello virtuális hálózati kell lennie. tooconfirm sikeres, nyissa meg tooyour app konzolon, és írja be a következő hello:

    SET WEBSITE_

Ha egy értéket, amely megfelel a hello cél virtuális hálózat nevét hello WEBSITE_VNETNAME nevű környezeti változó, az összes konfiguráció sikeres volt.

### <a name="update-classic-vnet-integration-information"></a>Klasszikus virtuális integráció frissítése
tooupdate vagy újraszinkronizálásra adatait, egyszerűen ismételje meg a hello hello integrációs hello első helyen létrehozásakor követett. Ezek a lépések a következők:

1. Adja meg a konfigurációs adatokat.
2. Hello virtuális hálózati toohello app deklarálható.
3. Hello pont-pont csomag beolvasása.
4. Hello pont-pont csomag tooyour alkalmazás feltöltéséhez.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Válassza le az alkalmazást egy klasszikus virtuális hálózaton
toodisconnect hello alkalmazást, és kell hello konfigurációs adatait, amely a virtuális hálózati integráció során lett beállítva. Az információk, nincs majd egy parancs toodisconnect az alkalmazás a virtuális hálózaton.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Erőforrás-kezelő virtuális hálózatok
Erőforrás-kezelő virtuális hálózat is létezik Azure Resource Manager API-k, amelyek egyszerűsítése bizonyos folyamatok klasszikus virtuális hálózatokat képest. Egy parancsfájl, amely segítséget hello a következő feladatok végrehajtásában vezetünk be:

* Hozzon létre egy erőforrás-kezelő virtuális hálózatot, és az alkalmazás integrálható.
* Hozzon létre egy pont – hely kapcsolat konfigurálása az erőforrás-kezelő már meglévő virtuális hálózat és az alkalmazás integrálható.
* Az alkalmazás integrálja egy már létező olyan erőforrás-kezelő virtuális hálózattal, amelyen egy átjáró és a pont – hely kapcsolat engedélyezett.
* Az alkalmazás leválasztása a virtuális hálózat.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Erőforrás-kezelő virtuális hálózat App Service integrációs parancsfájlját
Másolja az alábbi parancsfájlt, és mentse tooa fájl hello. Ha nem szeretné toouse hello parancsfájl, érzi, hogy a szabad toolearn toosee hogyan tooset folyamatot egy erőforrás-kezelő virtuális hálózattal.

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

Hello parancsfájl másolatának mentése. Ebben a cikkben V2VnetAllinOne.ps1 nevezik, de egy másik nevet. Nincsenek ehhez a parancsprogramhoz argumentumok. Egyszerűen futtassa azt. hello először thing hello parancsfájl hajt végre a rendszer felszólítja a toosign. Miután bejelentkezik, hello parancsfájl lekérdezi a fiók adatait, és előfizetések listáját adja vissza. A hitelesítő adatok hello kérelem nem számítva, hello kezdeti parancsfájl végrehajtása néz ki:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Bemutató előfizetés (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS-teszt (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Lila bemutató előfizetés (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Válasszon egy lehetőséget: 3

    Fiók: ccompy@microsoft.com környezet: AzureCloud előfizetés: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d bérlői: 722278f-fef1-499f-91ab-2323d011db47

    Adja meg az alkalmazás erőforráscsoport hello: hcdemo-rg adja meg az alkalmazás neve hello: v2vnetpowershell miről szeretne toodo?

    1) Egy új virtuális hálózat tooan alkalmazás hozzáadása
    2) Egy meglévő virtuális hálózat tooan alkalmazás hozzáadása
    3) Távolítsa el a virtuális hálózat az alkalmazásokból

Ez a szakasz többi hello ismerteti ezen három lehetőségek.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Erőforrás-kezelő VNet létrehozása, és azt integrálni
toocreate, hogy a használt hello Resource Manager üzembe helyezési modellben, és integrálhatja az alkalmazás a új virtuális hálózat kiválasztása **1) hozzáadása egy új virtuális hálózat tooan App**. Ez kérni fogja az hello hello virtuális hálózat nevét. Abban az esetben, ha ahogyan azt láthatja a beállításokat, a következő hello használt hello nevét, v2pshell.

hello parancsfájl tájékoztatást nyújt a hello hello virtuális hálózat létrehozása folyamatban van. Szeretném, ha bármelyik hello értékek módosíthatók. A példa végrehajtása a következő beállítások hello rendelkező virtuális hálózatban létrehozott:

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

Ha azt szeretné, toochange hello értékek bármelyikét, írja be a **Y** és hello módosíthatja. Ha elégedett hello virtuális hálózati beállításait, írja be a **N** vagy egyszerűen csak nyomja le az Enter hello beállításainak módosításával kapcsolatos megjelenésekor. Ezekből a befejezéséig hello parancsfájl megtudhatja, amit a rendszergazda némelyike "i's végre, amíg nem toocreate hello virtuális hálózati átjáró kezdődik. A lépés akár tooan órát is igénybe vehet. Ebben a fázisban nem folyamatjelző van, de hello parancsfájl lehetővé teszi, hogy ismeri a hello átjáró létrehozásakor.

Hello parancsfájl befejezése után, akkor megtudhatja, hogy **befejezett**. Ezen a ponton fog hello neve erőforrás-kezelő virtuális hálózat és a kiválasztott beállításokat. Az új virtuális hálózat is integrálják az alkalmazást.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Az alkalmazás integrálja a már meglévő Resource Manager virtuális hálózaton
Ha egy már meglévő virtuális hálózatot, ha megadja a pont – hely kapcsolat vagy átjáróként működő nem rendelkező erőforrás-kezelő virtuális hálózat most integrálása, hello parancsprogram beállítja, hogy. Hello virtuális hálózat már rendelkezik állítsa be ezeket a beállításokat, ha hello parancsfájlt halad egyenes toohello alkalmazásintegráció. toostart ezt a folyamatot, egyszerűen válassza **2) hozzáadása egy meglévő virtuális hálózat tooan App**.

Ez a beállítás csak akkor, ha már létező erőforrás-kezelő virtuális hálózattal rendelkezik, amely hello működik az alkalmazás ugyanahhoz az előfizetéshez. Miután hello lehetőséget választja, választhat a Resource Manager virtuális hálózatok listájával.   

    Select a VNET toointegrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Válasszon egy lehetőséget: 5

Hello virtuális hálózathoz, amelyet a toointegrate egyszerűen válassza. Ha már van egy átjáró, amely a pont – hely kapcsolat engedélyezve van, hello parancsfájl egyszerűen integrálható az alkalmazás a virtuális hálózat. Ha nem rendelkezik egy átjárót, szüksége lesz a toospecify hello átjáró-alhálózatot. Az átjáró-alhálózatot a virtuális hálózat címtere kell lennie, és nem lehet a más alhálózathoz. Ha egy virtuális hálózati átjáró nélkül, és futtassa ezt a lépést, akkor a dolgok néznek ki:

    This Virtual Network has no gateway. I will need toocreate one.
    Your VNET is in hello address space 172.16.0.0/16, with hello following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

Ebben a példában a virtuális hálózati átjáró, amely rendelkezik a következő beállítások hello létrehozott:

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

Ha azt szeretné, toochange ezeket a beállításokat, megteheti. Vagy nyomja le az ENTER billentyűt, és hello parancsfájl létrehoz az átjáró és az alkalmazás tooyour virtuális hálózat csatolása. hello átjáró létrehozásának ideje még nem állt le egy óra alatt, ha, ezért győződjön meg arról, hogy figyelembe venni. Ha minden befejeződött, hello parancsfájl megtudhatja, hogy **befejezett**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Válassza le az alkalmazást egy erőforrás-kezelő virtuális hálózat
Az alkalmazás leválasztása a virtuális hálózat nem hello átjáró le és tiltsa le a pont – hely kapcsolat. Előfordulhat, ha minden használni azt máshová. Azt is leválasztása nem azt minden más alkalmazás nem hello egy megadott. tooperform Ez a művelet, jelölje be **3) virtuális hálózat eltávolítása egy alkalmazás**. Ha így tesz, látni fogja például ehhez hasonló:

    Currently connected tooVNET v2pshell

    Confirm
    Are you sure you want toodelete hello following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Bár hello parancsfájl feliratú törlés, nem törli hello virtuális hálózat. Csak az éppen eltávolítja hello integráció. Miután meggyőződött róla, hogy ez a választható toodo, hello parancs meglehetősen gyorsan dolgoz fel, és jelzi, hogy **igaz** amikor elkészült.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
