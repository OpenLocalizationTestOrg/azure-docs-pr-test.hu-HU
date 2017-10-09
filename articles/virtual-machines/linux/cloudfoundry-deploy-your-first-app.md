---
title: "aaaDeploy az első alkalmazás tooCloud Foundry a Microsoft Azure |} Microsoft Docs"
description: "Egy alkalmazás tooCloud Foundry Azure telepítéséhez"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 878da38f6eabe32a339f02aa0ead811d6e5af9a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-first-app-toocloud-foundry-on-microsoft-azure"></a><span data-ttu-id="0b67c-103">Központi telepítése az első alkalmazás tooCloud Foundry a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="0b67c-103">Deploy your first app tooCloud Foundry on Microsoft Azure</span></span>

<span data-ttu-id="0b67c-104">[A felhő Foundry](http://cloudfoundry.org) érhető el egy népszerű nyílt forráskódú platform Microsoft Azure-on.</span><span class="sxs-lookup"><span data-stu-id="0b67c-104">[Cloud Foundry](http://cloudfoundry.org) is a popular open-source application platform available on Microsoft Azure.</span></span> <span data-ttu-id="0b67c-105">Ebben a cikkben megmutatjuk, hogyan toodeploy és kezelheti egy alkalmazás felhő Foundry Azure környezetben.</span><span class="sxs-lookup"><span data-stu-id="0b67c-105">In this article, we show how toodeploy and manage an application on Cloud Foundry in an Azure environment.</span></span>

## <a name="create-a-cloud-foundry-environment"></a><span data-ttu-id="0b67c-106">A felhő Foundry környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b67c-106">Create a Cloud Foundry environment</span></span>

<span data-ttu-id="0b67c-107">Nincsenek a felhő Foundry környezet létrehozása az Azure számos lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="0b67c-107">There are several options for creating a Cloud Foundry environment on Azure:</span></span>

- <span data-ttu-id="0b67c-108">Használjon hello [döntő felhő Foundry ajánlat] [ pcf-azuremarketplace] a hello Azure piactér toocreate egy szabványos környezetben, amely tartalmazza az PCF Ops Manager és hello Azure Service Broker.</span><span class="sxs-lookup"><span data-stu-id="0b67c-108">Use hello [Pivotal Cloud Foundry offer][pcf-azuremarketplace] in hello Azure Marketplace toocreate a standard environment that includes PCF Ops Manager and hello Azure Service Broker.</span></span> <span data-ttu-id="0b67c-109">Található [utasításokkal] [ pcf-azuremarketplace-pivotaldocs] hello piactér telepítéséhez kínálnak a hello döntő dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="0b67c-109">You can find [complete instructions][pcf-azuremarketplace-pivotaldocs] for deploying hello marketplace offer in hello Pivotal documentation.</span></span>
- <span data-ttu-id="0b67c-110">Hozzon létre egy testreszabott környezetet által [döntő felhő Foundry manuális telepítése][pcf-custom].</span><span class="sxs-lookup"><span data-stu-id="0b67c-110">Create a customized environment by [deploying Pivotal Cloud Foundry manually][pcf-custom].</span></span>
- <span data-ttu-id="0b67c-111">[Közvetlenül hello nyílt forráskódú felhő Foundry csomagok központi telepítése] [ oss-cf-bosh] be kell állítania egy [BOSH](http://bosh.io) igazgató, a virtuális gépek hello telepítési hello felhő Foundry környezet koordinálására.</span><span class="sxs-lookup"><span data-stu-id="0b67c-111">[Deploy hello open-source Cloud Foundry packages directly][oss-cf-bosh] by setting up a [BOSH](http://bosh.io) director, a VM that coordinates hello deployment of hello Cloud Foundry environment.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="0b67c-112">Ha PCF hello Azure Piactérről származó telepít, jegyezze fel a hello SYSTEMDOMAINURL, és hello rendszergazdai hitelesítő adataival szükséges tooaccess hello kulcsfontosságú alkalmazások kezelő mindkettőnek hello piactér telepítési útmutatóban leírt.</span><span class="sxs-lookup"><span data-stu-id="0b67c-112">If you are deploying PCF from hello Azure Marketplace, make a note of hello SYSTEMDOMAINURL and hello admin credentials required tooaccess hello Pivotal Apps Manager, both of which are described in hello marketplace deployment guide.</span></span> <span data-ttu-id="0b67c-113">Akkor szükséges toocomplete ebben az oktatóanyagban vannak.</span><span class="sxs-lookup"><span data-stu-id="0b67c-113">They are needed toocomplete this tutorial.</span></span> <span data-ttu-id="0b67c-114">Piactér-telepítések esetén hello SYSTEMDOMAINURL hello űrlap https://system van. *ip-cím*. cf.pcfazure.com.</span><span class="sxs-lookup"><span data-stu-id="0b67c-114">For marketplace deployments, hello SYSTEMDOMAINURL is in hello form https://system.*ip-address*.cf.pcfazure.com.</span></span>

## <a name="connect-toohello-cloud-controller"></a><span data-ttu-id="0b67c-115">Csatlakozás toohello felhő vezérlő</span><span class="sxs-lookup"><span data-stu-id="0b67c-115">Connect toohello Cloud Controller</span></span>

<span data-ttu-id="0b67c-116">hello felhő vezérlő hello elsődleges belépési pont tooa felhő Foundry környezet központi telepítésére és alkalmazások felügyeletére.</span><span class="sxs-lookup"><span data-stu-id="0b67c-116">hello Cloud Controller is hello primary entry point tooa Cloud Foundry environment for deploying and managing applications.</span></span> <span data-ttu-id="0b67c-117">hello core felhő vezérlő API (CCAPI) a REST API-t, de különböző eszközök keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0b67c-117">hello core Cloud Controller API (CCAPI) is a REST API, but it is accessible through various tools.</span></span> <span data-ttu-id="0b67c-118">Ebben az esetben azt vele keresztül kapcsolatba hello [felhő Foundry CLI][cf-cli].</span><span class="sxs-lookup"><span data-stu-id="0b67c-118">In this case, we interact with it through hello [Cloud Foundry CLI][cf-cli].</span></span> <span data-ttu-id="0b67c-119">Hello CLI telepítheti Linux, a MacOS vagy a Windows, de ha nem, akkor érhető el hello előre telepített tooinstall inkább [Azure Cloud rendszerhéj][cloudshell-docs].</span><span class="sxs-lookup"><span data-stu-id="0b67c-119">You can install hello CLI on Linux, MacOS, or Windows, but if you'd prefer not tooinstall it at all, it is available pre-installed in hello [Azure Cloud Shell][cloudshell-docs].</span></span>

<span data-ttu-id="0b67c-120">a, toolog illesztenie `api` toohello SYSTEMDOMAINURL beolvasott hello piactér telepítéséből.</span><span class="sxs-lookup"><span data-stu-id="0b67c-120">toolog in, prepend `api` toohello SYSTEMDOMAINURL that you obtained from hello marketplace deployment.</span></span> <span data-ttu-id="0b67c-121">Mivel hello alapértelmezett telepítési egy önaláírt tanúsítványt használ, akkor ki kell terjednie hello `skip-ssl-validation` váltani.</span><span class="sxs-lookup"><span data-stu-id="0b67c-121">Since hello default deployment uses a self-signed certificate, you should also include hello `skip-ssl-validation` switch.</span></span>

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

<span data-ttu-id="0b67c-122">A felhő vezérlő toohello rákérdezéses toolog áll.</span><span class="sxs-lookup"><span data-stu-id="0b67c-122">You are prompted toolog in toohello Cloud Controller.</span></span> <span data-ttu-id="0b67c-123">Hello rendszergazdai fiók hitelesítő adatait, amely hello piactér üzembe helyezés lépései beszerzett használja.</span><span class="sxs-lookup"><span data-stu-id="0b67c-123">Use hello admin account credentials that you acquired from hello marketplace deployment steps.</span></span>

<span data-ttu-id="0b67c-124">Felhő Foundry biztosít *szervezethez* és *szóközöket* névterek tooisolate hello csoportok és belül egy megosztott telepítési környezetekben.</span><span class="sxs-lookup"><span data-stu-id="0b67c-124">Cloud Foundry provides *orgs* and *spaces* as namespaces tooisolate hello teams and environments within a shared deployment.</span></span> <span data-ttu-id="0b67c-125">hello PCF piactér telepítési tartalmaz hello alapértelmezett *rendszer* szervezeti és a tárolóhelyek létrehozott toocontain hello alapkomponensek, például a hello automatikus skálázás szolgáltatás és hello Azure service broker.</span><span class="sxs-lookup"><span data-stu-id="0b67c-125">hello PCF marketplace deployment includes hello default *system* org and a set of spaces created toocontain hello base components, like hello autoscaling service and hello Azure service broker.</span></span> <span data-ttu-id="0b67c-126">Most, válassza ki a hello *rendszer* terület.</span><span class="sxs-lookup"><span data-stu-id="0b67c-126">For now, choose hello *system* space.</span></span>


## <a name="create-an-org-and-space"></a><span data-ttu-id="0b67c-127">Hozzon létre egy szervezeti és lemezterület</span><span class="sxs-lookup"><span data-stu-id="0b67c-127">Create an org and space</span></span>

<span data-ttu-id="0b67c-128">Ha `cf apps`, megjelenik egy adott rendszer alkalmazáscsoportra hello rendszer területen belül hello system. szervezeti alkalmazott</span><span class="sxs-lookup"><span data-stu-id="0b67c-128">If you type `cf apps`, you see a set of system applications that have been deployed in hello system space within hello system org.</span></span> 

<span data-ttu-id="0b67c-129">Hello kell tartania *rendszer* rendszer alkalmazások számára fenntartott szervezeti, hozzon létre egy szervezeti és a hely toohouse a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0b67c-129">You should keep hello *system* org reserved for system applications, so create an org and space toohouse our sample application.</span></span>

```bash
cf create-org myorg
cf create-space dev -o myorg
```

<span data-ttu-id="0b67c-130">Hello cél parancs tooswitch toohello új szervezeti és helyet használja:</span><span class="sxs-lookup"><span data-stu-id="0b67c-130">Use hello target command tooswitch toohello new org and space:</span></span>

```bash
cf target -o testorg -s dev
```

<span data-ttu-id="0b67c-131">Most az alkalmazás központi telepítésekor automatikusan létrejön hello új szervezeti és.</span><span class="sxs-lookup"><span data-stu-id="0b67c-131">Now, when you deploy an application, it is automatically created in hello new org and space.</span></span> <span data-ttu-id="0b67c-132">amely jelenleg tooconfirm hello új szervezeti/terület, az alkalmazás nem írja be a `cf apps` újra.</span><span class="sxs-lookup"><span data-stu-id="0b67c-132">tooconfirm that there are currently no apps in hello new org/space, type `cf apps` again.</span></span>

> [!NOTE] 
> <span data-ttu-id="0b67c-133">További információ a szervezethez és szóközöket és hogyan használhatók a szerepköralapú hozzáférés-vezérlést (RBAC): hello [felhő Foundry dokumentáció][cf-orgs-spaces-docs].</span><span class="sxs-lookup"><span data-stu-id="0b67c-133">For more information about orgs and spaces and how they can be used for role-based access control (RBAC), see hello [Cloud Foundry documentation][cf-orgs-spaces-docs].</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="0b67c-134">Alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0b67c-134">Deploy an application</span></span>

<span data-ttu-id="0b67c-135">Most használja a felhő Foundry mintaalkalmazás Hello rugó felhő, amelyet a rendszer a Java nyelven írt hello alapján nevű [rugó keretrendszer](http://spring.io) és [rugó rendszerindító](http://projects.spring.io/spring-boot/).</span><span class="sxs-lookup"><span data-stu-id="0b67c-135">Let's use a sample Cloud Foundry application called Hello Spring Cloud, which is written in Java and based on hello [Spring Framework](http://spring.io) and [Spring Boot](http://projects.spring.io/spring-boot/).</span></span>

### <a name="clone-hello-hello-spring-cloud-repository"></a><span data-ttu-id="0b67c-136">Hello Hello rugó felhő tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="0b67c-136">Clone hello Hello Spring Cloud repository</span></span>

<span data-ttu-id="0b67c-137">hello Hello rugó felhő mintaalkalmazást a Githubon érhető el.</span><span class="sxs-lookup"><span data-stu-id="0b67c-137">hello Hello Spring Cloud sample application is available on GitHub.</span></span> <span data-ttu-id="0b67c-138">Klónozza tooyour környezetben, és hello új könyvtárba módosítása:</span><span class="sxs-lookup"><span data-stu-id="0b67c-138">Clone it tooyour environment and change into hello new directory:</span></span>

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-hello-application"></a><span data-ttu-id="0b67c-139">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b67c-139">Build hello application</span></span>

<span data-ttu-id="0b67c-140">Build hello alkalmazás használatával [Apache Maven](http://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="0b67c-140">Build hello app using [Apache Maven](http://maven.apache.org).</span></span>

```bash
mvn clean package
```

### <a name="deploy-hello-application-with-cf-push"></a><span data-ttu-id="0b67c-141">Hello alkalmazás cf leküldéses telepítését</span><span class="sxs-lookup"><span data-stu-id="0b67c-141">Deploy hello application with cf push</span></span>

<span data-ttu-id="0b67c-142">A legtöbb alkalmazások tooCloud Foundry telepítése hello segítségével `push` parancs:</span><span class="sxs-lookup"><span data-stu-id="0b67c-142">You can deploy most applications tooCloud Foundry using hello `push` command:</span></span>

```bash
cf push
```

<span data-ttu-id="0b67c-143">Ha Ön *leküldéses* egy alkalmazást, a felhő Foundry hello alkalmazástípus (ebben az esetben egy alkalmazásban Java) észlel, és azonosítja a függőséget (Ez esetben hello rugó keretrendszer).</span><span class="sxs-lookup"><span data-stu-id="0b67c-143">When you *push* an application, Cloud Foundry detects hello type of application (in this case, a Java app) and identifies its dependencies (in this case, hello Spring framework).</span></span> <span data-ttu-id="0b67c-144">Majd csomagok mindent lemezképpel egy önálló tároló, ez a kód toorun szükséges egy *feldolgozó*.</span><span class="sxs-lookup"><span data-stu-id="0b67c-144">It then packages everything required toorun your code into a standalone container image, known as a *droplet*.</span></span> <span data-ttu-id="0b67c-145">Végül felhő Foundry ütemezések hello hello rendelkezésre gépek környezetében egyik alkalmazás, és létrehoz egy URL-címet, ahol érhető el, elérhető a hello hello parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="0b67c-145">Finally, Cloud Foundry schedules hello application on one of hello available machines in your environment and creates a URL where you can reach it, which is available in hello output of hello command.</span></span>

![Cf leküldéses parancs kimenete][cf-push-output]

<span data-ttu-id="0b67c-147">toosee hello hello felhőalapú rugó alkalmazás, a böngészőben nyissa meg a megadott hello URL-címe:</span><span class="sxs-lookup"><span data-stu-id="0b67c-147">toosee hello hello-spring-cloud application, open hello provided URL in your browser:</span></span>

![Alapértelmezett felhasználói felület Hello rugó felhő][hello-spring-cloud-basic]

> [!NOTE] 
> <span data-ttu-id="0b67c-149">További következményeiről során toolearn `cf push`, lásd: [hogyan vannak előkészített alkalmazások] [ cf-push-docs] hello felhő Foundry dokumentációjában található.</span><span class="sxs-lookup"><span data-stu-id="0b67c-149">toolearn more about what happens during `cf push`, see [How Applications Are Staged][cf-push-docs] in hello Cloud Foundry documentation.</span></span>

## <a name="view-application-logs"></a><span data-ttu-id="0b67c-150">Alkalmazás-naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="0b67c-150">View application logs</span></span>

<span data-ttu-id="0b67c-151">Hello felhő Foundry CLI tooview naplók az alkalmazás használatához a neve:</span><span class="sxs-lookup"><span data-stu-id="0b67c-151">You can use hello Cloud Foundry CLI tooview logs for an application by its name:</span></span>

```bash
cf logs hello-spring-cloud
```

<span data-ttu-id="0b67c-152">Alapértelmezés szerint a hello naplózza a parancs által használt *utóhívás*, amely jeleníti meg az új naplók az oktatóprogram.</span><span class="sxs-lookup"><span data-stu-id="0b67c-152">By default, hello logs command uses *tail*, which shows new logs as they are written.</span></span> <span data-ttu-id="0b67c-153">Új naplók toosee jelenik meg, frissítse a hello hello-rugó-felhőalkalmazás hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="0b67c-153">toosee new logs appear, refresh hello hello-spring-cloud app in hello browser.</span></span>

<span data-ttu-id="0b67c-154">tooview naplók írt már, vegye fel a hello `recent` váltani:</span><span class="sxs-lookup"><span data-stu-id="0b67c-154">tooview logs that have already been written, add hello `recent` switch:</span></span>

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-hello-application"></a><span data-ttu-id="0b67c-155">Skála hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="0b67c-155">Scale hello application</span></span>

<span data-ttu-id="0b67c-156">Alapértelmezés szerint `cf push` csak hoz létre az alkalmazás egyetlen példányát.</span><span class="sxs-lookup"><span data-stu-id="0b67c-156">By default, `cf push` only creates a single instance of your application.</span></span> <span data-ttu-id="0b67c-157">tooensure magas rendelkezésre állású és nagyobb átviteli sebesség eléréséhez kibővítési engedélyezése, általában a kívánt toorun több, mint az alkalmazások egy példánya.</span><span class="sxs-lookup"><span data-stu-id="0b67c-157">tooensure high availability and enable scale out for higher throughput, you generally want toorun more than one instance of your applications.</span></span> <span data-ttu-id="0b67c-158">Már telepített alkalmazások hello segítségével könnyen lehet horizontálisan `scale` parancs:</span><span class="sxs-lookup"><span data-stu-id="0b67c-158">You can easily scale out already deployed applications using hello `scale` command:</span></span>

```bash
cf scale -i 2 hello-spring-cloud
```

<span data-ttu-id="0b67c-159">Futó hello `cf app` hello alkalmazás a parancs megjeleníti, hogy a felhő Foundry hello alkalmazás egy másik példánya hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0b67c-159">Running hello `cf app` command on hello application shows that Cloud Foundry is creating another instance of hello application.</span></span> <span data-ttu-id="0b67c-160">Hello alkalmazás elindult, miután a felhő Foundry terheléselosztási forgalom tooit automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="0b67c-160">Once hello application has started, Cloud Foundry automatically starts load balancing traffic tooit.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0b67c-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b67c-161">Next steps</span></span>

- <span data-ttu-id="0b67c-162">[Olvasási hello felhő Foundry dokumentáció][cloudfoundry-docs]</span><span class="sxs-lookup"><span data-stu-id="0b67c-162">[Read hello Cloud Foundry documentation][cloudfoundry-docs]</span></span>
- <span data-ttu-id="0b67c-163">[Felhő Foundry hello Visual Studio Team Services beépülő modul beállítása][vsts-plugin]</span><span class="sxs-lookup"><span data-stu-id="0b67c-163">[Set up hello Visual Studio Team Services plugin for Cloud Foundry][vsts-plugin]</span></span>
- <span data-ttu-id="0b67c-164">[Felhő Foundry Microsoft napló Analytics dugulásellenőrzési hello konfigurálása][loganalytics-nozzle]</span><span class="sxs-lookup"><span data-stu-id="0b67c-164">[Configure hello Microsoft Log Analytics Nozzle for Cloud Foundry][loganalytics-nozzle]</span></span>

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: http://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png