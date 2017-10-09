---
title: "Webalkalmazás Linux aaaIntroduction tooAzure |} Microsoft Docs"
description: "További információk a Linux Azure-webalkalmazásban."
keywords: az Azure app service, linux, oss
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 43b9865ade251909a77429eb3e18fe0bcaac3bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-web-app-on-linux"></a><span data-ttu-id="be938-104">Bevezetés tooAzure webalkalmazás Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="be938-104">Introduction tooAzure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="be938-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="be938-105">Overview</span></span>
<span data-ttu-id="be938-106">Az ügyfelek használhatják a webalkalmazás Linux toohost webalkalmazásokban Linux rendszeren natív módon a támogatott alkalmazás verem.</span><span class="sxs-lookup"><span data-stu-id="be938-106">Customers can use Web App on Linux toohost web apps natively on Linux for supported application stacks.</span></span> <span data-ttu-id="be938-107">hello következő szakasz hello alkalmazás verem jelenleg támogatott.</span><span class="sxs-lookup"><span data-stu-id="be938-107">hello following section lists hello application stacks that are currently supported.</span></span> 

## <a name="features"></a><span data-ttu-id="be938-108">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="be938-108">Features</span></span>
<span data-ttu-id="be938-109">Webes alkalmazás Linux rendszeren jelenleg a következő alkalmazás verem hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="be938-109">Web App on Linux currently supports hello following application stacks:</span></span>

* <span data-ttu-id="be938-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="be938-110">Node.js</span></span>
    * <span data-ttu-id="be938-111">4.4</span><span class="sxs-lookup"><span data-stu-id="be938-111">4.4</span></span>
    * <span data-ttu-id="be938-112">4.5</span><span class="sxs-lookup"><span data-stu-id="be938-112">4.5</span></span>
    * <span data-ttu-id="be938-113">6.2</span><span class="sxs-lookup"><span data-stu-id="be938-113">6.2</span></span>
    * <span data-ttu-id="be938-114">6.6</span><span class="sxs-lookup"><span data-stu-id="be938-114">6.6</span></span>
    * <span data-ttu-id="be938-115">6.9</span><span class="sxs-lookup"><span data-stu-id="be938-115">6.9</span></span>
    * <span data-ttu-id="be938-116">6.10</span><span class="sxs-lookup"><span data-stu-id="be938-116">6.10</span></span>
    * <span data-ttu-id="be938-117">6.11</span><span class="sxs-lookup"><span data-stu-id="be938-117">6.11</span></span>
    * <span data-ttu-id="be938-118">8.0</span><span class="sxs-lookup"><span data-stu-id="be938-118">8.0</span></span>
    * <span data-ttu-id="be938-119">8.1</span><span class="sxs-lookup"><span data-stu-id="be938-119">8.1</span></span>
* <span data-ttu-id="be938-120">PHP</span><span class="sxs-lookup"><span data-stu-id="be938-120">PHP</span></span>
    * <span data-ttu-id="be938-121">5.6</span><span class="sxs-lookup"><span data-stu-id="be938-121">5.6</span></span>
    * <span data-ttu-id="be938-122">7.0</span><span class="sxs-lookup"><span data-stu-id="be938-122">7.0</span></span>
* <span data-ttu-id="be938-123">A .NET core</span><span class="sxs-lookup"><span data-stu-id="be938-123">.Net Core</span></span>
    * <span data-ttu-id="be938-124">1.0</span><span class="sxs-lookup"><span data-stu-id="be938-124">1.0</span></span>
    * <span data-ttu-id="be938-125">1.1</span><span class="sxs-lookup"><span data-stu-id="be938-125">1.1</span></span>
* <span data-ttu-id="be938-126">Ruby</span><span class="sxs-lookup"><span data-stu-id="be938-126">Ruby</span></span>
    * <span data-ttu-id="be938-127">2.3</span><span class="sxs-lookup"><span data-stu-id="be938-127">2.3</span></span>

<span data-ttu-id="be938-128">Központi telepítését az alkalmazások használatával:</span><span class="sxs-lookup"><span data-stu-id="be938-128">Customers can deploy their applications by using:</span></span>

* <span data-ttu-id="be938-129">FTP</span><span class="sxs-lookup"><span data-stu-id="be938-129">FTP</span></span>
* <span data-ttu-id="be938-130">Helyi Git</span><span class="sxs-lookup"><span data-stu-id="be938-130">Local Git</span></span>
* <span data-ttu-id="be938-131">GitHub</span><span class="sxs-lookup"><span data-stu-id="be938-131">GitHub</span></span>
* <span data-ttu-id="be938-132">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="be938-132">Bitbucket</span></span>

<span data-ttu-id="be938-133">Az alkalmazás méretezés:</span><span class="sxs-lookup"><span data-stu-id="be938-133">For application scaling:</span></span>

* <span data-ttu-id="be938-134">Az ügyfelek is méretezhető webalkalmazások felfelé és lefelé az App Service-csomag hello szintjének módosítása</span><span class="sxs-lookup"><span data-stu-id="be938-134">Customers can scale web apps up and down by changing hello tier of their App Service plan</span></span>
* <span data-ttu-id="be938-135">Ügyfelek horizontális felskálázás alkalmazások és azok metódust hello határain belül több alkalmazás példányának futtatása</span><span class="sxs-lookup"><span data-stu-id="be938-135">Customers can scale out applications and run multiple app instances within hello confines of their SKU</span></span>

<span data-ttu-id="be938-136">A Kudu, néhány alapvető hello-funkciókat:</span><span class="sxs-lookup"><span data-stu-id="be938-136">For Kudu, some of hello basic functionality:</span></span>

* <span data-ttu-id="be938-137">Környezetekben</span><span class="sxs-lookup"><span data-stu-id="be938-137">Environments</span></span>
* <span data-ttu-id="be938-138">Központi telepítés</span><span class="sxs-lookup"><span data-stu-id="be938-138">Deployments</span></span>
* <span data-ttu-id="be938-139">Alapszintű konzol</span><span class="sxs-lookup"><span data-stu-id="be938-139">Basic console</span></span>
* <span data-ttu-id="be938-140">SSH</span><span class="sxs-lookup"><span data-stu-id="be938-140">SSH</span></span>

<span data-ttu-id="be938-141">A devops:</span><span class="sxs-lookup"><span data-stu-id="be938-141">For devops:</span></span>

* <span data-ttu-id="be938-142">Átmeneti környezetek</span><span class="sxs-lookup"><span data-stu-id="be938-142">Staging environments</span></span>
* <span data-ttu-id="be938-143">ACR és DockerHub CI/CD</span><span class="sxs-lookup"><span data-stu-id="be938-143">ACR and DockerHub CI/CD</span></span>

## <a name="limitations"></a><span data-ttu-id="be938-144">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="be938-144">Limitations</span></span>
<span data-ttu-id="be938-145">hello Azure-portálon megjelenítése csak olyan funkciókat, amelyek jelenleg webalkalmazás Linux rendszeren működik és elrejtése hello rest.</span><span class="sxs-lookup"><span data-stu-id="be938-145">hello Azure portal shows only features that currently work for Web App on Linux and hides hello rest.</span></span> <span data-ttu-id="be938-146">Engedélyezzük a szolgáltatásokat, mivel ezek láthatók lesznek a hello portál.</span><span class="sxs-lookup"><span data-stu-id="be938-146">As we enable more features, they will be visible on hello portal.</span></span>

<span data-ttu-id="be938-147">Egyes szolgáltatások, például a virtuális hálózati integráció, az Azure Active Directory vagy harmadik fél hitelesítést és a Kudu helyhez kiterjesztések, még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="be938-147">Some features, such as virtual network integration, Azure Active Directory/third-party authentication, or Kudu site extensions, are not available yet.</span></span> <span data-ttu-id="be938-148">Ezek a funkciók érhetők el, ha a dokumentáció és hello változásaira vonatkozó blog frissítjük.</span><span class="sxs-lookup"><span data-stu-id="be938-148">Once these features are available, we will update our documentation and blog about hello changes.</span></span>

<span data-ttu-id="be938-149">A nyilvános előzetes verzió jelenleg csak a következő régiókban hello érhető el:</span><span class="sxs-lookup"><span data-stu-id="be938-149">This public preview is currently only available in hello following regions:</span></span>

* <span data-ttu-id="be938-150">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="be938-150">West US</span></span>
* <span data-ttu-id="be938-151">USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="be938-151">East US</span></span>
* <span data-ttu-id="be938-152">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="be938-152">West Europe</span></span>
* <span data-ttu-id="be938-153">Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="be938-153">North Europe</span></span>
* <span data-ttu-id="be938-154">USA déli középső régiója</span><span class="sxs-lookup"><span data-stu-id="be938-154">South Central US</span></span>
* <span data-ttu-id="be938-155">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="be938-155">North Central US</span></span>
* <span data-ttu-id="be938-156">Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="be938-156">Southeast Asia</span></span>
* <span data-ttu-id="be938-157">Kelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="be938-157">East Asia</span></span>
* <span data-ttu-id="be938-158">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="be938-158">Australia East</span></span>
* <span data-ttu-id="be938-159">Kelet-Japán</span><span class="sxs-lookup"><span data-stu-id="be938-159">Japan East</span></span>
* <span data-ttu-id="be938-160">Dél-Brazília</span><span class="sxs-lookup"><span data-stu-id="be938-160">Brazil South</span></span>
* <span data-ttu-id="be938-161">Dél-India</span><span class="sxs-lookup"><span data-stu-id="be938-161">South India</span></span>

<span data-ttu-id="be938-162">Web Apps Linux csak akkor támogatott a hello dedikált app service-csomagok, és nem rendelkezik egy ingyenes vagy közös réteg.</span><span class="sxs-lookup"><span data-stu-id="be938-162">Web Apps on Linux is only supported in hello Dedicated app service plans and does not have a Free or Shared tier.</span></span> <span data-ttu-id="be938-163">App Service-csomagokról a normál és a Linux-webalkalmazások is kölcsönösen kizárja egymást, így a nem Linux app service-csomag nem hozható létre egy Linux-webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="be938-163">Also, App Service plans for regular and Linux web apps are mutually exclusive, so you cannot create a Linux web app in a non-Linux app service plan.</span></span>

<span data-ttu-id="be938-164">Web Apps Linux létre kell hozni egy erőforráscsoport, amely nem tartalmazza a hello nem Linux webalkalmazások ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="be938-164">Web Apps on Linux must be created in a resource group that does not contain non-Linux web apps in hello same region.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="be938-165">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="be938-165">Troubleshooting</span></span> ##

<span data-ttu-id="be938-166">Az alkalmazás toostart meghibásodik, vagy az alkalmazás kívánt toocheck hello naplózási, ellenőrizze a Docker bejelentkezik hello naplófájlok directory hello.</span><span class="sxs-lookup"><span data-stu-id="be938-166">When your application fails toostart or you want toocheck hello logging from your app, check hello Docker logs in hello LogFiles directory.</span></span> <span data-ttu-id="be938-167">Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="be938-167">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="be938-168">toolog hello `stdout` és `stderr` a tárolóból tooenable kell **Docker-tároló naplózási** alatt **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="be938-168">toolog hello `stdout` and `stderr` from your container, you need tooenable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Naplózás engedélyezése][2]

![A Kudu tooview Docker-naplók segítségével][1]

<span data-ttu-id="be938-171">Van-e hozzáférési hello SCM helyet **speciális eszközök** a hello **Fejlesztőeszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="be938-171">You can access hello SCM site from **Advanced Tools** in hello **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be938-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be938-172">Next steps</span></span>
<span data-ttu-id="be938-173">Tekintse meg a következő hivatkozások tooget az App Service Linux hello.</span><span class="sxs-lookup"><span data-stu-id="be938-173">See hello following links tooget started with App Service on Linux.</span></span> <span data-ttu-id="be938-174">Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="be938-174">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="be938-175">Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="be938-175">How toouse a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="be938-176">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="be938-176">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="be938-177">.NET Core használatát az Azure App Service webalkalmazásba Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="be938-177">Using .NET Core in Azure App Service Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="be938-178">Ruby használatát az Azure App Service webalkalmazásba Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="be938-178">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="be938-179">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="be938-179">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="be938-180">Azure Web Apps Linux SSH támogatása</span><span class="sxs-lookup"><span data-stu-id="be938-180">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="be938-181">Átmeneti környezet az Azure App Service beállítása</span><span class="sxs-lookup"><span data-stu-id="be938-181">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="be938-182">Docker Hub folyamatos üzembe helyezés Linux Azure-webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="be938-182">Docker Hub Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png