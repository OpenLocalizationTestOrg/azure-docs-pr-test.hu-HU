---
title: "Az Azure Tárolószolgáltatásban Linux rugó rendszerindító webalkalmazás üzembe helyezése |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan, ha egy rugó rendszerindító alkalmazásra, amely a Microsoft Azure Linux webes alkalmazás telepítésének a lépéseit."
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
ms.openlocfilehash: 5f0b470bd46cfeaf00b3092dbe9db507ed50f622
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a><span data-ttu-id="f1fab-103">A rugó rendszerindító alkalmazás az Azure Tárolószolgáltatásban Linux központi telepítése</span><span class="sxs-lookup"><span data-stu-id="f1fab-103">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>

<span data-ttu-id="f1fab-104">A  **[rugó keretrendszer]**  egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="f1fab-104">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="f1fab-105">A beépített több népszerű projektek a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f1fab-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="f1fab-106">**[Docker]**  van nyílt forráskódú megoldások, amelyek segítségével a fejlesztők automatizálja a központi telepítési méretezés és a tárolók a futó alkalmazások kezelését.</span><span class="sxs-lookup"><span data-stu-id="f1fab-106">**[Docker]** is open-source solutions that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="f1fab-107">Ez az oktatóanyag bemutatja, hogyan Docker használatával történő fejlesztéséhez és a Linux gazdagépre rugó rendszerindító alkalmazást központilag a [Azure tároló szolgáltatás (ACS)].</span><span class="sxs-lookup"><span data-stu-id="f1fab-107">This tutorial walks you through using Docker to develop and deploy a Spring Boot application to a Linux host in the [Azure Container Service (ACS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1fab-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f1fab-108">Prerequisites</span></span>

<span data-ttu-id="f1fab-109">Az oktatóanyagban szereplő lépések végrehajtásához kell rendelkeznie a következő előfeltételek teljesülését:</span><span class="sxs-lookup"><span data-stu-id="f1fab-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="f1fab-110">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="f1fab-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="f1fab-111">A [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="f1fab-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="f1fab-112">Egy naprakész [Java fejlesztői készlet (JDK)].</span><span class="sxs-lookup"><span data-stu-id="f1fab-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="f1fab-113">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f1fab-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="f1fab-114">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f1fab-114">A [Git] client.</span></span>
* <span data-ttu-id="f1fab-115">A [Docker] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f1fab-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="f1fab-116">Ez az oktatóanyag virtualizálási követelményeinek, mert; virtuális gépen nem kövesse a cikkben leírt lépéseket a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.</span><span class="sxs-lookup"><span data-stu-id="f1fab-116">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="f1fab-117">A rugó rendszerindító létrehozása webalkalmazásban Docker első lépések</span><span class="sxs-lookup"><span data-stu-id="f1fab-117">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="f1fab-118">A következő lépések végigvezetik hozzon létre egy egyszerű rugó rendszerindító webalkalmazást, és helyben tesztelheti a lépéseit.</span><span class="sxs-lookup"><span data-stu-id="f1fab-118">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="f1fab-119">Nyisson meg egy parancssort, és hozzon létre egy helyi könyvtárat az alkalmazás tárolására, és módosítsa a könyvtárhoz; Példa:</span><span class="sxs-lookup"><span data-stu-id="f1fab-119">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="f1fab-120">– vagy –</span><span class="sxs-lookup"><span data-stu-id="f1fab-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="f1fab-121">Klónozott a [rugó rendszerindító a Docker bevezetés] mintaprojektet a könyvtárba, amely létrehozta; például:</span><span class="sxs-lookup"><span data-stu-id="f1fab-121">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="f1fab-122">Módosítsa a könyvtárat a befejezett projekthez; Példa:</span><span class="sxs-lookup"><span data-stu-id="f1fab-122">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="f1fab-123">Build Maven; használatával JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="f1fab-123">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="f1fab-124">A webalkalmazás létrehozása után módosítsa a könyvtárat a `target` könyvtárban, ahol a JAR-fájlra is, és indítsa el a webalkalmazást; például:</span><span class="sxs-lookup"><span data-stu-id="f1fab-124">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="f1fab-125">A webes alkalmazás tesztelése a azt helyileg a webböngésző segítségével.</span><span class="sxs-lookup"><span data-stu-id="f1fab-125">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="f1fab-126">Például ha a curl érhető el, és konfigurálni a Tomcat kiszolgálón 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="f1fab-126">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="f1fab-127">A következő üzenet jelenik meg: **Hello Docker World!**</span><span class="sxs-lookup"><span data-stu-id="f1fab-127">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="f1fab-129">Hozzon létre egy Azure-tároló beállításjegyzék kívánja használni, mint egy titkos Docker-beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="f1fab-129">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="f1fab-130">A következő lépések végigvezetik Önt az Azure portál segítségével hozza létre az Azure-tároló beállításkulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="f1fab-130">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="f1fab-131">Ha szeretné használni az Azure parancssori felület helyett az Azure-portálon, kövesse a [hozza létre az Azure CLI 2.0 használatával saját Docker tároló beállításkulcs](../../container-registry/container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f1fab-131">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span></span>
>

1. <span data-ttu-id="f1fab-132">Keresse meg a [Azure-portálon] , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f1fab-132">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="f1fab-133">Miután bejelentkezett az Azure portálon fiókjába, a lépések a [hozza létre az Azure portál használatával saját Docker tároló beállításkulcs] cikk, amely a sake a következő lépések vannak átfogalmazni célszerűségi.</span><span class="sxs-lookup"><span data-stu-id="f1fab-133">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="f1fab-134">Kattintson az menü **+ új**, majd kattintson a **tárolók**, és kattintson a **Azure tároló beállításjegyzék**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-134">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Hozzon létre egy új Azure-tároló beállításjegyzék][AR01]

1. <span data-ttu-id="f1fab-136">Ha az adatokat az Azure-tároló beállításjegyzék sablon megjelenik, kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-136">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Hozzon létre egy új Azure-tároló beállításjegyzék][AR02]

1. <span data-ttu-id="f1fab-138">Ha a **létrehozás tároló beállításjegyzék** lap is megjelenik, írja be a **beállításjegyzék neve** és **erőforráscsoport**, válassza a **engedélyezése** a a **Rendszergazdai jogú felhasználó**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-138">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Azure-tároló beállításjegyzék-beállítások konfigurálása][AR03]

1. <span data-ttu-id="f1fab-140">Miután létrejött a tároló beállításjegyzék, keresse meg a tároló beállításjegyzék, az Azure portálon, és kattintson **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-140">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="f1fab-141">Jegyezze fel a felhasználónevet és jelszót a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f1fab-141">Take note of the username and password for the next steps.</span></span>

   ![Az Azure tároló beállításkulcsok hozzáférés][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="f1fab-143">Maven használata az Azure tároló hozzáférés beállításkulcsok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1fab-143">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="f1fab-144">Nyissa meg a Maven telepítés konfigurációs könyvtára, és nyissa meg a *settings.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="f1fab-144">Navigate to the configuration directory for your Maven installation and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="f1fab-145">Ez az oktatóanyag előző szakaszából a Azure tároló beállításjegyzék-hozzáférési beállítások hozzáadása a `<servers>` gyűjtemény a *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="f1fab-145">Add your Azure Container Registry access settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="f1fab-146">Keresse meg a befejezett projekt könyvtárát a rugó rendszerindító alkalmazás (például: "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* "), és nyissa meg a *pom.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="f1fab-146">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="f1fab-147">Frissítés a `<properties>` gyűjtemény a *pom.xml* Ez az oktatóanyag előző szakaszából a Azure tároló beállításjegyzék bejelentkezési kiszolgáló értékű fájlt például:</span><span class="sxs-lookup"><span data-stu-id="f1fab-147">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="f1fab-148">Frissítés a `<plugins>` gyűjtemény a *pom.xml* fájlt úgy, hogy a `<plugin>` bejelentkezési kiszolgáló címét és a beállításjegyzék nevét tartalmazza az Azure-tároló beállításjegyzék Ez az oktatóanyag előző szakaszából.</span><span class="sxs-lookup"><span data-stu-id="f1fab-148">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="f1fab-149">Példa:</span><span class="sxs-lookup"><span data-stu-id="f1fab-149">For example:</span></span>

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

1. <span data-ttu-id="f1fab-150">Keresse meg a befejezett projekt könyvtárát rugó rendszerindító alkalmazás, és építse újra az alkalmazást, és küldje le a tároló az Azure-tároló beállításjegyzék a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f1fab-150">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="f1fab-151">A Docker-tároló leküldendő az Azure-ba, amikor is megjelenhet az alábbihoz hasonló hibaüzenetet még akkor is, abban az esetben, ha a Docker-tároló létrehozása sikerült:</span><span class="sxs-lookup"><span data-stu-id="f1fab-151">When you are pushing your Docker container to Azure, you may receive an error message that is similar to one of the following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="f1fab-152">Ha ez történik, szükség lehet az Azure-fiókjával jelentkezzen be a Docker parancssorból; Példa:</span><span class="sxs-lookup"><span data-stu-id="f1fab-152">If this happens, you may need to sign in to your Azure account from the Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="f1fab-153">Majd tolható ki a tárolót a parancssorból; Példa:</span><span class="sxs-lookup"><span data-stu-id="f1fab-153">You can then push your container from the command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="f1fab-154">Webalkalmazás létrehozása az Azure App Service a tároló lemezkép használata Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="f1fab-154">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="f1fab-155">Keresse meg a [Azure-portálon] , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f1fab-155">Browse to the [Azure portal] and sign in.</span></span>

1. <span data-ttu-id="f1fab-156">Kattintson az menü **+ új**, majd kattintson **Web + mobil**, és kattintson a **webalkalmazás Linux**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-156">Click the menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![Új webalkalmazás létrehozása az Azure-portálon][LX01]

1. <span data-ttu-id="f1fab-158">Ha a **Linux webalkalmazás** lap is megjelenik, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="f1fab-158">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="f1fab-159">a.</span><span class="sxs-lookup"><span data-stu-id="f1fab-159">a.</span></span> <span data-ttu-id="f1fab-160">Adjon meg egy egyedi nevet a **alkalmazásnév**; például: "*wingtiptoyslinux*."</span><span class="sxs-lookup"><span data-stu-id="f1fab-160">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="f1fab-161">b.</span><span class="sxs-lookup"><span data-stu-id="f1fab-161">b.</span></span> <span data-ttu-id="f1fab-162">Válassza ki a **előfizetés** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="f1fab-162">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="f1fab-163">c.</span><span class="sxs-lookup"><span data-stu-id="f1fab-163">c.</span></span> <span data-ttu-id="f1fab-164">Válasszon egy meglévő **erőforráscsoport**, vagy adjon meg egy új erőforráscsoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f1fab-164">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="f1fab-165">d.</span><span class="sxs-lookup"><span data-stu-id="f1fab-165">d.</span></span> <span data-ttu-id="f1fab-166">Kattintson a **konfigurálása tároló** , és írja be a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="f1fab-166">Click **Configure container** and enter the following information:</span></span>

      * <span data-ttu-id="f1fab-167">Válasszon **titkos beállításjegyzék**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-167">Choose **Private registry**.</span></span>

      * <span data-ttu-id="f1fab-168">**Kép és a nem kötelező címke**: írja be a tároló nevét, a korábbi, például: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span><span class="sxs-lookup"><span data-stu-id="f1fab-168">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

      * <span data-ttu-id="f1fab-169">**URL-címe**: Adja meg a beállításjegyzék URL-CÍMÉT, korábban; például: "*https://wingtiptoysregistry.azurecr.io*"</span><span class="sxs-lookup"><span data-stu-id="f1fab-169">**Server URL**: Specify your registry URL from earlier; for example: "*https://wingtiptoysregistry.azurecr.io*"</span></span>

      * <span data-ttu-id="f1fab-170">**Bejelentkezési felhasználónevének** és **jelszó**: Adja meg a bejelentkezési hitelesítő adatait a **hívóbetűk** , amelyet az előző lépésben használt.</span><span class="sxs-lookup"><span data-stu-id="f1fab-170">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="f1fab-171">e.</span><span class="sxs-lookup"><span data-stu-id="f1fab-171">e.</span></span> <span data-ttu-id="f1fab-172">Miután megadta a fenti adatokat, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-172">Once you have entered all of the above information, click **OK**.</span></span>

   ![A webalkalmazás-beállítások konfigurálása][LX02]

1. <span data-ttu-id="f1fab-174">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f1fab-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="f1fab-175">Azure automatikusan felelteti meg beágyazott Tomcat kiszolgálót, a 80-as vagy a 8080-as szabványos portok futó Internet kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="f1fab-175">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="f1fab-176">Azonban ha konfigurálta a beágyazott Tomcat kiszolgálót futhat az egyéni portot, meg kell egy környezeti változó hozzáadása a webalkalmazáshoz, amely meghatározza a beágyazott Tomcat-kiszolgáló port.</span><span class="sxs-lookup"><span data-stu-id="f1fab-176">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="f1fab-177">Ehhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="f1fab-177">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="f1fab-178">Keresse meg a [Azure-portálon] , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f1fab-178">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="f1fab-179">Kattintson az **alkalmazásszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-179">Click the icon for **App Services**.</span></span> <span data-ttu-id="f1fab-180">(Lásd az alábbi képen #1 elemére.)</span><span class="sxs-lookup"><span data-stu-id="f1fab-180">(See item #1 in the image below.)</span></span>
>
> 3. <span data-ttu-id="f1fab-181">Válassza ki a webalkalmazását a listáról.</span><span class="sxs-lookup"><span data-stu-id="f1fab-181">Select your web app from the list.</span></span> <span data-ttu-id="f1fab-182">(#2 elem az alábbi képen.)</span><span class="sxs-lookup"><span data-stu-id="f1fab-182">(Item #2 in the image below.)</span></span>
>
> 4. <span data-ttu-id="f1fab-183">Kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="f1fab-183">Click **Application Settings**.</span></span> <span data-ttu-id="f1fab-184">(Eleme #3 az alábbi képen.)</span><span class="sxs-lookup"><span data-stu-id="f1fab-184">(Item #3 in the image below.)</span></span>
>
> 5. <span data-ttu-id="f1fab-185">A a **Alkalmazásbeállítások** területen adjon hozzá egy új környezeti változó nevű **PORT** és adja meg az egyéni portszám értékét.</span><span class="sxs-lookup"><span data-stu-id="f1fab-185">In the **App settings** section, add a new environment variable named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="f1fab-186">(#4 a eleme az alábbi képen.)</span><span class="sxs-lookup"><span data-stu-id="f1fab-186">(Item #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="f1fab-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f1fab-187">Click **Save**.</span></span> <span data-ttu-id="f1fab-188">(#5 a eleme az alábbi képen.)</span><span class="sxs-lookup"><span data-stu-id="f1fab-188">(Item #5 in the image below.)</span></span>
>
> ![Egyéni portszám mentése az Azure-portálon][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="f1fab-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1fab-190">Next steps</span></span>

<span data-ttu-id="f1fab-191">Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="f1fab-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="f1fab-192">Az Azure App Service egy rugó rendszerindító alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="f1fab-192">Deploy a Spring Boot Application to the Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="f1fab-193">A rugó rendszerindító alkalmazás az az Azure Tárolószolgáltatásban Kubernetes fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="f1fab-193">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="f1fab-194">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].</span><span class="sxs-lookup"><span data-stu-id="f1fab-194">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="f1fab-195">A rugó rendszerindító Docker mintaprojektet a további részleteiért lásd: [rugó rendszerindító a Docker bevezetés].</span><span class="sxs-lookup"><span data-stu-id="f1fab-195">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="f1fab-196">Első lépések a saját rugó rendszerindító alkalmazásokkal kapcsolatban lásd: a **rugó Initializr** https://start.spring.io/ címen.</span><span class="sxs-lookup"><span data-stu-id="f1fab-196">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="f1fab-197">Első lépések egy egyszerű rugó rendszerindító alkalmazás létrehozásával kapcsolatos további információkért tekintse meg a következő https://start.spring.io/ rugó Initializr.</span><span class="sxs-lookup"><span data-stu-id="f1fab-197">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="f1fab-198">Az egyéni Docker lemezképek használata Azure további példákért lásd [Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával].</span><span class="sxs-lookup"><span data-stu-id="f1fab-198">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

<span data-ttu-id="f1fab-199">[Azure parancssori felület (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="f1fab-199">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
<span data-ttu-id="f1fab-200">[Azure tároló szolgáltatás (ACS)]: https://azure.microsoft.com/services/container-service/</span><span class="sxs-lookup"><span data-stu-id="f1fab-200">[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span></span>
<span data-ttu-id="f1fab-201">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="f1fab-201">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="f1fab-202">[Azure-portálon]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="f1fab-202">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="f1fab-203">[hozza létre az Azure portál használatával saját Docker tároló beállításkulcs]: /azure/container-registry/container-registry-get-started-portal</span><span class="sxs-lookup"><span data-stu-id="f1fab-203">[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal</span></span>
<span data-ttu-id="f1fab-204">[Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span><span class="sxs-lookup"><span data-stu-id="f1fab-204">[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span></span>
<span data-ttu-id="f1fab-205">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="f1fab-205">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="f1fab-206">[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="f1fab-206">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="f1fab-207">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="f1fab-207">[Git]: https://github.com/</span></span>
<span data-ttu-id="f1fab-208">[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span><span class="sxs-lookup"><span data-stu-id="f1fab-208">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span></span>
<span data-ttu-id="f1fab-209">[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="f1fab-209">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="f1fab-210">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="f1fab-210">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="f1fab-211">[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="f1fab-211">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="f1fab-212">[rugó rendszerindító]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="f1fab-212">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="f1fab-213">[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="f1fab-213">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
<span data-ttu-id="f1fab-214">[rugó keretrendszer]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="f1fab-214">[Spring Framework]: https://spring.io/</span></span>

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
