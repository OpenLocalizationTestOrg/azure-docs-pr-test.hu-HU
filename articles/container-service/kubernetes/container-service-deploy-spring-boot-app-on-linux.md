---
title: "az Azure Tárolószolgáltatásban Linux rugó rendszerindító webalkalmazás aaaDeploy |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan azonban hello lépéseket toodeploy egy rugó rendszerindító alkalmazásra, amely Linux webes alkalmazás a Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 2c44be1c7f66a38f48239001f0be9e90c7e6edef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-hello-azure-container-service"></a><span data-ttu-id="823d2-103">Az Azure Tárolószolgáltatás hello Linux rugó rendszerindító-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="823d2-103">Deploy a Spring Boot application on Linux in hello Azure Container Service</span></span>

<span data-ttu-id="823d2-104">Hello  **[rugó keretrendszer]**  egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="823d2-104">hello **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="823d2-105">Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="823d2-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="823d2-106">**[Docker]**  van nyílt forráskódú megoldások, amelyek segítségével a fejlesztők automatizálásához hello telepítési méretezés és a tárolók a futó alkalmazások kezelését.</span><span class="sxs-lookup"><span data-stu-id="823d2-106">**[Docker]** is open-source solutions that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="823d2-107">Ez az oktatóanyag bemutatja, hogyan használja a Docker toodevelop és rugó rendszerindító alkalmazás tooa Linux-állomást telepíthet hello [Azure tároló szolgáltatás (ACS)].</span><span class="sxs-lookup"><span data-stu-id="823d2-107">This tutorial walks you through using Docker toodevelop and deploy a Spring Boot application tooa Linux host in hello [Azure Container Service (ACS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="823d2-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="823d2-108">Prerequisites</span></span>

<span data-ttu-id="823d2-109">A sorrend toocomplete hello lépések ebben az oktatóanyagban kell toohave hello a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="823d2-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="823d2-110">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="823d2-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="823d2-111">Hello [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="823d2-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="823d2-112">Egy naprakész [Java fejlesztői készlet (JDK)].</span><span class="sxs-lookup"><span data-stu-id="823d2-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="823d2-113">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="823d2-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="823d2-114">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="823d2-114">A [Git] client.</span></span>
* <span data-ttu-id="823d2-115">A [Docker] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="823d2-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="823d2-116">Ez az oktatóanyag toohello virtualizálási követelményeinek miatt nem lépések hello ebben a cikkben; virtuális gépen a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.</span><span class="sxs-lookup"><span data-stu-id="823d2-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="823d2-117">A webalkalmazás első lépések a Docker hello rugó rendszerindító létrehozása</span><span class="sxs-lookup"><span data-stu-id="823d2-117">Create hello Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="823d2-118">hello következő lépések végigvezetik szükséges toocreate rugó rendszerindító egyszerű webalkalmazások és tesztelik azt helyileg hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="823d2-118">hello following steps walk you through hello steps that are required toocreate a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="823d2-119">Nyisson meg egy parancssort, és hozzon létre egy helyi könyvtár toohold, az alkalmazáshoz, majd a Könyvtárváltás toothat; Példa:</span><span class="sxs-lookup"><span data-stu-id="823d2-119">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="823d2-120">– vagy –</span><span class="sxs-lookup"><span data-stu-id="823d2-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="823d2-121">Klónozás hello [rugó rendszerindító a Docker bevezetés] mintaprojektet könyvtárba, amely hello létrehozott; például:</span><span class="sxs-lookup"><span data-stu-id="823d2-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="823d2-122">Directory befejeződött toohello projekt; módosítása Példa:</span><span class="sxs-lookup"><span data-stu-id="823d2-122">Change directory toohello completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="823d2-123">Build Maven; használatával hello JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="823d2-123">Build hello JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="823d2-124">Hello webalkalmazás létrehozása, módosítása directory toohello `target` könyvtárat, ahol hello JAR-fájlra található start hello webalkalmazás; például:</span><span class="sxs-lookup"><span data-stu-id="823d2-124">Once hello web app has been created, change directory toohello `target` directory where hello JAR file is located and start hello web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="823d2-125">Hello webes alkalmazás tesztelése a helyileg a webböngésző segítségével tooit megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="823d2-125">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="823d2-126">Például ha a curl érhető el, és úgy hello Tomcat kiszolgálón toorun 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="823d2-126">For example, if you have curl available and you configured hello Tomcat server toorun on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="823d2-127">A következő üzenet jelenik meg hello kell megjelennie: **Hello Docker World!**</span><span class="sxs-lookup"><span data-stu-id="823d2-127">You should see hello following message displayed: **Hello Docker World!**</span></span>

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-container-registry-toouse-as-a-private-docker-registry"></a><span data-ttu-id="823d2-129">Hozzon létre egy Azure-tároló beállításjegyzék toouse titkos Docker-beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="823d2-129">Create an Azure Container Registry toouse as a Private Docker Registry</span></span>

<span data-ttu-id="823d2-130">hello következő lépések végigvezetik hello Azure portál toocreate egy Azure-tároló beállításjegyzék használatával.</span><span class="sxs-lookup"><span data-stu-id="823d2-130">hello following steps walk you through using hello Azure portal toocreate an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="823d2-131">Ha azt szeretné, hogy toouse hello Azure parancssori felület helyett hello Azure-portálon, kövesse hello [hozza létre a titkos Docker tároló beállításkulcs használatával hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="823d2-131">If you want toouse hello Azure CLI instead of hello Azure portal, follow hello steps in [Create a private Docker container registry using hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span></span>
>

1. <span data-ttu-id="823d2-132">Keresse meg a toohello [Azure-portálon] , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="823d2-132">Browse toohello [Azure portal] and sign in.</span></span>

   <span data-ttu-id="823d2-133">Miután bejelentkezett fiók tooyour hello Azure-portálon, a lépésekkel hello a hello [hozza létre a titkos Docker tároló beállításkulcs hello Azure-portál használatával] cikket, amely a következő lépéseket a hello hello átfogalmazni vannak szakét célszerűségi.</span><span class="sxs-lookup"><span data-stu-id="823d2-133">Once you have signed in tooyour account on hello Azure portal, you can follow hello steps in hello [Create a private Docker container registry using hello Azure portal] article, which are paraphrased in hello following steps for hello sake of expediency.</span></span>

1. <span data-ttu-id="823d2-134">Hello menü ikonra a **+ új**, majd kattintson a **tárolók**, és kattintson a **Azure tároló beállításjegyzék**.</span><span class="sxs-lookup"><span data-stu-id="823d2-134">Click hello menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Hozzon létre egy új Azure-tároló beállításjegyzék][AR01]

1. <span data-ttu-id="823d2-136">Amikor hello Azure tároló beállításjegyzék sablon hello adatai lap megjelenik, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="823d2-136">When hello information page for hello Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Hozzon létre egy új Azure-tároló beállításjegyzék][AR02]

1. <span data-ttu-id="823d2-138">Ha hello **létrehozás tároló beállításjegyzék** lap is megjelenik, írja be a **beállításjegyzék neve** és **erőforráscsoport**, válassza a **engedélyezése** a Hello **rendszergazdai jogú felhasználó**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="823d2-138">When hello **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for hello **Admin user**, and then click **Create**.</span></span>

   ![Azure-tároló beállításjegyzék-beállítások konfigurálása][AR03]

1. <span data-ttu-id="823d2-140">Miután létrejött a tároló beállításjegyzék, keresse meg a tároló regisztrációs tooyour hello Azure-portálon, és kattintson a **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="823d2-140">Once your container registry has been created, navigate tooyour container registry in hello Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="823d2-141">Jegyezze fel a hello felhasználónév és jelszó hello további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="823d2-141">Take note of hello username and password for hello next steps.</span></span>

   ![Az Azure tároló beállításkulcsok hozzáférés][AR04]

## <a name="configure-maven-toouse-your-azure-container-registry-access-keys"></a><span data-ttu-id="823d2-143">Maven toouse az Azure tároló hozzáférés beállításkulcsok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="823d2-143">Configure Maven toouse your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="823d2-144">Nyissa meg a toohello konfigurációs könyvtára a Maven telepítéséhez, és nyissa meg a hello *settings.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="823d2-144">Navigate toohello configuration directory for your Maven installation and open hello *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="823d2-145">Az Azure-tároló beállításjegyzék-hozzáférési beállítások hozzáadása az előző szakaszából hello ezen oktatóanyag toohello `<servers>` hello gyűjtemény *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="823d2-145">Add your Azure Container Registry access settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="823d2-146">A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg (például: "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker / teljes*"), és nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="823d2-146">Navigate toohello completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="823d2-147">Frissítés hello `<properties>` hello gyűjtemény *pom.xml* hello bejelentkezési kiszolgáló értéke az hello Ez az oktatóanyag előző szakaszából a Azure tároló rendszerleíró fájlt például:</span><span class="sxs-lookup"><span data-stu-id="823d2-147">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry from hello previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="823d2-148">Frissítés hello `<plugins>` hello gyűjtemény *pom.xml* fájlt úgy, hogy hello `<plugin>` hello bejelentkezési cím és a beállításjegyzék kiszolgálónév a hello Ez az oktatóanyag előző szakaszából a Azure tároló beállításjegyzék tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="823d2-148">Update hello `<plugins>` collection in hello *pom.xml* file so that hello `<plugin>` contains hello login server address and registry name for your Azure Container Registry from hello previous section of this tutorial.</span></span> <span data-ttu-id="823d2-149">Példa:</span><span class="sxs-lookup"><span data-stu-id="823d2-149">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. <span data-ttu-id="823d2-150">A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg és futtassa a következő parancs toorebuild hello alkalmazás hello és hello tároló tooyour Azure tároló beállításjegyzék leküldéses:</span><span class="sxs-lookup"><span data-stu-id="823d2-150">Navigate toohello completed project directory for your Spring Boot application and run hello following command toorebuild hello application and push hello container tooyour Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="823d2-151">Amikor a Docker-tároló tooAzure leküldendő, amely hasonló tooone a hello annak ellenére, hogy a Docker-tároló létrehozása sikerült a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="823d2-151">When you are pushing your Docker container tooAzure, you may receive an error message that is similar tooone of hello following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="823d2-152">Ha ez történik, szükség lehet a toosign a tooyour hello Docker parancssori; az Azure-fiók Példa:</span><span class="sxs-lookup"><span data-stu-id="823d2-152">If this happens, you may need toosign in tooyour Azure account from hello Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="823d2-153">Majd tolható ki a tároló parancssorból hello; Példa:</span><span class="sxs-lookup"><span data-stu-id="823d2-153">You can then push your container from hello command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="823d2-154">Webalkalmazás létrehozása az Azure App Service a tároló lemezkép használata Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="823d2-154">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="823d2-155">Keresse meg a toohello [Azure-portálon] , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="823d2-155">Browse toohello [Azure portal] and sign in.</span></span>

1. <span data-ttu-id="823d2-156">Hello menü ikonra a **+ új**, majd kattintson a **Web + mobil**, és kattintson a **Linux webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="823d2-156">Click hello menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![Egy új webalkalmazás létrehozása az Azure-portálon hello][LX01]

1. <span data-ttu-id="823d2-158">Ha hello **Linux webalkalmazás** lap is megjelenik, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="823d2-158">When hello **Web App on Linux** page is displayed, enter hello following information:</span></span>

   <span data-ttu-id="823d2-159">a.</span><span class="sxs-lookup"><span data-stu-id="823d2-159">a.</span></span> <span data-ttu-id="823d2-160">Adjon meg egy egyedi nevet hello **alkalmazásnév**; például: "*wingtiptoyslinux*."</span><span class="sxs-lookup"><span data-stu-id="823d2-160">Enter a unique name for hello **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="823d2-161">b.</span><span class="sxs-lookup"><span data-stu-id="823d2-161">b.</span></span> <span data-ttu-id="823d2-162">Válassza ki a **előfizetés** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="823d2-162">Choose your **Subscription** from hello drop-down list.</span></span>

   <span data-ttu-id="823d2-163">c.</span><span class="sxs-lookup"><span data-stu-id="823d2-163">c.</span></span> <span data-ttu-id="823d2-164">Válasszon egy meglévő **erőforráscsoport**, vagy adjon meg egy nevet toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="823d2-164">Choose an existing **Resource Group**, or specify a name toocreate a new resource group.</span></span>

   <span data-ttu-id="823d2-165">d.</span><span class="sxs-lookup"><span data-stu-id="823d2-165">d.</span></span> <span data-ttu-id="823d2-166">Kattintson a **konfigurálása tároló** , és írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="823d2-166">Click **Configure container** and enter hello following information:</span></span>

      * <span data-ttu-id="823d2-167">Válasszon **titkos beállításjegyzék**.</span><span class="sxs-lookup"><span data-stu-id="823d2-167">Choose **Private registry**.</span></span>

      * <span data-ttu-id="823d2-168">**Kép és a nem kötelező címke**: írja be a tároló nevét, a korábbi, például: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span><span class="sxs-lookup"><span data-stu-id="823d2-168">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

      * <span data-ttu-id="823d2-169">**URL-címe**: Adja meg a beállításjegyzék URL-CÍMÉT, korábban; például: "*https://wingtiptoysregistry.azurecr.io*"</span><span class="sxs-lookup"><span data-stu-id="823d2-169">**Server URL**: Specify your registry URL from earlier; for example: "*https://wingtiptoysregistry.azurecr.io*"</span></span>

      * <span data-ttu-id="823d2-170">**Bejelentkezési felhasználónevének** és **jelszó**: Adja meg a bejelentkezési hitelesítő adatait a **hívóbetűk** , amelyet az előző lépésben használt.</span><span class="sxs-lookup"><span data-stu-id="823d2-170">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="823d2-171">e.</span><span class="sxs-lookup"><span data-stu-id="823d2-171">e.</span></span> <span data-ttu-id="823d2-172">Miután megadta az összes hello fent információkat, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="823d2-172">Once you have entered all of hello above information, click **OK**.</span></span>

   ![A webalkalmazás-beállítások konfigurálása][LX02]

1. <span data-ttu-id="823d2-174">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="823d2-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="823d2-175">Azure automatikusan felelteti meg internetes kérelmek tooembedded Tomcat kiszolgálót, hogy fut-hello szabványos 80-as vagy a 8080-as port.</span><span class="sxs-lookup"><span data-stu-id="823d2-175">Azure will automatically map Internet requests tooembedded Tomcat server that is running on hello standard ports of 80 or 8080.</span></span> <span data-ttu-id="823d2-176">Azonban ha a beágyazott Tomcat kiszolgálón toorun konfigurált egy egyéni portot, kell tooadd egy környezeti változó tooyour webalkalmazás, amely meghatározza a beágyazott Tomcat kiszolgálót hello port.</span><span class="sxs-lookup"><span data-stu-id="823d2-176">However, if you configured your embedded Tomcat server toorun on a custom port, you need tooadd an environment variable tooyour web app that defines hello port for your embedded Tomcat server.</span></span> <span data-ttu-id="823d2-177">toodo Igen, használja a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="823d2-177">toodo so, use hello following steps:</span></span>
>
> 1. <span data-ttu-id="823d2-178">Keresse meg a toohello [Azure-portálon] , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="823d2-178">Browse toohello [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="823d2-179">Hello ikonra a **alkalmazásszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="823d2-179">Click hello icon for **App Services**.</span></span> <span data-ttu-id="823d2-180">(Lásd az alábbi képen hello #1 elemére.)</span><span class="sxs-lookup"><span data-stu-id="823d2-180">(See item #1 in hello image below.)</span></span>
>
> 3. <span data-ttu-id="823d2-181">Válassza ki a webalkalmazás hello listából.</span><span class="sxs-lookup"><span data-stu-id="823d2-181">Select your web app from hello list.</span></span> <span data-ttu-id="823d2-182">(#2 elem az alábbi képen hello.)</span><span class="sxs-lookup"><span data-stu-id="823d2-182">(Item #2 in hello image below.)</span></span>
>
> 4. <span data-ttu-id="823d2-183">Kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="823d2-183">Click **Application Settings**.</span></span> <span data-ttu-id="823d2-184">(Eleme #3 az alábbi képen hello.)</span><span class="sxs-lookup"><span data-stu-id="823d2-184">(Item #3 in hello image below.)</span></span>
>
> 5. <span data-ttu-id="823d2-185">A hello **Alkalmazásbeállítások** területen adjon hozzá egy új környezeti változó nevű **PORT** , és írja be az egyéni portszám hello értékhez.</span><span class="sxs-lookup"><span data-stu-id="823d2-185">In hello **App settings** section, add a new environment variable named **PORT** and enter your custom port number for hello value.</span></span> <span data-ttu-id="823d2-186">(Eleme #4 hello az alábbi képen.)</span><span class="sxs-lookup"><span data-stu-id="823d2-186">(Item #4 in hello image below.)</span></span>
>
> 6. <span data-ttu-id="823d2-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="823d2-187">Click **Save**.</span></span> <span data-ttu-id="823d2-188">(#5 a eleme az alábbi képen hello.)</span><span class="sxs-lookup"><span data-stu-id="823d2-188">(Item #5 in hello image below.)</span></span>
>
> ![Elmenti egy egyéni portszám hello Azure-portálon][LX03]
>

<!--
##  OPTIONAL: Configure hello embedded Tomcat server toorun on a different port

hello embedded Tomcat server in hello sample Spring Boot application is configured toorun on port 8080 by default. However, if you want toorun hello embedded Tomcat server toorun on a different port, such as port 80 for local testing, you can configure hello port by using hello following steps.

1. Go toohello *resources* directory (or create hello directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open hello *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify hello **server** setting so that hello server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close hello *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="823d2-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="823d2-190">Next steps</span></span>

<span data-ttu-id="823d2-191">Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="823d2-191">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="823d2-192">A rugó rendszerindító alkalmazás toohello Azure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="823d2-192">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="823d2-193">A rugó rendszerindító alkalmazás Kubernetes gazdagépfürtökön hello Azure Tárolószolgáltatás a központi telepítése</span><span class="sxs-lookup"><span data-stu-id="823d2-193">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="823d2-194">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="823d2-194">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="823d2-195">További hello rugó rendszerindító a Docker mintaprojektet kapcsolatos további információkért lásd: [rugó rendszerindító a Docker bevezetés].</span><span class="sxs-lookup"><span data-stu-id="823d2-195">For further details about hello Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="823d2-196">Első lépések a saját rugó rendszerindító alkalmazásokkal kapcsolatban lásd: hello **rugó Initializr** https://start.spring.io/ címen.</span><span class="sxs-lookup"><span data-stu-id="823d2-196">For help with getting started with your own Spring Boot applications, see hello **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="823d2-197">Első lépések egy egyszerű rugó rendszerindító alkalmazás létrehozásával kapcsolatos további információkért lásd: hello rugó Initializr https://start.spring.io/ címen.</span><span class="sxs-lookup"><span data-stu-id="823d2-197">For more information about getting started with creating a simple Spring Boot application, see hello Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="823d2-198">További példák a hogyan toouse egyéni Docker lemezképek az Azure-ral [Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával].</span><span class="sxs-lookup"><span data-stu-id="823d2-198">For additional examples for how toouse custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure tároló szolgáltatás (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[hozza létre a titkos Docker tároló beállításkulcs hello Azure-portál használatával]: /azure/container-registry/container-registry-get-started-portal
[Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
