---
title: "Azure Tárolószolgáltatás aaaDC/OS-ügynök készleteket |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS-fürtről a hello nyilvános és titkos ügynök készletek működése"
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
ms.openlocfilehash: c7d3889db07cb9908e8b68b668bd8a14ef3c2552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a><span data-ttu-id="b9a6c-104">Azure Tárolószolgáltatási DC/OS-ügynök készleteket</span><span class="sxs-lookup"><span data-stu-id="b9a6c-104">DC/OS agent pools for Azure Container Service</span></span>
<span data-ttu-id="b9a6c-105">Azure Tárolószolgáltatási DC/OS fürtök ügynök csomópontok két rendelkezik, egy nyilvános készlet és egy személyes készletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-105">DC/OS clusters in Azure Container Service contain agent nodes in two pools, a public pool and a private pool.</span></span> <span data-ttu-id="b9a6c-106">Egy alkalmazás telepíthető tooeither készletbe, a tárolószolgáltatás gépek közötti kisegítő érintő.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-106">An application can be deployed tooeither pool, affecting accessibility between machines in your container service.</span></span> <span data-ttu-id="b9a6c-107">hello gépek lehet kitett toohello internet (nyilvános) vagy belső (magánhálózati).</span><span class="sxs-lookup"><span data-stu-id="b9a6c-107">hello machines can be exposed toohello internet (public) or kept internal (private).</span></span> <span data-ttu-id="b9a6c-108">Ez a cikk rövid áttekintést nyújt, ezért azonban nyilvános és titkos.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-108">This article gives a brief overview of why there are public and private pools.</span></span>


* <span data-ttu-id="b9a6c-109">**Személyes ügynökök**: titkos ügynök csomópontokat átszervezhető nem irányítható hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-109">**Private agents**: Private agent nodes run through a non-routable network.</span></span> <span data-ttu-id="b9a6c-110">Ez a hálózat hello admin zónából vagy hello nyilvános zóna peremhálózati útválasztó csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-110">This network is only accessible from hello admin zone or through hello public zone edge router.</span></span> <span data-ttu-id="b9a6c-111">Alapértelmezés szerint a DC/OS indít alkalmazások titkos ügynök csomópontján.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-111">By default, DC/OS launches apps on private agent nodes.</span></span> 

* <span data-ttu-id="b9a6c-112">**Nyilvános ügynökök**: nyilvános ügynök csomópontok DC/OS-alkalmazások és szolgáltatások futtatása egy nyilvánosan elérhető a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-112">**Public agents**: Public agent nodes run DC/OS apps and services through a publicly accessible network.</span></span> 

<span data-ttu-id="b9a6c-113">A DC/OS hálózati biztonsággal kapcsolatos további információkért lásd: hello [DC/OS-dokumentáció](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span><span class="sxs-lookup"><span data-stu-id="b9a6c-113">For more information about DC/OS network security, see hello [DC/OS documentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span></span>

## <a name="deploy-agent-pools"></a><span data-ttu-id="b9a6c-114">Ügynök-készletek központi telepítése</span><span class="sxs-lookup"><span data-stu-id="b9a6c-114">Deploy agent pools</span></span>

<span data-ttu-id="b9a6c-115">hello DC/OS-ügynök készletek az Azure Tárolószolgáltatás jönnek létre az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b9a6c-115">hello DC/OS agent pools In Azure Container Service are created as follows:</span></span>

* <span data-ttu-id="b9a6c-116">Hello **titkos készlet** hello számú meg kell adnia mikor ügynök csomópontot tartalmaz, [hello DC/OS-fürt üzembe](container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="b9a6c-116">hello **private pool** contains hello number of agent nodes that you specify when you [deploy hello DC/OS cluster](container-service-deployment.md).</span></span> 

* <span data-ttu-id="b9a6c-117">Hello **nyilvános készlet** kezdetben a egy előre meghatározott számú ügynök csomópontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-117">hello **public pool** initially contains a predetermined number of agent nodes.</span></span> <span data-ttu-id="b9a6c-118">Amikor hello DC/OS-fürt ki van építve a rendszer automatikusan hozzáadja a készlet.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-118">This pool is added automatically when hello DC/OS cluster is provisioned.</span></span>

<span data-ttu-id="b9a6c-119">hello titkos készlet és hello nyilvános készlet olyan Azure virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-119">hello private pool and hello public pool are Azure virtual machine scale sets.</span></span> <span data-ttu-id="b9a6c-120">Telepítés után a készletek is átméretezhetők.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-120">You can resize these pools after deployment.</span></span>

## <a name="use-agent-pools"></a><span data-ttu-id="b9a6c-121">Ügynök-készletek használatára</span><span class="sxs-lookup"><span data-stu-id="b9a6c-121">Use agent pools</span></span>
<span data-ttu-id="b9a6c-122">Alapértelmezés szerint **Marathon** telepíti, minden új alkalmazás toohello *titkos* ügynök csomópontok.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-122">By default, **Marathon** deploys any new application toohello *private* agent nodes.</span></span> <span data-ttu-id="b9a6c-123">Hello alkalmazás toohello telepítése tooexplicitly rendelkezik *nyilvános* csomópontok hello alkalmazás hello létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-123">You have tooexplicitly deploy hello application toohello *public* nodes during hello creation of hello application.</span></span> <span data-ttu-id="b9a6c-124">Jelölje be hello **nem kötelező** lapra, és írja be **slave_public** a hello **elfogadott erőforrás-szerepkörök** érték.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-124">Select hello **Optional** tab and enter **slave_public** for hello **Accepted Resource Roles** value.</span></span> <span data-ttu-id="b9a6c-125">Ez a folyamat dokumentált [Itt](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) és hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-125">This process is documented [here](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) and in hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9a6c-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9a6c-126">Next steps</span></span>
* <span data-ttu-id="b9a6c-127">Tudjon meg többet az [a DC/OS-tárolók kezelése](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="b9a6c-127">Read more about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

* <span data-ttu-id="b9a6c-128">Ismerje meg, hogyan túl[nyissa meg a hello tűzfal](container-service-enable-public-access.md) Azure tooallow nyilvános hozzáférés tooyour DC/OS tárolók által biztosított.</span><span class="sxs-lookup"><span data-stu-id="b9a6c-128">Learn how too[open hello firewall](container-service-enable-public-access.md) provided by Azure tooallow public access tooyour DC/OS containers.</span></span>

