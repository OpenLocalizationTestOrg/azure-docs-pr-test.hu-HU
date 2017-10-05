---
title: "Az Azure App Service rugó rendszerindító alkalmazást központilag |} Microsoft Docs"
description: "Ez az oktatóanyag ismerteti a lépéseit fejlesztők számára, hogy a rendszerindító bevezetés rugó webalkalmazás telepítése az Azure App Service szolgáltatásban."
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
ms.openlocfilehash: 0c388862d927a1492745832225c686670c071f86
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a><span data-ttu-id="3e593-103">Spring Boot-alkalmazás üzembe helyezése az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="3e593-103">Deploy a Spring Boot Application to the Azure App Service</span></span>

<span data-ttu-id="3e593-104">A  **[rugó keretrendszer]**  nyílt forráskódú megoldás, amely segít a vállalati szintű alkalmazásokat Java fejlesztői, és a további népszerű platformra épülő projektek egyik [ Rendszerindító érintkező], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3e593-104">The **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications, and one of the more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="3e593-105">Ez az oktatóanyag bemutatja, ha a rendszerindító bevezetés rugó webes mintaalkalmazás létrehozását és telepítését a [Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="3e593-105">This tutorial will walk you though creating the sample Spring Boot Getting Started web app and deploying it to [Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3e593-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3e593-106">Prerequisites</span></span>

<span data-ttu-id="3e593-107">Az oktatóanyagban szereplő lépések végrehajtásához kell rendelkeznie a következő:</span><span class="sxs-lookup"><span data-stu-id="3e593-107">In order to complete the steps in this tutorial, you need to have the following:</span></span>

* <span data-ttu-id="3e593-108">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="3e593-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="3e593-109">Egy naprakész [Java fejlesztői készlet (JDK)].</span><span class="sxs-lookup"><span data-stu-id="3e593-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="3e593-110">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3e593-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="3e593-111">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3e593-111">A [Git] client.</span></span>

## <a name="create-the-spring-boot-getting-started-web-app"></a><span data-ttu-id="3e593-112">A rugó rendszerindító első lépések a webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e593-112">Create the Spring Boot Getting Started web app</span></span>

<span data-ttu-id="3e593-113">A következő lépésekkel haladhat végig hozzon létre egy egyszerű rugó rendszerindító webalkalmazást, és helyben tesztelheti a lépéseit.</span><span class="sxs-lookup"><span data-stu-id="3e593-113">The following steps will walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="3e593-114">Nyisson meg egy parancssort, és hozzon létre egy helyi könyvtárat az alkalmazás tárolására, és módosítsa a könyvtárhoz; Példa:</span><span class="sxs-lookup"><span data-stu-id="3e593-114">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="3e593-115">– vagy –</span><span class="sxs-lookup"><span data-stu-id="3e593-115">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="3e593-116">Klónozott a [rugó rendszerindító bevezetés] mintaprojektet a könyvtárba, újonnan létrehozott; például:</span><span class="sxs-lookup"><span data-stu-id="3e593-116">Clone the [Spring Boot Getting Started] sample project into the directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="3e593-117">Módosítsa a könyvtárat a befejezett projekthez; Példa:</span><span class="sxs-lookup"><span data-stu-id="3e593-117">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="3e593-118">Build Maven; használatával JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="3e593-118">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="3e593-119">A webalkalmazás létrehozása után módosítsa a könyvtárat a JAR-fájlra, és indítsa el a webalkalmazást; Példa:</span><span class="sxs-lookup"><span data-stu-id="3e593-119">Once the web app has been created, change directory to the JAR file and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="3e593-120">A webes alkalmazás tesztelése a webböngésző segítségével 8080 tallózással, vagy használja a szintaxist, az alábbi példához hasonló, ha a curl érhető el:</span><span class="sxs-lookup"><span data-stu-id="3e593-120">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="3e593-121">A következő üzenet jelenik meg: **hónap rugó rendszerindításból!**</span><span class="sxs-lookup"><span data-stu-id="3e593-121">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Keresse meg a mintaalkalmazás][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="3e593-123">Java hozzon létre egy Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="3e593-123">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="3e593-124">A következő lépésekkel haladhat végig a lépéseket az Azure-webalkalmazás létrehozása, konfigurálja a szükséges beállításokat, Java és az FTP-hitelesítő adatok beállítása.</span><span class="sxs-lookup"><span data-stu-id="3e593-124">The following steps will walk you through the steps to create an Azure Web App, configure the required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="3e593-125">Keresse meg a [Azure-portálon] , és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="3e593-125">Browse to the [Azure portal] and log in.</span></span>

1. <span data-ttu-id="3e593-126">Miután jelentkezett, a fiókot az Azure portálon, kattintson az menü **alkalmazásszolgáltatások**:</span><span class="sxs-lookup"><span data-stu-id="3e593-126">Once you have logged into your account on the Azure portal, click the menu icon for **App Services**:</span></span>
   
   ![Azure Portal][AZ01]

1. <span data-ttu-id="3e593-128">Ha a **alkalmazásszolgáltatások** lap is megjelenik, kattintson a **+ Hozzáadás** egy új App Service létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3e593-128">When the **App Services** page is displayed, click **+ Add** to create a new App Service.</span></span>

   ![Az App Service létrehozása][AZ02]

1. <span data-ttu-id="3e593-130">Amikor megjelenik a webes alkalmazás sablonok listáján, a Microsoft alapszintű webalkalmazást a hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="3e593-130">When the list of web app templates is displayed, click the link for the basic Microsoft Web App.</span></span>

   ![Webes alkalmazás sablonok][AZ03]

1. <span data-ttu-id="3e593-132">Ha az adatokat a Web App sablon megjelenik, kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3e593-132">When the information page for the Web App template is displayed, click **Create**.</span></span>

   ![Webalkalmazás létrehozása][AZ04]

1. <span data-ttu-id="3e593-134">Adjon egyedi nevet a webalkalmazás, és adja meg az esetleges egyéb beállításokat, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3e593-134">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![A webalkalmazás-beállítások létrehozása][AZ05]

1. <span data-ttu-id="3e593-136">A webalkalmazás létrehozása után kattintson az menü **alkalmazásszolgáltatások**, majd kattintson az újonnan létrehozott webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="3e593-136">Once your web app has been created, click the menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![Lista webalkalmazások][AZ06]

1. <span data-ttu-id="3e593-138">Amikor megjelenik a webalkalmazás, adja meg a Java-verziót a következő lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="3e593-138">When your web app is displayed, specify the Java version by using the following steps:</span></span>

   <span data-ttu-id="3e593-139">a.</span><span class="sxs-lookup"><span data-stu-id="3e593-139">a.</span></span> <span data-ttu-id="3e593-140">Kattintson a **Alkalmazásbeállítások** menüpont.</span><span class="sxs-lookup"><span data-stu-id="3e593-140">Click the **Application Settings** menu item.</span></span>

   <span data-ttu-id="3e593-141">b.</span><span class="sxs-lookup"><span data-stu-id="3e593-141">b.</span></span> <span data-ttu-id="3e593-142">Válasszon **Java 8** a Java-verzió.</span><span class="sxs-lookup"><span data-stu-id="3e593-142">Choose **Java 8** for the Java version.</span></span>

   <span data-ttu-id="3e593-143">c.</span><span class="sxs-lookup"><span data-stu-id="3e593-143">c.</span></span> <span data-ttu-id="3e593-144">Válasszon **legújabb** a kisebb Java-verzió.</span><span class="sxs-lookup"><span data-stu-id="3e593-144">Choose **Newest** for the minor Java version.</span></span>

   <span data-ttu-id="3e593-145">d.</span><span class="sxs-lookup"><span data-stu-id="3e593-145">d.</span></span> <span data-ttu-id="3e593-146">Válasszon **legújabb Tomcat 8.5** a webes tároló.</span><span class="sxs-lookup"><span data-stu-id="3e593-146">Choose **Newest Tomcat 8.5** for the web container.</span></span> <span data-ttu-id="3e593-147">(Ez a tároló nem ténylegesen használja; Azure fogja használni a rugó rendszerindító alkalmazás a tárolót.)</span><span class="sxs-lookup"><span data-stu-id="3e593-147">(This container will not actually be used; Azure will use the container from your Spring Boot application.)</span></span>

   <span data-ttu-id="3e593-148">e.</span><span class="sxs-lookup"><span data-stu-id="3e593-148">e.</span></span> <span data-ttu-id="3e593-149">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3e593-149">Click **Save**.</span></span>

   ![Alkalmazásbeállítások][AZ07]

1. <span data-ttu-id="3e593-151">Adja meg az FTP telepítési hitelesítő adatokat az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="3e593-151">Specify your FTP deployment credentials by using the following steps:</span></span>

   <span data-ttu-id="3e593-152">a.</span><span class="sxs-lookup"><span data-stu-id="3e593-152">a.</span></span> <span data-ttu-id="3e593-153">Kattintson a **üzembe helyezési hitelesítő adatok** menüpont.</span><span class="sxs-lookup"><span data-stu-id="3e593-153">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="3e593-154">b.</span><span class="sxs-lookup"><span data-stu-id="3e593-154">b.</span></span> <span data-ttu-id="3e593-155">Adja meg a felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3e593-155">Specify your username and password.</span></span>

   <span data-ttu-id="3e593-156">c.</span><span class="sxs-lookup"><span data-stu-id="3e593-156">c.</span></span> <span data-ttu-id="3e593-157">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3e593-157">Click **Save**.</span></span>

   ![Adja meg az üzembe helyezési hitelesítő adatok][AZ08]

1. <span data-ttu-id="3e593-159">Az FTP-kiszolgáló kapcsolati adatainak beolvasása a következő lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="3e593-159">Retrieve your FTP connection information by using the following steps:</span></span>

   <span data-ttu-id="3e593-160">a.</span><span class="sxs-lookup"><span data-stu-id="3e593-160">a.</span></span> <span data-ttu-id="3e593-161">Kattintson a **üzembe helyezési hitelesítő adatok** menüpont.</span><span class="sxs-lookup"><span data-stu-id="3e593-161">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="3e593-162">b.</span><span class="sxs-lookup"><span data-stu-id="3e593-162">b.</span></span> <span data-ttu-id="3e593-163">Másolja a teljes FTP-felhasználónév és az URL-címet, és mentse őket az oktatóanyag következő szakasza.</span><span class="sxs-lookup"><span data-stu-id="3e593-163">Copy your full FTP username and URL and save them for the next section of this tutorial.</span></span>

   ![FTP URL-CÍMEK és a hitelesítő adatok][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a><span data-ttu-id="3e593-165">A rugó rendszerindító webalkalmazás telepítése az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="3e593-165">Deploy your Spring Boot web app to Azure</span></span>

<span data-ttu-id="3e593-166">A következő lépésekkel haladhat végig a lépéseket a rugó rendszerindító webalkalmazás telepítése az Azure.</span><span class="sxs-lookup"><span data-stu-id="3e593-166">The following steps will walk you through the steps to deploy your Spring Boot web app to Azure.</span></span>

1. <span data-ttu-id="3e593-167">Nyisson meg egy szövegszerkesztőt, például a Jegyzettömbben Windows és a következő szöveg beillesztése egy új dokumentumot, majd mentse a fájlt *web.config*:</span><span class="sxs-lookup"><span data-stu-id="3e593-167">Open a text editor such as Windows Notepad and paste the following text into a new document, then save the file as *web.config*:</span></span>
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

1. <span data-ttu-id="3e593-168">Mentés után a *web.config* fájlt a rendszer, a webes alkalmazás URL-címet, felhasználónevet és jelszót használó Ez az oktatóanyag előző szakaszából FTP-n keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="3e593-168">After you have saved the *web.config* file to your system, connect to your web app via FTP using the URL, username, and password from the preceding section of this tutorial.</span></span> <span data-ttu-id="3e593-169">Példa:</span><span class="sxs-lookup"><span data-stu-id="3e593-169">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="3e593-170">A távoli könyvtár módosítsa a webalkalmazás gyökérmappájában (amely */hely/wwwroot*), majd másolja a JAR-fájlra a rugó rendszerindító alkalmazás és a *web.config* a korábbi.</span><span class="sxs-lookup"><span data-stu-id="3e593-170">Change the remote directory to the root folder of your web app, (which is at */site/wwwroot*), then copy the JAR file from your Spring Boot application and the *web.config* from earlier.</span></span> <span data-ttu-id="3e593-171">Példa:</span><span class="sxs-lookup"><span data-stu-id="3e593-171">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="3e593-172">Miután telepítette a JAR és *web.config* a webalkalmazás fájlokat, újra kell indítania a webalkalmazás az Azure portál használatával:</span><span class="sxs-lookup"><span data-stu-id="3e593-172">After you have deployed your JAR and *web.config* files to your web app, you need to restart your web app using the Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="3e593-173">A webes alkalmazás tesztelése a keresse meg a webalkalmazás URL-címre, egy webböngésző segítségével, vagy használja a szintaxist, az alábbi példához hasonló, ha a curl érhető el:</span><span class="sxs-lookup"><span data-stu-id="3e593-173">Test the web app by browsing to your web app's URL using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="3e593-174">A következő üzenet jelenik meg: **hónap rugó rendszerindításból!**</span><span class="sxs-lookup"><span data-stu-id="3e593-174">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Keresse meg a mintaalkalmazás][SB02]

## <a name="next-steps"></a><span data-ttu-id="3e593-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e593-176">Next steps</span></span>

<span data-ttu-id="3e593-177">Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="3e593-177">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="3e593-178">A rugó rendszerindító alkalmazás Linux az az Azure Tárolószolgáltatásban központi telepítése</span><span class="sxs-lookup"><span data-stu-id="3e593-178">Deploy a Spring Boot Application on Linux in the Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [<span data-ttu-id="3e593-179">A rugó rendszerindító alkalmazás az az Azure Tárolószolgáltatásban Kubernetes fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="3e593-179">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="3e593-180">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].</span><span class="sxs-lookup"><span data-stu-id="3e593-180">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="3e593-181">További információt az Azure-bA depoying webalkalmazások használata FTP, lásd: [telepítse az alkalmazást az Azure App Service segítségével FTP/S].</span><span class="sxs-lookup"><span data-stu-id="3e593-181">For additional information about depoying web apps to Azure using FTP, see [Deploy your app to Azure App Service using FTP/S].</span></span>

<span data-ttu-id="3e593-182">A rugó rendszerindító mintaprojektet további részleteiért lásd: [rugó rendszerindító bevezetés].</span><span class="sxs-lookup"><span data-stu-id="3e593-182">For further details about the Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="3e593-183">Első lépések a saját rugó rendszerindító alkalmazásokkal kapcsolatban lásd: a **rugó Initializr** https://start.spring.io/ címen.</span><span class="sxs-lookup"><span data-stu-id="3e593-183">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="3e593-184">A webalkalmazás további beállítások konfigurálásával kapcsolatos további információkért lásd: [webes alkalmazások konfigurálása az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="3e593-184">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

<span data-ttu-id="3e593-185">[Azure App Service]: https://azure.microsoft.com/services/app-service/</span><span class="sxs-lookup"><span data-stu-id="3e593-185">[Azure App Service]: https://azure.microsoft.com/services/app-service/</span></span>
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
<span data-ttu-id="3e593-186">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="3e593-186">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="3e593-187">[Azure-portálon]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="3e593-187">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="3e593-188">[webes alkalmazások konfigurálása az Azure App Service]: /azure/app-service-web/web-sites-configure</span><span class="sxs-lookup"><span data-stu-id="3e593-188">[Configure web apps in Azure App Service]: /azure/app-service-web/web-sites-configure</span></span>
<span data-ttu-id="3e593-189">[telepítse az alkalmazást az Azure App Service segítségével FTP/S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp</span><span class="sxs-lookup"><span data-stu-id="3e593-189">[Deploy your app to Azure App Service using FTP/S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp</span></span>
<span data-ttu-id="3e593-190">[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="3e593-190">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="3e593-191">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="3e593-191">[Git]: https://github.com/</span></span>
<span data-ttu-id="3e593-192">[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span><span class="sxs-lookup"><span data-stu-id="3e593-192">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span></span>
<span data-ttu-id="3e593-193">[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="3e593-193">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="3e593-194">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="3e593-194">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="3e593-195">[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="3e593-195">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="3e593-196">[ Rendszerindító érintkező]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="3e593-196">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="3e593-197">[rugó rendszerindító bevezetés]: https://github.com/spring-guides/gs-spring-boot</span><span class="sxs-lookup"><span data-stu-id="3e593-197">[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot</span></span>
<span data-ttu-id="3e593-198">[rugó keretrendszer]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="3e593-198">[Spring Framework]: https://spring.io/</span></span>

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
