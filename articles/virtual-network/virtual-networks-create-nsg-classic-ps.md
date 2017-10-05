---
title: "Hálózati biztonsági csoportokat (klasszikus) Azure - PowerShell létre |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és telepíthet az NSG-k klasszikus módban PowerShell használatával"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 86810b0d-0240-46a2-8548-fca22daa56f3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: e3f84e4757e3854fc63e3069e179446174f0c0bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-nsgs-classic-in-powershell"></a><span data-ttu-id="2f96b-103">Az NSG-k (klasszikus) létrehozása a PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f96b-103">How to create NSGs (classic) in PowerShell</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="2f96b-104">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2f96b-104">This article covers the classic deployment model.</span></span> <span data-ttu-id="2f96b-105">Emellett [NSG-k létrehozása a Resource Manager üzembe helyezési modellel](virtual-networks-create-nsg-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2f96b-105">You can also [create NSGs in the Resource Manager deployment model](virtual-networks-create-nsg-arm-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="2f96b-106">A minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell a fenti forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="2f96b-106">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="2f96b-107">Ha szeretné a parancsokat a jelen dokumentum megjelenített, először által a tesztkörnyezet felépítéséhez [létre virtuális hálózatok](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2f96b-107">If you want to run the commands as they are displayed in this document, first build the test environment by [creating a VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="2f96b-108">Az NSG az előtér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f96b-108">How to create the NSG for the front-end subnet</span></span>
<span data-ttu-id="2f96b-109">Hozzon létre egy NSG nevű nevű **NSG-előtérbeli** a fenti forgatókönyv alapján, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2f96b-109">To create an NSG named named **NSG-FrontEnd** based on the scenario above, follow the steps below:</span></span>

1. <span data-ttu-id="2f96b-110">Ha még nem használta az Azure PowerShellt, tekintse meg [How to Install and Configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) című részt, majd kövesse az utasításokat egészen az utolsó lépésig az Azure-ba való bejelentkezéshez és az előfizetése kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="2f96b-110">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="2f96b-111">Hozzon létre egy hálózati biztonsági csoport nevű **NSG-előtérbeli**.</span><span class="sxs-lookup"><span data-stu-id="2f96b-111">Create a network security group named **NSG-FrontEnd**.</span></span>
   
        New-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" -Location uswest `
            -Label "Front end subnet NSG"
   
    <span data-ttu-id="2f96b-112">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2f96b-112">Expected output:</span></span>

        Name         Location   Label               
        
        NSG-FrontEnd West US     Front end subnet NSG

3. <span data-ttu-id="2f96b-113">Hozzon létre egy biztonsági szabályt, amely engedélyezi webtartalmak elérését az internetről 3389-es port.</span><span class="sxs-lookup"><span data-stu-id="2f96b-113">Create a security rule allowing access from the Internet to port 3389.</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
        | Set-AzureNetworkSecurityRule -Name rdp-rule `
            -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
            -SourceAddressPrefix Internet  -SourcePortRange '*' `
            -DestinationAddressPrefix '*' -DestinationPortRange '3389'
   
    <span data-ttu-id="2f96b-114">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2f96b-114">Expected output:</span></span>
   
        Name     : NSG-FrontEnd
        Location : Central US
        Label    : Front end subnet NSG
        Rules    :
   
                      Type: Inbound
   
                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   rdp-rule             100       Allow    INTERNET        *             *                3389           TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       

                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *

1. <span data-ttu-id="2f96b-115">Hozzáférés engedélyezése az internetről a 80-as port biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2f96b-115">Create a security rule allowing access from the Internet to port 80.</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
        | Set-AzureNetworkSecurityRule -Name web-rule `
            -Action Allow -Protocol TCP -Type Inbound -Priority 200 `
            -SourceAddressPrefix Internet  -SourcePortRange '*' `
            -DestinationAddressPrefix '*' -DestinationPortRange '80'
   
    <span data-ttu-id="2f96b-116">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2f96b-116">Expected output:</span></span>

        Name     : NSG-FrontEnd
        Location : Central US
        Label    : Front end subnet NSG
        Rules    :

                      Type: Inbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   rdp-rule             100       Allow    INTERNET        *             *                3389           TCP     
                   web-rule             200       Allow    INTERNET        *             *                80             TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       


                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *   

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="2f96b-117">Az NSG a háttérbeli alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f96b-117">How to create the NSG for the back end subnet</span></span>
1. <span data-ttu-id="2f96b-118">Hozzon létre egy hálózati biztonsági csoport nevű **NSG-háttérrendszer**.</span><span class="sxs-lookup"><span data-stu-id="2f96b-118">Create a network security group named **NSG-BackEnd**.</span></span>
   
        New-AzureNetworkSecurityGroup -Name "NSG-BackEnd" -Location uswest `
            -Label "Back end subnet NSG"
   
    <span data-ttu-id="2f96b-119">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2f96b-119">Expected output:</span></span>
   
        Name        Location   Label              
        
        NSG-BackEnd West US    Back end subnet NSG
2. <span data-ttu-id="2f96b-120">A 1433-as port (alapértelmezett SQL Server által használt) az előtér-alhálózatból hozzáférést a biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2f96b-120">Create a security rule allowing access from the front end subnet to port 1433 (default port used by SQL Server).</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
        | Set-AzureNetworkSecurityRule -Name rdp-rule `
            -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
            -SourceAddressPrefix 192.168.1.0/24  -SourcePortRange '*' `
            -DestinationAddressPrefix '*' -DestinationPortRange '1433'
   
    <span data-ttu-id="2f96b-121">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2f96b-121">Expected output:</span></span>
   
        Name     : NSG-BackEnd
        Location : Central US
        Label    : Back end subnet NSG
        Rules    :
   
                      Type: Inbound
   
                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   fe-rule              100       Allow    192.168.1.0/24  *             *                1433           TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       

                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *      

1. <span data-ttu-id="2f96b-122">Hozzon létre egy biztonsági szabály blokkolja a hozzáférést az alhálózatból az internethez.</span><span class="sxs-lookup"><span data-stu-id="2f96b-122">Create a security rule blocking access from the subnet to the Internet.</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-BackEnd" `
        | Set-AzureNetworkSecurityRule -Name block-internet `
            -Action Deny -Protocol '*' -Type Outbound -Priority 200 `
            -SourceAddressPrefix '*'  -SourcePortRange '*' `
            -DestinationAddressPrefix Internet -DestinationPortRange '*'
   
    <span data-ttu-id="2f96b-123">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2f96b-123">Expected output:</span></span>
   
        Name     : NSG-BackEnd
        Location : Central US
        Label    : Back end subnet NSG
        Rules    :
   
                      Type: Inbound
   
                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   fe-rule              100       Allow    192.168.1.0/24  *             *                1433           TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       

                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   block-internet       200       Deny     *               *             INTERNET         *              *       
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *   
