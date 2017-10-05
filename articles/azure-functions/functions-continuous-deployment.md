---
title: "Folyamatos üzembe helyezés az Azure Functions |} Microsoft Docs"
description: "Folyamatos üzembe helyezés létesítményekben az Azure App Service segítségével az Azure Functions közzététele."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 3756f1a039730bfd99b0375ce9bfeaf27178f2e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-deployment-for-azure-functions"></a><span data-ttu-id="1bbc7-103">Azure Functions – folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="1bbc7-103">Continuous deployment for Azure Functions</span></span>
<span data-ttu-id="1bbc7-104">Az Azure Functions megkönnyíti az App Service folyamatos integrációt használó függvény alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-104">Azure Functions makes it easy to deploy your function app using App Service continuous integration.</span></span> <span data-ttu-id="1bbc7-105">Funkciók integrálható a Bitbucketből, a dropbox-ba, a GitHub és a Visual Studio Team Services (VSTS).</span><span class="sxs-lookup"><span data-stu-id="1bbc7-105">Functions integrates with BitBucket, Dropbox, GitHub, and Visual Studio Team Services (VSTS).</span></span> <span data-ttu-id="1bbc7-106">Ez lehetővé teszi egy munkafolyamatot, ha frissíti a funkciókódot ezek integrált szolgáltatások eseményindító telepítése az Azure használatával végrehajtott.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-106">This enables a workflow where function code updates made by using one of these integrated services trigger deployment to Azure.</span></span> <span data-ttu-id="1bbc7-107">Ha most ismerkedik az Azure Functions, kezdje [Azure Functions áttekintése](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1bbc7-107">If you are new to Azure Functions, start with [Azure Functions Overview](functions-overview.md).</span></span>

<span data-ttu-id="1bbc7-108">A folyamatos üzembe helyezés jó megoldás lehet olyan projektek esetén, amelyeknél többszöri és gyakori közreműködői változtatást kell integrálni.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-108">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span> <span data-ttu-id="1bbc7-109">Lehetővé teszi a funkciók kódja verziókezelő karbantartása.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-109">It also lets you maintain source control on your functions code.</span></span> <span data-ttu-id="1bbc7-110">A következő központi telepítési források jelenleg támogatottak:</span><span class="sxs-lookup"><span data-stu-id="1bbc7-110">The following deployment sources are currently supported:</span></span>

* [<span data-ttu-id="1bbc7-111">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="1bbc7-111">Bitbucket</span></span>](https://bitbucket.org/)
* [<span data-ttu-id="1bbc7-112">Dropbox-bA</span><span class="sxs-lookup"><span data-stu-id="1bbc7-112">Dropbox</span></span>](https://www.dropbox.com/)
* <span data-ttu-id="1bbc7-113">Külső tárház (Git vagy Mercurial)</span><span class="sxs-lookup"><span data-stu-id="1bbc7-113">External repository (Git or Mercurial)</span></span>
* [<span data-ttu-id="1bbc7-114">Helyi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="1bbc7-114">Git local repository</span></span>](../app-service-web/app-service-deploy-local-git.md)
* [<span data-ttu-id="1bbc7-115">GitHub</span><span class="sxs-lookup"><span data-stu-id="1bbc7-115">GitHub</span></span>](https://github.com)
* [<span data-ttu-id="1bbc7-116">Onedrive vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="1bbc7-116">OneDrive</span></span>](https://onedrive.live.com/)
* [<span data-ttu-id="1bbc7-117">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="1bbc7-117">Visual Studio Team Services</span></span>](https://www.visualstudio.com/team-services/)

<span data-ttu-id="1bbc7-118">Központi telepítések függvény alkalmazás szinten vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-118">Deployments are configured on a per-function app basis.</span></span> <span data-ttu-id="1bbc7-119">Folyamatos üzembe helyezés engedélyezése után a portálon funkciókódot elérésére értéke *írásvédett*.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-119">After continuous deployment is enabled, access to function code in the portal is set to *read-only*.</span></span>

## <a name="continuous-deployment-requirements"></a><span data-ttu-id="1bbc7-120">Folyamatos üzembe helyezés feltételeit</span><span class="sxs-lookup"><span data-stu-id="1bbc7-120">Continuous deployment requirements</span></span>

<span data-ttu-id="1bbc7-121">A központi telepítés forrásának beállítása a folyamatos üzembe helyezés előtt a konfigurálva telepítési forrás és a funkciók kódot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-121">You must have your deployment source configured and your functions code in the deployment source before you set up continuous deployment.</span></span> <span data-ttu-id="1bbc7-122">Egy adott funkció alkalmazások központi telepítésének egyes függvény él, egy elnevezett alkönyvtárra, ahol a könyvtárnév pedig a függvény nevét.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-122">In a given function app deployment, each function lives in a named subdirectory, where the directory name is the name of the function.</span></span>  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a><span data-ttu-id="1bbc7-123">Folyamatos üzembe helyezés beállítása</span><span class="sxs-lookup"><span data-stu-id="1bbc7-123">Set up continuous deployment</span></span>
<span data-ttu-id="1bbc7-124">Ezzel az eljárással konfigurálhatja folyamatos üzembe egy meglévő függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-124">Use this procedure to configure continuous deployment for an existing function app.</span></span> <span data-ttu-id="1bbc7-125">Ezeket a lépéseket egy GitHub-tárházban az integráció bemutatásához, de hasonló lépésekkel a Visual Studio Team Services vagy egyéb telepítési érvényesek.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-125">These steps demonstrate integration with a GitHub repository, but similar steps apply for Visual Studio Team Services or other deployment services.</span></span>

1. <span data-ttu-id="1bbc7-126">Az alkalmazás a függvény a [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **központi telepítési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-126">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="1bbc7-128">Ezt a a **központi telepítések** panelen kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-128">Then in the **Deployments** blade click **Setup**.</span></span>
 
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="1bbc7-130">Az a **központi telepítés forrásának** panelen kattintson a **forrás választása**, majd adja meg a kiválasztott központi telepítés forrásának adatait, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-130">In the **Deployment source** blade, click **Choose source**, then fill in the information for your chosen deployment source and click **OK**.</span></span>
   
    ![Válassza ki a központi telepítés forrása](./media/functions-continuous-deployment/choose-deployment-source.png)

<span data-ttu-id="1bbc7-132">Folyamatos üzembe helyezés beállítása után a központi telepítés forrásának összes fájl módosításának kerülnek a függvény alkalmazást, és akkor váltódik ki, a teljes helyen történő központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-132">After continuous deployment is configured, all file changes in your deployment source are copied to the function app and a full site deployment is triggered.</span></span> <span data-ttu-id="1bbc7-133">A hely frissítésekor a fájljai van újratelepítése.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-133">The site is redeployed when files in the source are updated.</span></span>

## <a name="deployment-options"></a><span data-ttu-id="1bbc7-134">Üzembe helyezési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="1bbc7-134">Deployment options</span></span>

<span data-ttu-id="1bbc7-135">Az alábbiakban néhány tipikus telepítési forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="1bbc7-135">The following are some typical deployment scenarios:</span></span>

- [<span data-ttu-id="1bbc7-136">Átmeneti központi telepítés</span><span class="sxs-lookup"><span data-stu-id="1bbc7-136">Create a staging deployment</span></span>](#staging)
- [<span data-ttu-id="1bbc7-137">Helyezze át a meglévő funkciók a folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="1bbc7-137">Move existing functions to continuous deployment</span></span>](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a><span data-ttu-id="1bbc7-138">Átmeneti központi telepítés</span><span class="sxs-lookup"><span data-stu-id="1bbc7-138">Create a staging deployment</span></span>

<span data-ttu-id="1bbc7-139">Alkalmazások funkció még nem támogatja a üzembe helyezési pontok.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-139">Function Apps doesn't yet support deployment slots.</span></span> <span data-ttu-id="1bbc7-140">Folyamatos integrációt használatával azonban külön átmeneti és üzemi központi telepítések továbbra is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-140">However, you can still manage separate staging and production deployments by using continuous integration.</span></span>

<span data-ttu-id="1bbc7-141">A folyamat konfigurálását és használatát az átmeneti telepítést általában formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="1bbc7-141">The process to configure and work with a staging deployment looks generally like this:</span></span>

1. <span data-ttu-id="1bbc7-142">Az előfizetés, az éles kódot és egy az átmeneti két függvény-alkalmazásai létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-142">Create two function apps in your subscription, one for the production code and one for staging.</span></span> 

2. <span data-ttu-id="1bbc7-143">Hozzon létre olyan telepítési forrás, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-143">Create a deployment source, if you don't already have one.</span></span> <span data-ttu-id="1bbc7-144">Ez a példa [GitHub].</span><span class="sxs-lookup"><span data-stu-id="1bbc7-144">This example uses [GitHub].</span></span>

3. <span data-ttu-id="1bbc7-145">Az éles függvény alkalmazás teljes a fenti lépéseket **folyamatos üzembe helyezés beállítása** , és a központi telepítés ágat a GitHub-tárház főágába.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-145">For your production function app, complete the preceding steps in **Set up continuous deployment** and set the deployment branch to the master branch of your GitHub repository.</span></span>
   
    ![Válassza ki a központi telepítés ág](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. <span data-ttu-id="1bbc7-147">Ismételje meg ezt a lépést az átmeneti tárolási függvény alkalmazás, de ehelyett válassza ki a átmeneti ágat a GitHub-tárház.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-147">Repeat this step for the staging function app, but choose the staging branch instead in your GitHub repo.</span></span> <span data-ttu-id="1bbc7-148">Ha a központi telepítési forrás nem támogatja a ugorhat, használjon egy másik mappába.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-148">If your deployment source doesn't support branching, use a different folder.</span></span>
    
5. <span data-ttu-id="1bbc7-149">Hogy a kód a átmeneti fiókirodai vagy mappát, majd győződjön meg arról, hogy ezek a módosítások megjelennek-e a átmeneti központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-149">Make updates to your code in the staging branch or folder, then verify that those changes are reflected in the staging deployment.</span></span>

6. <span data-ttu-id="1bbc7-150">Tesztelés után lévő módosítások átmeneti ágat a főágba történő.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-150">After testing, merge changes from the staging branch into the master branch.</span></span> <span data-ttu-id="1bbc7-151">A merge elindítja a központi telepítést, hogy az éles függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-151">This merge triggers deployment to the production function app.</span></span> <span data-ttu-id="1bbc7-152">Ha a központi telepítési forrás nem támogatja az ágakkal rendelkező, a termelési mappában lévő fájlok felülírása az átmeneti mappából a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-152">If your deployment source doesn't support branches, overwrite the files in the production folder with the files from the staging folder.</span></span>

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a><span data-ttu-id="1bbc7-153">Helyezze át a meglévő funkciók a folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="1bbc7-153">Move existing functions to continuous deployment</span></span>
<span data-ttu-id="1bbc7-154">Ha rendelkezik meglévő függvények, amelyek létrehozása és karbantartása a portálon, le kell töltenie a meglévő függvény kódfájlok FTP használatával, vagy a helyi Git-tárház előtt állíthat be a folyamatos üzembe helyezés fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-154">When you have existing functions that you have created and maintained in the portal, you need to download your existing function code files using FTP or the local Git repository before you can set up continuous deployment as described above.</span></span> <span data-ttu-id="1bbc7-155">Ehhez az App Service-beállítások az funkciót alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-155">You can do this in the App Service settings for your function app.</span></span> <span data-ttu-id="1bbc7-156">A fájlok letöltését követően töltse fel őket a kiválasztott folyamatos üzembe helyezés forráshoz.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-156">After your files are downloaded, you can upload them to your chosen continuous deployment source.</span></span>

> [!NOTE]
> <span data-ttu-id="1bbc7-157">Miután konfigurálta a folyamatos integrációt, már nem lesz a forrásfájlok a Functions portálon szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-157">After you configure continuous integration, you will no longer be able to edit your source files in the Functions portal.</span></span>

- [<span data-ttu-id="1bbc7-158">Hogyan: üzembe helyezési hitelesítő adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1bbc7-158">How to: Configure deployment credentials</span></span>](#credentials)
- [<span data-ttu-id="1bbc7-159">Útmutató: FTP használata fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="1bbc7-159">How to: Download files using FTP</span></span>](#downftp)
- [<span data-ttu-id="1bbc7-160">Útmutató: a helyi Git-tárház fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="1bbc7-160">How to: Download files using the local Git repository</span></span>](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a><span data-ttu-id="1bbc7-161">Hogyan: üzembe helyezési hitelesítő adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1bbc7-161">How to: Configure deployment credentials</span></span>
<span data-ttu-id="1bbc7-162">A függvény alkalmazás FTP-vagy helyi Git-tárház letöltheti a fájlokat, konfigurálnia kell a hitelesítő adatok a webhelyhez való hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-162">Before you can download files from your function app with FTP or local Git repository, you must configure your credentials to access the site.</span></span> <span data-ttu-id="1bbc7-163">A függvény alkalmazási szintű hitelesítő adatok beállítása.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-163">Credentials are set at the Function app level.</span></span> <span data-ttu-id="1bbc7-164">Az alábbi lépések segítségével üzembe helyezési hitelesítő adatok beállítása az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="1bbc7-164">Use the following steps to set deployment credentials in the Azure portal:</span></span>

1. <span data-ttu-id="1bbc7-165">Az alkalmazás a függvény a [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-165">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment credentials**.</span></span>
   
    ![Helyi telepítési hitelesítő adatok beállítása](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. <span data-ttu-id="1bbc7-167">Írja be a felhasználónevet és jelszót, majd kattintson az **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-167">Type in a username and password, then click **Save**.</span></span> <span data-ttu-id="1bbc7-168">A függvény app hozzáférni az FTP- vagy a beépített Git-tárház használhatja ezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-168">You can now use these credentials to access your function app from FTP or the built-in Git repo.</span></span>

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a><span data-ttu-id="1bbc7-169">Útmutató: FTP használata fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="1bbc7-169">How to: Download files using FTP</span></span>

1. <span data-ttu-id="1bbc7-170">Az alkalmazás a függvény a [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **tulajdonságok**, majd másolja a **FTP vagy üzembe helyező felhasználó**, **FTP állomásnév**, és **FTPS állomásnév**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-170">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Properties**, then copy the values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span>  

    <span data-ttu-id="1bbc7-171">**FTP-vagy üzembe helyező felhasználó** meg kell adni, megjelenik a portálon, beleértve az alkalmazásnév, megfelelő környezetet biztosít az FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-171">**FTP/Deployment User** must be entered as displayed in the portal, including the app name, to provide proper context for the FTP server.</span></span>
   
    ![A központi telepítési információk](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. <span data-ttu-id="1bbc7-173">Az FTP-ügyfél, használja a kapcsolati adatok összegyűjtött való csatlakozáshoz az alkalmazáshoz és a funkciók számára a forrásfájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-173">From your FTP client, use the connection information you gathered to connect to your app and download the source files for your functions.</span></span>

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a><span data-ttu-id="1bbc7-174">Hogyan: Töltse le a fájlokat a helyi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="1bbc7-174">How to: Download files using a local Git repository</span></span>

1. <span data-ttu-id="1bbc7-175">Az alkalmazás a függvény a [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **központi telepítési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-175">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="1bbc7-177">Ezt a a **központi telepítések** panelen kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-177">Then in the **Deployments** blade click **Setup**.</span></span>
 
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="1bbc7-179">Az a **központi telepítés forrásának** panelen kattintson a **helyi Git-tárház** , majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-179">In the **Deployment source** blade, click **Local Git repository** and then click **OK**.</span></span>

3. <span data-ttu-id="1bbc7-180">A **Platform funkciói**, kattintson a **tulajdonságok** és Git URL-cím értékét.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-180">In **Platform features**, click **Properties** and note the value of Git URL.</span></span> 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. <span data-ttu-id="1bbc7-182">Klónozza a tárházat a helyi számítógépen a Git-kompatibilis parancssor vagy a kedvenc Git eszköz használatával.</span><span class="sxs-lookup"><span data-stu-id="1bbc7-182">Clone the repository on your local machine using a Git-aware command prompt or your favorite Git tool.</span></span> <span data-ttu-id="1bbc7-183">A Git-klón parancs így néz ki:</span><span class="sxs-lookup"><span data-stu-id="1bbc7-183">The Git clone command looks like this:</span></span>
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. <span data-ttu-id="1bbc7-184">Lehívási fájlok a függvény alkalmazás a klón a helyi számítógépen, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="1bbc7-184">Fetch files from your function app to the clone on your local computer, as in the following example:</span></span>
   
        git pull origin master
   
    <span data-ttu-id="1bbc7-185">Ha szükséges, adja meg a [üzembe helyezési hitelesítő adatok konfigurált](#credentials).</span><span class="sxs-lookup"><span data-stu-id="1bbc7-185">If requested, supply your [configured deployment credentials](#credentials).</span></span>  

<span data-ttu-id="1bbc7-186">[GitHub]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="1bbc7-186">[GitHub]: https://github.com/</span></span>
