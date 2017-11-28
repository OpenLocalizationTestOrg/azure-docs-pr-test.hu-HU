---
title: "az Azure Functions aaaContinuous telepítési |} Microsoft Docs"
description: "Az Azure App Service toopublish folyamatos üzembe helyezés létesítményekben az Azure Functions használja."
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
ms.openlocfilehash: 28c44f737dad3feab3cf54f7dd42b6a978d0617e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-for-azure-functions"></a><span data-ttu-id="dd8c8-103">Azure Functions – folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="dd8c8-103">Continuous deployment for Azure Functions</span></span>
<span data-ttu-id="dd8c8-104">Az Azure Functions segítségével könnyen toodeploy az App Service folyamatos integrációt használó függvény alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-104">Azure Functions makes it easy toodeploy your function app using App Service continuous integration.</span></span> <span data-ttu-id="dd8c8-105">Funkciók integrálható a Bitbucketből, a dropbox-ba, a GitHub és a Visual Studio Team Services (VSTS).</span><span class="sxs-lookup"><span data-stu-id="dd8c8-105">Functions integrates with BitBucket, Dropbox, GitHub, and Visual Studio Team Services (VSTS).</span></span> <span data-ttu-id="dd8c8-106">Ez lehetővé teszi egy munkafolyamatot, ha frissíti a funkciókódot ezek integrált szolgáltatások eseményindító telepítési tooAzure használatával végrehajtott.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-106">This enables a workflow where function code updates made by using one of these integrated services trigger deployment tooAzure.</span></span> <span data-ttu-id="dd8c8-107">Ha új tooAzure funkciók, kezdődnie [Azure Functions áttekintése](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dd8c8-107">If you are new tooAzure Functions, start with [Azure Functions Overview](functions-overview.md).</span></span>

<span data-ttu-id="dd8c8-108">A folyamatos üzembe helyezés jó megoldás lehet olyan projektek esetén, amelyeknél többszöri és gyakori közreműködői változtatást kell integrálni.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-108">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span> <span data-ttu-id="dd8c8-109">Lehetővé teszi a funkciók kódja verziókezelő karbantartása.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-109">It also lets you maintain source control on your functions code.</span></span> <span data-ttu-id="dd8c8-110">a következő központi telepítési források hello jelenleg támogatottak:</span><span class="sxs-lookup"><span data-stu-id="dd8c8-110">hello following deployment sources are currently supported:</span></span>

* [<span data-ttu-id="dd8c8-111">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="dd8c8-111">Bitbucket</span></span>](https://bitbucket.org/)
* [<span data-ttu-id="dd8c8-112">Dropbox-bA</span><span class="sxs-lookup"><span data-stu-id="dd8c8-112">Dropbox</span></span>](https://www.dropbox.com/)
* <span data-ttu-id="dd8c8-113">Külső tárház (Git vagy Mercurial)</span><span class="sxs-lookup"><span data-stu-id="dd8c8-113">External repository (Git or Mercurial)</span></span>
* [<span data-ttu-id="dd8c8-114">Helyi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="dd8c8-114">Git local repository</span></span>](../app-service-web/app-service-deploy-local-git.md)
* [<span data-ttu-id="dd8c8-115">GitHub</span><span class="sxs-lookup"><span data-stu-id="dd8c8-115">GitHub</span></span>](https://github.com)
* [<span data-ttu-id="dd8c8-116">Onedrive vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="dd8c8-116">OneDrive</span></span>](https://onedrive.live.com/)
* [<span data-ttu-id="dd8c8-117">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="dd8c8-117">Visual Studio Team Services</span></span>](https://www.visualstudio.com/team-services/)

<span data-ttu-id="dd8c8-118">Központi telepítések függvény alkalmazás szinten vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-118">Deployments are configured on a per-function app basis.</span></span> <span data-ttu-id="dd8c8-119">Folyamatos üzembe helyezés engedélyezése után túl van-e állítva a hozzáférési toofunction kód hello portálon*írásvédett*.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-119">After continuous deployment is enabled, access toofunction code in hello portal is set too*read-only*.</span></span>

## <a name="continuous-deployment-requirements"></a><span data-ttu-id="dd8c8-120">Folyamatos üzembe helyezés feltételeit</span><span class="sxs-lookup"><span data-stu-id="dd8c8-120">Continuous deployment requirements</span></span>

<span data-ttu-id="dd8c8-121">A konfigurálva telepítési forrás és a funkciók kódot hello központi telepítés forrásának beállítása a folyamatos üzembe helyezés előtt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-121">You must have your deployment source configured and your functions code in hello deployment source before you set up continuous deployment.</span></span> <span data-ttu-id="dd8c8-122">Egy adott funkció alkalmazások központi telepítésének egyes függvény él, egy elnevezett alkönyvtárra, ahol hello könyvtárnév az hello hello függvény neve.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-122">In a given function app deployment, each function lives in a named subdirectory, where hello directory name is hello name of hello function.</span></span>  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a><span data-ttu-id="dd8c8-123">Folyamatos üzembe helyezés beállítása</span><span class="sxs-lookup"><span data-stu-id="dd8c8-123">Set up continuous deployment</span></span>
<span data-ttu-id="dd8c8-124">Ez az eljárás tooconfigure folyamatos üzembe helyezés használjon egy meglévő függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-124">Use this procedure tooconfigure continuous deployment for an existing function app.</span></span> <span data-ttu-id="dd8c8-125">Ezeket a lépéseket egy GitHub-tárházban az integráció bemutatásához, de hasonló lépésekkel a Visual Studio Team Services vagy egyéb telepítési érvényesek.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-125">These steps demonstrate integration with a GitHub repository, but similar steps apply for Visual Studio Team Services or other deployment services.</span></span>

1. <span data-ttu-id="dd8c8-126">Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **központi telepítési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-126">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="dd8c8-128">Ezt a hello **központi telepítések** panelen kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-128">Then in hello **Deployments** blade click **Setup**.</span></span>
 
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="dd8c8-130">A hello **központi telepítés forrásának** panelen kattintson a **forrás választása**, majd adja meg a választott központi telepítési forrás hello adatait, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-130">In hello **Deployment source** blade, click **Choose source**, then fill in hello information for your chosen deployment source and click **OK**.</span></span>
   
    ![Válassza ki a központi telepítés forrása](./media/functions-continuous-deployment/choose-deployment-source.png)

<span data-ttu-id="dd8c8-132">Folyamatos üzembe helyezés beállítása után a központi telepítés forrásának összes fájl módosításának másolt toohello függvény alkalmazást, és akkor váltódik ki, a teljes helyen történő központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-132">After continuous deployment is configured, all file changes in your deployment source are copied toohello function app and a full site deployment is triggered.</span></span> <span data-ttu-id="dd8c8-133">hello hely van újratelepítése hello fájljai frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-133">hello site is redeployed when files in hello source are updated.</span></span>

## <a name="deployment-options"></a><span data-ttu-id="dd8c8-134">Üzembe helyezési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="dd8c8-134">Deployment options</span></span>

<span data-ttu-id="dd8c8-135">hello az alábbiakban néhány tipikus telepítési forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="dd8c8-135">hello following are some typical deployment scenarios:</span></span>

- [<span data-ttu-id="dd8c8-136">Átmeneti központi telepítés</span><span class="sxs-lookup"><span data-stu-id="dd8c8-136">Create a staging deployment</span></span>](#staging)
- [<span data-ttu-id="dd8c8-137">Helyezze át a meglévő funkciók toocontinuous telepítés</span><span class="sxs-lookup"><span data-stu-id="dd8c8-137">Move existing functions toocontinuous deployment</span></span>](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a><span data-ttu-id="dd8c8-138">Átmeneti központi telepítés</span><span class="sxs-lookup"><span data-stu-id="dd8c8-138">Create a staging deployment</span></span>

<span data-ttu-id="dd8c8-139">Alkalmazások funkció még nem támogatja a üzembe helyezési pontok.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-139">Function Apps doesn't yet support deployment slots.</span></span> <span data-ttu-id="dd8c8-140">Folyamatos integrációt használatával azonban külön átmeneti és üzemi központi telepítések továbbra is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-140">However, you can still manage separate staging and production deployments by using continuous integration.</span></span>

<span data-ttu-id="dd8c8-141">folyamat tooconfigure hello és az átmeneti központi telepítés használata általában ilyen formátumú:</span><span class="sxs-lookup"><span data-stu-id="dd8c8-141">hello process tooconfigure and work with a staging deployment looks generally like this:</span></span>

1. <span data-ttu-id="dd8c8-142">Az előfizetés, hello éles kódot és egy az átmeneti két függvény-alkalmazásai létrehozására.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-142">Create two function apps in your subscription, one for hello production code and one for staging.</span></span> 

2. <span data-ttu-id="dd8c8-143">Hozzon létre olyan telepítési forrás, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-143">Create a deployment source, if you don't already have one.</span></span> <span data-ttu-id="dd8c8-144">Ez a példa [GitHub].</span><span class="sxs-lookup"><span data-stu-id="dd8c8-144">This example uses [GitHub].</span></span>

3. <span data-ttu-id="dd8c8-145">Az éles függvény alkalmazás előző teljes hello lépései **folyamatos üzembe helyezés beállítása** és set hello telepítési fiókirodai toohello főágába a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-145">For your production function app, complete hello preceding steps in **Set up continuous deployment** and set hello deployment branch toohello master branch of your GitHub repository.</span></span>
   
    ![Válassza ki a központi telepítés ág](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. <span data-ttu-id="dd8c8-147">Átmeneti függvény app hello ismételje meg ezt a lépést, de ehelyett staging ágat a GitHub-tárház hello válassza.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-147">Repeat this step for hello staging function app, but choose hello staging branch instead in your GitHub repo.</span></span> <span data-ttu-id="dd8c8-148">Ha a központi telepítési forrás nem támogatja a ugorhat, használjon egy másik mappába.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-148">If your deployment source doesn't support branching, use a different folder.</span></span>
    
5. <span data-ttu-id="dd8c8-149">Elágazás, illetve mappa átmeneti hello frissítések tooyour kód készítsen, majd győződjön meg arról, hogy ezek a módosítások megjelennek-e telepítési átmeneti hello.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-149">Make updates tooyour code in hello staging branch or folder, then verify that those changes are reflected in hello staging deployment.</span></span>

6. <span data-ttu-id="dd8c8-150">Tesztelés után lévő módosítások hello átmeneti fiókirodai hello főágba történő.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-150">After testing, merge changes from hello staging branch into hello master branch.</span></span> <span data-ttu-id="dd8c8-151">Az egyesítési eseményindítókat telepítési toohello éles függvény alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-151">This merge triggers deployment toohello production function app.</span></span> <span data-ttu-id="dd8c8-152">Ha a központi telepítési forrás nem támogatja a elágazásokat, felülírni a hello fájlokat hello éles mappában átmeneti mappája hello hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-152">If your deployment source doesn't support branches, overwrite hello files in hello production folder with hello files from hello staging folder.</span></span>

<a name="existing"></a>
### <a name="move-existing-functions-toocontinuous-deployment"></a><span data-ttu-id="dd8c8-153">Helyezze át a meglévő funkciók toocontinuous telepítés</span><span class="sxs-lookup"><span data-stu-id="dd8c8-153">Move existing functions toocontinuous deployment</span></span>
<span data-ttu-id="dd8c8-154">Ha meglévő függvények, amelyek létrehozott és karbantartott hello portálon toodownload van szüksége van a meglévő működik kódfájlok FTP vagy hello helyi Git-tárház használata előtt állíthat be a folyamatos üzembe helyezés fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-154">When you have existing functions that you have created and maintained in hello portal, you need toodownload your existing function code files using FTP or hello local Git repository before you can set up continuous deployment as described above.</span></span> <span data-ttu-id="dd8c8-155">Ehhez a hello App Service-beállítások az funkciót alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-155">You can do this in hello App Service settings for your function app.</span></span> <span data-ttu-id="dd8c8-156">A fájlok letöltését követően feltöltheti azokat a kiválasztott tooyour folyamatos üzembe helyezés forrás.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-156">After your files are downloaded, you can upload them tooyour chosen continuous deployment source.</span></span>

> [!NOTE]
> <span data-ttu-id="dd8c8-157">Miután konfigurálta a folyamatos integrációt, már nem lesz képes tooedit a forrásfájlok hello funkciók portálon.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-157">After you configure continuous integration, you will no longer be able tooedit your source files in hello Functions portal.</span></span>

- [<span data-ttu-id="dd8c8-158">Hogyan: üzembe helyezési hitelesítő adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dd8c8-158">How to: Configure deployment credentials</span></span>](#credentials)
- [<span data-ttu-id="dd8c8-159">Útmutató: FTP használata fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="dd8c8-159">How to: Download files using FTP</span></span>](#downftp)
- [<span data-ttu-id="dd8c8-160">Hogyan: Töltse le a fájlokat a hello helyi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="dd8c8-160">How to: Download files using hello local Git repository</span></span>](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a><span data-ttu-id="dd8c8-161">Hogyan: üzembe helyezési hitelesítő adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dd8c8-161">How to: Configure deployment credentials</span></span>
<span data-ttu-id="dd8c8-162">A függvény alkalmazás FTP-vagy helyi Git-tárház letöltheti a fájlokat, konfigurálnia kell a hitelesítő adatok tooaccess hello helyet.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-162">Before you can download files from your function app with FTP or local Git repository, you must configure your credentials tooaccess hello site.</span></span> <span data-ttu-id="dd8c8-163">Hitelesítő adatok beállítása hello függvény alkalmazási szintű.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-163">Credentials are set at hello Function app level.</span></span> <span data-ttu-id="dd8c8-164">A következő lépéseket tooset üzembe helyezési hitelesítő adatokat az Azure-portálon hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="dd8c8-164">Use hello following steps tooset deployment credentials in hello Azure portal:</span></span>

1. <span data-ttu-id="dd8c8-165">Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-165">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment credentials**.</span></span>
   
    ![Helyi telepítési hitelesítő adatok beállítása](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. <span data-ttu-id="dd8c8-167">Írja be a felhasználónevet és jelszót, majd kattintson az **mentése**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-167">Type in a username and password, then click **Save**.</span></span> <span data-ttu-id="dd8c8-168">Most már használhatja a hitelesítő adatok tooaccess az FTP- vagy hello beépített Git-tárház függvény alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-168">You can now use these credentials tooaccess your function app from FTP or hello built-in Git repo.</span></span>

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a><span data-ttu-id="dd8c8-169">Útmutató: FTP használata fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="dd8c8-169">How to: Download files using FTP</span></span>

1. <span data-ttu-id="dd8c8-170">Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **tulajdonságok**, hello értékeit másolja **FTPvagyüzembehelyezőfelhasználó**, **FTP állomásnév**, és **FTPS állomásnév**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-170">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Properties**, then copy hello values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span>  

    <span data-ttu-id="dd8c8-171">**FTP-vagy üzembe helyező felhasználó** hello portálon, beleértve a hello alkalmazás neve, tooprovide megfelelő környezet hello FTP-kiszolgáló jelenik meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-171">**FTP/Deployment User** must be entered as displayed in hello portal, including hello app name, tooprovide proper context for hello FTP server.</span></span>
   
    ![A központi telepítési információk](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. <span data-ttu-id="dd8c8-173">Az FTP-programból tooconnect tooyour app összegyűjtött információkat felhasználva hello kapcsolat, és a függvények hello forrásfájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-173">From your FTP client, use hello connection information you gathered tooconnect tooyour app and download hello source files for your functions.</span></span>

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a><span data-ttu-id="dd8c8-174">Hogyan: Töltse le a fájlokat a helyi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="dd8c8-174">How to: Download files using a local Git repository</span></span>

1. <span data-ttu-id="dd8c8-175">Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **központi telepítési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-175">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="dd8c8-177">Ezt a hello **központi telepítések** panelen kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-177">Then in hello **Deployments** blade click **Setup**.</span></span>
 
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="dd8c8-179">A hello **központi telepítés forrásának** panelen kattintson **helyi Git-tárház** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-179">In hello **Deployment source** blade, click **Local Git repository** and then click **OK**.</span></span>

3. <span data-ttu-id="dd8c8-180">A **Platform funkciói**, kattintson a **tulajdonságok** és jegyezze fel a Git URL-cím hello értékét.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-180">In **Platform features**, click **Properties** and note hello value of Git URL.</span></span> 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. <span data-ttu-id="dd8c8-182">A Git-kompatibilis parancssor vagy a kedvenc Git használatával, a helyi gépen hello tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="dd8c8-182">Clone hello repository on your local machine using a Git-aware command prompt or your favorite Git tool.</span></span> <span data-ttu-id="dd8c8-183">hello Git-klón parancs így néz ki:</span><span class="sxs-lookup"><span data-stu-id="dd8c8-183">hello Git clone command looks like this:</span></span>
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. <span data-ttu-id="dd8c8-184">A függvény app toohello Klónozás fájlok beolvasni a helyi számítógépen, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="dd8c8-184">Fetch files from your function app toohello clone on your local computer, as in hello following example:</span></span>
   
        git pull origin master
   
    <span data-ttu-id="dd8c8-185">Ha szükséges, adja meg a [üzembe helyezési hitelesítő adatok konfigurált](#credentials).</span><span class="sxs-lookup"><span data-stu-id="dd8c8-185">If requested, supply your [configured deployment credentials](#credentials).</span></span>  

[GitHub]: https://github.com/
