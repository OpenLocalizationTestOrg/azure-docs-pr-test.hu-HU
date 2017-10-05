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
# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Csatlakozás egy alkalmazással a virtuális hálózatra a PowerShell használatával
## <a name="overview"></a>Áttekintés
Az Azure App Service-ben kapcsolódhat az alkalmazás (webes, mobil, vagy API) egy Azure virtuális hálózatot (VNet) az előfizetésben. A szolgáltatás virtuális integráció neve. A virtuális hálózat integrációs szolgáltatás számára nem kell összetéveszthető az App Service Environment-környezet funkció, amely lehetővé teszi a virtuális hálózat egy példányát az Azure App Service futtatását.

A virtuális hálózat integrációs szolgáltatás számára az új portálon, hogy a virtuális hálózatok, a klasszikus üzembe helyezési modellt vagy az Azure Resource Manager telepítési modell segítségével telepített integrálása használhatja a felhasználói felület (UI) rendelkezik. Ha azt szeretné, további információt a szolgáltatást, lásd: [az alkalmazás integrálása az Azure virtuális hálózat](web-sites-integrate-with-vnet.md).

Ez a cikk van, nem a felhasználói felület használatával kapcsolatban, de ahelyett, hogy engedélyezésével kapcsolatos integrációs PowerShell használatával. Mivel minden üzembe helyezési modellben a parancsokat, akkor ez a cikk szakasza minden központi telepítési modell.  

Ez a cikk a folytatás előtt győződjön meg arról, hogy:

* A legújabb Azure PowerShell SDK telepítve. Az a Webplatform-telepítővel telepíthető.
* Az Azure App Service egy Standard vagy Premium Termékváltozat futó alkalmazások.

## <a name="classic-virtual-networks"></a>Klasszikus virtuális hálózatok
Ez a szakasz ismerteti a klasszikus üzembe helyezési modellt használó virtuális hálózatok három feladatok:

1. Az alkalmazás kapcsolódni tudjon egy már meglévő virtuális hálózati átjáró és a pont – hely kapcsolat van konfigurálva.
2. Az alkalmazás a virtuális hálózati integráció adatok frissítése.
3. Az alkalmazás leválasztása a virtuális hálózat.

### <a name="connect-an-app-to-a-classic-vnet"></a>Az alkalmazás kapcsolódni tudjon egy klasszikus virtuális hálózaton
Egy alkalmazás egy virtuális hálózathoz való kapcsolódáshoz, kövesse a fenti három lépést:

1. Deklarálja azt a webalkalmazást, hogy az csatlakozni fog egy adott virtuális hálózaton. Az alkalmazás egy pont – hely kapcsolat a virtuális hálózathoz megadott tanúsítványt hoz létre.
2. A webes alkalmazás-tanúsítvány feltöltése a virtuális hálózathoz, és majd lekérheti a pont-pont VPN csomag URI.
3. A web app virtuális hálózati kapcsolat frissítése a pont-pont csomag URI.

Az első és harmadik lépéseket teljes parancsfájlos, de a második lépés szükséges a portálon, vagy hajtsa végre a hozzáférést egy egyszeri, manuális művelet **PUT** vagy **javítás** műveletek a virtuális hálózati Azure Resource Manager-végpont. Kérje az Azure támogatási ez engedélyezve van. Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e a klasszikus virtuális hálózatot, amelyen már engedélyezve van a pont – hely kapcsolat és a telepített átjárót. Az átjáró létrehozásához, és engedélyezze a pont – hely kapcsolatot, akkor portált kell használniuk a részben ismertetett módon [VPN-átjáró létrehozása][createvpngateway].

A klasszikus virtuális hálózaton kell lennie az App Service-csomag, amely tárolja az alkalmazást, amely integrációhoz tárolóként ugyanazt az előfizetést.

##### <a name="set-up-azure-powershell-sdk"></a>Azure PowerShell SDK beállítása
Nyisson meg egy PowerShell-ablakot, és állítsa be az Azure-fiókja és -előfizetést használatával:

    Login-AzureRmAccount

Ez a parancs jelenít meg az Azure hitelesítő adatainak lekérése nyílik meg. Miután bejelentkezik, az alábbi parancsok egyikét segítségével válassza ki a használni kívánt előfizetést. Győződjön meg arról, hogy az előfizetés, amelyek a virtuális hálózat és az App Service-csomagot használ-e.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

vagy

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>A cikk ezt használja változók
Egyszerűbbé teheti a parancsok, helyezünk egy **$Configuration** PowerShell változó az adott konfigurációval.

Egy változót az alábbiak szerint állíthatja a PowerShellben a következő paraméterekkel:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Az alkalmazás bármely szóközök nélkül a helyen kell lennie. USA nyugati régiója például westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

A következő elem, ahol lehet írni a tanúsítványt. Azt a helyi számítógépen írható elérési útnak kell lennie. Ügyeljen arra, hogy tartalmazzák a .cer végén.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Akkor állítsa be parancsot kell beírnia **$Configuration**.

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

Ez a szakasz a többi feltételezi, hogy rendelkezik-e csak leírtak változók.

##### <a name="declare-the-virtual-network-to-the-app"></a>Deklarálja az alkalmazás a virtuális hálózat
A következő paranccsal kérje meg az alkalmazást, hogy azt fogja használni az adott virtuális hálózati. Ez azt eredményezi, az alkalmazásnak, hogy a szükséges tanúsítványok létrehozása:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Ha ez a parancs végrehajtása sikeres, **$vnet** rendelkeznie kell egy **tulajdonságok** változó azt. A **tulajdonságok** változót kell tartalmaznia, a tanúsítvány-ujjlenyomat, mind a tanúsítványának adatait.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>A webes alkalmazás-tanúsítvány feltöltése a virtuális hálózathoz
Kézi, egyszeri lépés akkor szükséges, az egyes előfizetés és a virtuális hálózati kombinációja. Ez azt jelenti, hogy ha a virtuális hálózati alkalmazások az előfizetést A kapcsolódik, akkor csak egyszer, függetlenül attól, hogy hány alkalmazások konfigurálása ehhez a lépéshez. Ha egy másik virtuális hálózathoz ad hozzá egy új alkalmazást, szüksége újra. Ennek oka, hogy a tanúsítványok készletét jön létre az Azure App Service előfizetési szinten, és a készletben jön létre egyszer minden virtuális hálózathoz, amelyek csatlakozni fognak az alkalmazások.

A tanúsítványok lesznek rendelkezik már be van állítva Ha követte ezeket a lépéseket, vagy ha a portál használatával integrálva van az azonos virtuális hálózatban.

Az első lépés a .cer-fájl létrehozásához. A második lépésben fel kell töltenie a .cer fájlt a virtuális hálózathoz. Az API-hívás az előző lépésben a .cer-fájl létrehozásához futtassa az alábbi parancsokat.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

A tanúsítvány a helyen találhatók, amely **$Configuration.GeneratedCertificatePath** határozza meg.

A tanúsítvány manuális feltöltése, használja a [Azure-portálon] [ azureportal] és **Tallózás virtuális hálózat (klasszikus)** > **VPN-kapcsolatok** > **pont-pont** > **tanúsítványok kezelése**. Itt a tanúsítvány feltöltése.

##### <a name="get-the-point-to-site-package"></a>A pont-pont csomag
A következő lépése a virtuális hálózati kapcsolat a webalkalmazás beállítása, hogy a pont-pont csomagjának és adja meg a webes alkalmazást.

A következő sablon mentése nevű GetNetworkPackageUri.json valahol a számítógépre, például C:\Azure\Templates\GetNetworkPackageUri.json fájlba.

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

A parancsprogram hívása:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


A változó **$output. Outputs.packageUri** mostantól tartalmazza a csomag URI-t kell megadni a webalkalmazáshoz.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>A pont-pont csomag feltölteni az alkalmazást
Az utolsó lépés, hogy adja meg az alkalmazás ezzel a csomaggal. Egyszerűen futtassa a következő parancsot:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Ha a rendszer megkérdezi, hogy ellenőrizze, hogy valamelyik meglévő erőforrására másolattal, győződjön meg arról, hogy engedélyezi-e.

Után ez a parancs sikeres, az alkalmazás most a virtuális hálózathoz kell csatlakoztatni. Erősítse meg a sikeres, az alkalmazás-konzolon, és írja be a következőt:

    SET WEBSITE_

Ha egy környezeti változó neve, amely rendelkezik egy érték, amely megegyezik-e a cél virtuális hálózat WEBSITE_VNETNAME, az összes konfiguráció sikeres volt.

### <a name="update-classic-vnet-integration-information"></a>Klasszikus virtuális integráció frissítése
Frissítéséhez, vagy el az újraszinkronizálást, az adatait, egyszerűen ismételje meg a integrálását az elsőként létrehozott követett. Ezek a lépések a következők:

1. Adja meg a konfigurációs adatokat.
2. Deklarálja az alkalmazás a virtuális hálózat.
3. A pont-pont csomag beolvasása.
4. A pont-pont csomag feltölteni az alkalmazást.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Válassza le az alkalmazást egy klasszikus virtuális hálózaton
Válassza le az alkalmazást, a virtuális hálózati integráció során megadott konfigurációs információkat kell. Az információk, nincs majd az alkalmazás bontja a virtuális hálózat egy parancs.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Erőforrás-kezelő virtuális hálózatok
Erőforrás-kezelő virtuális hálózat is létezik Azure Resource Manager API-k, amelyek egyszerűsítése bizonyos folyamatok klasszikus virtuális hálózatokat képest. Van olyan parancsfájlt, amely segít a hajtsa végre a következő feladatokat:

* Hozzon létre egy erőforrás-kezelő virtuális hálózatot, és az alkalmazás integrálható.
* Hozzon létre egy pont – hely kapcsolat konfigurálása az erőforrás-kezelő már meglévő virtuális hálózat és az alkalmazás integrálható.
* Az alkalmazás integrálja egy már létező olyan erőforrás-kezelő virtuális hálózattal, amelyen egy átjáró és a pont – hely kapcsolat engedélyezett.
* Az alkalmazás leválasztása a virtuális hálózat.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Erőforrás-kezelő virtuális hálózat App Service integrációs parancsfájlját
Másolja a következő parancsfájlt, és mentse a fájlt. Ha nem szeretné a parancsfájlt, nyugodtan megtudjuk módjával dolgot beállítása egy erőforrás-kezelő virtuális hálózattal.

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

A parancsfájl másolatának mentése. Ebben a cikkben V2VnetAllinOne.ps1 nevezik, de egy másik nevet. Nincsenek ehhez a parancsprogramhoz argumentumok. Egyszerűen futtassa azt. A parancsfájl fog tenni elsőként felszólítja, hogy jelentkezzen be. Miután bejelentkezik, a parancsfájl lekérdezi a fiók adatait, és előfizetések listáját adja vissza. A kérelem a hitelesítő adatok nem számítva, a kezdeti parancsfájl végrehajtása néz ki:

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

    Adja meg az alkalmazás az erőforráscsoport: hcdemo-rg adja meg az alkalmazás nevét: v2vnetpowershell mi történjen a teendő?

    1) ÚJ virtuális hálózat hozzáadása egy alkalmazáshoz
    2) Egy meglévő virtuális hálózat hozzáadása egy alkalmazáshoz
    3) Távolítsa el a virtuális hálózat az alkalmazásokból

Ez a szakasz a többi ismerteti ezen három lehetőségek.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Erőforrás-kezelő VNet létrehozása, és azt integrálni
A Resource Manager üzembe helyezési modellel használó új virtuális hálózat létrehozásához, és integrálhatja a az alkalmazás, válassza ki a **1) egy új virtuális hálózat hozzáadása egy alkalmazáshoz**. Ez kérni fogja a virtuális hálózat nevét. Abban az esetben, ha ahogy látja, a következő beállításokat, a használt a név, v2pshell.

A parancsfájl tájékoztatást nyújt a a kiválasztott virtuális hálózat létrehozása folyamatban van. Ha szeretné, módosíthatók valamely értékét. A példa végrehajtása a létrehozott egy virtuális hálózatot, a következő beállításokkal:

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

Ha valamely értékét módosítani kívánja, írja be a **Y** , és hajtsa végre a módosításokat. Ha elégedett a virtuális hálózati beállításait, írja be a **N** vagy egyszerűen csak nyomja le az ENTER billentyűt, amikor a rendszer kéri, a beállítások módosításával. Innen a befejezéséig, a parancsfájl megtudhatja, amit a rendszergazda némelyike "i's egészen a virtuális hálózati átjáró létrehozása során. A lépés egy óráig is eltarthat. Ebben a fázisban nem folyamatjelző van, de a parancsfájl lehetővé teszi, hogy tudja, hogy az átjáró létrehozásakor.

A parancsfájl befejezése után, akkor megtudhatja, hogy **befejezett**. Ezen a ponton, amelyen a nevét, és a megadott beállításokat, erőforrás-kezelő virtuális hálózat fog. Az új virtuális hálózat is integrálják az alkalmazást.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Az alkalmazás integrálja a már meglévő Resource Manager virtuális hálózaton
Ha egy már meglévő virtuális hálózatot, ha megadja a pont – hely kapcsolat vagy átjáróként működő nem rendelkező erőforrás-kezelő virtuális hálózat most integrálása, a parancsfájl beállításához nyújt útmutatást, amelyek. Ha a virtuális hálózat már rendelkezik állítsa be ezeket a beállításokat, a parancsfájl ugrik rögtön az alkalmazásintegráció. Ez a folyamat indításához egyszerűen válassza **2) egy meglévő virtuális hálózat hozzáadása egy alkalmazáshoz**.

Ez a beállítás csak akkor, ha már létező erőforrás-kezelő virtuális hálózattal rendelkezik, amely ugyanazt az előfizetést, az alkalmazás működik. A beállítást, akkor megjelenik a Resource Manager virtuális hálózatokból álló listát.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Válasszon egy lehetőséget: 5

Egyszerűen jelölje ki a virtuális hálózat, amelyet integrálni szeretne. Ha már van egy átjáró, amely a pont – hely kapcsolat engedélyezve van, a parancsfájl egyszerűen integrálható az alkalmazás a virtuális hálózat. Ha nem rendelkezik egy átjárót, akkor adja meg az átjáró-alhálózatot. Az átjáró-alhálózatot a virtuális hálózat címtere kell lennie, és nem lehet a más alhálózathoz. Ha egy virtuális hálózati átjáró nélkül, és futtassa ezt a lépést, akkor a dolgok néznek ki:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

Ebben a példában létrehozott egy virtuális hálózati átjáró a következő beállításokkal:

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

Ha szeretné módosítani ezeket a beállításokat, megteheti. Vagy nyomja le az ENTER billentyűt, és a parancsfájl létrehoz az átjáró és az alkalmazás csatlakoztatása a virtuális hálózat. Az átjáró létrehozása idő továbbra is egy óra alatt, ha, ezért győződjön meg arról, hogy figyelembe venni. Ha minden befejeződött, a parancsfájl megtudhatja, hogy **befejezett**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Válassza le az alkalmazást egy erőforrás-kezelő virtuális hálózat
Az alkalmazás leválasztása a virtuális hálózat nem az átjáró le és tiltsa le a pont – hely kapcsolat. Előfordulhat, ha minden használni azt máshová. Azt is nem válassza le azt minden más alkalmazás nem a megadott. Ez a művelet elvégzéséhez válasszon **3) virtuális hálózat eltávolítása egy alkalmazás**. Ha így tesz, látni fogja például ehhez hasonló:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

A parancsfájl törlése szerint, de nem törli a virtuális hálózat. Csak az éppen eltávolítja az integráció. Miután meggyőződött róla, hogy ez mit kíván tenni a, a parancs meglehetősen gyorsan dolgoz fel, és jelzi, hogy **igaz** amikor elkészült.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
