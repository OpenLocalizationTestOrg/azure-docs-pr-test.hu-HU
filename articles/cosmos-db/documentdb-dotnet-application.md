---
title: "ASP.NET MVC oktatóprogram az Azure Cosmos DB szolgáltatáshoz: webalkalmazás-fejlesztés | Microsoft Docs"
description: "ASP.NET MVC oktatóprogram toocreate egy MVC webalkalmazását az Azure Cosmos DB használatával. A JSON-fájlok tárolása és az adatok elérése az Azure-webhelyeken tárolt teendőkezelő alkalmazásból történik – ASP NET MVC oktatóprogram lépésről lépésre."
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
ms.openlocfilehash: dac2a9599b395524533e6fe14983789ff095331f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="b4819-105"><a name="_Toc395809351"></a>ASP.NET MVC oktatóprogram: webalkalmazás fejlesztése az Azure Cosmos DB szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b4819-105"><a name="_Toc395809351"></a>ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b4819-106">.NET</span><span class="sxs-lookup"><span data-stu-id="b4819-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="b4819-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="b4819-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="b4819-108">Java</span><span class="sxs-lookup"><span data-stu-id="b4819-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="b4819-109">Python</span><span class="sxs-lookup"><span data-stu-id="b4819-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="b4819-110">toohighlight hogyan hatékonyan kihasználja az Azure Cosmos DB toostore és lekérdezni a JSON-dokumentumok, a cikkben egy végpontok közötti körűen, bemutatja, hogyan toobuild teendőkezelő alkalmazást az Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b4819-110">toohighlight how you can efficiently leverage Azure Cosmos DB toostore and query JSON documents, this article provides an end-to-end walk-through showing you how toobuild a todo app using Azure Cosmos DB.</span></span> <span data-ttu-id="b4819-111">hello feladatok Azure Cosmos DB JSON-dokumentumokként tárolja.</span><span class="sxs-lookup"><span data-stu-id="b4819-111">hello tasks will be stored as JSON documents in Azure Cosmos DB.</span></span>

![Képernyőfelvétel a hello teendőlista MVC webalkalmazás hozta létre az oktatóanyag – ASP NET MVC oktatóprogram lépésről lépésre](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image01.png)

<span data-ttu-id="b4819-113">Ez az útmutató bemutatja, hogyan toouse hello Azure Cosmos DB szolgáltatás toostore és hozzáférés az Azure rendszeren üzemeltetett ASP.NET MVC webalkalmazás adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4819-113">This walk-through shows you how toouse hello Azure Cosmos DB service toostore and access data from an ASP.NET MVC web application hosted on Azure.</span></span> <span data-ttu-id="b4819-114">Ha egy oktatóanyag, amely csak Azure Cosmos DB keres, és nem hello ASP.NET MVC összetevőkkel, lásd: [egy Azure Cosmos DB C# Konzolalkalmazás létrehozása](documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b4819-114">If you're looking for a tutorial that focuses only on Azure Cosmos DB, and not hello ASP.NET MVC components, see [Build an Azure Cosmos DB C# console application](documentdb-get-started.md).</span></span>

> [!TIP]
> <span data-ttu-id="b4819-115">Ez az oktatóprogram feltételezi, hogy van korábbi tapasztalata az ASP.NET MVC és az Azure webhelyek használatában.</span><span class="sxs-lookup"><span data-stu-id="b4819-115">This tutorial assumes that you have prior experience using ASP.NET MVC and Azure Websites.</span></span> <span data-ttu-id="b4819-116">Ha új tooASP.NET vagy hello [előfeltételt jelentő eszközöket](#_Toc395637760), érdemes letöltenie hello teljes mintaprojektet a [GitHub] [ GitHub] és hello utasításait követve Ez a minta.</span><span class="sxs-lookup"><span data-stu-id="b4819-116">If you are new tooASP.NET or hello [prerequisite tools](#_Toc395637760), we recommend downloading hello complete sample project from [GitHub][GitHub] and following hello instructions in this sample.</span></span> <span data-ttu-id="b4819-117">Ha felépítette, ezen cikk toogain insight hello kódja hello projekt környezetében hello tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="b4819-117">Once you have it built, you can review this article toogain insight on hello code in hello context of hello project.</span></span>
> 
> 

## <span data-ttu-id="b4819-118"><a name="_Toc395637760"></a>Az adatbázis-oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="b4819-118"><a name="_Toc395637760"></a>Prerequisites for this database tutorial</span></span>
<span data-ttu-id="b4819-119">Ez a cikk hello utasításait követve, előtt győződjön meg, hogy rendelkezik-e hello következő:</span><span class="sxs-lookup"><span data-stu-id="b4819-119">Before following hello instructions in this article, you should ensure that you have hello following:</span></span>

* <span data-ttu-id="b4819-120">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="b4819-120">An active Azure account.</span></span> <span data-ttu-id="b4819-121">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="b4819-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b4819-122">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b4819-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

    <span data-ttu-id="b4819-123">VAGY</span><span class="sxs-lookup"><span data-stu-id="b4819-123">OR</span></span>

    <span data-ttu-id="b4819-124">Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="b4819-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="b4819-125">[A Visual Studio 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b4819-125">[Visual Studio 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="b4819-126">A Microsoft Azure SDK for .NET for Visual Studio 2017, a Visual Studio telepítő hello keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="b4819-126">Microsoft Azure SDK for .NET for Visual Studio 2017, available through hello Visual Studio Installer.</span></span>

<span data-ttu-id="b4819-127">Ez a cikk összes hello képernyőképek használatával a Microsoft Visual Studio Community 2017 került sor.</span><span class="sxs-lookup"><span data-stu-id="b4819-127">All hello screen shots in this article have been taken using Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="b4819-128">Ha a rendszer a lehetséges, hogy a képernyők és beállítások nem egyeznek tökéletesen, de ha megfelel a fenti előfeltételek hello ebben a megoldásban kell működnie egy másik verzió van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b4819-128">If your system is configured with a different version it is possible that your screens and options won't match entirely, but if you meet hello above prerequisites this solution should work.</span></span>

## <span data-ttu-id="b4819-129"><a name="_Toc395637761"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4819-129"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="b4819-130">Először hozzon létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="b4819-130">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="b4819-131">Ha már rendelkezik SQL (DocumentDB) fiókkal az Azure Cosmos DB vagy rendszer használata esetén ez az oktatóanyag hello Azure Cosmos DB emulátor, kihagyhatja túl[hozzon létre egy új ASP.NET MVC alkalmazást](#_Toc395637762).</span><span class="sxs-lookup"><span data-stu-id="b4819-131">If you already have a SQL (DocumentDB) account for Azure Cosmos DB or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Create a new ASP.NET MVC application](#_Toc395637762).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
<span data-ttu-id="b4819-132">Most végigvezetjük azon hogyan toocreate egy új ASP.NET MVC alkalmazás hello ground felfelé.</span><span class="sxs-lookup"><span data-stu-id="b4819-132">We will now walk through how toocreate a new ASP.NET MVC application from hello ground-up.</span></span> 

## <span data-ttu-id="b4819-133"><a name="_Toc395637762"></a>2. lépés: Új ASP.NET MVC alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4819-133"><a name="_Toc395637762"></a>Step 2: Create a new ASP.NET MVC application</span></span>

1. <span data-ttu-id="b4819-134">A Visual Studio, a hello **fájl** menüben mutasson túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="b4819-134">In Visual Studio, on hello **File** menu, point too**New**, and then click **Project**.</span></span> <span data-ttu-id="b4819-135">Hello **új projekt** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4819-135">hello **New Project** dialog box appears.</span></span>

2. <span data-ttu-id="b4819-136">A hello **projekt típusok** ablaktáblában bontsa ki **sablonok**, **Visual C#**, **webes**, majd válassza ki **ASP.NET Webalkalmazásként való kezelése** .</span><span class="sxs-lookup"><span data-stu-id="b4819-136">In hello **Project types** pane, expand **Templates**, **Visual C#**, **Web**, and then select **ASP.NET Web Application**.</span></span>

      ![Képernyőfelvétel a hello új projekt párbeszédpanel a hello ASP.NET webalkalmazás projekttípus van kijelölve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. <span data-ttu-id="b4819-138">A hello **neve** mezőjébe hello projekt hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="b4819-138">In hello **Name** box, type hello name of hello project.</span></span> <span data-ttu-id="b4819-139">Ez az oktatóanyag hello neve "todo" használja.</span><span class="sxs-lookup"><span data-stu-id="b4819-139">This tutorial uses hello name "todo".</span></span> <span data-ttu-id="b4819-140">Ha úgy dönt, toouse más nevet, majd bárhol ebben az oktatóanyagban beszél hello todo névtér, kell tooadjust megadott hello kód minták toouse függetlenül nevű az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b4819-140">If you choose toouse something other than this, then wherever this tutorial talks about hello todo namespace, you need tooadjust hello provided code samples toouse whatever you named your application.</span></span> 
4. <span data-ttu-id="b4819-141">Kattintson a **Tallózás** toonavigate toohello mappa hol kívánja toocreate hello projekt, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4819-141">Click **Browse** toonavigate toohello folder where you would like toocreate hello project, and then click **OK**.</span></span>
   
      <span data-ttu-id="b4819-142">Hello **új ASP.NET-webalkalmazás** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4819-142">hello **New ASP.NET Web Application** dialog box appears.</span></span>
   
    ![Képernyőfelvétel a hello MVC alkalmazássablon van kiemelve hello új ASP.NET Webalkalmazásként való kezelése párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. <span data-ttu-id="b4819-144">Hello sablonok panelén válassza **MVC**.</span><span class="sxs-lookup"><span data-stu-id="b4819-144">In hello templates pane, select **MVC**.</span></span>

6. <span data-ttu-id="b4819-145">Kattintson a **OK** és lehetővé teszik a Visual Studio kialakítsa állványok hello üres ASP.NET MVC sablonban.</span><span class="sxs-lookup"><span data-stu-id="b4819-145">Click **OK** and let Visual Studio do its thing around scaffolding hello empty ASP.NET MVC template.</span></span> 

          
7. <span data-ttu-id="b4819-146">Visual Studio befejezte a hello bolierplate MVC alkalmazás létrehozása után a felhasználó egy üres ASP.NET alkalmazást, amelyet helyileg futtathat.</span><span class="sxs-lookup"><span data-stu-id="b4819-146">Once Visual Studio has finished creating hello boilerplate MVC application you have an empty ASP.NET application that you can run locally.</span></span>
   
    <span data-ttu-id="b4819-147">Kihagyjuk a futó hello projekt helyileg, mert nem használom, azt is, hogy minden látott hello ASP.NET "Hello World" alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b4819-147">We'll skip running hello project locally because I'm sure we've all seen hello ASP.NET "Hello World" application.</span></span> <span data-ttu-id="b4819-148">Ugorjunk egyenes tooadding Azure Cosmos DB toothis projekt és az alkalmazás felépítésére.</span><span class="sxs-lookup"><span data-stu-id="b4819-148">Let's go straight tooadding Azure Cosmos DB toothis project and building our application.</span></span>

## <span data-ttu-id="b4819-149"><a name="_Toc395637767"></a>3. lépés:, Adja hozzá Azure Cosmos DB tooyour MVC webalkalmazás projekthez</span><span class="sxs-lookup"><span data-stu-id="b4819-149"><a name="_Toc395637767"></a>Step 3: Add Azure Cosmos DB tooyour MVC web application project</span></span>
<span data-ttu-id="b4819-150">Most, hogy ehhez a megoldáshoz szükséges hello ASP.NET MVC bekötések nagy többségét, folytassuk toohello valódi céljával, amely ebben az oktatóanyagban Azure Cosmos DB tooour MVC webalkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b4819-150">Now that we have most of hello ASP.NET MVC plumbing that we need for this solution, let's get toohello real purpose of this tutorial, adding Azure Cosmos DB tooour MVC web application.</span></span>

1. <span data-ttu-id="b4819-151">hello Azure Cosmos DB .NET SDK csomagolt és a NuGet-csomag terjesztése.</span><span class="sxs-lookup"><span data-stu-id="b4819-151">hello Azure Cosmos DB .NET SDK is packaged and distributed as a NuGet package.</span></span> <span data-ttu-id="b4819-152">tooget hello Visual Studio NuGet-csomagot, hello NuGet-Csomagkezelő használja a Visual Studióban kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** majd **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="b4819-152">tooget hello NuGet package in Visual Studio, use hello NuGet package manager in Visual Studio by right-clicking on hello project in **Solution Explorer** and then clicking **Manage NuGet Packages**.</span></span>
   
    ![Képernyőfelvétel a hello gombbal hello webalkalmazás projekthez a Megoldáskezelőben, a Manage NuGet Packages kiemelt beállításait.](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    <span data-ttu-id="b4819-154">Hello **NuGet-csomagok kezelése** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4819-154">hello **Manage NuGet Packages** dialog box appears.</span></span>
2. <span data-ttu-id="b4819-155">A hello NuGet **Tallózás** mezőbe írja be ***Azure DocumentDB***.</span><span class="sxs-lookup"><span data-stu-id="b4819-155">In hello NuGet **Browse** box, type ***Azure DocumentDB***.</span></span> <span data-ttu-id="b4819-156">(hello csomag neve nem lett frissítve tooAzure Cosmos DB.)</span><span class="sxs-lookup"><span data-stu-id="b4819-156">(hello package name has not been updated tooAzure Cosmos DB.)</span></span>
   
    <span data-ttu-id="b4819-157">Hello eredmények közül telepítse a hello **Microsoft Microsoft.Azure.DocumentDB** csomag.</span><span class="sxs-lookup"><span data-stu-id="b4819-157">From hello results, install hello **Microsoft.Azure.DocumentDB by Microsoft** package.</span></span> <span data-ttu-id="b4819-158">Ez letölti és telepíti a hello Azure Cosmos DB csomagot, valamint az összes függőségét, például a newtonsoft.JSON elemet.</span><span class="sxs-lookup"><span data-stu-id="b4819-158">This will download and install hello Azure Cosmos DB package as well as all dependencies, such as Newtonsoft.Json.</span></span> <span data-ttu-id="b4819-159">Kattintson a **OK** hello a **előzetes** ablakot, és **elfogadom** hello a **licenc elfogadása** ablak toocomplete hello telepítése.</span><span class="sxs-lookup"><span data-stu-id="b4819-159">Click **OK** in hello **Preview** window, and **I Accept** in hello **License Acceptance** window toocomplete hello install.</span></span>
   
    ![Hello NuGet-csomagok kezelése ablakban, a Microsoft Azure DocumentDB Client Library elem van kiemelve hello képernyőfelvétele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      <span data-ttu-id="b4819-161">Másik lehetőségként hello Csomagkezelő konzol tooinstall hello csomagot is használhat.</span><span class="sxs-lookup"><span data-stu-id="b4819-161">Alternatively you can use hello Package Manager Console tooinstall hello package.</span></span> <span data-ttu-id="b4819-162">toodo Igen, a hello **eszközök** menüben kattintson a **NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="b4819-162">toodo so, on hello **Tools** menu, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span> <span data-ttu-id="b4819-163">Hello parancssorba írja be a következő hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-163">At hello prompt, type hello following.</span></span>
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. <span data-ttu-id="b4819-164">Hello csomag telepítve van, a Visual Studio megoldás hello következő hivatkozásokkal két új hozzáadott hivatkozással: Microsoft.Azure.Documents.Client és Newtonsoft.Json kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="b4819-164">Once hello package is installed, your Visual Studio solution should resemble hello following with two new references added, Microsoft.Azure.Documents.Client and Newtonsoft.Json.</span></span>
   
    ![Hello két hivatkozás képernyőfelvétele toohello JSON adatprojekthez hozzáadva a Megoldáskezelőben](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <span data-ttu-id="b4819-166"><a name="_Toc395637763"></a>4. lépés: Hello ASP.NET MVC alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="b4819-166"><a name="_Toc395637763"></a>Step 4: Set up hello ASP.NET MVC application</span></span>
<span data-ttu-id="b4819-167">Most adjuk hozzá hello modellek, nézetekkel és vezérlőkkel toothis MVC alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="b4819-167">Now let's add hello models, views, and controllers toothis MVC application:</span></span>

* <span data-ttu-id="b4819-168">[Modell hozzáadása](#_Toc395637764).</span><span class="sxs-lookup"><span data-stu-id="b4819-168">[Add a model](#_Toc395637764).</span></span>
* <span data-ttu-id="b4819-169">[Vezérlő hozzáadása](#_Toc395637765).</span><span class="sxs-lookup"><span data-stu-id="b4819-169">[Add a controller](#_Toc395637765).</span></span>
* <span data-ttu-id="b4819-170">[Nézetek hozzáadása](#_Toc395637766).</span><span class="sxs-lookup"><span data-stu-id="b4819-170">[Add views](#_Toc395637766).</span></span>

### <span data-ttu-id="b4819-171"><a name="_Toc395637764"></a>JSON adatmodell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4819-171"><a name="_Toc395637764"></a>Add a JSON data model</span></span>
<span data-ttu-id="b4819-172">Hozzuk létre hello **M** hello az mvc-ben, a modell.</span><span class="sxs-lookup"><span data-stu-id="b4819-172">Let's begin by creating hello **M** in MVC, hello model.</span></span> 

1. <span data-ttu-id="b4819-173">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **modellek** mappát, kattintson a **hozzáadása**, és kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="b4819-173">In **Solution Explorer**, right-click hello **Models** folder, click **Add**, and then click **Class**.</span></span>
   
      <span data-ttu-id="b4819-174">Hello **új elem hozzáadása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4819-174">hello **Add New Item** dialog box appears.</span></span>
2. <span data-ttu-id="b4819-175">Adja az új osztálynak az **Item.cs** nevet, és kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4819-175">Name your new class **Item.cs** and click **Add**.</span></span> 
3. <span data-ttu-id="b4819-176">Ebben az új **Item.cs** fájlt, adja hozzá a hello következő hello után utolsó *utasítás használatával*.</span><span class="sxs-lookup"><span data-stu-id="b4819-176">In this new **Item.cs** file, add hello following after hello last *using statement*.</span></span>
   
        using Newtonsoft.Json;
4. <span data-ttu-id="b4819-177">Most cserélje le ezt a kódot</span><span class="sxs-lookup"><span data-stu-id="b4819-177">Now replace this code</span></span> 
   
        public class Item
        {
        }
   
    <span data-ttu-id="b4819-178">a kód a következő hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-178">with hello following code.</span></span>
   
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
   
    <span data-ttu-id="b4819-179">Az Azure Cosmos Adatbázisba az összes adat hello hálózaton keresztül továbbított és JSON-ként tárolja.</span><span class="sxs-lookup"><span data-stu-id="b4819-179">All data in Azure Cosmos DB is passed over hello wire and stored as JSON.</span></span> <span data-ttu-id="b4819-180">toocontrol hello módon az objektumok által használható JSON.NET szerializált/deszerializálása hello **JsonProperty** attribútumot, ahogyan az hello **elem** imént létrehozott osztályt.</span><span class="sxs-lookup"><span data-stu-id="b4819-180">toocontrol hello way your objects are serialized/deserialized by JSON.NET you can use hello **JsonProperty** attribute as demonstrated in hello **Item** class we just created.</span></span> <span data-ttu-id="b4819-181">Nem **rendelkezik** toodo ez, de szeretné, hogy a tulajdonságaim követik hello JSON camelCase elnevezési konvenciói tooensure.</span><span class="sxs-lookup"><span data-stu-id="b4819-181">You don't **have** toodo this but I want tooensure that my properties follow hello JSON camelCase naming conventions.</span></span> 
   
    <span data-ttu-id="b4819-182">Nem csak vezérelheti hello hello tulajdonságnév formátumát a JSON-ba kerül, de teljesen át is nevezheti a .NET tulajdonságokat, mint ahogyan a hello **leírás** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4819-182">Not only can you control hello format of hello property name when it goes into JSON, but you can entirely rename your .NET properties like I did with hello **Description** property.</span></span> 

### <span data-ttu-id="b4819-183"><a name="_Toc395637765"></a>Vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4819-183"><a name="_Toc395637765"></a>Add a controller</span></span>
<span data-ttu-id="b4819-184">Amely gondoskodik hello **M**, most hozzuk létre az hello **C** az mvc-ben, a vezérlő osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="b4819-184">That takes care of hello **M**, now let's create hello **C** in MVC, a controller class.</span></span>

1. <span data-ttu-id="b4819-185">A **Solution Explorer**, kattintson a jobb gombbal hello **tartományvezérlők** mappát, kattintson a **Hozzáadás**, és kattintson a **vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="b4819-185">In **Solution Explorer**, right-click hello **Controllers** folder, click **Add**, and then click **Controller**.</span></span>
   
    <span data-ttu-id="b4819-186">Hello **hozzáadása Scaffold** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4819-186">hello **Add Scaffold** dialog box appears.</span></span>
2. <span data-ttu-id="b4819-187">Válassza az **MVC 5 Controller - Empty** (MVC 5 vezérlő - Üres) elemet, majd kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4819-187">Select **MVC 5 Controller - Empty** and then click **Add**.</span></span>
   
    ![Képernyőfelvétel a hello MVC 5 vezérlő - üres beállítás kiemelt hello Scaffold hozzáadása párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. <span data-ttu-id="b4819-189">Adja az **ItemController** nevet az új vezérlőnek.</span><span class="sxs-lookup"><span data-stu-id="b4819-189">Name your new Controller, **ItemController.**</span></span>
   
    ![Képernyőfelvétel a hello vezérlő hozzáadása párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    <span data-ttu-id="b4819-191">Hello-fájl létrehozása a Visual Studio megoldás kell hasonlítania hello következő hello új ItemController.cs fájllal a **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="b4819-191">Once hello file is created, your Visual Studio solution should resemble hello following with hello new ItemController.cs file in **Solution Explorer**.</span></span> <span data-ttu-id="b4819-192">hello korábban létrehozott új Item.cs fájl is látható.</span><span class="sxs-lookup"><span data-stu-id="b4819-192">hello new Item.cs file created earlier is also shown.</span></span>
   
    ![Képernyőfelvétel a hello Visual Studio megoldás - megoldáskezelő a hello új ItemController.cs és Item.cs fájl kiemelve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    <span data-ttu-id="b4819-194">Bezárhatja az ItemController.cs, visszatérünk tooit később.</span><span class="sxs-lookup"><span data-stu-id="b4819-194">You can close ItemController.cs, we'll come back tooit later.</span></span> 

### <span data-ttu-id="b4819-195"><a name="_Toc395637766"></a>Nézetek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4819-195"><a name="_Toc395637766"></a>Add views</span></span>
<span data-ttu-id="b4819-196">Most hozzuk létre az hello **V** mvc, hello nézetek:</span><span class="sxs-lookup"><span data-stu-id="b4819-196">Now, let's create hello **V** in MVC, hello views:</span></span>

* <span data-ttu-id="b4819-197">[Elemindexnézet hozzáadása](#AddItemIndexView).</span><span class="sxs-lookup"><span data-stu-id="b4819-197">[Add an Item Index view](#AddItemIndexView).</span></span>
* <span data-ttu-id="b4819-198">[Új elemnézet hozzáadása](#AddNewIndexView).</span><span class="sxs-lookup"><span data-stu-id="b4819-198">[Add a New Item view](#AddNewIndexView).</span></span>
* <span data-ttu-id="b4819-199">[Elemszerkesztési nézet hozzáadása](#_Toc395888515).</span><span class="sxs-lookup"><span data-stu-id="b4819-199">[Add an Edit Item view](#_Toc395888515).</span></span>

#### <span data-ttu-id="b4819-200"><a name="AddItemIndexView"></a>Elemindexnézet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4819-200"><a name="AddItemIndexView"></a>Add an Item Index view</span></span>
1. <span data-ttu-id="b4819-201">A **Megoldáskezelőben**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal hello üres **elem** mappát, amely a Visual Studio létrehozza azt hello hozzáadásakor  **ItemController** korábbi, kattintson a **Hozzáadás**, és kattintson a **nézet**.</span><span class="sxs-lookup"><span data-stu-id="b4819-201">In **Solution Explorer**, expand hello **Views**  folder, right-click hello empty **Item** folder that Visual Studio created for you when you added hello **ItemController** earlier, click **Add**, and then click **View**.</span></span>
   
    ![Képernyőfelvétel a Megoldáskezelőben hello elem mappa, amely a Visual Studio létre hello nézet hozzáadása parancsok vannak kiemelve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. <span data-ttu-id="b4819-203">A hello **nézet hozzáadása** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="b4819-203">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="b4819-204">A hello **nézetnév** mezőbe írja be ***Index***.</span><span class="sxs-lookup"><span data-stu-id="b4819-204">In hello **View name** box, type ***Index***.</span></span>
   * <span data-ttu-id="b4819-205">A hello **sablon** mezőben válassza ***lista***.</span><span class="sxs-lookup"><span data-stu-id="b4819-205">In hello **Template** box, select ***List***.</span></span>
   * <span data-ttu-id="b4819-206">A hello **Model class** mezőben válassza ***elem (todo. Modellek)***.</span><span class="sxs-lookup"><span data-stu-id="b4819-206">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="b4819-207">Hello elrendezés lap mezőbe írja be ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="b4819-207">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
     
   ![Képernyőfelvétel a változásszinkronizálás ábrázoló hello nézet hozzáadása párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. <span data-ttu-id="b4819-209">Amikor ezen értékek mindegyike már be van állítva, kattintson az **Add** (Hozzáadás) gombra és várja meg, hogy a Visual Studio létrehozzon egy új sablonnézetet.</span><span class="sxs-lookup"><span data-stu-id="b4819-209">Once all these values are set, click **Add** and let Visual Studio create a new template view.</span></span> <span data-ttu-id="b4819-210">Ha ezzel végzett, akkor megnyílik hello létrehozott cshtml fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4819-210">Once it is done, it will open hello cshtml file  that was created.</span></span> <span data-ttu-id="b4819-211">Azt is zárja be a fájlt a Visual Studio, azt fogja térjen vissza tooit később.</span><span class="sxs-lookup"><span data-stu-id="b4819-211">We can close that file in Visual Studio as we will come back tooit later.</span></span>

#### <span data-ttu-id="b4819-212"><a name="AddNewIndexView"></a>Új elemnézet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4819-212"><a name="AddNewIndexView"></a>Add a New Item view</span></span>
<span data-ttu-id="b4819-213">Hasonló toohow létrehoztunk egy **Elemindex** nézet, most létrehozunk egy új nézetet új létrehozása **elemek**.</span><span class="sxs-lookup"><span data-stu-id="b4819-213">Similar toohow we created an **Item Index** view, we will now create a new view for creating new **Items**.</span></span>

1. <span data-ttu-id="b4819-214">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **elem** mappát, kattintson a **Hozzáadás**, és kattintson a **nézet**.</span><span class="sxs-lookup"><span data-stu-id="b4819-214">In **Solution Explorer**, right-click hello **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="b4819-215">A hello **nézet hozzáadása** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="b4819-215">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="b4819-216">A hello **nézetnév** mezőbe írja be ***létrehozása***.</span><span class="sxs-lookup"><span data-stu-id="b4819-216">In hello **View name** box, type ***Create***.</span></span>
   * <span data-ttu-id="b4819-217">A hello **sablon** mezőben válassza ***létrehozása***.</span><span class="sxs-lookup"><span data-stu-id="b4819-217">In hello **Template** box, select ***Create***.</span></span>
   * <span data-ttu-id="b4819-218">A hello **Model class** mezőben válassza ***elem (todo. Modellek)***.</span><span class="sxs-lookup"><span data-stu-id="b4819-218">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="b4819-219">Hello elrendezés lap mezőbe írja be ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="b4819-219">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="b4819-220">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="b4819-220">Click **Add**.</span></span>
   
#### <span data-ttu-id="b4819-221"><a name="_Toc395888515"></a>Elemszerkesztési nézet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4819-221"><a name="_Toc395888515"></a>Add an Edit Item view</span></span>
<span data-ttu-id="b4819-222">És végül adja hozzá egy utolsó nézetet szerkeszthető egy **elem** a hello ahogyan azt korábban.</span><span class="sxs-lookup"><span data-stu-id="b4819-222">And finally, add one last view for editing an **Item** in hello same way as before.</span></span>

1. <span data-ttu-id="b4819-223">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **elem** mappát, kattintson a **Hozzáadás**, és kattintson a **nézet**.</span><span class="sxs-lookup"><span data-stu-id="b4819-223">In **Solution Explorer**, right-click hello **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="b4819-224">A hello **nézet hozzáadása** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="b4819-224">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="b4819-225">A hello **nézetnév** mezőbe írja be ***szerkesztése***.</span><span class="sxs-lookup"><span data-stu-id="b4819-225">In hello **View name** box, type ***Edit***.</span></span>
   * <span data-ttu-id="b4819-226">A hello **sablon** mezőben válassza ***szerkesztése***.</span><span class="sxs-lookup"><span data-stu-id="b4819-226">In hello **Template** box, select ***Edit***.</span></span>
   * <span data-ttu-id="b4819-227">A hello **Model class** mezőben válassza ***elem (todo. Modellek)***.</span><span class="sxs-lookup"><span data-stu-id="b4819-227">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="b4819-228">Hello elrendezés lap mezőbe írja be ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="b4819-228">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="b4819-229">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="b4819-229">Click **Add**.</span></span>

<span data-ttu-id="b4819-230">Ha ezzel végzett, zárja be az összes hello cshtml dokumentumot a Visual Studio azt toothese nézetek később fog visszaadni.</span><span class="sxs-lookup"><span data-stu-id="b4819-230">Once this is done, close all hello cshtml documents in Visual Studio as we will return toothese views later.</span></span>

## <span data-ttu-id="b4819-231"><a name="_Toc395637769"></a>5. lépés: Az Azure Cosmos DB csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="b4819-231"><a name="_Toc395637769"></a>Step 5: Wiring up Azure Cosmos DB</span></span>
<span data-ttu-id="b4819-232">Most, hogy hello szabványos MVC elvégeztük a, adjuk tooadding hello kód Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b4819-232">Now that hello standard MVC stuff is taken care of, let's turn tooadding hello code for Azure Cosmos DB.</span></span> 

<span data-ttu-id="b4819-233">Ebben a szakaszban kód toohandle hello következő fel kell venni:</span><span class="sxs-lookup"><span data-stu-id="b4819-233">In this section, we'll add code toohandle hello following:</span></span>

* <span data-ttu-id="b4819-234">[Hiányos elemek listázása](#_Toc395637770).</span><span class="sxs-lookup"><span data-stu-id="b4819-234">[Listing incomplete Items](#_Toc395637770).</span></span>
* <span data-ttu-id="b4819-235">[Elemek hozzáadása](#_Toc395637771).</span><span class="sxs-lookup"><span data-stu-id="b4819-235">[Adding Items](#_Toc395637771).</span></span>
* <span data-ttu-id="b4819-236">[Elemek szerkesztése](#_Toc395637772).</span><span class="sxs-lookup"><span data-stu-id="b4819-236">[Editing Items](#_Toc395637772).</span></span>

### <span data-ttu-id="b4819-237"><a name="_Toc395637770"></a>Hiányos elemek listázása az MVC webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="b4819-237"><a name="_Toc395637770"></a>Listing incomplete Items in your MVC web application</span></span>
<span data-ttu-id="b4819-238">hello első lépésként toodo itt van vegyen fel egy osztály, amely tartalmazza az összes hello logika tooconnect tooand használata Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b4819-238">hello first thing toodo here is add a class that contains all hello logic tooconnect tooand use Azure Cosmos DB.</span></span> <span data-ttu-id="b4819-239">Az oktatóanyag azt fogja foglalják magukban ezen logikák tooa adattár osztályt DocumentDBRepository nevű adattárba foglaljuk.</span><span class="sxs-lookup"><span data-stu-id="b4819-239">For this tutorial we'll encapsulate all this logic in tooa repository class called DocumentDBRepository.</span></span> 

1. <span data-ttu-id="b4819-240">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projekt, kattintson a **Hozzáadás**, és kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="b4819-240">In **Solution Explorer**, right-click on hello project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="b4819-241">Hello új osztály neve **DocumentDBRepository** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b4819-241">Name hello new class **DocumentDBRepository** and click **Add**.</span></span>
2. <span data-ttu-id="b4819-242">Az újonnan létrehozott hello **DocumentDBRepository** osztályhoz, és adja hozzá a következő hello *utasítások segítségével* fent hello *névtér* nyilatkozat</span><span class="sxs-lookup"><span data-stu-id="b4819-242">In hello newly created **DocumentDBRepository** class and add hello following *using statements* above hello *namespace* declaration</span></span>
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    <span data-ttu-id="b4819-243">Most cserélje le ezt a kódot</span><span class="sxs-lookup"><span data-stu-id="b4819-243">Now replace this code</span></span> 
   
        public class DocumentDBRepository
        {
        }
   
    <span data-ttu-id="b4819-244">a kód a következő hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-244">with hello following code.</span></span>
   
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
   
    
3. <span data-ttu-id="b4819-245">Konfigurációból adunk néhány érték, ezért nyissa meg a hello **Web.config** fájlt az alkalmazás, és adja hozzá a következő sorokat a hello hello `<AppSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4819-245">We're reading some values from configuration, so open hello **Web.config** file of your application and add hello following lines under hello `<AppSettings>` section.</span></span>
   
        <add key="endpoint" value="enter hello URI from hello Keys blade of hello Azure Portal"/>
        <add key="authKey" value="enter hello PRIMARY KEY, or hello SECONDARY KEY, from hello Keys blade of hello Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. <span data-ttu-id="b4819-246">Most frissítse hello értékeinek *végpont* és *authKey* hello (kulcsok) panelén hello Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="b4819-246">Now, update hello values for *endpoint* and *authKey* using hello Keys blade of hello Azure Portal.</span></span> <span data-ttu-id="b4819-247">Hello használata **URI** hello kulcsok paneljén hello végpont beállítás, és használjon hello hello értékeként **elsődleges kulcs**, vagy **másodlagos kulcs** hello kulcsok paneljén hello hello értéke authKey beállítás értékeként.</span><span class="sxs-lookup"><span data-stu-id="b4819-247">Use hello **URI** from hello Keys blade as hello value of hello endpoint setting, and use hello **PRIMARY KEY**, or **SECONDARY KEY** from hello Keys blade as hello value of hello authKey setting.</span></span>

    <span data-ttu-id="b4819-248">Hogy vesz gondot elvégeztük hello Azure Cosmos DB tárház, most adjuk hozzá az alkalmazás logikáját.</span><span class="sxs-lookup"><span data-stu-id="b4819-248">That takes care of wiring up hello Azure Cosmos DB repository, now let's add our application logic.</span></span>

1. <span data-ttu-id="b4819-249">hello először thing szeretnénk toobe képes toodo rendelkező a teendőlista alkalmazásában toodisplay hello hiányos elemeket.</span><span class="sxs-lookup"><span data-stu-id="b4819-249">hello first thing we want toobe able toodo with a todo list application is toodisplay hello incomplete items.</span></span>  <span data-ttu-id="b4819-250">Másolja és illessze be a következő kódrészletet bárhová belül hello hello **DocumentDBRepository** osztály.</span><span class="sxs-lookup"><span data-stu-id="b4819-250">Copy and paste hello following code snippet anywhere within hello **DocumentDBRepository** class.</span></span>
   
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
2. <span data-ttu-id="b4819-251">Nyissa meg hello **ItemController** azt korábban hozzáadott, és adja hozzá a következő hello *utasítások segítségével* hello névtér-deklaráció fent.</span><span class="sxs-lookup"><span data-stu-id="b4819-251">Open hello **ItemController** we added earlier and add hello following *using statements* above hello namespace declaration.</span></span>
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    <span data-ttu-id="b4819-252">Ha a projekt neve nem "todo", akkor újra kell tooupdate használja a "todo. Modellek"; a projekt tooreflect hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b4819-252">If your project is not named "todo", then you need tooupdate using "todo.Models"; tooreflect hello name of your project.</span></span>
   
    <span data-ttu-id="b4819-253">Most cserélje le ezt a kódot</span><span class="sxs-lookup"><span data-stu-id="b4819-253">Now replace this code</span></span>
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    <span data-ttu-id="b4819-254">a kód a következő hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-254">with hello following code.</span></span>
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. <span data-ttu-id="b4819-255">Nyissa meg **Global.asax.cs** , és adja hozzá a következő sor toohello hello **Application_Start** módszer</span><span class="sxs-lookup"><span data-stu-id="b4819-255">Open **Global.asax.cs** and add hello following line toohello **Application_Start** method</span></span> 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

<span data-ttu-id="b4819-256">Ezen a ponton a megoldás képes toobuild hiba nélkül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4819-256">At this point your solution should be able toobuild without any errors.</span></span>

<span data-ttu-id="b4819-257">Ha hello alkalmazás most már futott, kerülne toohello **HomeController** és hello **Index** nézetébe kerülne.</span><span class="sxs-lookup"><span data-stu-id="b4819-257">If you ran hello application now, you would go toohello **HomeController** and hello **Index** view of that controller.</span></span> <span data-ttu-id="b4819-258">Ez a hello hello hello start választott MVC sablonprojekt alapértelmezett viselkedése, de mi nem ezt szeretnénk!</span><span class="sxs-lookup"><span data-stu-id="b4819-258">This is hello default behavior for hello MVC template project we chose at hello start but we don't want that!</span></span> <span data-ttu-id="b4819-259">Módosítsuk hello az Útválasztás a MVC alkalmazás tooalter ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="b4819-259">Let's change hello routing on this MVC application tooalter this behavior.</span></span>

<span data-ttu-id="b4819-260">Nyissa meg ***App\_Start\RouteConfig.cs*** , és keresse meg a hello kezdetű sort "alapértelmezett értéke:" módosítsa azt a következő tooresemble hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-260">Open ***App\_Start\RouteConfig.cs*** and locate hello line starting with "defaults:" and change it tooresemble hello following.</span></span>

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

<span data-ttu-id="b4819-261">Ez most közli az ASP.NET MVC, ha a nem megadott érték hello URL-cím toocontrol hello helyette, amely az útválasztási viselkedés **Home**, használja **elem** hello, tartományvezérlői és felhasználói **Index** hello nézetként.</span><span class="sxs-lookup"><span data-stu-id="b4819-261">This now tells ASP.NET MVC that if you have not specified a value in hello URL toocontrol hello routing behavior that instead of **Home**, use **Item** as hello controller and user **Index** as hello view.</span></span>

<span data-ttu-id="b4819-262">Most hello alkalmazás futtatásakor, azt fogja belülre hívni a **ItemController** amely toohello adattár osztályt hívja és hello GetItems metódus tooreturn használja minden hello hiányos elemek toohello **nézetek** \\ **Elem**\\**Index** nézet.</span><span class="sxs-lookup"><span data-stu-id="b4819-262">Now if you run hello application, it will call into your **ItemController** which will call in toohello repository class and use hello GetItems method tooreturn all hello incomplete items toohello **Views**\\**Item**\\**Index** view.</span></span> 

<span data-ttu-id="b4819-263">Ha most felépíti és futtatja ezt a projektet, valami ilyesmit kell látnia.</span><span class="sxs-lookup"><span data-stu-id="b4819-263">If you build and run this project now, you should now see something that looks this.</span></span>    

![Képernyőfelvétel a hello teendőlista webalkalmazás adatbázis-oktatóprogram során létrehozott](./media/documentdb-dotnet-application/build-and-run-the-project-now.png)

### <span data-ttu-id="b4819-265"><a name="_Toc395637771"></a>Elemek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4819-265"><a name="_Toc395637771"></a>Adding Items</span></span>
<span data-ttu-id="b4819-266">Tegyünk néhány elemet az adatbázisba, hogy ne több mint egy üres táblát toolook címen.</span><span class="sxs-lookup"><span data-stu-id="b4819-266">Let's put some items into our database so we have something more than an empty grid toolook at.</span></span>

<span data-ttu-id="b4819-267">Adjunk néhány kódot túl Azure Cosmos DBRepository és ItemController toopersist hello rekordjának Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b4819-267">Let's add some code too Azure Cosmos DBRepository and ItemController toopersist hello record in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="b4819-268">Adja hozzá a következő metódus tooyour hello **DocumentDBRepository** osztály.</span><span class="sxs-lookup"><span data-stu-id="b4819-268">Add hello following method tooyour **DocumentDBRepository** class.</span></span>
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   <span data-ttu-id="b4819-269">Ez a metódus egyszerűen vesz igénybe egy tooit átadott objektum, és továbbra is fennáll az Azure Cosmos Adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="b4819-269">This method simply takes an object passed tooit and persists it in Azure Cosmos DB.</span></span>
2. <span data-ttu-id="b4819-270">Nyissa meg a hello ItemController.cs fájlt, és adja hozzá a következő kódrészletet hello osztályon belül hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-270">Open hello ItemController.cs file and add hello following code snippet within hello class.</span></span> <span data-ttu-id="b4819-271">Ez az ASP.NET MVC így tudja a hello milyen toodo **létrehozása** művelet.</span><span class="sxs-lookup"><span data-stu-id="b4819-271">This is how ASP.NET MVC knows what toodo for hello **Create** action.</span></span> <span data-ttu-id="b4819-272">Ebben az esetben csak leképezési hello társított Create.cshtml nézetet a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b4819-272">In this case just render hello associated Create.cshtml view created earlier.</span></span>
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    <span data-ttu-id="b4819-273">Most több kódra ebben a vezérlőben, amely elfogadja a hello hello küldésének kell **létrehozása** nézet.</span><span class="sxs-lookup"><span data-stu-id="b4819-273">We now need some more code in this controller that will accept hello submission from hello **Create** view.</span></span>
3. <span data-ttu-id="b4819-274">Adja hozzá a következő kódblokkot kód toohello ItemController.cs osztályhoz, amely közli az ASP.NET MVC és az ezen vezérlőből POST űrlapművelettel milyen toodo hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-274">Add hello next block of code toohello ItemController.cs class that tells ASP.NET MVC what toodo with a form POST for this controller.</span></span>
   
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
   
    <span data-ttu-id="b4819-275">Ez a kód a toohello DocumentDBRepository hívja, és hello CreateItemAsync metódus toopersist hello új teendő elem toohello adatbázist használ.</span><span class="sxs-lookup"><span data-stu-id="b4819-275">This code calls in toohello DocumentDBRepository and uses hello CreateItemAsync method toopersist hello new todo item toohello database.</span></span> 
   
    <span data-ttu-id="b4819-276">**Biztonsági megjegyzés**: hello **ValidateAntiForgeryToken** attribútum itt a toohelp az alkalmazást a webhelyközi kérések hamisításának megakadályozása támadások elleni védelméhez.</span><span class="sxs-lookup"><span data-stu-id="b4819-276">**Security Note**: hello **ValidateAntiForgeryToken** attribute is used here toohelp protect this application against cross-site request forgery attacks.</span></span> <span data-ttu-id="b4819-277">Ez az attribútum hozzáadásánál többről további tooit, a nézetek a hamisítás elleni tokennel rendelkező toowork kell.</span><span class="sxs-lookup"><span data-stu-id="b4819-277">There is more tooit than just adding this attribute, your views need toowork with this anti-forgery token as well.</span></span> <span data-ttu-id="b4819-278">További hello tulajdonos, és hogyan tooimplement ennek megfelelően, ellenőrizze a példák [megakadályozza a Webhelyközi kérések hamisításának megakadályozása][Preventing Cross-Site Request Forgery].</span><span class="sxs-lookup"><span data-stu-id="b4819-278">For more on hello subject, and examples of how tooimplement this correctly, please see [Preventing Cross-Site Request Forgery][Preventing Cross-Site Request Forgery].</span></span> <span data-ttu-id="b4819-279">közzétett forráskódban hello [GitHub] [ GitHub] hello teljes körű rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b4819-279">hello source code provided on [GitHub][GitHub] has hello full implementation in place.</span></span>
   
    <span data-ttu-id="b4819-280">**Biztonsági megjegyzés**: hello is használunk **kötési** hello metódus paraméter toohelp attribútum túlküldéses támadások elleni védelem érdekében.</span><span class="sxs-lookup"><span data-stu-id="b4819-280">**Security Note**: We also use hello **Bind** attribute on hello method parameter toohelp protect against over-posting attacks.</span></span> <span data-ttu-id="b4819-281">További részletekért lásd: [Alapvető CRUD műveletek az ASP.NET MVC-ben][Basic CRUD Operations in ASP.NET MVC].</span><span class="sxs-lookup"><span data-stu-id="b4819-281">For more details please see [Basic CRUD Operations in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].</span></span>

<span data-ttu-id="b4819-282">Ennyi lenne hello szükséges kód tooadd új elemek tooour adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4819-282">This concludes hello code required tooadd new Items tooour database.</span></span>

### <span data-ttu-id="b4819-283"><a name="_Toc395637772"></a>Elemek szerkesztése</span><span class="sxs-lookup"><span data-stu-id="b4819-283"><a name="_Toc395637772"></a>Editing Items</span></span>
<span data-ttu-id="b4819-284">Egyik utolsó teendő azon toodo, és ez tooadd hello képességét tooedit **elemek** hello adatbázisban és toomark őket, végezze el.</span><span class="sxs-lookup"><span data-stu-id="b4819-284">There is one last thing for us toodo, and that is tooadd hello ability tooedit **Items** in hello database and toomark them as complete.</span></span> <span data-ttu-id="b4819-285">hello szerkesztésre szolgáló nézet már fel van véve toohello projekt, ezért azt kell tooadd néhány kód tooour vezérlő és toohello **DocumentDBRepository** osztályból.</span><span class="sxs-lookup"><span data-stu-id="b4819-285">hello view for editing was already added toohello project, so we just need tooadd some code tooour controller and toohello **DocumentDBRepository** class again.</span></span>

1. <span data-ttu-id="b4819-286">Adja hozzá a következő toohello hello **DocumentDBRepository** osztály.</span><span class="sxs-lookup"><span data-stu-id="b4819-286">Add hello following toohello **DocumentDBRepository** class.</span></span>
   
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
   
    <span data-ttu-id="b4819-287">Ezek a módszerek közül első hello **GetItem** egy elemet kér le a Azure Cosmos-Adatbázisból, amelyet vissza toohello **ItemController** majd a toohello **szerkesztése** nézet.</span><span class="sxs-lookup"><span data-stu-id="b4819-287">hello first of these methods, **GetItem** fetches an Item from Azure Cosmos DB which is passed back toohello **ItemController** and then on toohello **Edit** view.</span></span>
   
    <span data-ttu-id="b4819-288">második hello módszerek hello épp most lett felvéve cserél hello **dokumentum** az Azure Cosmos Adatbázisba hello hello verziójával **dokumentum** hello átadott **ItemController**.</span><span class="sxs-lookup"><span data-stu-id="b4819-288">hello second of hello methods we just added replaces hello **Document** in Azure Cosmos DB with hello version of hello **Document** passed in from hello **ItemController**.</span></span>
2. <span data-ttu-id="b4819-289">Adja hozzá a következő toohello hello **ItemController** osztály.</span><span class="sxs-lookup"><span data-stu-id="b4819-289">Add hello following toohello **ItemController** class.</span></span>
   
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
   
    <span data-ttu-id="b4819-290">hello első módszer leírók hello Http GET, amely történik, amikor hello felhasználó kattint az hello **szerkesztése** hello mutató hivatkozást **Index** nézet.</span><span class="sxs-lookup"><span data-stu-id="b4819-290">hello first method handles hello Http GET that happens when hello user clicks on hello **Edit** link from hello **Index** view.</span></span> <span data-ttu-id="b4819-291">Ez a módszer lekéri a [ **dokumentum** ](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) a Azure Cosmos-Adatbázisból, és átadja toohello **szerkesztése** nézet.</span><span class="sxs-lookup"><span data-stu-id="b4819-291">This method fetches a [**Document**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) from Azure Cosmos DB and passes it toohello **Edit** view.</span></span>
   
    <span data-ttu-id="b4819-292">Hello **szerkesztése** nézet tegye egy Http POST toohello **IndexController**.</span><span class="sxs-lookup"><span data-stu-id="b4819-292">hello **Edit** view will then do an Http POST toohello **IndexController**.</span></span> 
   
    <span data-ttu-id="b4819-293">hello második sikeres frissítése hello objektum tooAzure Cosmos DB toobe leírók hozzáadott metódus hello adatbázisban maradnak.</span><span class="sxs-lookup"><span data-stu-id="b4819-293">hello second method we added handles passing hello updated object tooAzure Cosmos DB toobe persisted in hello database.</span></span>

<span data-ttu-id="b4819-294">Ennyi, amire szükségünk van toorun az alkalmazás által, a lista nem teljes **elemek**, hozzáadhat új **elemek**, és szerkesztheti a **elemek**.</span><span class="sxs-lookup"><span data-stu-id="b4819-294">That's it, that is everything we need toorun our application, list incomplete **Items**, add new **Items**, and edit **Items**.</span></span>

## <span data-ttu-id="b4819-295"><a name="_Toc395637773"></a>6. lépés: Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="b4819-295"><a name="_Toc395637773"></a>Step 6: Run hello application locally</span></span>
<span data-ttu-id="b4819-296">a helyi gépén, tootest hello alkalmazás hello a következő:</span><span class="sxs-lookup"><span data-stu-id="b4819-296">tootest hello application on your local machine, do hello following:</span></span>

1. <span data-ttu-id="b4819-297">Kattintson az F5 billentyűt a Visual Studio toobuild hello alkalmazás hibakeresési módban.</span><span class="sxs-lookup"><span data-stu-id="b4819-297">Hit F5 in Visual Studio toobuild hello application in debug mode.</span></span> <span data-ttu-id="b4819-298">Ez a kell hello alkalmazás létrehozása, és elindít egy böngészőt, a hello üres rácsoldallal korábban látott:</span><span class="sxs-lookup"><span data-stu-id="b4819-298">It should build hello application and launch a browser with hello empty grid page we saw before:</span></span>
   
    ![Képernyőfelvétel a hello teendőlista webalkalmazás adatbázis-oktatóprogram során létrehozott](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. <span data-ttu-id="b4819-300">Kattintson a hello **hozzon létre új** hivatkozásra, és adjon értékeket toohello **neve** és **leírás** mezők.</span><span class="sxs-lookup"><span data-stu-id="b4819-300">Click hello **Create New** link and add values toohello **Name** and **Description** fields.</span></span> <span data-ttu-id="b4819-301">Hagyja hello **befejezve** jelölőnégyzet nincs bejelölve egyéb hello új **elem** befejezett állapotban megjelenik, és nem jelenik meg a kiindulási lista hello.</span><span class="sxs-lookup"><span data-stu-id="b4819-301">Leave hello **Completed** check box unselected otherwise hello new **Item** will be added in a completed state and will not appear on hello initial list.</span></span>
   
    ![Képernyőfelvétel a hello nézet létrehozása](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. <span data-ttu-id="b4819-303">Kattintson a **létrehozása** és átirányított hátsó toohello **Index** megtekintése és a **elem** hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4819-303">Click **Create** and you are redirected back toohello **Index** view and your **Item** appears in hello list.</span></span>
   
    ![Képernyőfelvétel a hello Index nézetről](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    <span data-ttu-id="b4819-305">Érzi, hogy a szabad tooadd még néhány **elemek** tooyour teendőlistában.</span><span class="sxs-lookup"><span data-stu-id="b4819-305">Feel free tooadd a few more **Items** tooyour todo list.</span></span>
    
4. <span data-ttu-id="b4819-306">Kattintson a **szerkesztése** következő tooan **elem** hello listán, és készít a toohello **szerkesztése** nézet, ahol frissítheti az objektum, például hello bármely tulajdonságát  **Befejeződött** jelzőt.</span><span class="sxs-lookup"><span data-stu-id="b4819-306">Click **Edit** next tooan **Item** on hello list and you are taken toohello **Edit** view where you can update any property of your object, including hello **Completed** flag.</span></span> <span data-ttu-id="b4819-307">Ha meg van jelölve, akkor a hello **Complete** tulajdonsággal, és kattintson a **mentése**, hello **elem** eltávolítják az hello a hiányos feladatok listájából.</span><span class="sxs-lookup"><span data-stu-id="b4819-307">If you mark hello **Complete** flag and click **Save**, hello **Item** is removed from hello list of incomplete tasks.</span></span>
   
    ![Képernyőfelvétel a hello hello befejezve mezőben be van jelölve az Index nézetről](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. <span data-ttu-id="b4819-309">Miután hello alkalmazás tesztelését, nyomja le az Ctrl + F5 toostop hello app hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="b4819-309">Once you've tested hello app, press Ctrl+F5 toostop debugging hello app.</span></span> <span data-ttu-id="b4819-310">Most készen áll a toodeploy!</span><span class="sxs-lookup"><span data-stu-id="b4819-310">You're ready toodeploy!</span></span>

## <span data-ttu-id="b4819-311"><a name="_Toc395637774"></a>7. lépés: Hello alkalmazás tooAzure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="b4819-311"><a name="_Toc395637774"></a>Step 7: Deploy hello application tooAzure App Service</span></span> 
<span data-ttu-id="b4819-312">Most, hogy hello teljes alkalmazás megfelelően működik-e az Azure Cosmos DB programot fogjuk toodeploy a webes alkalmazás tooAzure App Service.</span><span class="sxs-lookup"><span data-stu-id="b4819-312">Now that you have hello complete application working correctly with Azure Cosmos DB we're going toodeploy this web app tooAzure App Service.</span></span>  

1. <span data-ttu-id="b4819-313">toopublish az alkalmazás összes toodo szüksége, kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="b4819-313">toopublish this application all you need toodo is right-click on hello project in **Solution Explorer** and click **Publish**.</span></span>
   
    ![Képernyőfelvétel a hello közzététel lehetőségről a Megoldáskezelőben](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. <span data-ttu-id="b4819-315">A hello **közzététel** párbeszédpanelen kattintson **Microsoft Azure App Service**, majd válassza **hozzon létre új** toocreate egy App Service profilt, vagy kattintson a **kiválasztása Meglévő** toouse egy meglévő profilt.</span><span class="sxs-lookup"><span data-stu-id="b4819-315">In hello **Publish** dialog box, click **Microsoft Azure App Service**, then select **Create New** toocreate an App Service profile, or click **Select Existing** toouse an existing profile.</span></span>

    ![A Visual Studio párbeszédpanel közzététele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. <span data-ttu-id="b4819-317">Ha egy meglévő Azure App Service-profilt, adja meg az előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="b4819-317">If you have an existing Azure App Service profile, enter your subscription name.</span></span> <span data-ttu-id="b4819-318">Használjon hello **nézet** toosort erőforráscsoport és erőforrások típus szerint szűrheti, majd válassza ki az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b4819-318">Use hello **View** filter toosort by resource group or resource type, then select your Azure App Service.</span></span> 
   
    ![A Visual Studio App Service párbeszédpanelen](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. <span data-ttu-id="b4819-320">toocreate egy új Azure App Service-profilt, kattintson a **hozzon létre új** a hello **közzététel** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="b4819-320">toocreate a new Azure App Service profile, click **Create New** in hello **Publish** dialog box.</span></span> <span data-ttu-id="b4819-321">A hello **létrehozása az App Service** párbeszédpanelen adja meg a webes alkalmazás neve és a megfelelő előfizetés, az erőforráscsoport és az App Service-csomag, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b4819-321">In hello **Create App Service** dialog, enter your Web App name and appropriate subscription, resource group, and App Service plan, then click **Create**.</span></span>

    ![App Service párbeszédpanelen létrehozása a Visual Studióban](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

<span data-ttu-id="b4819-323">Néhány másodpercen belül a Visual Studio befejezi a webalkalmazás közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!</span><span class="sxs-lookup"><span data-stu-id="b4819-323">In a few seconds, Visual Studio will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>



## <span data-ttu-id="b4819-324"><a name="_Toc395637775"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4819-324"><a name="_Toc395637775"></a>Next steps</span></span>
<span data-ttu-id="b4819-325">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="b4819-325">Congratulations!</span></span> <span data-ttu-id="b4819-326">Ebben az esetben az első ASP.NET MVC webalkalmazását az Azure Cosmos DB használatával építve, és közzétette azt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b4819-326">You just built your first ASP.NET MVC web application using Azure Cosmos DB and published it tooAzure.</span></span> <span data-ttu-id="b4819-327">hello hello teljes alkalmazás, beleértve a hello részletes forráskódja és törlési szolgáltatást, ez nem foglalt oktatóanyag is letölthető vagy klónozható a [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="b4819-327">hello source code for hello complete application, including hello detail and delete functionality that were not included in this tutorial can be downloaded or cloned from [GitHub][GitHub].</span></span> <span data-ttu-id="b4819-328">Ezért ha tooyour alkalmazást érdekli, adása hello kódot, és toothis alkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b4819-328">So if you're interested in adding that tooyour app, grab hello code and add it toothis app.</span></span>

<span data-ttu-id="b4819-329">tooadd további funkciók tooyour alkalmazás, tekintse át hello hello elérhető API-k [Azure Cosmos .NET kódtárban](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) és érzi, hogy a szabad toocontribute toohello Azure Cosmos .NET kódtárban a [GitHub] [GitHub].</span><span class="sxs-lookup"><span data-stu-id="b4819-329">tooadd additional functionality tooyour application, review hello APIs available in hello [Azure Cosmos DB .NET Library](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) and feel free toocontribute toohello Azure Cosmos DB .NET Library on [GitHub][GitHub].</span></span> 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
