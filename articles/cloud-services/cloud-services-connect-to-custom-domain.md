---
title: "egy felhőalapú szolgáltatás tooa aaaConnect egyéni tartományvezérlő |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect webes vagy feldolgozói szerepkörök tooa egyéni AD-tartomány PowerShell és az AD-tartomány bővítmény használatával"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 9540190ccf17c03e55159c6c68429eee29e0a558
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-azure-cloud-services-roles-tooa-custom-ad-domain-controller-hosted-in-azure"></a><span data-ttu-id="02006-103">Csatlakozás Azure Cloud Services szerepkörök tooa egyéni Azure-ban üzemeltetett AD-tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="02006-103">Connecting Azure Cloud Services Roles tooa custom AD Domain Controller hosted in Azure</span></span>
<span data-ttu-id="02006-104">Első üzembe helyezünk egy virtuális hálózatot (VNet) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="02006-104">We will first set up a Virtual Network (VNet) in Azure.</span></span> <span data-ttu-id="02006-105">Az Active Directory-tartományvezérlőhöz (az Azure virtuális gép üzemeltetett) toohello VNet majd adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="02006-105">We will then add an Active Directory Domain Controller (hosted on an Azure Virtual Machine) toohello VNet.</span></span> <span data-ttu-id="02006-106">A következő azt szolgáltatás hozzáadása a meglévő felhőalapú szolgáltatás szerepkörök toohello előre létrehozott virtuális hálózatot, majd csatlakoztassa őket toohello tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="02006-106">Next, we will add existing cloud service roles toohello pre-created VNet, then connect them toohello Domain Controller.</span></span>

<span data-ttu-id="02006-107">Mielőtt azt először néhány dolgot tookeep szem előtt a:</span><span class="sxs-lookup"><span data-stu-id="02006-107">Before we get started, couple of things tookeep in mind:</span></span>

1. <span data-ttu-id="02006-108">Ez az oktatóanyag PowerShell használja, ezért győződjön meg arról, hogy Azure PowerShell telepítése és toogo kész.</span><span class="sxs-lookup"><span data-stu-id="02006-108">This tutorial uses PowerShell, so make sure you have Azure PowerShell installed and ready toogo.</span></span> <span data-ttu-id="02006-109">tooget segítséget beállítása az Azure PowerShell, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="02006-109">tooget help with setting up Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="02006-110">Az AD-tartományvezérlő és a webes vagy feldolgozói szerepkör-példányok a hello VNet toobe kell.</span><span class="sxs-lookup"><span data-stu-id="02006-110">Your AD Domain Controller and Web/Worker Role instances need toobe in hello VNet.</span></span>

<span data-ttu-id="02006-111">Kövesse a cikk részletesen ismerteti, és ha probléma merül fel, hagyja meg velünk hello cikk végén hello megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="02006-111">Follow this step-by-step guide and if you run into any issues, leave us a comment at hello end of hello article.</span></span> <span data-ttu-id="02006-112">Valaki kap vissza tooyou (Igen, találtunk megjegyzések).</span><span class="sxs-lookup"><span data-stu-id="02006-112">Someone will get back tooyou (yes, we do read comments).</span></span>

<span data-ttu-id="02006-113">hello hálózati hello felhőalapú szolgáltatás által hivatkozott kell lennie egy **klasszikus virtuális hálózatot**.</span><span class="sxs-lookup"><span data-stu-id="02006-113">hello network that is referenced by hello cloud service must be a **classic virtual network**.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="02006-114">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="02006-114">Create a Virtual Network</span></span>
<span data-ttu-id="02006-115">Azure hello Azure-portálon vagy a PowerShell használatával is létrehozhat egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="02006-115">You can create a Virtual Network in Azure using hello Azure portal or PowerShell.</span></span> <span data-ttu-id="02006-116">Ebben az oktatóanyagban a PowerShell használjuk.</span><span class="sxs-lookup"><span data-stu-id="02006-116">For this tutorial, we will use PowerShell.</span></span> <span data-ttu-id="02006-117">a virtuális hálózat használatával toocreate hello Azure portál, lásd: [virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="02006-117">toocreate a Virtual Network using hello Azure portal, see [Create Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="02006-118">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="02006-118">Create a Virtual Machine</span></span>
<span data-ttu-id="02006-119">Hello virtuális hálózat beállításának befejezése után kell toocreate az AD tartományvezérlőt.</span><span class="sxs-lookup"><span data-stu-id="02006-119">Once you have completed setting up hello Virtual Network, you will need toocreate an AD Domain Controller.</span></span> <span data-ttu-id="02006-120">Ebben az oktatóanyagban azt szeretné állítani egy AD-tartományvezérlő egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="02006-120">For this tutorial, we will be setting up an AD Domain Controller on an Azure Virtual Machine.</span></span>

<span data-ttu-id="02006-121">toodo, hozzon létre egy virtuális gépet a PowerShell segítségével a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="02006-121">toodo this, create a virtual machine through PowerShell using hello following commands:</span></span>

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it toohello Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-tooa-domain-controller"></a><span data-ttu-id="02006-122">A virtuális gép tooa tartományvezérlő előléptetése</span><span class="sxs-lookup"><span data-stu-id="02006-122">Promote your Virtual Machine tooa Domain Controller</span></span>
<span data-ttu-id="02006-123">Virtuális gép tooconfigure hello AD-tartományvezérlő, mert először a virtuális gép toohello toolog kell, konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="02006-123">tooconfigure hello Virtual Machine as an AD Domain Controller, you will need toolog in toohello VM and configure it.</span></span>

<span data-ttu-id="02006-124">a virtuális gép toohello toolog, kaphat a hello RDP-fájlt a PowerShell segítségével, a következő parancsok használata hello:</span><span class="sxs-lookup"><span data-stu-id="02006-124">toolog in toohello VM, you can get hello RDP file through PowerShell, use hello following commands:</span></span>

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

<span data-ttu-id="02006-125">Ha be van jelentkezve toohello VM, be a virtuális gép AD-tartományvezérlő által következő hello részletes útmutató a [hogyan tooset az ügyfélnek AD-tartományvezérlő](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="02006-125">Once you are signed in toohello VM, set up your Virtual Machine as an AD Domain Controller by following hello step-by-step guide on [How tooset up your customer AD Domain Controller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span></span>

## <a name="add-your-cloud-service-toohello-virtual-network"></a><span data-ttu-id="02006-126">A felhőalapú szolgáltatás toohello virtuális hálózat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="02006-126">Add your Cloud Service toohello Virtual Network</span></span>
<span data-ttu-id="02006-127">A következő lépésben tooadd a felhőalapú szolgáltatás telepítési toohello új virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="02006-127">Next, you need tooadd your cloud service deployment toohello new VNet.</span></span> <span data-ttu-id="02006-128">toodo, módosítsa a felhőalapú szolgáltatás szolgáltatáskonfigurációs séma hello vonatkozó szakaszaihoz tooyour szolgáltatáskonfigurációs séma Visual Studio használatával hozzáadásával, vagy az Ön által választott szerkesztőben hello.</span><span class="sxs-lookup"><span data-stu-id="02006-128">toodo this, modify your cloud service cscfg by adding hello relevant sections tooyour cscfg using Visual Studio or hello editor of your choice.</span></span>

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

<span data-ttu-id="02006-129">Ezután a felhőalapú szolgáltatások projekt buildjének elkészítéséhez, és tooAzure telepítheti.</span><span class="sxs-lookup"><span data-stu-id="02006-129">Next build your cloud services project and deploy it tooAzure.</span></span> <span data-ttu-id="02006-130">a cloud services csomag tooAzure üzembe helyezésével járó tooget súgó lásd: [hogyan tooCreate és egy felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span><span class="sxs-lookup"><span data-stu-id="02006-130">tooget help with deploying your cloud services package tooAzure, see [How tooCreate and Deploy a Cloud Service](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span></span>

## <a name="connect-your-webworker-roles-toohello-domain"></a><span data-ttu-id="02006-131">A webes vagy feldolgozói szerepkörök toohello tartományhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="02006-131">Connect your web/worker roles toohello domain</span></span>
<span data-ttu-id="02006-132">Miután a felhőszolgáltatás-projekt Azure van telepítve, csatlakoztassa a szerepkör példányok toohello egyéni AD tartományi hello AD-tartomány bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="02006-132">Once your cloud service project is deployed on Azure, connect your role instances toohello custom AD domain using hello AD Domain Extension.</span></span> <span data-ttu-id="02006-133">tooadd hello AD-tartomány bővítmény tooyour meglévő felhőalapú szolgáltatások telepítése és csatlakozás hello egyéni tartományhoz, hajtsa végre a következő PowerShell-parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="02006-133">tooadd hello AD Domain Extension tooyour existing cloud services deployment and join hello custom domain, execute hello following commands in PowerShell:</span></span>

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension toohello cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

<span data-ttu-id="02006-134">És azt.</span><span class="sxs-lookup"><span data-stu-id="02006-134">And that's it.</span></span>

<span data-ttu-id="02006-135">A felhőalapú szolgáltatások csatlakoztatott tooyour egyéni tartományvezérlő kell lennie.</span><span class="sxs-lookup"><span data-stu-id="02006-135">Your cloud services should be joined tooyour custom domain controller.</span></span> <span data-ttu-id="02006-136">Ha azt szeretné, hogy további információk a különböző lehetőségekről hello toolearn hogyan tooconfigure AD-tartomány-bővítmény használatát hello PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="02006-136">If you would like toolearn more about hello different options available for how tooconfigure AD Domain Extension, use hello PowerShell help.</span></span> <span data-ttu-id="02006-137">Néhány példa kövesse:</span><span class="sxs-lookup"><span data-stu-id="02006-137">A couple of examples follow:</span></span>

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
