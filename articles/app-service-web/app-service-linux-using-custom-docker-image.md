---
title: "Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára |} Microsoft Docs"
description: "Hogyan használható az egyéni Docker-lemezkép Linux Azure webalkalmazás számára."
keywords: "az Azure app service, a webes alkalmazás, a linux, a docker, a tároló"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 1458217a31c4781b28877c030a665f5b22819e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a><span data-ttu-id="b92e4-104">Az Azure Web Apps Linux rendszeren egyéni Docker-lemezkép használatával</span><span class="sxs-lookup"><span data-stu-id="b92e4-104">Using a custom Docker image for Azure Web App on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="b92e4-105">App Service biztosít előre definiált alkalmazás verem Linux-támogatással rendelkező különböző verziókat, például a PHP 7.0 és a Node.js 4.5.</span><span class="sxs-lookup"><span data-stu-id="b92e4-105">App Service provides pre-defined application stacks on Linux with support for specific versions, such as PHP 7.0 and Node.js 4.5.</span></span> <span data-ttu-id="b92e4-106">App Service Linux Docker-tároló ezeket az előre elkészített alkalmazás-készleteket használ.</span><span class="sxs-lookup"><span data-stu-id="b92e4-106">App Service on Linux uses Docker containers to host these pre-built application stacks.</span></span> <span data-ttu-id="b92e4-107">Egyéni Docker-lemezkép használatával a webalkalmazás telepítése az egy alkalmazás verem nem definiált már az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b92e4-107">You can also use a custom Docker image to deploy your web app to an application stack that is not already defined in Azure.</span></span> <span data-ttu-id="b92e4-108">Egyéni Docker lemezképek alábbiakon tárolható vagy egy nyilvános vagy privát Docker-tárházat.</span><span class="sxs-lookup"><span data-stu-id="b92e4-108">Custom Docker images can be hosted on either a public or private Docker repository.</span></span>


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a><span data-ttu-id="b92e4-109">Útmutató: a webalkalmazáshoz tartozó egyéni Docker-lemezkép beállítása</span><span class="sxs-lookup"><span data-stu-id="b92e4-109">How to: set a custom Docker image for a web app</span></span>
<span data-ttu-id="b92e4-110">Az egyéni Docker lemezképet mindkét meglévő és új webhelyek alkalmazások állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="b92e4-110">You can set the custom Docker image for both new and existing webs apps.</span></span> <span data-ttu-id="b92e4-111">Ha a Linux-webalkalmazás létrehozása a [Azure-portálon](https://portal.azure.com/#create/Microsoft.AppSvcLinux), kattintson a **konfigurálása tároló** beállítása egy egyéni Docker-lemezképet:</span><span class="sxs-lookup"><span data-stu-id="b92e4-111">When you create a web app on Linux in the [Azure portal](https://portal.azure.com/#create/Microsoft.AppSvcLinux), click **Configure container** to set a custom Docker image:</span></span>

![Az új webalkalmazáshoz Linux rendszeren egyéni Docker kép][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a><span data-ttu-id="b92e4-113">Útmutató: a Docker Hub egyéni Docker kép használata</span><span class="sxs-lookup"><span data-stu-id="b92e4-113">How to: use a custom Docker image from Docker Hub</span></span> ##
<span data-ttu-id="b92e4-114">A Docker Hub egyéni Docker-lemezkép használata:</span><span class="sxs-lookup"><span data-stu-id="b92e4-114">To use a custom Docker image from Docker Hub:</span></span>

1. <span data-ttu-id="b92e4-115">Az a [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.</span><span class="sxs-lookup"><span data-stu-id="b92e4-115">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="b92e4-116">Válassza ki **Docker Hub** , a **lemezképforrás**, ezután kattintson **nyilvános** vagy **titkos** , és írja be a **lemezkép és nem kötelező címke neve**, például a `node:4.5`.</span><span class="sxs-lookup"><span data-stu-id="b92e4-116">Select **Docker Hub** as the **Image source**, then click either **Public** or **Private** and type the **Image and optional tag name**, such as `node:4.5`.</span></span> <span data-ttu-id="b92e4-117">A **indítási parancs** beállítása automatikusan alapul a Docker bináris fájlban meghatározott, de a saját parancsok állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="b92e4-117">The **Startup command** is set automatically based on what is defined in the Docker image file, but you can set your own commands.</span></span>  

    ![Kép: a nyilvános tárház Docker központ konfigurálása][2]

    <span data-ttu-id="b92e4-119">A lemezkép egy titkos tárházból esetén is kell a Docker Hub hitelesítő adatokat, mint (**bejelentkezési felhasználónevének** és **jelszó**) a saját Docker Hub tárház.</span><span class="sxs-lookup"><span data-stu-id="b92e4-119">When your image is from a private repository, you also need to enter the Docker Hub credentials as (**Login username** and **Password**) for the private Docker Hub repository.</span></span>

    ![Kép: Docker Hub személyes adattár konfigurálása][3]

3. <span data-ttu-id="b92e4-121">Miután konfigurálta a tárolót, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b92e4-121">After you have configured the container, click **Save**.</span></span>

## <a name="how-to-use-a-docker-image-from-a-private-image-registry"></a><span data-ttu-id="b92e4-122">A Docker-lemezkép használata a titkos kép beállításjegyzékből</span><span class="sxs-lookup"><span data-stu-id="b92e4-122">How to use a Docker image from a private image registry</span></span> ##
<span data-ttu-id="b92e4-123">Egyéni Docker-lemezkép használata a titkos kép beállításjegyzékből:</span><span class="sxs-lookup"><span data-stu-id="b92e4-123">To use a custom Docker image from a private image registry:</span></span>

1. <span data-ttu-id="b92e4-124">Az a [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.</span><span class="sxs-lookup"><span data-stu-id="b92e4-124">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="b92e4-125">Kattintson a **titkos beállításjegyzék** , a **lemezképforrás**.</span><span class="sxs-lookup"><span data-stu-id="b92e4-125">Click **Private registry** as the **Image source**.</span></span> <span data-ttu-id="b92e4-126">Adja meg a **kép és a választható címkenév**, **URL-címe** titkos beállításjegyzék, a hitelesítő adatokkal együtt (**bejelentkezési felhasználónevének** és **jelszó**).</span><span class="sxs-lookup"><span data-stu-id="b92e4-126">Enter the **Image and optional tag name**, **Server URL** for the private registry, along with the credentials (**Login username** and **Password**).</span></span> <span data-ttu-id="b92e4-127">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b92e4-127">Click **Save**.</span></span>

    ![Docker kép titkos beállításjegyzékből konfigurálása][4]


## <a name="how-to-set-the-port-used-by-your-docker-image"></a><span data-ttu-id="b92e4-129">Útmutató: a Docker-lemezkép által használt port beállítása</span><span class="sxs-lookup"><span data-stu-id="b92e4-129">How to: set the port used by your Docker image</span></span> ##

<span data-ttu-id="b92e4-130">Ha egy egyéni Docker lemezképet használ a webalkalmazás, használhatja a `WEBSITES_PORT` környezeti változót a Dockerfile, amely lekérdezi a generált tárolóhoz adni.</span><span class="sxs-lookup"><span data-stu-id="b92e4-130">When you use a custom Docker image for your web app, you can use the `WEBSITES_PORT` environment variable in your Dockerfile, which gets added to the generated container.</span></span> <span data-ttu-id="b92e4-131">Vegye figyelembe a következő példa egy docker-fájl egy Ruby alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="b92e4-131">Consider the following example of a docker file for a Ruby application:</span></span>

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

<span data-ttu-id="b92e4-132">A parancs utolsó sora megtekintheti, hogy a WEBSITES_PORT környezeti változó átadása futásidőben.</span><span class="sxs-lookup"><span data-stu-id="b92e4-132">On last line of the command, you can see that the WEBSITES_PORT environment variable is passed at runtime.</span></span> <span data-ttu-id="b92e4-133">Ne feledje, hogy a kis-és nagybetűk parancsokban számít.</span><span class="sxs-lookup"><span data-stu-id="b92e4-133">Remember that casing matters in commands.</span></span>

<span data-ttu-id="b92e4-134">A platform használta korábban `PORT` app beállításnál azt tervezi, hogy érvényteleníthető használatát az alkalmazás, beállítás és áthelyezése használatával `WEBSITES_PORT` kizárólag.</span><span class="sxs-lookup"><span data-stu-id="b92e4-134">Previously the platform was using `PORT` app setting, we are planning to deprecate the use this app setting and move to using `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="b92e4-135">Egy meglévő, beépített valaki más Docker kép használatakor esetleg adja meg az alkalmazás 80-as portra.</span><span class="sxs-lookup"><span data-stu-id="b92e4-135">When you use an existing Docker image built by someone else, you may need to specify a port other than port 80 for the application.</span></span> <span data-ttu-id="b92e4-136">Az a port megadásához nevű beállításával alkalmazás hozzáadása `WEBSITES_PORT` a értékű alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="b92e4-136">To configure the port, add an application setting named `WEBSITES_PORT` with the value as shown below:</span></span>

![Egyéni Docker-lemezképet alkalmazás portbeállítást konfigurálása][6]

## <a name="how-to-set-the-startup-time-for-your-docker-image"></a><span data-ttu-id="b92e4-138">Útmutató: a Docker-lemezkép indítási idejének beállítása</span><span class="sxs-lookup"><span data-stu-id="b92e4-138">How to: set the startup time for your Docker image</span></span> ##

<span data-ttu-id="b92e4-139">Alapértelmezés szerint a tároló nem indul el, mielőtt 230 másodperc, ha a platform újraindítja a tárolót.</span><span class="sxs-lookup"><span data-stu-id="b92e4-139">By default, if your container does not start before 230 seconds, the platform will restart your container.</span></span> <span data-ttu-id="b92e4-140">Ha az egyéni Docker-lemezkép 230 másodpercnél indul, használhatja a `WEBSITES_CONTAINER_START_TIME_LIMIT` másodpercben alkalmazás beállítása, a beállításnak az értéke, ez lehetővé teszi a platform tartsa a tárolóban fut, akkor az újraindítás előtt.</span><span class="sxs-lookup"><span data-stu-id="b92e4-140">If your custom Docker image starts in more than 230 seconds, you can use the `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting, the value for this setting is in seconds, this will allow the platform keep your container running before restarting it.</span></span> <span data-ttu-id="b92e4-141">Az alapértelmezett érték 230 másodperc, és a maximális megengedett értéke 600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="b92e4-141">The default value is 230 seconds, and the max allowed value is 600 seconds.</span></span>

## <a name="how-to-unmount-the-platform-provided-storage"></a><span data-ttu-id="b92e4-142">Útmutató: a megadott platform tárolási leválasztása</span><span class="sxs-lookup"><span data-stu-id="b92e4-142">How to: unmount the platform provided storage</span></span> ##

<span data-ttu-id="b92e4-143">Alapértelmezés szerint a platform egy állandó tároló megosztás fogja csatlakoztatni a `\home\` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="b92e4-143">By default, the platform will mount a persistent storage share to the `\home\` directory.</span></span> <span data-ttu-id="b92e4-144">Ha a tároló-lemezkép nem kell egy állandó megosztást, tárolási csatlakoztatása beállításával letilthatja a `WEBSITES_ENABLE_APP_SERVICE_STORAGE` Alkalmazásbeállítás `false`.</span><span class="sxs-lookup"><span data-stu-id="b92e4-144">If your container image does not need a persistent share, you can disable mounting that storage by setting the `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting to `false`.</span></span> <span data-ttu-id="b92e4-145">Ön továbbra is hozzáférhetnek a tárolási az scm-helyről, és minden Docker-naplók (Ha engedélyezve van), a platform által létrehozott naplófájlok lesz írva.</span><span class="sxs-lookup"><span data-stu-id="b92e4-145">You will still have access to that storage from the scm site, and all Docker logs (if enabled) will be written to the log files generated by the platform.</span></span>

## <a name="how-to-switch-back-to-using-a-built-in-image"></a><span data-ttu-id="b92e4-146">Hogyan: Váltás beépített lemezkép használatával</span><span class="sxs-lookup"><span data-stu-id="b92e4-146">How to: Switch back to using a built-in image</span></span> ##

<span data-ttu-id="b92e4-147">Váltás a beépített lemezképpel egyéni lemezkép használatával:</span><span class="sxs-lookup"><span data-stu-id="b92e4-147">To switch from using a custom image to using a built-in image:</span></span>

1. <span data-ttu-id="b92e4-148">Az a [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **App Service**.</span><span class="sxs-lookup"><span data-stu-id="b92e4-148">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **App Service**.</span></span>

2. <span data-ttu-id="b92e4-149">Válassza ki a **futásidejű verem** kíván használni a beépített lemezképet, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b92e4-149">Select your **Runtime Stack** to use for the built-in image, then click **Save**.</span></span> 

![Kép: a beépített Docker konfigurálása][5]


## <a name="troubleshooting"></a><span data-ttu-id="b92e4-151">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="b92e4-151">Troubleshooting</span></span> ##

<span data-ttu-id="b92e4-152">Ha az alkalmazás nem az egyéni Docker lemezképpel indul el, ellenőrizze a Docker naplózza a naplófájlok könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="b92e4-152">When your application fails to start with your custom Docker image, check the Docker logs in the LogFiles directory.</span></span> <span data-ttu-id="b92e4-153">Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="b92e4-153">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="b92e4-154">A napló a `stdout` és `stderr` a tárolóból, engedélyezni kell a **Docker-tároló naplózási** alatt **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="b92e4-154">To log the `stdout` and `stderr` from your container, you need to enable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Naplózás engedélyezése][8]

![A Kudu segítségével Docker naplók megtekintése][7]

<span data-ttu-id="b92e4-157">Érheti el az SCM helyet **speciális eszközök** a a **Fejlesztőeszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="b92e4-157">You can access the SCM site from **Advanced Tools** in the **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b92e4-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b92e4-158">Next Steps</span></span> ##

<span data-ttu-id="b92e4-159">Az alábbi hivatkozásokból Ismerkedés a Linux webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b92e4-159">Follow the following links to get started with Web App on Linux.</span></span>   

* [<span data-ttu-id="b92e4-160">Bevezetés az Azure-webalkalmazásban Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="b92e4-160">Introduction to Azure Web App on Linux</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="b92e4-161">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="b92e4-161">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](./app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="b92e4-162">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b92e4-162">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<span data-ttu-id="b92e4-163">Kérdések és aggályokat írjon [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="b92e4-163">Post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
