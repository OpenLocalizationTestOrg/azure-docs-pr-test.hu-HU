---
title: "Az Azure Tárolószolgáltatásban Kubernetes a rugó rendszerindító alkalmazás telepítése |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, abban az esetben, ha a Microsoft Azure fürt egy olyan Kubernetes rugó rendszerindító alkalmazás telepítésének a lépéseit."
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
ms.openlocfilehash: 7f726436b2d459b8c16abb02e07de099abfd8974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a><span data-ttu-id="e42fe-103">Spring Boot-alkalmazás üzembe helyezése Kubernetes-fürtön az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="e42fe-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>

<span data-ttu-id="e42fe-104">A  **[rugó keretrendszer]**  egy népszerű nyílt forráskódú keretrendszer, amely a fejlesztőket Java webes, mobil és API-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e42fe-104">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="e42fe-105">Ez az oktatóanyag használja egy mintaalkalmazást használatával létrehozott [rugó rendszerindító], rugó használatával történő gyors használatbavétel a egyezmény adatvezérelt megközelítést.</span><span class="sxs-lookup"><span data-stu-id="e42fe-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="e42fe-106">**[Kubernetes]**  és  **[Docker]**  vannak nyílt forráskódú megoldások, amelyek segítségével a fejlesztők automatizálhatja a telepítés, a méretezés és a felügyeleti a tárolókban lévő futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e42fe-106">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="e42fe-107">Ez az oktatóanyag bemutatja, hogyan abban az esetben, ha ezek két népszerű, nyílt forráskódú technológia fejlesztése és a Microsoft Azure rugó rendszerindító alkalmazást központilag kombinálásával.</span><span class="sxs-lookup"><span data-stu-id="e42fe-107">This tutorial walks you though combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="e42fe-108">Pontosabban, használhat  *[rugó rendszerindító]*  alkalmazásfejlesztés,  *[Kubernetes]*  tároló központi telepítéséhez és a [Azure tároló szolgáltatás (ACS)] az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e42fe-108">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Container Service (ACS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="e42fe-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e42fe-109">Prerequisites</span></span>

* <span data-ttu-id="e42fe-110">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="e42fe-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="e42fe-111">A [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="e42fe-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="e42fe-112">Egy naprakész [Java fejlesztői készlet (JDK)].</span><span class="sxs-lookup"><span data-stu-id="e42fe-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="e42fe-113">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e42fe-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="e42fe-114">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="e42fe-114">A [Git] client.</span></span>
* <span data-ttu-id="e42fe-115">A [Docker] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="e42fe-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="e42fe-116">Ez az oktatóanyag virtualizálási követelményeinek, mert; virtuális gépen nem kövesse a cikkben leírt lépéseket a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.</span><span class="sxs-lookup"><span data-stu-id="e42fe-116">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="e42fe-117">A rugó rendszerindító létrehozása webalkalmazásban Docker első lépések</span><span class="sxs-lookup"><span data-stu-id="e42fe-117">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="e42fe-118">A következő lépések végigvezetik a rugó rendszerindító webalkalmazás létrehozása, és helyi tesztelés keresztül.</span><span class="sxs-lookup"><span data-stu-id="e42fe-118">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="e42fe-119">Nyisson meg egy parancssort, és hozzon létre egy helyi könyvtárat az alkalmazás tárolására, és módosítsa a könyvtárhoz; Példa:</span><span class="sxs-lookup"><span data-stu-id="e42fe-119">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="e42fe-120">– vagy –</span><span class="sxs-lookup"><span data-stu-id="e42fe-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="e42fe-121">Klónozott a [rugó rendszerindító a Docker bevezetés] mintaprojektet a könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="e42fe-121">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="e42fe-122">Módosítsa a könyvtárat a befejezett projekthez.</span><span class="sxs-lookup"><span data-stu-id="e42fe-122">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="e42fe-123">Maven használatával hozza létre, és futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e42fe-123">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="e42fe-124">A webes alkalmazás tesztelése a 8080, illetve a következő tallózással `curl` parancs:</span><span class="sxs-lookup"><span data-stu-id="e42fe-124">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="e42fe-125">A következő üzenet jelenik meg: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="e42fe-125">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="e42fe-127">Hozzon létre egy Azure-tároló beállításjegyzék, az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="e42fe-127">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="e42fe-128">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="e42fe-128">Open a command prompt.</span></span>

1. <span data-ttu-id="e42fe-129">Jelentkezzen be az Azure-fiókjával:</span><span class="sxs-lookup"><span data-stu-id="e42fe-129">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="e42fe-130">Hozzon létre egy erőforráscsoportot ebben az oktatóanyagban használt Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="e42fe-130">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="e42fe-131">Hozzon létre egy saját Azure-tárolót beállításjegyzék erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="e42fe-131">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="e42fe-132">Az oktatóanyag leküldi mintaalkalmazás Docker képként a későbbi lépésekben a beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="e42fe-132">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="e42fe-133">Cserélje le `wingtiptoysregistry` a beállításjegyzék egyedi nevére.</span><span class="sxs-lookup"><span data-stu-id="e42fe-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="e42fe-134">Az alkalmazás leküldése a tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="e42fe-134">Push your app to the container registry</span></span>

1. <span data-ttu-id="e42fe-135">Keresse meg a Maven telepítés konfigurációs könyvtára (alapértelmezett ~/.m2/ vagy C:\Users\username\.m2), majd nyissa meg a *settings.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="e42fe-135">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="e42fe-136">A tároló beállításjegyzék jelszavát lekérése az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="e42fe-136">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="e42fe-137">Adja hozzá az Azure-tároló beállításjegyzék-azonosítót és jelszót egy új `<server>` gyűjtemény a *settings.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e42fe-137">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="e42fe-138">A `id` és `username` a rendszerleíró adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="e42fe-138">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="e42fe-139">Használja a `password` érték az előző parancsot (idézőjelek nélkül).</span><span class="sxs-lookup"><span data-stu-id="e42fe-139">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="e42fe-140">Keresse meg a befejezett projekt könyvtárát rugó rendszerindító alkalmazás (például "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), és nyissa meg a *pom.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="e42fe-140">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="e42fe-141">Frissítés a `<properties>` gyűjtemény a *pom.xml* a bejelentkezési kiszolgáló értéke az Azure-tároló rendszerleíró fájlt.</span><span class="sxs-lookup"><span data-stu-id="e42fe-141">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="e42fe-142">Frissítés a `<plugins>` gyűjtemény a *pom.xml* fájlt úgy, hogy a `<plugin>` bejelentkezési kiszolgáló címét és a beállításjegyzék nevét tartalmazza az Azure-tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="e42fe-142">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

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

1. <span data-ttu-id="e42fe-143">Keresse meg a befejezett projekt könyvtárát rugó rendszerindító alkalmazás, és a Docker-tároló létrehozásához, és küldje le a lemezképet a beállításjegyzékben a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e42fe-143">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="e42fe-144">Maven leküldi a kép Azure esetén a következőhöz hasonló hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="e42fe-144">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="e42fe-145">Ha a következő hibaüzenet jelenik meg, jelentkezzen be az Azure a Docker parancssorból.</span><span class="sxs-lookup"><span data-stu-id="e42fe-145">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="e42fe-146">Majd küldje le a tároló:</span><span class="sxs-lookup"><span data-stu-id="e42fe-146">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a><span data-ttu-id="e42fe-147">Hozzon létre egy Kubernetes fürtöt ACS az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="e42fe-147">Create a Kubernetes Cluster on ACS using the Azure CLI</span></span>

1. <span data-ttu-id="e42fe-148">Az Azure Tárolószolgáltatásban Kubernetes fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e42fe-148">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="e42fe-149">A következő parancs létrehoz egy *kubernetes* a fürt a *tartománynév-kubernetes* erőforrás csoport és *tartománynév-tárolószolgáltatás* neveként a fürt, és *tartománynév-kubernetes* előtag, a DNS-ben:</span><span class="sxs-lookup"><span data-stu-id="e42fe-149">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="e42fe-150">Ez a parancs is igénybe vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="e42fe-150">This command may take a while to complete.</span></span>

1. <span data-ttu-id="e42fe-151">Telepítés `kubectl` az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="e42fe-151">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="e42fe-152">Linux-felhasználók úgy lehet, hogy ez a parancs az előtag `sudo` óta telepíti a Kubernetes CLI való `/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="e42fe-152">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="e42fe-153">Töltse le a fürt konfigurációs adatokat, kezelheti a fürt a Kubernetes webes felület, és `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="e42fe-153">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="e42fe-154">A lemezkép telepítéséhez a Kubernetes fürt</span><span class="sxs-lookup"><span data-stu-id="e42fe-154">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="e42fe-155">Ez az oktatóanyag telepíti az alkalmazást használó `kubectl`, majd engedélyezze segítségével megismerheti a telepítés a Kubernetes webes felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="e42fe-155">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="e42fe-156">A Kubernetes webes felület telepítése</span><span class="sxs-lookup"><span data-stu-id="e42fe-156">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="e42fe-157">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="e42fe-157">Open a command prompt.</span></span>

1. <span data-ttu-id="e42fe-158">Az alapértelmezett böngészőben nyissa meg a konfigurációs webhely a Kubernetes fürt:</span><span class="sxs-lookup"><span data-stu-id="e42fe-158">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="e42fe-159">Ha a Kubernetes konfigurációs webhely megnyitása a böngészőben, kattintson a hivatkozásra kattintva **indexelése alkalmazás telepítése**:</span><span class="sxs-lookup"><span data-stu-id="e42fe-159">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes konfigurációs webhely][KB01]

1. <span data-ttu-id="e42fe-161">Ha a **indexelése alkalmazás telepítése** lap is megjelenik, adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="e42fe-161">When the **Deploy a containerized app** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="e42fe-162">a.</span><span class="sxs-lookup"><span data-stu-id="e42fe-162">a.</span></span> <span data-ttu-id="e42fe-163">Válassza ki **adja meg az alkalmazás adatait az alábbi**.</span><span class="sxs-lookup"><span data-stu-id="e42fe-163">Select **Specify app details below**.</span></span>

   <span data-ttu-id="e42fe-164">b.</span><span class="sxs-lookup"><span data-stu-id="e42fe-164">b.</span></span> <span data-ttu-id="e42fe-165">A rugó rendszerindító alkalmazás nevét adja meg a **alkalmazásnév**; például: "*gs-rugó-rendszerindítás – docker*".</span><span class="sxs-lookup"><span data-stu-id="e42fe-165">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="e42fe-166">c.</span><span class="sxs-lookup"><span data-stu-id="e42fe-166">c.</span></span> <span data-ttu-id="e42fe-167">Adja meg a bejelentkezési kiszolgáló és a tároló lemezképét a korábban a **tároló kép**; például: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span><span class="sxs-lookup"><span data-stu-id="e42fe-167">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="e42fe-168">d.</span><span class="sxs-lookup"><span data-stu-id="e42fe-168">d.</span></span> <span data-ttu-id="e42fe-169">Válasszon **külső** a a **szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="e42fe-169">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="e42fe-170">e.</span><span class="sxs-lookup"><span data-stu-id="e42fe-170">e.</span></span> <span data-ttu-id="e42fe-171">Adja meg a külső és belső portjainak a **Port** és **céloz port** szövegmezőket.</span><span class="sxs-lookup"><span data-stu-id="e42fe-171">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes konfigurációs webhely][KB02]


1. <span data-ttu-id="e42fe-173">Kattintson a **telepítés** a tároló üzembe.</span><span class="sxs-lookup"><span data-stu-id="e42fe-173">Click **Deploy** to deploy the container.</span></span>

   ![Tároló üzembe][KB05]

1. <span data-ttu-id="e42fe-175">Miután az alkalmazás telepítve van, megjelenik-e a felsorolt rugó rendszerindító alkalmazás **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="e42fe-175">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes szolgáltatások][KB06]

1. <span data-ttu-id="e42fe-177">Ha a hivatkozásra kattintva **külső végpontok száma**, az Azure-on futó rugó rendszerindító alkalmazás látható.</span><span class="sxs-lookup"><span data-stu-id="e42fe-177">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes szolgáltatások][KB07]

   ![Keresse meg a mintaalkalmazás az Azure-on][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="e42fe-180">Kubectl üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="e42fe-180">Deploy with kubectl</span></span>

1. <span data-ttu-id="e42fe-181">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="e42fe-181">Open a command prompt.</span></span>

1. <span data-ttu-id="e42fe-182">A tároló a Kubernetes fürt használatával futtassa a `kubectl run` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e42fe-182">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="e42fe-183">Adja meg az alkalmazást a Kubernetes szolgáltatás nevét és a teljes kép neve.</span><span class="sxs-lookup"><span data-stu-id="e42fe-183">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="e42fe-184">Példa:</span><span class="sxs-lookup"><span data-stu-id="e42fe-184">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="e42fe-185">Ebben a parancsban:</span><span class="sxs-lookup"><span data-stu-id="e42fe-185">In this command:</span></span>

   * <span data-ttu-id="e42fe-186">A tároló neve `gs-spring-boot-docker` közvetlenül után van megadva a `run` parancs</span><span class="sxs-lookup"><span data-stu-id="e42fe-186">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="e42fe-187">A `--image` paraméter határozza meg a kombinált bejelentkezési kiszolgáló- és képfájlok név szerint`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="e42fe-187">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="e42fe-188">Külsőleg használatával teszi közzé a Kubernetes fürt a `kubectl expose` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e42fe-188">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="e42fe-189">Adja meg a szolgáltatás, az alkalmazás eléréséhez használt nyilvánosan elérhető TCP-portot, és figyeli az alkalmazás belső cél portot.</span><span class="sxs-lookup"><span data-stu-id="e42fe-189">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="e42fe-190">Példa:</span><span class="sxs-lookup"><span data-stu-id="e42fe-190">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="e42fe-191">Ebben a parancsban:</span><span class="sxs-lookup"><span data-stu-id="e42fe-191">In this command:</span></span>

   * <span data-ttu-id="e42fe-192">A tároló neve `gs-spring-boot-docker` közvetlenül után van megadva a `expose deployment` parancs</span><span class="sxs-lookup"><span data-stu-id="e42fe-192">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="e42fe-193">A `--type` paraméter határozza meg, hogy a fürt használja-e a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="e42fe-193">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="e42fe-194">A `--port` paraméter határozza meg a nyilvánosan elérhető TCP-port a 80-as.</span><span class="sxs-lookup"><span data-stu-id="e42fe-194">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="e42fe-195">Az alkalmazás ezen a porton érhető el.</span><span class="sxs-lookup"><span data-stu-id="e42fe-195">You access the app on this port.</span></span>

   * <span data-ttu-id="e42fe-196">A `--target-port` paraméter határozza meg a belső, a 8080-as TCP-portot.</span><span class="sxs-lookup"><span data-stu-id="e42fe-196">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="e42fe-197">A load balancer továbbítja a kérelmeket az alkalmazáshoz ezen a porton.</span><span class="sxs-lookup"><span data-stu-id="e42fe-197">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="e42fe-198">Miután az alkalmazás telepítve van a fürt, lekérdezni a külső IP-cím, és nyissa meg a böngészőben:</span><span class="sxs-lookup"><span data-stu-id="e42fe-198">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Keresse meg a mintaalkalmazás az Azure-on][SB02]


## <a name="next-steps"></a><span data-ttu-id="e42fe-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e42fe-200">Next steps</span></span>

<span data-ttu-id="e42fe-201">Azure rugó rendszerindítással kapcsolatos további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="e42fe-201">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="e42fe-202">Az Azure App Service egy rugó rendszerindító alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="e42fe-202">Deploy a Spring Boot Application to the Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="e42fe-203">A rugó rendszerindító alkalmazás az Azure Tárolószolgáltatásban Linux központi telepítése</span><span class="sxs-lookup"><span data-stu-id="e42fe-203">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-linux.md)

<span data-ttu-id="e42fe-204">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].</span><span class="sxs-lookup"><span data-stu-id="e42fe-204">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="e42fe-205">A Docker mintaprojektet a rugó rendszerindító kapcsolatos további információkért lásd: [rugó rendszerindító a Docker bevezetés].</span><span class="sxs-lookup"><span data-stu-id="e42fe-205">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="e42fe-206">A következő hivatkozásokra kattintva rugó rendszerindító alkalmazások létrehozásáról további információt:</span><span class="sxs-lookup"><span data-stu-id="e42fe-206">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="e42fe-207">Egy egyszerű rugó rendszerindító alkalmazás létrehozásával kapcsolatos további információkért tekintse meg a következő https://start.spring.io/ rugó Initializr.</span><span class="sxs-lookup"><span data-stu-id="e42fe-207">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="e42fe-208">A következő hivatkozásokra kattintva Kubernetes az Azure-ral való használatáról további információkat:</span><span class="sxs-lookup"><span data-stu-id="e42fe-208">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="e42fe-209">A Tárolószolgáltatás Kubernetes fürt első lépések</span><span class="sxs-lookup"><span data-stu-id="e42fe-209">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="e42fe-210">Azure Tárolószolgáltatás a Kubernetes webes felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="e42fe-210">Using the Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="e42fe-211">Kubernetes parancssori felület használatával kapcsolatos további információkat a **kubectl** felhasználói útmutatóban talál: <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="e42fe-211">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="e42fe-212">A Kubernetes webhelyhez van több olyan cikket TITKOS nyilvántartó képek használatát ismertetik, amelyek:</span><span class="sxs-lookup"><span data-stu-id="e42fe-212">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="e42fe-213">[Három munkaállomás-csoporttal fiókjainak szolgáltatás konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="e42fe-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="e42fe-214">[Névterek]</span><span class="sxs-lookup"><span data-stu-id="e42fe-214">[Namespaces]</span></span>
* <span data-ttu-id="e42fe-215">[Húzza a lemezkép egy titkos beállításjegyzékből]</span><span class="sxs-lookup"><span data-stu-id="e42fe-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="e42fe-216">Az egyéni Docker lemezképek használata Azure további példákért lásd [Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával].</span><span class="sxs-lookup"><span data-stu-id="e42fe-216">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

<span data-ttu-id="e42fe-217">[Azure parancssori felület (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="e42fe-217">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
<span data-ttu-id="e42fe-218">[Azure tároló szolgáltatás (ACS)]: https://azure.microsoft.com/services/container-service/</span><span class="sxs-lookup"><span data-stu-id="e42fe-218">[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span></span>
<span data-ttu-id="e42fe-219">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="e42fe-219">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
<span data-ttu-id="e42fe-220">[Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span><span class="sxs-lookup"><span data-stu-id="e42fe-220">[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span></span>
<span data-ttu-id="e42fe-221">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="e42fe-221">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="e42fe-222">[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="e42fe-222">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="e42fe-223">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="e42fe-223">[Git]: https://github.com/</span></span>
<span data-ttu-id="e42fe-224">[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span><span class="sxs-lookup"><span data-stu-id="e42fe-224">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span></span>
<span data-ttu-id="e42fe-225">[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="e42fe-225">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="e42fe-226">[Kubernetes]: https://kubernetes.io/</span><span class="sxs-lookup"><span data-stu-id="e42fe-226">[Kubernetes]: https://kubernetes.io/</span></span>
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
<span data-ttu-id="e42fe-227">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="e42fe-227">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="e42fe-228">[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="e42fe-228">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="e42fe-229">[rugó rendszerindító]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="e42fe-229">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="e42fe-230">[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="e42fe-230">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
<span data-ttu-id="e42fe-231">[rugó keretrendszer]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="e42fe-231">[Spring Framework]: https://spring.io/</span></span>
<span data-ttu-id="e42fe-232">[Három munkaállomás-csoporttal fiókjainak szolgáltatás konfigurálása]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/</span><span class="sxs-lookup"><span data-stu-id="e42fe-232">[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/</span></span>
<span data-ttu-id="e42fe-233">[Névterek]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/</span><span class="sxs-lookup"><span data-stu-id="e42fe-233">[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/</span></span>
<span data-ttu-id="e42fe-234">[Húzza a lemezkép egy titkos beállításjegyzékből]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/</span><span class="sxs-lookup"><span data-stu-id="e42fe-234">[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/</span></span>

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
