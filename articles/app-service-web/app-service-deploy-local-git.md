---
title: "Helyi üzembe helyezés Git használatával az Azure App Service szolgáltatásban"
description: "Útmutató az Azure App Service helyi Git-telepítésének engedélyezése."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: f1c4911670d3aa32e74b3dfebd83cf3dc3830805
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="local-git-deployment-to-azure-app-service"></a><span data-ttu-id="9608b-103">Helyi üzembe helyezés Git használatával az Azure App Service szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="9608b-103">Local Git Deployment to Azure App Service</span></span>
<span data-ttu-id="9608b-104">Ez az oktatóanyag bemutatja, hogyan üzembe az alkalmazást, amely [Azure App Service] a Git-tárházat a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9608b-104">This tutorial shows you how to deploy your app to [Azure App Service] from a Git repository on your local computer.</span></span> <span data-ttu-id="9608b-105">App Service támogatja ezt a módszert a a **helyi Git** a központi telepítési lehetőség a [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="9608b-105">App Service supports this approach with the **Local Git** deployment option in the [Azure Portal].</span></span>  
<span data-ttu-id="9608b-106">A jelen cikkben ismertetett Git-parancsok többsége automatikusan megtörténik, egy App Service alkalmazás használatával létrehozásakor a [Azure parancssori felület] leírtak [Itt](app-service-web-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9608b-106">Many of the Git commands described in this article are performed automatically when creating an App Service app using the [Azure Command-Line Interface] as described [here](app-service-web-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9608b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9608b-107">Prerequisites</span></span>
<span data-ttu-id="9608b-108">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="9608b-108">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="9608b-109">Git.</span><span class="sxs-lookup"><span data-stu-id="9608b-109">Git.</span></span> <span data-ttu-id="9608b-110">Letöltheti a bináris telepítési [Itt](http://www.git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="9608b-110">You can download the installation binary [here](http://www.git-scm.com/downloads).</span></span>  
* <span data-ttu-id="9608b-111">Git alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="9608b-111">Basic knowledge of Git.</span></span>
* <span data-ttu-id="9608b-112">Egy Microsoft Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="9608b-112">A Microsoft Azure account.</span></span> <span data-ttu-id="9608b-113">Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span><span class="sxs-lookup"><span data-stu-id="9608b-113">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span></span>

> [!NOTE]
> <span data-ttu-id="9608b-114">Ha az Azure App Service első lépései az Azure-fiók regisztrálása előtt szeretné, folytassa a [App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű alkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="9608b-114">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter app in App Service.</span></span> <span data-ttu-id="9608b-115">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="9608b-115">No credit cards required; no commitments.</span></span>  
> 
> 

## <span data-ttu-id="9608b-116"><a name="Step1"></a>1. lépés: A helyi tárház létrehozása</span><span class="sxs-lookup"><span data-stu-id="9608b-116"><a name="Step1"></a>Step 1: Create a local repository</span></span>
<span data-ttu-id="9608b-117">A következő feladatokat hozzon létre egy új Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="9608b-117">Perform the following tasks to create a new Git repository.</span></span>

1. <span data-ttu-id="9608b-118">Indítsa el a parancssori eszköz, például a **GitBash** (Windows) vagy **Bash** (Unix rendszerhéj).</span><span class="sxs-lookup"><span data-stu-id="9608b-118">Start a command-line tool, such as **GitBash** (Windows) or **Bash** (Unix Shell).</span></span> <span data-ttu-id="9608b-119">OS X rendszeren is elérheti a parancssori keresztül a **Terminálszolgáltatások** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9608b-119">On OS X systems you can access the command-line through the **Terminal** application.</span></span>
2. <span data-ttu-id="9608b-120">Keresse meg azt a könyvtárat, amelyben a tartalom központi telepítéséhez volna található.</span><span class="sxs-lookup"><span data-stu-id="9608b-120">Navigate to the directory where the content to deploy would be located.</span></span>
3. <span data-ttu-id="9608b-121">A következő paranccsal egy új Git-tárház inicializálása:</span><span class="sxs-lookup"><span data-stu-id="9608b-121">Use the following command to initialize a new Git repository:</span></span>

```bash  
git init
```

## <span data-ttu-id="9608b-122"><a name="Step2"></a>2. lépés: A tartalom véglegesítése</span><span class="sxs-lookup"><span data-stu-id="9608b-122"><a name="Step2"></a>Step 2: Commit your content</span></span>
<span data-ttu-id="9608b-123">App Service számos programozási nyelven készült alkalmazások támogatja.</span><span class="sxs-lookup"><span data-stu-id="9608b-123">App Service supports applications created in a variety of programming languages.</span></span> 

1. <span data-ttu-id="9608b-124">Ha a tárház már tartalmazza a tartalom kihagyása a pont, és helyezze át az alábbi 2 mutasson.</span><span class="sxs-lookup"><span data-stu-id="9608b-124">If your repository already includes content skip this point and move to point 2 below.</span></span> <span data-ttu-id="9608b-125">Ha a tárház már tartalmazza a tartalom egyszerűen feltölteni egy statikus .html fájllal az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9608b-125">If your repository does not already include content simply populate with a static .html file as follows:</span></span> 
   
   * <span data-ttu-id="9608b-126">Egy szövegszerkesztő használatával hozzon létre egy új fájlt **index.html** a Git-tárház gyökérkönyvtárában</span><span class="sxs-lookup"><span data-stu-id="9608b-126">Using a text editor, create a new file named **index.html** at the root of the Git repository</span></span>
   * <span data-ttu-id="9608b-127">Adja hozzá a következő szöveget a index.html tartalma fájlt, és mentse: *Hello Git!*</span><span class="sxs-lookup"><span data-stu-id="9608b-127">Add the following text as the contents for the index.html file and save it: *Hello Git!*</span></span>
2. <span data-ttu-id="9608b-128">A parancssorból ellenőrizze, hogy a Git-tárház gyökerében.</span><span class="sxs-lookup"><span data-stu-id="9608b-128">From the command-line, verify that you are under the root of your Git repository.</span></span> <span data-ttu-id="9608b-129">A következő parancs használatával adja hozzá a fájlok a tárházhoz:</span><span class="sxs-lookup"><span data-stu-id="9608b-129">Then use the following command to add files to your repository:</span></span>

```bash  
git add -A
```
3. <span data-ttu-id="9608b-130">Ezt követően véglegesítse a módosításokat a tárházba a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="9608b-130">Next, commit the changes to the repository by using the following command:</span></span>

```bash  
git commit -m "Hello Azure App Service"
```  

## <span data-ttu-id="9608b-131"><a name="Step3"></a>3. lépés: Az App Service alkalmazás tárház engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9608b-131"><a name="Step3"></a>Step 3: Enable the App Service app repository</span></span>
<span data-ttu-id="9608b-132">A következő lépésekkel ahhoz, hogy egy App Service-alkalmazás Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="9608b-132">Perform the following steps to enable a Git repository for your App Service app.</span></span>

1. <span data-ttu-id="9608b-133">Jelentkezzen be az [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="9608b-133">Log in to the [Azure Portal].</span></span>
2. <span data-ttu-id="9608b-134">Az App Service-alkalmazás paneljén kattintson **beállítások > központi telepítés forrásának**.</span><span class="sxs-lookup"><span data-stu-id="9608b-134">In your App Service app's blade, click **Settings > Deployment source**.</span></span> <span data-ttu-id="9608b-135">Kattintson a **forrás választása**, majd kattintson a **helyi Git-tárház**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9608b-135">Click **Choose source**, then click **Local Git Repository**, and then click **OK**.</span></span>  
   
    ![Helyi Git-tárház](./media/app-service-deploy-local-git/local_git_selection.png)
3. <span data-ttu-id="9608b-137">Ha ez az első idő beállítása a tárházat az Azure-ban, a bejelentkezési hitelesítő adatok létrehozása az szeretné.</span><span class="sxs-lookup"><span data-stu-id="9608b-137">If this is your first time setting up a repository in Azure, you need to create login credentials for it.</span></span> <span data-ttu-id="9608b-138">Jelentkezzen be a Azure tárház és leküldéses módosítása a helyi Git-tárház a őket használandó.</span><span class="sxs-lookup"><span data-stu-id="9608b-138">You will use them to log into the Azure repository and push changes from your local Git repository.</span></span> <span data-ttu-id="9608b-139">Az alkalmazás paneljén kattintson **beállítások > üzembe helyezési hitelesítő adatok**, majd konfigurálja a központi telepítés felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9608b-139">From your app's blade, click **Settings > Deployment credentials**, then configure your deployment username and password.</span></span> <span data-ttu-id="9608b-140">Amikor elkészült, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9608b-140">When you're done, click **Save**.</span></span>
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <span data-ttu-id="9608b-141"><a name="Step4"></a>4. lépés: A projekt telepítése</span><span class="sxs-lookup"><span data-stu-id="9608b-141"><a name="Step4"></a>Step 4: Deploy your project</span></span>
<span data-ttu-id="9608b-142">Az alábbi lépések segítségével közzététele az alkalmazás az App Service helyi Git használatával.</span><span class="sxs-lookup"><span data-stu-id="9608b-142">Use the following steps to publish your app to App Service using Local Git.</span></span>

1. <span data-ttu-id="9608b-143">Az Azure portálon az alkalmazás paneljén kattintson **beállítások > Tulajdonságok** a a **Git URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="9608b-143">In your app's blade in the Azure Portal, click **Settings > Properties** for the **Git URL**.</span></span>
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    <span data-ttu-id="9608b-144">**Git URL-cím** a helyi tárházból telepítendő Távoli hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="9608b-144">**Git URL** is the remote reference to deploy to from your local repository.</span></span> <span data-ttu-id="9608b-145">Az URL-cím a következő lépésben fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9608b-145">You'll use this URL in the following steps.</span></span>
2. <span data-ttu-id="9608b-146">Használatával a parancssorból, ellenőrizze, hogy a helyi Git-tárház gyökérkönyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="9608b-146">Using the command-line, verify that you are in the root of your local Git repository.</span></span>
3. <span data-ttu-id="9608b-147">Használjon `git remote` hozzáadása a távoli leírásában felsorolt **Git URL-cím** az 1. lépés.</span><span class="sxs-lookup"><span data-stu-id="9608b-147">Use `git remote` to add the remote reference listed in **Git URL** from step 1.</span></span> <span data-ttu-id="9608b-148">A parancs az alábbihoz hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="9608b-148">Your command will look similar to the following:</span></span>
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > <span data-ttu-id="9608b-149">A **távoli** parancs hozzáad egy elnevezett hivatkozást egy távoli tárházba.</span><span class="sxs-lookup"><span data-stu-id="9608b-149">The **remote** command adds a named reference to a remote repository.</span></span> <span data-ttu-id="9608b-150">Ebben a példában a webalkalmazás-tárház "azure" nevű hivatkozást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9608b-150">In this example, it creates a reference named 'azure' for your web app's repository.</span></span>
   > 
   > 
4. <span data-ttu-id="9608b-151">Küldje le a tartalmat az új App Service **azure** távoli újonnan létrehozott.</span><span class="sxs-lookup"><span data-stu-id="9608b-151">Push your content to App Service using the new **azure** remote you just created.</span></span>

```bash  
git push azure master
```
    You will be prompted for the password you created earlier when you reset your deployment credentials in the Azure Portal. Enter the password (note that Gitbash does not echo asterisks to the console as you type your password). 
5. <span data-ttu-id="9608b-152">Térjen vissza az alkalmazást az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9608b-152">Go back to your app in the Azure Portal.</span></span> <span data-ttu-id="9608b-153">A legutóbbi leküldéses a naplóbejegyzést kell megjelennie a **központi telepítések** panelen.</span><span class="sxs-lookup"><span data-stu-id="9608b-153">A log entry of your most recent push should be displayed in the **Deployments** blade.</span></span> 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. <span data-ttu-id="9608b-154">Kattintson a **Tallózás** gomb a tartalom központi telepítésének ellenőrzése az alkalmazás panel tetején.</span><span class="sxs-lookup"><span data-stu-id="9608b-154">Click the **Browse** button at the top of the app's blade to verify the content has been deployed.</span></span> 

## <span data-ttu-id="9608b-155"><a name="Step5"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9608b-155"><a name="Step5"></a>Troubleshooting</span></span>
<span data-ttu-id="9608b-156">Hibák vagy problémák gyakran fordul elő, ha a Git használatával az Azure App Service alkalmazás közzététele a következők:</span><span class="sxs-lookup"><span data-stu-id="9608b-156">The following are errors or problems commonly encountered when using Git to publish to an App Service app in Azure:</span></span>

- - -
<span data-ttu-id="9608b-157">**Jelenség**: nem sikerült hozzáférési [siteURL]: nem sikerült csatlakozni az [scmAddress]</span><span class="sxs-lookup"><span data-stu-id="9608b-157">**Symptom**: Unable to access '[siteURL]': Failed to connect to [scmAddress]</span></span>

<span data-ttu-id="9608b-158">**OK**: Ez a hiba akkor fordulhat elő, ha az alkalmazás nem válik működőképessé.</span><span class="sxs-lookup"><span data-stu-id="9608b-158">**Cause**: This error can occur if the app is not up and running.</span></span>

<span data-ttu-id="9608b-159">**Megoldási**: Indítsa el az alkalmazást az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9608b-159">**Resolution**: Start the app in the Azure Portal.</span></span> <span data-ttu-id="9608b-160">Git-telepítés nem fog működni, kivéve, ha az alkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="9608b-160">Git deployment will not work unless the app is running.</span></span> 

- - -
<span data-ttu-id="9608b-161">**Jelenség**: nem sikerült feloldani a gazdagép "állomásnév"</span><span class="sxs-lookup"><span data-stu-id="9608b-161">**Symptom**: Couldn't resolve host 'hostname'</span></span>

<span data-ttu-id="9608b-162">**OK**: Ez a hiba akkor fordulhat elő, ha a megadott létrehozásakor az "azure" távoli címadatokat helytelen volt.</span><span class="sxs-lookup"><span data-stu-id="9608b-162">**Cause**: This error can occur if the address information entered when creating the 'azure' remote was incorrect.</span></span>

<span data-ttu-id="9608b-163">**Megoldási**: használja a `git remote -v` paranccsal listát készíthet az összes távvezérlő, és a kapcsolódó URL-címe.</span><span class="sxs-lookup"><span data-stu-id="9608b-163">**Resolution**: Use the `git remote -v` command to list all remotes, along with the associated URL.</span></span> <span data-ttu-id="9608b-164">Ellenőrizze, hogy helyes-e az "azure" távoli URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9608b-164">Verify that the URL for the 'azure' remote is correct.</span></span> <span data-ttu-id="9608b-165">Ha szükséges, távolítsa el és hozza létre újból a távoli, a helyes URL-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="9608b-165">If needed, remove and recreate this remote using the correct URL.</span></span>

- - -
<span data-ttu-id="9608b-166">**Jelenség**: nincs közös refs, és nincs megadva; semmi sem történt.</span><span class="sxs-lookup"><span data-stu-id="9608b-166">**Symptom**: No refs in common and none specified; doing nothing.</span></span> <span data-ttu-id="9608b-167">Lehet, hogy adja meg például a "master" ág.</span><span class="sxs-lookup"><span data-stu-id="9608b-167">Perhaps you should specify a branch such as 'master'.</span></span>

<span data-ttu-id="9608b-168">**OK**: Ez a hiba akkor fordulhat elő, ha nem ad meg egy ágat git push műveletet, és nem állított be Git által használt push.default értékét.</span><span class="sxs-lookup"><span data-stu-id="9608b-168">**Cause**: This error can occur if you do not specify a branch when performing a git push operation, and have not set the push.default value used by Git.</span></span>

<span data-ttu-id="9608b-169">**Megoldási**: végrehajtani a leküldéses műveletet, adja meg a főágba.</span><span class="sxs-lookup"><span data-stu-id="9608b-169">**Resolution**: Perform the push operation again, specifying the master branch.</span></span> <span data-ttu-id="9608b-170">Példa:</span><span class="sxs-lookup"><span data-stu-id="9608b-170">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="9608b-171">**Jelenség**: src refspec [branchname] nem felel meg egyik sem.</span><span class="sxs-lookup"><span data-stu-id="9608b-171">**Symptom**: src refspec [branchname] does not match any.</span></span>

<span data-ttu-id="9608b-172">**OK**: Ez a hiba akkor fordulhat elő, ha megkísérli leküldése egy ágat a főadatbázison kívül az "Azure" távoli.</span><span class="sxs-lookup"><span data-stu-id="9608b-172">**Cause**: This error can occur if you attempt to push to a branch other than master on the 'azure' remote.</span></span>

<span data-ttu-id="9608b-173">**Megoldási**: végrehajtani a leküldéses műveletet, adja meg a főágba.</span><span class="sxs-lookup"><span data-stu-id="9608b-173">**Resolution**: Perform the push operation again, specifying the master branch.</span></span> <span data-ttu-id="9608b-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="9608b-174">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="9608b-175">**Jelenség**: RPC sikertelen; a eredménye = 22, HTTP-kód = 502-es.</span><span class="sxs-lookup"><span data-stu-id="9608b-175">**Symptom**: RPC failed; result=22, HTTP code = 502.</span></span>

<span data-ttu-id="9608b-176">**OK**: Ez a hiba akkor fordulhat elő, ha megkísérli a nagy git-tárház leküldéses HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="9608b-176">**Cause**: This error can occur if you attempt to push a large git repository over HTTPS.</span></span>

<span data-ttu-id="9608b-177">**Megoldási**: a helyi gépen a postBuffer nagyobb git konfigurációjának módosítása</span><span class="sxs-lookup"><span data-stu-id="9608b-177">**Resolution**: Change the git configuration on the local machine to make the postBuffer bigger</span></span>

```bash  
git config --global http.postBuffer 524288000
```
- - -
<span data-ttu-id="9608b-178">**Jelenség**: hiba - távoli tárházba véglegesített módosításokat, de a webalkalmazás nem frissül.</span><span class="sxs-lookup"><span data-stu-id="9608b-178">**Symptom**: Error - Changes committed to remote repository but your web app not updated.</span></span>

<span data-ttu-id="9608b-179">**OK**: Ez a hiba akkor fordulhat elő, ha telepít egy Node.js-alkalmazás, amely meghatározza a további szükséges modulokat package.json fájlt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="9608b-179">**Cause**: This error can occur if you are deploying a Node.js app containing a package.json file that specifies additional required modules.</span></span>

<span data-ttu-id="9608b-180">**Megoldási**: további üzeneteket tartalmazó "npm hiba!"</span><span class="sxs-lookup"><span data-stu-id="9608b-180">**Resolution**: Additional messages containing 'npm ERR!'</span></span> <span data-ttu-id="9608b-181">Ez a hiba a naplózott előtt legyen, és a hiba esetén további környezetet biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="9608b-181">should be logged prior to this error, and can provide additional context on the failure.</span></span> <span data-ttu-id="9608b-182">A következő ismert oka ezt a hibát, és a megfelelő "npm hiba!"</span><span class="sxs-lookup"><span data-stu-id="9608b-182">The following are known causes of this error and the corresponding 'npm ERR!'</span></span> <span data-ttu-id="9608b-183">üzenet:</span><span class="sxs-lookup"><span data-stu-id="9608b-183">message:</span></span>

* <span data-ttu-id="9608b-184">**Nem megfelelően formázott package.json fájl**: npm hiba!</span><span class="sxs-lookup"><span data-stu-id="9608b-184">**Malformed package.json file**: npm ERR!</span></span> <span data-ttu-id="9608b-185">Nem lehetett olvasni a függőségek.</span><span class="sxs-lookup"><span data-stu-id="9608b-185">Couldn't read dependencies.</span></span>
* <span data-ttu-id="9608b-186">**Natív modul, amely nem rendelkezik Windows bináris terjesztési**:</span><span class="sxs-lookup"><span data-stu-id="9608b-186">**Native module that does not have a binary distribution for Windows**:</span></span>
  
  * <span data-ttu-id="9608b-187">npm hiba!</span><span class="sxs-lookup"><span data-stu-id="9608b-187">npm ERR!</span></span> <span data-ttu-id="9608b-188">\`cmd "/ c" "csomópont-gyp rebuild"\` sikertelen 1</span><span class="sxs-lookup"><span data-stu-id="9608b-188">\`cmd "/c" "node-gyp rebuild"\` failed with 1</span></span>
    
      <span data-ttu-id="9608b-189">VAGY</span><span class="sxs-lookup"><span data-stu-id="9608b-189">OR</span></span>
  * <span data-ttu-id="9608b-190">npm hiba!</span><span class="sxs-lookup"><span data-stu-id="9608b-190">npm ERR!</span></span> <span data-ttu-id="9608b-191">[modulename@version] preinstall: \`ellenőrizze || gmake\`</span><span class="sxs-lookup"><span data-stu-id="9608b-191">[modulename@version] preinstall: \`make || gmake\`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9608b-192">További források</span><span class="sxs-lookup"><span data-stu-id="9608b-192">Additional Resources</span></span>
* [<span data-ttu-id="9608b-193">A Git dokumentációja</span><span class="sxs-lookup"><span data-stu-id="9608b-193">Git documentation</span></span>](http://git-scm.com/documentation)
* [<span data-ttu-id="9608b-194">Project Kudu dokumentációja</span><span class="sxs-lookup"><span data-stu-id="9608b-194">Project Kudu documentation</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="9608b-195">Folyamatos telepítés az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9608b-195">Continous Deployment to Azure App Service</span></span>](app-service-continuous-deployment.md)
* [<span data-ttu-id="9608b-196">A PowerShell használata az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="9608b-196">How to use PowerShell for Azure</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="9608b-197">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="9608b-197">How to use the Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)

<span data-ttu-id="9608b-198">[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/</span><span class="sxs-lookup"><span data-stu-id="9608b-198">[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/</span></span>
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
<span data-ttu-id="9608b-199">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="9608b-199">[Azure Portal]: https://portal.azure.com</span></span>
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
<span data-ttu-id="9608b-200">[Azure parancssori felület]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/</span><span class="sxs-lookup"><span data-stu-id="9608b-200">[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/</span></span>

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
