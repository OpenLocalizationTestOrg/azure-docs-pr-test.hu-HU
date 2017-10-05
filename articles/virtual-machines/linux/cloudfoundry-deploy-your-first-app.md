---
title: "Az első alkalmazás telepítése a Microsoft Azure felhőbe Foundry |} Microsoft Docs"
description: "Azure Cloud Foundry alkalmazás központi telepítése"
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
ms.openlocfilehash: b617127fc0a3f8dcae293e356ea669edcfa5deff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a><span data-ttu-id="0b2a7-103">A Microsoft Azure felhőbe Foundry az első alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0b2a7-103">Deploy your first app to Cloud Foundry on Microsoft Azure</span></span>

<span data-ttu-id="0b2a7-104">[A felhő Foundry](http://cloudfoundry.org) érhető el egy népszerű nyílt forráskódú platform Microsoft Azure-on.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-104">[Cloud Foundry](http://cloudfoundry.org) is a popular open-source application platform available on Microsoft Azure.</span></span> <span data-ttu-id="0b2a7-105">Ebben a cikkben megmutatjuk, hogyan helyezheti üzembe és felügyelheti a felhő Foundry egy alkalmazást az Azure környezetben.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-105">In this article, we show how to deploy and manage an application on Cloud Foundry in an Azure environment.</span></span>

## <a name="create-a-cloud-foundry-environment"></a><span data-ttu-id="0b2a7-106">A felhő Foundry környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b2a7-106">Create a Cloud Foundry environment</span></span>

<span data-ttu-id="0b2a7-107">Nincsenek a felhő Foundry környezet létrehozása az Azure számos lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-107">There are several options for creating a Cloud Foundry environment on Azure:</span></span>

- <span data-ttu-id="0b2a7-108">Használja a [döntő felhő Foundry ajánlat] [ pcf-azuremarketplace] egy szabványos környezet, amely tartalmazza az PCF Ops Manager és az Azure Service Broker létrehozása az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-108">Use the [Pivotal Cloud Foundry offer][pcf-azuremarketplace] in the Azure Marketplace to create a standard environment that includes PCF Ops Manager and the Azure Service Broker.</span></span> <span data-ttu-id="0b2a7-109">Található [utasításokkal] [ pcf-azuremarketplace-pivotaldocs] a piactér üzembe helyezéséhez nyújtanak a döntő dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-109">You can find [complete instructions][pcf-azuremarketplace-pivotaldocs] for deploying the marketplace offer in the Pivotal documentation.</span></span>
- <span data-ttu-id="0b2a7-110">Hozzon létre egy testreszabott környezetet által [döntő felhő Foundry manuális telepítése][pcf-custom].</span><span class="sxs-lookup"><span data-stu-id="0b2a7-110">Create a customized environment by [deploying Pivotal Cloud Foundry manually][pcf-custom].</span></span>
- <span data-ttu-id="0b2a7-111">[Közvetlenül a nyílt forráskódú felhő Foundry csomagok központi telepítése] [ oss-cf-bosh] be kell állítania egy [BOSH](http://bosh.io) igazgató, amely koordinálja a felhő Foundry környezet központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-111">[Deploy the open-source Cloud Foundry packages directly][oss-cf-bosh] by setting up a [BOSH](http://bosh.io) director, a VM that coordinates the deployment of the Cloud Foundry environment.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="0b2a7-112">Ha az Azure piactérről PCF telepíti, jegyezze fel a SYSTEMDOMAINURL és a rendszergazda eléréséhez szükséges hitelesítő adatokat a kulcsfontosságú alkalmazások Manager mindkettőnek a piactér telepítési útmutatóban leírt.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-112">If you are deploying PCF from the Azure Marketplace, make a note of the SYSTEMDOMAINURL and the admin credentials required to access the Pivotal Apps Manager, both of which are described in the marketplace deployment guide.</span></span> <span data-ttu-id="0b2a7-113">Az oktatóanyag elvégzéséhez szükség.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-113">They are needed to complete this tutorial.</span></span> <span data-ttu-id="0b2a7-114">Piactér telepítések esetén a képernyő https://system a SYSTEMDOMAINURL van. *ip-cím*. cf.pcfazure.com.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-114">For marketplace deployments, the SYSTEMDOMAINURL is in the form https://system.*ip-address*.cf.pcfazure.com.</span></span>

## <a name="connect-to-the-cloud-controller"></a><span data-ttu-id="0b2a7-115">A felhő vezérlő kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="0b2a7-115">Connect to the Cloud Controller</span></span>

<span data-ttu-id="0b2a7-116">A felhő tartományvezérlő a felhő Foundry környezet központi telepítéséhez és alkalmazások kezelése az elsődleges belépési pontot.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-116">The Cloud Controller is the primary entry point to a Cloud Foundry environment for deploying and managing applications.</span></span> <span data-ttu-id="0b2a7-117">Az alapvető felhőalapú vezérlő API (CCAPI) a REST API-t, de különböző eszközök keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-117">The core Cloud Controller API (CCAPI) is a REST API, but it is accessible through various tools.</span></span> <span data-ttu-id="0b2a7-118">Ebben az esetben azt interaktívan keresztül a [felhő Foundry CLI][cf-cli].</span><span class="sxs-lookup"><span data-stu-id="0b2a7-118">In this case, we interact with it through the [Cloud Foundry CLI][cf-cli].</span></span> <span data-ttu-id="0b2a7-119">A CLI-t telepítheti Linux, MacOS vagy a Windows, de ha szeretné, hogy nem telepíti azt minden, akkor érhető el az előre telepített a [Azure Cloud rendszerhéj][cloudshell-docs].</span><span class="sxs-lookup"><span data-stu-id="0b2a7-119">You can install the CLI on Linux, MacOS, or Windows, but if you'd prefer not to install it at all, it is available pre-installed in the [Azure Cloud Shell][cloudshell-docs].</span></span>

<span data-ttu-id="0b2a7-120">A bejelentkezéshez, illesztenie `api` a SYSTEMDOMAINURL a piactér telepítésből beszerzett számára.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-120">To log in, prepend `api` to the SYSTEMDOMAINURL that you obtained from the marketplace deployment.</span></span> <span data-ttu-id="0b2a7-121">Mivel az alapértelmezett telepítési egy önaláírt tanúsítványt használ, akkor is tartalmaznia kell a `skip-ssl-validation` váltani.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-121">Since the default deployment uses a self-signed certificate, you should also include the `skip-ssl-validation` switch.</span></span>

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

<span data-ttu-id="0b2a7-122">Jelentkezzen be a felhő vezérlő kéri.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-122">You are prompted to log in to the Cloud Controller.</span></span> <span data-ttu-id="0b2a7-123">A rendszergazdai fiók hitelesítő adatait, amely a piactér üzembe helyezés lépései beszerzett használja.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-123">Use the admin account credentials that you acquired from the marketplace deployment steps.</span></span>

<span data-ttu-id="0b2a7-124">Felhő Foundry biztosít *szervezethez* és *szóközöket* , a csoportok és a megosztott telepítési belül környezetek elkülönítése névtereket.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-124">Cloud Foundry provides *orgs* and *spaces* as namespaces to isolate the teams and environments within a shared deployment.</span></span> <span data-ttu-id="0b2a7-125">A PCF piactér központi telepítést magában foglalja az alapértelmezett *rendszer* szervezeti és a szóközöket a alapkomponensek tartalmazza, például az automatikus skálázást szolgáltatás és az Azure service broker.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-125">The PCF marketplace deployment includes the default *system* org and a set of spaces created to contain the base components, like the autoscaling service and the Azure service broker.</span></span> <span data-ttu-id="0b2a7-126">Most, válassza ki a *rendszer* terület.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-126">For now, choose the *system* space.</span></span>


## <a name="create-an-org-and-space"></a><span data-ttu-id="0b2a7-127">Hozzon létre egy szervezeti és lemezterület</span><span class="sxs-lookup"><span data-stu-id="0b2a7-127">Create an org and space</span></span>

<span data-ttu-id="0b2a7-128">Ha `cf apps`, megjelenik egy adott rendszer alkalmazáscsoportra, a rendszer területen belül. a rendszer szervezeti alkalmazott</span><span class="sxs-lookup"><span data-stu-id="0b2a7-128">If you type `cf apps`, you see a set of system applications that have been deployed in the system space within the system org.</span></span> 

<span data-ttu-id="0b2a7-129">Érdemes megtartani a *rendszer* rendszer alkalmazások számára fenntartott szervezeti így szervezeti és a mintaalkalmazás elhelyezésére terület létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-129">You should keep the *system* org reserved for system applications, so create an org and space to house our sample application.</span></span>

```bash
cf create-org myorg
cf create-space dev -o myorg
```

<span data-ttu-id="0b2a7-130">A cél paranccsal váltson át az új szervezeti és lemezterület:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-130">Use the target command to switch to the new org and space:</span></span>

```bash
cf target -o testorg -s dev
```

<span data-ttu-id="0b2a7-131">Most az alkalmazás központi telepítésekor automatikusan létrejön az új szervezeti és.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-131">Now, when you deploy an application, it is automatically created in the new org and space.</span></span> <span data-ttu-id="0b2a7-132">Győződjön meg arról, hogy jelenleg nincs alkalmazásokat az új szervezeti/tárhely, írja be a következőt `cf apps` újra.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-132">To confirm that there are currently no apps in the new org/space, type `cf apps` again.</span></span>

> [!NOTE] 
> <span data-ttu-id="0b2a7-133">További információ a szervezethez és szóközöket és hogyan használhatók a szerepköralapú hozzáférés-vezérlést (RBAC): a [felhő Foundry dokumentáció][cf-orgs-spaces-docs].</span><span class="sxs-lookup"><span data-stu-id="0b2a7-133">For more information about orgs and spaces and how they can be used for role-based access control (RBAC), see the [Cloud Foundry documentation][cf-orgs-spaces-docs].</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="0b2a7-134">Alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0b2a7-134">Deploy an application</span></span>

<span data-ttu-id="0b2a7-135">Most használja a felhő Foundry mintaalkalmazás nevű Hello rugó felhő, amelyet a rendszer a Java nyelven írt alapján a [rugó keretrendszer](http://spring.io) és [rugó rendszerindító](http://projects.spring.io/spring-boot/).</span><span class="sxs-lookup"><span data-stu-id="0b2a7-135">Let's use a sample Cloud Foundry application called Hello Spring Cloud, which is written in Java and based on the [Spring Framework](http://spring.io) and [Spring Boot](http://projects.spring.io/spring-boot/).</span></span>

### <a name="clone-the-hello-spring-cloud-repository"></a><span data-ttu-id="0b2a7-136">A Hello rugó felhő tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="0b2a7-136">Clone the Hello Spring Cloud repository</span></span>

<span data-ttu-id="0b2a7-137">A Hello rugó felhő mintaalkalmazást a Githubon érhető el.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-137">The Hello Spring Cloud sample application is available on GitHub.</span></span> <span data-ttu-id="0b2a7-138">Klónozza a környezetben, és váltson át az új könyvtár:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-138">Clone it to your environment and change into the new directory:</span></span>

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a><span data-ttu-id="0b2a7-139">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b2a7-139">Build the application</span></span>

<span data-ttu-id="0b2a7-140">Build, az alkalmazás használatával [Apache Maven](http://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="0b2a7-140">Build the app using [Apache Maven](http://maven.apache.org).</span></span>

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a><span data-ttu-id="0b2a7-141">Az alkalmazás cf leküldéses telepítését</span><span class="sxs-lookup"><span data-stu-id="0b2a7-141">Deploy the application with cf push</span></span>

<span data-ttu-id="0b2a7-142">A legtöbb alkalmazásokat felhő Foundry használatával helyezhet üzembe a `push` parancs:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-142">You can deploy most applications to Cloud Foundry using the `push` command:</span></span>

```bash
cf push
```

<span data-ttu-id="0b2a7-143">Ha Ön *leküldéses* egy alkalmazást, a felhő Foundry (ebben az esetben egy alkalmazásban Java) alkalmazás észleli, és annak függőségeit (ebben az esetben az rugó keretében) azonosítja.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-143">When you *push* an application, Cloud Foundry detects the type of application (in this case, a Java app) and identifies its dependencies (in this case, the Spring framework).</span></span> <span data-ttu-id="0b2a7-144">Majd csomagok mindent lemezképpel egy önálló tároló, úgynevezett Ön kódjának futtatásához szükséges egy *feldolgozó*.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-144">It then packages everything required to run your code into a standalone container image, known as a *droplet*.</span></span> <span data-ttu-id="0b2a7-145">Végül felhő Foundry ütemezi az alkalmazás egy, a rendelkezésre álló környezetében levő készülékek a, és létrehoz egy URL-címet, ahol érhető el, a parancs kimenetében elérhető.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-145">Finally, Cloud Foundry schedules the application on one of the available machines in your environment and creates a URL where you can reach it, which is available in the output of the command.</span></span>

![Cf leküldéses parancs kimenete][cf-push-output]

<span data-ttu-id="0b2a7-147">A rugó-felhő hello alkalmazás megtekintéséhez nyissa meg a böngészőben a megadott URL-cím:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-147">To see the hello-spring-cloud application, open the provided URL in your browser:</span></span>

![Alapértelmezett felhasználói felület Hello rugó felhő][hello-spring-cloud-basic]

> [!NOTE] 
> <span data-ttu-id="0b2a7-149">További információt, hogy mi történik a során `cf push`, lásd: [hogyan vannak előkészített alkalmazások] [ cf-push-docs] a felhő Foundry dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-149">To learn more about what happens during `cf push`, see [How Applications Are Staged][cf-push-docs] in the Cloud Foundry documentation.</span></span>

## <a name="view-application-logs"></a><span data-ttu-id="0b2a7-150">Alkalmazás-naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="0b2a7-150">View application logs</span></span>

<span data-ttu-id="0b2a7-151">A felhő Foundry CLI segítségével megtekintheti a naplók az alkalmazás neve:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-151">You can use the Cloud Foundry CLI to view logs for an application by its name:</span></span>

```bash
cf logs hello-spring-cloud
```

<span data-ttu-id="0b2a7-152">Alapértelmezés szerint a naplók parancs által használt *utóhívás*, amely jeleníti meg az új naplók az oktatóprogram.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-152">By default, the logs command uses *tail*, which shows new logs as they are written.</span></span> <span data-ttu-id="0b2a7-153">Megjelenik az új naplók megtekintéséhez frissítse a hello felhőalapú rugó alkalmazást a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-153">To see new logs appear, refresh the hello-spring-cloud app in the browser.</span></span>

<span data-ttu-id="0b2a7-154">Naplók írt már megtekintéséhez adja hozzá a `recent` váltani:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-154">To view logs that have already been written, add the `recent` switch:</span></span>

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a><span data-ttu-id="0b2a7-155">Az alkalmazás skálázása</span><span class="sxs-lookup"><span data-stu-id="0b2a7-155">Scale the application</span></span>

<span data-ttu-id="0b2a7-156">Alapértelmezés szerint `cf push` csak hoz létre az alkalmazás egyetlen példányát.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-156">By default, `cf push` only creates a single instance of your application.</span></span> <span data-ttu-id="0b2a7-157">Magas rendelkezésre állásának biztosításához, és nagyobb átviteli sebesség eléréséhez kibővítési engedélyezéséhez, általában futtatni kívánt alkalmazás több példánya.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-157">To ensure high availability and enable scale out for higher throughput, you generally want to run more than one instance of your applications.</span></span> <span data-ttu-id="0b2a7-158">Már telepített alkalmazások segítségével könnyen lehet horizontálisan a `scale` parancs:</span><span class="sxs-lookup"><span data-stu-id="0b2a7-158">You can easily scale out already deployed applications using the `scale` command:</span></span>

```bash
cf scale -i 2 hello-spring-cloud
```

<span data-ttu-id="0b2a7-159">Fut a `cf app` az alkalmazás a parancs megjeleníti, hogy felhőalapú Foundry hoz létre az alkalmazás egy másik példánya.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-159">Running the `cf app` command on the application shows that Cloud Foundry is creating another instance of the application.</span></span> <span data-ttu-id="0b2a7-160">Után az alkalmazás elindult, a felhő Foundry terheléselosztási hozzá forgalom automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="0b2a7-160">Once the application has started, Cloud Foundry automatically starts load balancing traffic to it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0b2a7-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b2a7-161">Next steps</span></span>

- <span data-ttu-id="0b2a7-162">[Olvassa el a felhő Foundry dokumentációt][cloudfoundry-docs]</span><span class="sxs-lookup"><span data-stu-id="0b2a7-162">[Read the Cloud Foundry documentation][cloudfoundry-docs]</span></span>
- <span data-ttu-id="0b2a7-163">[Felhő Foundry a Visual Studio Team Services beépülő modul beállítása][vsts-plugin]</span><span class="sxs-lookup"><span data-stu-id="0b2a7-163">[Set up the Visual Studio Team Services plugin for Cloud Foundry][vsts-plugin]</span></span>
- <span data-ttu-id="0b2a7-164">[A Microsoft napló Analytics dugulásellenőrzési felhő Foundry konfigurálása][loganalytics-nozzle]</span><span class="sxs-lookup"><span data-stu-id="0b2a7-164">[Configure the Microsoft Log Analytics Nozzle for Cloud Foundry][loganalytics-nozzle]</span></span>

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