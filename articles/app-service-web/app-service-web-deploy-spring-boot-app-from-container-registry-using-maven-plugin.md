---
title: "Az Azure Web Apps Maven beépülő modul használata Azure tároló beállításjegyzék rugó rendszerindító alkalmazás telepítése az Azure App Service"
description: "Ez az oktatóanyag részletesen ismerteti, ha egy Azure-tároló beállításjegyzék rugó rendszerindító alkalmazás telepítési Azure az Azure App Service egy Maven beépülő modul használatával."
services: 
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
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: f47ee59d72ea49d62be2cb435ebaf8bc841e4198
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a><span data-ttu-id="c703a-103">Az Azure Web Apps Maven beépülő modul használata Azure tároló beállításjegyzék rugó rendszerindító alkalmazás telepítése az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c703a-103">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app in Azure Container Registry to Azure App Service</span></span>

<span data-ttu-id="c703a-104">A  **[rugó keretrendszer]**  egy népszerű nyílt forráskódú keretrendszer, amely a fejlesztőket Java webes, mobil és API-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c703a-104">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="c703a-105">Ez az oktatóanyag használja egy mintaalkalmazást használatával létrehozott [rugó rendszerindító], rugó használatával történő gyors használatbavétel a egyezmény adatvezérelt megközelítést.</span><span class="sxs-lookup"><span data-stu-id="c703a-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="c703a-106">Ez a cikk bemutatja, hogyan Azure tároló beállításjegyzék rugó rendszerindító mintaalkalmazás telepítése, és az Azure Web Apps Maven beépülő modul segítségével az Azure App Service alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="c703a-106">This article demonstrates how to deploy a sample Spring Boot application to Azure Container Registry, and then use the Maven Plugin for Azure Web Apps to deploy your application to Azure App Service.</span></span>

> [!NOTE]
>
> <span data-ttu-id="c703a-107">Az Azure Web Apps Maven beépülő modul jelenleg tartozik által megtekinthető villámnézetként.</span><span class="sxs-lookup"><span data-stu-id="c703a-107">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="c703a-108">Egyelőre csak az FTP-közzététel támogatott, bár a jövőben további funkciók tervbe van véve.</span><span class="sxs-lookup"><span data-stu-id="c703a-108">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="c703a-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c703a-109">Prerequisites</span></span>

<span data-ttu-id="c703a-110">Az oktatóanyagban szereplő lépések végrehajtásához kell rendelkeznie a következő előfeltételek teljesülését:</span><span class="sxs-lookup"><span data-stu-id="c703a-110">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="c703a-111">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="c703a-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="c703a-112">A [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="c703a-112">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="c703a-113">Egy naprakész [Java fejlesztői készlet (JDK)], 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c703a-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="c703a-114">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c703a-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="c703a-115">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="c703a-115">A [Git] client.</span></span>
* <span data-ttu-id="c703a-116">A [Docker] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="c703a-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="c703a-117">Ez az oktatóanyag virtualizálási követelményeinek, mert; virtuális gépen nem kövesse a cikkben leírt lépéseket a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c703a-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="c703a-118">A minta rugó rendszerindító Docker webalkalmazásban klónozása</span><span class="sxs-lookup"><span data-stu-id="c703a-118">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="c703a-119">Ebben a szakaszban egy indexelése rugó rendszerindító alkalmazás klónozása, és helyben tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c703a-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="c703a-120">Nyisson meg egy parancssort vagy terminálablakot, és hozzon létre egy helyi könyvtárat a rugó rendszerindító alkalmazás tárolására, és módosítsa a könyvtárhoz; Példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="c703a-121">– vagy –</span><span class="sxs-lookup"><span data-stu-id="c703a-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="c703a-122">Klónozott a [rugó rendszerindító a Docker bevezetés] mintaprojektet a könyvtárba, amely létrehozta; például:</span><span class="sxs-lookup"><span data-stu-id="c703a-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="c703a-123">Módosítsa a könyvtárat a befejezett projekthez; Példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="c703a-124">Build Maven; használatával JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="c703a-125">Ha a webalkalmazás létrejött, indítsa el a webalkalmazást, Maven; használatával Példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="c703a-126">A webes alkalmazás tesztelése a azt helyileg a webböngésző segítségével.</span><span class="sxs-lookup"><span data-stu-id="c703a-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="c703a-127">Használhatja például a következő parancsot, ha a curl érhető el:</span><span class="sxs-lookup"><span data-stu-id="c703a-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="c703a-128">A következő üzenet jelenik meg: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="c703a-128">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="c703a-130">Hozzon létre egy Azure szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="c703a-130">Create an Azure service principal</span></span>

<span data-ttu-id="c703a-131">Ez a szakasz hozzon létre egy Azure szolgáltatás egyszerű a Maven beépülő modult használó Azure a tárolót telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="c703a-131">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="c703a-132">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="c703a-132">Open a command prompt.</span></span>

1. <span data-ttu-id="c703a-133">Jelentkezzen be az Azure-fiókot az Azure parancssori felület használatával:</span><span class="sxs-lookup"><span data-stu-id="c703a-133">Sign into your Azure account by using the Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="c703a-134">Kövesse az utasításokat a bejelentkezési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c703a-134">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="c703a-135">Hozzon létre egy Azure-szolgáltatás egyszerű:</span><span class="sxs-lookup"><span data-stu-id="c703a-135">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="c703a-136">Ha `uuuuuuuu` a felhasználónév és `pppppppp` pedig a jelszót a szolgáltatás rendszerbiztonsági tag.</span><span class="sxs-lookup"><span data-stu-id="c703a-136">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="c703a-137">Azure JSON válaszol, a következőhöz hasonló: a következő példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-137">Azure responds with JSON that resembles the following example:</span></span>
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="c703a-138">Ez a JSON-válasz értékeinek fogja használni, a Maven beépülő modul a tároló üzembe az Azure-ba való konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="c703a-138">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="c703a-139">A `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, és `tttttttt` helyőrző értékek, ebben a példában használt könnyebben ezeket az értékeket leképezi a megfelelő elemeket a Maven konfigurálásakor `settings.xml` fájl a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="c703a-139">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="c703a-140">Hozzon létre egy Azure-tároló beállításjegyzék, az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="c703a-140">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="c703a-141">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="c703a-141">Open a command prompt.</span></span>

1. <span data-ttu-id="c703a-142">Jelentkezzen be az Azure-fiókjával:</span><span class="sxs-lookup"><span data-stu-id="c703a-142">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="c703a-143">Ez a cikk hozzon létre egy erőforráscsoportot az Azure-erőforrások fogja használni:</span><span class="sxs-lookup"><span data-stu-id="c703a-143">Create a resource group for the Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="c703a-144">Cserélje le `wingtiptoysresources` ebben a példában az erőforráscsoport számára egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="c703a-144">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="c703a-145">Hozzon létre egy saját Azure-tárolót beállításjegyzék az erőforráscsoport rugó rendszerindító alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="c703a-145">Create a private Azure container registry in the resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="c703a-146">Cserélje le `wingtiptoysregistry` ebben a példában a tároló beállításjegyzék egyedi nevére.</span><span class="sxs-lookup"><span data-stu-id="c703a-146">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="c703a-147">Lekéri a tároló beállításjegyzék jelszavát:</span><span class="sxs-lookup"><span data-stu-id="c703a-147">Retrieve the password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="c703a-148">Azure válaszol, és a jelszavát; Példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-148">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a><span data-ttu-id="c703a-149">Adja hozzá az Azure-tárolót beállításjegyzék és Azure egyszerű szolgáltatásnév a Maven-beállítások</span><span class="sxs-lookup"><span data-stu-id="c703a-149">Add your Azure container registry and Azure service principal to your Maven settings</span></span>

1. <span data-ttu-id="c703a-150">Nyissa meg a Maven `settings.xml` fájlt egy szövegszerkesztőben; lehet, hogy a fájl olyan elérési útja, például az alábbi példákat:</span><span class="sxs-lookup"><span data-stu-id="c703a-150">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="c703a-151">Az ebben a cikkben előző szakaszából a Azure tároló beállításjegyzék-hozzáférési beállítások hozzáadása a `<servers>` gyűjtemény a *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="c703a-151">Add your Azure Container Registry access settings from the previous section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="c703a-152">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="c703a-152">Where:</span></span>
   <span data-ttu-id="c703a-153">Elem</span><span class="sxs-lookup"><span data-stu-id="c703a-153">Element</span></span> | <span data-ttu-id="c703a-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="c703a-154">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="c703a-155">A saját Azure-tárolót beállításjegyzék nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c703a-155">Contains the name of your private Azure container registry.</span></span>
   `<username>` | <span data-ttu-id="c703a-156">A saját Azure-tárolót beállításjegyzék nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c703a-156">Contains the name of your private Azure container registry.</span></span>
   `<password>` | <span data-ttu-id="c703a-157">A jelszót, ez a cikk az előző szakaszban lekért tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c703a-157">Contains the password you retrieved in the previous section of this article.</span></span>

1. <span data-ttu-id="c703a-158">Ebben a cikkben egy korábbi szakaszában az Azure szolgáltatás egyszerű beállítás hozzáadása a `<servers>` gyűjtemény a *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="c703a-158">Add your Azure service principal settings from an earlier section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   <span data-ttu-id="c703a-159">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="c703a-159">Where:</span></span>
   <span data-ttu-id="c703a-160">Elem</span><span class="sxs-lookup"><span data-stu-id="c703a-160">Element</span></span> | <span data-ttu-id="c703a-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="c703a-161">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="c703a-162">Adja meg egy egyedi nevet, amely Maven használja a webalkalmazás az Azure-ba való telepítésekor a biztonsági beállítások kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="c703a-162">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="c703a-163">Tartalmazza a `appId` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="c703a-163">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="c703a-164">Tartalmazza a `tenant` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="c703a-164">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="c703a-165">Tartalmazza a `password` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="c703a-165">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="c703a-166">Határozza meg a cél Azure felhőalapú környezet, amely `AZURE` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="c703a-166">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="c703a-167">(A környezetek teljes listája megtalálható a [Maven beépülő modul Azure Web Apps] dokumentációja)</span><span class="sxs-lookup"><span data-stu-id="c703a-167">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="c703a-168">Mentse és zárja be a *settings.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="c703a-168">Save and close the *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a><span data-ttu-id="c703a-169">A Docker-tároló lemezképet létre, és hogy az Azure-tárolót beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="c703a-169">Build your Docker container image and push it to your Azure container registry</span></span>

1. <span data-ttu-id="c703a-170">Keresse meg a befejezett projekt könyvtárát a rugó rendszerindító alkalmazás (pl. "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), és nyissa meg a *pom.xml* szöveggel fájl szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="c703a-170">Navigate to the completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="c703a-171">Frissítés a `<properties>` gyűjtemény a *pom.xml* Ez az oktatóanyag előző szakaszából a Azure tároló beállításjegyzék bejelentkezési kiszolgáló értékű fájlt például:</span><span class="sxs-lookup"><span data-stu-id="c703a-171">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="c703a-172">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="c703a-172">Where:</span></span>
   <span data-ttu-id="c703a-173">Elem</span><span class="sxs-lookup"><span data-stu-id="c703a-173">Element</span></span> | <span data-ttu-id="c703a-174">Leírás</span><span class="sxs-lookup"><span data-stu-id="c703a-174">Description</span></span>
   ---|---|---
   `<azure.containerRegistry>` | <span data-ttu-id="c703a-175">Megadja a saját Azure-tárolót beállításjegyzék nevét.</span><span class="sxs-lookup"><span data-stu-id="c703a-175">Specifies the name of your private Azure container registry.</span></span>
   `<docker.image.prefix>` | <span data-ttu-id="c703a-176">Meghatározza a URL-címet a saját Azure-tárolót beállításjegyzékről, hozzáfűzésével származtatott ". azurecr.io" a személyes tárolót beállításjegyzék nevét.</span><span class="sxs-lookup"><span data-stu-id="c703a-176">Specifies the URL of your private Azure container registry, which is derived by appending ".azurecr.io" to the name of your private container registry.</span></span>

1. <span data-ttu-id="c703a-177">Ellenőrizze, hogy `<plugin>` a Docker beépülő modul a *pom.xml* fájl ebben az oktatóanyagban a bejelentkezési cím és a beállításjegyzék kiszolgálónév az előző lépésben megfelelő tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c703a-177">Verify that `<plugin>` for the Docker plugin in your *pom.xml* file contains the correct properties for the login server address and registry name from the previous step in this tutorial.</span></span> <span data-ttu-id="c703a-178">Példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-178">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   <span data-ttu-id="c703a-179">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="c703a-179">Where:</span></span>
   <span data-ttu-id="c703a-180">Elem</span><span class="sxs-lookup"><span data-stu-id="c703a-180">Element</span></span> | <span data-ttu-id="c703a-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="c703a-181">Description</span></span>
   ---|---|---
   `<serverId>` | <span data-ttu-id="c703a-182">Adja meg azt a tulajdonságot, mely tartalmazza a saját Azure-tárolót beállításjegyzék nevét.</span><span class="sxs-lookup"><span data-stu-id="c703a-182">Specifies the property which contains name of your private Azure container registry.</span></span>
   `<registryUrl>` | <span data-ttu-id="c703a-183">Megadja azt a tulajdonságot, amely tartalmazza a saját Azure-tárolót beállításjegyzék URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c703a-183">Specifies the property which contains the URL of your private Azure container registry.</span></span>

1. <span data-ttu-id="c703a-184">Keresse meg a befejezett projekt könyvtárát rugó rendszerindító alkalmazás, és építse újra az alkalmazást, és küldje le a tároló az Azure-tárolót beállításjegyzék a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c703a-184">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

1. <span data-ttu-id="c703a-185">Választható lehetőség: Keresse meg a [Azure-portálon] , és ellenőrizze, hogy van-e Docker-tároló kép nevű **gs-rugó-rendszerindítás – docker** a tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="c703a-185">OPTIONAL: Browse to the [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![Ellenőrizze a tároló az Azure-portálon][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a><span data-ttu-id="c703a-187">Testre szabhatja a pom.xml majd build és a tároló üzembe az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="c703a-187">Customize your pom.xml, then build and deploy your container to Azure</span></span>

<span data-ttu-id="c703a-188">Nyissa meg a `pom.xml` fájlt egy szövegszerkesztőben rugó rendszerindító alkalmazás, és keresse meg a `<plugin>` elem `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="c703a-188">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="c703a-189">Ez az elem az alábbihoz kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="c703a-189">This element should resemble the following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="c703a-190">Több érték, amely a Maven beépülő modul módosíthatja, és az ilyen elemek részletes leírását is elérhető a [Maven beépülő modul Azure Web Apps] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="c703a-190">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="c703a-191">Amely éppen említett, több érték, amelyek érdemes ebben a cikkben kiemelve:</span><span class="sxs-lookup"><span data-stu-id="c703a-191">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="c703a-192">Elem</span><span class="sxs-lookup"><span data-stu-id="c703a-192">Element</span></span> | <span data-ttu-id="c703a-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="c703a-193">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="c703a-194">Verzióját adja meg a [Maven beépülő modul Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="c703a-194">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="c703a-195">A felsorolt verzió ellenőrizni kell a [Maven központi tárház](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) annak érdekében, hogy a legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="c703a-195">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="c703a-196">Megadja a hitelesítő adatok az Azure-hoz, amely ebben a példában tartalmaz egy `<serverId>` elem, amely tartalmazza `azure-auth`; Maven használja ezt az értéket kereséséhez az Azure szolgáltatás egyszerű értékek a a Mavenben *settings.xml* fájl, ez a cikk korábbi részében megadott.</span><span class="sxs-lookup"><span data-stu-id="c703a-196">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="c703a-197">Adja meg a célként megadott erőforráscsoportja, amely `wingtiptoysresources` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="c703a-197">Specifies the target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="c703a-198">Az erőforráscsoport üzembe helyezése során létrejön, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="c703a-198">The resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="c703a-199">A cél neve a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c703a-199">Specifies the target name for your web app.</span></span> <span data-ttu-id="c703a-200">Ebben a példában a cél neve: `maven-linux-app-${maven.build.timestamp}`, ahol a `${maven.build.timestamp}` utótagot fűz hozzá a ütközés elkerülése érdekében ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="c703a-200">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="c703a-201">(Az időbélyeg nem kötelező, megadhatja, hogy minden egyedi karakterláncot az alkalmazás nevére.)</span><span class="sxs-lookup"><span data-stu-id="c703a-201">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="c703a-202">Megadja a cél régió nevét, amely ebben a példában `westus`.</span><span class="sxs-lookup"><span data-stu-id="c703a-202">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="c703a-203">(Teljes listája megtalálható a [Maven beépülő modul Azure Web Apps] dokumentációját.)</span><span class="sxs-lookup"><span data-stu-id="c703a-203">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<containerSettings>` | <span data-ttu-id="c703a-204">A tulajdonságokat tartalmazó nevét és a tároló URL-CÍMÉT adja meg.</span><span class="sxs-lookup"><span data-stu-id="c703a-204">Specifies the properties which contain the name and URL of your container.</span></span>
`<appSettings>` | <span data-ttu-id="c703a-205">Meghatározza a webalkalmazás telepítése az Azure-bA használandó Maven bármely egyedi beállításait.</span><span class="sxs-lookup"><span data-stu-id="c703a-205">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="c703a-206">Ebben a példában egy `<property>` elem tartalmazza-e a név-érték pár, amelyek adja meg a portot, az alkalmazás a.</span><span class="sxs-lookup"><span data-stu-id="c703a-206">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="c703a-207">Módosítsa a portszámot, ebben a példában a beállítások csak akkor szükség, ha a port módosítani az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="c703a-207">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

1. <span data-ttu-id="c703a-208">A parancssort vagy terminálablakot korábban használt, építse újra a Maven használatával, ha a módosítás JAR-fájlra a *pom.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="c703a-208">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="c703a-209">A webalkalmazás telepítése az Azure használatával Maven; Példa:</span><span class="sxs-lookup"><span data-stu-id="c703a-209">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="c703a-210">Maven telepíti a webalkalmazás Azure; Ha a webalkalmazás már nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="c703a-210">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="c703a-211">Ha a régiót, amelyet meg a `<region>` eleme a *pom.xml* fájl nem rendelkezik elegendő kiszolgáló érhető el, ha a telepítés megkezdése, láthatja az alábbi példához hasonló hiba:</span><span class="sxs-lookup"><span data-stu-id="c703a-211">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="c703a-212">Ha ez történik, adjon meg egy másik régióban, és futtassa újra a Maven-parancsot az alkalmazás közzétételéhez.</span><span class="sxs-lookup"><span data-stu-id="c703a-212">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="c703a-213">Ha a webes van telepítve, akkor fog tudni segítségével kezeli a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c703a-213">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="c703a-214">A webalkalmazás megjelenik **alkalmazásszolgáltatások**:</span><span class="sxs-lookup"><span data-stu-id="c703a-214">Your web app will be listed in **App Services**:</span></span>

   ![A webes alkalmazás szerepel az Azure-portálon App Service szolgáltatások][AP01]

* <span data-ttu-id="c703a-216">A webalkalmazás URL-címe megjelenik, és a **áttekintése** webalkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="c703a-216">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![A webalkalmazás URL-Címének meghatározása][AP02]

## <a name="next-steps"></a><span data-ttu-id="c703a-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c703a-218">Next steps</span></span>

<span data-ttu-id="c703a-219">A cikkben említett különböző technológiákkal kapcsolatos további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="c703a-219">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="c703a-220">[Maven beépülő modul Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="c703a-220">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="c703a-221">Jelentkezzen be az Azure az az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="c703a-221">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="c703a-222">Hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c703a-222">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="c703a-223">Maven-beállítások referenciája</span><span class="sxs-lookup"><span data-stu-id="c703a-223">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="c703a-224">[Docker Maven beépülő modul]</span><span class="sxs-lookup"><span data-stu-id="c703a-224">[Docker plugin for Maven]</span></span>

<!-- URL List -->

<span data-ttu-id="c703a-225">[Azure parancssori felület (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="c703a-225">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
<span data-ttu-id="c703a-226">[Azure-portálon]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="c703a-226">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="c703a-227">[Maven beépülő modul Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="c703a-227">[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span></span>
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
<span data-ttu-id="c703a-228">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="c703a-228">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="c703a-229">[Docker Maven beépülő modul]: https://github.com/spotify/docker-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="c703a-229">[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin</span></span>
<span data-ttu-id="c703a-230">[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="c703a-230">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="c703a-231">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="c703a-231">[Git]: https://github.com/</span></span>
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
<span data-ttu-id="c703a-232">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="c703a-232">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="c703a-233">[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="c703a-233">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="c703a-234">[rugó rendszerindító]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="c703a-234">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="c703a-235">[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="c703a-235">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
<span data-ttu-id="c703a-236">[rugó keretrendszer]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="c703a-236">[Spring Framework]: https://spring.io/</span></span>

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
