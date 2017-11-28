---
title: "Hajtsa végre az Azure parancssori felület a Jenkins |} Microsoft Docs"
description: "Azure CLI használata Java-webalkalmazás telepítése az Azure-bA Jenkins folyamat"
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
ms.openlocfilehash: 5ca8338d4bf343f08fe70081cff755fa76a126a9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a><span data-ttu-id="b9c26-103">Az Azure App Service Jenkins és az Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="b9c26-103">Deploy to Azure App Service with Jenkins and the Azure CLI</span></span>
<span data-ttu-id="b9c26-104">Java-webalkalmazás telepítése az Azure-ba, használja az Azure CLI [Jenkins csővezeték](https://jenkins.io/doc/book/pipeline/).</span><span class="sxs-lookup"><span data-stu-id="b9c26-104">To deploy a Java web app to Azure, you can use Azure CLI in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span></span> <span data-ttu-id="b9c26-105">Ebben az oktatóanyagban létrehoz egy CI/CD folyamat egy Azure virtuális gépen történő is beleértve:</span><span class="sxs-lookup"><span data-stu-id="b9c26-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b9c26-106">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9c26-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="b9c26-107">Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9c26-107">Configure Jenkins</span></span>
> * <span data-ttu-id="b9c26-108">Webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b9c26-108">Create a web app in Azure</span></span>
> * <span data-ttu-id="b9c26-109">A GitHub-tárházban előkészítése</span><span class="sxs-lookup"><span data-stu-id="b9c26-109">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="b9c26-110">Jenkins folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9c26-110">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="b9c26-111">A folyamat fut, és ellenőrizze a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b9c26-111">Run the pipeline and verify the web app</span></span>

<span data-ttu-id="b9c26-112">Az oktatóanyaghoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="b9c26-112">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b9c26-113">A verzió megkereséséhez futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b9c26-113">To find the version, run `az --version`.</span></span> <span data-ttu-id="b9c26-114">Ha frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b9c26-114">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="b9c26-115">Hozzon létre és Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9c26-115">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="b9c26-116">Ha még nem rendelkezik egy Jenkins master, kezdje a [Megoldássablonban](install-jenkins-solution-template.md), mely tartalmazza a szükséges [Azure hitelesítő adatok](https://plugins.jenkins.io/azure-credentials) beépülő modul alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b9c26-116">If you do not already have a Jenkins master, start with the [Solution Template](install-jenkins-solution-template.md), which includes the required [Azure Credentials](https://plugins.jenkins.io/azure-credentials) plugin by default.</span></span> 

<span data-ttu-id="b9c26-117">Az Azure hitelesítő adat beépülő modul a Microsoft Azure szolgáltatás egyszerű hitelesítő adatok tárolása Jenkins teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="b9c26-117">The Azure Credential plugin allows you to store Microsoft Azure service principal credentials in Jenkins.</span></span> <span data-ttu-id="b9c26-118">1.2-es verziója, az azt támogatása a úgy, hogy Jenkins csővezeték beszerezheti az Azure hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="b9c26-118">In version 1.2, we added the support so that Jenkins Pipeline can get the Azure credentials.</span></span> 

<span data-ttu-id="b9c26-119">Győződjön meg arról, hogy 1.2-es vagy újabb verziót:</span><span class="sxs-lookup"><span data-stu-id="b9c26-119">Ensure you have version 1.2 or later:</span></span>
* <span data-ttu-id="b9c26-120">Belül Jenkins irányítópulton kattintson **Jenkins kezelése beépülő modul Manager -> ->** keresse meg a **Azure hitelesítő**.</span><span class="sxs-lookup"><span data-stu-id="b9c26-120">Within the Jenkins dashboard, click **Manage Jenkins -> Plugin Manager ->** and search for **Azure Credential**.</span></span> 
* <span data-ttu-id="b9c26-121">Frissítse a beépülő modul, ha a régebbi, mint 1.2-es.</span><span class="sxs-lookup"><span data-stu-id="b9c26-121">Update the plugin if the version is earlier than 1.2.</span></span>

<span data-ttu-id="b9c26-122">A Jenkins fő Java JDK és Maven is szükséges.</span><span class="sxs-lookup"><span data-stu-id="b9c26-122">Java JDK and Maven are also required in the Jenkins master.</span></span> <span data-ttu-id="b9c26-123">Telepítéséhez Jenkins fő SSH használatával bejelentkezni, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b9c26-123">To install, log in to Jenkins master using SSH and run the following commands:</span></span>
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a><span data-ttu-id="b9c26-124">Adja hozzá az Azure szolgáltatás egyszerű Jenkins hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="b9c26-124">Add Azure service principal to Jenkins credential</span></span>

<span data-ttu-id="b9c26-125">Egy Azure hitelesítő adatok Azure CLI végrehajtására van szükség.</span><span class="sxs-lookup"><span data-stu-id="b9c26-125">An Azure credential is needed to execute Azure CLI.</span></span>

* <span data-ttu-id="b9c26-126">Belül Jenkins irányítópulton kattintson **hitelesítő adatok -> rendszer ->**.</span><span class="sxs-lookup"><span data-stu-id="b9c26-126">Within the Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="b9c26-127">Kattintson a **globális credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="b9c26-127">Click **Global credentials(unrestricted)**.</span></span>
* <span data-ttu-id="b9c26-128">Kattintson a **adja hozzá a hitelesítő adatok** hozzáadása egy [Microsoft Azure szolgáltatás egyszerű](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) a előfizetés-azonosító, az ügyfél-azonosító, a titkos Ügyfélkulcs és a OAuth 2.0 Token-végpont kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="b9c26-128">Click **Add Credentials** to add a [Microsoft Azure service principal](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) by filling out the Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="b9c26-129">Adja meg a Azonosítót az ezt követő lépés felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="b9c26-129">Provide an ID for use in subsequent step.</span></span>

![Adja hozzá a hitelesítő adatok](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a><span data-ttu-id="b9c26-131">Az Azure App Service üzembe helyezéséhez a Java-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9c26-131">Create an Azure App Service for deploying the Java web app</span></span>

<span data-ttu-id="b9c26-132">Az Azure App Service-csomagot hozzon létre a **szabad** árképzési szint használatával a [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="b9c26-132">Create an Azure App Service plan with the **FREE** pricing tier using the  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="b9c26-133">Az App Service-csomagot határoz meg a fizikai erőforrásokat, az alkalmazások tárolására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="b9c26-133">The appservice plan defines the physical resources used to host your apps.</span></span> <span data-ttu-id="b9c26-134">Az App Service-csomagra hozzárendelt összes alkalmazás ossza meg ezeket az erőforrásokat, lehetővé téve költség menthetők, ha több alkalmazást üzemeltető.</span><span class="sxs-lookup"><span data-stu-id="b9c26-134">All applications assigned to an appservice plan share these resources, allowing you to save cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="b9c26-135">Amikor készen áll a csomag, az Azure parancssori felület az alábbi példához hasonló kimenetét mutatja be:</span><span class="sxs-lookup"><span data-stu-id="b9c26-135">When the plan is ready, the Azure CLI shows similar output to the following example:</span></span>

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

### <a name="create-an-azure-web-app"></a><span data-ttu-id="b9c26-136">Azure-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9c26-136">Create an Azure Web app</span></span>

 <span data-ttu-id="b9c26-137">Használja a [az webalkalmazás létrehozása](/cli/azure/appservice/web#create) CLI parancs futtatásával hozzon létre egy webalkalmazás az app-definíciót a `myAppServicePlan` App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="b9c26-137">Use the [az webapp create](/cli/azure/appservice/web#create) CLI command to create a web app definition in the `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="b9c26-138">A web app-definíciót érhetik el az alkalmazást az URL-CÍMÉT tartalmazza, és beállítja a kód telepítésére, az Azure számos lehetőség közül választhat.</span><span class="sxs-lookup"><span data-stu-id="b9c26-138">The web app definition provides a URL to access your application with and configures several options to deploy your code to Azure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="b9c26-139">Helyettesítő a `<app_name>` helyőrző a saját egyedi alkalmazás nevével.</span><span class="sxs-lookup"><span data-stu-id="b9c26-139">Substitute the `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="b9c26-140">A egyedi név része a webes alkalmazás esetén az alapértelmezett tartomány nevét, a nevének egyedinek kell lennie az Azure-ban minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="b9c26-140">This unique name is part of the default domain name for the web app, so the name needs to be unique across all apps in Azure.</span></span> <span data-ttu-id="b9c26-141">A web app egy egyéni tartomány bejegyzése előtt a felhasználók számára közzétenni leképezheti.</span><span class="sxs-lookup"><span data-stu-id="b9c26-141">You can map a custom domain name entry to the web app before you expose it to your users.</span></span>

<span data-ttu-id="b9c26-142">Amikor készen áll a web app-definíciót, az Azure parancssori felület információkat jeleníti meg az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="b9c26-142">When the web app definition is ready, the Azure CLI shows information similar to the following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="b9c26-143">Java konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9c26-143">Configure Java</span></span> 

<span data-ttu-id="b9c26-144">A Java runtime konfiguráció az alkalmazást igénylő a [az App Service web config frissítés](/cli/azure/appservice/web/config#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b9c26-144">Set up the Java runtime configuration that your app needs with the  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="b9c26-145">A következő parancsot konfigurálása a webalkalmazás a legújabb Java 8 JDK futtathatnak és [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="b9c26-145">The following command configures the web app to run on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a><span data-ttu-id="b9c26-146">A GitHub-tárházban előkészítése</span><span class="sxs-lookup"><span data-stu-id="b9c26-146">Prepare a GitHub Repository</span></span>
<span data-ttu-id="b9c26-147">Nyissa meg a [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) tárházban.</span><span class="sxs-lookup"><span data-stu-id="b9c26-147">Open the [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo.</span></span> <span data-ttu-id="b9c26-148">A tárház a saját GitHub-fiók oszthatja ketté, kattintson a **elágazás** gombra a jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="b9c26-148">To fork the repo to your own GitHub account, click the **Fork** button in the top right-hand corner.</span></span>

* <span data-ttu-id="b9c26-149">Nyissa meg a GitHub webes felhasználói felület, **Jenkinsfile** fájlt.</span><span class="sxs-lookup"><span data-stu-id="b9c26-149">In GitHub web UI, open **Jenkinsfile** file.</span></span> <span data-ttu-id="b9c26-150">Kattintson a ceruza ikonra az erőforráscsoportot és a sor 20 és 21 webalkalmazás neve rendre frissíteni a fájl szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="b9c26-150">Click the pencil icon to edit this file to update the resource group and name of your web app on line 20 and 21 respectively.</span></span>

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* <span data-ttu-id="b9c26-151">Módosítsa a sor 23-hitelesítő adat azonosító Jenkins betűtípusainak módosítása</span><span class="sxs-lookup"><span data-stu-id="b9c26-151">Change line 23 to update credential ID in your Jenkins instance</span></span>

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a><span data-ttu-id="b9c26-152">Jenkins folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9c26-152">Create Jenkins pipeline</span></span>
<span data-ttu-id="b9c26-153">Nyissa meg webböngészővel Jenkins, kattintson **új elem**.</span><span class="sxs-lookup"><span data-stu-id="b9c26-153">Open Jenkins in a web browser, click **New Item**.</span></span> 

* <span data-ttu-id="b9c26-154">Adjon meg egy nevet a feladatot, és válasszon **csővezeték**.</span><span class="sxs-lookup"><span data-stu-id="b9c26-154">Provide a name for the job and select **Pipeline**.</span></span> <span data-ttu-id="b9c26-155">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b9c26-155">Click **OK**.</span></span>
* <span data-ttu-id="b9c26-156">Kattintson a **csővezeték** lapon mellett.</span><span class="sxs-lookup"><span data-stu-id="b9c26-156">Click the **Pipeline** tab next.</span></span> 
* <span data-ttu-id="b9c26-157">A **Definition**, jelölje be **parancsfájl az SCM-feldolgozási folyamat**.</span><span class="sxs-lookup"><span data-stu-id="b9c26-157">For **Definition**, select **Pipeline script from SCM**.</span></span>
* <span data-ttu-id="b9c26-158">A **SCM**, jelölje be **Git**.</span><span class="sxs-lookup"><span data-stu-id="b9c26-158">For **SCM**, select **Git**.</span></span>
* <span data-ttu-id="b9c26-159">Adja meg a GitHub URL-címet a villás tárház: https:\<a villás tárház\>.git</span><span class="sxs-lookup"><span data-stu-id="b9c26-159">Enter the GitHub URL for your forked repo: https:\<your forked repo\>.git</span></span>
* <span data-ttu-id="b9c26-160">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="b9c26-160">Click **Save**</span></span>

## <a name="test-your-pipeline"></a><span data-ttu-id="b9c26-161">A folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="b9c26-161">Test your pipeline</span></span>
* <span data-ttu-id="b9c26-162">Nyissa meg a létrehozott folyamat, kattintson a **Build most**</span><span class="sxs-lookup"><span data-stu-id="b9c26-162">Go to the pipeline you created, click **Build Now**</span></span>
* <span data-ttu-id="b9c26-163">Néhány másodpercen belül a build sikeres legyen, és nyissa meg a buildre, és kattintson a **konzol kimeneti** a részletek megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="b9c26-163">A build should succeed in a few seconds, and you can go to the build and click **Console Output** to see the details</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="b9c26-164">A webalkalmazás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b9c26-164">Verify your web app</span></span>
<span data-ttu-id="b9c26-165">A WAR ellenőrzése fájl sikeresen telepítve lett a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b9c26-165">To verify the WAR file is deployed successfully to your web app.</span></span> <span data-ttu-id="b9c26-166">Nyisson meg egy webböngészőt:</span><span class="sxs-lookup"><span data-stu-id="b9c26-166">Open a web browser:</span></span>

* <span data-ttu-id="b9c26-167">Keresse fel a http://&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping</span><span class="sxs-lookup"><span data-stu-id="b9c26-167">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping</span></span>  
<span data-ttu-id="b9c26-168">Lásd:</span><span class="sxs-lookup"><span data-stu-id="b9c26-168">You see:</span></span>

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* <span data-ttu-id="b9c26-169">Keresse fel a http://&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) lekérni az összegük az x és y</span><span class="sxs-lookup"><span data-stu-id="b9c26-169">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>

![A Számológép: hozzáadása](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a><span data-ttu-id="b9c26-171">Az Azure Web Apphoz Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="b9c26-171">Deploy to Azure Web App on Linux</span></span>
<span data-ttu-id="b9c26-172">Most, hogy ismeri az Azure parancssori felület használata a Jenkins folyamat, a parancsfájl központi telepítése Linux Azure Web Apps módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="b9c26-172">Now that you know how to use Azure CLI in your Jenkins pipeline, you can modify the script to deploy to an Azure Web App on Linux.</span></span>

<span data-ttu-id="b9c26-173">Webes alkalmazás Linux egy másik módja a környezetben, amely Docker használandó támogatja.</span><span class="sxs-lookup"><span data-stu-id="b9c26-173">Web App on Linux supports a different way to do the deployment, which is to use Docker.</span></span> <span data-ttu-id="b9c26-174">Ha szeretné telepíteni, meg kell adnia egy Dockerfile, amely csomagokat a webalkalmazás szolgáltatás futtatási idő mellett a Docker-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="b9c26-174">To deploy, you need to provide a Dockerfile that packages your web app with service runtime into a Docker image.</span></span> <span data-ttu-id="b9c26-175">A beépülő modul majd elkészíti a kép, hogy egy Docker-beállításjegyzék és a webalkalmazás a lemezkép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b9c26-175">The plugin will then build the image, push it to a Docker registry and deploy the image to your web app.</span></span>

* <span data-ttu-id="b9c26-176">Kövesse a lépéseket [Itt](/azure/app-service-web/app-service-linux-how-to-create-web-app) egy Azure Linux rendszeren futó webalkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b9c26-176">Follow the steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) to create an Azure Web App running on Linux.</span></span>
* <span data-ttu-id="b9c26-177">Ezen cikk utasításait követve telepítse Docker Jenkins példány [cikk](https://docs.docker.com/engine/installation/linux/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="b9c26-177">Install Docker on your Jenkins instance by following the instructions in this [article](https://docs.docker.com/engine/installation/linux/ubuntu/).</span></span>
* <span data-ttu-id="b9c26-178">Egy tároló beállításjegyzék létrehozása az Azure portálon lépések segítségével [Itt](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b9c26-178">Create a Container Registry in the Azure portal by using the steps [here](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
* <span data-ttu-id="b9c26-179">Ugyanazon [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) tárház ágazik el, módosítsa a **Jenkinsfile2** fájlt:</span><span class="sxs-lookup"><span data-stu-id="b9c26-179">In the same [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo you forked, edit the **Jenkinsfile2** file:</span></span>
    * <span data-ttu-id="b9c26-180">18-21. sor, illetve frissítse az erőforráscsoport, a web app és az ACR nevét.</span><span class="sxs-lookup"><span data-stu-id="b9c26-180">Line 18-21, update to the names of your resource group, web app, and ACR respectively.</span></span> 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * <span data-ttu-id="b9c26-181">Sor 24, a frissítés \<azsrvprincipal\> a hitelesítő adat azonosító</span><span class="sxs-lookup"><span data-stu-id="b9c26-181">Line 24, update \<azsrvprincipal\> to your credential ID</span></span>
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* <span data-ttu-id="b9c26-182">Hozzon létre egy új Jenkins folyamat adott ezúttal csak a Windows Azure-webalkalmazásban való üzembe helyezés esetén használhatja **Jenkinsfile2** helyette.</span><span class="sxs-lookup"><span data-stu-id="b9c26-182">Create a new Jenkins pipeline as you did when deploying to Azure web app in Windows, only this time, use **Jenkinsfile2** instead.</span></span>
* <span data-ttu-id="b9c26-183">Az új feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="b9c26-183">Run your new job.</span></span>
* <span data-ttu-id="b9c26-184">Az Azure parancssori felület futtatásával ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="b9c26-184">To verify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="b9c26-185">A következő eredményt kapja:</span><span class="sxs-lookup"><span data-stu-id="b9c26-185">You get the following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="b9c26-186">Keresse fel a http://&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="b9c26-186">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="b9c26-187">Az üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b9c26-187">You see the message:</span></span> 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="b9c26-188">Keresse fel a http://&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) lekérni az összegük az x és y</span><span class="sxs-lookup"><span data-stu-id="b9c26-188">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="b9c26-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9c26-189">Next steps</span></span>
<span data-ttu-id="b9c26-190">Ebben az oktatóanyagban egy Jenkins folyamatot, amely a forráskódot, a GitHub-tárház kivesz konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b9c26-190">In this tutorial, you configured a Jenkins pipeline that checks out the source code in GitHub repo.</span></span> <span data-ttu-id="b9c26-191">A war-fájl létrehozásához Maven fut, és az Azure parancssori felület használatával telepítése az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b9c26-191">Runs Maven to build a war file and then uses Azure CLI to deploy to Azure App Service.</span></span> <span data-ttu-id="b9c26-192">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="b9c26-192">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b9c26-193">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9c26-193">Create a Jenkins VM</span></span>
> * <span data-ttu-id="b9c26-194">Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9c26-194">Configure Jenkins</span></span>
> * <span data-ttu-id="b9c26-195">Webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b9c26-195">Create a web app in Azure</span></span>
> * <span data-ttu-id="b9c26-196">A GitHub-tárházban előkészítése</span><span class="sxs-lookup"><span data-stu-id="b9c26-196">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="b9c26-197">Jenkins folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9c26-197">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="b9c26-198">A folyamat fut, és ellenőrizze a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b9c26-198">Run the pipeline and verify the web app</span></span>
