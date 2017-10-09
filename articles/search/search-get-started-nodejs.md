---
title: "aaaGet Node.js az Azure Search használatába |} Microsoft Docs"
description: "Keresőalkalmazás felépítési útmutatója az Azure egy üzemeltetett felhőalapú keresőszolgáltatásához a Node.js programozási nyelv használatával."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 0625dc1b-9db6-40d5-ba9a-4738b75cbe19
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: evboyle
ms.openlocfilehash: e9c7d756c2ea191ee2a285485c90439b96aa73b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a><span data-ttu-id="62b3e-103">Bevezetés az Azure Search használatába Node.js-ben</span><span class="sxs-lookup"><span data-stu-id="62b3e-103">Get started with Azure Search in Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="62b3e-104">Portal</span><span class="sxs-lookup"><span data-stu-id="62b3e-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="62b3e-105">.NET</span><span class="sxs-lookup"><span data-stu-id="62b3e-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="62b3e-106">Ismerje meg, egy egyéni Node.js toobuild a keresési alkalmazás használ az Azure Search szolgáltatást a keresésekhez.</span><span class="sxs-lookup"><span data-stu-id="62b3e-106">Learn how toobuild a custom Node.js search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="62b3e-107">Ez az oktatóanyag használja hello [Azure Search szolgáltatás REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objektumok és műveletek ebben a gyakorlatban.</span><span class="sxs-lookup"><span data-stu-id="62b3e-107">This tutorial uses hello [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objects and operations used in this exercise.</span></span>

<span data-ttu-id="62b3e-108">Használtuk [Node.js](https://Nodejs.org) és NPM, [Sublime Text 3](http://www.sublimetext.com/3), és a Windows PowerShell, a Windows 8.1 toodevelop és tesztelheti ezt a kódot.</span><span class="sxs-lookup"><span data-stu-id="62b3e-108">We used [Node.js](https://Nodejs.org) and NPM, [Sublime Text 3](http://www.sublimetext.com/3), and Windows PowerShell on Windows 8.1 toodevelop and test this code.</span></span>

<span data-ttu-id="62b3e-109">toorun Ez a minta rendelkeznie kell egy Azure Search szolgáltatással, amely jelentkezhet be a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="62b3e-109">toorun this sample, you must have an Azure Search service, which you can sign up for in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="62b3e-110">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) részletes útmutatásait.</span><span class="sxs-lookup"><span data-stu-id="62b3e-110">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

## <a name="about-hello-data"></a><span data-ttu-id="62b3e-111">Hello adatokról</span><span class="sxs-lookup"><span data-stu-id="62b3e-111">About hello data</span></span>
<span data-ttu-id="62b3e-112">A mintaalkalmazás hello adatait használja [az Amerikai Egyesült Államok geológiai szolgáltatások (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), szűrt a hello Rhode Island államra tooreduce hello dataset mérete.</span><span class="sxs-lookup"><span data-stu-id="62b3e-112">This sample application uses data from hello [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on hello state of Rhode Island tooreduce hello dataset size.</span></span> <span data-ttu-id="62b3e-113">Az adatok toobuild, amely jellegzetes épületeket, például kórházakat és iskolákat, valamint geológiai jellegzetességeket, például folyókat, tavakat és hegycsúcsokat ad vissza egy olyan keresőalkalmazás fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="62b3e-113">We'll use this data toobuild a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="62b3e-114">Ebben az alkalmazásban hello **DataIndexer** program létrehozza és betölti az indexet hello egy [indexelő](https://msdn.microsoft.com/library/azure/dn798918.aspx) szerkezet hello szűrt USGS-adatkészletet a nyilvános Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="62b3e-114">In this application, hello **DataIndexer** program builds and loads hello index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving hello filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="62b3e-115">Hitelesítő adatok és csatlakozási adatokat toohello online adatforrás hello programkód valósul meg.</span><span class="sxs-lookup"><span data-stu-id="62b3e-115">Credentials and connection information toohello online data source is provided in hello program code.</span></span> <span data-ttu-id="62b3e-116">Nincs szükség további konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="62b3e-116">No further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="62b3e-117">Olyan szűrőt alkalmaztunk a dataset toostay hello 10 000 dokumentumos korlátja az ingyenes tarifacsomag hello alatt.</span><span class="sxs-lookup"><span data-stu-id="62b3e-117">We applied a filter on this dataset toostay under hello 10,000 document limit of hello free pricing tier.</span></span> <span data-ttu-id="62b3e-118">Hello standard csomagot használja, ha a nem vonatkozik ez a korlátozás.</span><span class="sxs-lookup"><span data-stu-id="62b3e-118">If you use hello standard tier, this limit does not apply.</span></span> <span data-ttu-id="62b3e-119">Az egyes tarifacsomagok kapacitásával kapcsolatos részletes információkat lásd: [A Search szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="62b3e-119">For details about capacity for each pricing tier, see [Search service limits](search-limits-quotas-capacity.md).</span></span>
> 
> 

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="62b3e-120">Hello szolgáltatás- és api-kulcsot az Azure Search szolgáltatás keresése</span><span class="sxs-lookup"><span data-stu-id="62b3e-120">Find hello service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="62b3e-121">Miután létrehozta a hello szolgáltatást, térjen vissza toohello portál tooget hello URL-cím vagy `api-key`.</span><span class="sxs-lookup"><span data-stu-id="62b3e-121">After you create hello service, return toohello portal tooget hello URL or `api-key`.</span></span> <span data-ttu-id="62b3e-122">Kapcsolatok tooyour keresési szolgáltatás szükséges, hogy rendelkezik-e mindkét hello URL-címet és egy `api-key` tooauthenticate hello hívás.</span><span class="sxs-lookup"><span data-stu-id="62b3e-122">Connections tooyour Search service require that you have both hello URL and an `api-key` tooauthenticate hello call.</span></span>

1. <span data-ttu-id="62b3e-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="62b3e-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="62b3e-124">Hello Ugrás sávon kattintson **keresési szolgáltatás** minden Azure Search szolgáltatás megjelenjen az előfizetéséhez kapcsolódó toolist.</span><span class="sxs-lookup"><span data-stu-id="62b3e-124">In hello jump bar, click **Search service** toolist all Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="62b3e-125">Válassza ki a kívánt toouse hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="62b3e-125">Select hello service you want toouse.</span></span>
4. <span data-ttu-id="62b3e-126">Hello szolgáltatás irányítópultján meg kell jelennie az alapvető információkat, például a hello adminisztrációs kulcsok eléréséhez szükséges kulcs ikon hello csempék.</span><span class="sxs-lookup"><span data-stu-id="62b3e-126">On hello service dashboard, you should see tiles for essential information, such as hello key icon for accessing hello admin keys.</span></span>
5. <span data-ttu-id="62b3e-127">Másolja a hello szolgáltatás URL-cím, egy adminisztrációs kulcsot és egy lekérdezési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="62b3e-127">Copy hello service URL, an admin key, and a query key.</span></span> <span data-ttu-id="62b3e-128">Három később szüksége amikor toohello config.js fájl hozzáadja őket.</span><span class="sxs-lookup"><span data-stu-id="62b3e-128">You need all three later when you add them toohello config.js file.</span></span>

## <a name="download-hello-sample-files"></a><span data-ttu-id="62b3e-129">Hello mintafájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="62b3e-129">Download hello sample files</span></span>
<span data-ttu-id="62b3e-130">A következő módszerekkel toodownload hello minta hello bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="62b3e-130">Use either one of hello following approaches toodownload hello sample.</span></span>

1. <span data-ttu-id="62b3e-131">Nyissa meg túl[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span><span class="sxs-lookup"><span data-stu-id="62b3e-131">Go too[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span></span>
2. <span data-ttu-id="62b3e-132">Kattintson a **töltse le a ZIP-**, mentse hello .zip fájlt, és bontsa ki a benne található összes hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="62b3e-132">Click **Download ZIP**, save hello .zip file, and then extract all hello files it contains.</span></span>

<span data-ttu-id="62b3e-133">Minden további fájlmódosítás és utasításfuttatás az ebben a mappában lévő fájlokra vonatkozóan történik.</span><span class="sxs-lookup"><span data-stu-id="62b3e-133">All subsequent file modifications and run statements are made against files in this folder.</span></span>

## <a name="update-hello-configjs-with-your-search-service-url-and-api-key"></a><span data-ttu-id="62b3e-134">Frissítse a hello config.js.</span><span class="sxs-lookup"><span data-stu-id="62b3e-134">Update hello config.js.</span></span> <span data-ttu-id="62b3e-135">a keresőszolgáltatása URL-címével és API-kulcsával</span><span class="sxs-lookup"><span data-stu-id="62b3e-135">with your Search service URL and api-key</span></span>
<span data-ttu-id="62b3e-136">URL-CÍMÉT és api-kulcsot, korábban kimásolt, használatával hello hello URL-Címének megadása, adminisztrációs-kulcsot, és a lekérdezési kulcsot a konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="62b3e-136">Using hello URL and api-key that you copied earlier, specify hello URL, admin-key, and query-key in configuration file.</span></span>

<span data-ttu-id="62b3e-137">Az adminisztrációs kulcsok teljes körű vezérlést biztosítanak a szolgáltatási műveletek felett, beleértve az index létrehozását vagy törlését, illetve a dokumentumok betöltését.</span><span class="sxs-lookup"><span data-stu-id="62b3e-137">Admin keys grant full control over service operations, including creating or deleting an index and loading documents.</span></span> <span data-ttu-id="62b3e-138">Ezzel szemben lekérdezés kulcsai csak olvasható műveletekhez, jellemzően tooAzure keresési szolgáltatáshoz kapcsolódó ügyfélalkalmazásokhoz által használt.</span><span class="sxs-lookup"><span data-stu-id="62b3e-138">In contrast, query keys are for read-only operations, typically used by client applications that connect tooAzure Search.</span></span>

<span data-ttu-id="62b3e-139">Ez a példa hello lekérdezési kulcs toohelp megerősítése hello ajánlott eljárás használatával hello lekérdezési kulcs ügyfélalkalmazásokban magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="62b3e-139">In this sample, we include hello query key toohelp reinforce hello best practice of using hello query key in client applications.</span></span>

<span data-ttu-id="62b3e-140">a következő képernyőfelvételen látható hello **config.js** hello egy szövegszerkesztőben megnyitott megfelelő bejegyzések meg vannak jelölve, hogy láthatja, ahol tooupdate hello fájl hello értékek, amelyek érvényesek a keresési szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="62b3e-140">hello following screenshot shows **config.js** open in a text editor, with hello relevant entries demarcated so that you can see where tooupdate hello file with hello values that are valid for your search service.</span></span>

![][5]

## <a name="host-a-runtime-environment-for-hello-sample"></a><span data-ttu-id="62b3e-141">Hello minta futtatókörnyezetének üzemeltetése</span><span class="sxs-lookup"><span data-stu-id="62b3e-141">Host a runtime environment for hello sample</span></span>
<span data-ttu-id="62b3e-142">hello minta HTTP-kiszolgáló, amely npm segítségével globálisan telepítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="62b3e-142">hello sample requires an HTTP server, which you can install globally using npm.</span></span>

<span data-ttu-id="62b3e-143">A következő parancsok hello használni egy PowerShell-ablakot.</span><span class="sxs-lookup"><span data-stu-id="62b3e-143">Use a PowerShell window for hello following commands.</span></span>

1. <span data-ttu-id="62b3e-144">Keresse meg a hello tartalmazó mappa toohello **package.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="62b3e-144">Navigate toohello folder that contains hello **package.json** file.</span></span>
2. <span data-ttu-id="62b3e-145">Gépelje be: `npm install`.</span><span class="sxs-lookup"><span data-stu-id="62b3e-145">Type `npm install`.</span></span>
3. <span data-ttu-id="62b3e-146">Gépelje be: `npm install -g http-server`.</span><span class="sxs-lookup"><span data-stu-id="62b3e-146">Type `npm install -g http-server`.</span></span>

## <a name="build-hello-index-and-run-hello-application"></a><span data-ttu-id="62b3e-147">Hello index létrehozása és hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="62b3e-147">Build hello index and run hello application</span></span>
1. <span data-ttu-id="62b3e-148">Gépelje be: `npm run indexDocuments`.</span><span class="sxs-lookup"><span data-stu-id="62b3e-148">Type `npm run indexDocuments`.</span></span>
2. <span data-ttu-id="62b3e-149">Gépelje be: `npm run build`.</span><span class="sxs-lookup"><span data-stu-id="62b3e-149">Type `npm run build`.</span></span>
3. <span data-ttu-id="62b3e-150">Gépelje be: `npm run start_server`.</span><span class="sxs-lookup"><span data-stu-id="62b3e-150">Type `npm run start_server`.</span></span>
4. <span data-ttu-id="62b3e-151">Irányítsa a böngészőt a következő helyre: `http://localhost:8080/index.html`</span><span class="sxs-lookup"><span data-stu-id="62b3e-151">Direct your browser at `http://localhost:8080/index.html`</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="62b3e-152">USGS-adatok keresése</span><span class="sxs-lookup"><span data-stu-id="62b3e-152">Search on USGS data</span></span>
<span data-ttu-id="62b3e-153">hello USGS-adatkészlet a Rhode Island államra vonatkozó toohello rekordokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="62b3e-153">hello USGS data set includes records that are relevant toohello state of Rhode Island.</span></span> <span data-ttu-id="62b3e-154">Ha **keresési** egy üres keresőmező kap hello 50 legfontosabb bejegyzés; hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="62b3e-154">If you click **Search** on an empty search box, you get hello top 50 entries, which is hello default.</span></span>

<span data-ttu-id="62b3e-155">A keresett kifejezés beírása ad hello keresőmotor valamit a toogo.</span><span class="sxs-lookup"><span data-stu-id="62b3e-155">Entering a search term gives hello search engine something toogo on.</span></span> <span data-ttu-id="62b3e-156">Próbáljon meg a helyhez kötődő nevet beírni.</span><span class="sxs-lookup"><span data-stu-id="62b3e-156">Try entering a regional name.</span></span> <span data-ttu-id="62b3e-157">"Roger Williams" volt Rhode Island első kormányzója hello.</span><span class="sxs-lookup"><span data-stu-id="62b3e-157">"Roger Williams" was hello first governor of Rhode Island.</span></span> <span data-ttu-id="62b3e-158">Számos parkot, épületet és iskolát neveztek el róla.</span><span class="sxs-lookup"><span data-stu-id="62b3e-158">Numerous parks, buildings, and schools are named after him.</span></span>

![][9]

<span data-ttu-id="62b3e-159">Megpróbálhatja beírni az alábbi kifejezések bármelyikét is:</span><span class="sxs-lookup"><span data-stu-id="62b3e-159">You could also try any of these terms:</span></span>

* <span data-ttu-id="62b3e-160">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="62b3e-160">Pawtucket</span></span>
* <span data-ttu-id="62b3e-161">Pembroke</span><span class="sxs-lookup"><span data-stu-id="62b3e-161">Pembroke</span></span>
* <span data-ttu-id="62b3e-162">goose+cape</span><span class="sxs-lookup"><span data-stu-id="62b3e-162">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="62b3e-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62b3e-163">Next steps</span></span>
<span data-ttu-id="62b3e-164">Ez az hello Azure Search első oktatóanyaga a Node.js és hello USGS-adatkészlet alapján.</span><span class="sxs-lookup"><span data-stu-id="62b3e-164">This is hello first Azure Search tutorial based on Node.js and hello USGS dataset.</span></span> <span data-ttu-id="62b3e-165">Idővel majd tovább bővítjük ezen oktatóanyag toodemonstrate kiegészítő keresési funkciókat, amelyeket esetleg szívesen toouse egyéni megoldásaiban.</span><span class="sxs-lookup"><span data-stu-id="62b3e-165">Over time, we'll extend this tutorial toodemonstrate additional search features you might want toouse in your custom solutions.</span></span>

<span data-ttu-id="62b3e-166">Ha már rendelkezik bizonyos tapasztalattal az Azure Search használatában, ezt a mintát akár ugródeszkaként is használhatja a javaslattevők (előre begépelt vagy automatikusan kitöltött lekérdezések), szűrők és a jellemzőalapú navigáció kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="62b3e-166">If you already have some background in Azure Search, you can use this sample as a springboard for trying suggesters (type-ahead or autocomplete queries), filters, and faceted navigation.</span></span> <span data-ttu-id="62b3e-167">Akkor is tovább fejlesztheti hello keresési eredmények oldalát által számok és kötegelt dokumentumok, így a felhasználók lapozhassanak az eredmények hello hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="62b3e-167">You can also improve upon hello search results page by adding counts and batching documents so that users can page through hello results.</span></span>

<span data-ttu-id="62b3e-168">Új tooAzure keresési?</span><span class="sxs-lookup"><span data-stu-id="62b3e-168">New tooAzure Search?</span></span> <span data-ttu-id="62b3e-169">Azt javasoljuk, próbáljon más oktatóanyagok toodevelop megismerhesse, mit hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="62b3e-169">We recommend trying other tutorials toodevelop an understanding of what you can create.</span></span> <span data-ttu-id="62b3e-170">Látogasson el a [dokumentációs oldal](https://azure.microsoft.com/documentation/services/search/) toofind több erőforrást.</span><span class="sxs-lookup"><span data-stu-id="62b3e-170">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) toofind more resources.</span></span> <span data-ttu-id="62b3e-171">Hello hivatkozások is megtekintheti a [videók és oktatóanyagok listáját](search-video-demo-tutorial-list.md) tooaccess további információt.</span><span class="sxs-lookup"><span data-stu-id="62b3e-171">You can also view hello links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) tooaccess more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
