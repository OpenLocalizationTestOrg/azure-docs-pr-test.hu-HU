---
title: "Azure Data Lake Analytics-feladatok aaaDevelop U-SQL-szerelvények |} Microsoft Docs"
description: "Ismerje meg, és hogyan lehet toodevelop szerelvények toobe használja fel újra a Data Lake Analytics feladatok. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: 86dd17b25e0967306ed36bb5b7f3178d9409d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a><span data-ttu-id="f35d4-103">Azure Data Lake Analytics-feladatok U-SQL-szerelvények fejlesztése</span><span class="sxs-lookup"><span data-stu-id="f35d4-103">Develop U-SQL assemblies for Azure Data Lake Analytics jobs</span></span>
<span data-ttu-id="f35d4-104">Ismerje meg, hogyan tooturn háttérkód szerelvények toobe használja, és fel újra a Data Lake Analytics-feladatok be.</span><span class="sxs-lookup"><span data-stu-id="f35d4-104">Learn how tooturn code-behind into assemblies toobe used and reused in Data Lake Analytics jobs.</span></span> 

<span data-ttu-id="f35d4-105">U-SQL lehetővé teszi a saját egyéni kód könnyen tooadd .net nyelven, például a C#, VB.Net vagy F #.</span><span class="sxs-lookup"><span data-stu-id="f35d4-105">U-SQL makes it easy tooadd your own custom code in .Net languages, such as C#, VB.Net or F#.</span></span> <span data-ttu-id="f35d4-106">Saját futásidejű toosupport más nyelveken is telepíteni.</span><span class="sxs-lookup"><span data-stu-id="f35d4-106">You can even deploy your own runtime toosupport other languages.</span></span>

<span data-ttu-id="f35d4-107">hello legegyszerűbb módja toouse egyéni kód toouse hello Data Lake Tools Visual Studio háttérkód képességeinek.</span><span class="sxs-lookup"><span data-stu-id="f35d4-107">hello easiest way toouse custom code is toouse hello Data Lake Tools for Visual Studio’s code-behind capabilities.</span></span> <span data-ttu-id="f35d4-108">További információkért lásd: [oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f35d4-108">For more information, see [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="f35d4-109">Van néhány hátrányai háttérkód használatával:</span><span class="sxs-lookup"><span data-stu-id="f35d4-109">There are a few drawbacks of using code-behind:</span></span>

- <span data-ttu-id="f35d4-110">hello forráskód minden parancsfájl benyújtására lekérdezi feltöltve.</span><span class="sxs-lookup"><span data-stu-id="f35d4-110">hello source code gets uploaded for every script submission.</span></span>
- <span data-ttu-id="f35d4-111">háttérkód nem osztható meg más feladatok.</span><span class="sxs-lookup"><span data-stu-id="f35d4-111">code-behind cannot be shared with other jobs.</span></span>

<span data-ttu-id="f35d4-112">tooaddress ezek hátrányai háttérkód ikonná szerelvényeket, és regisztrálja a hello szerelvények toohello Data Lake Analytics-katalógus.</span><span class="sxs-lookup"><span data-stu-id="f35d4-112">tooaddress these drawbacks, you can turn code-behind into assemblies, and register hello assemblies toohello Data Lake Analytics catalog.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f35d4-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f35d4-113">Prerequisites</span></span>
* <span data-ttu-id="f35d4-114">A Visual Studio 2017, a Visual Studio 2015, Visual Studio 2013 4. frissítéssel vagy Visual Studio 2012 és telepített Visual C++</span><span class="sxs-lookup"><span data-stu-id="f35d4-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed</span></span>
* <span data-ttu-id="f35d4-115">A Microsoft Azure SDK for .NET 2.5-ös verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f35d4-115">Microsoft Azure SDK for .NET version 2.5 or above.</span></span>  <span data-ttu-id="f35d4-116">Telepítse a webplatform-telepítő hello vagy Visual Studio-telepítő</span><span class="sxs-lookup"><span data-stu-id="f35d4-116">Install it using hello Web platform installer or Visual Studio Installer</span></span>
* <span data-ttu-id="f35d4-117">Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="f35d4-117">A Data Lake Analytics account.</span></span>  <span data-ttu-id="f35d4-118">Lásd: [Az Azure Data Lake Analytics használatának első lépései az Azure Portallal](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f35d4-118">See [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="f35d4-119">Lépkedjen végig hello [Ismerkedés az Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f35d4-119">Go through hello [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) tutorial.</span></span>
* <span data-ttu-id="f35d4-120">Csatlakozás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f35d4-120">Connect tooAzure.</span></span>
* <span data-ttu-id="f35d4-121">Hello forrásadatok feltöltése című [Ismerkedés az Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f35d4-121">Upload hello source data, see [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span></span> 

## <a name="develop-assemblies-for-u-sql"></a><span data-ttu-id="f35d4-122">A U-SQL szerelvények fejlesztése</span><span class="sxs-lookup"><span data-stu-id="f35d4-122">Develop assemblies for U-SQL</span></span>

<span data-ttu-id="f35d4-123">**toocreate, és küldje el a U-SQL-feladatot:**</span><span class="sxs-lookup"><span data-stu-id="f35d4-123">**toocreate and submit a U-SQL job**</span></span>

1. <span data-ttu-id="f35d4-124">A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="f35d4-124">From hello **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="f35d4-125">Bontsa ki a **telepített**, **sablonok**, **Azure Data Lake**, **U-SQL(ADLA)**, jelölje be hello **Class Library (az U-SQL Alkalmazás)** sablont, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="f35d4-125">Expand **Installed**, **Templates**, **Azure Data Lake**, **U-SQL(ADLA)**, select hello **Class Library (For U-SQL Application)** template, and then click **OK**.</span></span>
3. <span data-ttu-id="f35d4-126">Írja be a kódot Class1.cs.</span><span class="sxs-lookup"><span data-stu-id="f35d4-126">Write your code in Class1.cs.</span></span>  <span data-ttu-id="f35d4-127">hello az alábbiakban egy példát az.</span><span class="sxs-lookup"><span data-stu-id="f35d4-127">hello following is a code sample.</span></span>

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. <span data-ttu-id="f35d4-128">Kattintson a hello **Build** menüben, majd kattintson **megoldás fordítása** toocreate hello DLL-fájljában.</span><span class="sxs-lookup"><span data-stu-id="f35d4-128">Click hello **Build** menu, and then click **Build Solution** toocreate hello dll.</span></span>

## <a name="register-assemblies"></a><span data-ttu-id="f35d4-129">Szerelvények regisztrálása</span><span class="sxs-lookup"><span data-stu-id="f35d4-129">Register assemblies</span></span>

<span data-ttu-id="f35d4-130">Lásd: [használata Data Lake Analytics(U-SQL) katalógus](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="f35d4-130">See [Use Data Lake Analytics(U-SQL) catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>


## <a name="use-hello-assemblies"></a><span data-ttu-id="f35d4-131">Hello szerelvények használata</span><span class="sxs-lookup"><span data-stu-id="f35d4-131">Use hello assemblies</span></span>

<span data-ttu-id="f35d4-132">Lásd: [hello Azure Data Lake Tools használja a Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="f35d4-132">See [Use hello Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="f35d4-133">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f35d4-133">See also</span></span>
* [<span data-ttu-id="f35d4-134">A Data Lake Analytics PowerShell használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="f35d4-134">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="f35d4-135">Ismerkedés a Data Lake Analytics használatának hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f35d4-135">Get started with Data Lake Analytics using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="f35d4-136">Használja a Data Lake Tools for Visual Studio a U-SQL-alkalmazások fejlesztésével</span><span class="sxs-lookup"><span data-stu-id="f35d4-136">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="f35d4-137">Használja a Data Lake Analytics(U-SQL) katalógus</span><span class="sxs-lookup"><span data-stu-id="f35d4-137">Use Data Lake Analytics(U-SQL) catalog</span></span>](data-lake-analytics-use-u-sql-catalog.md)
* [<span data-ttu-id="f35d4-138">A Visual Studio Code hello Azure Data Lake Tools használata</span><span class="sxs-lookup"><span data-stu-id="f35d4-138">Use hello Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)
