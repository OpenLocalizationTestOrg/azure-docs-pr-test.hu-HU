---
title: "az Azure Web Apps Linux rendszeren egyéni Docker kép aaaHow toouse |} Microsoft Docs"
description: "Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára."
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
ms.openlocfilehash: 8853095d0e1067cfea4297bbd23b622fe4a0d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a><span data-ttu-id="161db-104">Az Azure Web Apps Linux rendszeren egyéni Docker-lemezkép használatával</span><span class="sxs-lookup"><span data-stu-id="161db-104">Using a custom Docker image for Azure Web App on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="161db-105">App Service biztosít előre definiált alkalmazás verem Linux-támogatással rendelkező különböző verziókat, például a PHP 7.0 és a Node.js 4.5.</span><span class="sxs-lookup"><span data-stu-id="161db-105">App Service provides pre-defined application stacks on Linux with support for specific versions, such as PHP 7.0 and Node.js 4.5.</span></span> <span data-ttu-id="161db-106">App Service Linux az Docker-tároló toohost ezek előzetesen elkészített alkalmazás verem.</span><span class="sxs-lookup"><span data-stu-id="161db-106">App Service on Linux uses Docker containers toohost these pre-built application stacks.</span></span> <span data-ttu-id="161db-107">A Docker egyéni rendszerkép toodeploy a webes alkalmazás tooan alkalmazáscsoportokat, amely már nincs definiálva az Azure-ban is használja.</span><span class="sxs-lookup"><span data-stu-id="161db-107">You can also use a custom Docker image toodeploy your web app tooan application stack that is not already defined in Azure.</span></span> <span data-ttu-id="161db-108">Egyéni Docker lemezképek alábbiakon tárolható vagy egy nyilvános vagy privát Docker-tárházat.</span><span class="sxs-lookup"><span data-stu-id="161db-108">Custom Docker images can be hosted on either a public or private Docker repository.</span></span>


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a><span data-ttu-id="161db-109">Útmutató: a webalkalmazáshoz tartozó egyéni Docker-lemezkép beállítása</span><span class="sxs-lookup"><span data-stu-id="161db-109">How to: set a custom Docker image for a web app</span></span>
<span data-ttu-id="161db-110">Hello egyéni Docker lemezképet mindkét új és meglévő Web apps állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="161db-110">You can set hello custom Docker image for both new and existing webs apps.</span></span> <span data-ttu-id="161db-111">Amikor hoz létre egy webalkalmazást Linux rendszeren a hello [Azure-portálon](https://portal.azure.com/#create/Microsoft.AppSvcLinux), kattintson a **konfigurálása tároló** tooset egyéni Docker-lemezkép:</span><span class="sxs-lookup"><span data-stu-id="161db-111">When you create a web app on Linux in hello [Azure portal](https://portal.azure.com/#create/Microsoft.AppSvcLinux), click **Configure container** tooset a custom Docker image:</span></span>

![Az új webalkalmazáshoz Linux rendszeren egyéni Docker kép][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a><span data-ttu-id="161db-113">Útmutató: a Docker Hub egyéni Docker kép használata</span><span class="sxs-lookup"><span data-stu-id="161db-113">How to: use a custom Docker image from Docker Hub</span></span> ##
<span data-ttu-id="161db-114">a Docker Hub egyéni Docker-lemezkép toouse:</span><span class="sxs-lookup"><span data-stu-id="161db-114">toouse a custom Docker image from Docker Hub:</span></span>

1. <span data-ttu-id="161db-115">A hello [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.</span><span class="sxs-lookup"><span data-stu-id="161db-115">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="161db-116">Válassza ki **Docker Hub** hello, **lemezképforrás**, ezután kattintson **nyilvános** vagy **titkos** és típus hello **lemezkép és nem kötelező címke neve**, például a `node:4.5`.</span><span class="sxs-lookup"><span data-stu-id="161db-116">Select **Docker Hub** as hello **Image source**, then click either **Public** or **Private** and type hello **Image and optional tag name**, such as `node:4.5`.</span></span> <span data-ttu-id="161db-117">Hello **indítási parancs** beállítása automatikusan alapuló hello Docker képfájl meghatározott, de a saját parancsok állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="161db-117">hello **Startup command** is set automatically based on what is defined in hello Docker image file, but you can set your own commands.</span></span>  

    ![Kép: a nyilvános tárház Docker központ konfigurálása][2]

    <span data-ttu-id="161db-119">A lemezkép egy titkos tárházból esetén szükség tooenter hello Docker Hub hitelesítő adatokat (**bejelentkezési felhasználónevének** és **jelszó**) hello titkos Docker Hub tárház.</span><span class="sxs-lookup"><span data-stu-id="161db-119">When your image is from a private repository, you also need tooenter hello Docker Hub credentials as (**Login username** and **Password**) for hello private Docker Hub repository.</span></span>

    ![Kép: Docker Hub személyes adattár konfigurálása][3]

3. <span data-ttu-id="161db-121">Hello tároló beállítását követően kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="161db-121">After you have configured hello container, click **Save**.</span></span>

## <a name="how-toouse-a-docker-image-from-a-private-image-registry"></a><span data-ttu-id="161db-122">Hogyan toouse egy Docker titkos kép beállításjegyzékbeli kép</span><span class="sxs-lookup"><span data-stu-id="161db-122">How toouse a Docker image from a private image registry</span></span> ##
<span data-ttu-id="161db-123">a titkos kép beállításjegyzékből egyéni Docker-lemezkép toouse:</span><span class="sxs-lookup"><span data-stu-id="161db-123">toouse a custom Docker image from a private image registry:</span></span>

1. <span data-ttu-id="161db-124">A hello [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.</span><span class="sxs-lookup"><span data-stu-id="161db-124">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="161db-125">Kattintson a **titkos beállításjegyzék** , hello **lemezképforrás**.</span><span class="sxs-lookup"><span data-stu-id="161db-125">Click **Private registry** as hello **Image source**.</span></span> <span data-ttu-id="161db-126">Adja meg a hello **kép és a nem kötelező címke neve**, **URL-címe** hello titkos rendszerleíró hello hitelesítő adatokkal együtt (**bejelentkezési felhasználónevének** és **jelszó** ).</span><span class="sxs-lookup"><span data-stu-id="161db-126">Enter hello **Image and optional tag name**, **Server URL** for hello private registry, along with hello credentials (**Login username** and **Password**).</span></span> <span data-ttu-id="161db-127">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="161db-127">Click **Save**.</span></span>

    ![Docker kép titkos beállításjegyzékből konfigurálása][4]


## <a name="how-to-set-hello-port-used-by-your-docker-image"></a><span data-ttu-id="161db-129">Útmutató: a Docker-lemezkép által használt hello port beállítása</span><span class="sxs-lookup"><span data-stu-id="161db-129">How to: set hello port used by your Docker image</span></span> ##

<span data-ttu-id="161db-130">A webalkalmazás egyéni Docker-lemezkép használatakor hello használhatja `WEBSITES_PORT` a Dockerfile, amely lekérdezi hozzá toohello létrehozott tároló környezeti változót.</span><span class="sxs-lookup"><span data-stu-id="161db-130">When you use a custom Docker image for your web app, you can use hello `WEBSITES_PORT` environment variable in your Dockerfile, which gets added toohello generated container.</span></span> <span data-ttu-id="161db-131">Vegye figyelembe a következő példa egy docker-fájl egy Ruby alkalmazáshoz hello:</span><span class="sxs-lookup"><span data-stu-id="161db-131">Consider hello following example of a docker file for a Ruby application:</span></span>

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

<span data-ttu-id="161db-132">Utolsó sora hello parancs megtekintheti hello WEBSITES_PORT környezeti változó átadása futásidőben.</span><span class="sxs-lookup"><span data-stu-id="161db-132">On last line of hello command, you can see that hello WEBSITES_PORT environment variable is passed at runtime.</span></span> <span data-ttu-id="161db-133">Ne feledje, hogy a kis-és nagybetűk parancsokban számít.</span><span class="sxs-lookup"><span data-stu-id="161db-133">Remember that casing matters in commands.</span></span>

<span data-ttu-id="161db-134">Hello platform használta korábban `PORT` app, azt tervezi, toodeprecate hello használata az alkalmazás, beállítás és toousing áthelyezése `WEBSITES_PORT` kizárólag.</span><span class="sxs-lookup"><span data-stu-id="161db-134">Previously hello platform was using `PORT` app setting, we are planning toodeprecate hello use this app setting and move toousing `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="161db-135">Ha valaki más beépített meglévő Docker lemezképet használja, szükség lehet hello alkalmazás toospecify 80-as portra.</span><span class="sxs-lookup"><span data-stu-id="161db-135">When you use an existing Docker image built by someone else, you may need toospecify a port other than port 80 for hello application.</span></span> <span data-ttu-id="161db-136">tooconfigure hello port, nevű beállításával alkalmazás hozzáadása `WEBSITES_PORT` hello értékkel alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="161db-136">tooconfigure hello port, add an application setting named `WEBSITES_PORT` with hello value as shown below:</span></span>

![Egyéni Docker-lemezképet alkalmazás portbeállítást konfigurálása][6]

## <a name="how-to-set-hello-startup-time-for-your-docker-image"></a><span data-ttu-id="161db-138">Útmutató: a Docker kép hello indítási idő beállítása</span><span class="sxs-lookup"><span data-stu-id="161db-138">How to: set hello startup time for your Docker image</span></span> ##

<span data-ttu-id="161db-139">Alapértelmezés szerint a tároló nem indul 230 másodperc, mielőtt hello platform újraindítja a tárolót.</span><span class="sxs-lookup"><span data-stu-id="161db-139">By default, if your container does not start before 230 seconds, hello platform will restart your container.</span></span> <span data-ttu-id="161db-140">A Docker egyéni rendszerkép 230 másodpercnél indítja el, ha használható hello `WEBSITES_CONTAINER_START_TIME_LIMIT` másodpercben app beállítása, a beállításnak hello értéke, ez lehetővé teszi a hello platform tartsa a tárolóban fut, akkor az újraindítás előtt.</span><span class="sxs-lookup"><span data-stu-id="161db-140">If your custom Docker image starts in more than 230 seconds, you can use hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting, hello value for this setting is in seconds, this will allow hello platform keep your container running before restarting it.</span></span> <span data-ttu-id="161db-141">hello alapértelmezett értéke 230 másodperc, és hello maximálisan megengedett értéke 600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="161db-141">hello default value is 230 seconds, and hello max allowed value is 600 seconds.</span></span>

## <a name="how-to-unmount-hello-platform-provided-storage"></a><span data-ttu-id="161db-142">Hogyan: hello megadott platform tárolási leválasztása</span><span class="sxs-lookup"><span data-stu-id="161db-142">How to: unmount hello platform provided storage</span></span> ##

<span data-ttu-id="161db-143">Alapértelmezés szerint hello platform fogja csatlakoztatni egy állandó tároló megosztás toohello `\home\` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="161db-143">By default, hello platform will mount a persistent storage share toohello `\home\` directory.</span></span> <span data-ttu-id="161db-144">Ha a tároló-lemezkép nem kell egy állandó megosztást, letilthatja, által hello beállítása, hogy a tároló csatlakoztatása `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app beállítás túl`false`.</span><span class="sxs-lookup"><span data-stu-id="161db-144">If your container image does not need a persistent share, you can disable mounting that storage by setting hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting too`false`.</span></span> <span data-ttu-id="161db-145">Továbbra is kapnak toothat tároló elérése hello scm helyről, valamint minden Docker-naplók (Ha engedélyezve van), a rendszer úgy tünteti toohello hello platform által generált naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="161db-145">You will still have access toothat storage from hello scm site, and all Docker logs (if enabled) will be written toohello log files generated by hello platform.</span></span>

## <a name="how-to-switch-back-toousing-a-built-in-image"></a><span data-ttu-id="161db-146">Útmutató: biztonsági toousing beépített kép kapcsoló</span><span class="sxs-lookup"><span data-stu-id="161db-146">How to: Switch back toousing a built-in image</span></span> ##

<span data-ttu-id="161db-147">egy egyéni lemezkép toousing beépített lemezkép használatával tooswitch:</span><span class="sxs-lookup"><span data-stu-id="161db-147">tooswitch from using a custom image toousing a built-in image:</span></span>

1. <span data-ttu-id="161db-148">A hello [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **App Service**.</span><span class="sxs-lookup"><span data-stu-id="161db-148">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **App Service**.</span></span>

2. <span data-ttu-id="161db-149">Válassza ki a **futásidejű verem** toouse hello beépített lemezképhez, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="161db-149">Select your **Runtime Stack** toouse for hello built-in image, then click **Save**.</span></span> 

![Kép: a beépített Docker konfigurálása][5]


## <a name="troubleshooting"></a><span data-ttu-id="161db-151">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="161db-151">Troubleshooting</span></span> ##

<span data-ttu-id="161db-152">Ha az alkalmazás nem sikerül toostart az egyéni Docker-lemezképre, ellenőrizze a Docker bejelentkezik hello naplófájlok directory hello.</span><span class="sxs-lookup"><span data-stu-id="161db-152">When your application fails toostart with your custom Docker image, check hello Docker logs in hello LogFiles directory.</span></span> <span data-ttu-id="161db-153">Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="161db-153">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="161db-154">toolog hello `stdout` és `stderr` a tárolóból tooenable kell **Docker-tároló naplózási** alatt **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="161db-154">toolog hello `stdout` and `stderr` from your container, you need tooenable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Naplózás engedélyezése][8]

![A Kudu tooview Docker-naplók segítségével][7]

<span data-ttu-id="161db-157">Van-e hozzáférési hello SCM helyet **speciális eszközök** a hello **Fejlesztőeszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="161db-157">You can access hello SCM site from **Advanced Tools** in hello **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="161db-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="161db-158">Next Steps</span></span> ##

<span data-ttu-id="161db-159">Hajtsa végre a következő hivatkozások tooget Linux webalkalmazás használatába hello.</span><span class="sxs-lookup"><span data-stu-id="161db-159">Follow hello following links tooget started with Web App on Linux.</span></span>   

* [<span data-ttu-id="161db-160">Bevezetés tooAzure webalkalmazás Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="161db-160">Introduction tooAzure Web App on Linux</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="161db-161">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="161db-161">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](./app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="161db-162">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="161db-162">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<span data-ttu-id="161db-163">Kérdések és aggályokat írjon [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="161db-163">Post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
