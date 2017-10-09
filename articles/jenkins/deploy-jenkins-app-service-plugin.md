---
title: "aaaDeploy tooAzure Jenkins beépülő modul az App Service |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure App Service Jenkins beépülő modul toodeploy Java web app tooAzure a Jenkins"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 080be7277555ce7d688dccdf38eef309e7a7b194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-plugin"></a><span data-ttu-id="7a605-103">App Service tooAzure rendelkező Jenkins beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="7a605-103">Deploy tooAzure App Service with Jenkins plugin</span></span> 
<span data-ttu-id="7a605-104">a Java web app tooAzure toodeploy, használhatja az Azure CLI [Jenkins csővezeték](/azure/jenkins/execute-cli-jenkins-pipeline) lehetőség hello használata [Azure App Service Jenkins beépülő modul](https://plugins.jenkins.io/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="7a605-104">toodeploy a Java web app tooAzure, you can use Azure CLI in [Jenkins Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) or you can make use of hello [Azure App Service Jenkins plugin](https://plugins.jenkins.io/azure-app-service).</span></span> <span data-ttu-id="7a605-105">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="7a605-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a605-106">Jenkins toodeploy tooAzure keresztül FTP App Service konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a605-106">Configure Jenkins toodeploy tooAzure App Service through FTP</span></span> 
> * <span data-ttu-id="7a605-107">Linux-rendszeren keresztül Docker Jenkins toodeploy tooAzure App Service konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a605-107">Configure Jenkins toodeploy tooAzure App Service on Linux through Docker</span></span> 

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="7a605-108">Hozzon létre és Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a605-108">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="7a605-109">Ha még nem rendelkezik egy Jenkins master, kezdje hello [Megoldássablonban](install-jenkins-solution-template.md), mely tartalmazza a JDK8 és hello szükséges beépülő modulok a következő:</span><span class="sxs-lookup"><span data-stu-id="7a605-109">If you do not already have a Jenkins master, start with hello [Solution Template](install-jenkins-solution-template.md), which includes JDK8 and hello following required plugins:</span></span>

* <span data-ttu-id="7a605-110">[Jenkins Git ügyfél beépülő modul](https://plugins.jenkins.io/git-client) v.2.4.6</span><span class="sxs-lookup"><span data-stu-id="7a605-110">[Jenkins Git client plugin](https://plugins.jenkins.io/git-client) v.2.4.6</span></span> 
* <span data-ttu-id="7a605-111">[Docker Commons beépülő modul](https://plugins.jenkins.io/docker-commons) v.1.4.0</span><span class="sxs-lookup"><span data-stu-id="7a605-111">[Docker Commons Plugin](https://plugins.jenkins.io/docker-commons) v.1.4.0</span></span>
* <span data-ttu-id="7a605-112">[Azure hitelesítő adataival](https://plugins.jenkins.io/azure-credentials) v.1.2</span><span class="sxs-lookup"><span data-stu-id="7a605-112">[Azure Credentials](https://plugins.jenkins.io/azure-credentials) v.1.2</span></span>
* <span data-ttu-id="7a605-113">[Az Azure App Service](https://plugins.jenkins.io/azure-app-server) v.0.1</span><span class="sxs-lookup"><span data-stu-id="7a605-113">[Azure App Service](https://plugins.jenkins.io/azure-app-server) v.0.1</span></span>

<span data-ttu-id="7a605-114">Hello App Service beépülő modul toodeploy webalkalmazást az Azure App Service által támogatott összes nyelven (például C#, PHP, Java és node.js stb.) is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7a605-114">You can use hello App Service plugin toodeploy Web App in all languages (for instance, C#, PHP, Java, and node.js etc.) supported by Azure App Service.</span></span> <span data-ttu-id="7a605-115">Az oktatóanyag Java hello mintaalkalmazást, használunk [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample).</span><span class="sxs-lookup"><span data-stu-id="7a605-115">In this tutorial, we are using hello sample Java app, [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample).</span></span> <span data-ttu-id="7a605-116">toofork hello tárház tooyour saját GitHub-fiók, kattintson a hello **elágazás** hello jobb felső sarkában található gombra.</span><span class="sxs-lookup"><span data-stu-id="7a605-116">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>  

<span data-ttu-id="7a605-117">Java JDK és Maven szükségesek hello Java projekt.</span><span class="sxs-lookup"><span data-stu-id="7a605-117">Java JDK and Maven are required for building hello Java project.</span></span> <span data-ttu-id="7a605-118">Ellenőrizze, hogy hello összetevőinek telepítését hello Jenkins fő vagy a Virtuálisgép-ügynök hello egy folyamatos integrációt a használatakor.</span><span class="sxs-lookup"><span data-stu-id="7a605-118">Make sure you install hello components in hello Jenkins master or hello VM agent if you use one for continuous integration.</span></span> 

<span data-ttu-id="7a605-119">tooinstall, jelentkezzen be toohello Jenkins példány SSH használatával, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="7a605-119">tooinstall, log in toohello Jenkins instance using SSH and run hello following commands:</span></span>

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

<span data-ttu-id="7a605-120">TooApp Linux szolgáltatás telepítéséhez, akkor is kell a Docker tooinstall hello Jenkins master vagy hello build használt Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="7a605-120">For deploying tooApp Service on Linux, you also need tooinstall Docker on hello Jenkins master or hello VM agent used for build.</span></span> <span data-ttu-id="7a605-121">Tekintse meg a toothis cikk tooinstall Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span><span class="sxs-lookup"><span data-stu-id="7a605-121">Refer toothis article tooinstall Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span></span>

## <a name="add-azure-service-principal-toojenkins-credential"></a><span data-ttu-id="7a605-122">Az Azure szolgáltatás egyszerű tooJenkins hitelesítő adatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a605-122">Add Azure service principal tooJenkins credential</span></span>

<span data-ttu-id="7a605-123">Egy Azure szolgáltatás egyszerű szükséges toodeploy tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7a605-123">An Azure service principal is needed toodeploy tooAzure.</span></span> 

<ol>
<li><span data-ttu-id="7a605-124">Használjon [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) vagy [Azure-portálon](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate egy egyszerű Azure szolgáltatást</span><span class="sxs-lookup"><span data-stu-id="7a605-124">Use [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) or [Azure portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate an Azure service principal</span></span></li>
<li><span data-ttu-id="7a605-125">Belül hello Jenkins irányítópultján kattintson **hitelesítő adatok -> rendszer ->**.</span><span class="sxs-lookup"><span data-stu-id="7a605-125">Within hello Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="7a605-126">Kattintson a **globális credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="7a605-126">Click **Global credentials(unrestricted)**.</span></span></li>
<li><span data-ttu-id="7a605-127">Kattintson a **adja hozzá a hitelesítő adatok** tooadd egy Microsoft Azure szolgáltatás egyszerű előfizetés-azonosító, az ügyfél-azonosító, a titkos Ügyfélkulcs és a OAuth 2.0 Token-végpont hello kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="7a605-127">Click **Add Credentials** tooadd a Microsoft Azure service principal by filling out hello Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="7a605-128">Adja meg az Azonosítót **mySp**, a későbbi lépésekben használatra.</span><span class="sxs-lookup"><span data-stu-id="7a605-128">Provide an ID, **mySp**, for use in subsequent steps.</span></span></li>
</ol>

## <a name="azure-app-service-plugin"></a><span data-ttu-id="7a605-129">Az Azure App Service beépülő modul</span><span class="sxs-lookup"><span data-stu-id="7a605-129">Azure App Service plugin</span></span>

<span data-ttu-id="7a605-130">Az Azure App Service beépülő modul 1.0-s verziója támogatja a folyamatos üzembe helyezés tooAzure Web App használatával:</span><span class="sxs-lookup"><span data-stu-id="7a605-130">Azure App Service plugin v1.0 supports continuous deployment tooAzure Web App through:</span></span>

* <span data-ttu-id="7a605-131">Git és az FTP</span><span class="sxs-lookup"><span data-stu-id="7a605-131">Git and FTP</span></span>
* <span data-ttu-id="7a605-132">Webalkalmazás Linux docker</span><span class="sxs-lookup"><span data-stu-id="7a605-132">Docker for Web App on Linux</span></span>

## <a name="configure-jenkins-toodeploy-web-app-through-ftp-using-hello-jenkins-dashboard"></a><span data-ttu-id="7a605-133">Jenkins toodeploy keresztül hello Jenkins irányítópulttal FTP webalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a605-133">Configure Jenkins toodeploy Web App through FTP using hello Jenkins dashboard</span></span>

<span data-ttu-id="7a605-134">toodeploy a projekt tooAzure Web App, feltöltheti a build összetevők (például .war fájl Java) Git vagy FTP segítségével.</span><span class="sxs-lookup"><span data-stu-id="7a605-134">toodeploy your project tooAzure Web App, you can upload your build artifacts (for example, .war file in Java) using Git or FTP.</span></span>

<span data-ttu-id="7a605-135">Mielőtt beállítaná a Jenkins hello feladat, kell az Azure App Service-csomag és a webes alkalmazás futó hello Java-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7a605-135">Before setting up hello job in Jenkins, you need an Azure App Service plan and a Web App for running hello Java app.</span></span>


1. <span data-ttu-id="7a605-136">Hozzon létre az Azure App Service-csomag hello **szabad** hello segítségével tarifacsomag [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="7a605-136">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="7a605-137">hello App Service-csomagot határoz meg hello használt fizikai erőforrások toohost az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="7a605-137">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="7a605-138">App Service-csomagot tooan hozzárendelt összes alkalmazás ossza meg ezeket az erőforrásokat, így toosave költség több alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="7a605-138">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span>
2. <span data-ttu-id="7a605-139">Webes alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7a605-139">Create a Web App.</span></span> <span data-ttu-id="7a605-140">Vagy használjon hello is [Azure-portálon](/azure/app-service-web/web-sites-configure) , vagy használjon hello Az parancssori parancsot:</span><span class="sxs-lookup"><span data-stu-id="7a605-140">You can either use hello [Azure portal](/azure/app-service-web/web-sites-configure) or use hello following Az CLI command:</span></span>
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. <span data-ttu-id="7a605-141">Ellenőrizze, hogy a kell hello Java runtime konfigurációs beállítása.</span><span class="sxs-lookup"><span data-stu-id="7a605-141">Make sure you set up hello Java runtime configuration that your app needs.</span></span> <span data-ttu-id="7a605-142">a következő parancs az Azure parancssori felület hello hello web app toorun konfigurálja a legújabb Java 8 JDK és [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="7a605-142">hello following Azure CLI command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-hello-jenkins-job"></a><span data-ttu-id="7a605-143">Hello Jenkins feladat beállítása</span><span class="sxs-lookup"><span data-stu-id="7a605-143">Set up hello Jenkins job</span></span>


1. <span data-ttu-id="7a605-144">Hozzon létre egy új **freestyle** projekt Jenkins irányítópult</span><span class="sxs-lookup"><span data-stu-id="7a605-144">Create a new **freestyle** project in Jenkins Dashboard</span></span>
2. <span data-ttu-id="7a605-145">Konfigurálása **forrás kód felügyeleti** toouse az a helyi elágazáshoz [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) hello megadásával **tárház URL-CÍMÉT**.</span><span class="sxs-lookup"><span data-stu-id="7a605-145">Configure **Source Code Management** toouse your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing hello **Repository URL**.</span></span> <span data-ttu-id="7a605-146">Például: http://github.com/&lt;yourID > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="7a605-146">For example: http://github.com/&lt;yourID>/javawebappsample.</span></span>
3. <span data-ttu-id="7a605-147">Adjon hozzá egy összeállítása lépés toobuild hello projektet Maven használatával.</span><span class="sxs-lookup"><span data-stu-id="7a605-147">Add a Build step toobuild hello project using Maven.</span></span> <span data-ttu-id="7a605-148">Ehhez adja hozzá egy **hajtható végre a rendszerhéj**.</span><span class="sxs-lookup"><span data-stu-id="7a605-148">Do so by adding an **Execute shell**.</span></span> <span data-ttu-id="7a605-149">Ebben a példában egy további lépést toorename hello *.war fájl tároló mappa tooROOT.war kell.</span><span class="sxs-lookup"><span data-stu-id="7a605-149">For this example, we need an additional step toorename hello *.war file in target folder tooROOT.war.</span></span>   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. <span data-ttu-id="7a605-150">Adja hozzá a létrehozás után végrehajtandó művelet kiválasztásával **közzététele az Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="7a605-150">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
5. <span data-ttu-id="7a605-151">Adjon meg, "mySp" hello Azure szolgáltatás egyszerű tárolja az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="7a605-151">Supply, "mySp", hello Azure service principal stored in previous step.</span></span>
6. <span data-ttu-id="7a605-152">A **Alkalmazáskonfiguráció** területen válassza ki a hello erőforrás csoport és a webes alkalmazást az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="7a605-152">In **App Configuration** section, choose hello resource group and web app in your subscription.</span></span> <span data-ttu-id="7a605-153">hello beépülő modul automatikusan észleli, hogy-e a Windows hello webalkalmazás vagy Linux-alapú.</span><span class="sxs-lookup"><span data-stu-id="7a605-153">hello plugin automatically detects whether hello Web App is Windows or Linux-based.</span></span> <span data-ttu-id="7a605-154">Egy Windows-alapú webalkalmazás "Közzététele fájlok" hello lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="7a605-154">For a Windows-based Web App, hello option "Publish Files" is presented.</span></span>
7. <span data-ttu-id="7a605-155">Töltse ki a hello fájlok kívánt toodeploy (például egy war-csomag használata Java.) Forrás és cél könyvtárat opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="7a605-155">Fill in hello files you want toodeploy (for example, a war package if you're using Java.) Source Directory and Target Directory are optional.</span></span> <span data-ttu-id="7a605-156">hello paraméterek lehetővé teszik toospecify forrása és célja mappák fájlok feltöltésekor.</span><span class="sxs-lookup"><span data-stu-id="7a605-156">hello parameters allow you toospecify source and target folders when uploading files.</span></span> <span data-ttu-id="7a605-157">Java-webalkalmazás az Azure-on fut egy Tomcat kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="7a605-157">Java web app on Azure is run in a Tomcat server.</span></span> <span data-ttu-id="7a605-158">Így feltöltött, háború csomagot a webapps mappát.</span><span class="sxs-lookup"><span data-stu-id="7a605-158">So you upload you war package to webapps folder.</span></span> <span data-ttu-id="7a605-159">Ebben a példában beállítása **forráskönyvtár** túl "cél" és "webalkalmazás" megadnia **célkönyvtár**.</span><span class="sxs-lookup"><span data-stu-id="7a605-159">For this example, set **Source Directory** too"target" and supply "webapps" for **Target Directory**.</span></span>
8. <span data-ttu-id="7a605-160">Ha azt szeretné, hogy toodeploy tooa tárolóhely éles eltérő, akkor is megadhatja **tárolóhely** nevét.</span><span class="sxs-lookup"><span data-stu-id="7a605-160">If you want toodeploy tooa slot other than production, you can also set **Slot** Name.</span></span>
9. <span data-ttu-id="7a605-161">Mentse hello projektet, és állítsa be úgy.</span><span class="sxs-lookup"><span data-stu-id="7a605-161">Save hello project and build it.</span></span> <span data-ttu-id="7a605-162">A webalkalmazás üzembe helyezett tooAzure esetén build befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7a605-162">Your web app is deployed tooAzure when build is complete.</span></span>

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a><span data-ttu-id="7a605-163">Webalkalmazás keresztül Jenkins kimenetátirányításának segítségével FTP telepítése</span><span class="sxs-lookup"><span data-stu-id="7a605-163">Deploy Web App through FTP using Jenkins pipeline</span></span>

<span data-ttu-id="7a605-164">hello beépülő modul az adatcsatorna használatra kész.</span><span class="sxs-lookup"><span data-stu-id="7a605-164">hello plugin is pipeline-ready.</span></span> <span data-ttu-id="7a605-165">Olvassa el a GitHub-tárház hello tooa mintát.</span><span class="sxs-lookup"><span data-stu-id="7a605-165">You can refer tooa sample in hello GitHub repo.</span></span>

1. <span data-ttu-id="7a605-166">Nyissa meg a GitHub webes felhasználói felület, **Jenkinsfile_ftp_plugin** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7a605-166">In GitHub web UI, open **Jenkinsfile_ftp_plugin** file.</span></span> <span data-ttu-id="7a605-167">Kattintson a gombra hello ceruza ikonra tooedit a fájl tooupdate hello erőforráscsoport és 11 és 12 sor a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="7a605-167">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="7a605-168">Sor 14 tooupdate hitelesítő adat azonosító Jenkins betűtípusainak módosítható.</span><span class="sxs-lookup"><span data-stu-id="7a605-168">Change line 14 tooupdate credential ID in your Jenkins instance.</span></span>    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a><span data-ttu-id="7a605-169">Hozzon létre egy Jenkins folyamat</span><span class="sxs-lookup"><span data-stu-id="7a605-169">Create a Jenkins pipeline</span></span>

1. <span data-ttu-id="7a605-170">Nyissa meg webböngészővel Jenkins, kattintson **új elem**.</span><span class="sxs-lookup"><span data-stu-id="7a605-170">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="7a605-171">Adja meg a hello feladatnak a nevét, majd válasszon **csővezeték**.</span><span class="sxs-lookup"><span data-stu-id="7a605-171">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="7a605-172">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a605-172">Click **OK**.</span></span>
3. <span data-ttu-id="7a605-173">Kattintson a hello **csővezeték** következő lap.</span><span class="sxs-lookup"><span data-stu-id="7a605-173">Click hello **Pipeline** tab next.</span></span>
4. <span data-ttu-id="7a605-174">A **Definition**, jelölje be **parancsfájl az SCM-feldolgozási folyamat**.</span><span class="sxs-lookup"><span data-stu-id="7a605-174">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="7a605-175">A **SCM**, jelölje be **Git**.</span><span class="sxs-lookup"><span data-stu-id="7a605-175">For **SCM**, select **Git**.</span></span> <span data-ttu-id="7a605-176">Adja meg a villás tárház a Githubon URL-cím hello: https:&lt;a villás tárház > .git</span><span class="sxs-lookup"><span data-stu-id="7a605-176">Enter hello GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span>
6. <span data-ttu-id="7a605-177">Frissítés **parancsfájl elérési útján** túl "Jenkinsfile_ftp_plugin"</span><span class="sxs-lookup"><span data-stu-id="7a605-177">Update **Script Path** too"Jenkinsfile_ftp_plugin"</span></span>
7. <span data-ttu-id="7a605-178">Kattintson a **mentése** és futtatási hello feladat.</span><span class="sxs-lookup"><span data-stu-id="7a605-178">Click **Save** and run hello job.</span></span>

## <a name="configure-jenkins-toodeploy-web-app-on-linux-through-docker"></a><span data-ttu-id="7a605-179">Linux keresztül Docker Jenkins toodeploy webalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a605-179">Configure Jenkins toodeploy Web App on Linux through Docker</span></span>

<span data-ttu-id="7a605-180">Git/FTP, leszámítva a webalkalmazás Linux állításra Docker támogatja.</span><span class="sxs-lookup"><span data-stu-id="7a605-180">Apart from Git/FTP, Web App on Linux supports deployment using Docker.</span></span> <span data-ttu-id="7a605-181">használja a Docker, toodeploy tooprovide egy Dockerfile, amely csomagokat a webalkalmazás szolgáltatás futtatási idő mellett a docker-lemezkép van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7a605-181">toodeploy using Docker, you need tooprovide a Dockerfile that packages your web app with service runtime into a docker image.</span></span> <span data-ttu-id="7a605-182">Hello beépülő modul majd hello képet alkot, leküldéses értesítések tooa docker beállításkulcs, és hello kép tooyour webes alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="7a605-182">Then hello plugin builds hello image, pushes it tooa docker registry and deploys hello image tooyour web app.</span></span>

<span data-ttu-id="7a605-183">Webes alkalmazás Linux támogatja a hagyományos módon, mint például a Git és az FTP, de csak beépített nyelveket (.NET Core, Node.js, PHP és Ruby) is.</span><span class="sxs-lookup"><span data-stu-id="7a605-183">Web App on Linux also supports traditional ways like Git and FTP, but only for built-in languages (.NET Core, Node.js, PHP, and Ruby).</span></span> <span data-ttu-id="7a605-184">A többi nyelvet kell toopackage az alkalmazás kódja és szolgáltatás futásidejű együtt egy docker-lemezképpel, és docker toodeploy használja.</span><span class="sxs-lookup"><span data-stu-id="7a605-184">For other languages, you need toopackage your application code and service runtime together into a docker image and use docker toodeploy.</span></span>

<span data-ttu-id="7a605-185">Mielőtt beállítaná a Jenkins hello feladat, kell Linux az Azure app service.</span><span class="sxs-lookup"><span data-stu-id="7a605-185">Before setting up hello job in Jenkins, you need an Azure app service on Linux.</span></span> <span data-ttu-id="7a605-186">Egy tároló beállításjegyzék is szükséges toostore, és a titkos Docker-tároló lemezképek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="7a605-186">A container registry is also needed toostore and manage your private Docker container images.</span></span> <span data-ttu-id="7a605-187">Használhatja a DockerHub; Azure-tároló beállításjegyzék ehhez a példához használjuk.</span><span class="sxs-lookup"><span data-stu-id="7a605-187">You can use DockerHub; we are using Azure Container Registry for this example.</span></span>

* <span data-ttu-id="7a605-188">A lépésekkel hello [Itt](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate Linux webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7a605-188">You can follow hello steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate a Web App on Linux</span></span> 
* <span data-ttu-id="7a605-189">Az Azure tároló beállításjegyzék egy felügyelt [Docker beállításjegyzék] alapján (https://docs.docker.com/registry/) szolgáltatás hello nyílt forráskódú Docker beállításjegyzék 2.0.</span><span class="sxs-lookup"><span data-stu-id="7a605-189">Azure Container Registry is a managed [Docker registry] (https://docs.docker.com/registry/) service based on hello open-source Docker Registry 2.0.</span></span> <span data-ttu-id="7a605-190">Lépések hello [itt] (/ azure/container-registry/container-registry-get-started-azure-cli) kapcsolatos további útmutatás toodo így.</span><span class="sxs-lookup"><span data-stu-id="7a605-190">Follow hello steps [here] (/azure/container-registry/container-registry-get-started-azure-cli) for more guidance on how toodo so.</span></span> <span data-ttu-id="7a605-191">DockerHub is használható.</span><span class="sxs-lookup"><span data-stu-id="7a605-191">You can also use DockerHub.</span></span>

### <a name="toodeploy-using-docker"></a><span data-ttu-id="7a605-192">toodeploy docker használatával:</span><span class="sxs-lookup"><span data-stu-id="7a605-192">toodeploy using docker:</span></span>

1. <span data-ttu-id="7a605-193">Hozzon létre új freestyle projekt Jenkins irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="7a605-193">Create a new freestyle project in Jenkins Dashboard.</span></span>
2. <span data-ttu-id="7a605-194">Konfigurálása **forrás kód felügyeleti** toouse az a helyi elágazáshoz [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) hello megadásával **tárház URL-CÍMÉT**.</span><span class="sxs-lookup"><span data-stu-id="7a605-194">Configure **Source Code Management** toouse your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing hello **Repository URL**.</span></span> <span data-ttu-id="7a605-195">Például: http://github.com/&lt;yourid > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="7a605-195">For example: http://github.com/&lt;yourid>/javawebappsample.</span></span>
<span data-ttu-id="7a605-196">Adjon hozzá egy összeállítása lépés toobuild hello projektet Maven használatával.</span><span class="sxs-lookup"><span data-stu-id="7a605-196">Add a Build step toobuild hello project using Maven.</span></span> <span data-ttu-id="7a605-197">Ehhez adja hozzá egy **hajtható végre a rendszerhéj** , és adja hozzá a következő sort a hello **parancs**:</span><span class="sxs-lookup"><span data-stu-id="7a605-197">Do so by adding an **Execute shell** and add hello following line in **Command**:</span></span>    
```bash
mvn clean package
```

3. <span data-ttu-id="7a605-198">Adja hozzá a létrehozás után végrehajtandó művelet kiválasztásával **közzététele az Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="7a605-198">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
4. <span data-ttu-id="7a605-199">Adja meg, **mySp**, hello Azure szolgáltatás egyszerű Azure hitelesítő adatként az előző lépésben tárolja.</span><span class="sxs-lookup"><span data-stu-id="7a605-199">Supply, **mySp**, hello Azure service principal stored in previous step as Azure Credentials.</span></span>
5. <span data-ttu-id="7a605-200">A **Alkalmazáskonfiguráció** területen válassza ki a hello az erőforráscsoportot és egy Linux-webalkalmazást az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="7a605-200">In **App Configuration** section, choose hello resource group and a Linux web app in your subscription.</span></span>
6. <span data-ttu-id="7a605-201">Válassza a Docker keresztül közzétenni.</span><span class="sxs-lookup"><span data-stu-id="7a605-201">Choose Publish via Docker.</span></span>
7. <span data-ttu-id="7a605-202">Töltse ki **Dockerfile** elérési útja.</span><span class="sxs-lookup"><span data-stu-id="7a605-202">Fill in **Dockerfile** path.</span></span> <span data-ttu-id="7a605-203">Hello alapértelmezett megtarthatja "/ Dockerfile" a **Docker beállításjegyzék URL-cím**, szállítási https:// hello formátumban&lt;myRegistry >. Ha Azure-tároló rendszerleíró azurecr.io.</span><span class="sxs-lookup"><span data-stu-id="7a605-203">You can keep hello default "/Dockerfile" For **Docker registry URL**, supply in hello format of https://&lt;myRegistry>.azurecr.io if you use Azure Container Registry.</span></span> <span data-ttu-id="7a605-204">Hagyja üresen a mezőt DockerHub használatakor.</span><span class="sxs-lookup"><span data-stu-id="7a605-204">Leave it blank if you use DockerHub.</span></span>
8. <span data-ttu-id="7a605-205">A **beállításjegyzék hitelesítő adatok**, vegye fel a hello Azure tároló beállításjegyzék hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="7a605-205">For **Registry credentials**, add hello credential for hello Azure Container Registry.</span></span> <span data-ttu-id="7a605-206">Hello felhasználónév és jelszó lekéréséhez, futtassa a következő Azure parancssori felület parancsai hello.</span><span class="sxs-lookup"><span data-stu-id="7a605-206">You can get hello userid and password by running hello following commands in Azure CLI.</span></span> <span data-ttu-id="7a605-207">hello első parancs lehetővé teszi, hogy a hello rendszergazdai fiókot.</span><span class="sxs-lookup"><span data-stu-id="7a605-207">hello first command enables hello administrator account.</span></span>    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. <span data-ttu-id="7a605-208">docker nevet és -címke hello **speciális** lapon opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="7a605-208">hello docker image name and tag in **Advanced** tab are optional.</span></span> <span data-ttu-id="7a605-209">Alapértelmezés szerint a lemezkép neve egységekre hello lemezképből konfigurálta az Azure portál (a Docker-tároló beállítás.) hello címke neve $BUILD_NUMBER jön létre.</span><span class="sxs-lookup"><span data-stu-id="7a605-209">By default, image name is obtained from hello image name you configured in Azure portal (in Docker Container setting.) hello tag is generated from $BUILD_NUMBER.</span></span> <span data-ttu-id="7a605-210">Győződjön meg arról, adja meg a hello kép neve vagy az Azure portálon, vagy adjon meg egy értéket a **Docker kép** a **speciális** fülre. A jelen példában adja meg a "&lt;yourRegistry >.azurecr.io/calculator" a **Docker kép** és hagyja **Docker Képcímke** üres.</span><span class="sxs-lookup"><span data-stu-id="7a605-210">Make sure you specify hello image name in either Azure portal or supply a value for **Docker Image** in **Advanced** tab. For this example, supply "&lt;yourRegistry>.azurecr.io/calculator" for **Docker image** and leave **Docker Image Tag** blank.</span></span>
10. <span data-ttu-id="7a605-211">Megjegyzés: a telepítés sikertelen lesz, ha a beépített Docker lemezkép beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="7a605-211">Note deployment fails if you use built-in Docker image setting.</span></span> <span data-ttu-id="7a605-212">Ellenőrizze, hogy a docker config toouse egyéni lemezképként az Azure-portálon a Docker-tároló beállítást módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="7a605-212">Make sure you change docker config toouse custom image in Docker Container setting in Azure portal.</span></span> <span data-ttu-id="7a605-213">Beépített lemezkép használata a fájl feltöltése megközelítés toodeploy.</span><span class="sxs-lookup"><span data-stu-id="7a605-213">For built-in image, use file upload approach toodeploy.</span></span>
11. <span data-ttu-id="7a605-214">Hasonló fájlfeltöltés módszert használja toofile, dönthet úgy, nem éles különböző tárhely.</span><span class="sxs-lookup"><span data-stu-id="7a605-214">Similar toofile upload approach, you can choose a different slot other than production.</span></span>
12. <span data-ttu-id="7a605-215">Mentse és hello projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7a605-215">Save and build hello project.</span></span> <span data-ttu-id="7a605-216">Megjelenik a tároló lemezkép fejlesztőre tooyour beállításjegyzék és a webes alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="7a605-216">You see your container image is pushed tooyour registry and web app is deployed.</span></span>

### <a name="deploy-tooweb-app-on-linux-through-docker-using-jenkins-pipeline"></a><span data-ttu-id="7a605-217">TooWeb Jenkins kimenetátirányításának segítségével Docker keresztül Linux alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="7a605-217">Deploy tooWeb App on Linux through Docker using Jenkins pipeline</span></span>

1. <span data-ttu-id="7a605-218">Nyissa meg a GitHub webes felhasználói felület, **Jenkinsfile_container_plugin** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7a605-218">In GitHub web UI, open **Jenkinsfile_container_plugin** file.</span></span> <span data-ttu-id="7a605-219">Kattintson a gombra hello ceruza ikonra tooedit a fájl tooupdate hello erőforráscsoport és 11 és 12 sor a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="7a605-219">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="7a605-220">Sor 13 tooyour tároló beállításjegyzék kiszolgáló módosítása</span><span class="sxs-lookup"><span data-stu-id="7a605-220">Change line 13 tooyour container registry server</span></span>    
```java
def registryServer = '<registryURL>'
```    

3. <span data-ttu-id="7a605-221">Módosítsa a Jenkins példányát sor 16 tooupdate Hitelesítőadat-azonosító</span><span class="sxs-lookup"><span data-stu-id="7a605-221">Change line 16 tooupdate credential ID in your Jenkins instance</span></span>    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a><span data-ttu-id="7a605-222">Jenkins folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a605-222">Create Jenkins pipeline</span></span>    

1. <span data-ttu-id="7a605-223">Nyissa meg webböngészővel Jenkins, kattintson **új elem**.</span><span class="sxs-lookup"><span data-stu-id="7a605-223">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="7a605-224">Adja meg a hello feladatnak a nevét, majd válasszon **csővezeték**.</span><span class="sxs-lookup"><span data-stu-id="7a605-224">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="7a605-225">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a605-225">Click **OK**.</span></span>
3. <span data-ttu-id="7a605-226">Kattintson a hello **csővezeték** következő lap.</span><span class="sxs-lookup"><span data-stu-id="7a605-226">Click hello **Pipeline** tab next.</span></span>
4. <span data-ttu-id="7a605-227">A **Definition**, jelölje be **parancsfájl az SCM-feldolgozási folyamat**.</span><span class="sxs-lookup"><span data-stu-id="7a605-227">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="7a605-228">A **SCM**, jelölje be **Git**.</span><span class="sxs-lookup"><span data-stu-id="7a605-228">For **SCM**, select **Git**.</span></span>
6. <span data-ttu-id="7a605-229">Adja meg a villás tárház a Githubon URL-cím hello: https:&lt;a villás tárház > .git</span><span class="sxs-lookup"><span data-stu-id="7a605-229">Enter hello GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span></li>
<span data-ttu-id="7a605-230">7, frissítése **parancsfájl elérési útján** túl "Jenkinsfile_container_plugin"</span><span class="sxs-lookup"><span data-stu-id="7a605-230">7, Update **Script Path** too"Jenkinsfile_container_plugin"</span></span>
8. <span data-ttu-id="7a605-231">Kattintson a **mentése** és futtatási hello feladat.</span><span class="sxs-lookup"><span data-stu-id="7a605-231">Click **Save** and run hello job.</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="7a605-232">A webalkalmazás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7a605-232">Verify your web app</span></span>

1. <span data-ttu-id="7a605-233">tooverify hello WAR-fájl sikeresen telepítve lett tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7a605-233">tooverify hello WAR file is deployed successfully tooyour web app.</span></span> <span data-ttu-id="7a605-234">Nyisson meg egy webböngészőt.</span><span class="sxs-lookup"><span data-stu-id="7a605-234">Open a web browser.</span></span>
2. <span data-ttu-id="7a605-235">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping látja:</span><span class="sxs-lookup"><span data-stu-id="7a605-235">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping You see:</span></span>    
     <span data-ttu-id="7a605-236">Üdvözli a webes alkalmazás tooJava!!!</span><span class="sxs-lookup"><span data-stu-id="7a605-236">Welcome tooJava Web App!!!</span></span> <span data-ttu-id="7a605-237">A rendszer frissíti!</span><span class="sxs-lookup"><span data-stu-id="7a605-237">This is updated!</span></span>
   <span data-ttu-id="7a605-238">Sun jún 17 16:39:10 UTC 2017</span><span class="sxs-lookup"><span data-stu-id="7a605-238">Sun Jun 17 16:39:10 UTC 2017</span></span>
3. <span data-ttu-id="7a605-239">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) tooget hello számának összege, azaz x és y</span><span class="sxs-lookup"><span data-stu-id="7a605-239">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>        
    ![A Számológép: hozzáadása](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a><span data-ttu-id="7a605-241">Az App service Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="7a605-241">For App service on Linux</span></span>

* <span data-ttu-id="7a605-242">tooverify, az Azure parancssori felület futtassa:</span><span class="sxs-lookup"><span data-stu-id="7a605-242">tooverify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="7a605-243">A következő eredmény hello elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="7a605-243">You get hello following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="7a605-244">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="7a605-244">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="7a605-245">Hello üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="7a605-245">You see hello message:</span></span> 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="7a605-246">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) tooget hello számának összege, azaz x és y</span><span class="sxs-lookup"><span data-stu-id="7a605-246">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="7a605-247">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a605-247">Next steps</span></span>

<span data-ttu-id="7a605-248">Ebben az oktatóanyagban hello Azure App Service beépülő modul toodeploy tooAzure használja.</span><span class="sxs-lookup"><span data-stu-id="7a605-248">In this tutorial, you use hello Azure App Service plugin toodeploy tooAzure.</span></span>

<span data-ttu-id="7a605-249">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="7a605-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a605-250">Azure App Service segítségével FTP Jenkins toodeploy konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a605-250">Configure Jenkins toodeploy Azure App Service through FTP</span></span> 
> * <span data-ttu-id="7a605-251">Linux-rendszeren keresztül Docker Jenkins toodeploy tooAzure App Service konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a605-251">Configure Jenkins toodeploy tooAzure App Service on Linux through Docker</span></span> 
