---
title: "aaaAzure App Service Web App Linux – gyakori kérdések |} Microsoft Docs"
description: "Az Azure App Service webalkalmazásba Linux – gyakori kérdések."
keywords: "az Azure app service, webalkalmazás, gyakran ismételt kérdések, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: c7798d9144d936eecdc0e191fc870b0ee0b220c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a><span data-ttu-id="73212-104">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="73212-104">Azure App Service Web App on Linux FAQ</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="73212-105">Webalkalmazás Linux hello kiadással fejlesztjük szolgáltatások hozzáadására és fejlesztések tooour platform elvégzése.</span><span class="sxs-lookup"><span data-stu-id="73212-105">With hello release of Web App on Linux, we're working on adding features and making improvements tooour platform.</span></span> <span data-ttu-id="73212-106">Íme néhány gyakori kérdésekkel (GYIK) ügyfeleink kérésére velünk keresztül hello utolsó hónapban.</span><span class="sxs-lookup"><span data-stu-id="73212-106">Here are some frequently asked questions (FAQ) that our customers have been asking us over hello last months.</span></span>
<span data-ttu-id="73212-107">Ha egy kérdést, Megjegyzés hello cikken, és azt fogja fogadja a hívást a lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="73212-107">If you have a question, comment on hello article and we'll answer it as soon as possible.</span></span>

## <a name="built-in-images"></a><span data-ttu-id="73212-108">Beépített lemezképek</span><span class="sxs-lookup"><span data-stu-id="73212-108">Built-in images</span></span>

<span data-ttu-id="73212-109">**K:** kívánt toofork hello beépített Docker tárolókat hello platformot biztosít.</span><span class="sxs-lookup"><span data-stu-id="73212-109">**Q:** I want toofork hello built-in Docker containers that hello platform provides.</span></span> <span data-ttu-id="73212-110">Hol találok azokat a fájlokat?</span><span class="sxs-lookup"><span data-stu-id="73212-110">Where can I find those files?</span></span>

<span data-ttu-id="73212-111">**V:** minden Docker-fájl található [GitHub](https://github.com/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="73212-111">**A:** You can find all Docker files on [GitHub](https://github.com/azure-app-service).</span></span> <span data-ttu-id="73212-112">Az összes Docker-tároló található [Docker Hub](https://hub.docker.com/u/appsvc/).</span><span class="sxs-lookup"><span data-stu-id="73212-112">You can find all Docker containers on [Docker Hub](https://hub.docker.com/u/appsvc/).</span></span>

<span data-ttu-id="73212-113">**K:** hello várt értékek hello indítási fájl a szakaszban a amikor beállítani a hello futásidejű verem Mik?</span><span class="sxs-lookup"><span data-stu-id="73212-113">**Q:** What are hello expected values for hello Startup File section when I configure hello runtime stack?</span></span>

<span data-ttu-id="73212-114">**V:** a Node.Js megadott hello PM2 konfigurációs fájl vagy a parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="73212-114">**A:** For Node.Js, you specify hello PM2 configuration file or your script file.</span></span> <span data-ttu-id="73212-115">A .NET Core írja be a lefordított dll-fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="73212-115">For .NET Core, specify your compiled DLL name.</span></span> <span data-ttu-id="73212-116">Ruby, a Ruby parancsfájl hello adhat meg, amelyet tooinitialize az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="73212-116">For Ruby, you can specify hello Ruby script that you want tooinitialize your app with.</span></span>

## <a name="management"></a><span data-ttu-id="73212-117">Kezelés</span><span class="sxs-lookup"><span data-stu-id="73212-117">Management</span></span>

<span data-ttu-id="73212-118">**K:** mi történik, amikor hello Újraindítás gombra a hello Azure-portálon?</span><span class="sxs-lookup"><span data-stu-id="73212-118">**Q:** What happens when I press hello restart button in hello Azure portal?</span></span>

<span data-ttu-id="73212-119">**V:** ezzel egyenértékű Docker-újraindításának van hello.</span><span class="sxs-lookup"><span data-stu-id="73212-119">**A:** This is hello equivalent of Docker restart.</span></span>

<span data-ttu-id="73212-120">**K:** használható Secure Shell (SSH) tooconnect toohello app tároló virtuális gép (VM)?</span><span class="sxs-lookup"><span data-stu-id="73212-120">**Q:** Can I use Secure Shell (SSH) tooconnect toohello app container virtual machine (VM)?</span></span>

<span data-ttu-id="73212-121">**V:** Igen, akkor teheti meg, hogy hello SCM webhelyen, hello következő ellenőrzése a következő cikket: további információk [webalkalmazás Linux SSH-támogatás](./app-service-linux-ssh-support.md)</span><span class="sxs-lookup"><span data-stu-id="73212-121">**A:** Yes, you can do that through hello SCM site, check hello following article for more information [SSH support for Web App on Linux](./app-service-linux-ssh-support.md)</span></span>

<span data-ttu-id="73212-122">**K:** toocreate Linux App Service ponton keresztül SDK vagy egy ARM-sablont szeretnék, hogyan lehet elérése Ez?</span><span class="sxs-lookup"><span data-stu-id="73212-122">**Q:** I want toocreate a Linux App Service plane through SDK or an ARM template, how can I achieve this?</span></span>

<span data-ttu-id="73212-123">**V:** tooset hello kell `reserved` hello alkalmazás mező túl szolgáltatás`true`.</span><span class="sxs-lookup"><span data-stu-id="73212-123">**A:** You need tooset hello `reserved` field of hello app service too`true`.</span></span>

## <a name="continuous-integrationdeployment"></a><span data-ttu-id="73212-124">A folyamatos integrációs vagy üzembe helyező</span><span class="sxs-lookup"><span data-stu-id="73212-124">Continuous integration/deployment</span></span>

<span data-ttu-id="73212-125">**K:** a webes alkalmazás továbbra is használja egy Docker-tároló régi lemezképet, miután hello kép Docker központ frissítése már megtörtént.</span><span class="sxs-lookup"><span data-stu-id="73212-125">**Q:** My web app still uses an old Docker container image after I've updated hello image on Docker Hub.</span></span> <span data-ttu-id="73212-126">Egyéni tárolók folyamatos integráció/telepítésének támogatása?</span><span class="sxs-lookup"><span data-stu-id="73212-126">Do you support continuous integration/deployment of custom containers?</span></span>

<span data-ttu-id="73212-127">**V:** folyamatos integráció/üzembe által a következő cikket ellenőrzés hello Azure tároló beállításjegyzék vagy DockerHub lemezképet fel tooset [folyamatos üzembe helyezés az Azure Web Apps Linux](./app-service-linux-ci-cd.md).</span><span class="sxs-lookup"><span data-stu-id="73212-127">**A:** tooset up continuous integration/deployment for Azure Container Registry or DockerHub images by check hello following article [Continuous Deployment with Azure Web App on Linux](./app-service-linux-ci-cd.md).</span></span> <span data-ttu-id="73212-128">A titkos nyilvántartó hello tároló frissítéséhez a webalkalmazás majd indítása és leállítása.</span><span class="sxs-lookup"><span data-stu-id="73212-128">For private registries, you can refresh hello container by stopping and then starting your web app.</span></span> <span data-ttu-id="73212-129">Vagy módosíthatja vagy tooforce beállítása a tároló frissítését üres alkalmazás felvétele.</span><span class="sxs-lookup"><span data-stu-id="73212-129">Or you can change or add a dummy application setting tooforce a refresh of your container.</span></span>

<span data-ttu-id="73212-130">**K:** támogatott az átmeneti környezetben?</span><span class="sxs-lookup"><span data-stu-id="73212-130">**Q:** Do you support staging environments?</span></span>

<span data-ttu-id="73212-131">**V:** Igen.</span><span class="sxs-lookup"><span data-stu-id="73212-131">**A:** Yes.</span></span>

<span data-ttu-id="73212-132">**K:** használható **a web deploy** toodeploy a webes alkalmazást?</span><span class="sxs-lookup"><span data-stu-id="73212-132">**Q:** Can I use **web deploy** toodeploy my web app?</span></span>

<span data-ttu-id="73212-133">**V:** , kell tooset nevű beállítása alkalmazás `WEBSITE_WEBDEPLOY_USE_SCM` túl`false`.</span><span class="sxs-lookup"><span data-stu-id="73212-133">**A:** Yes, you need tooset an app setting called `WEBSITE_WEBDEPLOY_USE_SCM` too`false`.</span></span>

## <a name="language-support"></a><span data-ttu-id="73212-134">Nyelvi támogatás</span><span class="sxs-lookup"><span data-stu-id="73212-134">Language support</span></span>

<span data-ttu-id="73212-135">**K:** támogatják nem fordított .NET Core alkalmazások?</span><span class="sxs-lookup"><span data-stu-id="73212-135">**Q:** Do you support uncompiled .NET Core apps?</span></span>

<span data-ttu-id="73212-136">**V:** Igen.</span><span class="sxs-lookup"><span data-stu-id="73212-136">**A:** Yes.</span></span>

<span data-ttu-id="73212-137">**K:** támogatják a szerkesztő függőségi vezető PHP-alkalmazásokhoz?</span><span class="sxs-lookup"><span data-stu-id="73212-137">**Q:** Do you support Composer as a dependency manager for PHP apps?</span></span>

<span data-ttu-id="73212-138">**V:** Igen.</span><span class="sxs-lookup"><span data-stu-id="73212-138">**A:** Yes.</span></span> <span data-ttu-id="73212-139">A Git telepítés során a Kudu telepíti a PHP-alkalmazások (Köszönjük toohello jelenlétét a composer.json fájlok), és akkor indul el, a szerkesztő telepítése meg kell észleli.</span><span class="sxs-lookup"><span data-stu-id="73212-139">During a Git deployment, Kudu should detect that you are deploying a PHP application (thanks toohello presence of a composer.json file) and will trigger a composer install for you.</span></span>

## <a name="custom-containers"></a><span data-ttu-id="73212-140">Egyéni tárolók</span><span class="sxs-lookup"><span data-stu-id="73212-140">Custom containers</span></span>

<span data-ttu-id="73212-141">**K:** használom a saját egyéni tároló.</span><span class="sxs-lookup"><span data-stu-id="73212-141">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="73212-142">Saját alkalmazás hello található `\home\` könyvtár, de nem található a fájlok I hello tartalmak böngészése hello segítségével [SCM hely](https://github.com/projectkudu/kudu) vagy az FTP-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="73212-142">My app resides in hello `\home\` directory, but I can't find my files when I browse hello content by using hello [SCM site](https://github.com/projectkudu/kudu) or an FTP client.</span></span> <span data-ttu-id="73212-143">Hol találhatók a fájlok?</span><span class="sxs-lookup"><span data-stu-id="73212-143">Where are my files?</span></span>

<span data-ttu-id="73212-144">**V:** azt csatlakoztatása egy SMB megosztási toohello `\home\` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="73212-144">**A:** We mount an SMB share toohello `\home\` directory.</span></span> <span data-ttu-id="73212-145">Ez a művelet felülírja a telepített tartalmak van-e.</span><span class="sxs-lookup"><span data-stu-id="73212-145">This will override any content that's there.</span></span>

<span data-ttu-id="73212-146">**K:** használom a saját egyéni tároló.</span><span class="sxs-lookup"><span data-stu-id="73212-146">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="73212-147">Most nem kívánok hello platform toomount egy SMB megosztási toohello `\home\`.</span><span class="sxs-lookup"><span data-stu-id="73212-147">I don't want hello platform toomount an SMB share toohello `\home\`.</span></span>

<span data-ttu-id="73212-148">**V:** elvégezhető, amely a hello beállítása `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app beállítás túl`false`.</span><span class="sxs-lookup"><span data-stu-id="73212-148">**A:** You can do that by setting hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting too`false`.</span></span>

<span data-ttu-id="73212-149">**K:** saját egyéni tároló egy hosszú ideig toostart, valamint a hello platform újraindítás hello tároló példánytól, mielőtt befejezi indítása.</span><span class="sxs-lookup"><span data-stu-id="73212-149">**Q:** My custom container takes a long time toostart, and hello platform restart hello container before it finishes starting up.</span></span>

<span data-ttu-id="73212-150">**V:** beállíthatja a tároló újraindítása előtt várjon hello platform hello időt.</span><span class="sxs-lookup"><span data-stu-id="73212-150">**A:** You can configure hello time hello platform will wait before restarting your container.</span></span> <span data-ttu-id="73212-151">Ezt úgy teheti hello beállítása `WEBSITES_CONTAINER_START_TIME_LIMIT` app beállítás toohello a keresett másodpercben.</span><span class="sxs-lookup"><span data-stu-id="73212-151">This can be done by setting hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting toohello desired value in seconds.</span></span> <span data-ttu-id="73212-152">hello alapértelmezett érték 230 másodperc, és maximális hello 600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="73212-152">hello default is 230 seconds, and hello max is 600 seconds.</span></span>

<span data-ttu-id="73212-153">**K:** titkos beállításjegyzék-kiszolgáló URL-címének formátuma hello újdonságai?</span><span class="sxs-lookup"><span data-stu-id="73212-153">**Q:** What is hello format for private registry server url?</span></span>

<span data-ttu-id="73212-154">**V:** tooprovide hello teljes beállításjegyzék URL-címe például szüksége `http://` vagy `https://`.</span><span class="sxs-lookup"><span data-stu-id="73212-154">**A:** You need tooprovide hello full registry url including `http://` or `https://`.</span></span>

<span data-ttu-id="73212-155">**K:** Újdonságok hello formátum hello kép nevét a titkos beállításjegyzék-beállítást?</span><span class="sxs-lookup"><span data-stu-id="73212-155">**Q:** What is hello format for hello image name in private registry option?</span></span>

<span data-ttu-id="73212-156">**V:** szüksége tooadd hello teljes lemezkép neve például hello titkos beállításjegyzék URL-cím (pl..</span><span class="sxs-lookup"><span data-stu-id="73212-156">**A:** You need tooadd hello full image name including hello private registry url (eg.</span></span> <span data-ttu-id="73212-157">myacr.azurecr.IO/DotNet:Latest)</span><span class="sxs-lookup"><span data-stu-id="73212-157">myacr.azurecr.io/dotnet:latest)</span></span>

<span data-ttu-id="73212-158">**K:** kívánt tooexpose több port saját egyéni tároló lemezképen.</span><span class="sxs-lookup"><span data-stu-id="73212-158">**Q:** I want tooexpose more than one port on my custom container image.</span></span> <span data-ttu-id="73212-159">Van lehetőség, amely?</span><span class="sxs-lookup"><span data-stu-id="73212-159">Is that possible?</span></span>

<span data-ttu-id="73212-160">**V:** jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="73212-160">**A:** Currently, that isn't supported.</span></span>

<span data-ttu-id="73212-161">**K:** hozzáadhatom saját storage?</span><span class="sxs-lookup"><span data-stu-id="73212-161">**Q:** Can I bring my own storage?</span></span>

<span data-ttu-id="73212-162">**V:** jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="73212-162">**A:** Currently that isn't supported.</span></span>

<span data-ttu-id="73212-163">**K:** nem érhető saját egyéni tároló fájl rendszer futó folyamatok hello SCM helyről.</span><span class="sxs-lookup"><span data-stu-id="73212-163">**Q:** I can't browse my custom container's file system or running processes from hello SCM site.</span></span> <span data-ttu-id="73212-164">Az oka?</span><span class="sxs-lookup"><span data-stu-id="73212-164">Why is that?</span></span>

<span data-ttu-id="73212-165">**V:** hello SCM hely külön futtatja.</span><span class="sxs-lookup"><span data-stu-id="73212-165">**A:** hello SCM site runs in a separate container.</span></span> <span data-ttu-id="73212-166">Nem lehet ellenőrizni a hello fájlrendszer vagy a futó folyamatok hello app tároló.</span><span class="sxs-lookup"><span data-stu-id="73212-166">You can't check hello file system or running processes of hello app container.</span></span>

<span data-ttu-id="73212-167">**K:** saját egyéni tároló tooa port a 80-as port nem figyel.</span><span class="sxs-lookup"><span data-stu-id="73212-167">**Q:** My custom container listens tooa port other than port 80.</span></span> <span data-ttu-id="73212-168">Hogyan konfigurálható a saját alkalmazás tooroute hello kérelmek toothat port?</span><span class="sxs-lookup"><span data-stu-id="73212-168">How can I configure my app tooroute hello requests toothat port?</span></span>

<span data-ttu-id="73212-169">**V:** automatikus port észlelési van, egy alkalmazás nevű beállítása meg is **WEBSITES_PORT**, és adjon neki hello értéket várt hello portszámot.</span><span class="sxs-lookup"><span data-stu-id="73212-169">**A:** We have auto port detection, also you can specify an application setting called **WEBSITES_PORT**, and give it hello value of hello expected port number.</span></span> <span data-ttu-id="73212-170">Hello platform használta korábban `PORT` app, azt tervezi, toodeprecate hello használata az alkalmazás, beállítás és toousing áthelyezése `WEBSITES_PORT` kizárólag.</span><span class="sxs-lookup"><span data-stu-id="73212-170">Previously hello platform was using `PORT` app setting, we are planning toodeprecate hello use this app setting and move toousing `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="73212-171">**K:** van tooimplement HTTPS saját egyéni tárolóban.</span><span class="sxs-lookup"><span data-stu-id="73212-171">**Q:** Do I need tooimplement HTTPS in my custom container.</span></span>

<span data-ttu-id="73212-172">**V:** nem, hello platform kezeli-e a megosztott hello frontends HTTPS-záráshoz.</span><span class="sxs-lookup"><span data-stu-id="73212-172">**A:** No, hello platform handles HTTPS termination at hello shared frontends.</span></span>

## <a name="pricing-and-sla"></a><span data-ttu-id="73212-173">Díjszabás és SLA</span><span class="sxs-lookup"><span data-stu-id="73212-173">Pricing and SLA</span></span>

<span data-ttu-id="73212-174">**K:** mi van hello árképzési hello nyilvános előzetes használata során?</span><span class="sxs-lookup"><span data-stu-id="73212-174">**Q:** What's hello pricing while you're using hello public preview?</span></span>

<span data-ttu-id="73212-175">**V:** fele hello óraszámon belül, az alkalmazás, amelynek hello normál Azure App Service szolgáltatás díjszabása van szó.</span><span class="sxs-lookup"><span data-stu-id="73212-175">**A:** You are charged half hello number of hours that your app runs, with hello normal Azure App Service pricing.</span></span> <span data-ttu-id="73212-176">Ez azt jelenti, hogy a normál Azure App Service szolgáltatás díjszabása 50 % kedvezménnyel kap.</span><span class="sxs-lookup"><span data-stu-id="73212-176">This means that you get a 50 percent discount on normal Azure App Service pricing.</span></span>

## <a name="other"></a><span data-ttu-id="73212-177">Egyéb</span><span class="sxs-lookup"><span data-stu-id="73212-177">Other</span></span>

<span data-ttu-id="73212-178">**K:** Mik azok a beállítások az alkalmazásnevekben hello támogatott karakter?</span><span class="sxs-lookup"><span data-stu-id="73212-178">**Q:** What are hello supported characters in application settings names?</span></span>

<span data-ttu-id="73212-179">**V:** csak használja A-Z, a – z, 0-9, és az alkalmazásbeállítások aláhúzás hello.</span><span class="sxs-lookup"><span data-stu-id="73212-179">**A:** You can only use A-Z, a-z, 0-9, and hello underscore for application settings.</span></span>

<span data-ttu-id="73212-180">**K:** ahol is kérjen új szolgáltatások?</span><span class="sxs-lookup"><span data-stu-id="73212-180">**Q:** Where can I request new features?</span></span>

<span data-ttu-id="73212-181">**V:** elküldheti a képet, hello [webalkalmazások visszajelzési fórumon](https://aka.ms/webapps-uservoice).</span><span class="sxs-lookup"><span data-stu-id="73212-181">**A:** You can submit your idea at hello [Web Apps feedback forum](https://aka.ms/webapps-uservoice).</span></span> <span data-ttu-id="73212-182">Adja hozzá a ötlet toohello címe "[Linux]".</span><span class="sxs-lookup"><span data-stu-id="73212-182">Add "[Linux]" toohello title of your idea.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73212-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73212-183">Next steps</span></span>
* [<span data-ttu-id="73212-184">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="73212-184">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="73212-185">Azure Web Apps Linux SSH támogatása</span><span class="sxs-lookup"><span data-stu-id="73212-185">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="73212-186">Átmeneti környezet az Azure App Service beállítása</span><span class="sxs-lookup"><span data-stu-id="73212-186">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="73212-187">Folyamatos üzembe helyezés az Azure-webalkalmazásban Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="73212-187">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
