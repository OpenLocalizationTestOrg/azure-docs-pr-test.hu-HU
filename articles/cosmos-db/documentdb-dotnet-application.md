---
title: "ASP.NET MVC oktatóprogram az Azure Cosmos DB szolgáltatáshoz: webalkalmazás-fejlesztés | Microsoft Docs"
description: "ASP.NET MVC oktatóprogram MVC webalkalmazás létrehozásához az Azure Cosmos DB szolgáltatással. A JSON-fájlok tárolása és az adatok elérése az Azure-webhelyeken tárolt teendőkezelő alkalmazásból történik – ASP NET MVC oktatóprogram lépésről lépésre."
keywords: "asp.net mvc oktatóanyag, webalkalmazás fejlesztése, mvc-webalkalmazás, asp net mvc lépésről lépésre haladó oktatóanyag"
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: mimig
ms.openlocfilehash: 3f2950fe25feb8f3ee81cc0a79bf624f0ee33bd5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <span data-ttu-id="bd981-105"><a name="_Toc395809351"></a>ASP.NET MVC oktatóprogram: webalkalmazás fejlesztése az Azure Cosmos DB szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="bd981-105"><a name="_Toc395809351"></a>ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bd981-106">.NET</span><span class="sxs-lookup"><span data-stu-id="bd981-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="bd981-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="bd981-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="bd981-108">Java</span><span class="sxs-lookup"><span data-stu-id="bd981-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="bd981-109">Python</span><span class="sxs-lookup"><span data-stu-id="bd981-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="bd981-110">Ez a cikk teljes körűen bemutatja, hogyan építhet teendőkezelő alkalmazást az Azure Cosmos DB eszközzel, és ezáltal hogyan használhatja hatékonyan az Azure Cosmos DB-t a JSON-dokumentumok tárolására és lekérdezésére.</span><span class="sxs-lookup"><span data-stu-id="bd981-110">To highlight how you can efficiently leverage Azure Cosmos DB to store and query JSON documents, this article provides an end-to-end walk-through showing you how to build a todo app using Azure Cosmos DB.</span></span> <span data-ttu-id="bd981-111">A feladatok JSON-dokumentumokként lesznek tárolva az Azure Cosmos DB-ben.</span><span class="sxs-lookup"><span data-stu-id="bd981-111">The tasks will be stored as JSON documents in Azure Cosmos DB.</span></span>

![Az oktatóprogram során létrehozott teendőlista-kezelő MVC webalkalmazás képernyőfelvétele – ASP NET MVC oktatóprogram lépésről lépésre](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image01.png)

<span data-ttu-id="bd981-113">Ez az útmutató bemutatja, hogyan tárolja az Azure Cosmos DB szolgáltatás használatára és Azure rendszeren üzemeltetett ASP.NET MVC webalkalmazás érheti el adatait.</span><span class="sxs-lookup"><span data-stu-id="bd981-113">This walk-through shows you how to use the Azure Cosmos DB service to store and access data from an ASP.NET MVC web application hosted on Azure.</span></span> <span data-ttu-id="bd981-114">Ha olyan oktatóprogramot keres, amely csak az Azure Cosmos DB szolgáltatással foglalkozik, az ASP.NET MVC összetevőkkel nem, akkor tekintse meg: [Azure Cosmos DB C# konzolalkalmazás felépítése](documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bd981-114">If you're looking for a tutorial that focuses only on Azure Cosmos DB, and not the ASP.NET MVC components, see [Build an Azure Cosmos DB C# console application](documentdb-get-started.md).</span></span>

> [!TIP]
> <span data-ttu-id="bd981-115">Ez az oktatóprogram feltételezi, hogy van korábbi tapasztalata az ASP.NET MVC és az Azure webhelyek használatában.</span><span class="sxs-lookup"><span data-stu-id="bd981-115">This tutorial assumes that you have prior experience using ASP.NET MVC and Azure Websites.</span></span> <span data-ttu-id="bd981-116">Ha nem ismeri az ASP.NET rendszert vagy az [előfeltételt jelentő eszközöket](#_Toc395637760), érdemes letöltenie a teljes mintaprojektet a [GitHubról][GitHub], és követni a mintában lévő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="bd981-116">If you are new to ASP.NET or the [prerequisite tools](#_Toc395637760), we recommend downloading the complete sample project from [GitHub][GitHub] and following the instructions in this sample.</span></span> <span data-ttu-id="bd981-117">Ha felépítette, ezen cikk áttekintésével betekintést nyerhet a kódba a projekt környezetében.</span><span class="sxs-lookup"><span data-stu-id="bd981-117">Once you have it built, you can review this article to gain insight on the code in the context of the project.</span></span>
> 
> 

## <span data-ttu-id="bd981-118"><a name="_Toc395637760"></a>Az adatbázis-oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="bd981-118"><a name="_Toc395637760"></a>Prerequisites for this database tutorial</span></span>
<span data-ttu-id="bd981-119">A jelen cikkben lévő utasítások követése előtt rendelkeznie kell a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="bd981-119">Before following the instructions in this article, you should ensure that you have the following:</span></span>

* <span data-ttu-id="bd981-120">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="bd981-120">An active Azure account.</span></span> <span data-ttu-id="bd981-121">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="bd981-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bd981-122">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd981-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

    <span data-ttu-id="bd981-123">VAGY</span><span class="sxs-lookup"><span data-stu-id="bd981-123">OR</span></span>

    <span data-ttu-id="bd981-124">Az [Azure Cosmos DB Emulator](local-emulator.md) helyi telepítése.</span><span class="sxs-lookup"><span data-stu-id="bd981-124">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="bd981-125">[A Visual Studio 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="bd981-125">[Visual Studio 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="bd981-126">A Microsoft Azure SDK for .NET for Visual Studio 2017, a Visual Studio telepítő keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="bd981-126">Microsoft Azure SDK for .NET for Visual Studio 2017, available through the Visual Studio Installer.</span></span>

<span data-ttu-id="bd981-127">Ez a cikk összes képernyőfelvétele használatával a Microsoft Visual Studio Community 2017 került sor.</span><span class="sxs-lookup"><span data-stu-id="bd981-127">All the screen shots in this article have been taken using Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="bd981-128">Ha a rendszer a lehetséges, hogy a képernyők és beállítások nem egyeznek tökéletesen, de ha megfelel a fenti előfeltételeknek ebben a megoldásban kell működnie egy másik verzió van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="bd981-128">If your system is configured with a different version it is possible that your screens and options won't match entirely, but if you meet the above prerequisites this solution should work.</span></span>

## <span data-ttu-id="bd981-129"><a name="_Toc395637761"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd981-129"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="bd981-130">Először hozzon létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="bd981-130">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="bd981-131">Ha már szerepel egy SQL (DocumentDB) fiók Azure Cosmos DB, vagy ha az oktatóanyag az Azure Cosmos DB Emulator használ, továbbléphet a [hozzon létre egy új ASP.NET MVC alkalmazást](#_Toc395637762).</span><span class="sxs-lookup"><span data-stu-id="bd981-131">If you already have a SQL (DocumentDB) account for Azure Cosmos DB or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Create a new ASP.NET MVC application](#_Toc395637762).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
<span data-ttu-id="bd981-132">Most végigvezetjük azon, hogyan hozhat létre új ASP.NET MVC alkalmazást az alapoktól.</span><span class="sxs-lookup"><span data-stu-id="bd981-132">We will now walk through how to create a new ASP.NET MVC application from the ground-up.</span></span> 

## <span data-ttu-id="bd981-133"><a name="_Toc395637762"></a>2. lépés: Új ASP.NET MVC alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd981-133"><a name="_Toc395637762"></a>Step 2: Create a new ASP.NET MVC application</span></span>

1. <span data-ttu-id="bd981-134">A Visual Studio programban, a **File** (Fájl) menüben mutasson a **New** (Új) elemre, majd kattintson a **Project** (Projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="bd981-134">In Visual Studio, on the **File** menu, point to **New**, and then click **Project**.</span></span> <span data-ttu-id="bd981-135">Megjelenik a **New project** (Új projekt) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bd981-135">The **New Project** dialog box appears.</span></span>

2. <span data-ttu-id="bd981-136">A **Project types** (Projekttípusok) panelen bontsa ki a **Templates** (Sablonok), **Visual C#**, **Web** elemeket, majd válassza az **ASP.NET Web Application** (ASP.NET webalkalmazás) elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-136">In the **Project types** pane, expand **Templates**, **Visual C#**, **Web**, and then select **ASP.NET Web Application**.</span></span>

      ![Képernyőfelvétel a New Project (Új projekt) párbeszédpanelről, ahol az ASP.NET webalkalmazás projekttípus van kijelölve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. <span data-ttu-id="bd981-138">A **Name** (Név) szövegmezőbe írja be a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="bd981-138">In the **Name** box, type the name of the project.</span></span> <span data-ttu-id="bd981-139">Ez az oktatóprogram a „todo” (teendők) nevet használja.</span><span class="sxs-lookup"><span data-stu-id="bd981-139">This tutorial uses the name "todo".</span></span> <span data-ttu-id="bd981-140">Ha más nevet választ, akkor amikor az oktatóprogram a „todo” (teendők) névteréről beszél, akkor a megadott kódmintákat úgy kell módosítania, hogy az alkalmazás tényleges nevét használja.</span><span class="sxs-lookup"><span data-stu-id="bd981-140">If you choose to use something other than this, then wherever this tutorial talks about the todo namespace, you need to adjust the provided code samples to use whatever you named your application.</span></span> 
4. <span data-ttu-id="bd981-141">Kattintson a **Browse** (Böngészés) gombra azon mappa megkereséséhez, ahol létre szeretné hozni a projektet, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-141">Click **Browse** to navigate to the folder where you would like to create the project, and then click **OK**.</span></span>
   
      <span data-ttu-id="bd981-142">A **új ASP.NET-webalkalmazás** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bd981-142">The **New ASP.NET Web Application** dialog box appears.</span></span>
   
    ![Az MVC alkalmazássablon van kiemelve az új ASP.NET Webalkalmazásként való kezelése párbeszédpanel képernyőképe](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. <span data-ttu-id="bd981-144">A sablonok panelén válassza az **MVC** elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-144">In the templates pane, select **MVC**.</span></span>

6. <span data-ttu-id="bd981-145">Kattintson az **OK** gombra, és várja meg, hogy a Visual Studio kialakítsa a szerkezetet az üres ASP.NET MVC sablonban.</span><span class="sxs-lookup"><span data-stu-id="bd981-145">Click **OK** and let Visual Studio do its thing around scaffolding the empty ASP.NET MVC template.</span></span> 

          
7. <span data-ttu-id="bd981-146">Ha a Visual Studio befejezte a sablonszöveges MVC alkalmazás létrehozását, egy üres ASP.NET alkalmazást kap, amelyet helyileg futtathat.</span><span class="sxs-lookup"><span data-stu-id="bd981-146">Once Visual Studio has finished creating the boilerplate MVC application you have an empty ASP.NET application that you can run locally.</span></span>
   
    <span data-ttu-id="bd981-147">Kihagyjuk a projekt helyi futtatását, mert biztosan mindannyian láttuk az ASP.NET „Hello World” alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bd981-147">We'll skip running the project locally because I'm sure we've all seen the ASP.NET "Hello World" application.</span></span> <span data-ttu-id="bd981-148">Ugorjunk közvetlenül az Azure Cosmos DB ezen projekthez való hozzáadására és az alkalmazás felépítésére.</span><span class="sxs-lookup"><span data-stu-id="bd981-148">Let's go straight to adding Azure Cosmos DB to this project and building our application.</span></span>

## <span data-ttu-id="bd981-149"><a name="_Toc395637767"></a>3. lépés: Azure Cosmos DB hozzáadása az MVC webalkalmazás projekthez</span><span class="sxs-lookup"><span data-stu-id="bd981-149"><a name="_Toc395637767"></a>Step 3: Add Azure Cosmos DB to your MVC web application project</span></span>
<span data-ttu-id="bd981-150">Most, hogy rendelkezünk a megoldáshoz szükséges ASP.NET MVC bekötések nagy részével, folytassuk az oktatóprogram valódi céljával, amely az Azure Cosmos DB MVC webalkalmazáshoz adása.</span><span class="sxs-lookup"><span data-stu-id="bd981-150">Now that we have most of the ASP.NET MVC plumbing that we need for this solution, let's get to the real purpose of this tutorial, adding Azure Cosmos DB to our MVC web application.</span></span>

1. <span data-ttu-id="bd981-151">Az Azure Cosmos DB .NET SDK csomagolt és a NuGet-csomag terjesztése.</span><span class="sxs-lookup"><span data-stu-id="bd981-151">The Azure Cosmos DB .NET SDK is packaged and distributed as a NuGet package.</span></span> <span data-ttu-id="bd981-152">A Visual Studióban a NuGet-csomag beszerzéséhez használja a Visual Studio NuGet-csomagkezelőjét. Ehhez kattintson a jobb gombbal a projektre a **Megoldáskezelőben**, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="bd981-152">To get the NuGet package in Visual Studio, use the NuGet package manager in Visual Studio by right-clicking on the project in **Solution Explorer** and then clicking **Manage NuGet Packages**.</span></span>
   
    ![A Megoldáskezelőben a webalkalmazás projekt helyi menüjének képernyőfelvétele, ahol a Manage NuGet Packages (NuGet-csomagok kezelése) parancs van kiemelve.](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    <span data-ttu-id="bd981-154">Megjelenik a **Manage NuGet Packages** (NuGet-csomagok kezelése) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bd981-154">The **Manage NuGet Packages** dialog box appears.</span></span>
2. <span data-ttu-id="bd981-155">A NuGet **Browse** (Tallózás) mezőjébe írja be az ***Azure DocumentDB*** szöveget.</span><span class="sxs-lookup"><span data-stu-id="bd981-155">In the NuGet **Browse** box, type ***Azure DocumentDB***.</span></span> <span data-ttu-id="bd981-156">(A csomag neve nem frissült az Azure Cosmos-Adatbázishoz.)</span><span class="sxs-lookup"><span data-stu-id="bd981-156">(The package name has not been updated to Azure Cosmos DB.)</span></span>
   
    <span data-ttu-id="bd981-157">Az eredmények közül telepítse a **Microsoft Microsoft.Azure.DocumentDB** csomag.</span><span class="sxs-lookup"><span data-stu-id="bd981-157">From the results, install the **Microsoft.Azure.DocumentDB by Microsoft** package.</span></span> <span data-ttu-id="bd981-158">Ez letölti és telepíti az Azure Cosmos DB csomagot, valamint az összes függőségét, például a newtonsoft.JSON elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-158">This will download and install the Azure Cosmos DB package as well as all dependencies, such as Newtonsoft.Json.</span></span> <span data-ttu-id="bd981-159">Kattintson az **OK** gombra a **Preview** (Előnézet) ablakban, majd az **I Accept** (Elfogadás) gombra a **License Acceptance** (Licenc elfogadása) ablakban a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bd981-159">Click **OK** in the **Preview** window, and **I Accept** in the **License Acceptance** window to complete the install.</span></span>
   
    ![A Manage NuGet Packages (NuGet-csomagok kezelése) ablak képernyőfelvétele, ahol a Microsoft Azure DocumentDB Client Library elem van kiemelve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      <span data-ttu-id="bd981-161">A Csomagkezelő konzollal is telepítheti a csomagot.</span><span class="sxs-lookup"><span data-stu-id="bd981-161">Alternatively you can use the Package Manager Console to install the package.</span></span> <span data-ttu-id="bd981-162">Ehhez a **Tools** (Eszközök) menüben kattintson a **NuGet Package Manager** (NuGet-csomagkezelő) elemre, majd kattintson a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="bd981-162">To do so, on the **Tools** menu, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span> <span data-ttu-id="bd981-163">A parancssorba írja be a következőt.</span><span class="sxs-lookup"><span data-stu-id="bd981-163">At the prompt, type the following.</span></span>
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. <span data-ttu-id="bd981-164">A csomag telepítése után a Visual Studio megoldásnak a következőre kell hasonlítania két hozzáadott hivatkozással: Microsoft.Azure.Documents.Client és Newtonsoft.Json.</span><span class="sxs-lookup"><span data-stu-id="bd981-164">Once the package is installed, your Visual Studio solution should resemble the following with two new references added, Microsoft.Azure.Documents.Client and Newtonsoft.Json.</span></span>
   
    ![A Megoldáskezelőben a JSON adatprojekthez adott két hivatkozás képernyőfelvétele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <span data-ttu-id="bd981-166"><a name="_Toc395637763"></a>4. lépés: Az ASP.NET MVC alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="bd981-166"><a name="_Toc395637763"></a>Step 4: Set up the ASP.NET MVC application</span></span>
<span data-ttu-id="bd981-167">Most adjuk hozzá a modelleket, a nézeteket és a vezérlőket ehhez az MVC alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="bd981-167">Now let's add the models, views, and controllers to this MVC application:</span></span>

* <span data-ttu-id="bd981-168">[Modell hozzáadása](#_Toc395637764).</span><span class="sxs-lookup"><span data-stu-id="bd981-168">[Add a model](#_Toc395637764).</span></span>
* <span data-ttu-id="bd981-169">[Vezérlő hozzáadása](#_Toc395637765).</span><span class="sxs-lookup"><span data-stu-id="bd981-169">[Add a controller](#_Toc395637765).</span></span>
* <span data-ttu-id="bd981-170">[Nézetek hozzáadása](#_Toc395637766).</span><span class="sxs-lookup"><span data-stu-id="bd981-170">[Add views](#_Toc395637766).</span></span>

### <span data-ttu-id="bd981-171"><a name="_Toc395637764"></a>JSON adatmodell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd981-171"><a name="_Toc395637764"></a>Add a JSON data model</span></span>
<span data-ttu-id="bd981-172">Először hozzuk létre az **M-et** az MVC-ből, a modellt.</span><span class="sxs-lookup"><span data-stu-id="bd981-172">Let's begin by creating the **M** in MVC, the model.</span></span> 

1. <span data-ttu-id="bd981-173">A **Megoldáskezelőben** kattintson a jobb gombbal a **Models** (Modellek) mappára, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-173">In **Solution Explorer**, right-click the **Models** folder, click **Add**, and then click **Class**.</span></span>
   
      <span data-ttu-id="bd981-174">Megjelenik az **Add New Item** (Új elem hozzáadása) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bd981-174">The **Add New Item** dialog box appears.</span></span>
2. <span data-ttu-id="bd981-175">Adja az új osztálynak az **Item.cs** nevet, és kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-175">Name your new class **Item.cs** and click **Add**.</span></span> 
3. <span data-ttu-id="bd981-176">Ebben az új **Item.cs** fájlban adja hozzá a következőket az utolsó *használati utasítás* után.</span><span class="sxs-lookup"><span data-stu-id="bd981-176">In this new **Item.cs** file, add the following after the last *using statement*.</span></span>
   
        using Newtonsoft.Json;
4. <span data-ttu-id="bd981-177">Most cserélje le ezt a kódot</span><span class="sxs-lookup"><span data-stu-id="bd981-177">Now replace this code</span></span> 
   
        public class Item
        {
        }
   
    <span data-ttu-id="bd981-178">a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="bd981-178">with the following code.</span></span>
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    <span data-ttu-id="bd981-179">Az Azure Cosmos DB összes adata átkerül a hálózaton keresztül, és JSON-fájlként lesz tárolva.</span><span class="sxs-lookup"><span data-stu-id="bd981-179">All data in Azure Cosmos DB is passed over the wire and stored as JSON.</span></span> <span data-ttu-id="bd981-180">Az objektumok JSON.NET általi szerializálási/deszerializálási módjának beállításához használhatja a **JsonProperty** attribútumot, ahogyan az az imént létrehozott **Item** (Elem) osztályban látható.</span><span class="sxs-lookup"><span data-stu-id="bd981-180">To control the way your objects are serialized/deserialized by JSON.NET you can use the **JsonProperty** attribute as demonstrated in the **Item** class we just created.</span></span> <span data-ttu-id="bd981-181">Nem **kell** ezt csinálnia, de biztosítani szeretném, hogy a tulajdonságaim követik a JSON camelCase elnevezési konvenciókat.</span><span class="sxs-lookup"><span data-stu-id="bd981-181">You don't **have** to do this but I want to ensure that my properties follow the JSON camelCase naming conventions.</span></span> 
   
    <span data-ttu-id="bd981-182">Nem csak a tulajdonságnév formátumát vezérelheti, amikor a JSON-ba kerül, hanem teljesen át is nevezheti a .NET tulajdonságokat, mint ahogyan a **Description** (Leírás) tulajdonsággal tettem.</span><span class="sxs-lookup"><span data-stu-id="bd981-182">Not only can you control the format of the property name when it goes into JSON, but you can entirely rename your .NET properties like I did with the **Description** property.</span></span> 

### <span data-ttu-id="bd981-183"><a name="_Toc395637765"></a>Vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd981-183"><a name="_Toc395637765"></a>Add a controller</span></span>
<span data-ttu-id="bd981-184">Ezzel megvagyunk az **M-mel**, most hozzuk létre az MVC **C-jét**, amely vezérlőosztály.</span><span class="sxs-lookup"><span data-stu-id="bd981-184">That takes care of the **M**, now let's create the **C** in MVC, a controller class.</span></span>

1. <span data-ttu-id="bd981-185">A **Megoldáskezelőben** kattintson a jobb gombbal a **Controllers** (Vezérlők) mappára, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Controller** (Vezérlő) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-185">In **Solution Explorer**, right-click the **Controllers** folder, click **Add**, and then click **Controller**.</span></span>
   
    <span data-ttu-id="bd981-186">Megjelenik az **Add Scaffold** (Szerkezet hozzáadása) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bd981-186">The **Add Scaffold** dialog box appears.</span></span>
2. <span data-ttu-id="bd981-187">Válassza az **MVC 5 Controller - Empty** (MVC 5 vezérlő - Üres) elemet, majd kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-187">Select **MVC 5 Controller - Empty** and then click **Add**.</span></span>
   
    ![Az Add Scaffold (Szerkezet hozzáadása) párbeszédpanel képernyőfelvétele, ahol ki van jelölve az MVC 5 Controller - Empty (MVC 5 vezérlő - Üres) lehetőség](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. <span data-ttu-id="bd981-189">Adja az **ItemController** nevet az új vezérlőnek.</span><span class="sxs-lookup"><span data-stu-id="bd981-189">Name your new Controller, **ItemController.**</span></span>
   
    ![Az Add Controller (Vezérlő hozzáadása) párbeszédpanel képernyőfelvétele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    <span data-ttu-id="bd981-191">A fájl létrehozása után a Visual Studio megoldásnak a következőre kell hasonlítania az új ItemController.cs fájllal a **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="bd981-191">Once the file is created, your Visual Studio solution should resemble the following with the new ItemController.cs file in **Solution Explorer**.</span></span> <span data-ttu-id="bd981-192">A korábban létrehozott új Item.cs fájl is látható.</span><span class="sxs-lookup"><span data-stu-id="bd981-192">The new Item.cs file created earlier is also shown.</span></span>
   
    ![A Visual Studio megoldás - Megoldáskezelő képernyőfelvétele, ahol ki van emelve az új ItemController.cs és Item.cs fájl](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    <span data-ttu-id="bd981-194">Bezárhatja az ItemController.cs fájlt, később visszatérünk ahhoz.</span><span class="sxs-lookup"><span data-stu-id="bd981-194">You can close ItemController.cs, we'll come back to it later.</span></span> 

### <span data-ttu-id="bd981-195"><a name="_Toc395637766"></a>Nézetek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd981-195"><a name="_Toc395637766"></a>Add views</span></span>
<span data-ttu-id="bd981-196">Most hozzuk létre az MVC **V** elemét, a nézeteket:</span><span class="sxs-lookup"><span data-stu-id="bd981-196">Now, let's create the **V** in MVC, the views:</span></span>

* <span data-ttu-id="bd981-197">[Elemindexnézet hozzáadása](#AddItemIndexView).</span><span class="sxs-lookup"><span data-stu-id="bd981-197">[Add an Item Index view](#AddItemIndexView).</span></span>
* <span data-ttu-id="bd981-198">[Új elemnézet hozzáadása](#AddNewIndexView).</span><span class="sxs-lookup"><span data-stu-id="bd981-198">[Add a New Item view](#AddNewIndexView).</span></span>
* <span data-ttu-id="bd981-199">[Elemszerkesztési nézet hozzáadása](#_Toc395888515).</span><span class="sxs-lookup"><span data-stu-id="bd981-199">[Add an Edit Item view](#_Toc395888515).</span></span>

#### <span data-ttu-id="bd981-200"><a name="AddItemIndexView"></a>Elemindexnézet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd981-200"><a name="AddItemIndexView"></a>Add an Item Index view</span></span>
1. <span data-ttu-id="bd981-201">A **Megoldáskezelőben** bontsa ki a **Nézetek** mappát, kattintson a jobb gombbal az üres **Elem** mappára, amelyet a Visual Studio az **ItemController** korábbi hozzáadásakor hozott létre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **View** (Nézet) elemre.</span><span class="sxs-lookup"><span data-stu-id="bd981-201">In **Solution Explorer**, expand the **Views**  folder, right-click the empty **Item** folder that Visual Studio created for you when you added the **ItemController** earlier, click **Add**, and then click **View**.</span></span>
   
    ![A Megoldáskezelő képernyőfelvétele, amelyen a Visual Studio által létrehozott Item mappa látható, és az Add View (Nézet hozzáadása) parancsok vannak kiemelve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. <span data-ttu-id="bd981-203">Az **Add View** (Nézet hozzáadása) párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bd981-203">In the **Add View** dialog box, do the following:</span></span>
   
   * <span data-ttu-id="bd981-204">A **View name** (Nézet neve) mezőbe írja be az ***Index*** nevet.</span><span class="sxs-lookup"><span data-stu-id="bd981-204">In the **View name** box, type ***Index***.</span></span>
   * <span data-ttu-id="bd981-205">A **Template** (Sablon) mezőben válassza a ***List*** (Lista) elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-205">In the **Template** box, select ***List***.</span></span>
   * <span data-ttu-id="bd981-206">A **Model class** (Modellosztály) mezőben válassza ki az ***Item (todo.Models)*** elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-206">In the **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="bd981-207">A layout page (elrendezéslap) mezőbe írja be a ***~/Views/Shared/_Layout.cshtml*** szöveget.</span><span class="sxs-lookup"><span data-stu-id="bd981-207">In the layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
     
   ![Az Add View (Nézet hozzáadása) párbeszédpanelt megjelenítő képernyőfelvétel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. <span data-ttu-id="bd981-209">Amikor ezen értékek mindegyike már be van állítva, kattintson az **Add** (Hozzáadás) gombra és várja meg, hogy a Visual Studio létrehozzon egy új sablonnézetet.</span><span class="sxs-lookup"><span data-stu-id="bd981-209">Once all these values are set, click **Add** and let Visual Studio create a new template view.</span></span> <span data-ttu-id="bd981-210">Ha ezzel végzett, a rendszer megnyitja a létrehozott cshtml fájlt.</span><span class="sxs-lookup"><span data-stu-id="bd981-210">Once it is done, it will open the cshtml file  that was created.</span></span> <span data-ttu-id="bd981-211">Bezárhatjuk ezt a fájlt a Visual Studióban, mivel később visszatérünk hozzá.</span><span class="sxs-lookup"><span data-stu-id="bd981-211">We can close that file in Visual Studio as we will come back to it later.</span></span>

#### <span data-ttu-id="bd981-212"><a name="AddNewIndexView"></a>Új elemnézet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd981-212"><a name="AddNewIndexView"></a>Add a New Item view</span></span>
<span data-ttu-id="bd981-213">Az **Elemindex** nézet létrehozásához hasonlóan most létrehozunk egy új nézetet új **elemek** létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="bd981-213">Similar to how we created an **Item Index** view, we will now create a new view for creating new **Items**.</span></span>

1. <span data-ttu-id="bd981-214">A **Megoldáskezelőben** ismét kattintson a jobb gombbal az **Item** (Elem) mappára, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **View** (Nézet) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-214">In **Solution Explorer**, right-click the **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="bd981-215">Az **Add View** (Nézet hozzáadása) párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bd981-215">In the **Add View** dialog box, do the following:</span></span>
   
   * <span data-ttu-id="bd981-216">A **View name** (Nézet neve) mezőbe írja be a ***Create*** (Létrehozás) nevet.</span><span class="sxs-lookup"><span data-stu-id="bd981-216">In the **View name** box, type ***Create***.</span></span>
   * <span data-ttu-id="bd981-217">A **Template** (Sablon) mezőben válassza a ***Create*** (Létrehozás) elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-217">In the **Template** box, select ***Create***.</span></span>
   * <span data-ttu-id="bd981-218">A **Model class** (Modellosztály) mezőben válassza ki az ***Item (todo.Models)*** elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-218">In the **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="bd981-219">A layout page (elrendezéslap) mezőbe írja be a ***~/Views/Shared/_Layout.cshtml*** szöveget.</span><span class="sxs-lookup"><span data-stu-id="bd981-219">In the layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="bd981-220">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="bd981-220">Click **Add**.</span></span>
   
#### <span data-ttu-id="bd981-221"><a name="_Toc395888515"></a>Elemszerkesztési nézet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd981-221"><a name="_Toc395888515"></a>Add an Edit Item view</span></span>
<span data-ttu-id="bd981-222">És végül adjon hozzá egy utolsó nézetet az **elemek** szerkesztéséhez, ahogyan azt korábban is tette.</span><span class="sxs-lookup"><span data-stu-id="bd981-222">And finally, add one last view for editing an **Item** in the same way as before.</span></span>

1. <span data-ttu-id="bd981-223">A **Megoldáskezelőben** ismét kattintson a jobb gombbal az **Item** (Elem) mappára, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **View** (Nézet) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-223">In **Solution Explorer**, right-click the **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="bd981-224">Az **Add View** (Nézet hozzáadása) párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bd981-224">In the **Add View** dialog box, do the following:</span></span>
   
   * <span data-ttu-id="bd981-225">A **View name** (Nézet neve) mezőbe írja be az ***Edit*** (Szerkesztés) nevet.</span><span class="sxs-lookup"><span data-stu-id="bd981-225">In the **View name** box, type ***Edit***.</span></span>
   * <span data-ttu-id="bd981-226">A **Template** (Sablon) mezőben válassza az ***Edit*** (Szerkesztés) elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-226">In the **Template** box, select ***Edit***.</span></span>
   * <span data-ttu-id="bd981-227">A **Model class** (Modellosztály) mezőben válassza ki az ***Item (todo.Models)*** elemet.</span><span class="sxs-lookup"><span data-stu-id="bd981-227">In the **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="bd981-228">A layout page (elrendezéslap) mezőbe írja be a ***~/Views/Shared/_Layout.cshtml*** szöveget.</span><span class="sxs-lookup"><span data-stu-id="bd981-228">In the layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="bd981-229">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="bd981-229">Click **Add**.</span></span>

<span data-ttu-id="bd981-230">Ha ezzel végzett, zárja be az összes cshtml dokumentumot a Visual Studióban, mivel később vissza fog térni ezekhez a nézetekhez.</span><span class="sxs-lookup"><span data-stu-id="bd981-230">Once this is done, close all the cshtml documents in Visual Studio as we will return to these views later.</span></span>

## <span data-ttu-id="bd981-231"><a name="_Toc395637769"></a>5. lépés: Az Azure Cosmos DB csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="bd981-231"><a name="_Toc395637769"></a>Step 5: Wiring up Azure Cosmos DB</span></span>
<span data-ttu-id="bd981-232">Most, hogy elvégeztük az MVC-vel kapcsolatos szokásos feladatokat, adjuk hozzá az Azure Cosmos DB kódját.</span><span class="sxs-lookup"><span data-stu-id="bd981-232">Now that the standard MVC stuff is taken care of, let's turn to adding the code for Azure Cosmos DB.</span></span> 

<span data-ttu-id="bd981-233">Ebben a szakaszban a következők kezeléséhez adunk hozzá kódot:</span><span class="sxs-lookup"><span data-stu-id="bd981-233">In this section, we'll add code to handle the following:</span></span>

* <span data-ttu-id="bd981-234">[Hiányos elemek listázása](#_Toc395637770).</span><span class="sxs-lookup"><span data-stu-id="bd981-234">[Listing incomplete Items](#_Toc395637770).</span></span>
* <span data-ttu-id="bd981-235">[Elemek hozzáadása](#_Toc395637771).</span><span class="sxs-lookup"><span data-stu-id="bd981-235">[Adding Items](#_Toc395637771).</span></span>
* <span data-ttu-id="bd981-236">[Elemek szerkesztése](#_Toc395637772).</span><span class="sxs-lookup"><span data-stu-id="bd981-236">[Editing Items](#_Toc395637772).</span></span>

### <span data-ttu-id="bd981-237"><a name="_Toc395637770"></a>Hiányos elemek listázása az MVC webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="bd981-237"><a name="_Toc395637770"></a>Listing incomplete Items in your MVC web application</span></span>
<span data-ttu-id="bd981-238">Itt először hozzá kell adni egy osztályt, amely tartalmazza az Azure Cosmos DB-adatbázishoz való csatlakozás és a DocumentDB használatának összes logikáját.</span><span class="sxs-lookup"><span data-stu-id="bd981-238">The first thing to do here is add a class that contains all the logic to connect to and use Azure Cosmos DB.</span></span> <span data-ttu-id="bd981-239">Ehhez az oktatóprogramhoz ezen logikák mindegyikét a DocumentDBRepository nevű adattárba foglaljuk.</span><span class="sxs-lookup"><span data-stu-id="bd981-239">For this tutorial we'll encapsulate all this logic in to a repository class called DocumentDBRepository.</span></span> 

1. <span data-ttu-id="bd981-240">A **Megoldáskezelőben** kattintson a jobb gombbal a projektre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-240">In **Solution Explorer**, right-click on the project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="bd981-241">Adja az új osztálynak a **DocumentDBRepository** nevet, és kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd981-241">Name the new class **DocumentDBRepository** and click **Add**.</span></span>
2. <span data-ttu-id="bd981-242">Az újonnan létrehozott **DocumentDBRepository** osztályban adja a következő *használati utasításokat* a *névtér-deklaráció* fölé</span><span class="sxs-lookup"><span data-stu-id="bd981-242">In the newly created **DocumentDBRepository** class and add the following *using statements* above the *namespace* declaration</span></span>
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    <span data-ttu-id="bd981-243">Most cserélje le ezt a kódot</span><span class="sxs-lookup"><span data-stu-id="bd981-243">Now replace this code</span></span> 
   
        public class DocumentDBRepository
        {
        }
   
    <span data-ttu-id="bd981-244">a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="bd981-244">with the following code.</span></span>
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
    
3. <span data-ttu-id="bd981-245">A konfigurációból adunk hozzá néhány értéket, ezért nyissa meg az alkalmazás **Web.config** fájlját, és adja hozzá a következő sorokat az `<AppSettings>` szakasz alá.</span><span class="sxs-lookup"><span data-stu-id="bd981-245">We're reading some values from configuration, so open the **Web.config** file of your application and add the following lines under the `<AppSettings>` section.</span></span>
   
        <add key="endpoint" value="enter the URI from the Keys blade of the Azure Portal"/>
        <add key="authKey" value="enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. <span data-ttu-id="bd981-246">Most frissítse az *endpoint* (végpont) és az *authKey* (hitelesítési kulcs) értékeit az Azure-portál Keys (Kulcsok) panelén.</span><span class="sxs-lookup"><span data-stu-id="bd981-246">Now, update the values for *endpoint* and *authKey* using the Keys blade of the Azure Portal.</span></span> <span data-ttu-id="bd981-247">A Keys (Kulcsok) panel **URI-címét** használja a végpontbeállítás értékeként, és a Keys (Kulcsok) panel **PRIMARY KEY** (ELSŐDLEGES KULCS) vagy **SECONDARY KEY** (MÁSODLAGOS KULCS) értékét használja az authKey beállítás értékeként.</span><span class="sxs-lookup"><span data-stu-id="bd981-247">Use the **URI** from the Keys blade as the value of the endpoint setting, and use the **PRIMARY KEY**, or **SECONDARY KEY** from the Keys blade as the value of the authKey setting.</span></span>

    <span data-ttu-id="bd981-248">Hogy vesz gondot elvégeztük a Azure Cosmos DB-tárházban, most adjuk hozzá az alkalmazás logikáját.</span><span class="sxs-lookup"><span data-stu-id="bd981-248">That takes care of wiring up the Azure Cosmos DB repository, now let's add our application logic.</span></span>

1. <span data-ttu-id="bd981-249">Először is meg szeretnénk tudni jeleníteni a hiányos elemeket a teendőlista alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="bd981-249">The first thing we want to be able to do with a todo list application is to display the incomplete items.</span></span>  <span data-ttu-id="bd981-250">Másolja és illessze be a következő kódrészletet bárhová a **DocumentDBRepository** osztályban.</span><span class="sxs-lookup"><span data-stu-id="bd981-250">Copy and paste the following code snippet anywhere within the **DocumentDBRepository** class.</span></span>
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. <span data-ttu-id="bd981-251">Nyissa meg a korábban hozzáadott **ItemController** elemet, és adja a következő *használati utasításokat* a névtér-deklaráció fölé</span><span class="sxs-lookup"><span data-stu-id="bd981-251">Open the **ItemController** we added earlier and add the following *using statements* above the namespace declaration.</span></span>
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    <span data-ttu-id="bd981-252">Ha a projekt neve nem „todo” (teendők), akkor frissítenie kell a „todo.Models” paranccsal, hogy tükrözze a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="bd981-252">If your project is not named "todo", then you need to update using "todo.Models"; to reflect the name of your project.</span></span>
   
    <span data-ttu-id="bd981-253">Most cserélje le ezt a kódot</span><span class="sxs-lookup"><span data-stu-id="bd981-253">Now replace this code</span></span>
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    <span data-ttu-id="bd981-254">a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="bd981-254">with the following code.</span></span>
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. <span data-ttu-id="bd981-255">Nyissa meg a **Global.asax.cs** fájlt, és adja hozzá a következő sort az **Application_Start** metódushoz</span><span class="sxs-lookup"><span data-stu-id="bd981-255">Open **Global.asax.cs** and add the following line to the **Application_Start** method</span></span> 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

<span data-ttu-id="bd981-256">Ekkor ideális esetben hibák nélkül fel kell tudnia építenie az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bd981-256">At this point your solution should be able to build without any errors.</span></span>

<span data-ttu-id="bd981-257">Ha most futtatná az alkalmazást, a **HomeController** vezérlőbe és annak **Index** nézetébe kerülne.</span><span class="sxs-lookup"><span data-stu-id="bd981-257">If you ran the application now, you would go to the **HomeController** and the **Index** view of that controller.</span></span> <span data-ttu-id="bd981-258">Ez az először választott MVC sablonprojekt alapértelmezett viselkedése, de mi nem ezt szeretnénk!</span><span class="sxs-lookup"><span data-stu-id="bd981-258">This is the default behavior for the MVC template project we chose at the start but we don't want that!</span></span> <span data-ttu-id="bd981-259">Módosítsuk a jelen MVC alkalmazás útválasztását ezen viselkedés megváltoztatásához.</span><span class="sxs-lookup"><span data-stu-id="bd981-259">Let's change the routing on this MVC application to alter this behavior.</span></span>

<span data-ttu-id="bd981-260">Nyissa meg az ***App\_Start\RouteConfig.cs*** fájlt, keresse meg a „defaults:” kezdetű sort, és módosítsa úgy, hogy a következőhöz hasonlítson.</span><span class="sxs-lookup"><span data-stu-id="bd981-260">Open ***App\_Start\RouteConfig.cs*** and locate the line starting with "defaults:" and change it to resemble the following.</span></span>

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

<span data-ttu-id="bd981-261">Ez most közli az ASP.NET MVC-vel, ha nem adott meg értéket az URL-címben az útválasztási viselkedés vezérléséhez, hogy a **Home** (Kezdőlap) helyett az **Item** (Elem) elemet használja vezérlőként és a felhasználói **Index** elemet nézetként.</span><span class="sxs-lookup"><span data-stu-id="bd981-261">This now tells ASP.NET MVC that if you have not specified a value in the URL to control the routing behavior that instead of **Home**, use **Item** as the controller and user **Index** as the view.</span></span>

<span data-ttu-id="bd981-262">Ha most futtatja az alkalmazást, az az **ItemController** vezérlőt hívja meg, amely az adattár osztályt hívja meg és a GetItems metódussal adja vissza az összes hiányos elemet a **Views**\\**Item**\\**Index** (Nézetek > Elem > Index) nézetben.</span><span class="sxs-lookup"><span data-stu-id="bd981-262">Now if you run the application, it will call into your **ItemController** which will call in to the repository class and use the GetItems method to return all the incomplete items to the **Views**\\**Item**\\**Index** view.</span></span> 

<span data-ttu-id="bd981-263">Ha most felépíti és futtatja ezt a projektet, valami ilyesmit kell látnia.</span><span class="sxs-lookup"><span data-stu-id="bd981-263">If you build and run this project now, you should now see something that looks this.</span></span>    

![A jelen adatbázis-oktatóprogram során létrehozott teendőlista webalkalmazás képernyőfelvétele](./media/documentdb-dotnet-application/build-and-run-the-project-now.png)

### <span data-ttu-id="bd981-265"><a name="_Toc395637771"></a>Elemek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd981-265"><a name="_Toc395637771"></a>Adding Items</span></span>
<span data-ttu-id="bd981-266">Tegyünk néhány elemet az adatbázisba, hogy ne csak egy üres táblát lássunk.</span><span class="sxs-lookup"><span data-stu-id="bd981-266">Let's put some items into our database so we have something more than an empty grid to look at.</span></span>

<span data-ttu-id="bd981-267">Adjunk néhány kódot az Azure Cosmos DBRepository és az ItemController elemhez, hogy megmaradjon a rekord az Azure Cosmos DB-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="bd981-267">Let's add some code to  Azure Cosmos DBRepository and ItemController to persist the record in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="bd981-268">Adja hozzá a **DocumentDBRepository** tárhoz a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="bd981-268">Add the following method to your **DocumentDBRepository** class.</span></span>
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   <span data-ttu-id="bd981-269">Ez a metódus egyszerűen vesz igénybe egy neki küldött objektumot, és továbbra is fennáll az Azure Cosmos-Adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="bd981-269">This method simply takes an object passed to it and persists it in Azure Cosmos DB.</span></span>
2. <span data-ttu-id="bd981-270">Nyissa meg az ItemController.cs fájlt, és adja hozzá a következő kódrészletet az osztályon belül.</span><span class="sxs-lookup"><span data-stu-id="bd981-270">Open the ItemController.cs file and add the following code snippet within the class.</span></span> <span data-ttu-id="bd981-271">Az ASP.NET MVC így tudja, hogy mit tegyen a **Create** (Létrehozás) művelethez.</span><span class="sxs-lookup"><span data-stu-id="bd981-271">This is how ASP.NET MVC knows what to do for the **Create** action.</span></span> <span data-ttu-id="bd981-272">Ebben az esetben csak jelenítse meg a korábban létrehozott társított Create.cshtml nézetet.</span><span class="sxs-lookup"><span data-stu-id="bd981-272">In this case just render the associated Create.cshtml view created earlier.</span></span>
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    <span data-ttu-id="bd981-273">Most több kódra van szükségünk ebben a vezérlőben, amely elfogadja a **Create** (Létrehozás) nézetből végzett elküldést.</span><span class="sxs-lookup"><span data-stu-id="bd981-273">We now need some more code in this controller that will accept the submission from the **Create** view.</span></span>
3. <span data-ttu-id="bd981-274">Adja hozzá a következő kódblokkot az ItemController.cs osztályhoz, amely közli az ASP.NET MVC-vel, hogy mit tegyen az ezen vezérlőből származó POST űrlapművelettel.</span><span class="sxs-lookup"><span data-stu-id="bd981-274">Add the next block of code to the ItemController.cs class that tells ASP.NET MVC what to do with a form POST for this controller.</span></span>
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    <span data-ttu-id="bd981-275">Ez a kód a DocumentDBRepository tárat hívja be, és a CreateItemAsync metódussal őrzi meg az új teendőelemet az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="bd981-275">This code calls in to the DocumentDBRepository and uses the CreateItemAsync method to persist the new todo item to the database.</span></span> 
   
    <span data-ttu-id="bd981-276">**Biztonsági megjegyzés**: A **ValidateAntiForgeryToken** attribútum itt segít megvédeni az alkalmazást a webhelyközi kérések hamisítása ellen.</span><span class="sxs-lookup"><span data-stu-id="bd981-276">**Security Note**: The **ValidateAntiForgeryToken** attribute is used here to help protect this application against cross-site request forgery attacks.</span></span> <span data-ttu-id="bd981-277">Az attribútum hozzáadásánál többről van szó, a nézeteknek is működniük kell ezzel a hamisítás elleni tokennel.</span><span class="sxs-lookup"><span data-stu-id="bd981-277">There is more to it than just adding this attribute, your views need to work with this anti-forgery token as well.</span></span> <span data-ttu-id="bd981-278">A témáról további részletekért és a megfelelő megvalósításának példáiért lásd: [Webhelyközi kérések hamisításának megakadályozása][Preventing Cross-Site Request Forgery].</span><span class="sxs-lookup"><span data-stu-id="bd981-278">For more on the subject, and examples of how to implement this correctly, please see [Preventing Cross-Site Request Forgery][Preventing Cross-Site Request Forgery].</span></span> <span data-ttu-id="bd981-279">A [GitHubon][GitHub] közzétett forráskódban szerepel a teljes megvalósítás.</span><span class="sxs-lookup"><span data-stu-id="bd981-279">The source code provided on [GitHub][GitHub] has the full implementation in place.</span></span>
   
    <span data-ttu-id="bd981-280">**Biztonsági megjegyzés**: A metódus paraméteren a **Bind** (Kötés) attribútummal is segítünk a túlküldéses támadások elleni védelemben.</span><span class="sxs-lookup"><span data-stu-id="bd981-280">**Security Note**: We also use the **Bind** attribute on the method parameter to help protect against over-posting attacks.</span></span> <span data-ttu-id="bd981-281">További részletekért lásd: [Alapvető CRUD műveletek az ASP.NET MVC-ben][Basic CRUD Operations in ASP.NET MVC].</span><span class="sxs-lookup"><span data-stu-id="bd981-281">For more details please see [Basic CRUD Operations in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].</span></span>

<span data-ttu-id="bd981-282">Ennyi lenne az adatbázishoz új elemek hozzáadásához szükséges kód.</span><span class="sxs-lookup"><span data-stu-id="bd981-282">This concludes the code required to add new Items to our database.</span></span>

### <span data-ttu-id="bd981-283"><a name="_Toc395637772"></a>Elemek szerkesztése</span><span class="sxs-lookup"><span data-stu-id="bd981-283"><a name="_Toc395637772"></a>Editing Items</span></span>
<span data-ttu-id="bd981-284">Az egyik utolsó teendő azon funkció hozzáadása, amellyel az **elemek** szerkeszthetők az adatbázisban és megjelölhetők befejezettként.</span><span class="sxs-lookup"><span data-stu-id="bd981-284">There is one last thing for us to do, and that is to add the ability to edit **Items** in the database and to mark them as complete.</span></span> <span data-ttu-id="bd981-285">A szerkesztésre szolgáló nézet már a projekthez lett adva, így csak néhány kódot kell ismét hozzáadnunk a vezérlőhöz és a **DocumentDBRepository** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="bd981-285">The view for editing was already added to the project, so we just need to add some code to our controller and to the **DocumentDBRepository** class again.</span></span>

1. <span data-ttu-id="bd981-286">Adja hozzá a következőt a **DocumentDBRepository** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="bd981-286">Add the following to the **DocumentDBRepository** class.</span></span>
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    <span data-ttu-id="bd981-287">Ezen metódusok közül az első, a **GetItem** egy elemet kér le az Azure Cosmos DB-adatbázisból, amelyet visszaküld az **ItemController** vezérlőhöz, majd az **Edit** (Szerkesztés) nézethez.</span><span class="sxs-lookup"><span data-stu-id="bd981-287">The first of these methods, **GetItem** fetches an Item from Azure Cosmos DB which is passed back to the **ItemController** and then on to the **Edit** view.</span></span>
   
    <span data-ttu-id="bd981-288">A most hozzáadott metódusok közül a második lecseréli a **dokumentumot** az Azure Cosmos DB-adatbázisban a **dokumentum** azon verziójával, amely az **ItemController** vezérlőből származik.</span><span class="sxs-lookup"><span data-stu-id="bd981-288">The second of the methods we just added replaces the **Document** in Azure Cosmos DB with the version of the **Document** passed in from the **ItemController**.</span></span>
2. <span data-ttu-id="bd981-289">Adja hozzá a következőt az **ItemController** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="bd981-289">Add the following to the **ItemController** class.</span></span>
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    <span data-ttu-id="bd981-290">Az első metódus a Http GET kérést kezeli, amely akkor történik meg, amikor a felhasználó az **Edit** (Szerkesztés) hivatkozásra kattint az **Index** nézetből.</span><span class="sxs-lookup"><span data-stu-id="bd981-290">The first method handles the Http GET that happens when the user clicks on the **Edit** link from the **Index** view.</span></span> <span data-ttu-id="bd981-291">Ez a metódus [**dokumentumot**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) kér le az Azure Cosmos DB-adatbázisból, és az **Edit** (Szerkesztés) nézetbe küldi azt.</span><span class="sxs-lookup"><span data-stu-id="bd981-291">This method fetches a [**Document**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) from Azure Cosmos DB and passes it to the **Edit** view.</span></span>
   
    <span data-ttu-id="bd981-292">Az **Edit** (Szerkesztés) nézet ezután Http POST kérést küld az **IndexController** vezérlőnek.</span><span class="sxs-lookup"><span data-stu-id="bd981-292">The **Edit** view will then do an Http POST to the **IndexController**.</span></span> 
   
    <span data-ttu-id="bd981-293">A második hozzáadott metódus kezeli a frissített objektum átadását az Azure Cosmos DB-adatbázisnak, hogy megmaradjon az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="bd981-293">The second method we added handles passing the updated object to Azure Cosmos DB to be persisted in the database.</span></span>

<span data-ttu-id="bd981-294">Ennyi, ez minden, amire szükségünk van az alkalmazás futtatásához, a hiányos **elemek** listázásához és új **elemek** hozzáadásához, valamint az **elemek** szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="bd981-294">That's it, that is everything we need to run our application, list incomplete **Items**, add new **Items**, and edit **Items**.</span></span>

## <span data-ttu-id="bd981-295"><a name="_Toc395637773"></a>6. lépés: Az alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="bd981-295"><a name="_Toc395637773"></a>Step 6: Run the application locally</span></span>
<span data-ttu-id="bd981-296">Az alkalmazás helyi gépen való teszteléséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bd981-296">To test the application on your local machine, do the following:</span></span>

1. <span data-ttu-id="bd981-297">Nyomja le az F5 billentyűt a Visual Studióban az alkalmazás hibakeresési módban történő összeállításához.</span><span class="sxs-lookup"><span data-stu-id="bd981-297">Hit F5 in Visual Studio to build the application in debug mode.</span></span> <span data-ttu-id="bd981-298">Ennek fel kell építenie az alkalmazást és el kell indítania egy böngészőt a korábban látott üres rácsoldallal:</span><span class="sxs-lookup"><span data-stu-id="bd981-298">It should build the application and launch a browser with the empty grid page we saw before:</span></span>
   
    ![A jelen adatbázis-oktatóprogram során létrehozott teendőlista webalkalmazás képernyőfelvétele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. <span data-ttu-id="bd981-300">Kattintson a **Create New** (Új létrehozása) hivatkozásra, és adjon értékeket a **Name** (Név) és a **Description** (Leírás) mezőkbe.</span><span class="sxs-lookup"><span data-stu-id="bd981-300">Click the **Create New** link and add values to the **Name** and **Description** fields.</span></span> <span data-ttu-id="bd981-301">Hagyja bejelöletlenül a **Completed** (Befejezve) jelölőnégyzetet, különben az új **elem** befejezett állapotban lesz hozzáadva és nem jelenik meg a kiindulási listában.</span><span class="sxs-lookup"><span data-stu-id="bd981-301">Leave the **Completed** check box unselected otherwise the new **Item** will be added in a completed state and will not appear on the initial list.</span></span>
   
    ![Képernyőfelvétel a Create (Létrehozás) nézetről](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. <span data-ttu-id="bd981-303">Kattintson a **Create** (Létrehozás) gombra, és a rendszer visszairányítja az **Index** nézetre, ahol az **elem** megjelenik a listában.</span><span class="sxs-lookup"><span data-stu-id="bd981-303">Click **Create** and you are redirected back to the **Index** view and your **Item** appears in the list.</span></span>
   
    ![Képernyőfelvétel az Index nézetről](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    <span data-ttu-id="bd981-305">Nyugodtan adjon hozzá még néhány **elemet** a teendőlistához.</span><span class="sxs-lookup"><span data-stu-id="bd981-305">Feel free to add a few more **Items** to your todo list.</span></span>
    
4. <span data-ttu-id="bd981-306">Kattintson az **Edit** (Szerkesztés) gombra a lista egy **eleme** mellett, és az **Edit** (Szerkesztés) nézetbe kerül, ahol frissítheti az objektum bármely tulajdonságát, beleértve a **Completed** (Befejezve) jelzőt.</span><span class="sxs-lookup"><span data-stu-id="bd981-306">Click **Edit** next to an **Item** on the list and you are taken to the **Edit** view where you can update any property of your object, including the **Completed** flag.</span></span> <span data-ttu-id="bd981-307">Ha bejelöli a **Complete** (Befejezve) jelzőt és a **Save** (Mentés) gombra kattint, azzal eltávolítja az **elemet** a hiányos feladatok listájából.</span><span class="sxs-lookup"><span data-stu-id="bd981-307">If you mark the **Complete** flag and click **Save**, the **Item** is removed from the list of incomplete tasks.</span></span>
   
    ![Képernyőfelvétel az Index nézetről, bejelölt Completed (Befejezve) jelölőnégyzettel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. <span data-ttu-id="bd981-309">Ha befejezte az alkalmazás tesztelését, nyomja meg a Ctrl+F5 billentyűkombinációt az alkalmazás hibakeresésének befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bd981-309">Once you've tested the app, press Ctrl+F5 to stop debugging the app.</span></span> <span data-ttu-id="bd981-310">Készen áll a telepítésre!</span><span class="sxs-lookup"><span data-stu-id="bd981-310">You're ready to deploy!</span></span>

## <span data-ttu-id="bd981-311"><a name="_Toc395637774"></a>7. lépés: Az Azure App Service-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="bd981-311"><a name="_Toc395637774"></a>Step 7: Deploy the application to Azure App Service</span></span> 
<span data-ttu-id="bd981-312">Most, hogy a teljes alkalmazás megfelelően működik-e az Azure Cosmos DB programot fogjuk a webalkalmazás telepítése az Azure App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="bd981-312">Now that you have the complete application working correctly with Azure Cosmos DB we're going to deploy this web app to Azure App Service.</span></span>  

1. <span data-ttu-id="bd981-313">Az alkalmazás közzétételéhez egyszerűen a jobb gombbal a projektre kell kattintania a **Megoldáskezelőben**, majd a **Publish** (Közzététel) parancsot választania.</span><span class="sxs-lookup"><span data-stu-id="bd981-313">To publish this application all you need to do is right-click on the project in **Solution Explorer** and click **Publish**.</span></span>
   
    ![Képernyőfelvétel a Közzététel lehetőségről a Megoldáskezelőben](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. <span data-ttu-id="bd981-315">A a **közzététel** párbeszédpanel, kattintson a **Microsoft Azure App Service**, majd jelölje be **hozzon létre új** hozzon létre egy App Service-profilt, vagy kattintson **meglévő** egy meglévő profilt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="bd981-315">In the **Publish** dialog box, click **Microsoft Azure App Service**, then select **Create New** to create an App Service profile, or click **Select Existing** to use an existing profile.</span></span>

    ![A Visual Studio párbeszédpanel közzététele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. <span data-ttu-id="bd981-317">Ha egy meglévő Azure App Service-profilt, adja meg az előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="bd981-317">If you have an existing Azure App Service profile, enter your subscription name.</span></span> <span data-ttu-id="bd981-318">Használja a **nézet** erőforráscsoportba vagy erőforrástípus rendezés szűréséhez, majd válassza ki az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bd981-318">Use the **View** filter to sort by resource group or resource type, then select your Azure App Service.</span></span> 
   
    ![A Visual Studio App Service párbeszédpanelen](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. <span data-ttu-id="bd981-320">Új Azure App Service-profil létrehozásához kattintson a **hozzon létre új** a a **közzététel** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bd981-320">To create a new Azure App Service profile, click **Create New** in the **Publish** dialog box.</span></span> <span data-ttu-id="bd981-321">Az a **létrehozása az App Service** párbeszédpanelen adja meg a webes alkalmazás neve és a megfelelő előfizetés, az erőforráscsoport és az App Service-csomag, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bd981-321">In the **Create App Service** dialog, enter your Web App name and appropriate subscription, resource group, and App Service plan, then click **Create**.</span></span>

    ![App Service párbeszédpanelen létrehozása a Visual Studióban](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

<span data-ttu-id="bd981-323">Néhány másodpercen belül a Visual Studio befejezi a webalkalmazás közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!</span><span class="sxs-lookup"><span data-stu-id="bd981-323">In a few seconds, Visual Studio will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>



## <span data-ttu-id="bd981-324"><a name="_Toc395637775"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd981-324"><a name="_Toc395637775"></a>Next steps</span></span>
<span data-ttu-id="bd981-325">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="bd981-325">Congratulations!</span></span> <span data-ttu-id="bd981-326">Ebben az esetben a beépített az első ASP.NET MVC webalkalmazását az Azure Cosmos DB használatával, és közzétette azt Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="bd981-326">You just built your first ASP.NET MVC web application using Azure Cosmos DB and published it to Azure.</span></span> <span data-ttu-id="bd981-327">A teljes alkalmazás forráskódja, beleértve az oktatóprogramban nem szereplő részletezési és törlési funkciót, letölthető vagy klónozható a [GitHubról][GitHub].</span><span class="sxs-lookup"><span data-stu-id="bd981-327">The source code for the complete application, including the detail and delete functionality that were not included in this tutorial can be downloaded or cloned from [GitHub][GitHub].</span></span> <span data-ttu-id="bd981-328">Így ha továbbra is érdekli ezen funkcióknak az alkalmazáshoz adása, a kóddal ezt megteheti.</span><span class="sxs-lookup"><span data-stu-id="bd981-328">So if you're interested in adding that to your app, grab the code and add it to this app.</span></span>

<span data-ttu-id="bd981-329">További funkciókat szeretne az alkalmazáshoz adni, tekintse át az elérhető API-kat a [Azure Cosmos .NET kódtárban](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) és nyugodtan az Azure Cosmos .NET kódtárban hozzájárulnak a [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="bd981-329">To add additional functionality to your application, review the APIs available in the [Azure Cosmos DB .NET Library](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) and feel free to contribute to the Azure Cosmos DB .NET Library on [GitHub][GitHub].</span></span> 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
