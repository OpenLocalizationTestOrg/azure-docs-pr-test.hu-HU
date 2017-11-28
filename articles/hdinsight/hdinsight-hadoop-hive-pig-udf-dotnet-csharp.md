---
title: "aaaUse C# a Hive és a Hadoop a Hdinsightban - Azure Pig |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse C# felhasználói függvény (UDF) a Hive és az Azure HDInsight streaming Pig."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: dd35409766f2dafe4d8050c3f9bc351949473ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="3eb81-103">C# felhasználó által definiált függvények használata a Hive és a HDInsight hadoop streamelési Pig használatával</span><span class="sxs-lookup"><span data-stu-id="3eb81-103">Use C# user-defined functions with Hive and Pig streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="3eb81-104">Ismerje meg, hogyan toouse C# felhasználói függvény (UDF) Apache Hive és a Pig a hdinsight platformon.</span><span class="sxs-lookup"><span data-stu-id="3eb81-104">Learn how toouse C# user defined functions (UDF) with Apache Hive and Pig on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3eb81-105">hello ebben a dokumentumban a lépések a Linux-alapú, és a Windows-alapú HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="3eb81-105">hello steps in this document work with both Linux-based and Windows-based HDInsight clusters.</span></span> <span data-ttu-id="3eb81-106">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="3eb81-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3eb81-107">További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3eb81-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="3eb81-108">Mind a Hive és a Pig teljen tooexternal alkalmazások feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="3eb81-108">Both Hive and Pig can pass data tooexternal applications for processing.</span></span> <span data-ttu-id="3eb81-109">Ezt a folyamatot nevezik _streaming_.</span><span class="sxs-lookup"><span data-stu-id="3eb81-109">This process is known as _streaming_.</span></span> <span data-ttu-id="3eb81-110">A .NET hatóságuknál használatakor hello adatok toohello alkalmazás átadódik STDIN, és hello alkalmazás STDOUT-on hello eredményeket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="3eb81-110">When using a .NET applciation, hello data is passed toohello application on STDIN, and hello application returns hello results on STDOUT.</span></span> <span data-ttu-id="3eb81-111">tooread és STDIN és a STDOUT írási, használhat `Console.ReadLine()` és `Console.WriteLine()` konzol alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3eb81-111">tooread and write from STDIN and STDOUT, you can use `Console.ReadLine()` and `Console.WriteLine()` from a console application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3eb81-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3eb81-112">Prerequisites</span></span>

* <span data-ttu-id="3eb81-113">Egy írást, és C#-kódban, amelynek célpontja a .NET-keretrendszer 4.5 felépítése ismeretét.</span><span class="sxs-lookup"><span data-stu-id="3eb81-113">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span>

    * <span data-ttu-id="3eb81-114">Bármilyen IDE szeretne használni.</span><span class="sxs-lookup"><span data-stu-id="3eb81-114">Use whatever IDE you want.</span></span> <span data-ttu-id="3eb81-115">Ajánlott [Visual Studio](https://www.visualstudio.com/vs) 2015-ös, 2017, vagy [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="3eb81-115">We recommend [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, or [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="3eb81-116">Ez a dokumentum használatát a Visual Studio 2017 hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="3eb81-116">hello steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="3eb81-117">Egy módon tooupload .exe fájlok toohello fürt, és futtassa a Pig és Hive feladat.</span><span class="sxs-lookup"><span data-stu-id="3eb81-117">A way tooupload .exe files toohello cluster and run Pig and Hive jobs.</span></span> <span data-ttu-id="3eb81-118">Hello Data Lake Tools for Visual Studio, Azure PowerShell és az Azure parancssori felület javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="3eb81-118">We recommend hello Data Lake Tools for Visual Studio, Azure PowerShell and Azure CLI.</span></span> <span data-ttu-id="3eb81-119">hello jelen dokumentumban leírt lépések hello Data Lake Tools használni a Visual Studio tooupload hello fájlokhoz és hello példa Hive-lekérdezések futtatása.</span><span class="sxs-lookup"><span data-stu-id="3eb81-119">hello steps in this document use hello Data Lake Tools for Visual Studio tooupload hello files and run hello example Hive query.</span></span>

    <span data-ttu-id="3eb81-120">Más módokon toorun Hive-lekérdezések és a Pig-feladatokhoz kapcsolatos tudnivalókat lásd: a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="3eb81-120">For information on other ways toorun Hive queries and Pig jobs, see hello following documents:</span></span>

    * [<span data-ttu-id="3eb81-121">Apache Hive használata a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="3eb81-121">Use Apache Hive with HDInsight</span></span>](hdinsight-use-hive.md)

    * [<span data-ttu-id="3eb81-122">Apache Pig használata a HDInsight</span><span class="sxs-lookup"><span data-stu-id="3eb81-122">Use Apache Pig with HDInsight</span></span>](hdinsight-use-pig.md)

* <span data-ttu-id="3eb81-123">A Hadoop on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="3eb81-123">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="3eb81-124">A fürtök létrehozásáról további információk: [HDInsight-fürtök létrehozása](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3eb81-124">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="3eb81-125">A HDInsight .NET</span><span class="sxs-lookup"><span data-stu-id="3eb81-125">.NET on HDInsight</span></span>

* <span data-ttu-id="3eb81-126">__Linux-alapú HDInsight__ használó fürtök [monó (https://mono-project.com)](https://mono-project.com) toorun .NET-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3eb81-126">__Linux-based HDInsight__ clusters using [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="3eb81-127">Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója.</span><span class="sxs-lookup"><span data-stu-id="3eb81-127">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span>

    <span data-ttu-id="3eb81-128">A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="3eb81-128">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

    <span data-ttu-id="3eb81-129">Monó, egy adott verziójához toouse lásd: hello [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="3eb81-129">toouse a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

* <span data-ttu-id="3eb81-130">__Windows-alapú HDInsight__ fürtök hello Microsoft .NET CLR toorun .NET alkalmazásokat használnak.</span><span class="sxs-lookup"><span data-stu-id="3eb81-130">__Windows-based HDInsight__ clusters use hello Microsoft .NET CLR toorun .NET applications.</span></span>

<span data-ttu-id="3eb81-131">Hello verzióján hello .NET-keretrendszer és monó részét képező HDInsight-verziókról további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3eb81-131">For more information on hello version of hello .NET framework and Mono included with HDInsight versions, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span>

## <a name="create-hello-c-projects"></a><span data-ttu-id="3eb81-132">Hozzon létre hello C\# projektek</span><span class="sxs-lookup"><span data-stu-id="3eb81-132">Create hello C\# projects</span></span>

### <a name="hive-udf"></a><span data-ttu-id="3eb81-133">Hive UDF-ben</span><span class="sxs-lookup"><span data-stu-id="3eb81-133">Hive UDF</span></span>

1. <span data-ttu-id="3eb81-134">Nyissa meg a Visual Studio és a megoldás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3eb81-134">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="3eb81-135">Hello projekttípus, válassza a **Konzolalkalmazás (.NET-keretrendszer)**, és a név hello új projekt **HiveCSharp**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-135">For hello project type, select **Console App (.NET Framework)**, and name hello new project **HiveCSharp**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3eb81-136">Válassza ki __.NET-keretrendszer 4.5__ egy Linux-alapú HDInsight-fürt használata.</span><span class="sxs-lookup"><span data-stu-id="3eb81-136">Select __.NET Framework 4.5__ if you are using a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="3eb81-137">A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="3eb81-137">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

2. <span data-ttu-id="3eb81-138">Cserélje le a hello tartalmát **Program.cs** hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="3eb81-138">Replace hello contents of **Program.cs** with hello following:</span></span>

    ```csharp
    using System;
    using System.Security.Cryptography;
    using System.Text;
    using System.Threading.Tasks;

    namespace HiveCSharp
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Parse hello string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data toostdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for hello given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array toohex string
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < hash.Length; i++)
                {
                    sb.Append(hash[i].ToString("x2"));
                }
                return sb.ToString();
            }
        }
    }
    ```

3. <span data-ttu-id="3eb81-139">Hello projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3eb81-139">Build hello project.</span></span>

### <a name="pig-udf"></a><span data-ttu-id="3eb81-140">Pig UDF-ben</span><span class="sxs-lookup"><span data-stu-id="3eb81-140">Pig UDF</span></span>

1. <span data-ttu-id="3eb81-141">Nyissa meg a Visual Studio és a megoldás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3eb81-141">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="3eb81-142">Hello projekttípus, válassza a **Konzolalkalmazás**, és a név hello új projekt **PigUDF**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-142">For hello project type, select **Console Application**, and name hello new project **PigUDF**.</span></span>

2. <span data-ttu-id="3eb81-143">Cserélje le a hello hello tartalmát **Program.cs** hello kód a következő fájlt:</span><span class="sxs-lookup"><span data-stu-id="3eb81-143">Replace hello contents of hello **Program.cs** file with hello following code:</span></span>

    ```csharp
    using System;

    namespace PigUDF
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Fix formatting on lines that begin with an exception
                    if(line.StartsWith("java.lang.Exception"))
                    {
                        // Trim hello error info off hello beginning and add a note toohello end of hello line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split hello fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    <span data-ttu-id="3eb81-144">Ez az alkalmazás elemez hello Pig által küldött és újraformázása sorokat kezdődő `java.lang.Exception`.</span><span class="sxs-lookup"><span data-stu-id="3eb81-144">This application parses hello lines sent from Pig, and reformat lines that begin with `java.lang.Exception`.</span></span>

3. <span data-ttu-id="3eb81-145">Mentés **Program.cs**, majd a hello projekt felépítéséhez és.</span><span class="sxs-lookup"><span data-stu-id="3eb81-145">Save **Program.cs**, and then build hello project.</span></span>

## <a name="upload-toostorage"></a><span data-ttu-id="3eb81-146">Toostorage feltöltése</span><span class="sxs-lookup"><span data-stu-id="3eb81-146">Upload toostorage</span></span>

1. <span data-ttu-id="3eb81-147">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-147">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="3eb81-148">Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.</span><span class="sxs-lookup"><span data-stu-id="3eb81-148">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="3eb81-149">Ha a rendszer kéri, adja meg Azure-előfizetés hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-149">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="3eb81-150">Bontsa ki, hogy az alkalmazás kívánja toodeploy hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="3eb81-150">Expand hello HDInsight cluster that you wish toodeploy this application to.</span></span> <span data-ttu-id="3eb81-151">Hello szöveg bejegyzés __(alapértelmezett Tárfiók)__ szerepel.</span><span class="sxs-lookup"><span data-stu-id="3eb81-151">An entry with hello text __(Default Storage Account)__ is listed.</span></span>

    ![Server Explorer hello tárfiók hello fürt megjelenítése](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="3eb81-153">Ez a bejegyzés bővíthetők, ha az egy __Azure Storage-fiók__ hello fürt alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="3eb81-153">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for hello cluster.</span></span> <span data-ttu-id="3eb81-154">tooview hello fájlok hello alapértelmezett tároló hello fürt, bontsa ki a hello bejegyzést, és kattintson duplán a hello __(alapértelmezett tároló)__.</span><span class="sxs-lookup"><span data-stu-id="3eb81-154">tooview hello files on hello default storage for hello cluster, expand hello entry and then double-click hello __(Default Container)__.</span></span>

    * <span data-ttu-id="3eb81-155">Ez a bejegyzés nem bonthatók ki, ha az __Azure Data Lake Store__ hello fürt hello alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="3eb81-155">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as hello default storage for hello cluster.</span></span> <span data-ttu-id="3eb81-156">tooview hello fájlok hello alapértelmezett tároló hello fürt, kattintson duplán a hello __(alapértelmezett Tárfiók)__ bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="3eb81-156">tooview hello files on hello default storage for hello cluster, double-click hello __(Default Storage Account)__ entry.</span></span>

6. <span data-ttu-id="3eb81-157">tooupload hello .exe fájlok, a hello a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="3eb81-157">tooupload hello .exe files, use one of hello following methods:</span></span>

    * <span data-ttu-id="3eb81-158">Ha használ egy __Azure Storage-fiók__, hello Feltöltés ikonra, és tallózással keressen toohello **bin\debug** hello mappa **HiveCSharp** projekt.</span><span class="sxs-lookup"><span data-stu-id="3eb81-158">If using an __Azure Storage Account__, click hello upload icon, and then browse toohello **bin\debug** folder for hello **HiveCSharp** project.</span></span> <span data-ttu-id="3eb81-159">Végül válassza ki a hello **HiveCSharp.exe** fájlt, és kattintson a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-159">Finally, select hello **HiveCSharp.exe** file and click **Ok**.</span></span>

        ![Töltse fel az ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="3eb81-161">Ha használ __Azure Data Lake Store__, kattintson a jobb gombbal egy üres területre a hello felsoroló, és válassza __feltöltése__.</span><span class="sxs-lookup"><span data-stu-id="3eb81-161">If using __Azure Data Lake Store__, right-click an empty area in hello file listing, and then select __Upload__.</span></span> <span data-ttu-id="3eb81-162">Végül válassza ki a hello **HiveCSharp.exe** fájlt, és kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-162">Finally, select hello **HiveCSharp.exe** file and click **Open**.</span></span>

    <span data-ttu-id="3eb81-163">Egyszer hello __HiveCSharp.exe__ feltöltése befejeződött, ismétlődő hello feltöltési folyamat a hello __PigUDF.exe__ fájlt.</span><span class="sxs-lookup"><span data-stu-id="3eb81-163">Once hello __HiveCSharp.exe__ upload has finished, repeat hello upload process for hello __PigUDF.exe__ file.</span></span>

## <a name="run-a-hive-query"></a><span data-ttu-id="3eb81-164">Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="3eb81-164">Run a Hive query</span></span>

1. <span data-ttu-id="3eb81-165">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-165">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="3eb81-166">Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.</span><span class="sxs-lookup"><span data-stu-id="3eb81-166">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="3eb81-167">Kattintson a jobb gombbal hello fürt hello telepített **HiveCSharp** alkalmazás számára, és adja **Hive lekérdezés írása**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-167">Right-click hello cluster that you deployed hello **HiveCSharp** application to, and then select **Write a Hive Query**.</span></span>

4. <span data-ttu-id="3eb81-168">A következő szöveg hello Hive lekérdezés hello használata:</span><span class="sxs-lookup"><span data-stu-id="3eb81-168">Use hello following text for hello Hive query:</span></span>

    ```hiveql
    -- Uncomment hello following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment hello following if you are using Azure Data Lake Store
    -- add file adl:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="3eb81-169">Állítsa vissza a hello `add file` utasítást, amely megfelel a fürthöz használt alapértelmezett tárolási hello típusának.</span><span class="sxs-lookup"><span data-stu-id="3eb81-169">Uncomment hello `add file` statement that matches hello type of default storage used for your cluster.</span></span>

    <span data-ttu-id="3eb81-170">Ez a lekérdezés kiválasztja hello `clientid`, `devicemake`, és `devicemodel` mezőit a `hivesampletable`, és továbbadja hello mezők toohello HiveCSharp.exe alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3eb81-170">This query selects hello `clientid`, `devicemake`, and `devicemodel` fields from `hivesampletable`, and passes hello fields toohello HiveCSharp.exe application.</span></span> <span data-ttu-id="3eb81-171">hello lekérdezést vár hello alkalmazás tooreturn három mezőt, mint a tárolt `clientid`, `phoneLabel`, és `phoneHash`.</span><span class="sxs-lookup"><span data-stu-id="3eb81-171">hello query expects hello application tooreturn three fields, which are stored as `clientid`, `phoneLabel`, and `phoneHash`.</span></span> <span data-ttu-id="3eb81-172">hello lekérdezés is vár toofind HiveCSharp.exe hello alapértelmezett tároló hello gyökerében található.</span><span class="sxs-lookup"><span data-stu-id="3eb81-172">hello query also expects toofind HiveCSharp.exe in hello root of hello default storage container.</span></span>

5. <span data-ttu-id="3eb81-173">Kattintson a **Submit** toosubmit hello feladat toohello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="3eb81-173">Click **Submit** toosubmit hello job toohello HDInsight cluster.</span></span> <span data-ttu-id="3eb81-174">Hello **Hive feladat összegzése** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="3eb81-174">hello **Hive Job Summary** window opens.</span></span>

6. <span data-ttu-id="3eb81-175">Kattintson a **frissítése** toorefresh hello összefoglaló, amíg **feladatállapot** túl változik**befejezve**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-175">Click **Refresh** toorefresh hello summary until **Job Status** changes too**Completed**.</span></span> <span data-ttu-id="3eb81-176">tooview hello feladat kimeneti, kattintson a **Job Output**.</span><span class="sxs-lookup"><span data-stu-id="3eb81-176">tooview hello job output, click **Job Output**.</span></span>

## <a name="run-a-pig-job"></a><span data-ttu-id="3eb81-177">A Pig-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="3eb81-177">Run a Pig job</span></span>

1. <span data-ttu-id="3eb81-178">A következő módszerek tooconnect tooyour HDInsight-fürt hello egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="3eb81-178">Use one of hello following methods tooconnect tooyour HDInsight cluster:</span></span>

    * <span data-ttu-id="3eb81-179">Ha használ egy __Linux-alapú__ HDInsight fürt esetén az SSH használata.</span><span class="sxs-lookup"><span data-stu-id="3eb81-179">If you are using a __Linux-based__ HDInsight cluster, use SSH.</span></span> <span data-ttu-id="3eb81-180">Például: `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="3eb81-180">For example, `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="3eb81-181">További információkért lásd: [az SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="3eb81-181">For more information, see [Use SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
    
    * <span data-ttu-id="3eb81-182">Ha használ egy __Windows-alapú__ HDInsight-fürtjéhez, [Connect toohello fürt távoli asztali kapcsolattal](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="3eb81-182">If you are using a __Windows-based__ HDInsight cluster, [Connect toohello cluster using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>

2. <span data-ttu-id="3eb81-183">A következő parancs toostart hello Pig parancssori egy hello használata:</span><span class="sxs-lookup"><span data-stu-id="3eb81-183">Use one hello following command toostart hello Pig command line:</span></span>

        pig

    > [!IMPORTANT]
    > <span data-ttu-id="3eb81-184">Ha egy Windows-alapú fürtöt használ, használja inkább a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="3eb81-184">If you are using a Windows-based cluster, use hello following commands instead:</span></span>
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    <span data-ttu-id="3eb81-185">A `grunt>` kérdés megjelenik.</span><span class="sxs-lookup"><span data-stu-id="3eb81-185">A `grunt>` prompt is displayed.</span></span>

3. <span data-ttu-id="3eb81-186">Adja meg a következő toorun hello .NET-keretrendszer alkalmazás által a Pig feladatot hello:</span><span class="sxs-lookup"><span data-stu-id="3eb81-186">Enter hello following toorun a Pig job that uses hello .NET Framework application:</span></span>

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    <span data-ttu-id="3eb81-187">Hello `DEFINE` utasítás hoz létre az alias `streamer` hello pigudf.exe alkalmazásokhoz, és `CACHE` betölti azt az alapértelmezett hello fürt tárolóhelyét.</span><span class="sxs-lookup"><span data-stu-id="3eb81-187">hello `DEFINE` statement creates an alias of `streamer` for hello pigudf.exe applications, and `CACHE` loads it from default storage for hello cluster.</span></span> <span data-ttu-id="3eb81-188">Később `streamer` hello használt `STREAM` operátor tooprocess hello szereplő oszlopok sorozataként NAPLÓHOZ és visszatérési hello az egyetlen sort.</span><span class="sxs-lookup"><span data-stu-id="3eb81-188">Later, `streamer` is used with hello `STREAM` operator tooprocess hello single lines contained in LOG and return hello data as a series of columns.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3eb81-189">az adatfolyamként történő használt alkalmazásnév hello kell lennie.%n hello \` (backtick) karakter mikor aliasnevet, és a "(szimpla idézőjel) használata esetén `SHIP\`.</span><span class="sxs-lookup"><span data-stu-id="3eb81-189">hello application name that is used for streaming must be surrounded by hello \` (backtick) character when aliased, and ' (single quote) when used with `SHIP\`.</span></span>

4. <span data-ttu-id="3eb81-190">Után utolsó sora hello bevitelét, hello feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="3eb81-190">After entering hello last line, hello job should start.</span></span> <span data-ttu-id="3eb81-191">Azt jelzi, hogy a kimeneti hasonló toohello szöveg a következő:</span><span class="sxs-lookup"><span data-stu-id="3eb81-191">It returns output similar toohello following text:</span></span>

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a><span data-ttu-id="3eb81-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3eb81-192">Next steps</span></span>

<span data-ttu-id="3eb81-193">Ebből a dokumentumból megtanulta, hogyan toouse a Hive és a Pig a HDInsight .NET-keretrendszer alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3eb81-193">In this document, you have learned how toouse a .NET Framework application from Hive and Pig on HDInsight.</span></span> <span data-ttu-id="3eb81-194">Ha azt szeretné, hogy hogyan toouse a Hive és a Pig, Python: toolearn [a Hive és a hdinsight a Pig használata Python](hdinsight-python.md).</span><span class="sxs-lookup"><span data-stu-id="3eb81-194">If you would like toolearn how toouse Python with Hive and Pig, see [Use Python with Hive and Pig in HDInsight](hdinsight-python.md).</span></span>

<span data-ttu-id="3eb81-195">Más módokon toouse Pig és a Hive és a toolearn MapReduce használatáról tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="3eb81-195">For other ways toouse Pig and Hive, and toolearn about using MapReduce, see hello following documents:</span></span>

* [<span data-ttu-id="3eb81-196">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="3eb81-196">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="3eb81-197">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="3eb81-197">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="3eb81-198">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="3eb81-198">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
