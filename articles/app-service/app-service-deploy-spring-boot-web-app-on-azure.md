---
title: "a rugó rendszerindító alkalmazás toohello Azure App Service aaaDeploy |} Microsoft Docs"
description: "Ez az oktatóanyag hello lépéseket toodeploy hello rugó rendszerindító bevezetés web app tooAzure App Service segítségével a fejlesztők ismerteti."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 69f9c4903fd740125194402cdb4b4db46a1f2773
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-toohello-azure-app-service"></a><span data-ttu-id="8f767-103">A rugó rendszerindító alkalmazás toohello Azure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="8f767-103">Deploy a Spring Boot Application toohello Azure App Service</span></span>

<span data-ttu-id="8f767-104">Hello  **[rugó keretrendszer]**  nyílt forráskódú megoldás, amely segít a vállalati szintű alkalmazásokat Java fejlesztői, és egyik hello több népszerű projektek platformra épül [Rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8f767-104">hello **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications, and one of hello more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="8f767-105">Ez az oktatóanyag bemutatja, ha a hello minta rugó rendszerindító bevezetés webalkalmazás létrehozását és telepítését túl[Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="8f767-105">This tutorial will walk you though creating hello sample Spring Boot Getting Started web app and deploying it too[Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8f767-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f767-106">Prerequisites</span></span>

<span data-ttu-id="8f767-107">A sorrend toocomplete hello lépések ebben az oktatóanyagban toohave hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="8f767-107">In order toocomplete hello steps in this tutorial, you need toohave hello following:</span></span>

* <span data-ttu-id="8f767-108">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="8f767-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="8f767-109">Egy naprakész [Java fejlesztői készlet (JDK)].</span><span class="sxs-lookup"><span data-stu-id="8f767-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="8f767-110">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8f767-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="8f767-111">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="8f767-111">A [Git] client.</span></span>

## <a name="create-hello-spring-boot-getting-started-web-app"></a><span data-ttu-id="8f767-112">Hello rugó rendszerindító bevezetés webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f767-112">Create hello Spring Boot Getting Started web app</span></span>

<span data-ttu-id="8f767-113">hello következő lépésekkel haladhat végig szükséges toocreate rugó rendszerindító egyszerű webalkalmazások és tesztelik azt helyileg hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="8f767-113">hello following steps will walk you through hello steps that are required toocreate a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="8f767-114">Nyisson meg egy parancssort, és hozzon létre egy helyi könyvtár toohold, az alkalmazáshoz, majd a Könyvtárváltás toothat; Példa:</span><span class="sxs-lookup"><span data-stu-id="8f767-114">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="8f767-115">– vagy –</span><span class="sxs-lookup"><span data-stu-id="8f767-115">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="8f767-116">Klónozás hello [rugó rendszerindító bevezetés] mintaprojektet hello könyvtárba, újonnan létrehozott; például:</span><span class="sxs-lookup"><span data-stu-id="8f767-116">Clone hello [Spring Boot Getting Started] sample project into hello directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="8f767-117">Directory befejeződött toohello projekt; módosítása Példa:</span><span class="sxs-lookup"><span data-stu-id="8f767-117">Change directory toohello completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="8f767-118">Build Maven; használatával hello JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="8f767-118">Build hello JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="8f767-119">Hello webalkalmazás létrehozása után módosítsa a könyvtárat toohello JAR-fájlra, és indítsa el a hello webalkalmazás; Példa:</span><span class="sxs-lookup"><span data-stu-id="8f767-119">Once hello web app has been created, change directory toohello JAR file and start hello web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="8f767-120">Keresse meg webböngészővel toohttp://localhost:8080 hello webes alkalmazás tesztelése, vagy használja a következő példa, ha van elérhető curl hello hello szintaxist:</span><span class="sxs-lookup"><span data-stu-id="8f767-120">Test hello web app by browsing toohttp://localhost:8080 using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="8f767-121">A következő üzenet jelenik meg hello kell megjelennie: **hónap rugó rendszerindításból!**</span><span class="sxs-lookup"><span data-stu-id="8f767-121">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Keresse meg a mintaalkalmazás][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="8f767-123">Java hozzon létre egy Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="8f767-123">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="8f767-124">hello lépések végigvezetik Önt hello lépéseket toocreate az Azure Web Apps, Java hello szükséges beállítások konfigurálása, és állítsa be az FTP hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="8f767-124">hello following steps will walk you through hello steps toocreate an Azure Web App, configure hello required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="8f767-125">Keresse meg a toohello [Azure-portálon] , és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="8f767-125">Browse toohello [Azure portal] and log in.</span></span>

1. <span data-ttu-id="8f767-126">Ha jelentkezett be fiókjába a hello Azure-portálon, kattintson a hello menü ikonja **alkalmazásszolgáltatások**:</span><span class="sxs-lookup"><span data-stu-id="8f767-126">Once you have logged into your account on hello Azure portal, click hello menu icon for **App Services**:</span></span>
   
   ![Azure Portal][AZ01]

1. <span data-ttu-id="8f767-128">Ha hello **alkalmazásszolgáltatások** lap is megjelenik, kattintson a **+ Hozzáadás** toocreate egy új App Service.</span><span class="sxs-lookup"><span data-stu-id="8f767-128">When hello **App Services** page is displayed, click **+ Add** toocreate a new App Service.</span></span>

   ![Az App Service létrehozása][AZ02]

1. <span data-ttu-id="8f767-130">Amikor megjelenik a webes alkalmazás sablonok hello listája, hello hello hivatkozásra kattintva alapszintű Microsoft webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8f767-130">When hello list of web app templates is displayed, click hello link for hello basic Microsoft Web App.</span></span>

   ![Webes alkalmazás sablonok][AZ03]

1. <span data-ttu-id="8f767-132">Amikor hello Web App sablon hello adatai lap megjelenik, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8f767-132">When hello information page for hello Web App template is displayed, click **Create**.</span></span>

   ![Webalkalmazás létrehozása][AZ04]

1. <span data-ttu-id="8f767-134">Adjon egyedi nevet a webalkalmazás, és adja meg az esetleges egyéb beállításokat, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8f767-134">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![A webalkalmazás-beállítások létrehozása][AZ05]

1. <span data-ttu-id="8f767-136">A webalkalmazás létrehozása után kattintson a hello ikonjára **alkalmazásszolgáltatások**, majd kattintson az újonnan létrehozott webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="8f767-136">Once your web app has been created, click hello menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![Lista webalkalmazások][AZ06]

1. <span data-ttu-id="8f767-138">Amikor megjelenik a webalkalmazás, adja meg a hello Java-verziót hello lépések használatával:</span><span class="sxs-lookup"><span data-stu-id="8f767-138">When your web app is displayed, specify hello Java version by using hello following steps:</span></span>

   <span data-ttu-id="8f767-139">a.</span><span class="sxs-lookup"><span data-stu-id="8f767-139">a.</span></span> <span data-ttu-id="8f767-140">Kattintson a hello **Alkalmazásbeállítások** menüpont.</span><span class="sxs-lookup"><span data-stu-id="8f767-140">Click hello **Application Settings** menu item.</span></span>

   <span data-ttu-id="8f767-141">b.</span><span class="sxs-lookup"><span data-stu-id="8f767-141">b.</span></span> <span data-ttu-id="8f767-142">Válasszon **Java 8** hello Java-verzió.</span><span class="sxs-lookup"><span data-stu-id="8f767-142">Choose **Java 8** for hello Java version.</span></span>

   <span data-ttu-id="8f767-143">c.</span><span class="sxs-lookup"><span data-stu-id="8f767-143">c.</span></span> <span data-ttu-id="8f767-144">Válasszon **legújabb** hello kisebb Java-verzió.</span><span class="sxs-lookup"><span data-stu-id="8f767-144">Choose **Newest** for hello minor Java version.</span></span>

   <span data-ttu-id="8f767-145">d.</span><span class="sxs-lookup"><span data-stu-id="8f767-145">d.</span></span> <span data-ttu-id="8f767-146">Válasszon **legújabb Tomcat 8.5** hello webes tároló.</span><span class="sxs-lookup"><span data-stu-id="8f767-146">Choose **Newest Tomcat 8.5** for hello web container.</span></span> <span data-ttu-id="8f767-147">(Ez a tároló nem ténylegesen használja; Azure fogja használni a rugó rendszerindító alkalmazás hello tárolót.)</span><span class="sxs-lookup"><span data-stu-id="8f767-147">(This container will not actually be used; Azure will use hello container from your Spring Boot application.)</span></span>

   <span data-ttu-id="8f767-148">e.</span><span class="sxs-lookup"><span data-stu-id="8f767-148">e.</span></span> <span data-ttu-id="8f767-149">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f767-149">Click **Save**.</span></span>

   ![Alkalmazásbeállítások][AZ07]

1. <span data-ttu-id="8f767-151">Adja meg az FTP telepítési hitelesítő adatokat a lépéseket követve hello használatával:</span><span class="sxs-lookup"><span data-stu-id="8f767-151">Specify your FTP deployment credentials by using hello following steps:</span></span>

   <span data-ttu-id="8f767-152">a.</span><span class="sxs-lookup"><span data-stu-id="8f767-152">a.</span></span> <span data-ttu-id="8f767-153">Kattintson a hello **üzembe helyezési hitelesítő adatok** menüpont.</span><span class="sxs-lookup"><span data-stu-id="8f767-153">Click hello **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="8f767-154">b.</span><span class="sxs-lookup"><span data-stu-id="8f767-154">b.</span></span> <span data-ttu-id="8f767-155">Adja meg a felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="8f767-155">Specify your username and password.</span></span>

   <span data-ttu-id="8f767-156">c.</span><span class="sxs-lookup"><span data-stu-id="8f767-156">c.</span></span> <span data-ttu-id="8f767-157">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f767-157">Click **Save**.</span></span>

   ![Adja meg az üzembe helyezési hitelesítő adatok][AZ08]

1. <span data-ttu-id="8f767-159">Az FTP-kiszolgáló kapcsolati adatainak beolvasása hello lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="8f767-159">Retrieve your FTP connection information by using hello following steps:</span></span>

   <span data-ttu-id="8f767-160">a.</span><span class="sxs-lookup"><span data-stu-id="8f767-160">a.</span></span> <span data-ttu-id="8f767-161">Kattintson a hello **üzembe helyezési hitelesítő adatok** menüpont.</span><span class="sxs-lookup"><span data-stu-id="8f767-161">Click hello **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="8f767-162">b.</span><span class="sxs-lookup"><span data-stu-id="8f767-162">b.</span></span> <span data-ttu-id="8f767-163">Másolja a teljes FTP-felhasználónév és az URL-címet, és mentse őket az oktatóprogram következő szakaszában hello.</span><span class="sxs-lookup"><span data-stu-id="8f767-163">Copy your full FTP username and URL and save them for hello next section of this tutorial.</span></span>

   ![FTP URL-CÍMEK és a hitelesítő adatok][AZ09]

## <a name="deploy-your-spring-boot-web-app-tooazure"></a><span data-ttu-id="8f767-165">A rugó rendszerindító web app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="8f767-165">Deploy your Spring Boot web app tooAzure</span></span>

<span data-ttu-id="8f767-166">hello lépésekkel haladhat végig hello lépéseket toodeploy a rugó rendszerindító web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8f767-166">hello following steps will walk you through hello steps toodeploy your Spring Boot web app tooAzure.</span></span>

1. <span data-ttu-id="8f767-167">Nyisson meg egy szövegszerkesztőt, például a Jegyzettömbben Windows és illessze be a következő szöveg egy új dokumentumba hello, majd mentse hello fájlt *web.config*:</span><span class="sxs-lookup"><span data-stu-id="8f767-167">Open a text editor such as Windows Notepad and paste hello following text into a new document, then save hello file as *web.config*:</span></span>
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. <span data-ttu-id="8f767-168">Hello mentése után *web.config* tooyour fájlrendszer, a tooyour web app használatával hello URL-cím, a felhasználónév és a jelszó az ebben az oktatóanyagban szakasza megelőző hello FTP-n keresztül csatlakozniuk.</span><span class="sxs-lookup"><span data-stu-id="8f767-168">After you have saved hello *web.config* file tooyour system, connect tooyour web app via FTP using hello URL, username, and password from hello preceding section of this tutorial.</span></span> <span data-ttu-id="8f767-169">Példa:</span><span class="sxs-lookup"><span data-stu-id="8f767-169">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="8f767-170">Hello távoli könyvtár toohello legfelső szintű mappa módosítása, webalkalmazás (amely */hely/wwwroot*), majd másolja a rugó rendszerindító alkalmazás hello JAR-fájlra, és hello *web.config* a korábbi.</span><span class="sxs-lookup"><span data-stu-id="8f767-170">Change hello remote directory toohello root folder of your web app, (which is at */site/wwwroot*), then copy hello JAR file from your Spring Boot application and hello *web.config* from earlier.</span></span> <span data-ttu-id="8f767-171">Példa:</span><span class="sxs-lookup"><span data-stu-id="8f767-171">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="8f767-172">Miután telepítette a JAR és *web.config* fájlok tooyour web app alkalmazásban toorestart a webalkalmazás hello Azure-portál használatával van szüksége:</span><span class="sxs-lookup"><span data-stu-id="8f767-172">After you have deployed your JAR and *web.config* files tooyour web app, you need toorestart your web app using hello Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="8f767-173">Hello webes alkalmazás tesztelése a tooyour webes alkalmazás URL-cím, egy webböngésző segítségével keresse meg azt, vagy használja a következő példa, ha van elérhető curl hello hello szintaxist:</span><span class="sxs-lookup"><span data-stu-id="8f767-173">Test hello web app by browsing tooyour web app's URL using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="8f767-174">A következő üzenet jelenik meg hello kell megjelennie: **hónap rugó rendszerindításból!**</span><span class="sxs-lookup"><span data-stu-id="8f767-174">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Keresse meg a mintaalkalmazás][SB02]

## <a name="next-steps"></a><span data-ttu-id="8f767-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f767-176">Next steps</span></span>

<span data-ttu-id="8f767-177">Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="8f767-177">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="8f767-178">A rugó rendszerindító alkalmazás Linux telepítése hello Azure Tárolószolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8f767-178">Deploy a Spring Boot Application on Linux in hello Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [<span data-ttu-id="8f767-179">A rugó rendszerindító alkalmazás Kubernetes gazdagépfürtökön hello Azure Tárolószolgáltatás a központi telepítése</span><span class="sxs-lookup"><span data-stu-id="8f767-179">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="8f767-180">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="8f767-180">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="8f767-181">Depoying web apps tooAzure FTP használatával kapcsolatos további információkért lásd: [telepítheti az alkalmazást tooAzure használatával az FTP/S App Service].</span><span class="sxs-lookup"><span data-stu-id="8f767-181">For additional information about depoying web apps tooAzure using FTP, see [Deploy your app tooAzure App Service using FTP/S].</span></span>

<span data-ttu-id="8f767-182">További hello rugó rendszerindító mintaprojektet kapcsolatos további információkért lásd: [rugó rendszerindító bevezetés].</span><span class="sxs-lookup"><span data-stu-id="8f767-182">For further details about hello Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="8f767-183">Első lépések a saját rugó rendszerindító alkalmazásokkal kapcsolatban lásd: hello **rugó Initializr** https://start.spring.io/ címen.</span><span class="sxs-lookup"><span data-stu-id="8f767-183">For help with getting started with your own Spring Boot applications, see hello **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="8f767-184">A webalkalmazás további beállítások konfigurálásával kapcsolatos további információkért lásd: [webes alkalmazások konfigurálása az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="8f767-184">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[webes alkalmazások konfigurálása az Azure App Service]: /azure/app-service-web/web-sites-configure
[telepítheti az alkalmazást tooAzure használatával az FTP/S App Service]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító bevezetés]: https://github.com/spring-guides/gs-spring-boot
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
