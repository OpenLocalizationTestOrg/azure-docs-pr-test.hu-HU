---
title: "Bevezetés az Azure-webalkalmazásban Linux |} Microsoft Docs"
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
ms.openlocfilehash: 0870f811845ec7c705da13f08abdfa762d25b209
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-azure-web-app-on-linux"></a><span data-ttu-id="283f3-104">Bevezetés az Azure-webalkalmazásban Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="283f3-104">Introduction to Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="283f3-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="283f3-105">Overview</span></span>
<span data-ttu-id="283f3-106">Felhasználók használhatják a webalkalmazás Linux gazdagép webalkalmazások Linux rendszeren natív módon a támogatott alkalmazás verem.</span><span class="sxs-lookup"><span data-stu-id="283f3-106">Customers can use Web App on Linux to host web apps natively on Linux for supported application stacks.</span></span> <span data-ttu-id="283f3-107">A következő szakasz az alkalmazás verem jelenleg támogatott.</span><span class="sxs-lookup"><span data-stu-id="283f3-107">The following section lists the application stacks that are currently supported.</span></span> 

## <a name="features"></a><span data-ttu-id="283f3-108">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="283f3-108">Features</span></span>
<span data-ttu-id="283f3-109">Webes alkalmazás Linux rendszeren jelenleg a következő alkalmazás verem támogatja:</span><span class="sxs-lookup"><span data-stu-id="283f3-109">Web App on Linux currently supports the following application stacks:</span></span>

* <span data-ttu-id="283f3-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="283f3-110">Node.js</span></span>
    * <span data-ttu-id="283f3-111">4.4</span><span class="sxs-lookup"><span data-stu-id="283f3-111">4.4</span></span>
    * <span data-ttu-id="283f3-112">4.5</span><span class="sxs-lookup"><span data-stu-id="283f3-112">4.5</span></span>
    * <span data-ttu-id="283f3-113">6.2</span><span class="sxs-lookup"><span data-stu-id="283f3-113">6.2</span></span>
    * <span data-ttu-id="283f3-114">6.6</span><span class="sxs-lookup"><span data-stu-id="283f3-114">6.6</span></span>
    * <span data-ttu-id="283f3-115">6.9</span><span class="sxs-lookup"><span data-stu-id="283f3-115">6.9</span></span>
    * <span data-ttu-id="283f3-116">6.10</span><span class="sxs-lookup"><span data-stu-id="283f3-116">6.10</span></span>
    * <span data-ttu-id="283f3-117">6.11</span><span class="sxs-lookup"><span data-stu-id="283f3-117">6.11</span></span>
    * <span data-ttu-id="283f3-118">8.0</span><span class="sxs-lookup"><span data-stu-id="283f3-118">8.0</span></span>
    * <span data-ttu-id="283f3-119">8.1</span><span class="sxs-lookup"><span data-stu-id="283f3-119">8.1</span></span>
* <span data-ttu-id="283f3-120">PHP</span><span class="sxs-lookup"><span data-stu-id="283f3-120">PHP</span></span>
    * <span data-ttu-id="283f3-121">5.6</span><span class="sxs-lookup"><span data-stu-id="283f3-121">5.6</span></span>
    * <span data-ttu-id="283f3-122">7.0</span><span class="sxs-lookup"><span data-stu-id="283f3-122">7.0</span></span>
* <span data-ttu-id="283f3-123">A .NET core</span><span class="sxs-lookup"><span data-stu-id="283f3-123">.Net Core</span></span>
    * <span data-ttu-id="283f3-124">1.0</span><span class="sxs-lookup"><span data-stu-id="283f3-124">1.0</span></span>
    * <span data-ttu-id="283f3-125">1.1</span><span class="sxs-lookup"><span data-stu-id="283f3-125">1.1</span></span>
* <span data-ttu-id="283f3-126">Ruby</span><span class="sxs-lookup"><span data-stu-id="283f3-126">Ruby</span></span>
    * <span data-ttu-id="283f3-127">2.3</span><span class="sxs-lookup"><span data-stu-id="283f3-127">2.3</span></span>

<span data-ttu-id="283f3-128">Központi telepítését az alkalmazások használatával:</span><span class="sxs-lookup"><span data-stu-id="283f3-128">Customers can deploy their applications by using:</span></span>

* <span data-ttu-id="283f3-129">FTP</span><span class="sxs-lookup"><span data-stu-id="283f3-129">FTP</span></span>
* <span data-ttu-id="283f3-130">Helyi Git</span><span class="sxs-lookup"><span data-stu-id="283f3-130">Local Git</span></span>
* <span data-ttu-id="283f3-131">GitHub</span><span class="sxs-lookup"><span data-stu-id="283f3-131">GitHub</span></span>
* <span data-ttu-id="283f3-132">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="283f3-132">Bitbucket</span></span>

<span data-ttu-id="283f3-133">Az alkalmazás méretezés:</span><span class="sxs-lookup"><span data-stu-id="283f3-133">For application scaling:</span></span>

* <span data-ttu-id="283f3-134">Az ügyfelek is méretezhető webalkalmazások felfelé és lefelé az App Service-csomag szintjének módosítása</span><span class="sxs-lookup"><span data-stu-id="283f3-134">Customers can scale web apps up and down by changing the tier of their App Service plan</span></span>
* <span data-ttu-id="283f3-135">Ügyfelek horizontális felskálázás alkalmazások és azok metódust belül több alkalmazás példányának futtatása</span><span class="sxs-lookup"><span data-stu-id="283f3-135">Customers can scale out applications and run multiple app instances within the confines of their SKU</span></span>

<span data-ttu-id="283f3-136">A Kudu, néhány alapvető funkcióit:</span><span class="sxs-lookup"><span data-stu-id="283f3-136">For Kudu, some of the basic functionality:</span></span>

* <span data-ttu-id="283f3-137">Környezetekben</span><span class="sxs-lookup"><span data-stu-id="283f3-137">Environments</span></span>
* <span data-ttu-id="283f3-138">Központi telepítés</span><span class="sxs-lookup"><span data-stu-id="283f3-138">Deployments</span></span>
* <span data-ttu-id="283f3-139">Alapszintű konzol</span><span class="sxs-lookup"><span data-stu-id="283f3-139">Basic console</span></span>
* <span data-ttu-id="283f3-140">SSH</span><span class="sxs-lookup"><span data-stu-id="283f3-140">SSH</span></span>

<span data-ttu-id="283f3-141">A devops:</span><span class="sxs-lookup"><span data-stu-id="283f3-141">For devops:</span></span>

* <span data-ttu-id="283f3-142">Átmeneti környezetek</span><span class="sxs-lookup"><span data-stu-id="283f3-142">Staging environments</span></span>
* <span data-ttu-id="283f3-143">ACR és DockerHub CI/CD</span><span class="sxs-lookup"><span data-stu-id="283f3-143">ACR and DockerHub CI/CD</span></span>

## <a name="limitations"></a><span data-ttu-id="283f3-144">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="283f3-144">Limitations</span></span>
<span data-ttu-id="283f3-145">Az Azure-portálon megjelenítése csak olyan funkciókat, amelyek a webalkalmazás Linux rendszeren jelenleg működik, és a többi elrejtése.</span><span class="sxs-lookup"><span data-stu-id="283f3-145">The Azure portal shows only features that currently work for Web App on Linux and hides the rest.</span></span> <span data-ttu-id="283f3-146">Engedélyezzük a szolgáltatásokat, azok lesznek láthatók a portálon.</span><span class="sxs-lookup"><span data-stu-id="283f3-146">As we enable more features, they will be visible on the portal.</span></span>

<span data-ttu-id="283f3-147">Egyes szolgáltatások, például a virtuális hálózati integráció, az Azure Active Directory vagy harmadik fél hitelesítést és a Kudu helyhez kiterjesztések, még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="283f3-147">Some features, such as virtual network integration, Azure Active Directory/third-party authentication, or Kudu site extensions, are not available yet.</span></span> <span data-ttu-id="283f3-148">Ezek a funkciók érhetők el, ha a dokumentáció és a változásokról blog frissítjük.</span><span class="sxs-lookup"><span data-stu-id="283f3-148">Once these features are available, we will update our documentation and blog about the changes.</span></span>

<span data-ttu-id="283f3-149">A nyilvános előzetes verzió érhető el jelenleg csak a következő régióban:</span><span class="sxs-lookup"><span data-stu-id="283f3-149">This public preview is currently only available in the following regions:</span></span>

* <span data-ttu-id="283f3-150">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="283f3-150">West US</span></span>
* <span data-ttu-id="283f3-151">USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="283f3-151">East US</span></span>
* <span data-ttu-id="283f3-152">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="283f3-152">West Europe</span></span>
* <span data-ttu-id="283f3-153">Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="283f3-153">North Europe</span></span>
* <span data-ttu-id="283f3-154">USA déli középső régiója</span><span class="sxs-lookup"><span data-stu-id="283f3-154">South Central US</span></span>
* <span data-ttu-id="283f3-155">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="283f3-155">North Central US</span></span>
* <span data-ttu-id="283f3-156">Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="283f3-156">Southeast Asia</span></span>
* <span data-ttu-id="283f3-157">Kelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="283f3-157">East Asia</span></span>
* <span data-ttu-id="283f3-158">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="283f3-158">Australia East</span></span>
* <span data-ttu-id="283f3-159">Kelet-Japán</span><span class="sxs-lookup"><span data-stu-id="283f3-159">Japan East</span></span>
* <span data-ttu-id="283f3-160">Dél-Brazília</span><span class="sxs-lookup"><span data-stu-id="283f3-160">Brazil South</span></span>
* <span data-ttu-id="283f3-161">Dél-India</span><span class="sxs-lookup"><span data-stu-id="283f3-161">South India</span></span>

<span data-ttu-id="283f3-162">Web Apps Linux csak akkor támogatott a kijelölt app service-csomagok, és nem rendelkezik egy ingyenes vagy közös réteg.</span><span class="sxs-lookup"><span data-stu-id="283f3-162">Web Apps on Linux is only supported in the Dedicated app service plans and does not have a Free or Shared tier.</span></span> <span data-ttu-id="283f3-163">App Service-csomagokról a normál és a Linux-webalkalmazások is kölcsönösen kizárja egymást, így a nem Linux app service-csomag nem hozható létre egy Linux-webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="283f3-163">Also, App Service plans for regular and Linux web apps are mutually exclusive, so you cannot create a Linux web app in a non-Linux app service plan.</span></span>

<span data-ttu-id="283f3-164">Web Apps Linux egy erőforráscsoport, amely nem tartalmazza a nem Linux webalkalmazások ugyanabban a régióban kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="283f3-164">Web Apps on Linux must be created in a resource group that does not contain non-Linux web apps in the same region.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="283f3-165">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="283f3-165">Troubleshooting</span></span> ##

<span data-ttu-id="283f3-166">Az alkalmazás nem indul el, vagy a naplózás a alkalmazás ellenőrizni kívánja, hogy a Docker naplózza a naplófájlok könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="283f3-166">When your application fails to start or you want to check the logging from your app, check the Docker logs in the LogFiles directory.</span></span> <span data-ttu-id="283f3-167">Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="283f3-167">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="283f3-168">A napló a `stdout` és `stderr` a tárolóból, engedélyezni kell a **Docker-tároló naplózási** alatt **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="283f3-168">To log the `stdout` and `stderr` from your container, you need to enable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Naplózás engedélyezése][2]

![A Kudu segítségével Docker naplók megtekintése][1]

<span data-ttu-id="283f3-171">Érheti el az SCM helyet **speciális eszközök** a a **Fejlesztőeszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="283f3-171">You can access the SCM site from **Advanced Tools** in the **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="283f3-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="283f3-172">Next steps</span></span>
<span data-ttu-id="283f3-173">Tekintse meg az alábbi hivatkozásokra kattintva az App Service Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="283f3-173">See the following links to get started with App Service on Linux.</span></span> <span data-ttu-id="283f3-174">Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="283f3-174">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="283f3-175">Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="283f3-175">How to use a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="283f3-176">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="283f3-176">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="283f3-177">.NET Core használatát az Azure App Service webalkalmazásba Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="283f3-177">Using .NET Core in Azure App Service Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="283f3-178">Ruby használatát az Azure App Service webalkalmazásba Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="283f3-178">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="283f3-179">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="283f3-179">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="283f3-180">Azure Web Apps Linux SSH támogatása</span><span class="sxs-lookup"><span data-stu-id="283f3-180">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="283f3-181">Átmeneti környezet az Azure App Service beállítása</span><span class="sxs-lookup"><span data-stu-id="283f3-181">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="283f3-182">Docker Hub folyamatos üzembe helyezés Linux Azure-webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="283f3-182">Docker Hub Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png