---
title: "Azure Web Apps toodeploy egy indexelése rugó rendszerindító alkalmazás tooAzure aaaHow toouse hello Maven beépülő modul"
description: "Ismerje meg, hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazás tooAzure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: e7e760d4ef5bd4c92a4126a50a2b12e5c8f2b4a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-containerized-spring-boot-app-tooazure"></a><span data-ttu-id="81a52-103">Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy indexelése rugó rendszerindító alkalmazás tooAzure</span><span class="sxs-lookup"><span data-stu-id="81a52-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a containerized Spring Boot app tooAzure</span></span>

<span data-ttu-id="81a52-104">Hello [Maven beépülő modul Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) a [Apache Maven](http://maven.apache.org/) Maven-projektek az Azure App Service zökkenőmentes integrációt biztosít, és megkönnyíti a fejlesztők toodeploy web Apps hello folyamat App Service tooAzure.</span><span class="sxs-lookup"><span data-stu-id="81a52-104">hello [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines hello process for developers toodeploy web apps tooAzure App Service .</span></span>

<span data-ttu-id="81a52-105">Ez a cikk bemutatja az Azure Web Apps toodeploy hello Maven beépülő modul használatával egy Docker-tároló tooAzure alkalmazásszolgáltatások a mintaalkalmazás rugó rendszerindító.</span><span class="sxs-lookup"><span data-stu-id="81a52-105">This article demonstrates using hello Maven Plugin for Azure Web Apps toodeploy a sample Spring Boot application in a Docker container tooAzure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="81a52-106">hello Azure Web Apps Maven beépülő modul jelenleg tartozik által megtekinthető villámnézetként.</span><span class="sxs-lookup"><span data-stu-id="81a52-106">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="81a52-107">Egyelőre csak az FTP-közzététel támogatott, bár a további szolgáltatások későbbi hello tervbe van véve.</span><span class="sxs-lookup"><span data-stu-id="81a52-107">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="81a52-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="81a52-108">Prerequisites</span></span>

<span data-ttu-id="81a52-109">A sorrend toocomplete hello lépések ebben az oktatóanyagban kell toohave hello a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="81a52-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="81a52-110">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="81a52-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="81a52-111">Hello [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="81a52-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="81a52-112">Egy naprakész [Java fejlesztői készlet (JDK)], 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="81a52-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="81a52-113">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81a52-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="81a52-114">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="81a52-114">A [Git] client.</span></span>
* <span data-ttu-id="81a52-115">A [Docker] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="81a52-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="81a52-116">Ez az oktatóanyag toohello virtualizálási követelményeinek miatt nem lépések hello ebben a cikkben; virtuális gépen a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.</span><span class="sxs-lookup"><span data-stu-id="81a52-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="81a52-117">Klónozás hello minta rugó rendszerindító Docker-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="81a52-117">Clone hello sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="81a52-118">Ebben a szakaszban egy indexelése rugó rendszerindító alkalmazás klónozása, és helyben tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="81a52-118">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="81a52-119">Nyisson meg egy parancssort vagy terminálablakot, és hozzon létre egy helyi könyvtár toohold, a rugó rendszerindító alkalmazás, és a Könyvtárváltás toothat; Példa:</span><span class="sxs-lookup"><span data-stu-id="81a52-119">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="81a52-120">– vagy –</span><span class="sxs-lookup"><span data-stu-id="81a52-120">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="81a52-121">Klónozás hello [rugó rendszerindító a Docker bevezetés] mintaprojektet könyvtárba, amely hello létrehozott; például:</span><span class="sxs-lookup"><span data-stu-id="81a52-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="81a52-122">Directory befejeződött toohello projekt; módosítása Példa:</span><span class="sxs-lookup"><span data-stu-id="81a52-122">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="81a52-123">Build Maven; használatával hello JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="81a52-123">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="81a52-124">Hello webalkalmazás létrehozásakor Maven; használatával hello webalkalmazás elindítása Példa:</span><span class="sxs-lookup"><span data-stu-id="81a52-124">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="81a52-125">Hello webes alkalmazás tesztelése a helyileg a webböngésző segítségével tooit megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="81a52-125">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="81a52-126">Például a következő parancsot, ha van elérhető curl hello használata:</span><span class="sxs-lookup"><span data-stu-id="81a52-126">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="81a52-127">A következő üzenet jelenik meg hello kell megjelennie: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="81a52-127">You should see hello following message displayed: **Hello Docker World**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="81a52-128">Hozzon létre egy Azure szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="81a52-128">Create an Azure service principal</span></span>

<span data-ttu-id="81a52-129">Ez a szakasz hozzon létre egy Azure hello Maven beépülő modul által használt, a tároló tooAzure telepítésekor egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="81a52-129">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="81a52-130">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="81a52-130">Open a command prompt.</span></span>

1. <span data-ttu-id="81a52-131">Jelentkezzen be az Azure-fiók használatával hello Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="81a52-131">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="81a52-132">Hajtsa végre a hello utasításokat toocomplete hello bejelentkezési folyamat.</span><span class="sxs-lookup"><span data-stu-id="81a52-132">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="81a52-133">Hozzon létre egy Azure-szolgáltatás egyszerű:</span><span class="sxs-lookup"><span data-stu-id="81a52-133">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="81a52-134">Ha `uuuuuuuu` hello felhasználónév és `pppppppp` hello szolgáltatás egyszerű hello-jelszó.</span><span class="sxs-lookup"><span data-stu-id="81a52-134">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="81a52-135">Azure válaszol, a következő példa hello levő JSON:</span><span class="sxs-lookup"><span data-stu-id="81a52-135">Azure responds with JSON that resembles hello following example:</span></span>
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
   > <span data-ttu-id="81a52-136">A JSON-válasz hello értékeket fogja használni, a tároló tooAzure hello Maven beépülő modul toodeploy konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="81a52-136">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your container tooAzure.</span></span> <span data-ttu-id="81a52-137">Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, és `tttttttt` helyőrző értékeket, amelyek vannak használatban a példa toomake azt könnyebb toomap ezen értékek tootheir megfelelő elemek a Maven konfigurálásakor `settings.xml` hello a következő fájl a szakasz.</span><span class="sxs-lookup"><span data-stu-id="81a52-137">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a><span data-ttu-id="81a52-138">Maven toouse konfigurálása az Azure szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="81a52-138">Configure Maven toouse your Azure service principal</span></span>

<span data-ttu-id="81a52-139">Ebben a szakaszban a hello értékek a az Azure szolgáltatás egyszerű tooconfigure hello authentication Maven által használt, a tároló tooAzure telepítésekor használhat.</span><span class="sxs-lookup"><span data-stu-id="81a52-139">In this section, you use hello values from your Azure service principal tooconfigure hello authentication that Maven will use when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="81a52-140">Nyissa meg a Maven `settings.xml` fájlt egy szövegszerkesztőben; lehet, hogy a fájl olyan elérési útja, például a következő példák hello:</span><span class="sxs-lookup"><span data-stu-id="81a52-140">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="81a52-141">Az Azure-szolgáltatás egyszerű beállítások hozzáadása az előző szakaszából hello ezen oktatóanyag toohello `<servers>` hello gyűjtemény *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="81a52-141">Add your Azure service principal settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="81a52-142">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="81a52-142">Where:</span></span>
   <span data-ttu-id="81a52-143">Elem</span><span class="sxs-lookup"><span data-stu-id="81a52-143">Element</span></span> | <span data-ttu-id="81a52-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="81a52-144">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="81a52-145">Adja meg egy egyedi nevet, amely Maven toolook a biztonsági beállításokat használja, a webes alkalmazás tooAzure központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="81a52-145">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="81a52-146">Hello tartalmaz `appId` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="81a52-146">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="81a52-147">Hello tartalmaz `tenant` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="81a52-147">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="81a52-148">Hello tartalmaz `password` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="81a52-148">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="81a52-149">Hello Azure felhőben célkörnyezet, amely definiálja `AZURE` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="81a52-149">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="81a52-150">(Környezetek teljes listája megtalálható hello [Maven beépülő modul Azure Web Apps] dokumentációja)</span><span class="sxs-lookup"><span data-stu-id="81a52-150">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="81a52-151">Mentse és zárja be a hello *settings.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="81a52-151">Save and close hello *settings.xml* file.</span></span>

## <a name="optional-deploy-your-local-docker-file-toodocker-hub"></a><span data-ttu-id="81a52-152">Választható lehetőség: A helyi Docker-fájl tooDocker központi telepítése</span><span class="sxs-lookup"><span data-stu-id="81a52-152">OPTIONAL: Deploy your local Docker file tooDocker Hub</span></span>

<span data-ttu-id="81a52-153">Ha egy Docker-fiókot, létrehozhatja a Docker tároló kép helyileg és leküldeni tooDocker Hub.</span><span class="sxs-lookup"><span data-stu-id="81a52-153">If you have a Docker account, you can build your Docker container image locally and push it tooDocker Hub.</span></span> <span data-ttu-id="81a52-154">toodo Igen, használja a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="81a52-154">toodo so, use hello following steps.</span></span>

1. <span data-ttu-id="81a52-155">Nyissa meg hello `pom.xml` fájlt egy szövegszerkesztőben rugó rendszerindító alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="81a52-155">Open hello `pom.xml` file for your Spring Boot application in a text editor.</span></span>

1. <span data-ttu-id="81a52-156">Keresse meg a hello `<imageName>` hello gyermekeleme `<containerSettings>` elemet.</span><span class="sxs-lookup"><span data-stu-id="81a52-156">Locate hello `<imageName>` child element of hello `<containerSettings>` element.</span></span>

1. <span data-ttu-id="81a52-157">Frissítés hello `${docker.image.prefix}` érték és a Docker-fiók neve:</span><span class="sxs-lookup"><span data-stu-id="81a52-157">Update hello `${docker.image.prefix}` value with your Docker account name:</span></span>
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. <span data-ttu-id="81a52-158">A következő központi telepítési módszerekkel hello közül választhat:</span><span class="sxs-lookup"><span data-stu-id="81a52-158">Choose one of hello following deployment methods:</span></span>

   * <span data-ttu-id="81a52-159">A tároló lemezkép helyileg fejlesztheti az Maven, és a Docker toopush a tároló tooDocker Hub:</span><span class="sxs-lookup"><span data-stu-id="81a52-159">Build your container image locally with Maven, and then use Docker toopush your container tooDocker Hub:</span></span>
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * <span data-ttu-id="81a52-160">Ha hello [Docker beépülő modul Maven] automatikusan létrehozhat telepítve, és a tároló kép tooDocker központ használatával hello `-DpushImage` paraméter:</span><span class="sxs-lookup"><span data-stu-id="81a52-160">If you have hello [Docker plugin for Maven] installed, you can automatically build and your container image tooDocker Hub by using hello `-DpushImage` parameter:</span></span>
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-tooazure"></a><span data-ttu-id="81a52-161">Választható lehetőség: Testre szabhatja a pom.xml a tároló tooAzure telepítése előtt</span><span class="sxs-lookup"><span data-stu-id="81a52-161">OPTIONAL: Customize your pom.xml before deploying your container tooAzure</span></span>

<span data-ttu-id="81a52-162">Nyissa meg hello `pom.xml` fájlt egy szövegszerkesztőben rugó rendszerindító alkalmazás, és keresse a hello `<plugin>` elem `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="81a52-162">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="81a52-163">Ez az elem a következő példa hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="81a52-163">This element should resemble hello following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
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

<span data-ttu-id="81a52-164">Több érték, amely hello Maven beépülő modul módosíthatja, és az ilyen elemek részletes leírását érhető el hello [Maven beépülő modul Azure Web Apps] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="81a52-164">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="81a52-165">Amely éppen említett, több érték, amelyek érdemes ebben a cikkben kiemelve:</span><span class="sxs-lookup"><span data-stu-id="81a52-165">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="81a52-166">Elem</span><span class="sxs-lookup"><span data-stu-id="81a52-166">Element</span></span> | <span data-ttu-id="81a52-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="81a52-167">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="81a52-168">Adja meg a hello hello verziója [Maven beépülő modul Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="81a52-168">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="81a52-169">Ellenőrizni kell a felsorolt hello hello verzió [Maven központi tárház](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure, Ön által használt hello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="81a52-169">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="81a52-170">Adja meg a hello hitelesítési adatokat az Azure-hoz, amely ebben a példában tartalmaz egy `<serverId>` elem, amely tartalmazza `azure-auth`; Maven adott érték toolook hello Azure szolgáltatás egyszerű értékeket használja a Maven *settings.xml* fájl, ez a cikk korábbi részében megadott.</span><span class="sxs-lookup"><span data-stu-id="81a52-170">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="81a52-171">Adja meg a hello célként megadott erőforráscsoportja, amely `maven-plugin` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="81a52-171">Specifies hello target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="81a52-172">hello erőforráscsoport üzembe helyezése során létrejön, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="81a52-172">hello resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="81a52-173">A webalkalmazás hello cél nevét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="81a52-173">Specifies hello target name for your web app.</span></span> <span data-ttu-id="81a52-174">Ebben a példában hello cél neve van `maven-linux-app-${maven.build.timestamp}`, ahol hello `${maven.build.timestamp}` utótagot fűz hozzá a példa tooavoid ütközik.</span><span class="sxs-lookup"><span data-stu-id="81a52-174">In this example, hello target name is `maven-linux-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="81a52-175">(hello időbélyeg választható; akkor is megadhat bármilyen hello alkalmazásnév egyedi karakterláncot.)</span><span class="sxs-lookup"><span data-stu-id="81a52-175">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="81a52-176">Adja meg a cél hello régió, amely ebben a példában `westus`.</span><span class="sxs-lookup"><span data-stu-id="81a52-176">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="81a52-177">(Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)</span><span class="sxs-lookup"><span data-stu-id="81a52-177">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<appSettings>` | <span data-ttu-id="81a52-178">Meghatározza a Maven toouse bármely egyedi beállításait, a webes alkalmazás tooAzure telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="81a52-178">Specifies any unique settings for Maven toouse when deploying your web app tooAzure.</span></span> <span data-ttu-id="81a52-179">Ebben a példában egy `<property>` elem név-érték párból, amelyek adja meg az alkalmazás hello portot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="81a52-179">In this example, a `<property>` element contains a name/value pair of child elements that specify hello port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="81a52-180">hello beállítások toochange hello portszám ebben a példában csak akkor szükség, ha hello port hello alapértelmezett módosítani.</span><span class="sxs-lookup"><span data-stu-id="81a52-180">hello settings toochange hello port number in this example are only necessary when you are changing hello port from hello default.</span></span>
>

## <a name="build-and-deploy-your-container-tooazure"></a><span data-ttu-id="81a52-181">Hozza létre, és a tároló tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="81a52-181">Build and deploy your container tooAzure</span></span>

<span data-ttu-id="81a52-182">Amennyiben az összes hello beállítás állított be a cikk szakaszaiban megelőző hello, készen áll a toodeploy áll a tároló tooAzure.</span><span class="sxs-lookup"><span data-stu-id="81a52-182">Once you have configured all of hello settings in hello preceding sections of this article, you are ready toodeploy your container tooAzure.</span></span> <span data-ttu-id="81a52-183">toodo Igen, használja a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="81a52-183">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="81a52-184">Hello parancssort vagy terminálablakot korábban használt, építse újra a Maven használatával, ha bármely módosítások toohello hello JAR-fájlra *pom.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="81a52-184">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="81a52-185">A webes alkalmazás tooAzure telepíteni a Maven; Példa:</span><span class="sxs-lookup"><span data-stu-id="81a52-185">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="81a52-186">Maven telepíti a webes alkalmazás tooAzure; Ha hello webalkalmazás már nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="81a52-186">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="81a52-187">Ha hello régió hello megadható `<region>` eleme a *pom.xml* fájl nem rendelkezik elegendő kiszolgáló érhető el, ha a telepítés megkezdése, egy hasonló toohello hiba, a következő példában láthatja:</span><span class="sxs-lookup"><span data-stu-id="81a52-187">If hello region which you specify in hello `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar toohello following example:</span></span>
>
> ```
> [INFO] Start deploying tooWeb App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed tooexecute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="81a52-188">Ha ez történik, adjon meg egy másik régióban, és futtassa újra a hello Maven parancs toodeploy az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="81a52-188">If this happens, you can specify another region and re-run hello Maven command toodeploy your application.</span></span>
>
>

<span data-ttu-id="81a52-189">A webes telepítésekor fogja tudni toomanage azt hello segítségével [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="81a52-189">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="81a52-190">A webalkalmazás megjelenik **alkalmazásszolgáltatások**:</span><span class="sxs-lookup"><span data-stu-id="81a52-190">Your web app will be listed in **App Services**:</span></span>

   ![A webes alkalmazás szerepel az Azure-portálon App Service szolgáltatások][AP01]

* <span data-ttu-id="81a52-192">Hello URL-címet a hello megjelenik a webalkalmazás és **áttekintése** webalkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="81a52-192">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

   ![A webalkalmazás URL-címe hello meghatározása][AP02]

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

## <a name="next-steps"></a><span data-ttu-id="81a52-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81a52-194">Next steps</span></span>

<span data-ttu-id="81a52-195">Ebben a cikkben ismertetett különböző technológiákkal hello további információ: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="81a52-195">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="81a52-196">[Maven beépülő modul Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="81a52-196">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="81a52-197">Jelentkezzen be az Azure CLI hello tooAzure</span><span class="sxs-lookup"><span data-stu-id="81a52-197">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="81a52-198">Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazás tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="81a52-198">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app tooAzure App Service </span></span>](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="81a52-199">Hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="81a52-199">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="81a52-200">Maven-beállítások referenciája</span><span class="sxs-lookup"><span data-stu-id="81a52-200">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="81a52-201">[Docker beépülő modul Maven]</span><span class="sxs-lookup"><span data-stu-id="81a52-201">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Docker beépülő modul Maven]: https://github.com/spotify/docker-maven-plugin
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven beépülő modul Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
