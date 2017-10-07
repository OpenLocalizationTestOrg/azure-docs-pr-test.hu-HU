---
title: "a rugó rendszerindító alkalmazások az Azure-tároló beállításjegyzék tooAzure App Service Azure Web Apps toodeploy aaaHow toouse hello Maven beépülő modul"
description: "Ez az oktatóanyag részletesen ismerteti, ha hello lépéseket toodeploy egy rugó rendszerindító alkalmazás az Azure-tároló beállításjegyzék tooAzure tooAzure App Service egy Maven beépülő modul használatával."
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
ms.openlocfilehash: 55b95e310c9ee186a6d77d941c5a620c2e259d8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-in-azure-container-registry-tooazure-app-service"></a><span data-ttu-id="d1c44-103">Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazást az Azure-tároló beállításjegyzék tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="d1c44-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app in Azure Container Registry tooAzure App Service</span></span>

<span data-ttu-id="d1c44-104">Hello  **[rugó keretrendszer]**  egy népszerű nyílt forráskódú keretrendszer, amely a fejlesztőket Java webes, mobil és API-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d1c44-104">hello **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="d1c44-105">Ez az oktatóanyag használja egy mintaalkalmazást használatával létrehozott [rugó rendszerindító], egy egyezmény adatvezérelt megközelítés rugó tooget használatával gyorsan lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d1c44-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring tooget started quickly.</span></span>

<span data-ttu-id="d1c44-106">Ez a cikk bemutatja, hogyan toodeploy egy minta rugó rendszerindító alkalmazás tooAzure tároló beállításjegyzék, majd használja hello Maven beépülő modul Azure Web Apps toodeploy az alkalmazás tooAzure App Service.</span><span class="sxs-lookup"><span data-stu-id="d1c44-106">This article demonstrates how toodeploy a sample Spring Boot application tooAzure Container Registry, and then use hello Maven Plugin for Azure Web Apps toodeploy your application tooAzure App Service.</span></span>

> [!NOTE]
>
> <span data-ttu-id="d1c44-107">hello Azure Web Apps Maven beépülő modul jelenleg tartozik által megtekinthető villámnézetként.</span><span class="sxs-lookup"><span data-stu-id="d1c44-107">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="d1c44-108">Egyelőre csak az FTP-közzététel támogatott, bár a további szolgáltatások későbbi hello tervbe van véve.</span><span class="sxs-lookup"><span data-stu-id="d1c44-108">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="d1c44-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d1c44-109">Prerequisites</span></span>

<span data-ttu-id="d1c44-110">A sorrend toocomplete hello lépések ebben az oktatóanyagban kell toohave hello a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="d1c44-110">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="d1c44-111">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="d1c44-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="d1c44-112">Hello [Azure parancssori felület (CLI)].</span><span class="sxs-lookup"><span data-stu-id="d1c44-112">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="d1c44-113">Egy naprakész [Java fejlesztői készlet (JDK)], 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="d1c44-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="d1c44-114">Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d1c44-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="d1c44-115">A [Git] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="d1c44-115">A [Git] client.</span></span>
* <span data-ttu-id="d1c44-116">A [Docker] ügyfél.</span><span class="sxs-lookup"><span data-stu-id="d1c44-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="d1c44-117">Ez az oktatóanyag toohello virtualizálási követelményeinek miatt nem lépések hello ebben a cikkben; virtuális gépen a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d1c44-117">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="d1c44-118">Klónozás hello minta rugó rendszerindító Docker-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="d1c44-118">Clone hello sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="d1c44-119">Ebben a szakaszban egy indexelése rugó rendszerindító alkalmazás klónozása, és helyben tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d1c44-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="d1c44-120">Nyisson meg egy parancssort vagy terminálablakot, és hozzon létre egy helyi könyvtár toohold, a rugó rendszerindító alkalmazás, és a Könyvtárváltás toothat; Példa:</span><span class="sxs-lookup"><span data-stu-id="d1c44-120">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="d1c44-121">– vagy –</span><span class="sxs-lookup"><span data-stu-id="d1c44-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="d1c44-122">Klónozás hello [rugó rendszerindító a Docker bevezetés] mintaprojektet könyvtárba, amely hello létrehozott; például:</span><span class="sxs-lookup"><span data-stu-id="d1c44-122">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="d1c44-123">Directory befejeződött toohello projekt; módosítása Példa:</span><span class="sxs-lookup"><span data-stu-id="d1c44-123">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="d1c44-124">Build Maven; használatával hello JAR-fájlra Példa:</span><span class="sxs-lookup"><span data-stu-id="d1c44-124">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="d1c44-125">Hello webalkalmazás létrehozásakor Maven; használatával hello webalkalmazás elindítása Példa:</span><span class="sxs-lookup"><span data-stu-id="d1c44-125">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="d1c44-126">Hello webes alkalmazás tesztelése a helyileg a webböngésző segítségével tooit megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="d1c44-126">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="d1c44-127">Például a következő parancsot, ha van elérhető curl hello használata:</span><span class="sxs-lookup"><span data-stu-id="d1c44-127">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="d1c44-128">A következő üzenet jelenik meg hello kell megjelennie: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="d1c44-128">You should see hello following message displayed: **Hello Docker World**</span></span>

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="d1c44-130">Hozzon létre egy Azure szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="d1c44-130">Create an Azure service principal</span></span>

<span data-ttu-id="d1c44-131">Ez a szakasz hozzon létre egy Azure hello Maven beépülő modul által használt, a tároló tooAzure telepítésekor egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d1c44-131">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="d1c44-132">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="d1c44-132">Open a command prompt.</span></span>

1. <span data-ttu-id="d1c44-133">Jelentkezzen be az Azure-fiók használatával hello Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="d1c44-133">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="d1c44-134">Hajtsa végre a hello utasításokat toocomplete hello bejelentkezési folyamat.</span><span class="sxs-lookup"><span data-stu-id="d1c44-134">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="d1c44-135">Hozzon létre egy Azure-szolgáltatás egyszerű:</span><span class="sxs-lookup"><span data-stu-id="d1c44-135">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="d1c44-136">Ha `uuuuuuuu` hello felhasználónév és `pppppppp` hello szolgáltatás egyszerű hello-jelszó.</span><span class="sxs-lookup"><span data-stu-id="d1c44-136">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="d1c44-137">Azure válaszol, a következő példa hello levő JSON:</span><span class="sxs-lookup"><span data-stu-id="d1c44-137">Azure responds with JSON that resembles hello following example:</span></span>
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
   > <span data-ttu-id="d1c44-138">A JSON-válasz hello értékeket fogja használni, a tároló tooAzure hello Maven beépülő modul toodeploy konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="d1c44-138">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your container tooAzure.</span></span> <span data-ttu-id="d1c44-139">Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, és `tttttttt` helyőrző értékeket, amelyek vannak használatban a példa toomake azt könnyebb toomap ezen értékek tootheir megfelelő elemek a Maven konfigurálásakor `settings.xml` hello a következő fájl a szakasz.</span><span class="sxs-lookup"><span data-stu-id="d1c44-139">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a><span data-ttu-id="d1c44-140">Hozzon létre egy Azure tároló beállításjegyzék hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d1c44-140">Create an Azure Container Registry using hello Azure CLI</span></span>

1. <span data-ttu-id="d1c44-141">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="d1c44-141">Open a command prompt.</span></span>

1. <span data-ttu-id="d1c44-142">Jelentkezzen be tooyour Azure-fiók:</span><span class="sxs-lookup"><span data-stu-id="d1c44-142">Log in tooyour Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="d1c44-143">Hozzon létre egy erőforráscsoportot hello Azure-erőforrások fogja használni a cikkben:</span><span class="sxs-lookup"><span data-stu-id="d1c44-143">Create a resource group for hello Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="d1c44-144">Cserélje le `wingtiptoysresources` ebben a példában az erőforráscsoport számára egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="d1c44-144">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="d1c44-145">Hozzon létre egy saját Azure-tárolót beállításjegyzék hello erőforráscsoport rugó rendszerindító alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="d1c44-145">Create a private Azure container registry in hello resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="d1c44-146">Cserélje le `wingtiptoysregistry` ebben a példában a tároló beállításjegyzék egyedi nevére.</span><span class="sxs-lookup"><span data-stu-id="d1c44-146">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="d1c44-147">Lekéri a tároló beállításjegyzék hello jelszavát:</span><span class="sxs-lookup"><span data-stu-id="d1c44-147">Retrieve hello password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="d1c44-148">Azure válaszol, és a jelszavát; Példa:</span><span class="sxs-lookup"><span data-stu-id="d1c44-148">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-tooyour-maven-settings"></a><span data-ttu-id="d1c44-149">Adja hozzá az Azure-tárolót beállításjegyzék és az Azure szolgáltatás egyszerű tooyour Maven beállításai</span><span class="sxs-lookup"><span data-stu-id="d1c44-149">Add your Azure container registry and Azure service principal tooyour Maven settings</span></span>

1. <span data-ttu-id="d1c44-150">Nyissa meg a Maven `settings.xml` fájlt egy szövegszerkesztőben; lehet, hogy a fájl olyan elérési útja, például a következő példák hello:</span><span class="sxs-lookup"><span data-stu-id="d1c44-150">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="d1c44-151">Az Azure-tároló beállításjegyzék-hozzáférési beállítások hozzáadása az előző szakaszából hello Ez a cikk toohello `<servers>` hello gyűjtemény *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="d1c44-151">Add your Azure Container Registry access settings from hello previous section of this article toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="d1c44-152">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="d1c44-152">Where:</span></span>
   <span data-ttu-id="d1c44-153">Elem</span><span class="sxs-lookup"><span data-stu-id="d1c44-153">Element</span></span> | <span data-ttu-id="d1c44-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1c44-154">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="d1c44-155">A saját Azure-tárolót beállításjegyzék hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d1c44-155">Contains hello name of your private Azure container registry.</span></span>
   `<username>` | <span data-ttu-id="d1c44-156">A saját Azure-tárolót beállításjegyzék hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d1c44-156">Contains hello name of your private Azure container registry.</span></span>
   `<password>` | <span data-ttu-id="d1c44-157">Ez a cikk korábbi részében hello lekért hello jelszót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d1c44-157">Contains hello password you retrieved in hello previous section of this article.</span></span>

1. <span data-ttu-id="d1c44-158">Ez a cikk toohello egy korábbi szakaszában az Azure-szolgáltatás egyszerű beállítás hozzáadása `<servers>` hello gyűjtemény *settings.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="d1c44-158">Add your Azure service principal settings from an earlier section of this article toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="d1c44-159">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="d1c44-159">Where:</span></span>
   <span data-ttu-id="d1c44-160">Elem</span><span class="sxs-lookup"><span data-stu-id="d1c44-160">Element</span></span> | <span data-ttu-id="d1c44-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1c44-161">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="d1c44-162">Adja meg egy egyedi nevet, amely Maven toolook a biztonsági beállításokat használja, a webes alkalmazás tooAzure központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d1c44-162">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="d1c44-163">Hello tartalmaz `appId` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="d1c44-163">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="d1c44-164">Hello tartalmaz `tenant` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="d1c44-164">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="d1c44-165">Hello tartalmaz `password` a szolgáltatás egyszerű közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="d1c44-165">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="d1c44-166">Hello Azure felhőben célkörnyezet, amely definiálja `AZURE` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="d1c44-166">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="d1c44-167">(Környezetek teljes listája megtalálható hello [Maven beépülő modul Azure Web Apps] dokumentációja)</span><span class="sxs-lookup"><span data-stu-id="d1c44-167">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="d1c44-168">Mentse és zárja be a hello *settings.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="d1c44-168">Save and close hello *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-tooyour-azure-container-registry"></a><span data-ttu-id="d1c44-169">A Docker-tároló lemezképet létre, és leküldeni tooyour Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="d1c44-169">Build your Docker container image and push it tooyour Azure container registry</span></span>

1. <span data-ttu-id="d1c44-170">Keresse meg a rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában (pl. "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), és nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="d1c44-170">Navigate toohello completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="d1c44-171">Frissítés hello `<properties>` hello gyűjtemény *pom.xml* hello bejelentkezési kiszolgáló értéke az hello Ez az oktatóanyag előző szakaszából a Azure tároló rendszerleíró fájlt például:</span><span class="sxs-lookup"><span data-stu-id="d1c44-171">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry from hello previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="d1c44-172">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="d1c44-172">Where:</span></span>
   <span data-ttu-id="d1c44-173">Elem</span><span class="sxs-lookup"><span data-stu-id="d1c44-173">Element</span></span> | <span data-ttu-id="d1c44-174">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1c44-174">Description</span></span>
   ---|---|---
   `<azure.containerRegistry>` | <span data-ttu-id="d1c44-175">A saját Azure-tárolót beállításjegyzék hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d1c44-175">Specifies hello name of your private Azure container registry.</span></span>
   `<docker.image.prefix>` | <span data-ttu-id="d1c44-176">Adja meg a saját Azure-tárolót a rendszerleíró adatbázis hozzáfűzésével származtatott hello URL-címe ". azurecr.io" a személyes tárolót beállításjegyzék toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="d1c44-176">Specifies hello URL of your private Azure container registry, which is derived by appending ".azurecr.io" toohello name of your private container registry.</span></span>

1. <span data-ttu-id="d1c44-177">Ellenőrizze, hogy `<plugin>` hello Docker beépülő modul a számára a *pom.xml* fájl hello bejelentkezési cím és a beállításjegyzék kiszolgálónév hello előző lépésben hello megfelelő tulajdonságait tartalmazza az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d1c44-177">Verify that `<plugin>` for hello Docker plugin in your *pom.xml* file contains hello correct properties for hello login server address and registry name from hello previous step in this tutorial.</span></span> <span data-ttu-id="d1c44-178">Példa:</span><span class="sxs-lookup"><span data-stu-id="d1c44-178">For example:</span></span>

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
   <span data-ttu-id="d1c44-179">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="d1c44-179">Where:</span></span>
   <span data-ttu-id="d1c44-180">Elem</span><span class="sxs-lookup"><span data-stu-id="d1c44-180">Element</span></span> | <span data-ttu-id="d1c44-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1c44-181">Description</span></span>
   ---|---|---
   `<serverId>` | <span data-ttu-id="d1c44-182">Adja meg azt a hello tulajdonságot, mely tartalmazza a saját Azure-tárolót beállításjegyzék nevét.</span><span class="sxs-lookup"><span data-stu-id="d1c44-182">Specifies hello property which contains name of your private Azure container registry.</span></span>
   `<registryUrl>` | <span data-ttu-id="d1c44-183">Adja meg a saját Azure-tárolót beállításjegyzék hello URL-CÍMÉT tartalmazó hello tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d1c44-183">Specifies hello property which contains hello URL of your private Azure container registry.</span></span>

1. <span data-ttu-id="d1c44-184">A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg és futtassa a következő parancs toorebuild hello alkalmazás hello és hello tároló tooyour Azure tároló beállításjegyzék leküldéses:</span><span class="sxs-lookup"><span data-stu-id="d1c44-184">Navigate toohello completed project directory for your Spring Boot application and run hello following command toorebuild hello application and push hello container tooyour Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

1. <span data-ttu-id="d1c44-185">Választható lehetőség: Tallózás toohello [Azure-portálon] , és ellenőrizze, hogy van-e Docker-tároló kép nevű **gs-rugó-rendszerindítás – docker** a tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="d1c44-185">OPTIONAL: Browse toohello [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![Ellenőrizze a tároló az Azure-portálon][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-tooazure"></a><span data-ttu-id="d1c44-187">Testre szabhatja a pom.xml, majd és üzembe helyezheti a tároló tooAzure</span><span class="sxs-lookup"><span data-stu-id="d1c44-187">Customize your pom.xml, then build and deploy your container tooAzure</span></span>

<span data-ttu-id="d1c44-188">Nyissa meg hello `pom.xml` fájlt egy szövegszerkesztőben rugó rendszerindító alkalmazás, és keresse a hello `<plugin>` elem `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="d1c44-188">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="d1c44-189">Ez az elem a következő példa hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="d1c44-189">This element should resemble hello following example:</span></span>

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

<span data-ttu-id="d1c44-190">Több érték, amely hello Maven beépülő modul módosíthatja, és az ilyen elemek részletes leírását érhető el hello [Maven beépülő modul Azure Web Apps] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d1c44-190">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="d1c44-191">Amely éppen említett, több érték, amelyek érdemes ebben a cikkben kiemelve:</span><span class="sxs-lookup"><span data-stu-id="d1c44-191">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="d1c44-192">Elem</span><span class="sxs-lookup"><span data-stu-id="d1c44-192">Element</span></span> | <span data-ttu-id="d1c44-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1c44-193">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="d1c44-194">Adja meg a hello hello verziója [Maven beépülő modul Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="d1c44-194">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="d1c44-195">Ellenőrizni kell a felsorolt hello hello verzió [Maven központi tárház](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure, Ön által használt hello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="d1c44-195">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="d1c44-196">Adja meg a hello hitelesítési adatokat az Azure-hoz, amely ebben a példában tartalmaz egy `<serverId>` elem, amely tartalmazza `azure-auth`; Maven adott érték toolook hello Azure szolgáltatás egyszerű értékeket használja a Maven *settings.xml* fájl, ez a cikk korábbi részében megadott.</span><span class="sxs-lookup"><span data-stu-id="d1c44-196">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="d1c44-197">Adja meg a hello célként megadott erőforráscsoportja, amely `wingtiptoysresources` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="d1c44-197">Specifies hello target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="d1c44-198">hello erőforráscsoport üzembe helyezése során létrejön, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="d1c44-198">hello resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="d1c44-199">A webalkalmazás hello cél nevét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d1c44-199">Specifies hello target name for your web app.</span></span> <span data-ttu-id="d1c44-200">Ebben a példában hello cél neve van `maven-linux-app-${maven.build.timestamp}`, ahol hello `${maven.build.timestamp}` utótagot fűz hozzá a példa tooavoid ütközik.</span><span class="sxs-lookup"><span data-stu-id="d1c44-200">In this example, hello target name is `maven-linux-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="d1c44-201">(hello időbélyeg választható; akkor is megadhat bármilyen hello alkalmazásnév egyedi karakterláncot.)</span><span class="sxs-lookup"><span data-stu-id="d1c44-201">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="d1c44-202">Adja meg a cél hello régió, amely ebben a példában `westus`.</span><span class="sxs-lookup"><span data-stu-id="d1c44-202">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="d1c44-203">(Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)</span><span class="sxs-lookup"><span data-stu-id="d1c44-203">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<containerSettings>` | <span data-ttu-id="d1c44-204">Hello tulajdonságokat tartalmazó hello nevét és a tároló URL-CÍMÉT adja meg.</span><span class="sxs-lookup"><span data-stu-id="d1c44-204">Specifies hello properties which contain hello name and URL of your container.</span></span>
`<appSettings>` | <span data-ttu-id="d1c44-205">Meghatározza a Maven toouse bármely egyedi beállításait, a webes alkalmazás tooAzure telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d1c44-205">Specifies any unique settings for Maven toouse when deploying your web app tooAzure.</span></span> <span data-ttu-id="d1c44-206">Ebben a példában egy `<property>` elem név-érték párból, amelyek adja meg az alkalmazás hello portot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d1c44-206">In this example, a `<property>` element contains a name/value pair of child elements that specify hello port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="d1c44-207">hello beállítások toochange hello portszám ebben a példában csak akkor szükség, ha hello port hello alapértelmezett módosítani.</span><span class="sxs-lookup"><span data-stu-id="d1c44-207">hello settings toochange hello port number in this example are only necessary when you are changing hello port from hello default.</span></span>
>

1. <span data-ttu-id="d1c44-208">Hello parancssort vagy terminálablakot korábban használt, építse újra a Maven használatával, ha bármely módosítások toohello hello JAR-fájlra *pom.xml* fájl; például:</span><span class="sxs-lookup"><span data-stu-id="d1c44-208">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="d1c44-209">A webes alkalmazás tooAzure telepíteni a Maven; Példa:</span><span class="sxs-lookup"><span data-stu-id="d1c44-209">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="d1c44-210">Maven telepíti a webes alkalmazás tooAzure; Ha hello webalkalmazás már nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="d1c44-210">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="d1c44-211">Ha hello régió hello megadható `<region>` eleme a *pom.xml* fájl nem rendelkezik elegendő kiszolgáló érhető el, ha a telepítés megkezdése, egy hasonló toohello hiba, a következő példában láthatja:</span><span class="sxs-lookup"><span data-stu-id="d1c44-211">If hello region which you specify in hello `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar toohello following example:</span></span>
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
> <span data-ttu-id="d1c44-212">Ha ez történik, adjon meg egy másik régióban, és futtassa újra a hello Maven parancs toodeploy az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d1c44-212">If this happens, you can specify another region and re-run hello Maven command toodeploy your application.</span></span>
>
>

<span data-ttu-id="d1c44-213">A webes telepítésekor fogja tudni toomanage azt hello segítségével [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="d1c44-213">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="d1c44-214">A webalkalmazás megjelenik **alkalmazásszolgáltatások**:</span><span class="sxs-lookup"><span data-stu-id="d1c44-214">Your web app will be listed in **App Services**:</span></span>

   ![A webes alkalmazás szerepel az Azure-portálon App Service szolgáltatások][AP01]

* <span data-ttu-id="d1c44-216">Hello URL-címet a hello megjelenik a webalkalmazás és **áttekintése** webalkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="d1c44-216">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

   ![A webalkalmazás URL-címe hello meghatározása][AP02]

## <a name="next-steps"></a><span data-ttu-id="d1c44-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1c44-218">Next steps</span></span>

<span data-ttu-id="d1c44-219">Ebben a cikkben ismertetett különböző technológiákkal hello további információ: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="d1c44-219">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="d1c44-220">[Maven beépülő modul Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="d1c44-220">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="d1c44-221">Jelentkezzen be az Azure CLI hello tooAzure</span><span class="sxs-lookup"><span data-stu-id="d1c44-221">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="d1c44-222">Hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d1c44-222">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="d1c44-223">Maven-beállítások referenciája</span><span class="sxs-lookup"><span data-stu-id="d1c44-223">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="d1c44-224">[Docker Maven beépülő modul]</span><span class="sxs-lookup"><span data-stu-id="d1c44-224">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[Maven beépülő modul Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Docker Maven beépülő modul]: https://github.com/spotify/docker-maven-plugin
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
