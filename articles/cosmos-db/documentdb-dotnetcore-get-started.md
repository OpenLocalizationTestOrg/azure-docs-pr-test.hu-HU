---
title: "Azure Cosmos DB: DocumentDB API bevezetés .NET Core oktatóanyaggal | Microsoft Docs"
description: "Oktatóanyag, amely létrehoz egy online adatbázist és egy C# konzolalkalmazást az Azure Cosmos DB DocumentDB API .NET SDK Core használatával."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: jhubbard
editor: 
ms.assetid: 9f93e276-9936-4efb-a534-a9889fa7c7d2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 7536978bbb1e41b6484b66fd1b51c900fc3e545d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-getting-started-with-the-documentdb-api-and-net-core"></a><span data-ttu-id="1e18a-103">Azure Cosmos DB: DocumentDB API és .NET Core bevezetés</span><span class="sxs-lookup"><span data-stu-id="1e18a-103">Azure Cosmos DB: Getting started with the DocumentDB API and .NET Core</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e18a-104">.NET</span><span class="sxs-lookup"><span data-stu-id="1e18a-104">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="1e18a-105">.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e18a-105">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="1e18a-106">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="1e18a-106">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="1e18a-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="1e18a-107">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="1e18a-108">Java</span><span class="sxs-lookup"><span data-stu-id="1e18a-108">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="1e18a-109">C++</span><span class="sxs-lookup"><span data-stu-id="1e18a-109">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="1e18a-110">Üdvözöljük a DocumentDB API Azure Cosmos DB Ismerkedés a .NET Core-oktatóanyag!</span><span class="sxs-lookup"><span data-stu-id="1e18a-110">Welcome to the DocumentDB API for Azure Cosmos DB getting started with .NET Core tutorial!</span></span> <span data-ttu-id="1e18a-111">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="1e18a-111">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="1e18a-112">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="1e18a-112">We'll cover:</span></span>

* <span data-ttu-id="1e18a-113">Azure Cosmos DB-fiók létrehozása és csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="1e18a-113">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="1e18a-114">A Visual Studio megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1e18a-114">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="1e18a-115">Online adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e18a-115">Creating an online database</span></span>
* <span data-ttu-id="1e18a-116">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e18a-116">Creating a collection</span></span>
* <span data-ttu-id="1e18a-117">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e18a-117">Creating JSON documents</span></span>
* <span data-ttu-id="1e18a-118">A gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1e18a-118">Querying the collection</span></span>
* <span data-ttu-id="1e18a-119">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="1e18a-119">Replacing a document</span></span>
* <span data-ttu-id="1e18a-120">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="1e18a-120">Deleting a document</span></span>
* <span data-ttu-id="1e18a-121">Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="1e18a-121">Deleting the database</span></span>

<span data-ttu-id="1e18a-122">Nincs elég ideje?</span><span class="sxs-lookup"><span data-stu-id="1e18a-122">Don't have time?</span></span> <span data-ttu-id="1e18a-123">Ne aggódjon!</span><span class="sxs-lookup"><span data-stu-id="1e18a-123">Don't worry!</span></span> <span data-ttu-id="1e18a-124">A teljes megoldás elérhető a [GitHubon](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1e18a-124">The complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span> <span data-ttu-id="1e18a-125">A gyors utasításokért ugorjon [A teljes megoldás beszerzése szakaszra](#GetSolution).</span><span class="sxs-lookup"><span data-stu-id="1e18a-125">Jump to the [Get the complete solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="1e18a-126">A Xamarin iOS, Android vagy űrlapok szeretne a DocumentDB API és a .NET Core SDK használata alkalmazás?</span><span class="sxs-lookup"><span data-stu-id="1e18a-126">Want to build a Xamarin iOS, Android, or Forms application using the DocumentDB API and .NET Core SDK?</span></span> <span data-ttu-id="1e18a-127">Lásd: [használó alkalmazások mobil a Xamarinnal és Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="1e18a-127">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1e18a-128">Az Azure Cosmos DB .NET Core SDK ebben az oktatóanyagban használt még nem kompatibilis az univerzális Windows Platform (UWP-) alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="1e18a-128">The Azure Cosmos DB .NET Core SDK used in this tutorial is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="1e18a-129">A .NET Core SDK UWP-alkalmazásokat is támogató előzetes verziójáért küldjön e-mailt a következő címre: [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="1e18a-129">For a preview version of the .NET Core SDK that does support UWP apps, send email to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

<span data-ttu-id="1e18a-130">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="1e18a-130">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e18a-131">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1e18a-131">Prerequisites</span></span>
<span data-ttu-id="1e18a-132">Győződjön meg róla, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="1e18a-132">Please make sure you have the following:</span></span>

* <span data-ttu-id="1e18a-133">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="1e18a-133">An active Azure account.</span></span> <span data-ttu-id="1e18a-134">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1e18a-134">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="1e18a-135">Másik lehetőségként használhatja az [Azure Cosmos DB Emulatort](local-emulator.md) az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="1e18a-135">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="1e18a-136">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1e18a-136">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/) 
    * <span data-ttu-id="1e18a-137">Ha MacOS vagy Linux rendszeren dolgozik, a parancssorból is fejleszthet .NET Core-alkalmazásokat, ha telepíti a [.NET Core SDK-t](https://www.microsoft.com/net/core#macos) a választott platformra.</span><span class="sxs-lookup"><span data-stu-id="1e18a-137">If you're working on MacOS or Linux, you can develop .NET Core apps from the command-line by installing the [.NET Core SDK](https://www.microsoft.com/net/core#macos) for the platform of your choice.</span></span> 
    * <span data-ttu-id="1e18a-138">Ha Windows rendszeren dolgozik, a parancssorból is fejleszthet .NET Core alkalmazásokat, ha telepíti a [.NET Core SDK-t](https://www.microsoft.com/net/core#windows).</span><span class="sxs-lookup"><span data-stu-id="1e18a-138">If you're working on Windows, you can develop .NET Core apps from the command-line by installing the [.NET Core SDK](https://www.microsoft.com/net/core#windows).</span></span> 
    * <span data-ttu-id="1e18a-139">Használhat saját szerkesztőt is, vagy letöltheti az ingyenes [Visual Studio Code](https://code.visualstudio.com/) alkalmazást, amely Windows, Linux és MacOS rendszeren egyaránt működik.</span><span class="sxs-lookup"><span data-stu-id="1e18a-139">You can use your own editor, or download [Visual Studio Code](https://code.visualstudio.com/) which is free and works on Windows, Linux, and MacOS.</span></span> 

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="1e18a-140">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e18a-140">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="1e18a-141">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="1e18a-141">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="1e18a-142">Ha van már olyan fiókja, amelyet használni szeretne, ugorjon előre a [Visual Studio megoldás beállítása](#SetupVS) című lépésre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-142">If you already have an account you want to use, you can skip ahead to [Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="1e18a-143">Ha az Azure Cosmos DB Emulatort használja, kövesse az [Azure Cosmos DB Emulatornál](local-emulator.md) leírt lépéseket az emulátor telepítéséhez, majd ugorjon előre a [Visual Studio Solution beállítása](#SetupVS) című lépésre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-143">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="1e18a-144"><a id="SetupVS"></a>2. lépés: A Visual Studio megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="1e18a-144"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="1e18a-145">Nyissa meg a **Visual Studio 2017-et** a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="1e18a-145">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="1e18a-146">A **Fájl** menüben válassza az **Új**, majd a **Projekt** elemet.</span><span class="sxs-lookup"><span data-stu-id="1e18a-146">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="1e18a-147">A **New Project** (Új projekt) párbeszédpanelen válassza a **Templates** (Sablonok)  / **Visual C#** / **.NET Core**/**Console Application (.NET Core)** (Konzolalkalmazás (.NET Core)) elemet, adja a projektnek a **DocumentDBGettingStarted** nevet, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e18a-147">In the **New Project** dialog, select **Templates** / **Visual C#** / **.NET Core**/**Console Application (.NET Core)**, name your project **DocumentDBGettingStarted**, and then click **OK**.</span></span>

   ![Képernyőfelvétel az Új projekt ablakról](./media/documentdb-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. <span data-ttu-id="1e18a-149">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a **DocumentDBGettingStarted** elemre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-149">In the **Solution Explorer**, right click on **DocumentDBGettingStarted**.</span></span>
5. <span data-ttu-id="1e18a-150">Ezután továbbra is a menüben kattintson a **Manage NuGet Packages…** (NuGet-csomagok kezelése…) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-150">Then without leaving the menu, click on **Manage NuGet Packages...**.</span></span>

   ![A Projekt jobb gombos kattintással elérhető menüjének képernyőfelvétele](./media/documentdb-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. <span data-ttu-id="1e18a-152">A **NuGet** lapon kattintson a **Browse** (Tallózás) elemre az ablak tetején, majd írja be az **azure documentdb** kifejezést a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="1e18a-152">In the **NuGet** tab, click **Browse** at the top of the window, and type **azure documentdb** in the search box.</span></span>
7. <span data-ttu-id="1e18a-153">A találatok között keresse meg a **Microsoft.Azure.DocumentDB.Core** elemet, majd kattintson a **Telepítés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-153">Within the results, find **Microsoft.Azure.DocumentDB.Core** and click **Install**.</span></span>
   <span data-ttu-id="1e18a-154">A .NET Core DocumentDB ügyfélkódtárának csomagazonosítója a következő: [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span><span class="sxs-lookup"><span data-stu-id="1e18a-154">The package ID for the DocumentDB Client Library for .NET Core is [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span></span> <span data-ttu-id="1e18a-155">Ha a .NET-keretrendszer olyan verzióját célozza meg (például net461), amelyet ez a .NET Core NuGet csomag nem támogat, használja a [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)-t, amely a .NET-keretrendszer 4.5-ös verziójától kezdődően minden verziót támogat.</span><span class="sxs-lookup"><span data-stu-id="1e18a-155">If you are targeting a .NET Framework version (like net461) that is not supported by this .NET Core NuGet package, then use [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) that supports all .NET Framework versions starting .NET Framework 4.5.</span></span>
8. <span data-ttu-id="1e18a-156">Amikor a rendszer kéri, fogadja el a NuGet-csomag telepítését és a licencmegállapodást.</span><span class="sxs-lookup"><span data-stu-id="1e18a-156">At the prompts, accept the NuGet package installations and the license agreement.</span></span>

<span data-ttu-id="1e18a-157">Remek!</span><span class="sxs-lookup"><span data-stu-id="1e18a-157">Great!</span></span> <span data-ttu-id="1e18a-158">Most, hogy befejeztük a beállítást, lássunk neki a kód megírásának!</span><span class="sxs-lookup"><span data-stu-id="1e18a-158">Now that we finished the setup, let's start writing some code.</span></span> <span data-ttu-id="1e18a-159">A [GitHubon](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) megtalálhatja az oktatóanyagban szereplő kódprojekt befejezett változatát.</span><span class="sxs-lookup"><span data-stu-id="1e18a-159">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span>

## <span data-ttu-id="1e18a-160"><a id="Connect"></a>3. lépés: Csatlakozás egy Azure Cosmos DB-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="1e18a-160"><a id="Connect"></a>Step 3: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="1e18a-161">Először adja hozzá az alábbi hivatkozásokat a C# alkalmazás elejéhez a Program.cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="1e18a-161">First, add these references to the beginning of your C# application, in the Program.cs file:</span></span>

```csharp
using System;

// ADD THIS PART TO YOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

> [!IMPORTANT]
> <span data-ttu-id="1e18a-162">Az oktatóanyag befejezéséhez mindenképpen adja hozzá az alábbi függőségeket.</span><span class="sxs-lookup"><span data-stu-id="1e18a-162">In order to complete this tutorial, make sure you add the dependencies above.</span></span>

<span data-ttu-id="1e18a-163">Most adja hozzá ezt a két állandót és az *ügyfél* változót a *Program* nyilvános osztály alatt.</span><span class="sxs-lookup"><span data-stu-id="1e18a-163">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

<span data-ttu-id="1e18a-164">Ezután látogasson el az [Azure-portálra](https://portal.azure.com) az URI és az elsődleges kulcs beszerzéséért.</span><span class="sxs-lookup"><span data-stu-id="1e18a-164">Next, head to the [Azure Portal](https://portal.azure.com) to retrieve your URI and primary key.</span></span> <span data-ttu-id="1e18a-165">Az Azure Cosmos DB URI és primary key szükség, hogy hova kell csatlakoznia, hogy az alkalmazás, illetve Azure Cosmos DB megbízik az alkalmazás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1e18a-165">The Azure Cosmos DB URI and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span>

<span data-ttu-id="1e18a-166">Az Azure Portalon lépjen a Azure Cosmos DB-fiókra, majd kattintson a **Kulcsok** elemre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-166">In the Azure Portal, navigate to your Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="1e18a-167">Másolja ki az URI-t a portálról, és illessze be a program.cs fájl `<your endpoint URI>` elemébe.</span><span class="sxs-lookup"><span data-stu-id="1e18a-167">Copy the URI from the portal and paste it into `<your endpoint URI>` in the program.cs file.</span></span> <span data-ttu-id="1e18a-168">Ezután másolja ki a PRIMARY KEY kulcsot a portálról, és illessze be a `<your key>` elembe.</span><span class="sxs-lookup"><span data-stu-id="1e18a-168">Then copy the PRIMARY KEY from the portal and paste it into `<your key>`.</span></span> <span data-ttu-id="1e18a-169">Ha az Azure Cosmos DB Emulatort használja, használjon `https://localhost:8081` értéket végpontként, valamint a jól definiált engedélyezési kulcsot a [Fejlesztés az Azure Cosmos DB Emulator használatával](local-emulator.md) című részből.</span><span class="sxs-lookup"><span data-stu-id="1e18a-169">If you are using the Azure Cosmos DB Emulator, use `https://localhost:8081` as the endpoint, and the well-defined authorization key from [How to develop using the Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="1e18a-170">Mindenképp távolítsa el a < és a > jelet, azonban az idézőjeleket hagyja meg a végpont és a kulcs körül.</span><span class="sxs-lookup"><span data-stu-id="1e18a-170">Make sure to remove the < and > but leave the double quotes around your endpoint and key.</span></span>

![Képernyőfelvétel a NoSQL-oktatóanyagban a C# konzolalkalmazás létrehozásához használt Azure-portálról.][keys]

<span data-ttu-id="1e18a-173">Először létrehozunk egy új **DocumentClient** példányt az első lépések alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="1e18a-173">We'll start the getting started application by creating a new instance of the **DocumentClient**.</span></span>

<span data-ttu-id="1e18a-174">A **Fő** metódus alatt adja hozzá a **GetStartedDemo** elnevezésű új aszinkron feladatot, amely létrehozza nekünk az új **DocumentClient** példányt.</span><span class="sxs-lookup"><span data-stu-id="1e18a-174">Below the **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART TO YOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

<span data-ttu-id="1e18a-175">Adja hozzá a következő kódot az aszinkron feladat **Fő** metódusból való futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-175">Add the following code to run your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="1e18a-176">A **Fő** metódus észleli a kivételeket, és a konzolba írja azokat.</span><span class="sxs-lookup"><span data-stu-id="1e18a-176">The **Main** method will catch exceptions and write them to the console.</span></span>

```csharp
static void Main(string[] args)
{
        // ADD THIS PART TO YOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key to exit.");
                Console.ReadKey();
        }
```

<span data-ttu-id="1e18a-177">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás létrehozásához és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-177">Press the **DocumentDBGettingStarted** button to build and run the application.</span></span>

<span data-ttu-id="1e18a-178">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-178">Congratulations!</span></span> <span data-ttu-id="1e18a-179">Sikeresen csatlakozott egy Azure Cosmos DB-fiókhoz. Most vessünk egy pillantást az Azure Cosmos DB-erőforrások használatára.</span><span class="sxs-lookup"><span data-stu-id="1e18a-179">You have successfully connected to an Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="1e18a-180">4. lépés: Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e18a-180">Step 4: Create a database</span></span>
<span data-ttu-id="1e18a-181">Mielőtt hozzáadja a kódot az adatbázis létrehozásához, adjon hozzá egy segédmetódust a konzolba való íráshoz.</span><span class="sxs-lookup"><span data-stu-id="1e18a-181">Before you add the code for creating a database, add a helper method for writing to the console.</span></span>

<span data-ttu-id="1e18a-182">Másolja, majd illessze be a **WriteToConsoleAndPromptToContinue** metódust a **GetStartedDemo** metódus alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-182">Copy and paste the **WriteToConsoleAndPromptToContinue** method underneath the **GetStartedDemo** method.</span></span>

```csharp
// ADD THIS PART TO YOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

<span data-ttu-id="1e18a-183">Az Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) használatával hozható létre a [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metódusában a **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="1e18a-183">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using the [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="1e18a-184">Az adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója.</span><span class="sxs-lookup"><span data-stu-id="1e18a-184">A database is the logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="1e18a-185">Másolja, majd illessze be az alábbi kódot a **GetStartedDemo** metódusba az ügyfél létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="1e18a-185">Copy and paste the following code to your **GetStartedDemo** method underneath the client creation.</span></span> <span data-ttu-id="1e18a-186">Ezzel létrehoz egy *FamilyDB* elnevezésű adatbázist.</span><span class="sxs-lookup"><span data-stu-id="1e18a-186">This will create a database named *FamilyDB*.</span></span>

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

<span data-ttu-id="1e18a-187">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-187">Press the **DocumentDBGettingStarted** button to run your application.</span></span>

<span data-ttu-id="1e18a-188">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-188">Congratulations!</span></span> <span data-ttu-id="1e18a-189">Sikeresen létrehozott egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="1e18a-189">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="1e18a-190"><a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e18a-190"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="1e18a-191">A **CreateDocumentCollectionAsync** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után.</span><span class="sxs-lookup"><span data-stu-id="1e18a-191">**CreateDocumentCollectionAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="1e18a-192">További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="1e18a-192">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>

<span data-ttu-id="1e18a-193">Egy [gyűjtemény](documentdb-resources.md#collections) a **DocumentClient** osztály [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metódusának használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-193">A [collection](documentdb-resources.md#collections) can be created by using the [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="1e18a-194">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="1e18a-194">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="1e18a-195">Másolja, majd illessze be az alábbi kódot a **GetStartedDemo** metódusba az adatbázis létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="1e18a-195">Copy and paste the following code to your **GetStartedDemo** method underneath the database creation.</span></span> <span data-ttu-id="1e18a-196">Ezzel létrehoz egy *FamilyCollection_oa* elnevezésű dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="1e18a-196">This will create a document collection named *FamilyCollection_oa*.</span></span>

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExists("FamilyDB_oa");

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

<span data-ttu-id="1e18a-197">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-197">Press the **DocumentDBGettingStarted** button to run your application.</span></span>

<span data-ttu-id="1e18a-198">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-198">Congratulations!</span></span> <span data-ttu-id="1e18a-199">Sikeresen létrehozott egy Azure Cosmos DB-dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="1e18a-199">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="1e18a-200"><a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e18a-200"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="1e18a-201">A [dokumentumok](documentdb-resources.md#documents) a **DocumentClient** osztály [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metódusának használatával hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-201">A [document](documentdb-resources.md#documents) can be created by using the [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="1e18a-202">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="1e18a-202">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="1e18a-203">Most már beilleszthetünk egy vagy több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="1e18a-203">We can now insert one or more documents.</span></span> <span data-ttu-id="1e18a-204">Ha már rendelkezik az adatbázisban tárolni kívánt adatokat, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="1e18a-204">If you already have data you'd like to store in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md).</span></span>

<span data-ttu-id="1e18a-205">Először létre kell hozni egy **Család** osztályt, amely ebben a mintában az Azure Cosmos DB-ben tárolt objektumokat képviseli.</span><span class="sxs-lookup"><span data-stu-id="1e18a-205">First, we need to create a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="1e18a-206">Létrehozunk még egy **Szülő**, **Gyermek**, **Háziállat** és **Cím** alosztályt is a **Család** osztályban való használatra.</span><span class="sxs-lookup"><span data-stu-id="1e18a-206">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="1e18a-207">Ne feledje, hogy a dokumentumoknak rendelkezniük kell egy **Azonosító** tulajdonsággal, amely a JSON-fájlban **id**-ként van szerializálva.</span><span class="sxs-lookup"><span data-stu-id="1e18a-207">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="1e18a-208">Az osztályok létrehozásához adja hozzá az alábbi belső alosztályokat a **GetStartedDemo** metódus után.</span><span class="sxs-lookup"><span data-stu-id="1e18a-208">Create these classes by adding the following internal sub-classes after the **GetStartedDemo** method.</span></span>

<span data-ttu-id="1e18a-209">Másolja, majd illessze be a **Család**, **Szülő**, **Gyermek**, **Háziállat** és **Cím** osztályokat a **WriteToConsoleAndPromptToContinue** metódus alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-209">Copy and paste the **Family**, **Parent**, **Child**, **Pet**, and **Address** classes underneath the **WriteToConsoleAndPromptToContinue** method.</span></span>

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key to continue ...");
    Console.ReadKey();
}

// ADD THIS PART TO YOUR CODE
public class Family
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public string LastName { get; set; }
    public Parent[] Parents { get; set; }
    public Child[] Children { get; set; }
    public Address Address { get; set; }
    public bool IsRegistered { get; set; }
    public override string ToString()
    {
            return JsonConvert.SerializeObject(this);
    }
}

public class Parent
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
}

public class Child
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
    public string Gender { get; set; }
    public int Grade { get; set; }
    public Pet[] Pets { get; set; }
}

public class Pet
{
    public string GivenName { get; set; }
}

public class Address
{
    public string State { get; set; }
    public string County { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="1e18a-210">Másolja, majd illessze be a **CreateFamilyDocumentIfNotExists** metódust a **CreateDocumentCollectionIfNotExists** metódus alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-210">Copy and paste the **CreateFamilyDocumentIfNotExists** method underneath your **CreateDocumentCollectionIfNotExists** method.</span></span>

```csharp
// ADD THIS PART TO YOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

<span data-ttu-id="1e18a-211">Ezután szúrjon be két dokumentumot, egyet az Andersen családhoz, egyet pedig a Wakefield családhoz.</span><span class="sxs-lookup"><span data-stu-id="1e18a-211">And insert two documents, one each for the Andersen Family and the Wakefield Family.</span></span>

<span data-ttu-id="1e18a-212">Másolja, majd illessze be a `// ADD THIS PART TO YOUR CODE` után álló kódot a **GetStartedDemo** metódusba, a dokumentumgyűjtemény létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="1e18a-212">Copy and paste the code that follows `// ADD THIS PART TO YOUR CODE` to your **GetStartedDemo** method underneath the document collection creation.</span></span>

```csharp
await this.CreateDatabaseIfNotExists("FamilyDB_oa");

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FirstName = "Henriette Thaulow",
                        Gender = "female",
                        Grade = 5,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Fluffy" }
                        }
                }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FamilyName = "Merriam",
                        FirstName = "Jesse",
                        Gender = "female",
                        Grade = 8,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Goofy" },
                                new Pet { GivenName = "Shadow" }
                        }
                },
                new Child
                {
                        FamilyName = "Miller",
                        FirstName = "Lisa",
                        Gender = "female",
                        Grade = 1
                }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

<span data-ttu-id="1e18a-213">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-213">Press the **DocumentDBGettingStarted** button to run your application.</span></span>

<span data-ttu-id="1e18a-214">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-214">Congratulations!</span></span> <span data-ttu-id="1e18a-215">Sikeresen létrehozott két Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="1e18a-215">You have successfully created two Azure Cosmos DB documents.</span></span>  

![A diagram a NoSQL-oktatóanyagban a C# konzolalkalmazás létrehozásához használt fiók, online adatbázis, gyűjtemény és dokumentumok hierarchikus kapcsolatát ábrázolja.](./media/documentdb-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="1e18a-217"><a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1e18a-217"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="1e18a-218">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="1e18a-218">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="1e18a-219">Az alábbi kódminta több olyan lekérdezést mutat be – az Azure Cosmos DB SQL-szintaxis és a LINQ használatával egyaránt – amelyeket az előző lépésben beszúrt dokumentumokon futtathatunk.</span><span class="sxs-lookup"><span data-stu-id="1e18a-219">The following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against the documents we inserted in the previous step.</span></span>

<span data-ttu-id="1e18a-220">Másolja, majd illessze be a **ExecuteSimpleQuery** metódust a **CreateFamilyDocumentIfNotExists** metódus alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-220">Copy and paste the **ExecuteSimpleQuery** method underneath your **CreateFamilyDocumentIfNotExists** method.</span></span>

```csharp
// ADD THIS PART TO YOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find the Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute the same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

<span data-ttu-id="1e18a-221">Másolja, majd illessze be az alábbi kódot a **GetStartedDemo** metódusba a második dokumentum létrehozása alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-221">Copy and paste the following code to your **GetStartedDemo** method underneath the second document creation.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART TO YOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="1e18a-222">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-222">Press the **DocumentDBGettingStarted** button to run your application.</span></span>

<span data-ttu-id="1e18a-223">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-223">Congratulations!</span></span> <span data-ttu-id="1e18a-224">Sikeres lekérdezést végzett egy Azure Cosmos DB-gyűjteményen.</span><span class="sxs-lookup"><span data-stu-id="1e18a-224">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="1e18a-225">Az alábbi diagram bemutatja, hogyan indít hívást az Azure Cosmos DB SQL-lekérdezési szintaxisa a létrehozott gyűjteményre. Ugyanez a logika vonatkozik a LINQ-lekérdezésekre is.</span><span class="sxs-lookup"><span data-stu-id="1e18a-225">The following diagram illustrates how the Azure Cosmos DB SQL query syntax is called against the collection you created, and the same logic applies to the LINQ query as well.</span></span>

![A NoSQL-oktatóanyagban a C# konzolalkalmazás létrehozásához használt lekérdezés hatókörét és jelentését ábrázoló diagram.](./media/documentdb-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="1e18a-227">A [FROM](documentdb-sql-query.md#FromClause) kulcsszó kihagyható a lekérdezésből mert Azure Cosmos adatbázis-lekérdezések hatóköre eleve egyetlen gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="1e18a-227">The [FROM](documentdb-sql-query.md#FromClause) keyword is optional in the query because Azure Cosmos DB queries are already scoped to a single collection.</span></span> <span data-ttu-id="1e18a-228">Ezért a „FROM Families f” lecserélhető a „FROM root r” vagy bármilyen tetszőleges változónévre.</span><span class="sxs-lookup"><span data-stu-id="1e18a-228">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="1e18a-229">A DocumentDB úgy veszi, hogy a Families, a root vagy a tetszőleges változónév alapértelmezés szerint az aktuális gyűjteményre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="1e18a-229">DocumentDB will infer that Families, root, or the variable name you chose, reference the current collection by default.</span></span>

## <span data-ttu-id="1e18a-230"><a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje</span><span class="sxs-lookup"><span data-stu-id="1e18a-230"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="1e18a-231">Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="1e18a-231">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="1e18a-232">Másolja, majd illessze be a **ReplaceFamilyDocument** metódust az **ExecuteSimpleQuery** metódus alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-232">Copy and paste the **ReplaceFamilyDocument** method underneath your **ExecuteSimpleQuery** method.</span></span>

```csharp
// ADD THIS PART TO YOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    try
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
        this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

<span data-ttu-id="1e18a-233">Másolja, majd illessze be az alábbi kódot a **GetStartedDemo** metódusba a lekérdezés végrehajtása alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-233">Copy and paste the following code to your **GetStartedDemo** method underneath the query execution.</span></span> <span data-ttu-id="1e18a-234">Ezáltal a dokumentum cseréje után ismét ugyanaz a lekérdezés fog lefutni a megváltozott dokumentum megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1e18a-234">After replacing the document, this will run the same query again to view the changed document.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
// Update the Grade of the Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="1e18a-235">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-235">Press the **DocumentDBGettingStarted** button to run your application.</span></span>

<span data-ttu-id="1e18a-236">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-236">Congratulations!</span></span> <span data-ttu-id="1e18a-237">Sikeresen lecserélt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="1e18a-237">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="1e18a-238"><a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése</span><span class="sxs-lookup"><span data-stu-id="1e18a-238"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="1e18a-239">Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.</span><span class="sxs-lookup"><span data-stu-id="1e18a-239">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="1e18a-240">Másolja, majd illessze be a **DeleteFamilyDocument** metódust a **ReplaceFamilyDocument** metódus alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-240">Copy and paste the **DeleteFamilyDocument** method underneath your **ReplaceFamilyDocument** method.</span></span>

```csharp
// ADD THIS PART TO YOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    try
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine("Deleted Family {0}", documentName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

<span data-ttu-id="1e18a-241">Másolja, majd illessze be az alábbi kódot a **GetStartedDemo** metódusba a második lekérdezés végrehajtása alá.</span><span class="sxs-lookup"><span data-stu-id="1e18a-241">Copy and paste the following code to your **GetStartedDemo** method underneath the second query execution.</span></span>

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO CODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

<span data-ttu-id="1e18a-242">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-242">Press the **DocumentDBGettingStarted** button to run your application.</span></span>

<span data-ttu-id="1e18a-243">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-243">Congratulations!</span></span> <span data-ttu-id="1e18a-244">Sikeresen törölt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="1e18a-244">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="1e18a-245"><a id="DeleteDatabase"></a>10. lépés: Az adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="1e18a-245"><a id="DeleteDatabase"></a>Step 10: Delete the database</span></span>
<span data-ttu-id="1e18a-246">A létrehozott adatbázis törlésével az adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.) is törlődik.</span><span class="sxs-lookup"><span data-stu-id="1e18a-246">Deleting the created database will remove the database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="1e18a-247">Másolja, majd illessze be az alábbi kódot a **GetStartedDemo** metódusba a dokumentum törlése alá az adatbázis és az összes gyermek-erőforrás törléséhez.</span><span class="sxs-lookup"><span data-stu-id="1e18a-247">Copy and paste the following code to your **GetStartedDemo** method underneath the document delete to delete the entire database and all children resources.</span></span>

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART TO CODE
// Clean up/delete the database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

<span data-ttu-id="1e18a-248">Nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-248">Press the **DocumentDBGettingStarted** button to run your application.</span></span>

<span data-ttu-id="1e18a-249">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-249">Congratulations!</span></span> <span data-ttu-id="1e18a-250">Sikeresen törölt egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="1e18a-250">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="1e18a-251"><a id="Run"></a>11. lépés: Futtassa a teljes C# konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="1e18a-251"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="1e18a-252">A Visual Studióban nyomja meg a **DocumentDBGettingStarted** gombot az alkalmazás hibakeresési módban történő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1e18a-252">Press the **DocumentDBGettingStarted** button in Visual Studio to build the application in debug mode.</span></span>

<span data-ttu-id="1e18a-253">A konzolablakban az első lépések alkalmazás kimenetének kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1e18a-253">You should see the output of your get started app in the console window.</span></span> <span data-ttu-id="1e18a-254">A kimenet megjeleníti a hozzáadott lekérdezések eredményeit, amelynek meg kell egyeznie az alábbi mintaszöveggel.</span><span class="sxs-lookup"><span data-stu-id="1e18a-254">The output will show the results of the queries we added and should match the example text below.</span></span>

```
Created FamilyDB_oa
Press any key to continue ...
Created FamilyCollection_oa
Press any key to continue ...
Created Family Andersen.1
Press any key to continue ...
Created Family Wakefield.7
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key to exit.
```

<span data-ttu-id="1e18a-255">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1e18a-255">Congratulations!</span></span> <span data-ttu-id="1e18a-256">Elvégezte az oktatóanyagot, és egy működőképes C# konzolalkalmazással rendelkezik!</span><span class="sxs-lookup"><span data-stu-id="1e18a-256">You've completed the tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="1e18a-257"><a id="GetSolution"></a> Az oktatóanyagban szereplő teljes megoldás beszerzése</span><span class="sxs-lookup"><span data-stu-id="1e18a-257"><a id="GetSolution"></a> Get the complete tutorial solution</span></span>
<span data-ttu-id="1e18a-258">A cikkben szereplő összes mintát tartalmazó GetStarted-megoldás összeállításához az alábbiakra lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="1e18a-258">To build the GetStarted solution that contains all the samples in this article, you will need the following:</span></span>

* <span data-ttu-id="1e18a-259">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="1e18a-259">An active Azure account.</span></span> <span data-ttu-id="1e18a-260">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1e18a-260">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="1e18a-261">Egy [Azure Cosmos DB fiók][create-documentdb-dotnet.md#create-account].</span><span class="sxs-lookup"><span data-stu-id="1e18a-261">An [Azure Cosmos DB account][create-documentdb-dotnet.md#create-account].</span></span>
* <span data-ttu-id="1e18a-262">A GitHubon elérhető [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) megoldás.</span><span class="sxs-lookup"><span data-stu-id="1e18a-262">The [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="1e18a-263">A DocumentDB API hivatkozásainak Azure Cosmos DB .NET Core SDK-t a Visual Studio visszaállításához kattintson a jobb gombbal a **GetStarted** Megoldáskezelőben, majd megoldás **engedélyezése NuGet-csomagok visszaállításának**.</span><span class="sxs-lookup"><span data-stu-id="1e18a-263">To restore the references to the DocumentDB API for Azure Cosmos DB .NET Core SDK in Visual Studio, right-click the **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="1e18a-264">Következő lépésként a Program.cs fájlban frissítse az EndpointUrl és az AuthorizationKey értékeket leírtak szerint [csatlakozás Azure Cosmos DB fiók](#Connect).</span><span class="sxs-lookup"><span data-stu-id="1e18a-264">Next, in the Program.cs file, update the EndpointUrl and AuthorizationKey values as described in [Connect to an Azure Cosmos DB account](#Connect).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e18a-265">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e18a-265">Next steps</span></span>
* <span data-ttu-id="1e18a-266">Összetettebb ASP.NET MVC-oktatóanyagot szeretne?</span><span class="sxs-lookup"><span data-stu-id="1e18a-266">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="1e18a-267">Lásd: [ASP.NET MVC oktatóprogram: webalkalmazás fejlesztése a Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="1e18a-267">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="1e18a-268">A Xamarin iOS, Android vagy űrlapok fejlesztése a DocumentDB API-t használó Azure Cosmos DB .NET Core SDK alkalmazások?</span><span class="sxs-lookup"><span data-stu-id="1e18a-268">Want to develop a Xamarin iOS, Android, or Forms application using the DocumentDB API for Azure Cosmos DB .NET Core SDK?</span></span> <span data-ttu-id="1e18a-269">Lásd: [használó alkalmazások mobil a Xamarinnal és Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="1e18a-269">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>
* <span data-ttu-id="1e18a-270">Méret- és teljesítménytesztelést szeretne végezni az Azure Cosmos DB használatával?</span><span class="sxs-lookup"><span data-stu-id="1e18a-270">Want to perform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="1e18a-271">Lásd: [teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="1e18a-271">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="1e18a-272">Megtudhatja, hogyan [figyelő Azure Cosmos DB kéréseket, a használati és a tárolási](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="1e18a-272">Learn how to [Monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="1e18a-273">Futtasson lekérdezéseket a minta-adatkészleteken a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) használatával.</span><span class="sxs-lookup"><span data-stu-id="1e18a-273">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="1e18a-274">A programozási modellel kapcsolatos további információkért lásd: [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="1e18a-274">To learn more about the programming model, see [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

[create-documentdb-dotnet.md#create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-dotnetcore-get-started/nosql-tutorial-keys.png
