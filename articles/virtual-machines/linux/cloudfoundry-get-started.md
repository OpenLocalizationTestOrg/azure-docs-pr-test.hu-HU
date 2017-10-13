---
title: "a Microsoft Azure-felhő Foundry elindítva aaaGetting |} Microsoft Docs"
description: "A Microsoft Azure OSS vagy döntő felhő Foundry futtató"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 56300d5a0a75b5a9f46145a49ed3f5daa774375a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-foundry-on-azure"></a><span data-ttu-id="b8e20-103">Cloud Foundry az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b8e20-103">Cloud Foundry on Azure</span></span>

<span data-ttu-id="b8e20-104">Felhő Foundry egy nyílt forráskódú platform,--szolgáltatás (PaaS) létrehozása, telepítése és a különböző nyelv és keretrendszer 12-tényező alkalmazások operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="b8e20-104">Cloud Foundry is an open-source platform-as-a-service (PaaS) for building, deploying, and operating 12-factor applications developed in various languages and frameworks.</span></span> <span data-ttu-id="b8e20-105">Ez a dokumentum ismerteti a felhő Foundry futó Azure, és hogyan kezdheti rendelkezik hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="b8e20-105">This document describes hello options you have for running Cloud Foundry on Azure and how you can get started.</span></span>

## <a name="cloud-foundry-offerings"></a><span data-ttu-id="b8e20-106">Foundry felhőajánlatokat</span><span class="sxs-lookup"><span data-stu-id="b8e20-106">Cloud Foundry offerings</span></span>

<span data-ttu-id="b8e20-107">Nincsenek elérhető toorun felhő Foundry Azure kétféle: nyílt forráskódú felhő Foundry (OSS CF) és döntő felhő Foundry (PCF).</span><span class="sxs-lookup"><span data-stu-id="b8e20-107">There are two forms of Cloud Foundry available toorun on Azure: open-source Cloud Foundry (OSS CF) and Pivotal Cloud Foundry (PCF).</span></span> <span data-ttu-id="b8e20-108">OSS CF van egy teljes mértékben [nyílt forráskódú](https://github.com/cloudfoundry) hello felhő Foundry Foundation által kezelt felhő Foundry verzióját.</span><span class="sxs-lookup"><span data-stu-id="b8e20-108">OSS CF is an entirely [open-source](https://github.com/cloudfoundry) version of Cloud Foundry managed by hello Cloud Foundry Foundation.</span></span> <span data-ttu-id="b8e20-109">Döntő felhő Foundry egy vállalati felhő Foundry döntő szoftver Inc. terjesztése Úgy tekintünk, néhány hello különbségei hello két ajánlatokat.</span><span class="sxs-lookup"><span data-stu-id="b8e20-109">Pivotal Cloud Foundry is an enterprise distribution of Cloud Foundry from Pivotal Software Inc. We look at some of hello differences between hello two offerings.</span></span>

### <a name="open-source-cloud-foundry"></a><span data-ttu-id="b8e20-110">Nyílt forráskódú felhő Foundry</span><span class="sxs-lookup"><span data-stu-id="b8e20-110">Open-source Cloud Foundry</span></span>

<span data-ttu-id="b8e20-111">Először telepítése BOSH igazgató, és majd a felhő Foundry hello segítségével telepítésével OSS felhő Foundry Azure telepíthet [utasításokat a Githubon](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span><span class="sxs-lookup"><span data-stu-id="b8e20-111">You can deploy OSS Cloud Foundry on Azure by first deploying a BOSH director and then deploying Cloud Foundry, using hello [instructions provided on GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span></span> <span data-ttu-id="b8e20-112">toolearn több OSS CF használatáról lásd: hello [dokumentáció](https://docs.cloudfoundry.org/) hello felhő Foundry Foundation által biztosított.</span><span class="sxs-lookup"><span data-stu-id="b8e20-112">toolearn more about using OSS CF, see hello [documentation](https://docs.cloudfoundry.org/) provided by hello Cloud Foundry Foundation.</span></span>

<span data-ttu-id="b8e20-113">Microsoft közösségi csatornák a következő hello keresztül elérhető legjobb OSS CF támogatást nyújt a:</span><span class="sxs-lookup"><span data-stu-id="b8e20-113">Microsoft provides best-effort support for OSS CF through hello following community channels:</span></span>

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a><span data-ttu-id="b8e20-114">a bosh-azure-cpi csatorna [felhő Foundry Slackhez](https://slack.cloudfoundry.org/)</span><span class="sxs-lookup"><span data-stu-id="b8e20-114">bosh-azure-cpi channel on [Cloud Foundry Slack](https://slack.cloudfoundry.org/)</span></span>
- [<span data-ttu-id="b8e20-115">CF-bosh levelezési lista</span><span class="sxs-lookup"><span data-stu-id="b8e20-115">cf-bosh mailing list</span></span>](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- <span data-ttu-id="b8e20-116">GitHub problémái hello [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) és [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span><span class="sxs-lookup"><span data-stu-id="b8e20-116">GitHub issues for hello [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) and [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span></span>

>[!NOTE]
> <span data-ttu-id="b8e20-117">hello szintű támogatást az Azure-erőforrások, például a felhő Foundry futtató hello virtuális gépek az Azure támogatási foglalt alapul.</span><span class="sxs-lookup"><span data-stu-id="b8e20-117">hello level of support for your Azure resources, such as hello virtual machines where you run Cloud Foundry, is based on your Azure support agreement.</span></span> <span data-ttu-id="b8e20-118">Legjobb közösségi támogatás csak érvényes toohello felhő Foundry vonatkozó összetevőket.</span><span class="sxs-lookup"><span data-stu-id="b8e20-118">Best-effort community support only applies toohello Cloud Foundry-specific components.</span></span>

### <a name="pivotal-cloud-foundry"></a><span data-ttu-id="b8e20-119">Foundry döntő felhő</span><span class="sxs-lookup"><span data-stu-id="b8e20-119">Pivotal Cloud Foundry</span></span>

<span data-ttu-id="b8e20-120">Döntő felhő Foundry hello tartalmaz ugyanazon alapvető platformok hello OSS terjesztési, valamint egyéb felügyeleti eszközök és a nagyvállalati támogatással készlete.</span><span class="sxs-lookup"><span data-stu-id="b8e20-120">Pivotal Cloud Foundry includes hello same core platform as hello OSS distribution, along with a set of proprietary management tools and enterprise support.</span></span> <span data-ttu-id="b8e20-121">az Azure-on PCF toorun, be kell szerezni Pivotal licencet.</span><span class="sxs-lookup"><span data-stu-id="b8e20-121">toorun PCF on Azure, you must acquire a license from Pivotal.</span></span> <span data-ttu-id="b8e20-122">hello PCF ajánlatot hello Azure piactér 90 napos próba licenc is tartozik.</span><span class="sxs-lookup"><span data-stu-id="b8e20-122">hello PCF offer from hello Azure marketplace includes a 90-day trial license.</span></span>

<span data-ttu-id="b8e20-123">hello eszközök közé tartozik [döntő Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), egy webes alkalmazás, amely egyszerűbbé teszi a központi telepítés és a felhő Foundry Foundation felügyeleti és [kulcsfontosságú alkalmazások Manager](https://docs.pivotal.io/pivotalcf/console/), egy webes alkalmazás kezelése felhasználók és az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b8e20-123">hello tools include [Pivotal Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), a web application that simplifies deployment and management of a Cloud Foundry foundation, and [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/), a web application for managing users and applications.</span></span>

<span data-ttu-id="b8e20-124">Ezenkívül toohello támogatási csatornáit OSS CF fent felsorolt, PCF licenc felügyeleti lehetőségeket kínál, toocontact Pivotal támogatásához.</span><span class="sxs-lookup"><span data-stu-id="b8e20-124">In addition toohello support channels listed for OSS CF above, a PCF license entitles you toocontact Pivotal for support.</span></span> <span data-ttu-id="b8e20-125">A Microsoft és Pivotal is engedélyezve van támogatási munkafolyamatok, amelyek lehetővé teszik toocontact bármelyik fél segítségért és a lekérdezés attól függően, hogy hol helyezkedik el hello probléma megfelelően irányított rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b8e20-125">Microsoft and Pivotal have also enabled support workflows that allow you toocontact either party for assistance and have your inquiry routed appropriately depending on where hello issue lies.</span></span>

## <a name="azure-service-broker"></a><span data-ttu-id="b8e20-126">Az Azure Service Broker</span><span class="sxs-lookup"><span data-stu-id="b8e20-126">Azure Service Broker</span></span>

<span data-ttu-id="b8e20-127">Felhő Foundry bátorítja hello ["tizenkét tényezős alkalmazás"](https://12factor.net/) módszertan használatával, amely elősegíti a tiszta választani egymástól a állapot nélküli alkalmazások folyamatokat és az állapotalapú alkalmazások és szolgáltatások biztonsági szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b8e20-127">Cloud Foundry encourages hello ["twelve-factor app"](https://12factor.net/) methodology, which promotes a clean separation of stateless application processes and stateful backing services.</span></span> <span data-ttu-id="b8e20-128">[Szolgáltatás brókerek](https://docs.cloudfoundry.org/services/api.html) egy egységes módon tooprovision kínálnak, és biztonsági szolgáltatások tooapplications kötni.</span><span class="sxs-lookup"><span data-stu-id="b8e20-128">[Service brokers](https://docs.cloudfoundry.org/services/api.html) offer a consistent way tooprovision and bind backing services tooapplications.</span></span> <span data-ttu-id="b8e20-129">Hello [Azure service broker](https://github.com/Azure/meta-azure-service-broker) biztosít néhány hello kulcs Azure-szolgáltatások díjairól ezt a csatornát, beleértve az Azure storage és az Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="b8e20-129">hello [Azure service broker](https://github.com/Azure/meta-azure-service-broker) provides some of hello key Azure services through this channel, including Azure storage and Azure SQL.</span></span>

<span data-ttu-id="b8e20-130">Ha döntő felhő Foundry használ, hello service broker is [csempe érhető el](https://docs.pivotal.io/azure-sb/installing.html) a hello döntő hálózati.</span><span class="sxs-lookup"><span data-stu-id="b8e20-130">If you are using Pivotal Cloud Foundry, hello service broker is also [available as a tile](https://docs.pivotal.io/azure-sb/installing.html) from hello Pivotal Network.</span></span>

## <a name="related-resources"></a><span data-ttu-id="b8e20-131">Kapcsolódó források (lehet, hogy a cikkek angol nyelvűek)</span><span class="sxs-lookup"><span data-stu-id="b8e20-131">Related resources</span></span>

### <a name="visual-studio-team-services-plugin"></a><span data-ttu-id="b8e20-132">Visual Studio Team Services beépülő modul</span><span class="sxs-lookup"><span data-stu-id="b8e20-132">Visual Studio Team Services plugin</span></span>

<span data-ttu-id="b8e20-133">Felhő Foundry kiválóan alkalmas tooagile szoftverfejlesztői, beleértve a folyamatos integrációs (CI) és a folyamatos kézbesítési (CD) hello használatát.</span><span class="sxs-lookup"><span data-stu-id="b8e20-133">Cloud Foundry is well suited tooagile software development, including hello use of continuous integration (CI) and continuous delivery (CD).</span></span> <span data-ttu-id="b8e20-134">Ha használja a Visual Studio Team Services (VSTS) toomanage a projekteket, és szeretné tooset célzó felhőalapú Foundry CI/CD adatcsatorna be, használhatja a hello [VSTS felhő Foundry összeállítási kiterjesztés](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span><span class="sxs-lookup"><span data-stu-id="b8e20-134">If you use Visual Studio Team Services (VSTS) toomanage your projects and would like tooset up a CI/CD pipeline targeting Cloud Foundry, you can use hello [VSTS Cloud Foundry build extension](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span></span> <span data-ttu-id="b8e20-135">hello beépülő modul köszönhetően egyszerű tooconfigure és automatizálhatja a központi telepítések tooCloud Foundry, hogy fut az Azure vagy a másik környezetet.</span><span class="sxs-lookup"><span data-stu-id="b8e20-135">hello plugin makes it simple tooconfigure and automate deployments tooCloud Foundry, whether running in Azure or another environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8e20-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8e20-136">Next steps</span></span>

- [<span data-ttu-id="b8e20-137">Hello Azure Piactérről származó döntő felhő Foundry telepítése</span><span class="sxs-lookup"><span data-stu-id="b8e20-137">Deploy Pivotal Cloud Foundry from hello Azure Marketplace</span></span>](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [<span data-ttu-id="b8e20-138">Központi telepítése egy app tooCloud Foundry az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b8e20-138">Deploy an app tooCloud Foundry in Azure</span></span>](./cloudfoundry-deploy-your-first-app.md)