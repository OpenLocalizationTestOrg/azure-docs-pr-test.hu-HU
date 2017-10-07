---
title: "a rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Kubernetes aaaDeploy |} Microsoft Docs"
description: "Ez az oktatóanyag részletesen ismerteti, ha hello lépéseket toodeploy egy rugó rendszerindító alkalmazás Kubernetes fürtben Microsoft Azure-on."
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
ms.openlocfilehash: 2bf9df459f874a1f478f43cdd29992d86c370837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-hello-azure-container-service"></a><span data-ttu-id="2d6c9-103">A rugó rendszerindító alkalmazás Kubernetes gazdagépfürtökön hello Azure Tárolószolgáltatás a központi telepítése</span><span class="sxs-lookup"><span data-stu-id="2d6c9-103">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>

<span data-ttu-id="2d6c9-104">Hello  **[rugó keretrendszer]**  egy népszerű nyílt forráskódú keretrendszer, amely a fejlesztőket Java webes, mobil és API-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-104">hello **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="2d6c9-105">Ez az oktatóanyag használja egy mintaalkalmazást használatával létrehozott [rugó rendszerindító], egy egyezmény adatvezérelt megközelítés rugó tooget használatával gyorsan lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring tooget started quickly.</span></span>

<span data-ttu-id="2d6c9-106">**[Kubernetes]**  és  **[Docker]**  vannak nyílt forráskódú megoldások, amelyek segítségével a fejlesztők hello telepítési, méretezés és automatizálását a futó alkalmazások kezelése a tárolók.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-106">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate hello deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="2d6c9-107">Ez az oktatóanyag bemutatja, hogyan abban az esetben, ha a két népszerű, nyílt forráskódú technológiák toodevelop kombinálásával, és telepítheti a rugó rendszerindító alkalmazás tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-107">This tutorial walks you though combining these two popular, open-source technologies toodevelop and deploy a Spring Boot application tooMicrosoft Azure.</span></span> <span data-ttu-id="2d6c9-108">Pontosabban, használhat  *[rugó rendszerindító]*  alkalmazásfejlesztés,  *[Kubernetes]*  tároló üzembe helyezését és hello [Azure tároló szolgáltatás (ACS)] toohost az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-108">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and hello [Azure Container Service (ACS)] toohost your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2d6c9-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2d6c9-109">Prerequisites</span></span>

* <span data-ttu-id="2d6c9-110">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="2d6c9-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="2d6c9-111">Hello [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="2d6c9-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="2d6c9-112">Egy naprakész [Java fejlesztői készlet (JDK)].</span><span class="sxs-lookup"><span data-stu-id="2d6c9-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="2d6c9-113">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="2d6c9-114">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-114">A [Git] client.</span></span>
* <span data-ttu-id="2d6c9-115">A [Docker] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="2d6c9-116">Ez az oktatóanyag toohello virtualizálási követelményeinek miatt nem lépések hello ebben a cikkben; virtuális gépen a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="2d6c9-117">A webalkalmazás első lépések a Docker hello rugó rendszerindító létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d6c9-117">Create hello Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="2d6c9-118">hello lépések végigvezetik a rugó rendszerindító webalkalmazás létrehozása, és helyi tesztelés keresztül.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-118">hello following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="2d6c9-119">Nyisson meg egy parancssort, és hozzon létre egy helyi könyvtár toohold, az alkalmazáshoz, majd a Könyvtárváltás toothat; Példa:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-119">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="2d6c9-120">– vagy –</span><span class="sxs-lookup"><span data-stu-id="2d6c9-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="2d6c9-121">Klónozás hello [rugó rendszerindító a Docker bevezetés] mintaprojektet hello könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="2d6c9-122">Directory befejeződött toohello projekt módosítása.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-122">Change directory toohello completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="2d6c9-123">Használja a Maven toobuild és futtatási hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-123">Use Maven toobuild and run hello sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="2d6c9-124">Teszt hello webalkalmazás toohttp://localhost:8080 megkeresésével, vagy hello következőre `curl` parancs:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-124">Test hello web app by browsing toohttp://localhost:8080, or with hello following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="2d6c9-125">A következő üzenet jelenik meg hello kell megjelennie: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="2d6c9-125">You should see hello following message displayed: **Hello Docker World**</span></span>

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a><span data-ttu-id="2d6c9-127">Hozzon létre egy Azure tároló beállításjegyzék hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="2d6c9-127">Create an Azure Container Registry using hello Azure CLI</span></span>

1. <span data-ttu-id="2d6c9-128">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-128">Open a command prompt.</span></span>

1. <span data-ttu-id="2d6c9-129">Jelentkezzen be tooyour Azure-fiók:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-129">Log in tooyour Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="2d6c9-130">Hozzon létre egy erőforráscsoportot hello ebben az oktatóanyagban használt Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-130">Create a resource group for hello Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="2d6c9-131">Hozzon létre egy saját Azure-tárolót beállításjegyzék hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-131">Create a private Azure container registry in hello resource group.</span></span> <span data-ttu-id="2d6c9-132">hello oktatóanyag hello mintaalkalmazást, a későbbi lépésekben Docker kép toothis beállításjegyzékbeli leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-132">hello tutorial pushes hello sample app as a Docker image toothis registry in later steps.</span></span> <span data-ttu-id="2d6c9-133">Cserélje le `wingtiptoysregistry` a beállításjegyzék egyedi nevére.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-toohello-container-registry"></a><span data-ttu-id="2d6c9-134">Az alkalmazás toohello tároló beállításjegyzék leküldéses</span><span class="sxs-lookup"><span data-stu-id="2d6c9-134">Push your app toohello container registry</span></span>

1. <span data-ttu-id="2d6c9-135">Keresse meg a Maven telepítés toohello konfigurációs könyvtára (alapértelmezett ~/.m2/ vagy C:\Users\username\.m2) és a nyitott hello *settings.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-135">Navigate toohello configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open hello *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="2d6c9-136">A tároló beállításjegyzék hello jelszó lekérése hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-136">Retrieve hello password for your container registry from hello Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="2d6c9-137">Adja hozzá az Azure-tároló beállításjegyzék ID azonosítója és jelszava tooa új `<server>` hello gyűjtemény *settings.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-137">Add your Azure Container Registry id and password tooa new `<server>` collection in hello *settings.xml* file.</span></span>
<span data-ttu-id="2d6c9-138">Hello `id` és `username` hello beállításjegyzék hello neve van.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-138">hello `id` and `username` are hello name of hello registry.</span></span> <span data-ttu-id="2d6c9-139">Használjon hello `password` hello előző parancs (idézőjelek nélkül) értéket.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-139">Use hello `password` value from hello previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="2d6c9-140">A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg (például "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker / teljes*"), és nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-140">Navigate toohello completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="2d6c9-141">Frissítés hello `<properties>` hello gyűjtemény *pom.xml* hello bejelentkezési kiszolgáló értéke az Azure-tároló rendszerleíró fájlt.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-141">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="2d6c9-142">Frissítés hello `<plugins>` hello gyűjtemény *pom.xml* fájlt úgy, hogy hello `<plugin>` hello bejelentkezési kiszolgáló címét és a beállításkulcs neve az Azure-tároló beállításjegyzék tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-142">Update hello `<plugins>` collection in hello *pom.xml* file so that hello `<plugin>` contains hello login server address and registry name for your Azure Container Registry.</span></span>

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

1. <span data-ttu-id="2d6c9-143">A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg, és futtassa a következő parancs toobuild hello Docker-tároló és a leküldéses hello kép toohello beállításjegyzék hello:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-143">Navigate toohello completed project directory for your Spring Boot application and run hello following command toobuild hello Docker container and push hello image toohello registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="2d6c9-144">Hello következő hasonló tooone esetén Maven leküldéses értesítések hello kép tooAzure hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-144">You may receive an error message that is similar tooone of hello following when Maven pushes hello image tooAzure:</span></span>
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="2d6c9-145">Ha ez a hibaüzenet jelenik meg, jelentkezzen be a Docker parancssori hello tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-145">If you get this error, log in tooAzure from hello Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="2d6c9-146">Majd küldje le a tároló:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-146">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-hello-azure-cli"></a><span data-ttu-id="2d6c9-147">Hozzon létre egy Kubernetes fürtöt ACS hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="2d6c9-147">Create a Kubernetes Cluster on ACS using hello Azure CLI</span></span>

1. <span data-ttu-id="2d6c9-148">Az Azure Tárolószolgáltatásban Kubernetes fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-148">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="2d6c9-149">hello következő parancs létrehoz egy *kubernetes* hello fürt *tartománynév-kubernetes* erőforrás csoport és *tartománynév-tárolószolgáltatás* hello fürtként név, és *tartománynév-kubernetes* , hello DNS-előtagja:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-149">hello following command creates a *kubernetes* cluster in hello *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as hello cluster name, and *wingtiptoys-kubernetes* as hello DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="2d6c9-150">Ez a parancs eltarthat egy ideig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-150">This command may take a while toocomplete.</span></span>

1. <span data-ttu-id="2d6c9-151">Telepítés `kubectl` Azure parancssori felület használatával hello.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-151">Install `kubectl` using hello Azure CLI.</span></span> <span data-ttu-id="2d6c9-152">Linux-felhasználók rendelkezhetnek tooprefix erre a parancsra `sudo` óta hello Kubernetes CLI túl telepíti`/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-152">Linux users may have tooprefix this command with `sudo` since it deploys hello Kubernetes CLI too`/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="2d6c9-153">Töltse le a hello fürt konfigurációs adatait, kezelheti a fürt hello Kubernetes webes felületet és `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-153">Download hello cluster configuration information so you can manage your cluster from hello Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-hello-image-tooyour-kubernetes-cluster"></a><span data-ttu-id="2d6c9-154">Hello kép tooyour Kubernetes fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="2d6c9-154">Deploy hello image tooyour Kubernetes cluster</span></span>

<span data-ttu-id="2d6c9-155">Ez az oktatóanyag telepíti hello alkalmazás használatával `kubectl`, lehetővé teszi tooexplore hello telepítési hello Kubernetes webes felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-155">This tutorial deploys hello app using `kubectl`, then allow you tooexplore hello deployment through hello Kubernetes web interface.</span></span>

### <a name="deploy-with-hello-kubernetes-web-interface"></a><span data-ttu-id="2d6c9-156">Hello Kubernetes webes felület telepítése</span><span class="sxs-lookup"><span data-stu-id="2d6c9-156">Deploy with hello Kubernetes web interface</span></span>

1. <span data-ttu-id="2d6c9-157">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-157">Open a command prompt.</span></span>

1. <span data-ttu-id="2d6c9-158">Nyissa meg az alapértelmezett böngésző hello konfigurációs webhely a Kubernetes fürt:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-158">Open hello configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="2d6c9-159">Hello Kubernetes konfigurációs webhely megnyitása a böngészőben, hivatkozásra hello túl**indexelése alkalmazás telepítése**:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-159">When hello Kubernetes configuration website opens in your browser, click hello link too**deploy a containerized app**:</span></span>

   ![Kubernetes konfigurációs webhely][KB01]

1. <span data-ttu-id="2d6c9-161">Ha hello **indexelése alkalmazás telepítése** lap is megjelenik, adja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-161">When hello **Deploy a containerized app** page is displayed, specify hello following options:</span></span>

   <span data-ttu-id="2d6c9-162">a.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-162">a.</span></span> <span data-ttu-id="2d6c9-163">Válassza ki **adja meg az alkalmazás adatait az alábbi**.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-163">Select **Specify app details below**.</span></span>

   <span data-ttu-id="2d6c9-164">b.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-164">b.</span></span> <span data-ttu-id="2d6c9-165">Adja meg a rugó rendszerindító alkalmazás neve hello **alkalmazásnév**; például: "*gs-rugó-rendszerindítás – docker*".</span><span class="sxs-lookup"><span data-stu-id="2d6c9-165">Enter your Spring Boot application name for hello **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="2d6c9-166">c.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-166">c.</span></span> <span data-ttu-id="2d6c9-167">Adja meg a bejelentkezési kiszolgáló és a tároló lemezkép korábbi hello **tároló kép**; például: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span><span class="sxs-lookup"><span data-stu-id="2d6c9-167">Enter your login server and container image from earlier for hello **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="2d6c9-168">d.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-168">d.</span></span> <span data-ttu-id="2d6c9-169">Válasszon **külső** a hello **szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-169">Choose **External** for hello **Service**.</span></span>

   <span data-ttu-id="2d6c9-170">e.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-170">e.</span></span> <span data-ttu-id="2d6c9-171">Adja meg a külső és belső portok hello **Port** és **céloz port** szövegmezőket.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-171">Specify your external and internal ports in hello **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes konfigurációs webhely][KB02]


1. <span data-ttu-id="2d6c9-173">Kattintson a **telepítés** toodeploy hello tároló.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-173">Click **Deploy** toodeploy hello container.</span></span>

   ![Tároló üzembe][KB05]

1. <span data-ttu-id="2d6c9-175">Miután az alkalmazás telepítve van, megjelenik-e a felsorolt rugó rendszerindító alkalmazás **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-175">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes szolgáltatások][KB06]

1. <span data-ttu-id="2d6c9-177">Ha hello hivatkozásra kattintva **külső végpontok száma**, az Azure-on futó rugó rendszerindító alkalmazás látható.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-177">If you click hello link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes szolgáltatások][KB07]

   ![Keresse meg a mintaalkalmazás az Azure-on][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="2d6c9-180">Kubectl üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="2d6c9-180">Deploy with kubectl</span></span>

1. <span data-ttu-id="2d6c9-181">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-181">Open a command prompt.</span></span>

1. <span data-ttu-id="2d6c9-182">A tároló hello Kubernetes fürt hello használatával futtassa `kubectl run` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-182">Run your container in hello Kubernetes cluster by using hello `kubectl run` command.</span></span> <span data-ttu-id="2d6c9-183">Adja meg a szolgáltatás nevét az alkalmazáshoz a Kubernetes és hello teljes kép neve.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-183">Give a service name for your app in Kubernetes and hello full image name.</span></span> <span data-ttu-id="2d6c9-184">Példa:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-184">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="2d6c9-185">Ebben a parancsban:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-185">In this command:</span></span>

   * <span data-ttu-id="2d6c9-186">hello Tárolónév `gs-spring-boot-docker` hello után azonnal megadott `run` parancs</span><span class="sxs-lookup"><span data-stu-id="2d6c9-186">hello container name `gs-spring-boot-docker` is specified immediately after hello `run` command</span></span>

   * <span data-ttu-id="2d6c9-187">Hello `--image` hello kombinált bejelentkezési kiszolgáló és a lemezkép neve megegyezik a paraméter`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="2d6c9-187">hello `--image` parameter specifies hello combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="2d6c9-188">Külsőleg hello segítségével teszi közzé a Kubernetes fürt `kubectl expose` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-188">Expose your Kubernetes cluster externally by using hello `kubectl expose` command.</span></span> <span data-ttu-id="2d6c9-189">Adja meg a szolgáltatás nevét, hello nyilvánosan elérhető TCP használt port tooaccess hello alkalmazást, és a hello belső célportnak figyeli az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-189">Specify your service name, hello public-facing TCP port used tooaccess hello app, and hello internal target port your app listens on.</span></span> <span data-ttu-id="2d6c9-190">Példa:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-190">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="2d6c9-191">Ebben a parancsban:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-191">In this command:</span></span>

   * <span data-ttu-id="2d6c9-192">hello Tárolónév `gs-spring-boot-docker` hello után azonnal megadott `expose deployment` parancs</span><span class="sxs-lookup"><span data-stu-id="2d6c9-192">hello container name `gs-spring-boot-docker` is specified immediately after hello `expose deployment` command</span></span>

   * <span data-ttu-id="2d6c9-193">Hello `--type` paraméter határozza meg, hogy hello fürt terheléselosztót használ</span><span class="sxs-lookup"><span data-stu-id="2d6c9-193">hello `--type` parameter specifies that hello cluster uses load balancer</span></span>

   * <span data-ttu-id="2d6c9-194">Hello `--port` paraméterrel hello nyilvánosan elérhető TCP-port a 80-as.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-194">hello `--port` parameter specifies hello public-facing TCP port of 80.</span></span> <span data-ttu-id="2d6c9-195">Ezen a porton hello alkalmazás érhető el.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-195">You access hello app on this port.</span></span>

   * <span data-ttu-id="2d6c9-196">Hello `--target-port` paraméterrel hello belső TCP-port a 8080-as.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-196">hello `--target-port` parameter specifies hello internal TCP port of 8080.</span></span> <span data-ttu-id="2d6c9-197">hello terheléselosztó kérelmek tooyour alkalmazás ezen a porton továbbítja.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-197">hello load balancer forwards requests tooyour app on this port.</span></span>

1. <span data-ttu-id="2d6c9-198">Miután hello alkalmazás telepítve van a toohello fürt lekérdezése hello külső IP-cím, és nyissa meg a böngészőben:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-198">Once hello app is deployed toohello cluster, query hello external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Keresse meg a mintaalkalmazás az Azure-on][SB02]


## <a name="next-steps"></a><span data-ttu-id="2d6c9-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d6c9-200">Next steps</span></span>

<span data-ttu-id="2d6c9-201">Azure rugó rendszerindítással kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-201">For more information about using Spring Boot on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="2d6c9-202">A rugó rendszerindító alkalmazás toohello Azure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="2d6c9-202">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="2d6c9-203">Az Azure Tárolószolgáltatás hello Linux rugó rendszerindító-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="2d6c9-203">Deploy a Spring Boot application on Linux in hello Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-linux.md)

<span data-ttu-id="2d6c9-204">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="2d6c9-204">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="2d6c9-205">A Docker mintaprojektet hello rugó rendszerindító kapcsolatos további információkért lásd: [rugó rendszerindító a Docker bevezetés].</span><span class="sxs-lookup"><span data-stu-id="2d6c9-205">For more information about hello Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="2d6c9-206">a következő hivatkozások hello rugó rendszerindító alkalmazások létrehozásával kapcsolatos további információkat:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-206">hello following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="2d6c9-207">Egy egyszerű rugó rendszerindító alkalmazás létrehozásával kapcsolatos további információkért lásd: hello rugó Initializr https://start.spring.io/ címen.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-207">For more information about creating a simple Spring Boot application, see hello Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="2d6c9-208">hello következő hivatkozásokra kattintva további információt az Azure-ral Kubernetes használatáról:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-208">hello following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="2d6c9-209">A Tárolószolgáltatás Kubernetes fürt első lépések</span><span class="sxs-lookup"><span data-stu-id="2d6c9-209">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="2d6c9-210">Hello segítségével Kubernetes webes felhasználói felülete, az Azure Tárolószolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="2d6c9-210">Using hello Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="2d6c9-211">További információ a Kubernetes parancssori felület használatával érhető el hello **kubectl** felhasználói útmutatóban talál: <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="2d6c9-211">More information about using Kubernetes command-line interface is available in hello **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="2d6c9-212">hello Kubernetes webhely TITKOS nyilvántartó képek használatát ismertetik, amelyek több cikkek rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="2d6c9-212">hello Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="2d6c9-213">[Három munkaállomás-csoporttal fiókjainak szolgáltatás konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="2d6c9-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="2d6c9-214">[Névterek]</span><span class="sxs-lookup"><span data-stu-id="2d6c9-214">[Namespaces]</span></span>
* <span data-ttu-id="2d6c9-215">[Húzza a lemezkép egy titkos beállításjegyzékből]</span><span class="sxs-lookup"><span data-stu-id="2d6c9-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="2d6c9-216">További példák a hogyan toouse egyéni Docker lemezképek az Azure-ral [Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával].</span><span class="sxs-lookup"><span data-stu-id="2d6c9-216">For additional examples for how toouse custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure tároló szolgáltatás (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[rugó keretrendszer]: https://spring.io/
[Három munkaállomás-csoporttal fiókjainak szolgáltatás konfigurálása]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Névterek]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Húzza a lemezkép egy titkos beállításjegyzékből]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
