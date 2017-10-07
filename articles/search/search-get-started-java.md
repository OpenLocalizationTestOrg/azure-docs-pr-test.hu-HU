---
title: "aaaGet Azure Search Java használatába |} Microsoft Docs"
description: "A szolgáltatott felhők toobuild keresési alkalmazás Azure-ban a Java programozási nyelv."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 8b4df3c9-3ae5-4e3a-b4bb-74b516a91c8e
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
ms.openlocfilehash: 5476a2103f3b60fe6ec78ff3d3fdba9fcff55c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-java"></a><span data-ttu-id="665ee-103">Bevezetés az Azure Search használatába Java nyelven</span><span class="sxs-lookup"><span data-stu-id="665ee-103">Get started with Azure Search in Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="665ee-104">Portál</span><span class="sxs-lookup"><span data-stu-id="665ee-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="665ee-105">.NET</span><span class="sxs-lookup"><span data-stu-id="665ee-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="665ee-106">Ismerje meg, olyan egyéni Java toobuild a keresési alkalmazás az Azure Search szolgáltatást a keresésekhez használ.</span><span class="sxs-lookup"><span data-stu-id="665ee-106">Learn how toobuild a custom Java search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="665ee-107">Ez az oktatóanyag használja hello [Azure Search szolgáltatás REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objektumok és műveletek ebben a gyakorlatban.</span><span class="sxs-lookup"><span data-stu-id="665ee-107">This tutorial uses hello [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objects and operations used in this exercise.</span></span>

<span data-ttu-id="665ee-108">toorun Ez a minta rendelkeznie kell egy Azure Search szolgáltatással, amely jelentkezhet be a hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="665ee-108">toorun this sample, you must have an Azure Search service, which you can sign up for in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="665ee-109">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) részletes útmutatásait.</span><span class="sxs-lookup"><span data-stu-id="665ee-109">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

<span data-ttu-id="665ee-110">Azt a következő szoftver toobuild hello használt, és ez a minta teszteléséhez:</span><span class="sxs-lookup"><span data-stu-id="665ee-110">We used hello following software toobuild and test this sample:</span></span>

* <span data-ttu-id="665ee-111">[Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span><span class="sxs-lookup"><span data-stu-id="665ee-111">[Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span></span> <span data-ttu-id="665ee-112">Lehet, hogy toodownload hello EE-verziót.</span><span class="sxs-lookup"><span data-stu-id="665ee-112">Be sure toodownload hello EE version.</span></span> <span data-ttu-id="665ee-113">Egy szolgáltatás, amely csak a jelen kiadásában megtalálható hello ellenőrzési lépések egyikét igényli.</span><span class="sxs-lookup"><span data-stu-id="665ee-113">One of hello verification steps requires a feature that is found only in this edition.</span></span>
* [<span data-ttu-id="665ee-114">JDK 8u40</span><span class="sxs-lookup"><span data-stu-id="665ee-114">JDK 8u40</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [<span data-ttu-id="665ee-115">Apache Tomcat 8.0</span><span class="sxs-lookup"><span data-stu-id="665ee-115">Apache Tomcat 8.0</span></span>](http://tomcat.apache.org/download-80.cgi)

## <a name="about-hello-data"></a><span data-ttu-id="665ee-116">Hello adatokról</span><span class="sxs-lookup"><span data-stu-id="665ee-116">About hello data</span></span>
<span data-ttu-id="665ee-117">A mintaalkalmazás hello adatait használja [az Amerikai Egyesült Államok geológiai szolgáltatások (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), szűrt a hello Rhode Island államra tooreduce hello dataset mérete.</span><span class="sxs-lookup"><span data-stu-id="665ee-117">This sample application uses data from hello [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on hello state of Rhode Island tooreduce hello dataset size.</span></span> <span data-ttu-id="665ee-118">Az adatok toobuild, amely jellegzetes épületeket, például kórházakat és iskolákat, valamint geológiai jellegzetességeket, például folyókat, tavakat és hegycsúcsokat ad vissza egy olyan keresőalkalmazás fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="665ee-118">We'll use this data toobuild a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="665ee-119">Ebben az alkalmazásban hello **SearchServlet.java** program létrehozza és betölti az indexet hello egy [indexelő](https://msdn.microsoft.com/library/azure/dn798918.aspx) szerkezet hello szűrt USGS-adatkészletet a nyilvános Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="665ee-119">In this application, hello **SearchServlet.java** program builds and loads hello index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving hello filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="665ee-120">Előre meghatározott hitelesítő adatokat és kapcsolati adatokat toohello online adatforrás hello programkód találhatók.</span><span class="sxs-lookup"><span data-stu-id="665ee-120">Predefined credentials and connection  information toohello online data source are provided in hello program code.</span></span> <span data-ttu-id="665ee-121">Az adatelérés szempontjából nincs szükség további konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="665ee-121">In terms of data access, no further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="665ee-122">Olyan szűrőt alkalmaztunk a dataset toostay hello 10 000 dokumentumos korlátja az ingyenes tarifacsomag hello alatt.</span><span class="sxs-lookup"><span data-stu-id="665ee-122">We applied a filter on this dataset toostay under hello 10,000 document limit of hello free pricing tier.</span></span> <span data-ttu-id="665ee-123">Hello standard csomagot használja, ha nem vonatkozik ez a korlátozás, és módosíthatja a kód toouse nagyobb adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="665ee-123">If you use hello standard tier, this limit does not apply, and you can modify this code toouse a bigger dataset.</span></span> <span data-ttu-id="665ee-124">Az egyes tarifacsomagok kapacitásával kapcsolatos részletes információkat lásd: [Korlátozások és megkötések](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="665ee-124">For details about capacity for each pricing tier, see [Limits and constraints](search-limits-quotas-capacity.md).</span></span>
> 
> 

## <a name="about-hello-program-files"></a><span data-ttu-id="665ee-125">Hello program fájlokkal kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="665ee-125">About hello program files</span></span>
<span data-ttu-id="665ee-126">hello alábbi lista részletesen ismerteti, amelyek megfelelő toothis minta hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="665ee-126">hello following list describes hello files that are relevant toothis sample.</span></span>

* <span data-ttu-id="665ee-127">Search.jsp: Hello felhasználói felületet biztosít.</span><span class="sxs-lookup"><span data-stu-id="665ee-127">Search.jsp: Provides hello user interface</span></span>
* <span data-ttu-id="665ee-128">SearchServlet.java: Biztosít módszerek (hasonló tooa az MVC-vezérlőhöz)</span><span class="sxs-lookup"><span data-stu-id="665ee-128">SearchServlet.java: Provides methods (similar tooa controller in MVC)</span></span>
* <span data-ttu-id="665ee-129">SearchServiceClient.java: a HTTP-kérelmeket kezeli</span><span class="sxs-lookup"><span data-stu-id="665ee-129">SearchServiceClient.java: Handles HTTP requests</span></span>
* <span data-ttu-id="665ee-130">SearchServiceHelper.java: egy statikus módszereket biztosító segítőosztály</span><span class="sxs-lookup"><span data-stu-id="665ee-130">SearchServiceHelper.java: A helper class that provides static methods</span></span>
* <span data-ttu-id="665ee-131">Document.Java: Hello adatok modellt biztosít.</span><span class="sxs-lookup"><span data-stu-id="665ee-131">Document.java: Provides hello data model</span></span>
* <span data-ttu-id="665ee-132">Config.Properties: Beállítja hello Search szolgáltatás URL-CÍMÉT és api-kulcs</span><span class="sxs-lookup"><span data-stu-id="665ee-132">config.properties: Sets hello Search service URL and api-key</span></span>
* <span data-ttu-id="665ee-133">Pom.XML: Maven-függőség</span><span class="sxs-lookup"><span data-stu-id="665ee-133">Pom.xml: A Maven dependency</span></span>

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="665ee-134">Hello szolgáltatás- és api-kulcsot az Azure Search szolgáltatás keresése</span><span class="sxs-lookup"><span data-stu-id="665ee-134">Find hello service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="665ee-135">Azure Search szolgáltatásba történő minden REST API-hívások meg kell adnia hello szolgáltatás URL-CÍMÉT és api-kulcsát.</span><span class="sxs-lookup"><span data-stu-id="665ee-135">All REST API calls into Azure Search require that you provide hello service URL and an api-key.</span></span> 

1. <span data-ttu-id="665ee-136">Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="665ee-136">Sign in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="665ee-137">Hello Ugrás sávon kattintson **keresési szolgáltatás** toolist megjelenjen az előfizetéséhez kapcsolódó összes hello Azure Search szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="665ee-137">In hello jump bar, click **Search service** toolist all of hello Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="665ee-138">Válassza ki a kívánt toouse hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="665ee-138">Select hello service you want toouse.</span></span>
4. <span data-ttu-id="665ee-139">A hello szolgáltatás irányítópultján megjelennek az alapvető információkat csempék, valamint hello adminisztrációs kulcsok eléréséhez szükséges kulcs ikon hello.</span><span class="sxs-lookup"><span data-stu-id="665ee-139">On hello service dashboard, you'll see tiles for essential information as well as hello key icon for accessing hello admin keys.</span></span>
   
      ![][3]
5. <span data-ttu-id="665ee-140">Másolja a hello szolgáltatás URL-címet és egy adminisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="665ee-140">Copy hello service URL and an admin key.</span></span> <span data-ttu-id="665ee-141">Később szüksége lesz rájuk, miután hozzáadta őket toohello **config.properties** fájlt.</span><span class="sxs-lookup"><span data-stu-id="665ee-141">You will need them later, when you add them toohello **config.properties** file.</span></span>

## <a name="download-hello-sample-files"></a><span data-ttu-id="665ee-142">Hello mintafájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="665ee-142">Download hello sample files</span></span>
1. <span data-ttu-id="665ee-143">Nyissa meg túl[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) a Githubon.</span><span class="sxs-lookup"><span data-stu-id="665ee-143">Go too[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) on GitHub.</span></span>
2. <span data-ttu-id="665ee-144">Kattintson a **töltse le a ZIP-**, hello .zip fájl toodisk mentéséhez, és bontsa ki a benne található összes hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="665ee-144">Click **Download ZIP**, save hello .zip file toodisk, and then extract all hello files it contains.</span></span> <span data-ttu-id="665ee-145">Vegye figyelembe a kibontása hello fájlok azokat a Java-munkaterület toomake könnyebb toofind hello projekt később.</span><span class="sxs-lookup"><span data-stu-id="665ee-145">Consider extracting hello files into your Java workspace toomake it easier toofind hello project later.</span></span>
3. <span data-ttu-id="665ee-146">hello mintafájlt csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="665ee-146">hello sample files are read-only.</span></span> <span data-ttu-id="665ee-147">Kattintson a jobb gombbal a mappa tulajdonságai és egyértelmű hello csak olvasható attribútumot.</span><span class="sxs-lookup"><span data-stu-id="665ee-147">Right-click folder properties and clear hello read-only attribute.</span></span>

<span data-ttu-id="665ee-148">Minden további fájlmódosítás és utasításfuttatás az ebben a mappában lévő fájlokra vonatkozóan fog történni.</span><span class="sxs-lookup"><span data-stu-id="665ee-148">All subsequent file modifications and run statements will be made against files in this folder.</span></span>  

## <a name="import-project"></a><span data-ttu-id="665ee-149">Projekt importálása</span><span class="sxs-lookup"><span data-stu-id="665ee-149">Import project</span></span>
1. <span data-ttu-id="665ee-150">Az Eclipse-ben válassza ki a **File** (Fájl)  > **Import** (Importálás)  > **General** (Általános)  > **Existing Projects into Workspace** (Meglévő projekteket a munkaterületre) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="665ee-150">In Eclipse, choose **File** > **Import** > **General** > **Existing Projects into Workspace**.</span></span>
   
    ![][4]
2. <span data-ttu-id="665ee-151">A **Select root directory**, keresse meg a mintafájlokat tartalmazó toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="665ee-151">In **Select root directory**, browse toohello folder containing sample files.</span></span> <span data-ttu-id="665ee-152">Válassza ki a hello .project mappát tartalmazó hello mappára.</span><span class="sxs-lookup"><span data-stu-id="665ee-152">Select hello folder that contains hello .project folder.</span></span> <span data-ttu-id="665ee-153">hello meg kell jelennie a projektben hello **projektek** listában kiválasztott elemként.</span><span class="sxs-lookup"><span data-stu-id="665ee-153">hello project should appear in hello **Projects** list as a selected item.</span></span>
   
    ![][12]
3. <span data-ttu-id="665ee-154">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="665ee-154">Click **Finish**.</span></span>
4. <span data-ttu-id="665ee-155">Használjon **Project Explorer** tooview és Szerkesztés hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="665ee-155">Use **Project Explorer** tooview and edit hello files.</span></span> <span data-ttu-id="665ee-156">Ha még nincs nyitva, kattintson a **ablak** > **nézet megjelenítése** > **Project Explorer** vagy hello helyi tooopen használja azt.</span><span class="sxs-lookup"><span data-stu-id="665ee-156">If it's not already open, click **Window** > **Show View** > **Project Explorer** or use hello shortcut tooopen it.</span></span>

## <a name="configure-hello-service-url-and-api-key"></a><span data-ttu-id="665ee-157">Hello szolgáltatás URL-CÍMÉT és api-kulcsának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="665ee-157">Configure hello service URL and api-key</span></span>
1. <span data-ttu-id="665ee-158">A **Project Explorer**, kattintson duplán a **config.properties** tooedit hello konfigurációs beállításokat tartalmazó hello kiszolgálónevet és api-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="665ee-158">In **Project Explorer**, double-click **config.properties** tooedit hello configuration settings containing hello server name and api-key.</span></span>
2. <span data-ttu-id="665ee-159">Tekintse meg a cikkben korábban, ha Ön megtalálható hello szolgáltatás URL-CÍMÉT és api-kulcs hello toohello lépéseket [Azure Portal](https://portal.azure.com), akkor írja be a tooget hello értékek **config.properties**.</span><span class="sxs-lookup"><span data-stu-id="665ee-159">Refer toohello steps earlier in this article, where you found hello service URL and api-key in hello [Azure Portal](https://portal.azure.com), tooget hello values you will now enter into **config.properties**.</span></span>
3. <span data-ttu-id="665ee-160">A **config.properties**, "Api-kulcsot" cserélje hello api-kulcsot a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="665ee-160">In **config.properties**, replace "Api Key" with hello api-key for your service.</span></span> <span data-ttu-id="665ee-161">A következő szolgáltatás neve (hello hello URL-cím http://servicename.search.windows.net első összetevője) behelyettesíti "a szolgáltatás neve" hello ugyanazt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="665ee-161">Next, service name (hello first component of hello URL http://servicename.search.windows.net) replaces "service name" in hello same file.</span></span>
   
    ![][5]

## <a name="configure-hello-project-build-and-runtime-environments"></a><span data-ttu-id="665ee-162">Hello projekt, a build és a futtatókörnyezetek környezetek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="665ee-162">Configure hello project, build and runtime environments</span></span>
1. <span data-ttu-id="665ee-163">Az Eclipse Project Explorer, kattintson a jobb gombbal hello project > **tulajdonságok** > **Project Facets**.</span><span class="sxs-lookup"><span data-stu-id="665ee-163">In Eclipse, in Project Explorer, right-click hello project > **Properties** > **Project Facets**.</span></span>
2. <span data-ttu-id="665ee-164">Válassza ki a **Dynamic Web Module** (Dinamikus webmodul), a **Java** és a **JavaScript** elemet.</span><span class="sxs-lookup"><span data-stu-id="665ee-164">Select **Dynamic Web Module**, **Java**, and **JavaScript**.</span></span>
   
    ![][6]
3. <span data-ttu-id="665ee-165">Kattintson az **Apply** (Alkalmaz) gombra.</span><span class="sxs-lookup"><span data-stu-id="665ee-165">Click **Apply**.</span></span>
4. <span data-ttu-id="665ee-166">Válassza ki a **Window** (Ablak)  > **Preferences** (Beállítások)  > **Server** (Kiszolgáló)  > **Runtime Environments** (Futtatókörnyezetek)  > **Add..** (Hozzáadás) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="665ee-166">Select **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add..**.</span></span>
5. <span data-ttu-id="665ee-167">Bontsa ki az Apache, és válassza ki a korábban telepített hello Apache Tomcat kiszolgálót hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="665ee-167">Expand Apache and select hello version of hello Apache Tomcat server you previously installed.</span></span> <span data-ttu-id="665ee-168">Mi a 8-as verziót telepítettük a rendszerünkre.</span><span class="sxs-lookup"><span data-stu-id="665ee-168">On our system, we installed version 8.</span></span>
   
    ![][7]
6. <span data-ttu-id="665ee-169">Hello következő lapon adja meg a hello Tomcat telepítési könyvtárát.</span><span class="sxs-lookup"><span data-stu-id="665ee-169">On hello next page, specify hello Tomcat installation directory.</span></span> <span data-ttu-id="665ee-170">Windows rendszerű számítógépeken ez valószínűleg a következő lesz: C:\Program Files\Apache Software Foundation\Tomcat *verzió*.</span><span class="sxs-lookup"><span data-stu-id="665ee-170">On a Windows computer, this will most likely be C:\Program Files\Apache Software Foundation\Tomcat *version*.</span></span>
7. <span data-ttu-id="665ee-171">Kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="665ee-171">Click **Finish**.</span></span>
8. <span data-ttu-id="665ee-172">Válassza ki a **Window** (Ablak)  > **Preferences** (Beállítások)  > **Java** > **Installed JREs** (Telepített JRE-k)  > **Add** (Hozzáadás) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="665ee-172">Select **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.</span></span>
9. <span data-ttu-id="665ee-173">Az **Add JRE** (JRE hozzáadása) panelen válassza ki a **Standard VM** elemet.</span><span class="sxs-lookup"><span data-stu-id="665ee-173">In **Add JRE**, select **Standard VM**.</span></span>
10. <span data-ttu-id="665ee-174">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="665ee-174">Click **Next**.</span></span>
11. <span data-ttu-id="665ee-175">A JRE Definition (JRE_definíció) ablakban, a JRE kezdőlapján kattintson a **Directory** (Könyvtár) elemre.</span><span class="sxs-lookup"><span data-stu-id="665ee-175">In JRE Definition, in JRE home, click **Directory**.</span></span>
12. <span data-ttu-id="665ee-176">Keresse meg a túl**Program Files** > **Java** válassza ki a korábban telepített JDK hello.</span><span class="sxs-lookup"><span data-stu-id="665ee-176">Navigate too**Program Files** > **Java** and select hello JDK you previously installed.</span></span> <span data-ttu-id="665ee-177">Fontos tooselect hello JDK hello JRE szerint is.</span><span class="sxs-lookup"><span data-stu-id="665ee-177">It's important tooselect hello JDK as hello JRE.</span></span>
13. <span data-ttu-id="665ee-178">A telepített JREs, válassza a hello **JDK**.</span><span class="sxs-lookup"><span data-stu-id="665ee-178">In Installed JREs, choose hello **JDK**.</span></span> <span data-ttu-id="665ee-179">A beállítások a következő képernyőkép hasonló toohello kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="665ee-179">Your settings should look similar toohello following screenshot.</span></span>
    
    ![][9]
14. <span data-ttu-id="665ee-180">Bejelölheti **ablak** > **webböngésző** > **Internet Explorer** tooopen hello alkalmazást egy külső böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="665ee-180">Optionally, select **Window** > **Web Browser** > **Internet Explorer** tooopen hello application in an external browser window.</span></span> <span data-ttu-id="665ee-181">Külső böngészővel fokozhatja a webalkalmazás használatának élményét.</span><span class="sxs-lookup"><span data-stu-id="665ee-181">Using an external browser gives you a better Web application experience.</span></span>
    
    ![][8]

<span data-ttu-id="665ee-182">Hello konfigurációs feladatok most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="665ee-182">You have now completed hello configuration tasks.</span></span> <span data-ttu-id="665ee-183">A következő lesz hozza létre, és futtassa hello projektet.</span><span class="sxs-lookup"><span data-stu-id="665ee-183">Next, you'll build and run hello project.</span></span>

## <a name="build-hello-project"></a><span data-ttu-id="665ee-184">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="665ee-184">Build hello project</span></span>
1. <span data-ttu-id="665ee-185">A Project Explorer, kattintson a jobb gombbal a projekt neve hello, és válassza a **futtató** > **Maven build...**  tooconfigure hello projekt.</span><span class="sxs-lookup"><span data-stu-id="665ee-185">In Project Explorer, right-click hello project name and choose **Run As** > **Maven build...** tooconfigure hello project.</span></span>
   
    ![][10]
2. <span data-ttu-id="665ee-186">Az Edit Configuration (Konfiguráció szerkesztése) panelen a Goals (Célok) mezőbe írja be a „clean install” („tiszta telepítés”) kifejezést, majd kattintson a **Run** (Futtatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="665ee-186">In Edit Configuration, in Goals, type "clean install", and then click **Run**.</span></span>

<span data-ttu-id="665ee-187">Állapotüzenetek kimeneti toohello konzolablakban.</span><span class="sxs-lookup"><span data-stu-id="665ee-187">Status messages are output toohello console window.</span></span> <span data-ttu-id="665ee-188">BUILD SIKERESSÉGÉT jelző hello projektet egy hiba nélkül kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="665ee-188">You should see BUILD SUCCESS indicating hello project built without errors.</span></span>

## <a name="run-hello-app"></a><span data-ttu-id="665ee-189">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="665ee-189">Run hello app</span></span>
<span data-ttu-id="665ee-190">Ezen utolsó lépésében hello alkalmazás a helyi kiszolgáló futtatókörnyezetében fog futni.</span><span class="sxs-lookup"><span data-stu-id="665ee-190">In this last step, you will run hello application in a local server runtime environment.</span></span>

<span data-ttu-id="665ee-191">Ha az eclipse-ben még nem határozta meg egy kiszolgáló futtatókörnyezetét, toodo, először lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="665ee-191">If you haven't yet specified a server runtime environment in Eclipse, you'll need toodo that first.</span></span>

1. <span data-ttu-id="665ee-192">A Project Explorer (Projektböngésző) nézetben bontsa ki a **WebContent** elemet.</span><span class="sxs-lookup"><span data-stu-id="665ee-192">In Project Explorer, expand **WebContent**.</span></span>
2. <span data-ttu-id="665ee-193">Kattintson a jobb gombbal a **Search.jsp** fájlra, majd kattintson a  > **Run As** (Futtatás másként)  > **Run on Server** (Futtatás a kiszolgálón) elemre.</span><span class="sxs-lookup"><span data-stu-id="665ee-193">Right-click **Search.jsp** > **Run As** > **Run on Server**.</span></span> <span data-ttu-id="665ee-194">Válassza ki a hello Apache Tomcat kiszolgálót, és kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="665ee-194">Select hello Apache Tomcat server, and then click **Run**.</span></span>

> [!TIP]
> <span data-ttu-id="665ee-195">Ha egy nem alapértelmezett munkaterületen toostore a projekthez, szüksége lesz a toomodify **futtatása konfigurációs** toopoint toohello projekt helye tooavoid Kiszolgálóindítási hiba.</span><span class="sxs-lookup"><span data-stu-id="665ee-195">If you used a non-default workspace toostore your project, you'll need toomodify **Run Configuration** toopoint toohello project location tooavoid a server start-up error.</span></span> <span data-ttu-id="665ee-196">A Project Explorer (Projektböngésző) nézetben kattintson a jobb gombbal a **Search.jsp** fájlra, majd kattintson a  > **Run As** (Futtatás másként)  > **Run Configurations** (Konfigurációk futtatása) elemre.</span><span class="sxs-lookup"><span data-stu-id="665ee-196">In Project Explorer, right-click **Search.jsp** > **Run As** > **Run Configurations**.</span></span> <span data-ttu-id="665ee-197">Válassza ki a hello Apache Tomcat kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="665ee-197">Select hello Apache Tomcat server.</span></span> <span data-ttu-id="665ee-198">Kattintson az **Arguments** (Argumentumok) elemre.</span><span class="sxs-lookup"><span data-stu-id="665ee-198">Click **Arguments**.</span></span> <span data-ttu-id="665ee-199">Kattintson a **munkaterület** vagy **fájlrendszer** tooset hello csomagot hello projektet tartalmazó mappa.</span><span class="sxs-lookup"><span data-stu-id="665ee-199">Click **Workspace** or **File System** tooset hello folder containing hello project.</span></span>
> 
> 

<span data-ttu-id="665ee-200">Hello alkalmazás futtatásakor kell megjelennie a böngészőablakban feltételek megadása a keresőmező biztosítva.</span><span class="sxs-lookup"><span data-stu-id="665ee-200">When you run hello application, you should see a browser window, providing a search box for entering terms.</span></span>

<span data-ttu-id="665ee-201">Várjon körülbelül egy percet való kattintás előtt **keresési** toogive hello szolgáltatás idő és a toocreate hello indexet.</span><span class="sxs-lookup"><span data-stu-id="665ee-201">Wait about one minute before clicking **Search** toogive hello service time toocreate and load hello index.</span></span> <span data-ttu-id="665ee-202">HTTP 404 hibaüzenetet kap, ha csupán be kell toowait, majd próbálkozzon újra egy kicsit tovább.</span><span class="sxs-lookup"><span data-stu-id="665ee-202">If you get an HTTP 404 error, you just need toowait a little bit longer before trying again.</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="665ee-203">USGS-adatok keresése</span><span class="sxs-lookup"><span data-stu-id="665ee-203">Search on USGS data</span></span>
<span data-ttu-id="665ee-204">hello USGS-adatkészlet a Rhode Island államra vonatkozó toohello rekordokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="665ee-204">hello USGS data set includes records that are relevant toohello state of Rhode Island.</span></span> <span data-ttu-id="665ee-205">Ha **keresési** egy üres keresőmező kap hello 50 legfontosabb bejegyzés; hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="665ee-205">If you click **Search** on an empty search box, you will get hello top 50 entries, which is hello default.</span></span>

<span data-ttu-id="665ee-206">A keresett kifejezés beírása rendszerében hello keresőmotor valami toogo meg.</span><span class="sxs-lookup"><span data-stu-id="665ee-206">Entering a search term will give hello search engine something toogo on.</span></span> <span data-ttu-id="665ee-207">Próbáljon meg a helyhez kötődő nevet beírni.</span><span class="sxs-lookup"><span data-stu-id="665ee-207">Try entering a regional name.</span></span> <span data-ttu-id="665ee-208">"Roger Williams" volt Rhode Island első kormányzója hello.</span><span class="sxs-lookup"><span data-stu-id="665ee-208">"Roger Williams" was hello first governor of Rhode Island.</span></span> <span data-ttu-id="665ee-209">Számos parkot, épületet és iskolát neveztek el róla.</span><span class="sxs-lookup"><span data-stu-id="665ee-209">Numerous parks, buildings, and schools are named after him.</span></span>

![][11]

<span data-ttu-id="665ee-210">Megpróbálhatja beírni az alábbi kifejezések bármelyikét is:</span><span class="sxs-lookup"><span data-stu-id="665ee-210">You could also try any of these terms:</span></span>

* <span data-ttu-id="665ee-211">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="665ee-211">Pawtucket</span></span>
* <span data-ttu-id="665ee-212">Pembroke</span><span class="sxs-lookup"><span data-stu-id="665ee-212">Pembroke</span></span>
* <span data-ttu-id="665ee-213">goose+cape</span><span class="sxs-lookup"><span data-stu-id="665ee-213">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="665ee-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="665ee-214">Next steps</span></span>
<span data-ttu-id="665ee-215">Ez az hello Azure Search első oktatóanyaga Java és USGS-adatkészletet hello alapján.</span><span class="sxs-lookup"><span data-stu-id="665ee-215">This is hello first Azure Search tutorial based on Java and hello USGS dataset.</span></span> <span data-ttu-id="665ee-216">Idővel majd tovább bővítjük ezen oktatóanyag toodemonstrate kiegészítő keresési funkciókat, amelyeket esetleg szívesen toouse egyéni megoldásaiban.</span><span class="sxs-lookup"><span data-stu-id="665ee-216">Over time, we'll extend this tutorial toodemonstrate additional search features you might want toouse in your custom solutions.</span></span>

<span data-ttu-id="665ee-217">Ha már rendelkezik bizonyos tapasztalattal az Azure Search, használhatja ezt a mintát akár ugródeszkaként a további kísérletezéshez, például bővítheti a hello [keresőoldalt](search-pagination-page-layout.md), vagy a végrehajtási [jellemzőalapú navigációs](search-faceted-navigation.md).</span><span class="sxs-lookup"><span data-stu-id="665ee-217">If you already have some background in Azure Search, you can use this sample as a springboard for further experimentation, perhaps augmenting hello [search page](search-pagination-page-layout.md), or implementing [faceted navigation](search-faceted-navigation.md).</span></span> <span data-ttu-id="665ee-218">Akkor is tovább fejlesztheti hello keresési eredmények oldalát által számok és kötegelt dokumentumok, így a felhasználók lapozhassanak az eredmények hello hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="665ee-218">You can also improve upon hello search results page by adding counts and batching documents so that users can page through hello results.</span></span>

<span data-ttu-id="665ee-219">Új tooAzure keresési?</span><span class="sxs-lookup"><span data-stu-id="665ee-219">New tooAzure Search?</span></span> <span data-ttu-id="665ee-220">Azt javasoljuk, próbáljon más oktatóanyagok toodevelop megismerhesse, mit hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="665ee-220">We recommend trying other tutorials toodevelop an understanding of what you can create.</span></span> <span data-ttu-id="665ee-221">Látogasson el a [dokumentációs oldal](https://azure.microsoft.com/documentation/services/search/) toofind több erőforrást.</span><span class="sxs-lookup"><span data-stu-id="665ee-221">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) toofind more resources.</span></span> <span data-ttu-id="665ee-222">Hello hivatkozások is megtekintheti a [videók és oktatóanyagok listáját](search-video-demo-tutorial-list.md) tooaccess további információt.</span><span class="sxs-lookup"><span data-stu-id="665ee-222">You can also view hello links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) tooaccess more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
