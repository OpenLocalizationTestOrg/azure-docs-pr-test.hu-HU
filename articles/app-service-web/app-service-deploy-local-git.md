---
title: "aaaLocal Git-telepítésének tooAzure App Service"
description: "Megtudhatja, hogyan tooenable helyi Git telepítési tooAzure App Service."
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
ms.openlocfilehash: 1905e0b7acd58d8dd6496a14f6e4f38f9f8c0212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="local-git-deployment-tooazure-app-service"></a><span data-ttu-id="55174-103">Helyi Git-telepítésének tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="55174-103">Local Git Deployment tooAzure App Service</span></span>
<span data-ttu-id="55174-104">Az oktatóanyag bemutatja, hogyan toodeploy az alkalmazás túl[Azure App Service] a Git-tárházat a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="55174-104">This tutorial shows you how toodeploy your app too[Azure App Service] from a Git repository on your local computer.</span></span> <span data-ttu-id="55174-105">App Service támogatja ezt a módszert a hello **helyi Git** hello a rendszerbe állítási beállításának [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="55174-105">App Service supports this approach with hello **Local Git** deployment option in hello [Azure Portal].</span></span>  
<span data-ttu-id="55174-106">Ebben a cikkben leírt hello Git-parancsok számos automatikusan megtörténik, egy App Service alkalmazáshoz hello létrehozásakor [Azure parancssori felület] leírtak [Itt](app-service-web-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="55174-106">Many of hello Git commands described in this article are performed automatically when creating an App Service app using hello [Azure Command-Line Interface] as described [here](app-service-web-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55174-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="55174-107">Prerequisites</span></span>
<span data-ttu-id="55174-108">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="55174-108">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="55174-109">Git.</span><span class="sxs-lookup"><span data-stu-id="55174-109">Git.</span></span> <span data-ttu-id="55174-110">Letöltheti a bináris hello telepítési [Itt](http://www.git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="55174-110">You can download hello installation binary [here](http://www.git-scm.com/downloads).</span></span>  
* <span data-ttu-id="55174-111">Git alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="55174-111">Basic knowledge of Git.</span></span>
* <span data-ttu-id="55174-112">Egy Microsoft Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="55174-112">A Microsoft Azure account.</span></span> <span data-ttu-id="55174-113">Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span><span class="sxs-lookup"><span data-stu-id="55174-113">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span></span>

> [!NOTE]
> <span data-ttu-id="55174-114">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű alkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="55174-114">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter app in App Service.</span></span> <span data-ttu-id="55174-115">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="55174-115">No credit cards required; no commitments.</span></span>  
> 
> 

## <span data-ttu-id="55174-116"><a name="Step1"></a>1. lépés: A helyi tárház létrehozása</span><span class="sxs-lookup"><span data-stu-id="55174-116"><a name="Step1"></a>Step 1: Create a local repository</span></span>
<span data-ttu-id="55174-117">Hajtsa végre a következő feladatok toocreate új Git-tárház hello.</span><span class="sxs-lookup"><span data-stu-id="55174-117">Perform hello following tasks toocreate a new Git repository.</span></span>

1. <span data-ttu-id="55174-118">Indítsa el a parancssori eszköz, például a **GitBash** (Windows) vagy **Bash** (Unix rendszerhéj).</span><span class="sxs-lookup"><span data-stu-id="55174-118">Start a command-line tool, such as **GitBash** (Windows) or **Bash** (Unix Shell).</span></span> <span data-ttu-id="55174-119">OS X rendszereken érheti el parancssori hello hello **Terminálszolgáltatások** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="55174-119">On OS X systems you can access hello command-line through hello **Terminal** application.</span></span>
2. <span data-ttu-id="55174-120">Keresse meg a toohello könyvtár, amelyben hello tartalom toodeploy volna található.</span><span class="sxs-lookup"><span data-stu-id="55174-120">Navigate toohello directory where hello content toodeploy would be located.</span></span>
3. <span data-ttu-id="55174-121">A következő parancs tooinitialize új Git-tárház hello használata:</span><span class="sxs-lookup"><span data-stu-id="55174-121">Use hello following command tooinitialize a new Git repository:</span></span>

```bash  
git init
```

## <span data-ttu-id="55174-122"><a name="Step2"></a>2. lépés: A tartalom véglegesítése</span><span class="sxs-lookup"><span data-stu-id="55174-122"><a name="Step2"></a>Step 2: Commit your content</span></span>
<span data-ttu-id="55174-123">App Service számos programozási nyelven készült alkalmazások támogatja.</span><span class="sxs-lookup"><span data-stu-id="55174-123">App Service supports applications created in a variety of programming languages.</span></span> 

1. <span data-ttu-id="55174-124">Ha a tárház már tartalmazza a tartalom kihagyása a pont, és helyezze át az alábbi 2 toopoint.</span><span class="sxs-lookup"><span data-stu-id="55174-124">If your repository already includes content skip this point and move toopoint 2 below.</span></span> <span data-ttu-id="55174-125">Ha a tárház már tartalmazza a tartalom egyszerűen feltölteni egy statikus .html fájllal az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="55174-125">If your repository does not already include content simply populate with a static .html file as follows:</span></span> 
   
   * <span data-ttu-id="55174-126">Egy szövegszerkesztő használatával hozzon létre egy új fájlt **index.html** hello Git-tárház hello gyökérkönyvtárában</span><span class="sxs-lookup"><span data-stu-id="55174-126">Using a text editor, create a new file named **index.html** at hello root of hello Git repository</span></span>
   * <span data-ttu-id="55174-127">Adja hozzá a hello hello index.html hello tartalmának fájlt, és mentse a következő szöveget: *Hello Git!*</span><span class="sxs-lookup"><span data-stu-id="55174-127">Add hello following text as hello contents for hello index.html file and save it: *Hello Git!*</span></span>
2. <span data-ttu-id="55174-128">A parancssori hello ellenőrizze, hogy a Git-tárház hello gyökere alatt.</span><span class="sxs-lookup"><span data-stu-id="55174-128">From hello command-line, verify that you are under hello root of your Git repository.</span></span> <span data-ttu-id="55174-129">Kövesse a következő parancs tooadd fájlok tooyour tárház hello:</span><span class="sxs-lookup"><span data-stu-id="55174-129">Then use hello following command tooadd files tooyour repository:</span></span>

```bash  
git add -A
```
3. <span data-ttu-id="55174-130">Ezt követően véglegesítse hello módosítások toohello tárház hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="55174-130">Next, commit hello changes toohello repository by using hello following command:</span></span>

```bash  
git commit -m "Hello Azure App Service"
```  

## <span data-ttu-id="55174-131"><a name="Step3"></a>3. lépés: Az App Service alkalmazás tárház hello engedélyezése</span><span class="sxs-lookup"><span data-stu-id="55174-131"><a name="Step3"></a>Step 3: Enable hello App Service app repository</span></span>
<span data-ttu-id="55174-132">Hajtsa végre a következő lépéseket tooenable egy App Service-alkalmazás Git-tárházat hello.</span><span class="sxs-lookup"><span data-stu-id="55174-132">Perform hello following steps tooenable a Git repository for your App Service app.</span></span>

1. <span data-ttu-id="55174-133">Jelentkezzen be toohello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="55174-133">Log in toohello [Azure Portal].</span></span>
2. <span data-ttu-id="55174-134">Az App Service-alkalmazás paneljén kattintson **beállítások > központi telepítés forrásának**.</span><span class="sxs-lookup"><span data-stu-id="55174-134">In your App Service app's blade, click **Settings > Deployment source**.</span></span> <span data-ttu-id="55174-135">Kattintson a **forrás választása**, majd kattintson a **helyi Git-tárház**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="55174-135">Click **Choose source**, then click **Local Git Repository**, and then click **OK**.</span></span>  
   
    ![Helyi Git-tárház](./media/app-service-deploy-local-git/local_git_selection.png)
3. <span data-ttu-id="55174-137">Ha ez az első idő beállítása a tárházat az Azure-ban, hozzá kell toocreate bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="55174-137">If this is your first time setting up a repository in Azure, you need toocreate login credentials for it.</span></span> <span data-ttu-id="55174-138">Szüksége lesz őket az Azure-tárházat hello toolog, és küldje el a módosításokat a helyi Git-tárház a.</span><span class="sxs-lookup"><span data-stu-id="55174-138">You will use them toolog into hello Azure repository and push changes from your local Git repository.</span></span> <span data-ttu-id="55174-139">Az alkalmazás paneljén kattintson **beállítások > üzembe helyezési hitelesítő adatok**, majd konfigurálja a központi telepítés felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="55174-139">From your app's blade, click **Settings > Deployment credentials**, then configure your deployment username and password.</span></span> <span data-ttu-id="55174-140">Amikor elkészült, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="55174-140">When you're done, click **Save**.</span></span>
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <span data-ttu-id="55174-141"><a name="Step4"></a>4. lépés: A projekt telepítése</span><span class="sxs-lookup"><span data-stu-id="55174-141"><a name="Step4"></a>Step 4: Deploy your project</span></span>
<span data-ttu-id="55174-142">A következő lépéseket toopublish hello használata az alkalmazás tooApp helyi Git-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="55174-142">Use hello following steps toopublish your app tooApp Service using Local Git.</span></span>

1. <span data-ttu-id="55174-143">A hello Azure portálra az alkalmazás paneljén kattintson **beállítások > Tulajdonságok** a hello **Git URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="55174-143">In your app's blade in hello Azure Portal, click **Settings > Properties** for hello **Git URL**.</span></span>
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    <span data-ttu-id="55174-144">**Git URL-cím** hello távoli referencia toodeploy toofrom van a helyi tárházzal.</span><span class="sxs-lookup"><span data-stu-id="55174-144">**Git URL** is hello remote reference toodeploy toofrom your local repository.</span></span> <span data-ttu-id="55174-145">Az alábbi lépésekkel hello URL-címet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="55174-145">You'll use this URL in hello following steps.</span></span>
2. <span data-ttu-id="55174-146">Parancssori hello használ, győződjön meg arról, hogy a helyi Git-tárház hello gyökerében vannak-e.</span><span class="sxs-lookup"><span data-stu-id="55174-146">Using hello command-line, verify that you are in hello root of your local Git repository.</span></span>
3. <span data-ttu-id="55174-147">Használjon `git remote` tooadd hello távoli leírásában felsorolt **Git URL-cím** az 1. lépés.</span><span class="sxs-lookup"><span data-stu-id="55174-147">Use `git remote` tooadd hello remote reference listed in **Git URL** from step 1.</span></span> <span data-ttu-id="55174-148">A parancshoz hasonló toohello következő fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="55174-148">Your command will look similar toohello following:</span></span>
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > <span data-ttu-id="55174-149">Hello **távoli** parancs hozzáadása egy elnevezett hivatkozás tooa távoli tárházba.</span><span class="sxs-lookup"><span data-stu-id="55174-149">hello **remote** command adds a named reference tooa remote repository.</span></span> <span data-ttu-id="55174-150">Ebben a példában a webalkalmazás-tárház "azure" nevű hivatkozást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="55174-150">In this example, it creates a reference named 'azure' for your web app's repository.</span></span>
   > 
   > 
4. <span data-ttu-id="55174-151">A tartalom tooApp szolgáltatás Push használatával új hello **azure** távoli újonnan létrehozott.</span><span class="sxs-lookup"><span data-stu-id="55174-151">Push your content tooApp Service using hello new **azure** remote you just created.</span></span>

```bash  
git push azure master
```
    You will be prompted for hello password you created earlier when you reset your deployment credentials in hello Azure Portal. Enter hello password (note that Gitbash does not echo asterisks toohello console as you type your password). 
5. <span data-ttu-id="55174-152">Lépjen vissza a tooyour alkalmazás hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="55174-152">Go back tooyour app in hello Azure Portal.</span></span> <span data-ttu-id="55174-153">A legutóbbi leküldéses a naplóbejegyzést kell megjelennie hello **központi telepítések** panelen.</span><span class="sxs-lookup"><span data-stu-id="55174-153">A log entry of your most recent push should be displayed in hello **Deployments** blade.</span></span> 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. <span data-ttu-id="55174-154">Kattintson a hello **Tallózás** hello app panel tooverify hello tartalom hello tetején gomb van telepítve.</span><span class="sxs-lookup"><span data-stu-id="55174-154">Click hello **Browse** button at hello top of hello app's blade tooverify hello content has been deployed.</span></span> 

## <span data-ttu-id="55174-155"><a name="Step5"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="55174-155"><a name="Step5"></a>Troubleshooting</span></span>
<span data-ttu-id="55174-156">hello az alábbiakban láthatók a hibák vagy problémák gyakran fordul elő, ha a Git toopublish tooan App Service alkalmazás használatával az Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="55174-156">hello following are errors or problems commonly encountered when using Git toopublish tooan App Service app in Azure:</span></span>

- - -
<span data-ttu-id="55174-157">**Jelenség**: "[siteURL]" nem tooaccess: nem sikerült túl tooconnect [scmAddress]</span><span class="sxs-lookup"><span data-stu-id="55174-157">**Symptom**: Unable tooaccess '[siteURL]': Failed tooconnect too[scmAddress]</span></span>

<span data-ttu-id="55174-158">**OK**: Ez a hiba akkor fordulhat elő, ha hello alkalmazás nem válik működőképessé.</span><span class="sxs-lookup"><span data-stu-id="55174-158">**Cause**: This error can occur if hello app is not up and running.</span></span>

<span data-ttu-id="55174-159">**Megoldási**: Start hello alkalmazás hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="55174-159">**Resolution**: Start hello app in hello Azure Portal.</span></span> <span data-ttu-id="55174-160">Git-telepítés nem fog működni, ha fut a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="55174-160">Git deployment will not work unless hello app is running.</span></span> 

- - -
<span data-ttu-id="55174-161">**Jelenség**: nem sikerült feloldani a gazdagép "állomásnév"</span><span class="sxs-lookup"><span data-stu-id="55174-161">**Symptom**: Couldn't resolve host 'hostname'</span></span>

<span data-ttu-id="55174-162">**OK**: Ez a hiba akkor fordulhat elő, ha hello címadatok létrehozásakor megadott hello "azure" távoli helytelen volt.</span><span class="sxs-lookup"><span data-stu-id="55174-162">**Cause**: This error can occur if hello address information entered when creating hello 'azure' remote was incorrect.</span></span>

<span data-ttu-id="55174-163">**Megoldási**: használata hello `git remote -v` toolist parancsot minden távvezérlő együtt hello tartozó URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="55174-163">**Resolution**: Use hello `git remote -v` command toolist all remotes, along with hello associated URL.</span></span> <span data-ttu-id="55174-164">Ellenőrizze, hogy helyes-e hello hello "azure" távoli URL-címe.</span><span class="sxs-lookup"><span data-stu-id="55174-164">Verify that hello URL for hello 'azure' remote is correct.</span></span> <span data-ttu-id="55174-165">Ha szükséges, távolítsa el és hozza létre újból a távoli hello helyes URL-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="55174-165">If needed, remove and recreate this remote using hello correct URL.</span></span>

- - -
<span data-ttu-id="55174-166">**Jelenség**: nincs közös refs, és nincs megadva; semmi sem történt.</span><span class="sxs-lookup"><span data-stu-id="55174-166">**Symptom**: No refs in common and none specified; doing nothing.</span></span> <span data-ttu-id="55174-167">Lehet, hogy adja meg például a "master" ág.</span><span class="sxs-lookup"><span data-stu-id="55174-167">Perhaps you should specify a branch such as 'master'.</span></span>

<span data-ttu-id="55174-168">**OK**: Ez a hiba akkor fordulhat elő, ha nem adja meg egy ágat, ha a git push művelet végrehajtása, és rendelkezik nem hello push.default érték beállítása Git használják.</span><span class="sxs-lookup"><span data-stu-id="55174-168">**Cause**: This error can occur if you do not specify a branch when performing a git push operation, and have not set hello push.default value used by Git.</span></span>

<span data-ttu-id="55174-169">**Megoldási**: hello főághoz megadó hello leküldéses megismételni a műveletet, hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="55174-169">**Resolution**: Perform hello push operation again, specifying hello master branch.</span></span> <span data-ttu-id="55174-170">Példa:</span><span class="sxs-lookup"><span data-stu-id="55174-170">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="55174-171">**Jelenség**: src refspec [branchname] nem felel meg egyik sem.</span><span class="sxs-lookup"><span data-stu-id="55174-171">**Symptom**: src refspec [branchname] does not match any.</span></span>

<span data-ttu-id="55174-172">**OK**: Ez a hiba akkor fordulhat elő, ha toopush tooa fiókirodai hello "azure" távoli a főadatbázison kívül.</span><span class="sxs-lookup"><span data-stu-id="55174-172">**Cause**: This error can occur if you attempt toopush tooa branch other than master on hello 'azure' remote.</span></span>

<span data-ttu-id="55174-173">**Megoldási**: hello főághoz megadó hello leküldéses megismételni a műveletet, hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="55174-173">**Resolution**: Perform hello push operation again, specifying hello master branch.</span></span> <span data-ttu-id="55174-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="55174-174">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="55174-175">**Jelenség**: RPC sikertelen; a eredménye = 22, HTTP-kód = 502-es.</span><span class="sxs-lookup"><span data-stu-id="55174-175">**Symptom**: RPC failed; result=22, HTTP code = 502.</span></span>

<span data-ttu-id="55174-176">**OK**: A hiba akkor fordulhat elő, ha toopush egy nagy git-tárház HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="55174-176">**Cause**: This error can occur if you attempt toopush a large git repository over HTTPS.</span></span>

<span data-ttu-id="55174-177">**Megoldási**: hello hello helyi számítógép toomake hello postBuffer nagyobb a git konfigurációjának módosítása</span><span class="sxs-lookup"><span data-stu-id="55174-177">**Resolution**: Change hello git configuration on hello local machine toomake hello postBuffer bigger</span></span>

```bash  
git config --global http.postBuffer 524288000
```
- - -
<span data-ttu-id="55174-178">**Jelenség**: hiba - módosítások véglegesítése tooremote tárház azonban a webalkalmazás nem frissül.</span><span class="sxs-lookup"><span data-stu-id="55174-178">**Symptom**: Error - Changes committed tooremote repository but your web app not updated.</span></span>

<span data-ttu-id="55174-179">**OK**: Ez a hiba akkor fordulhat elő, ha telepít egy Node.js-alkalmazás, amely meghatározza a további szükséges modulokat package.json fájlt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="55174-179">**Cause**: This error can occur if you are deploying a Node.js app containing a package.json file that specifies additional required modules.</span></span>

<span data-ttu-id="55174-180">**Megoldási**: további üzeneteket tartalmazó "npm hiba!"</span><span class="sxs-lookup"><span data-stu-id="55174-180">**Resolution**: Additional messages containing 'npm ERR!'</span></span> <span data-ttu-id="55174-181">a korábbi naplózott toothis hiba legyen, és képes szolgáltatáskészletének hello hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="55174-181">should be logged prior toothis error, and can provide additional context on hello failure.</span></span> <span data-ttu-id="55174-182">hello következő a hiba oka ismert, és megfelelő "npm hiba!" hello</span><span class="sxs-lookup"><span data-stu-id="55174-182">hello following are known causes of this error and hello corresponding 'npm ERR!'</span></span> <span data-ttu-id="55174-183">üzenet:</span><span class="sxs-lookup"><span data-stu-id="55174-183">message:</span></span>

* <span data-ttu-id="55174-184">**Nem megfelelően formázott package.json fájl**: npm hiba!</span><span class="sxs-lookup"><span data-stu-id="55174-184">**Malformed package.json file**: npm ERR!</span></span> <span data-ttu-id="55174-185">Nem lehetett olvasni a függőségek.</span><span class="sxs-lookup"><span data-stu-id="55174-185">Couldn't read dependencies.</span></span>
* <span data-ttu-id="55174-186">**Natív modul, amely nem rendelkezik Windows bináris terjesztési**:</span><span class="sxs-lookup"><span data-stu-id="55174-186">**Native module that does not have a binary distribution for Windows**:</span></span>
  
  * <span data-ttu-id="55174-187">npm hiba!</span><span class="sxs-lookup"><span data-stu-id="55174-187">npm ERR!</span></span> <span data-ttu-id="55174-188">\`cmd "/ c" "csomópont-gyp rebuild"\` sikertelen 1</span><span class="sxs-lookup"><span data-stu-id="55174-188">\`cmd "/c" "node-gyp rebuild"\` failed with 1</span></span>
    
      <span data-ttu-id="55174-189">VAGY</span><span class="sxs-lookup"><span data-stu-id="55174-189">OR</span></span>
  * <span data-ttu-id="55174-190">npm hiba!</span><span class="sxs-lookup"><span data-stu-id="55174-190">npm ERR!</span></span> <span data-ttu-id="55174-191">[modulename@version] preinstall: \`ellenőrizze || gmake\`</span><span class="sxs-lookup"><span data-stu-id="55174-191">[modulename@version] preinstall: \`make || gmake\`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="55174-192">További források</span><span class="sxs-lookup"><span data-stu-id="55174-192">Additional Resources</span></span>
* [<span data-ttu-id="55174-193">A Git dokumentációja</span><span class="sxs-lookup"><span data-stu-id="55174-193">Git documentation</span></span>](http://git-scm.com/documentation)
* [<span data-ttu-id="55174-194">Project Kudu dokumentációja</span><span class="sxs-lookup"><span data-stu-id="55174-194">Project Kudu documentation</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="55174-195">Folyamatos telepítés tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="55174-195">Continous Deployment tooAzure App Service</span></span>](app-service-continuous-deployment.md)
* [<span data-ttu-id="55174-196">Hogyan toouse az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="55174-196">How toouse PowerShell for Azure</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="55174-197">Hogyan toouse hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="55174-197">How toouse hello Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)

[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure parancssori felület]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
