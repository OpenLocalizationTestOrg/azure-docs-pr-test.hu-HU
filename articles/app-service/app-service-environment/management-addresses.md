---
title: "aaaAzure App Service Environment-környezet felügyeleti címei"
description: "Listák hello felügyeleti címek használt toocommand egy App Service Environment-környezet"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: b34b6266dc3a35915421b14bf34eddc07c2825c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-management-addresses"></a><span data-ttu-id="8a951-103">App Service Environment-környezet felügyeleti címei</span><span class="sxs-lookup"><span data-stu-id="8a951-103">App Service Environment management addresses</span></span>

<span data-ttu-id="8a951-104">App Service Environment(ASE) hello hello Azure App Service egy alhálózatba az Azure Virtual Network (VNet) a központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="8a951-104">hello App Service Environment(ASE) is a deployment of hello Azure App Service into a subnet in your Azure Virtual Network (VNet).</span></span>  <span data-ttu-id="8a951-105">hello ASE hello Azure App Service elérhetőknek kell lenniük, hogy a fiók kezelhető.</span><span class="sxs-lookup"><span data-stu-id="8a951-105">hello ASE must be accessible from hello Azure App Service so that it can be managed.</span></span>  <span data-ttu-id="8a951-106">Ez ASE felügyeleti forgalom halad át a felhasználó által vezérelt hálózati hello.</span><span class="sxs-lookup"><span data-stu-id="8a951-106">This ASE management traffic traverses hello user controlled network.</span></span>  <span data-ttu-id="8a951-107">Azure App Service management kiszolgálók toohello nyilvános VIP kapcsolódó hello mértékéig származik.</span><span class="sxs-lookup"><span data-stu-id="8a951-107">It comes from Azure App Service management servers toohello public VIP that is associated with hello ASE.</span></span>  <span data-ttu-id="8a951-108">Hello ASE részleteinek a hálózat függőségek olvasási [hálózati szempontok és az App Service Environment-környezet hello][networking].</span><span class="sxs-lookup"><span data-stu-id="8a951-108">For details on hello ASE networking dependencies read [Networking considerations and hello App Service Environment][networking].</span></span>  <span data-ttu-id="8a951-109">Általános információk a hello ASE Kezdésként használhatja az [bemutatása toohello App Service Environment-környezet][intro].</span><span class="sxs-lookup"><span data-stu-id="8a951-109">For general information on hello ASE you can start with [Introduction toohello App Service Environment][intro].</span></span>

<span data-ttu-id="8a951-110">Ez a dokumentum hello forrás IP-címet a felügyeleti forgalom toohello ASE sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="8a951-110">This document lists hello source IPs for management traffic toohello ASE.</span></span> <span data-ttu-id="8a951-111">Használja a címek toocreate hálózati biztonsági csoportok toolock le bejövő forgalmat, vagy azok használatát az útvonaltáblák igény szerint.</span><span class="sxs-lookup"><span data-stu-id="8a951-111">You can use these addresses toocreate Network Security Groups toolock down incoming traffic or use them in Route Tables as needed.</span></span>  <span data-ttu-id="8a951-112">toouse toouse kell ezt az információt:</span><span class="sxs-lookup"><span data-stu-id="8a951-112">toouse this information you need toouse:</span></span>

* <span data-ttu-id="8a951-113">Minden egyes felsorolt hello IP-címek</span><span class="sxs-lookup"><span data-stu-id="8a951-113">hello IP addresses that are listed for All regions</span></span>
* <span data-ttu-id="8a951-114">hello IP-címek, amelyek a ASE rendszerbe való egyezés toohello régió.</span><span class="sxs-lookup"><span data-stu-id="8a951-114">hello IP addresses that match toohello region that your ASE is deployed into.</span></span>

<span data-ttu-id="8a951-115">hello bejövő felügyeleti forgalom származik következő IP-címek tooports 454 és a 455.</span><span class="sxs-lookup"><span data-stu-id="8a951-115">hello incoming management traffic comes in from these IP addresses tooports 454 and 455.</span></span>

| <span data-ttu-id="8a951-116">Régió</span><span class="sxs-lookup"><span data-stu-id="8a951-116">Region</span></span> | <span data-ttu-id="8a951-117">Címek</span><span class="sxs-lookup"><span data-stu-id="8a951-117">Addresses</span></span> |
|--------|-----------|
| <span data-ttu-id="8a951-118">Minden egyes</span><span class="sxs-lookup"><span data-stu-id="8a951-118">All regions</span></span> | <span data-ttu-id="8a951-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span><span class="sxs-lookup"><span data-stu-id="8a951-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span></span> |
| <span data-ttu-id="8a951-120">USA déli középső RÉGIÓJA & északi USA középső RÉGIÓJA</span><span class="sxs-lookup"><span data-stu-id="8a951-120">South Central US & North Central US</span></span> | <span data-ttu-id="8a951-121">23.102.188.65, 191.236.154.88</span><span class="sxs-lookup"><span data-stu-id="8a951-121">23.102.188.65, 191.236.154.88</span></span> |
| <span data-ttu-id="8a951-122">Ausztrália délkeleti & Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="8a951-122">Australia Southeast & Australia East</span></span> | <span data-ttu-id="8a951-123">23.101.234.41, 104.210.90.65</span><span class="sxs-lookup"><span data-stu-id="8a951-123">23.101.234.41, 104.210.90.65</span></span> |
| <span data-ttu-id="8a951-124">USA nyugati régiója & USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="8a951-124">US West & US East</span></span> | <span data-ttu-id="8a951-125">104.45.227.37, 191.236.60.72</span><span class="sxs-lookup"><span data-stu-id="8a951-125">104.45.227.37, 191.236.60.72</span></span> |
| <span data-ttu-id="8a951-126">Nyugat-Európában & Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="8a951-126">West Europe & North Europe</span></span> | <span data-ttu-id="8a951-127">191.233.94.45, 191.237.222.191</span><span class="sxs-lookup"><span data-stu-id="8a951-127">191.233.94.45, 191.237.222.191</span></span> |
| <span data-ttu-id="8a951-128">Központi USA nyugati régiója & 2 USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="8a951-128">West Central US & West US 2</span></span> | <span data-ttu-id="8a951-129">13.78.148.75, 13.66.225.188</span><span class="sxs-lookup"><span data-stu-id="8a951-129">13.78.148.75, 13.66.225.188</span></span> |
| <span data-ttu-id="8a951-130">USA középső RÉGIÓJA & USA keleti régiója 2. régiója</span><span class="sxs-lookup"><span data-stu-id="8a951-130">Central US & East US 2</span></span> | <span data-ttu-id="8a951-131">104.43.165.73, 104.46.108.135</span><span class="sxs-lookup"><span data-stu-id="8a951-131">104.43.165.73, 104.46.108.135</span></span> |
| <span data-ttu-id="8a951-132">Kelet-Ázsia & Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="8a951-132">East Asia & Southeast Asia</span></span> | <span data-ttu-id="8a951-133">23.99.115.5, 104.215.158.33</span><span class="sxs-lookup"><span data-stu-id="8a951-133">23.99.115.5, 104.215.158.33</span></span> |
| <span data-ttu-id="8a951-134">Kelet-japán & Nyugat-japán</span><span class="sxs-lookup"><span data-stu-id="8a951-134">Japan East & Japan West</span></span> | <span data-ttu-id="8a951-135">104.41.185.116, 191.239.104.48</span><span class="sxs-lookup"><span data-stu-id="8a951-135">104.41.185.116, 191.239.104.48</span></span> |
| <span data-ttu-id="8a951-136">Kanadában központi & Kanada keleti régiója</span><span class="sxs-lookup"><span data-stu-id="8a951-136">Canada Central & Canada East</span></span> | <span data-ttu-id="8a951-137">40.85.230.101, 40.86.229.100</span><span class="sxs-lookup"><span data-stu-id="8a951-137">40.85.230.101, 40.86.229.100</span></span> |
| <span data-ttu-id="8a951-138">Egyesült Királyság nyugati régiója & Egyesült Királyság déli régiója</span><span class="sxs-lookup"><span data-stu-id="8a951-138">UK West & UK South</span></span> | <span data-ttu-id="8a951-139">51.141.8.34, 51.140.185.75</span><span class="sxs-lookup"><span data-stu-id="8a951-139">51.141.8.34, 51.140.185.75</span></span> |
| <span data-ttu-id="8a951-140">Koreai Dél & Koreai központi</span><span class="sxs-lookup"><span data-stu-id="8a951-140">Korea South & Korea Central</span></span> | <span data-ttu-id="8a951-141">52.231.200.177, 52.231.32.117</span><span class="sxs-lookup"><span data-stu-id="8a951-141">52.231.200.177, 52.231.32.117</span></span> |
| <span data-ttu-id="8a951-142">Dél-Brazília & USA déli középső RÉGIÓJA</span><span class="sxs-lookup"><span data-stu-id="8a951-142">Brazil South & South Central US</span></span>| <span data-ttu-id="8a951-143">104.41.46.178, 23.102.188.65</span><span class="sxs-lookup"><span data-stu-id="8a951-143">104.41.46.178, 23.102.188.65</span></span> |
| <span data-ttu-id="8a951-144">Közép-Indiában és Dél-India</span><span class="sxs-lookup"><span data-stu-id="8a951-144">Central India & South India</span></span> | <span data-ttu-id="8a951-145">104.211.98.24, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="8a951-145">104.211.98.24, 104.211.225.66</span></span> |
| <span data-ttu-id="8a951-146">Nyugat-Indiában és Dél-India</span><span class="sxs-lookup"><span data-stu-id="8a951-146">West India & South India</span></span> | <span data-ttu-id="8a951-147">104.211.160.229, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="8a951-147">104.211.160.229, 104.211.225.66</span></span> |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
