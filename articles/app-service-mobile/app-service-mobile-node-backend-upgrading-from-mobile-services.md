---
title: a Mobile Services tooAzure App Service - Node.js aaaUpgrade
description: "Ismerje meg, hogyan tooeasily frissítése a Mobile Services alkalmazás tooan App Service Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 722cda244d4f633247827f58ea6f1397137ea600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-tooapp-service"></a><span data-ttu-id="fa538-103">A meglévő Node.js Azure Mobile Services mobilszolgáltatás tooApp szolgáltatás frissítése</span><span class="sxs-lookup"><span data-stu-id="fa538-103">Upgrade your existing Node.js Azure Mobile Service tooApp Service</span></span>
<span data-ttu-id="fa538-104">App Service Mobile egy új módon toobuild alkalmazásokat a Microsoft Azure használatával.</span><span class="sxs-lookup"><span data-stu-id="fa538-104">App Service Mobile is a new way toobuild mobile applications using Microsoft Azure.</span></span> <span data-ttu-id="fa538-105">több, lásd: toolearn [Mik azok a Mobile Apps?].</span><span class="sxs-lookup"><span data-stu-id="fa538-105">toolearn more, see [What are Mobile Apps?].</span></span>

<span data-ttu-id="fa538-106">Ez a témakör ismerteti, hogyan tooupgrade egy meglévő Node.js háttérrendszer alkalmazást az Azure Mobile Services tooa új App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="fa538-106">This topic describes how tooupgrade an existing Node.js backend application from Azure Mobile Services tooa new App Service Mobile Apps.</span></span> <span data-ttu-id="fa538-107">Ez a frissítés végrehajtása során a meglévő Mobile Services-alkalmazás tovább toooperate.</span><span class="sxs-lookup"><span data-stu-id="fa538-107">While you perform this upgrade, your existing Mobile Services application can continue toooperate.</span></span>  <span data-ttu-id="fa538-108">Ha egy Node.js a háttéralkalmazás tooupgrade van szüksége, tekintse meg a túl[frissítése a .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).</span><span class="sxs-lookup"><span data-stu-id="fa538-108">If you need tooupgrade a Node.js backend application, refer too[Upgrading your .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).</span></span>

<span data-ttu-id="fa538-109">Ha mobil-háttéralkalmazást frissített tooAzure App Service, rendelkezik-e hozzáférési tooall App Service-szolgáltatások és a számlázott szerint túl[App Service díjszabás], nem a Mobile Services díjszabása.</span><span class="sxs-lookup"><span data-stu-id="fa538-109">When a mobile backend is upgraded tooAzure App Service, it has access tooall App Service features and are billed according too[App Service pricing], not Mobile Services pricing.</span></span>

## <a name="migrate-vs-upgrade"></a><span data-ttu-id="fa538-110">Frissítése és áttelepítése</span><span class="sxs-lookup"><span data-stu-id="fa538-110">Migrate vs. upgrade</span></span>
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> <span data-ttu-id="fa538-111">Javasoljuk, hogy [áttelepítés](app-service-mobile-migrating-from-mobile-services.md) áthaladás a frissítés előtt.</span><span class="sxs-lookup"><span data-stu-id="fa538-111">It is recommended that you [perform a migration](app-service-mobile-migrating-from-mobile-services.md) before going through an upgrade.</span></span> <span data-ttu-id="fa538-112">Így adhat meg az alkalmazás mindkét verziója azonos App Service-csomag hello és nem jelent többletköltséget fel Önnek.</span><span class="sxs-lookup"><span data-stu-id="fa538-112">This way, you can put both versions of your application on hello same App Service Plan and incur no additional cost.</span></span>
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a><span data-ttu-id="fa538-113">A Mobile Apps Node.js server SDK fejlesztései</span><span class="sxs-lookup"><span data-stu-id="fa538-113">Improvements in Mobile Apps Node.js server SDK</span></span>
<span data-ttu-id="fa538-114">Új toohello frissítése [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) számos javítást tartalmaz, például:</span><span class="sxs-lookup"><span data-stu-id="fa538-114">Upgrading toohello new [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) provides a lot of improvements, including:</span></span>

* <span data-ttu-id="fa538-115">Hello alapján [keretrendszer Express](http://expressjs.com/en/index.html), hello új csomópont SDK nem passzív és a tervezett tookeep mentése új csomópont verziókkal, a származnak. Testre szabhatja az Express közbenső szoftvert hello alkalmazás működését.</span><span class="sxs-lookup"><span data-stu-id="fa538-115">Based on hello [Express framework](http://expressjs.com/en/index.html), hello new Node SDK is light-weight and designed tookeep up with new Node versions as they come out. You can customize hello application behavior with Express middleware.</span></span>
* <span data-ttu-id="fa538-116">Jelentős teljesítményjavulást eredményezhet toohello Mobile Services SDK képest.</span><span class="sxs-lookup"><span data-stu-id="fa538-116">Significant performance improvements compared toohello Mobile Services SDK.</span></span>
* <span data-ttu-id="fa538-117">Üzemeltethető webhely és a mobil-háttéralkalmazást; Hasonlóképpen könnyen tooadd hello Azure Mobile SDK tooany meglévő express.v4 alkalmazás is.</span><span class="sxs-lookup"><span data-stu-id="fa538-117">You can now host a website together with your mobile backend; similarly, it's easy tooadd hello Azure Mobile SDK tooany existing express.v4 application.</span></span>
* <span data-ttu-id="fa538-118">Platformok közötti és a helyi fejlesztési, hello Mobile Apps SDK-t is kell fejlesztett és helyileg Windows, Linux vagy OSX platformokon futtatható.</span><span class="sxs-lookup"><span data-stu-id="fa538-118">Built for cross-platform and local development, hello Mobile Apps SDK can be developed and run locally on Windows, Linux, and OSX platforms.</span></span> <span data-ttu-id="fa538-119">Mostantól egyszerűen toouse közös csomópont fejlesztői módszerek például futó [galuskánál](https://mochajs.org/) előzetes toodeployment teszteli.</span><span class="sxs-lookup"><span data-stu-id="fa538-119">It's now easy toouse common Node development techniques like running [Mocha](https://mochajs.org/) tests prior toodeployment.</span></span>

## <span data-ttu-id="fa538-120"><a name="overview"></a>Alapszintű frissítési áttekintése</span><span class="sxs-lookup"><span data-stu-id="fa538-120"><a name="overview"></a>Basic upgrade overview</span></span>
<span data-ttu-id="fa538-121">a Node.js-háttéralkalmazáshoz, Azure App Service frissítése tooaid nyújtott kompatibilitási csomagot.</span><span class="sxs-lookup"><span data-stu-id="fa538-121">tooaid in upgrading a Node.js backend, Azure App Service has provided a compatibility package.</span></span>  <span data-ttu-id="fa538-122">Frissítés után akkor egy niew helyet, amely a telepített tooa új App Service-helyen lehet.</span><span class="sxs-lookup"><span data-stu-id="fa538-122">After upgrade, you will have a niew site that can be deployed tooa new App Service site.</span></span>

<span data-ttu-id="fa538-123">hello Mobile Services-ügyfél SDK-k vannak **nem** hello új Mobile Apps server SDK kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="fa538-123">hello Mobile Services client SDKs are **not** compatible with hello new Mobile Apps server SDK.</span></span> <span data-ttu-id="fa538-124">A sorrend tooprovide szolgáltatási folytonosság érdekében az alkalmazások nem módosítások tooa webhelyet jelenleg szolgál a közzétett ügyfelek kell közzétenni.</span><span class="sxs-lookup"><span data-stu-id="fa538-124">In order tooprovide continuity of service for your app, you should not publish changes tooa site currently serving published clients.</span></span> <span data-ttu-id="fa538-125">Ehelyett kell hozzon létre egy új mobil-alkalmazást, amely duplikált funkcionál.</span><span class="sxs-lookup"><span data-stu-id="fa538-125">Instead, you should create a new mobile app that serves as a duplicate.</span></span> <span data-ttu-id="fa538-126">Ez az alkalmazás helyezhet el hello a járulékos költségekkel nélül tooavoid ugyanaz az App Service tervezze meg.</span><span class="sxs-lookup"><span data-stu-id="fa538-126">You can put this application on hello same App Service plan tooavoid incurring additional financial cost.</span></span>

<span data-ttu-id="fa538-127">Ezután meg kell hello alkalmazás két verziója: egy marad hello azonos szolgál hello helyettesítő a közzétett alkalmazást és az egyéb, amely ezután frissítheti és egy új ügyfél célként kiadási.</span><span class="sxs-lookup"><span data-stu-id="fa538-127">You will then have two versions of hello application: one that stays hello same and serves published apps in hello wild, and the other which you can then upgrade and target with a new client release.</span></span> <span data-ttu-id="fa538-128">Helyezze át, és tesztelheti a kódját a ütemben, de meg kell győződnie arról, hogy a bármely hibajavításokat tartalmaz, akkor alkalmazott tooboth beolvasása.</span><span class="sxs-lookup"><span data-stu-id="fa538-128">You can move and test your code at your pace, but you should make sure that any bug fixes you make get applied tooboth.</span></span> <span data-ttu-id="fa538-129">Ha úgy véli, hogy a helyettesítő alkalmazásokat ügyfél kívánt számát toohello legújabb verzióra frissítette, ha felügyelni hello eredeti áttelepített alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="fa538-129">Once you feel that a desired number of client apps in the wild have updated toohello latest version, you can delete hello original migrated app if you desire.</span></span> <span data-ttu-id="fa538-130">Azt nem költségnövekedéssel bármely pénzügyi, ha ugyanaz az App Service megtervezése, mint a Mobile Apps hello tárolva.</span><span class="sxs-lookup"><span data-stu-id="fa538-130">It doesn't incur any additional monetary costs, if hosted in hello same App Service plan as your Mobile App.</span></span>

<span data-ttu-id="fa538-131">hello teljes vázlat hello frissítési folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="fa538-131">hello full outline for hello upgrade process is as follows:</span></span>

1. <span data-ttu-id="fa538-132">Töltse le a meglévő (áttelepített) Azure Mobile szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fa538-132">Download your existing (migrated) Azure Mobile Service.</span></span>
2. <span data-ttu-id="fa538-133">Átalakítás hello projekt tooan Azure Mobile Apps hello kompatibilitási csomag használatával.</span><span class="sxs-lookup"><span data-stu-id="fa538-133">Convert hello project tooan Azure Mobile App using hello compatibility package.</span></span>
3. <span data-ttu-id="fa538-134">Javítsa ki az összes különbséget (például a hitelesítési beállításait).</span><span class="sxs-lookup"><span data-stu-id="fa538-134">Correct any differences (such as authentication settings).</span></span>
4. <span data-ttu-id="fa538-135">A konvertált Azure Mobile Apps-projekt tooa telepítése új App Service.</span><span class="sxs-lookup"><span data-stu-id="fa538-135">Deploy your converted Azure Mobile App project tooa new App Service.</span></span>
5. <span data-ttu-id="fa538-136">Az ügyfél alkalmazás új verzióját, hogy használjon hello új Mobile Apps kiadási.</span><span class="sxs-lookup"><span data-stu-id="fa538-136">Release a new version of your client application that use hello new Mobile App.</span></span>
6. <span data-ttu-id="fa538-137">(Választható) Az eredeti áttelepített mobilszolgáltatás-alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="fa538-137">(Optional) Delete your original migrated mobile service app.</span></span>

<span data-ttu-id="fa538-138">Törlés akkor fordulhat elő, ha nem lát minden forgalom az eredeti áttelepített mobil szolgáltatáson.</span><span class="sxs-lookup"><span data-stu-id="fa538-138">Deletion can occur when you don't see any traffic on your original migrated mobile service.</span></span>

## <span data-ttu-id="fa538-139"><a name="install-npm-package"></a>Hello szükséges előfeltételek telepítése</span><span class="sxs-lookup"><span data-stu-id="fa538-139"><a name="install-npm-package"></a> Install hello Pre-requisites</span></span>
<span data-ttu-id="fa538-140">[Csomópont] a helyi számítógépre kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="fa538-140">You should install [Node] on your local machine.</span></span>  <span data-ttu-id="fa538-141">Hello kompatibilitási csomagot kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="fa538-141">You should also install hello compatibility package.</span></span>  <span data-ttu-id="fa538-142">Csomópont telepítése után futtathatja a következő parancsot egy új cmd vagy a PowerShell-parancssorba hello:</span><span class="sxs-lookup"><span data-stu-id="fa538-142">After Node is installed, you can run hello following command from a new cmd or PowerShell prompt:</span></span>

```npm i -g azure-mobile-apps-compatibility```

## <span data-ttu-id="fa538-143"><a name="obtain-ams-scripts"></a>Az Azure Mobile Services parancsfájlok beszerzése</span><span class="sxs-lookup"><span data-stu-id="fa538-143"><a name="obtain-ams-scripts"></a> Obtain your Azure Mobile Services Scripts</span></span>
* <span data-ttu-id="fa538-144">Jelentkezzen be toohello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="fa538-144">Log in toohello [Azure Portal].</span></span>
* <span data-ttu-id="fa538-145">Használatával **összes erőforrás** vagy **alkalmazásszolgáltatások**, a Mobile Services webhely található.</span><span class="sxs-lookup"><span data-stu-id="fa538-145">Using **All Resources** or **App Services**, find your Mobile Services site.</span></span>
* <span data-ttu-id="fa538-146">Hello webhelyen, kattintson a **eszközök** -> **Kudu** -> **Ugrás** tooopen hello Kudu hely.</span><span class="sxs-lookup"><span data-stu-id="fa538-146">Within hello site, click on **Tools** -> **Kudu** -> **Go** tooopen hello Kudu site.</span></span>
* <span data-ttu-id="fa538-147">Kattintson a **Debug konzol** -> **PowerShell** tooopen hello hibakereső konzolt.</span><span class="sxs-lookup"><span data-stu-id="fa538-147">Click on **Debug Console** -> **PowerShell** tooopen hello Debug console.</span></span>
* <span data-ttu-id="fa538-148">Keresse meg a túl`site/wwwroot/App_Data/config` kattintva pedig minden könyvtár</span><span class="sxs-lookup"><span data-stu-id="fa538-148">Navigate too`site/wwwroot/App_Data/config` by clicking on each directory in turn</span></span>
* <span data-ttu-id="fa538-149">Kattintson a hello letöltési ikon következő toohello `scripts` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="fa538-149">Click on hello download icon next toohello `scripts` directory.</span></span>

<span data-ttu-id="fa538-150">A ZIP-formátum hello parancsfájlok letöltése.</span><span class="sxs-lookup"><span data-stu-id="fa538-150">This will download hello scripts in ZIP format.</span></span>  <span data-ttu-id="fa538-151">Hozzon létre egy új könyvtárat a helyi számítógépen, és csomagolja ki a hello `scripts.ZIP` hello könyvtárban lévő fájlt.</span><span class="sxs-lookup"><span data-stu-id="fa538-151">Create a new directory on your local machine and unpack hello `scripts.ZIP` file within hello directory.</span></span>  <span data-ttu-id="fa538-152">Ezzel létrehoz egy `scripts` directory.</span><span class="sxs-lookup"><span data-stu-id="fa538-152">This will create a `scripts` directory.</span></span>

## <span data-ttu-id="fa538-153"><a name="scaffold-app"></a>Új Azure Mobile Apps-háttéralkalmazás hello strukturálása</span><span class="sxs-lookup"><span data-stu-id="fa538-153"><a name="scaffold-app"></a> Scaffold hello new Azure Mobile Apps backend</span></span>
<span data-ttu-id="fa538-154">Futtassa a parancsot követő hello parancsfájlok könyvtárat tartalmazó hello könyvtárból hello:</span><span class="sxs-lookup"><span data-stu-id="fa538-154">Run hello following command from hello directory containing hello scripts directory:</span></span>

```scaffold-mobile-app scripts out```

<span data-ttu-id="fa538-155">Ezzel létrehoz egy generált Azure Mobile Apps-háttéralkalmazás a hello `out` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="fa538-155">This will create a scaffolded Azure Mobile Apps backend in hello `out` directory.</span></span>  <span data-ttu-id="fa538-156">Bár nem kötelező megadni, akkor egy jó ötlet toocheck hello `out` könyvtárhoz, az Ön által választott forráskódraktárban be.</span><span class="sxs-lookup"><span data-stu-id="fa538-156">Although not required, it's a good idea toocheck hello `out` directory into a source code repository of your choice.</span></span>

## <span data-ttu-id="fa538-157"><a name="deploy-ama-app"></a>Azure Mobile Apps-háttéralkalmazásának telepítése</span><span class="sxs-lookup"><span data-stu-id="fa538-157"><a name="deploy-ama-app"></a> Deploy your Azure Mobile Apps backend</span></span>
<span data-ttu-id="fa538-158">A telepítés során a toodo hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="fa538-158">During deployment, you will need toodo hello following:</span></span>

1. <span data-ttu-id="fa538-159">Hozzon létre egy új Mobile Apps hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="fa538-159">Create a new Mobile App in hello [Azure Portal].</span></span>
2. <span data-ttu-id="fa538-160">Futtassa a hello `createViews.sql` a csatlakoztatott adatbázis a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="fa538-160">Run hello `createViews.sql` script on your connected database.</span></span>
3. <span data-ttu-id="fa538-161">Hivatkozás hello adatbázist csatolt tooyour Mobile Service tooyour új App Service.</span><span class="sxs-lookup"><span data-stu-id="fa538-161">Link hello database that is linked tooyour Mobile Service tooyour new App Service.</span></span>
4. <span data-ttu-id="fa538-162">Egyéb erőforrások (például a Notification hubs használatával) toohello hivatkozás új App Service.</span><span class="sxs-lookup"><span data-stu-id="fa538-162">Link any other resources (such as Notification Hubs) toohello new App Service.</span></span>
5. <span data-ttu-id="fa538-163">Hello generált kód tooyour új hely telepítése.</span><span class="sxs-lookup"><span data-stu-id="fa538-163">Deploy hello generated code tooyour new site.</span></span>

### <a name="create-a-new-mobile-app"></a><span data-ttu-id="fa538-164">Új mobileszköz-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa538-164">Create a new Mobile App</span></span>
1. <span data-ttu-id="fa538-165">Jelentkezzen be hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="fa538-165">Log in at hello [Azure Portal].</span></span>
2. <span data-ttu-id="fa538-166">Kattintson az **+ÚJ** > **Web + mobil** > **Mobile App** elemre, majd adjon meg egy nevet a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="fa538-166">Click **+NEW** > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="fa538-167">A hello **erőforráscsoport**, válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat (a neve megegyezik az alkalmazás használatával hello.)</span><span class="sxs-lookup"><span data-stu-id="fa538-167">For hello **Resource Group**, select an existing resource group, or create a new one (using hello same name as your app.)</span></span>

    <span data-ttu-id="fa538-168">Kiválaszthat egy másik App Service-csomagot, vagy újat is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="fa538-168">You can either select another App Service plan or create a new one.</span></span> <span data-ttu-id="fa538-169">További információk az App Service szolgáltatások terveket, és hogyan toocreate egy új csomagot egy másik tarifacsomagban réteg, majd a kívánt helyre, a következő látható: [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fa538-169">For more about App Services plans and how toocreate a new plan in a different pricing tier and in your desired location, see [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
4. <span data-ttu-id="fa538-170">A hello **App Service-csomag**, hello alapértelmezett terv (a hello [Standard csomagra](https://azure.microsoft.com/pricing/details/app-service/)) van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="fa538-170">For hello **App Service plan**, hello default plan (in hello [Standard tier](https://azure.microsoft.com/pricing/details/app-service/)) is selected.</span></span> <span data-ttu-id="fa538-171">Kiválaszthat egy másik App Service-csomagot is, vagy [létrehozhat egy újat](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="fa538-171">You can also  select a different plan, or [create a new one](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span></span> <span data-ttu-id="fa538-172">hello App Service-csomag beállításai határozzák meg hello [helyet, szolgáltatásokat, költségeket és számítási erőforrásokat](https://azure.microsoft.com/pricing/details/app-service/) az alkalmazáshoz társított.</span><span class="sxs-lookup"><span data-stu-id="fa538-172">hello App Service plan's settings determine hello [location, features, cost and compute resources](https://azure.microsoft.com/pricing/details/app-service/) associated with your app.</span></span>

    <span data-ttu-id="fa538-173">A hello terv kiválasztása után kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fa538-173">After you decide on hello plan, click **Create**.</span></span> <span data-ttu-id="fa538-174">Ez a Mobile Apps-háttéralkalmazás hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fa538-174">This creates hello Mobile App backend.</span></span>

### <a name="run-createviewssql"></a><span data-ttu-id="fa538-175">CreateViews.SQL futtatása</span><span class="sxs-lookup"><span data-stu-id="fa538-175">Run CreateViews.SQL</span></span>
<span data-ttu-id="fa538-176">hello generált alkalmazást tartalmaz egy nevű fájlt `createViews.sql`.</span><span class="sxs-lookup"><span data-stu-id="fa538-176">hello scaffolded app contains a file called `createViews.sql`.</span></span>  <span data-ttu-id="fa538-177">Ezt a parancsfájlt a cél adatbázisban kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="fa538-177">This script must be executed against the target database.</span></span>  <span data-ttu-id="fa538-178">hello kapcsolati karakterlánc hello céladatbázis lehet lekérni a hello áttelepített mobilszolgáltatáshoz **beállítások** részen **kapcsolati karakterláncok**.</span><span class="sxs-lookup"><span data-stu-id="fa538-178">hello connection string for hello target database can be obtained from your migrated mobile service from hello **Settings** blade under **Connection Strings**.</span></span>  <span data-ttu-id="fa538-179">A neve `MS_TableConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="fa538-179">It is named `MS_TableConnectionString`.</span></span>

<span data-ttu-id="fa538-180">Ez a parancsfájl az SQL Server Management Studio vagy Visual Studio belül is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="fa538-180">You can run this script from within SQL Server Management Studio or Visual Studio.</span></span>

### <a name="link-hello-database-tooyour-app-service"></a><span data-ttu-id="fa538-181">Hivatkozás hello adatbázis tooyour App Service</span><span class="sxs-lookup"><span data-stu-id="fa538-181">Link hello Database tooyour App Service</span></span>
<span data-ttu-id="fa538-182">Hivatkozás hello meglévő adatbázis tooyour App Service:</span><span class="sxs-lookup"><span data-stu-id="fa538-182">Link hello existing database tooyour App Service:</span></span>

* <span data-ttu-id="fa538-183">A hello [Azure Portal], nyissa meg az App Service.</span><span class="sxs-lookup"><span data-stu-id="fa538-183">In hello [Azure Portal], open your App Service.</span></span>
* <span data-ttu-id="fa538-184">Válassza ki **összes beállítás** -> **adatkapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="fa538-184">Select **All Settings** -> **Data Connections**.</span></span>
* <span data-ttu-id="fa538-185">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fa538-185">Click on **+ Add**.</span></span>
* <span data-ttu-id="fa538-186">Hello legördülő listán, válassza ki **SQL-adatbázis**</span><span class="sxs-lookup"><span data-stu-id="fa538-186">In hello drop-down, select **SQL Database**</span></span>
* <span data-ttu-id="fa538-187">A **SQL-adatbázis**, válassza ki a meglévő adatbázist, majd kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="fa538-187">Under **SQL Database**, select your existing database, then click on **Select**.</span></span>
* <span data-ttu-id="fa538-188">A **kapcsolati karakterlánc**, hello adatbázis hello felhasználónevet és jelszót adjon meg, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa538-188">Under **Connection string**, enter hello username and password for hello database, then click on **OK**.</span></span>
* <span data-ttu-id="fa538-189">A hello **adja hozzá az adatokhoz való kapcsolódást** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa538-189">In hello **Add data connections** blade, click on **OK**.</span></span>

<span data-ttu-id="fa538-190">hello felhasználónév és jelszó található kapcsolati karakterlánc hello hello céladatbázis az áttelepített Mobile Service megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="fa538-190">hello username and password can be found by viewing hello Connection String for hello target database in your migrated Mobile Service.</span></span>

### <a name="set-up-authentication"></a><span data-ttu-id="fa538-191">Hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="fa538-191">Set up authentication</span></span>
<span data-ttu-id="fa538-192">Az Azure Mobile Apps tooconfigure Azure Active Directory, a Facebook, a Google, a Microsoft és a Twitterhez hitelesítési belül hello szolgáltatás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="fa538-192">Azure Mobile Apps allows you tooconfigure Azure Active Directory, Facebook, Google, Microsoft and Twitter authentication within hello service.</span></span>  <span data-ttu-id="fa538-193">Egyéni hitelesítési toobe fejlesztett külön-külön kell.</span><span class="sxs-lookup"><span data-stu-id="fa538-193">Custom authentication will need toobe developed separately.</span></span>  <span data-ttu-id="fa538-194">Tekintse meg a hello [hitelesítési fogalmakkal] dokumentáció és [hitelesítés gyorsindító] dokumentációjában talál további információt.</span><span class="sxs-lookup"><span data-stu-id="fa538-194">Refer to hello [Authentication Concepts] documentation and [Authentication Quickstart] documentation for more information.</span></span>  

## <span data-ttu-id="fa538-195"><a name="updating-clients"></a>Mobil ügyfelek frissítése</span><span class="sxs-lookup"><span data-stu-id="fa538-195"><a name="updating-clients"></a>Update Mobile clients</span></span>
<span data-ttu-id="fa538-196">Miután egy operatív mobil-háttéralkalmazást, használhat az ügyfélalkalmazást, amely használ fel az új verziója.</span><span class="sxs-lookup"><span data-stu-id="fa538-196">Once you have an operational Mobile App backend, you can work on a new version of your client application which consumes it.</span></span> <span data-ttu-id="fa538-197">Mobile Apps is hello ügyfél SDK-k új verzióját tartalmazza, és a fenti a hasonló toohello kiszolgáló frissítése, szüksége lesz az összes hivatkozott toohello Mobile Services SDK előtt tooremove a Mobile Apps-verziók telepítése.</span><span class="sxs-lookup"><span data-stu-id="fa538-197">Mobile Apps also includes a new version of hello client SDKs, and similar toohello server upgrade above, you will need tooremove all references toohello Mobile Services SDKs before installing the Mobile Apps versions.</span></span>

<span data-ttu-id="fa538-198">Egyik fő változás hello hello verziói között, hogy a hello konstruktorok már nem szükséges-e egy alkalmazás-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="fa538-198">One of hello main changes between hello versions is that hello constructors no longer require an application key.</span></span>
<span data-ttu-id="fa538-199">Most már egyszerűen adja meg a a Mobile Apps hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="fa538-199">You now simply pass in hello URL of your Mobile App.</span></span> <span data-ttu-id="fa538-200">Például hello .NET ügyfeleken hello `MobileServiceClient` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="fa538-200">For example, on hello .NET clients, hello `MobileServiceClient` constructor is now:</span></span>

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of hello Mobile App
        );

<span data-ttu-id="fa538-201">Áttekintheti az telepítése új SDK-k hello és hello új struktúra keresztül az alábbi hivatkozások hello használata:</span><span class="sxs-lookup"><span data-stu-id="fa538-201">You can read about installing hello new SDKs and using hello new structure via hello links below:</span></span>

* [<span data-ttu-id="fa538-202">2.2 vagy újabb Android verziója</span><span class="sxs-lookup"><span data-stu-id="fa538-202">Android version 2.2 or later</span></span>](app-service-mobile-android-how-to-use-client-library.md)
* [<span data-ttu-id="fa538-203">iOS-verzióval 3.0.0 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="fa538-203">iOS version 3.0.0 or later</span></span>](app-service-mobile-ios-how-to-use-client-library.md)
* [<span data-ttu-id="fa538-204">.NET (Windows/Xamarin) 2.0.0 verzió vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="fa538-204">.NET (Windows/Xamarin) version 2.0.0 or later</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)
* [<span data-ttu-id="fa538-205">Apache Cordova 2.0 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="fa538-205">Apache Cordova version 2.0 or later</span></span>](app-service-mobile-cordova-how-to-use-client-library.md)

<span data-ttu-id="fa538-206">Ha az alkalmazás lehetővé teszi leküldéses értesítések használni, jegyezze fel a hello regisztráció utasításokat minden egyes platformhoz, néhány módosítás történt hiba is.</span><span class="sxs-lookup"><span data-stu-id="fa538-206">If your application makes use of push notifications, make note of hello specific registration instructions for each platform, as there have been some changes there as well.</span></span>

<span data-ttu-id="fa538-207">Ha hello új ügyfélverziót készen áll, próbálja ki a frissített kiszolgálón projekthez.</span><span class="sxs-lookup"><span data-stu-id="fa538-207">When you have hello new client version ready, try it out against your upgraded server project.</span></span> <span data-ttu-id="fa538-208">Után ellenőrzése, hogy működik-e, az alkalmazás toocustomers egy új verziója is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="fa538-208">After validating that it works, you can release a new version of your application toocustomers.</span></span> <span data-ttu-id="fa538-209">Előfordulhat Ha az ügyfelek rendelkezésére állt-e egy alkalommal tooreceive ezeket a frissítéseket, törölheti hello Mobile Services, verziószám: az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fa538-209">Eventually, once your customers have had a chance tooreceive these updates, you can delete hello Mobile Services version of your app.</span></span> <span data-ttu-id="fa538-210">Ezen a ponton frissített tooan App Service Mobile Apps hello legújabb Mobile Apps server SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="fa538-210">At this point, you have completely upgraded tooan App Service Mobile App using hello latest Mobile Apps server SDK.</span></span>

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Mik azok a Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[App Service díjszabás]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[hitelesítési fogalmakkal]: ../app-service/app-service-authentication-overview.md
[hitelesítés gyorsindító]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
