---
title: "a rugó rendszerindító alkalmazás tooAzure Azure Web Apps toodeploy aaaHow toouse hello Maven beépülő modul"
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
ms.openlocfilehash: 376fe90fe20621e15d7c9856214937c78b66026a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-tooazure"></a><span data-ttu-id="05765-103">Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazás tooAzure</span><span class="sxs-lookup"><span data-stu-id="05765-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app tooAzure</span></span>

<span data-ttu-id="05765-104">Hello [Maven beépülő modul Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) a [Apache Maven](http://maven.apache.org/) Maven-projektek az Azure App Service zökkenőmentes integrációt biztosít, és megkönnyíti a fejlesztők toodeploy web Apps hello folyamat App Service tooAzure.</span><span class="sxs-lookup"><span data-stu-id="05765-104">hello [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines hello process for developers toodeploy web apps tooAzure App Service.</span></span>

<span data-ttu-id="05765-105">Ez a cikk bemutatja az Azure Web Apps toodeploy hello Maven beépülő modul használatával egy minta rugó rendszerindító alkalmazás tooAzure alkalmazásszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="05765-105">This article demonstrates using hello Maven Plugin for Azure Web Apps toodeploy a sample Spring Boot application tooAzure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="05765-106">hello Azure Web Apps Maven beépülő modul jelenleg tartozik által megtekinthető villámnézetként.</span><span class="sxs-lookup"><span data-stu-id="05765-106">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="05765-107">Egyelőre csak az FTP-közzététel támogatott, bár a további szolgáltatások későbbi hello tervbe van véve.</span><span class="sxs-lookup"><span data-stu-id="05765-107">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="05765-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="05765-108">Prerequisites</span></span>

<span data-ttu-id="05765-109">A sorrend toocomplete hello lépések ebben az oktatóanyagban kell toohave hello a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="05765-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="05765-110">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="05765-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="05765-111">Hello [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="05765-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="05765-112">Egy naprakész [Java fejlesztői készlet (JDK)], 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="05765-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="05765-113">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="05765-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="05765-114">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="05765-114">A [Git] client.</span></span>

## <a name="clone-hello-sample-spring-boot-web-app"></a><span data-ttu-id="05765-115">Klónozás hello rugó rendszerindító webes mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="05765-115">Clone hello sample Spring Boot web app</span></span>

<span data-ttu-id="05765-116">Ebben a szakaszban egy befejezett rugó rendszerindító alkalmazás klónozása, és helyben tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="05765-116">In this section, you clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="05765-117">Nyisson meg egy parancssort vagy terminálablakot, és hozzon létre egy helyi könyvtár toohold, a rugó rendszerindító alkalmazás, és a Könyvtárváltás toothat; Példa:</span><span class="sxs-lookup"><span data-stu-id="05765-117">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="05765-118">– vagy –</span><span class="sxs-lookup"><span data-stu-id="05765-118">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="05765-119">Klónozás hello [rugó rendszerindító bevezetés] mintaprojektet könyvtárba, amely hello létrehozott; például:</span><span class="sxs-lookup"><span data-stu-id="05765-119">Clone hello [Spring Boot Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="05765-120">Directory befejeződött toohello projekt; módosítása Példa:</span><span class="sxs-lookup"><span data-stu-id="05765-120">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="05765-121">Build Maven; használatával hello JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="05765-121">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="05765-122">Hello webalkalmazás létrehozásakor Maven; használatával hello webalkalmazás elindítása Példa:</span><span class="sxs-lookup"><span data-stu-id="05765-122">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="05765-123">Hello webes alkalmazás tesztelése a helyileg a webböngésző segítségével tooit megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="05765-123">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="05765-124">Például a következő parancsot, ha van elérhető curl hello használata:</span><span class="sxs-lookup"><span data-stu-id="05765-124">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="05765-125">A következő üzenet jelenik meg hello kell megjelennie: **hónap rugó rendszerindításból!**</span><span class="sxs-lookup"><span data-stu-id="05765-125">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="05765-126">Hozzon létre egy Azure szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="05765-126">Create an Azure service principal</span></span>

<span data-ttu-id="05765-127">Ez a szakasz hozzon létre egy Azure hello Maven beépülő modul által használt, a webes alkalmazás tooAzure telepítésekor egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="05765-127">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your web app tooAzure.</span></span>

1. <span data-ttu-id="05765-128">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="05765-128">Open a command prompt.</span></span>

1. <span data-ttu-id="05765-129">Jelentkezzen be az Azure-fiók használatával hello Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="05765-129">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="05765-130">Hajtsa végre a hello utasításokat toocomplete hello bejelentkezési folyamat.</span><span class="sxs-lookup"><span data-stu-id="05765-130">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="05765-131">Hozzon létre egy Azure-szolgáltatás egyszerű:</span><span class="sxs-lookup"><span data-stu-id="05765-131">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="05765-132">Ha `uuuuuuuu` hello felhasználónév és `pppppppp` hello szolgáltatás egyszerű hello-jelszó.</span><span class="sxs-lookup"><span data-stu-id="05765-132">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="05765-133">Azure válaszol, a következő példa hello levő JSON:</span><span class="sxs-lookup"><span data-stu-id="05765-133">Azure responds with JSON that resembles hello following example:</span></span>
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
   > <span data-ttu-id="05765-134">A JSON-válasz hello értékeket fogja használni, a webes alkalmazás tooAzure hello Maven beépülő modul toodeploy konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="05765-134">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your web app tooAzure.</span></span> <span data-ttu-id="05765-135">Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, és `tttttttt` helyőrző értékeket, amelyek vannak használatban a példa toomake azt könnyebb toomap ezen értékek tootheir megfelelő elemek a Maven konfigurálásakor `settings.xml` hello a következő fájl a szakasz.</span><span class="sxs-lookup"><span data-stu-id="05765-135">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a><span data-ttu-id="05765-136">Maven toouse konfigurálása az Azure szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="05765-136">Configure Maven toouse your Azure service principal</span></span>

<span data-ttu-id="05765-137">Ebben a szakaszban a hello értékek a az Azure-szolgáltatás egyszerű tooconfigure hello authentication Maven használó a webes alkalmazás tooAzure telepítésekor használhat.</span><span class="sxs-lookup"><span data-stu-id="05765-137">In this section, you use hello values from your Azure service principal tooconfigure hello authentication that Maven uses when deploying your web app tooAzure.</span></span>

1. <span data-ttu-id="05765-138">Nyissa meg a Maven `settings.xml` fájlt egy szövegszerkesztőben; lehet, hogy a fájl olyan elérési útja, például a következő példák hello:</span><span class="sxs-lookup"><span data-stu-id="05765-138">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="05765-139">Az Azure-szolgáltatás egyszerű beállítások hozzáadása az előző szakaszából hello ezen oktatóanyag toohello `<servers>` hello gyűjtemény *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="05765-139">Add your Azure service principal settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="05765-140">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="05765-140">Where:</span></span>
   <span data-ttu-id="05765-141">Elem</span><span class="sxs-lookup"><span data-stu-id="05765-141">Element</span></span> | <span data-ttu-id="05765-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="05765-142">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="05765-143">Adja meg egy egyedi nevet, amely Maven toolook a biztonsági beállításokat használja, a webes alkalmazás tooAzure központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="05765-143">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="05765-144">Hello tartalmaz `appId` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="05765-144">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="05765-145">Hello tartalmaz `tenant` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="05765-145">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="05765-146">Hello tartalmaz `password` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="05765-146">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="05765-147">Hello Azure felhőben célkörnyezet, amely definiálja `AZURE` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="05765-147">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="05765-148">(Környezetek teljes listája megtalálható hello [Maven beépülő modul Azure Web Apps] dokumentációja)</span><span class="sxs-lookup"><span data-stu-id="05765-148">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="05765-149">Mentse és zárja be a hello *settings.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="05765-149">Save and close hello *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-tooazure"></a><span data-ttu-id="05765-150">Választható lehetőség: Testre szabhatja a pom.xml a webes alkalmazás tooAzure telepítése előtt</span><span class="sxs-lookup"><span data-stu-id="05765-150">OPTIONAL: Customize your pom.xml before deploying your web app tooAzure</span></span>

<span data-ttu-id="05765-151">Nyissa meg hello `pom.xml` fájlt egy szövegszerkesztőben rugó rendszerindító alkalmazás, és keresse a hello `<plugin>` elem `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="05765-151">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="05765-152">Ez az elem a következő példa hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="05765-152">This element should resemble hello following example:</span></span>

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="05765-153">Több érték, amely hello Maven beépülő modul módosíthatja, és az ilyen elemek részletes leírását érhető el hello [Maven beépülő modul Azure Web Apps] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="05765-153">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="05765-154">Amely éppen említett, több érték, amelyek érdemes ebben a cikkben kiemelve:</span><span class="sxs-lookup"><span data-stu-id="05765-154">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="05765-155">Elem</span><span class="sxs-lookup"><span data-stu-id="05765-155">Element</span></span> | <span data-ttu-id="05765-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="05765-156">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="05765-157">Adja meg a hello hello verziója [Maven beépülő modul Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="05765-157">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="05765-158">Ellenőrizni kell a felsorolt hello hello verzió [Maven központi tárház](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure, Ön által használt hello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="05765-158">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="05765-159">Adja meg a hello hitelesítési adatokat az Azure-hoz, amely ebben a példában tartalmaz egy `<serverId>` elem, amely tartalmazza `azure-auth`; Maven adott érték toolook hello Azure szolgáltatás egyszerű értékeket használja a Maven *settings.xml* fájl, ez a cikk korábbi részében megadott.</span><span class="sxs-lookup"><span data-stu-id="05765-159">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="05765-160">Adja meg a hello célként megadott erőforráscsoportja, amely `maven-plugin` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="05765-160">Specifies hello target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="05765-161">hello erőforráscsoport a telepítés során jön létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="05765-161">hello resource group is created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="05765-162">A webalkalmazás hello cél nevét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="05765-162">Specifies hello target name for your web app.</span></span> <span data-ttu-id="05765-163">Ebben a példában hello cél neve van `maven-web-app-${maven.build.timestamp}`, ahol hello `${maven.build.timestamp}` utótagot fűz hozzá a példa tooavoid ütközik.</span><span class="sxs-lookup"><span data-stu-id="05765-163">In this example, hello target name is `maven-web-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="05765-164">(hello időbélyeg választható; akkor is megadhat bármilyen hello alkalmazásnév egyedi karakterláncot.)</span><span class="sxs-lookup"><span data-stu-id="05765-164">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="05765-165">Adja meg a cél hello régió, amely ebben a példában `westus`.</span><span class="sxs-lookup"><span data-stu-id="05765-165">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="05765-166">(Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)</span><span class="sxs-lookup"><span data-stu-id="05765-166">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<javaVersion>` | <span data-ttu-id="05765-167">Megadja a webalkalmazás hello Java futásidejű verzióját.</span><span class="sxs-lookup"><span data-stu-id="05765-167">Specifies hello Java runtime version for your web app.</span></span> <span data-ttu-id="05765-168">(Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)</span><span class="sxs-lookup"><span data-stu-id="05765-168">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<deploymentType>` | <span data-ttu-id="05765-169">Adja meg a webalkalmazás központi telepítési típus.</span><span class="sxs-lookup"><span data-stu-id="05765-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="05765-170">Egyelőre csak `ftp` támogatott, bár a más központi telepítési típusok a fejlesztési támogatja.</span><span class="sxs-lookup"><span data-stu-id="05765-170">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span>
`<resources>` | <span data-ttu-id="05765-171">Megadja az erőforrások és a célként megadott célhelyekre Maven használja, a webes alkalmazás tooAzure telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="05765-171">Specifies resources and target destinations which Maven uses when deploying your web app tooAzure.</span></span> <span data-ttu-id="05765-172">Ebben a példában két `<resource>` elemet adja meg, hogy Maven helyezik üzembe a hello JAR-fájlra a webes alkalmazáshoz és hello *web.config* fájl hello rugó rendszerindító projektből.</span><span class="sxs-lookup"><span data-stu-id="05765-172">In this example, two `<resource>` elements specify that Maven will deploy hello JAR file for your web app and hello *web.config* file from hello Spring Boot project.</span></span>

## <a name="build-and-deploy-your-web-app-tooazure"></a><span data-ttu-id="05765-173">Hozza létre, és a webes alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="05765-173">Build and deploy your web app tooAzure</span></span>

<span data-ttu-id="05765-174">Amennyiben az összes hello beállítás állított be a cikk szakaszaiban megelőző hello, készen áll a toodeploy áll a webes alkalmazás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="05765-174">Once you have configured all of hello settings in hello preceding sections of this article, you are ready toodeploy your web app tooAzure.</span></span> <span data-ttu-id="05765-175">toodo Igen, használja a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="05765-175">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="05765-176">Hello parancssort vagy terminálablakot korábban használt, építse újra a Maven használatával, ha bármely módosítások toohello hello JAR-fájlra *pom.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="05765-176">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="05765-177">A webes alkalmazás tooAzure telepíteni a Maven; Példa:</span><span class="sxs-lookup"><span data-stu-id="05765-177">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="05765-178">Maven telepíti a webes alkalmazás tooAzure; Ha hello webalkalmazás már nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="05765-178">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

<span data-ttu-id="05765-179">A webes telepítésekor fogja tudni toomanage azt hello segítségével [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="05765-179">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="05765-180">A webalkalmazás megjelenik **alkalmazásszolgáltatások**:</span><span class="sxs-lookup"><span data-stu-id="05765-180">Your web app will be listed in **App Services**:</span></span>

   ![A webes alkalmazás szerepel az Azure-portálon App Service szolgáltatások][AP01]

* <span data-ttu-id="05765-182">Hello URL-címet a hello megjelenik a webalkalmazás és **áttekintése** webalkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="05765-182">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="05765-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05765-184">Next steps</span></span>

<span data-ttu-id="05765-185">Ebben a cikkben ismertetett különböző technológiákkal hello további információ: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="05765-185">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="05765-186">[Maven beépülő modul Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="05765-186">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="05765-187">Jelentkezzen be az Azure CLI hello tooAzure</span><span class="sxs-lookup"><span data-stu-id="05765-187">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="05765-188">Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy indexelése rugó rendszerindító alkalmazás tooAzure</span><span class="sxs-lookup"><span data-stu-id="05765-188">How toouse hello Maven Plugin for Azure Web Apps toodeploy a containerized Spring Boot app tooAzure</span></span>](app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="05765-189">Hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="05765-189">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="05765-190">Maven-beállítások referenciája</span><span class="sxs-lookup"><span data-stu-id="05765-190">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[rugó rendszerindító bevezetés]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven beépülő modul Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
