---
title: "U-SQL felhasználói operátorok (udo-k) kidolgozása |} Microsoft Docs"
description: "Megtudhatja, hogyan kell használni, és fel újra a Data Lake Analytics-feladatok felhasználói operátorok fejlesztése. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: fdee02fb60b633c26704fc1774dfc3a7825b5e0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="develop-u-sql-user-defined-operators-udos"></a><span data-ttu-id="ed5da-103">U-SQL felhasználói operátorok (udo-k) fejlesztése</span><span class="sxs-lookup"><span data-stu-id="ed5da-103">Develop U-SQL user-defined operators (UDOs)</span></span>
<span data-ttu-id="ed5da-104">Ismerje meg, hogyan fejleszthet operátorok felhasználói fel adatokat a U-SQL-feladatot.</span><span class="sxs-lookup"><span data-stu-id="ed5da-104">Learn how to develop user-defined operators to process data in a U-SQL job.</span></span>

<span data-ttu-id="ed5da-105">Általános célú szerelvényeket a U-SQL fejlesztésével, további információkért lásd: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md)</span><span class="sxs-lookup"><span data-stu-id="ed5da-105">For instructions on developing general-purpose assemblies for U-SQL, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md)</span></span>

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a><span data-ttu-id="ed5da-106">A felhasználó által definiált operátor használata U-SQL és megadása</span><span class="sxs-lookup"><span data-stu-id="ed5da-106">Define and use a user-defined operator in U-SQL</span></span>
<span data-ttu-id="ed5da-107">**Létrehozásához és elküldéséhez a U-SQL-feladatot:**</span><span class="sxs-lookup"><span data-stu-id="ed5da-107">**To create and submit a U-SQL job**</span></span>

1. <span data-ttu-id="ed5da-108">A Visual Studio válassza **fájl > Új > Projekt > U-SQL projekt**.</span><span class="sxs-lookup"><span data-stu-id="ed5da-108">From the Visual Studio select **File > New > Project > U-SQL Project**.</span></span>
2. <span data-ttu-id="ed5da-109">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ed5da-109">Click **OK**.</span></span> <span data-ttu-id="ed5da-110">A Visual Studio létrehoz egy megoldást Script.usql fájllal.</span><span class="sxs-lookup"><span data-stu-id="ed5da-110">Visual Studio creates a solution with a Script.usql file.</span></span>
3. <span data-ttu-id="ed5da-111">A **Megoldáskezelőben**, bontsa ki a Script.usql, és kattintson duplán **Script.usql.cs**.</span><span class="sxs-lookup"><span data-stu-id="ed5da-111">From **Solution Explorer**, expand Script.usql, and then double-click **Script.usql.cs**.</span></span>
4. <span data-ttu-id="ed5da-112">A fájlba illessze be a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="ed5da-112">Paste the following code into the file:</span></span>

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. <span data-ttu-id="ed5da-113">Nyissa meg **Script.usql**, és illessze be az alábbi U-SQL parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="ed5da-113">Open **Script.usql**, and paste the following U-SQL script:</span></span>

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. <span data-ttu-id="ed5da-114">Adja meg a Data Lake Analytics-fiókot, -adatbázist és -sémát.</span><span class="sxs-lookup"><span data-stu-id="ed5da-114">Specify the Data Lake Analytics account, Database, and Schema.</span></span>
8. <span data-ttu-id="ed5da-115">A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Build Script** (Parancsfájl létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="ed5da-115">From **Solution Explorer**, right-click **Script.usql**, and then click **Build Script**.</span></span>
9. <span data-ttu-id="ed5da-116">A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Submit Script** (Parancsfájl elküldése) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ed5da-116">From **Solution Explorer**, right-click **Script.usql**, and then click **Submit Script**.</span></span>
10. <span data-ttu-id="ed5da-117">Ha még nem kapcsolódik az Azure-előfizetéssel, kérni fogja az Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="ed5da-117">If you haven't connected to your Azure subscription, you will be prompted to enter your Azure account credentials.</span></span>
11. <span data-ttu-id="ed5da-118">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="ed5da-118">Click **Submit**.</span></span> <span data-ttu-id="ed5da-119">Után az eredmények és a feladatra mutató hivatkozás esetén érhetők el az eredmények ablakában az elküldés.</span><span class="sxs-lookup"><span data-stu-id="ed5da-119">Submission results and job link are available in the Results window when the submission is completed.</span></span>
12. <span data-ttu-id="ed5da-120">Kattintson a **frissítése** gombra kattintva megtekintheti a legutóbbi feladat állapota, és frissítse a képernyőt.</span><span class="sxs-lookup"><span data-stu-id="ed5da-120">Click the **Refresh** button to see the latest job status and refresh the screen.</span></span>

<span data-ttu-id="ed5da-121">**A kimenet megtekintéséhez**</span><span class="sxs-lookup"><span data-stu-id="ed5da-121">**To see the output**</span></span>

1. <span data-ttu-id="ed5da-122">A **Server Explorer**, bontsa ki a **Azure**, bontsa ki a **Data Lake Analytics**, bontsa ki a Data Lake Analytics-fiókjait, bontsa ki a **Tárfiókok**, kattintson a jobb gombbal az alapértelmezett tároló, és kattintson a **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ed5da-122">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click the Default Storage, and then click **Explorer**.</span></span>
2. <span data-ttu-id="ed5da-123">Bontsa ki a mintákat, bontsa ki a kimenetek, és kattintson duplán **Drivers.csv**.</span><span class="sxs-lookup"><span data-stu-id="ed5da-123">Expand Samples, expand Outputs, and then double-click **Drivers.csv**.</span></span>

## <a name="see-also"></a><span data-ttu-id="ed5da-124">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ed5da-124">See also</span></span>
* [<span data-ttu-id="ed5da-125">A Data Lake Analytics PowerShell használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="ed5da-125">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="ed5da-126">Ismerkedés a Data Lake Analytics az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="ed5da-126">Get started with Data Lake Analytics using the Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="ed5da-127">Használja a Data Lake Tools for Visual Studio a U-SQL-alkalmazások fejlesztésével</span><span class="sxs-lookup"><span data-stu-id="ed5da-127">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
