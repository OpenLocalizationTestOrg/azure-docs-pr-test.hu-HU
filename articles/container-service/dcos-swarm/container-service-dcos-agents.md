---
title: "Azure Tárolószolgáltatási DC/OS-ügynök készleteket |} Microsoft Docs"
description: "A nyilvános és titkos ügynök készletek működéséről az Azure tároló szolgáltatás DC/OS-fürt"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: da4a196b1a73c78dfff7d8310edcc349b8d10665
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a><span data-ttu-id="6b60a-104">Azure Tárolószolgáltatási DC/OS-ügynök készleteket</span><span class="sxs-lookup"><span data-stu-id="6b60a-104">DC/OS agent pools for Azure Container Service</span></span>
<span data-ttu-id="6b60a-105">Azure Tárolószolgáltatási DC/OS fürtök ügynök csomópontok két rendelkezik, egy nyilvános készlet és egy személyes készletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6b60a-105">DC/OS clusters in Azure Container Service contain agent nodes in two pools, a public pool and a private pool.</span></span> <span data-ttu-id="6b60a-106">Egy alkalmazás vagy a készletbe, a tárolószolgáltatás gépek közötti kisegítő érintő is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="6b60a-106">An application can be deployed to either pool, affecting accessibility between machines in your container service.</span></span> <span data-ttu-id="6b60a-107">A gépek kommunikál az internettel (nyilvános), vagy belső (magánhálózati) tartani.</span><span class="sxs-lookup"><span data-stu-id="6b60a-107">The machines can be exposed to the internet (public) or kept internal (private).</span></span> <span data-ttu-id="6b60a-108">Ez a cikk rövid áttekintést nyújt, ezért azonban nyilvános és titkos.</span><span class="sxs-lookup"><span data-stu-id="6b60a-108">This article gives a brief overview of why there are public and private pools.</span></span>


* <span data-ttu-id="6b60a-109">**Személyes ügynökök**: titkos ügynök csomópontokat átszervezhető nem irányítható hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="6b60a-109">**Private agents**: Private agent nodes run through a non-routable network.</span></span> <span data-ttu-id="6b60a-110">Ez a hálózat csak érhető el, az admin zónából, vagy a nyilvános zóna útválasztón keresztül.</span><span class="sxs-lookup"><span data-stu-id="6b60a-110">This network is only accessible from the admin zone or through the public zone edge router.</span></span> <span data-ttu-id="6b60a-111">Alapértelmezés szerint a DC/OS indít alkalmazások titkos ügynök csomópontján.</span><span class="sxs-lookup"><span data-stu-id="6b60a-111">By default, DC/OS launches apps on private agent nodes.</span></span> 

* <span data-ttu-id="6b60a-112">**Nyilvános ügynökök**: nyilvános ügynök csomópontok DC/OS-alkalmazások és szolgáltatások futtatása egy nyilvánosan elérhető a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="6b60a-112">**Public agents**: Public agent nodes run DC/OS apps and services through a publicly accessible network.</span></span> 

<span data-ttu-id="6b60a-113">A DC/OS hálózati biztonsággal kapcsolatos további információkért lásd: a [DC/OS-dokumentáció](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span><span class="sxs-lookup"><span data-stu-id="6b60a-113">For more information about DC/OS network security, see the [DC/OS documentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span></span>

## <a name="deploy-agent-pools"></a><span data-ttu-id="6b60a-114">Ügynök-készletek központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6b60a-114">Deploy agent pools</span></span>

<span data-ttu-id="6b60a-115">A DC/OS-ügynök készletek az Azure Tárolószolgáltatás jönnek létre az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6b60a-115">The DC/OS agent pools In Azure Container Service are created as follows:</span></span>

* <span data-ttu-id="6b60a-116">A **titkos készlet** tartalmazza, hogy mikor megadja ügynök csomópontok száma, [a DC/OS-fürt üzembe](container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="6b60a-116">The **private pool** contains the number of agent nodes that you specify when you [deploy the DC/OS cluster](container-service-deployment.md).</span></span> 

* <span data-ttu-id="6b60a-117">A **nyilvános készlet** kezdetben a egy előre meghatározott számú ügynök csomópontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6b60a-117">The **public pool** initially contains a predetermined number of agent nodes.</span></span> <span data-ttu-id="6b60a-118">Ha a DC/OS-fürt ki van építve a rendszer automatikusan hozzáadja a készlet.</span><span class="sxs-lookup"><span data-stu-id="6b60a-118">This pool is added automatically when the DC/OS cluster is provisioned.</span></span>

<span data-ttu-id="6b60a-119">A privát készlet és a nyilvános készlet olyan Azure virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="6b60a-119">The private pool and the public pool are Azure virtual machine scale sets.</span></span> <span data-ttu-id="6b60a-120">Telepítés után a készletek is átméretezhetők.</span><span class="sxs-lookup"><span data-stu-id="6b60a-120">You can resize these pools after deployment.</span></span>

## <a name="use-agent-pools"></a><span data-ttu-id="6b60a-121">Ügynök-készletek használatára</span><span class="sxs-lookup"><span data-stu-id="6b60a-121">Use agent pools</span></span>
<span data-ttu-id="6b60a-122">Alapértelmezés szerint **Marathon** bármely új alkalmazás telepíti a *titkos* ügynök csomópontok.</span><span class="sxs-lookup"><span data-stu-id="6b60a-122">By default, **Marathon** deploys any new application to the *private* agent nodes.</span></span> <span data-ttu-id="6b60a-123">Explicit módon telepítheti az alkalmazást, hogy a *nyilvános* csomópontok az alkalmazás létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="6b60a-123">You have to explicitly deploy the application to the *public* nodes during the creation of the application.</span></span> <span data-ttu-id="6b60a-124">Válassza ki a **nem kötelező** lapra, és írja be **slave_public** a a **elfogadott erőforrás szerepkörök** érték.</span><span class="sxs-lookup"><span data-stu-id="6b60a-124">Select the **Optional** tab and enter **slave_public** for the **Accepted Resource Roles** value.</span></span> <span data-ttu-id="6b60a-125">Ez a folyamat dokumentált [Itt](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) és a a [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6b60a-125">This process is documented [here](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) and in the [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b60a-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b60a-126">Next steps</span></span>
* <span data-ttu-id="6b60a-127">Tudjon meg többet az [a DC/OS-tárolók kezelése](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="6b60a-127">Read more about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

* <span data-ttu-id="6b60a-128">Megtudhatja, hogyan [nyissa meg a tűzfal](container-service-enable-public-access.md) a DC/OS-tárolók nyilvános hozzáférés engedélyezése az Azure által biztosított.</span><span class="sxs-lookup"><span data-stu-id="6b60a-128">Learn how to [open the firewall](container-service-enable-public-access.md) provided by Azure to allow public access to your DC/OS containers.</span></span>

