---
title: "aaaExecute hello Jenkins az Azure parancssori Felülettel |} Microsoft Docs"
description: Ismerje meg, hogyan toouse Azure CLI toodeploy Java web app tooAzure Jenkins folyamat
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 4bd1e12e6de1f010453ff51c835f84e7361962f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-and-hello-azure-cli"></a><span data-ttu-id="41548-103">TooAzure Jenkins az App Service telepítése és hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="41548-103">Deploy tooAzure App Service with Jenkins and hello Azure CLI</span></span>
<span data-ttu-id="41548-104">a Java web app tooAzure toodeploy, használhatja az Azure CLI [Jenkins csővezeték](https://jenkins.io/doc/book/pipeline/).</span><span class="sxs-lookup"><span data-stu-id="41548-104">toodeploy a Java web app tooAzure, you can use Azure CLI in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span></span> <span data-ttu-id="41548-105">Ebben az oktatóanyagban létrehoz egy CI/CD folyamat egy Azure virtuális gépen történő is beleértve:</span><span class="sxs-lookup"><span data-stu-id="41548-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41548-106">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="41548-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="41548-107">Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="41548-107">Configure Jenkins</span></span>
> * <span data-ttu-id="41548-108">Webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="41548-108">Create a web app in Azure</span></span>
> * <span data-ttu-id="41548-109">A GitHub-tárházban előkészítése</span><span class="sxs-lookup"><span data-stu-id="41548-109">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="41548-110">Jenkins folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="41548-110">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="41548-111">Hello folyamat fut, és ellenőrizze a hello webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="41548-111">Run hello pipeline and verify hello web app</span></span>

<span data-ttu-id="41548-112">Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="41548-112">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="41548-113">toofind hello verzió, futtassa `az --version`.</span><span class="sxs-lookup"><span data-stu-id="41548-113">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="41548-114">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="41548-114">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="41548-115">Hozzon létre és Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="41548-115">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="41548-116">Ha még nem rendelkezik egy Jenkins master, kezdje hello [Megoldássablonban](install-jenkins-solution-template.md), mely tartalmazza a szükséges hello [Azure hitelesítő adatok](https://plugins.jenkins.io/azure-credentials) beépülő modul alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="41548-116">If you do not already have a Jenkins master, start with hello [Solution Template](install-jenkins-solution-template.md), which includes hello required [Azure Credentials](https://plugins.jenkins.io/azure-credentials) plugin by default.</span></span> 

<span data-ttu-id="41548-117">hello Azure hitelesítő adat beépülő modul lehetővé teszi toostore Microsoft Azure szolgáltatás egyszerű hitelesítő adatait a Jenkins.</span><span class="sxs-lookup"><span data-stu-id="41548-117">hello Azure Credential plugin allows you toostore Microsoft Azure service principal credentials in Jenkins.</span></span> <span data-ttu-id="41548-118">1.2-es verziója, az azt támogatása hello úgy, hogy Jenkins csővezeték beszerezheti hello Azure hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="41548-118">In version 1.2, we added hello support so that Jenkins Pipeline can get hello Azure credentials.</span></span> 

<span data-ttu-id="41548-119">Győződjön meg arról, hogy 1.2-es vagy újabb verziót:</span><span class="sxs-lookup"><span data-stu-id="41548-119">Ensure you have version 1.2 or later:</span></span>
* <span data-ttu-id="41548-120">Belül hello Jenkins irányítópultján kattintson **Jenkins kezelése beépülő modul Manager -> ->** keresse meg a **Azure hitelesítő**.</span><span class="sxs-lookup"><span data-stu-id="41548-120">Within hello Jenkins dashboard, click **Manage Jenkins -> Plugin Manager ->** and search for **Azure Credential**.</span></span> 
* <span data-ttu-id="41548-121">Frissítse a hello beépülő modult, ha hello verziója korábbi, mint 1.2-es.</span><span class="sxs-lookup"><span data-stu-id="41548-121">Update hello plugin if hello version is earlier than 1.2.</span></span>

<span data-ttu-id="41548-122">Hello Jenkins fő Java JDK és Maven is szükséges.</span><span class="sxs-lookup"><span data-stu-id="41548-122">Java JDK and Maven are also required in hello Jenkins master.</span></span> <span data-ttu-id="41548-123">tooinstall, jelentkezzen be tooJenkins fő SSH használatával, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="41548-123">tooinstall, log in tooJenkins master using SSH and run hello following commands:</span></span>
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-toojenkins-credential"></a><span data-ttu-id="41548-124">Az Azure szolgáltatás egyszerű tooJenkins hitelesítő adatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="41548-124">Add Azure service principal tooJenkins credential</span></span>

<span data-ttu-id="41548-125">Meg kell szükséges tooexecute Azure parancssori felület egy Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="41548-125">An Azure credential is needed tooexecute Azure CLI.</span></span>

* <span data-ttu-id="41548-126">Belül hello Jenkins irányítópultján kattintson **hitelesítő adatok -> rendszer ->**.</span><span class="sxs-lookup"><span data-stu-id="41548-126">Within hello Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="41548-127">Kattintson a **globális credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="41548-127">Click **Global credentials(unrestricted)**.</span></span>
* <span data-ttu-id="41548-128">Kattintson a **adja hozzá a hitelesítő adatok** tooadd egy [Microsoft Azure szolgáltatás egyszerű](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) előfizetés-azonosító, az ügyfél-azonosító, a titkos Ügyfélkulcs és a OAuth 2.0 Token-végpont hello kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="41548-128">Click **Add Credentials** tooadd a [Microsoft Azure service principal](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) by filling out hello Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="41548-129">Adja meg a Azonosítót az ezt követő lépés felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="41548-129">Provide an ID for use in subsequent step.</span></span>

![Adja hozzá a hitelesítő adatok](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-hello-java-web-app"></a><span data-ttu-id="41548-131">Az Azure App Service üzembe helyezéséhez hello Java-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41548-131">Create an Azure App Service for deploying hello Java web app</span></span>

<span data-ttu-id="41548-132">Hozzon létre az Azure App Service-csomag hello **szabad** hello segítségével tarifacsomag [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="41548-132">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="41548-133">hello App Service-csomagot határoz meg hello használt fizikai erőforrások toohost az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="41548-133">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="41548-134">App Service-csomagot tooan hozzárendelt összes alkalmazás ossza meg ezeket az erőforrásokat, így toosave költség több alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="41548-134">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="41548-135">Amikor készen áll a hello terv, hello Azure CLI hasonló látható kimeneti toohello a következő példa:</span><span class="sxs-lookup"><span data-stu-id="41548-135">When hello plan is ready, hello Azure CLI shows similar output toohello following example:</span></span>

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a><span data-ttu-id="41548-136">Azure-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41548-136">Create an Azure Web app</span></span>

 <span data-ttu-id="41548-137">Használjon hello [az webalkalmazás létrehozása](/cli/azure/appservice/web#create) CLI parancs toocreate egy web app-definíciót a hello `myAppServicePlan` App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="41548-137">Use hello [az webapp create](/cli/azure/appservice/web#create) CLI command toocreate a web app definition in hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="41548-138">hello web app-definíciót tartalmaz egy URL-cím tooaccess az alkalmazásba, és konfigurálja a több beállítások toodeploy a kód tooAzure.</span><span class="sxs-lookup"><span data-stu-id="41548-138">hello web app definition provides a URL tooaccess your application with and configures several options toodeploy your code tooAzure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="41548-139">Helyettesítő hello `<app_name>` helyőrző a saját egyedi alkalmazás nevével.</span><span class="sxs-lookup"><span data-stu-id="41548-139">Substitute hello `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="41548-140">A egyedi név része hello alapértelmezett tartomány nevét hello webalkalmazás, így hello nevének kell toobe egyedi az Azure-ban minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="41548-140">This unique name is part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="41548-141">Ki kell alakítani az tooyour felhasználók hozzárendelhető egy egyéni tartomány nevét bejegyzés toohello webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="41548-141">You can map a custom domain name entry toohello web app before you expose it tooyour users.</span></span>

<span data-ttu-id="41548-142">Amikor készen áll a hello web app-definíciót, hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="41548-142">When hello web app definition is ready, hello Azure CLI shows information similar toohello following example:</span></span> 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a><span data-ttu-id="41548-143">Java konfigurálása</span><span class="sxs-lookup"><span data-stu-id="41548-143">Configure Java</span></span> 

<span data-ttu-id="41548-144">Hello Java runtime konfiguráció, amelyet az alkalmazás a hello [az App Service web config frissítés](/cli/azure/appservice/web/config#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="41548-144">Set up hello Java runtime configuration that your app needs with hello  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="41548-145">hello következő parancsot konfigurálása hello web app toorun a legújabb Java 8 JDK és [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="41548-145">hello following command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a><span data-ttu-id="41548-146">A GitHub-tárházban előkészítése</span><span class="sxs-lookup"><span data-stu-id="41548-146">Prepare a GitHub Repository</span></span>
<span data-ttu-id="41548-147">Nyissa meg hello [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) tárházban.</span><span class="sxs-lookup"><span data-stu-id="41548-147">Open hello [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo.</span></span> <span data-ttu-id="41548-148">toofork hello tárház tooyour saját GitHub-fiók, kattintson a hello **elágazás** hello jobb felső sarkában található gombra.</span><span class="sxs-lookup"><span data-stu-id="41548-148">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>

* <span data-ttu-id="41548-149">Nyissa meg a GitHub webes felhasználói felület, **Jenkinsfile** fájlt.</span><span class="sxs-lookup"><span data-stu-id="41548-149">In GitHub web UI, open **Jenkinsfile** file.</span></span> <span data-ttu-id="41548-150">Kattintson a gombra hello ceruza ikonra tooedit a fájl tooupdate hello erőforráscsoport és a sor 20 és 21 a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="41548-150">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 20 and 21 respectively.</span></span>

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* <span data-ttu-id="41548-151">Módosítsa a Jenkins példányát sor 23 tooupdate Hitelesítőadat-azonosító</span><span class="sxs-lookup"><span data-stu-id="41548-151">Change line 23 tooupdate credential ID in your Jenkins instance</span></span>

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a><span data-ttu-id="41548-152">Jenkins folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="41548-152">Create Jenkins pipeline</span></span>
<span data-ttu-id="41548-153">Nyissa meg webböngészővel Jenkins, kattintson **új elem**.</span><span class="sxs-lookup"><span data-stu-id="41548-153">Open Jenkins in a web browser, click **New Item**.</span></span> 

* <span data-ttu-id="41548-154">Adja meg a hello feladatnak a nevét, majd válasszon **csővezeték**.</span><span class="sxs-lookup"><span data-stu-id="41548-154">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="41548-155">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="41548-155">Click **OK**.</span></span>
* <span data-ttu-id="41548-156">Kattintson a hello **csővezeték** következő lap.</span><span class="sxs-lookup"><span data-stu-id="41548-156">Click hello **Pipeline** tab next.</span></span> 
* <span data-ttu-id="41548-157">A **Definition**, jelölje be **parancsfájl az SCM-feldolgozási folyamat**.</span><span class="sxs-lookup"><span data-stu-id="41548-157">For **Definition**, select **Pipeline script from SCM**.</span></span>
* <span data-ttu-id="41548-158">A **SCM**, jelölje be **Git**.</span><span class="sxs-lookup"><span data-stu-id="41548-158">For **SCM**, select **Git**.</span></span>
* <span data-ttu-id="41548-159">Adja meg a villás tárház a Githubon URL-cím hello: https:\<a villás tárház\>.git</span><span class="sxs-lookup"><span data-stu-id="41548-159">Enter hello GitHub URL for your forked repo: https:\<your forked repo\>.git</span></span>
* <span data-ttu-id="41548-160">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="41548-160">Click **Save**</span></span>

## <a name="test-your-pipeline"></a><span data-ttu-id="41548-161">A folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="41548-161">Test your pipeline</span></span>
* <span data-ttu-id="41548-162">Nyissa meg a létrehozott toohello csővezeték, kattintson a **Build most**</span><span class="sxs-lookup"><span data-stu-id="41548-162">Go toohello pipeline you created, click **Build Now**</span></span>
* <span data-ttu-id="41548-163">Néhány másodpercen belül a build sikeres legyen, és toohello build megnyithatja, és kattintson a **konzol kimeneti** toosee hello részletei</span><span class="sxs-lookup"><span data-stu-id="41548-163">A build should succeed in a few seconds, and you can go toohello build and click **Console Output** toosee hello details</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="41548-164">A webalkalmazás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41548-164">Verify your web app</span></span>
<span data-ttu-id="41548-165">tooverify hello WAR-fájl sikeresen telepítve lett tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="41548-165">tooverify hello WAR file is deployed successfully tooyour web app.</span></span> <span data-ttu-id="41548-166">Nyisson meg egy webböngészőt:</span><span class="sxs-lookup"><span data-stu-id="41548-166">Open a web browser:</span></span>

* <span data-ttu-id="41548-167">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping</span><span class="sxs-lookup"><span data-stu-id="41548-167">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping</span></span>  
<span data-ttu-id="41548-168">Lásd:</span><span class="sxs-lookup"><span data-stu-id="41548-168">You see:</span></span>

        Welcome tooJava Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* <span data-ttu-id="41548-169">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) tooget hello számának összege, azaz x és y</span><span class="sxs-lookup"><span data-stu-id="41548-169">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>

![A Számológép: hozzáadása](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-tooazure-web-app-on-linux"></a><span data-ttu-id="41548-171">TooAzure Linux webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="41548-171">Deploy tooAzure Web App on Linux</span></span>
<span data-ttu-id="41548-172">Most, hogy tudja, hogyan toouse a Jenkins az Azure CLI-feldolgozási, hello parancsfájl toodeploy tooan Azure Web Apps Linux módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="41548-172">Now that you know how toouse Azure CLI in your Jenkins pipeline, you can modify hello script toodeploy tooan Azure Web App on Linux.</span></span>

<span data-ttu-id="41548-173">Webes alkalmazás Linux egy másik módja toodo hello környezetben, amely toouse Docker támogatja.</span><span class="sxs-lookup"><span data-stu-id="41548-173">Web App on Linux supports a different way toodo hello deployment, which is toouse Docker.</span></span> <span data-ttu-id="41548-174">toodeploy, tooprovide egy Dockerfile, amely csomagokat a webalkalmazás szolgáltatás futtatási idő mellett a Docker-lemezkép szükséges.</span><span class="sxs-lookup"><span data-stu-id="41548-174">toodeploy, you need tooprovide a Dockerfile that packages your web app with service runtime into a Docker image.</span></span> <span data-ttu-id="41548-175">hello beépülő modul fog majd hello lemezkép, tooa Docker beállításjegyzék leküldeni és hello kép tooyour webalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="41548-175">hello plugin will then build hello image, push it tooa Docker registry and deploy hello image tooyour web app.</span></span>

* <span data-ttu-id="41548-176">Hello lépésekkel [Itt](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate egy Linux rendszeren futó Azure webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="41548-176">Follow hello steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate an Azure Web App running on Linux.</span></span>
* <span data-ttu-id="41548-177">A Jenkins példányán, ezen hello utasításokat követve telepítse Docker [cikk](https://docs.docker.com/engine/installation/linux/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="41548-177">Install Docker on your Jenkins instance by following hello instructions in this [article](https://docs.docker.com/engine/installation/linux/ubuntu/).</span></span>
* <span data-ttu-id="41548-178">Egy tároló beállításjegyzék hello Azure-portálon a hello lépések használatával hozható létre [Itt](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="41548-178">Create a Container Registry in hello Azure portal by using hello steps [here](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
* <span data-ttu-id="41548-179">Az azonos hello [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) ágazik el, hogy tárház szerkesztése hello **Jenkinsfile2** fájlt:</span><span class="sxs-lookup"><span data-stu-id="41548-179">In hello same [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo you forked, edit hello **Jenkinsfile2** file:</span></span>
    * <span data-ttu-id="41548-180">18-21. sor, illetve frissíteni az erőforráscsoport, a web app és az ACR toohello nevei.</span><span class="sxs-lookup"><span data-stu-id="41548-180">Line 18-21, update toohello names of your resource group, web app, and ACR respectively.</span></span> 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * <span data-ttu-id="41548-181">Sor 24, a frissítés \<azsrvprincipal\> tooyour Hitelesítőadat-azonosító</span><span class="sxs-lookup"><span data-stu-id="41548-181">Line 24, update \<azsrvprincipal\> tooyour credential ID</span></span>
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* <span data-ttu-id="41548-182">Hozzon létre egy új Jenkins folyamat tooAzure webes alkalmazás a Windows rendszerben csak megadott idő használata telepítése hasonló módon **Jenkinsfile2** helyette.</span><span class="sxs-lookup"><span data-stu-id="41548-182">Create a new Jenkins pipeline as you did when deploying tooAzure web app in Windows, only this time, use **Jenkinsfile2** instead.</span></span>
* <span data-ttu-id="41548-183">Az új feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="41548-183">Run your new job.</span></span>
* <span data-ttu-id="41548-184">tooverify, az Azure parancssori felület futtassa:</span><span class="sxs-lookup"><span data-stu-id="41548-184">tooverify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="41548-185">A következő eredmény hello elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="41548-185">You get hello following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="41548-186">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="41548-186">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="41548-187">Hello üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="41548-187">You see hello message:</span></span> 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="41548-188">Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) tooget hello számának összege, azaz x és y</span><span class="sxs-lookup"><span data-stu-id="41548-188">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="41548-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41548-189">Next steps</span></span>
<span data-ttu-id="41548-190">Ebben az oktatóanyagban egy Jenkins folyamatot, amely ellenőrzi a kimenő hello forráskód a GitHub-tárház konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="41548-190">In this tutorial, you configured a Jenkins pipeline that checks out hello source code in GitHub repo.</span></span> <span data-ttu-id="41548-191">Maven toobuild futtatja a war-fájl, és ezután használja az Azure CLI toodeploy tooAzure App Service.</span><span class="sxs-lookup"><span data-stu-id="41548-191">Runs Maven toobuild a war file and then uses Azure CLI toodeploy tooAzure App Service.</span></span> <span data-ttu-id="41548-192">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="41548-192">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41548-193">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="41548-193">Create a Jenkins VM</span></span>
> * <span data-ttu-id="41548-194">Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="41548-194">Configure Jenkins</span></span>
> * <span data-ttu-id="41548-195">Webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="41548-195">Create a web app in Azure</span></span>
> * <span data-ttu-id="41548-196">A GitHub-tárházban előkészítése</span><span class="sxs-lookup"><span data-stu-id="41548-196">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="41548-197">Jenkins folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="41548-197">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="41548-198">Hello folyamat fut, és ellenőrizze a hello webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="41548-198">Run hello pipeline and verify hello web app</span></span>
