---
title: "Erőforrás-szolgáltató áttekintés aaaNetwork |} Microsoft Docs"
description: "További tudnivalók: hello új hálózati erőforrás-szolgáltató az Azure Resource Manager"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 79bf09da-4809-45cb-8d21-705616ef24dc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 81b8f51fe8ee180d8f7885c6e04eb953904d7be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-resource-provider"></a><span data-ttu-id="9f5f9-103">Hálózatierőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="9f5f9-103">Network Resource Provider</span></span>
<span data-ttu-id="9f5f9-104">Egy megerősítő kell az aktuális üzleti sikeres, a lehetősége toobuild hello és nagy méretű hálózati alkalmazások kezelése a gyors, rugalmas, biztonságos és ismételhető módon.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-104">An underpinning need in today’s business success, is hello ability toobuild and manage large scale network aware applications in an agile, flexible, secure and repeatable way.</span></span> <span data-ttu-id="9f5f9-105">Az Azure Resource Manager lehetővé teszi, hogy Ön toocreate ilyen alkalmazások, mint az erőforrások erőforráscsoportok egyetlen gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-105">Azure Resource Manager enables you toocreate such applications, as a single collection of resources in resource groups.</span></span> <span data-ttu-id="9f5f9-106">Ilyen erőforrások kezelése különböző erőforrás-szolgáltató az erőforrás-kezelő használatával.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-106">Such resources are managed through various resource providers under Resource Manager.</span></span>

<span data-ttu-id="9f5f9-107">Az Azure Resource Manager támaszkodik a különböző erőforrás szolgáltatók tooprovide tooyour erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-107">Azure Resource Manager relies on different resource providers tooprovide access tooyour resources.</span></span> <span data-ttu-id="9f5f9-108">Nincsenek három fő erőforrás-szolgáltatókon: hálózati, tárolási és számítási.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-108">There are three main resource providers: Network, Storage and Compute.</span></span> <span data-ttu-id="9f5f9-109">Ez a dokumentum ismerteti a hello jellemzőit és a hálózatierőforrás-szolgáltatónál, hello előnyei többek között:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-109">This document discusses hello characteristics and benefits of hello Network Resource Provider, including:</span></span>

* <span data-ttu-id="9f5f9-110">**Metaadatok** – információk tooresources címkék használatával adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-110">**Metadata** – you can add information tooresources using tags.</span></span> <span data-ttu-id="9f5f9-111">Ezekkel a címkékkel használt tootrack erőforrás-használat lehet erőforráscsoport-sablonok és előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-111">These tags can be used tootrack resource utilization across resource groups and subscriptions.</span></span>
* <span data-ttu-id="9f5f9-112">**Hatékonyabb vezérlését a hálózati** - hálózati erőforrások lazán, és részletesebb módon szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-112">**Greater control of your network** - network resources are loosely coupled and you can control them in a more granular fashion.</span></span> <span data-ttu-id="9f5f9-113">Ez azt jelenti, hogy nagyobb rugalmasságot biztosítanak a hello hálózati erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-113">This means you have more flexibility in managing hello networking resources.</span></span>
* <span data-ttu-id="9f5f9-114">**Gyorsabb konfigurációs** -hálózati erőforrások vannak lazán, mert hozhat létre és levezényelni a hálózati erőforrások párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-114">**Faster configuration** - because network resources are loosely coupled, you can create and orchestrate network resources in parallel.</span></span> <span data-ttu-id="9f5f9-115">Ez jelentősen lecsökkentette konfigurációs idő.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-115">This has drastically reduced configuration time.</span></span>
* <span data-ttu-id="9f5f9-116">**Szerepköralapú hozzáférés-vezérlés** -RBAC biztosít az alapértelmezett szerepköröket, meghatározott biztonsági hatókörrel, a biztonságos felügyeleti egyéni szerepkörök hozzáadása tooallowing hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-116">**Role Based Access Control** - RBAC provides default roles, with specific security scope, in addition tooallowing hello creation of custom roles for secure management.</span></span>
* <span data-ttu-id="9f5f9-117">**Egyszerűbb kezelés és a telepítés** -könnyebb toodeploy, és kezelheti az alkalmazásokat, mivel is létrehozhat egész alkalmazáscsoportokat erőforráscsoport erőforrásainak egyetlen gyűjteményeként.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-117">**Easier management and deployment** - it’s easier toodeploy and manage applications since you can can create an entire application stack as a single collection of resources in a resource group.</span></span> <span data-ttu-id="9f5f9-118">És gyorsabb toodeploy, mivel, egyszerűen adja meg a sablon JSON-adattartalmat telepítése.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-118">And faster toodeploy, since you can deploy by simply providing a template JSON payload.</span></span>
* <span data-ttu-id="9f5f9-119">**Gyors testreszabási** -stílusú deklaratív sablonok tooenable megismételhető és gyors központi telepítés testreszabási is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-119">**Rapid customization** - you can use declarative-style templates tooenable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="9f5f9-120">**A testreszabáshoz megismételhető** -stílusú deklaratív sablonok tooenable megismételhető és gyors központi telepítés testreszabási is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-120">**Repeatable customization** - you can use declarative-style templates tooenable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="9f5f9-121">**Felügyeleti felületek** -az erőforrások is használja a következő illesztők toomanage hello valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-121">**Management interfaces** - you can use any of hello following interfaces toomanage your resources:</span></span>
  * <span data-ttu-id="9f5f9-122">REST-alapú API</span><span class="sxs-lookup"><span data-stu-id="9f5f9-122">REST based API</span></span>
  * <span data-ttu-id="9f5f9-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9f5f9-123">PowerShell</span></span>
  * <span data-ttu-id="9f5f9-124">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="9f5f9-124">.NET SDK</span></span>
  * <span data-ttu-id="9f5f9-125">Node.JS SDK</span><span class="sxs-lookup"><span data-stu-id="9f5f9-125">Node.JS SDK</span></span>
  * <span data-ttu-id="9f5f9-126">Java SDK</span><span class="sxs-lookup"><span data-stu-id="9f5f9-126">Java SDK</span></span>
  * <span data-ttu-id="9f5f9-127">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9f5f9-127">Azure CLI</span></span>
  * <span data-ttu-id="9f5f9-128">Betekintő portál</span><span class="sxs-lookup"><span data-stu-id="9f5f9-128">Preview Portal</span></span>
  * <span data-ttu-id="9f5f9-129">Erőforrás-kezelő sablon nyelve</span><span class="sxs-lookup"><span data-stu-id="9f5f9-129">Resource Manager template language</span></span>

## <a name="network-resources"></a><span data-ttu-id="9f5f9-130">Hálózati erőforrások</span><span class="sxs-lookup"><span data-stu-id="9f5f9-130">Network resources</span></span>
<span data-ttu-id="9f5f9-131">Most függetlenül is felügyelhetők hálózati erőforrások, így ahelyett hogy azokat egyetlen számítási erőforrás (virtuális gép) kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-131">You can now manage network resources independently, instead of having them all managed through a single compute resource (a virtual machine).</span></span> <span data-ttu-id="9f5f9-132">Ez biztosítja, hogy nagyobb fokú rugalmasságot és agilitást az erőforráscsoport egy összetett és nagy méretű méretezési infrastruktúra létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-132">This ensures a higher degree of flexibility and agility in composing a complex and large scale infrastructure in a resource group.</span></span>

<span data-ttu-id="9f5f9-133">Egy üzembe helyezési minta egy többrétegű alkalmazást magában foglaló konceptuális ábrázolása az alábbi táblázat mutatja.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-133">A conceptual view of a sample deployment involving a multi-tiered application is presented below.</span></span> <span data-ttu-id="9f5f9-134">Az egyes erőforrások jelenik meg, például a hálózati adapterek, a nyilvános IP-címek és a virtuális gépek, egymástól függetlenül is felügyelhetők.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-134">Each resource you see, such as NICs, public IP addresses, and VMs, can be managed independently.</span></span>

![Hálózati erőforrás-modellje](./media/resource-groups-networking/Figure2.png)

<span data-ttu-id="9f5f9-136">Minden erőforrás azokat egy közös tulajdonságokat, és az egyéni tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-136">Every resource contains a common set of properties, and their individual property set.</span></span> <span data-ttu-id="9f5f9-137">hello közös tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-137">hello common properties are:</span></span>

| <span data-ttu-id="9f5f9-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9f5f9-138">Property</span></span> | <span data-ttu-id="9f5f9-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="9f5f9-139">Description</span></span> | <span data-ttu-id="9f5f9-140">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="9f5f9-140">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f5f9-141">**név**</span><span class="sxs-lookup"><span data-stu-id="9f5f9-141">**name**</span></span> |<span data-ttu-id="9f5f9-142">Egyedi erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-142">Unique resource name.</span></span> <span data-ttu-id="9f5f9-143">Minden erőforrás saját vonatkozó elnevezési korlátozás rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-143">Each resource type has its own naming restrictions.</span></span> |<span data-ttu-id="9f5f9-144">PIP01, VM01, NIC01</span><span class="sxs-lookup"><span data-stu-id="9f5f9-144">PIP01, VM01, NIC01</span></span> |
| <span data-ttu-id="9f5f9-145">**hely**</span><span class="sxs-lookup"><span data-stu-id="9f5f9-145">**location**</span></span> |<span data-ttu-id="9f5f9-146">Az Azure-régió, mely hello erőforrás található</span><span class="sxs-lookup"><span data-stu-id="9f5f9-146">Azure region in which hello resource resides</span></span> |<span data-ttu-id="9f5f9-147">westus, eastus</span><span class="sxs-lookup"><span data-stu-id="9f5f9-147">westus, eastus</span></span> |
| <span data-ttu-id="9f5f9-148">**azonosítója**</span><span class="sxs-lookup"><span data-stu-id="9f5f9-148">**id**</span></span> |<span data-ttu-id="9f5f9-149">Egyedi alapú URI-azonosító</span><span class="sxs-lookup"><span data-stu-id="9f5f9-149">Unique URI based identification</span></span> |<span data-ttu-id="9f5f9-150">következő<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span><span class="sxs-lookup"><span data-stu-id="9f5f9-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span></span> |

<span data-ttu-id="9f5f9-151">Ellenőrizheti az alábbi hello szakaszaiban található forrásanyagok hello egyes tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-151">You can check hello individual properties of resources in hello sections below.</span></span>

[!INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[!INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[!INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[!INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[!INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[!INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[!INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[!INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[!INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[!INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a><span data-ttu-id="9f5f9-152">Felügyeleti felületek</span><span class="sxs-lookup"><span data-stu-id="9f5f9-152">Management interfaces</span></span>
<span data-ttu-id="9f5f9-153">Az Azure-hálózati erőforrások különböző felületek segítségével kezelheti.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-153">You can manage your Azure networking resources using different interfaces.</span></span> <span data-ttu-id="9f5f9-154">Ebben a dokumentumban tárgyaljuk ezeket felületek fonókábel: REST API-t, és a sablonokat.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-154">In this document we will focus on tow of those interfaces: REST API, and templates.</span></span>

### <a name="rest-api"></a><span data-ttu-id="9f5f9-155">REST API</span><span class="sxs-lookup"><span data-stu-id="9f5f9-155">REST API</span></span>
<span data-ttu-id="9f5f9-156">A korábban említett csatolókat, beleértve a REST API-t, a .NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure Portal és sablonok számos hálózati erőforrások is felügyelhetők.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-156">As mentioned earlier, network resources can be managed via a variety of interfaces, including REST API,.NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure Portal and templates.</span></span>

<span data-ttu-id="9f5f9-157">hello Rest API toohello HTTP 1.1 protokoll specifikációja felel meg.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-157">hello Rest API’s conform toohello HTTP 1.1 protocol specification.</span></span> <span data-ttu-id="9f5f9-158">hello általános URI szerkezete hello API alatt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-158">hello general URI structure of hello API is presented below:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

<span data-ttu-id="9f5f9-159">És a kapcsos zárójelek hello paraméterek határoz meg a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-159">And hello parameters in braces represent hello following elements:</span></span>

* <span data-ttu-id="9f5f9-160">**előfizetés-azonosító** -Azure-előfizetése azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-160">**subscription-id** - your Azure subscription id.</span></span>
* <span data-ttu-id="9f5f9-161">**erőforrás-szolgáltató-névtér** -használt hello szolgáltató névterét.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-161">**resource-provider-namespace** - namespace for hello provider being used.</span></span> <span data-ttu-id="9f5f9-162">hello hello hálózati erőforrás-szolgáltató értéke *Microsoft.Network*.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-162">hello value for hello network resource provider is *Microsoft.Network*.</span></span>
* <span data-ttu-id="9f5f9-163">**terület-neve** -hello Azure-régiót neve</span><span class="sxs-lookup"><span data-stu-id="9f5f9-163">**region-name** - hello Azure region name</span></span>

<span data-ttu-id="9f5f9-164">hello következő HTTP-metódus támogatottak, ha így hívások toohello REST API:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-164">hello following HTTP methods are supported when making calls toohello REST API:</span></span>

* <span data-ttu-id="9f5f9-165">**PUT** - használt toocreate egy adott típusú erőforrást egy erőforrás-tulajdonság módosítására, vagy módosítsa a erőforrások közötti társítást.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-165">**PUT** - used toocreate a resource of a given type, modify a resource property or change an association between resources.</span></span>
* <span data-ttu-id="9f5f9-166">**ELSŐ** -használt tooretrieve adatok kiosztott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-166">**GET** - used tooretrieve information for a provisioned resource.</span></span>
* <span data-ttu-id="9f5f9-167">**Törlés** -toodelete meglévő erőforrás használja.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-167">**DELETE** - used toodelete an existing resource.</span></span>

<span data-ttu-id="9f5f9-168">Hello kérelem és a válasz felel meg a tooa JSON-adattartalom formátuma.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-168">Both hello request and response conform tooa JSON payload format.</span></span> <span data-ttu-id="9f5f9-169">További részletekért lásd: [Azure erőforrás-kezelési API-k](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f5f9-169">For more details, see [Azure Resource Management APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span></span>

### <a name="resource-manager-template-language"></a><span data-ttu-id="9f5f9-170">Erőforrás-kezelő sablon nyelve</span><span class="sxs-lookup"><span data-stu-id="9f5f9-170">Resource Manager template language</span></span>
<span data-ttu-id="9f5f9-171">Továbbá toomanaging erőforrások imperatively (API-k vagy SDK) keresztül, a is használja a deklaratív programozási stílus toobuild és hálózati erőforrások kezelése hello Resource Manager sablon nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-171">In addition toomanaging resources imperatively (via APIs or SDK), you can also use a declarative programming style toobuild and manage network resources by using hello Resource Manager Template Language.</span></span>

<span data-ttu-id="9f5f9-172">A minta egy sablon megjelenítése alább –</span><span class="sxs-lookup"><span data-stu-id="9f5f9-172">A sample representation of a template is provided below –</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

<span data-ttu-id="9f5f9-173">hello sablon elsősorban JSON-leírásuk hello erőforrások és a paraméterek keresztül beszúrta hello példány értéke.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-173">hello template is primarily a JSON description of hello resources and hello instance values injected via parameters.</span></span> <span data-ttu-id="9f5f9-174">az alábbi példa hello használt toocreate 2 alhálózatok virtuális hálózat lehet.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-174">hello example below can be used toocreate a virtual network with 2 subnets.</span></span>

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

<span data-ttu-id="9f5f9-175">Lehetősége van hello biztosító hello paraméterértékek manuálisan, egy sablon használatakor, vagy egy paraméterfájl is használhat.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-175">You have hello option of providing hello parameter values manually when using a template, or you can use a parameter file.</span></span> <span data-ttu-id="9f5f9-176">hello az alábbi példában használt a fenti hello sablonnal paraméter értékek toobe lehetséges készlete:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-176">hello example below shows a possible set of parameter values toobe used with hello template above:</span></span>

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


<span data-ttu-id="9f5f9-177">hello fő előnyei-sablonokkal a következők:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-177">hello main advantages of using templates are:</span></span>

* <span data-ttu-id="9f5f9-178">Egy összetett infrastruktúra deklaratív Style erőforráscsoportban is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-178">You can build a complex infrastructure in a resource group in a declarative style.</span></span> <span data-ttu-id="9f5f9-179">hello hello erőforrások, például az függőségi felügyeleti létrehozásának orchestration, kezelését a kezelő által.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-179">hello orchestration of creating hello resources, including dependency management, is handled by Resource Manager.</span></span>
* <span data-ttu-id="9f5f9-180">hello infrastruktúra is létrehozható egy ismételhető módon különböző régiókban és régión belül paraméterek egyszerűen módosításával.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-180">hello infrastructure can be created in a repeatable way across various regions and within a region by simply changing parameters.</span></span>
* <span data-ttu-id="9f5f9-181">hello deklaratív stílus tooshorter átfutási idő hello sablonok létrehozása és terítésével hello infrastruktúra vezet.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-181">hello declarative style leads tooshorter lead time in building hello templates and rolling out hello infrastructure.</span></span>

<span data-ttu-id="9f5f9-182">A minta-sablonok, lásd: [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="9f5f9-182">For sample templates, see [Azure quickstart templates](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="9f5f9-183">A Resource Manager sablon nyelvi hello további információkért lásd: [Azure Resource Manager sablon nyelvi](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9f5f9-183">For more information on hello Resource Manager Template Language, see [Azure Resource Manager Template Language](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="9f5f9-184">a fenti hello mintasablon hello virtuális hálózati és alhálózati erőforrásokat használ.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-184">hello sample template above uses hello virtual network and subnet resources.</span></span> <span data-ttu-id="9f5f9-185">Nincsenek más hálózati erőforrásokhoz, használhatja az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-185">There are other network resources you can use as listed below:</span></span>

### <a name="using-a-template"></a><span data-ttu-id="9f5f9-186">Egy sablon használatával</span><span class="sxs-lookup"><span data-stu-id="9f5f9-186">Using a template</span></span>
<span data-ttu-id="9f5f9-187">Szolgáltatások tooAzure sablonból powershellel, AzureCLI, vagy a Githubról egy kattintson toodeploy végrehajtásával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-187">You can deploy services tooAzure from a template by using PowerShell, AzureCLI, or by performing a click toodeploy from GitHub.</span></span> <span data-ttu-id="9f5f9-188">a Githubon, sablonból toodeploy szolgáltatások hello lépésekkel hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="9f5f9-188">toodeploy services from a template in GitHub, execute hello following steps:</span></span>

1. <span data-ttu-id="9f5f9-189">Nyissa meg a hello template3 fájlt a Githubról.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-189">Open hello template3 file from GitHub.</span></span> <span data-ttu-id="9f5f9-190">Tegyük fel, nyissa meg a [, két alhálózattal a virtuális hálózati](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="9f5f9-190">As an example, open [Virtual network with two subnets](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span></span>
2. <span data-ttu-id="9f5f9-191">Kattintson a **tooAzure telepítése**, majd jelentkezzen be a toohello Azure-portálon a hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-191">Click on **Deploy tooAzure**, and then sign in on toohello Azure portal with your credentials.</span></span>
3. <span data-ttu-id="9f5f9-192">Ellenőrizze a hello sablont, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-192">Verify hello template, and then click **Save**.</span></span>
4. <span data-ttu-id="9f5f9-193">Kattintson a **paraméterek szerkesztése** és adjon meg egy helyet, például a *USA nyugati régiója*, hello hálózatok és alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-193">Click **Edit parameters** and select a location, such as *West US*, for hello vnet and subnets.</span></span>
5. <span data-ttu-id="9f5f9-194">Szükség esetén módosítsa a hello **CÍMELŐTAGJA** és **SUBNETPREFIX** paramétereket, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-194">If necessary, change hello **ADDRESSPREFIX** and **SUBNETPREFIX** parameters, and then click **OK**.</span></span>
6. <span data-ttu-id="9f5f9-195">Kattintson a **válasszon egy erőforráscsoportot** majd kattintson a kívánt tooadd hello hálózatok és alhálózatok hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-195">Click **Select a resource group** and then click on hello resource group you want tooadd hello vnet and subnets to.</span></span> <span data-ttu-id="9f5f9-196">Alternatív megoldásként létrehozhat egy új erőforráscsoportot kattintva **, vagy hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-196">Alternatively, you can create a new resource group by clicking **Or create new**.</span></span>
7. <span data-ttu-id="9f5f9-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-197">Click **Create**.</span></span> <span data-ttu-id="9f5f9-198">Figyelje meg, csempe megjelenítése hello **kiépítés sablon-üzembehelyezés**.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-198">Notice hello tile displaying **Provisioning Template deployment**.</span></span> <span data-ttu-id="9f5f9-199">Miután hello üzembe helyezés hasonló tooone az alábbi a képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9f5f9-199">Once hello deployment is done, you will see a screen similar tooone below.</span></span>

![Sablon üzembe helyezési minta](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a><span data-ttu-id="9f5f9-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f5f9-201">Next steps</span></span>
[<span data-ttu-id="9f5f9-202">Az Azure Resource Manager sablon nyelve</span><span class="sxs-lookup"><span data-stu-id="9f5f9-202">Azure Resource Manager Template Language</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[<span data-ttu-id="9f5f9-203">Az Azure hálózatkezelés – általánosan használt sablon</span><span class="sxs-lookup"><span data-stu-id="9f5f9-203">Azure Networking – commonly used templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

[<span data-ttu-id="9f5f9-204">Az Azure Resource Manager és klasszikus üzembe helyezési összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="9f5f9-204">Azure Resource Manager vs. classic deployment</span></span>](../azure-resource-manager/resource-manager-deployment-model.md)

[<span data-ttu-id="9f5f9-205">Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="9f5f9-205">Azure Resource Manager Overview</span></span>](../azure-resource-manager/resource-group-overview.md)

