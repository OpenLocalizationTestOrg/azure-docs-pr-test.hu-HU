---
title: "Szerializálni az adatokat a Hadoop - Microsoft Avro Library - Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan lehet szerializálni, és adatokat a Hadoop on HDInsight használatával a Microsoft az Avro Library megőrizni a memória, egy adatbázis vagy a fájl."
keywords: az avro, hadoop avro
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: c78dc20d-5d8d-4366-94ac-abbe89aaac58
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jgao
ms.custom: hdiseo17may2017
ms.openlocfilehash: d06bf8ff4a21e4f4b29593bac32bfa2b32601fc4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a><span data-ttu-id="db802-104">A Microsoft az Avro Library Hadoop adatok szerializálása</span><span class="sxs-lookup"><span data-stu-id="db802-104">Serialize data in Hadoop with the Microsoft Avro Library</span></span>

>[!NOTE]
><span data-ttu-id="db802-105">Az Avro SDK a Microsoft már nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="db802-105">The Avro SDK is no longer supported by Microsoft.</span></span> <span data-ttu-id="db802-106">A könyvtárban, nyílt forráskódú közösségi támogatott.</span><span class="sxs-lookup"><span data-stu-id="db802-106">The library is open source community supported.</span></span> <span data-ttu-id="db802-107">A szalagtár források találhatók [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="db802-107">The sources for the library are located in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

<span data-ttu-id="db802-108">Ez a témakör bemutatja, hogyan használja a [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) objektumok és egyéb adatstruktúrák szerializálni az adatfolyamok megőrizni a memória, adatbázis vagy egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="db802-108">This topic shows how to use the [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) to serialize objects and other data structures into streams to persist them to memory, a database, or a file.</span></span> <span data-ttu-id="db802-109">Azt is bemutatja, hogyan őket helyreállítása az eredeti objektum deszerializálása.</span><span class="sxs-lookup"><span data-stu-id="db802-109">It also shows how to deserialize them to recover the original objects.</span></span>

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a><span data-ttu-id="db802-110">Apache Avro</span><span class="sxs-lookup"><span data-stu-id="db802-110">Apache Avro</span></span>
<span data-ttu-id="db802-111">A <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> valósítja meg az Apache Avro szerializálási rendszer Microsoft.NET környezetre.</span><span class="sxs-lookup"><span data-stu-id="db802-111">The <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implements the Apache Avro data serialization system for the Microsoft.NET environment.</span></span> <span data-ttu-id="db802-112">Apache Avro kompakt bináris adatcsere adatformátum szerializálási biztosít.</span><span class="sxs-lookup"><span data-stu-id="db802-112">Apache Avro provides a compact binary data interchange format for serialization.</span></span> <span data-ttu-id="db802-113">Használja <a href="http://www.json.org" target="_blank">JSON</a> egy nyelvtől független sémát, amely lehetővé teszi a nyelvi együttműködést meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="db802-113">It uses <a href="http://www.json.org" target="_blank">JSON</a> to define a language-agnostic schema that underwrites language interoperability.</span></span> <span data-ttu-id="db802-114">Több nyelven érhetőek szerializált adatok olvashatók egy másik.</span><span class="sxs-lookup"><span data-stu-id="db802-114">Data serialized in one language can be read in another.</span></span> <span data-ttu-id="db802-115">Jelenleg a C, C++, C#, Java, PHP, Python és Ruby támogatottak.</span><span class="sxs-lookup"><span data-stu-id="db802-115">Currently C, C++, C#, Java, PHP, Python, and Ruby are supported.</span></span> <span data-ttu-id="db802-116">A részletes információk találhatók a <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro specifikációjában</a>.</span><span class="sxs-lookup"><span data-stu-id="db802-116">Detailed information on the format can be found in the <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span></span> 

>[!NOTE]
><span data-ttu-id="db802-117">A Microsoft az Avro Library nem támogatja a távoli eljáráshívásnak (RPC) hívások részét ez az előírás.</span><span class="sxs-lookup"><span data-stu-id="db802-117">The Microsoft Avro Library does not support the remote procedure calls (RPCs) part of this specification.</span></span>
>

<span data-ttu-id="db802-118">Az Avro rendszerben objektum szerializált ábrázolását két részből áll: séma, a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="db802-118">The serialized representation of an object in the Avro system consists of two parts: schema and actual value.</span></span> <span data-ttu-id="db802-119">Az Avro-séma a nyelvfüggetlen az a szerializált adatok JSON adatmodell ismerteti.</span><span class="sxs-lookup"><span data-stu-id="db802-119">The Avro schema describes the language-independent data model of the serialized data with JSON.</span></span> <span data-ttu-id="db802-120">A bináris adatok megjelenítése és jelölőnégyzetként jelenik.</span><span class="sxs-lookup"><span data-stu-id="db802-120">It is presented side by side with a binary representation of data.</span></span> <span data-ttu-id="db802-121">Rendelkező elkülönül a bináris megjelenítése a séma lehetővé teszi az egyes objektumok nem érték terhek, így gyors szerializálási, és a ábrázolását kis írni.</span><span class="sxs-lookup"><span data-stu-id="db802-121">Having the schema separate from the binary representation permits each object to be written with no per-value overheads, making serialization fast, and the representation small.</span></span>

## <a name="the-hadoop-scenario"></a><span data-ttu-id="db802-122">A Hadoop-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="db802-122">The Hadoop scenario</span></span>
<span data-ttu-id="db802-123">Az Apache Avro szerializálási formátum széles körben használt Azure HDInsight és egyéb Apache Hadoop-környezetekben.</span><span class="sxs-lookup"><span data-stu-id="db802-123">The Apache Avro serialization format is widely used in Azure HDInsight and other Apache Hadoop environments.</span></span> <span data-ttu-id="db802-124">Az Avro Hadoop MapReduce feladatot összetett adatstruktúra képviselő kényelmes megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="db802-124">Avro provides a convenient way to represent complex data structures within a Hadoop MapReduce job.</span></span> <span data-ttu-id="db802-125">Az Avro-fájlok (Avro tároló fájlt) formátuma támogatja az elosztott MapReduce programozási modellt tervezték.</span><span class="sxs-lookup"><span data-stu-id="db802-125">The format of Avro files (Avro object container file) has been designed to support the distributed MapReduce programming model.</span></span> <span data-ttu-id="db802-126">A kulcs szolgáltatása, amely lehetővé teszi, hogy a terjesztési, az, hogy a fájlok "feloszthatók", abban az értelemben, hogy egy fájl bármely pontot kikereshet, és az egy adott blokktól kezdheti-e.</span><span class="sxs-lookup"><span data-stu-id="db802-126">The key feature that enables the distribution is that the files are “splittable” in the sense that one can seek any point in a file and start reading from a particular block.</span></span>

## <a name="serialization-in-avro-library"></a><span data-ttu-id="db802-127">Az Avro könyvtárban szerializálási</span><span class="sxs-lookup"><span data-stu-id="db802-127">Serialization in Avro Library</span></span>
<span data-ttu-id="db802-128">A .NET könyvtár az avro-hoz támogatja a szerializálási objektumok két módon:</span><span class="sxs-lookup"><span data-stu-id="db802-128">The .NET Library for Avro supports two ways of serializing objects:</span></span>

* <span data-ttu-id="db802-129">**Fejlécreflexiós** -a JSON-séma a következő típusokhoz automatikusan össze az adatokat a .NET típusú szerializálandó szerződés attribútumok.</span><span class="sxs-lookup"><span data-stu-id="db802-129">**reflection** - The JSON schema for the types is automatically built from the data contract attributes of the .NET types to be serialized.</span></span>
* <span data-ttu-id="db802-130">**általános rekord** -A JSON-séma explicit módon megadott által képviselt rekordban a [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) Ha nincs .NET-típus megadásával írhatja le a sémát, az adatok a meglévő osztály szerializálni.</span><span class="sxs-lookup"><span data-stu-id="db802-130">**generic record** - A JSON schema is explicitly specified in a record represented by the [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) class when no .NET types are present to describe the schema for the data to be serialized.</span></span>

<span data-ttu-id="db802-131">Ha az adatok séma ismert író és az adatfolyam olvasó, az adatküldés sémájú nélkül.</span><span class="sxs-lookup"><span data-stu-id="db802-131">When the data schema is known to both the writer and reader of the stream, the data can be sent without its schema.</span></span> <span data-ttu-id="db802-132">Azokban az esetekben az Avro objektum tároló fájl használata esetén a séma tárolja a fájlon belül.</span><span class="sxs-lookup"><span data-stu-id="db802-132">In cases when an Avro object container file is used, the schema is stored within the file.</span></span> <span data-ttu-id="db802-133">Más paraméterek, például a használt adatok tömörítési kodek adható meg.</span><span class="sxs-lookup"><span data-stu-id="db802-133">Other parameters, such as the codec used for data compression, can be specified.</span></span> <span data-ttu-id="db802-134">Ezek a forgatókönyvek részletesen ismertetett és a következő kód példákban szereplő megoldásokat:</span><span class="sxs-lookup"><span data-stu-id="db802-134">These scenarios are outlined in more detail and illustrated in the following code examples:</span></span>

## <a name="install-avro-library"></a><span data-ttu-id="db802-135">Telepítse az Avro-könyvtár</span><span class="sxs-lookup"><span data-stu-id="db802-135">Install Avro Library</span></span>
<span data-ttu-id="db802-136">A következőkre szükség a szalagtár telepítése előtt:</span><span class="sxs-lookup"><span data-stu-id="db802-136">The following are required before you install the library:</span></span>

* <span data-ttu-id="db802-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">A Microsoft .NET-keretrendszer 4</a></span><span class="sxs-lookup"><span data-stu-id="db802-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span></span>
* <span data-ttu-id="db802-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="db802-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 or later)</span></span>

<span data-ttu-id="db802-139">Vegye figyelembe, hogy a Newtonsoft.Json.dll függőség a Microsoft az Avro Library a telepítés automatikusan le.</span><span class="sxs-lookup"><span data-stu-id="db802-139">Note that the Newtonsoft.Json.dll dependency is downloaded automatically with the installation of the Microsoft Avro Library.</span></span> <span data-ttu-id="db802-140">Az eljárás a következő szakaszban találhatók:</span><span class="sxs-lookup"><span data-stu-id="db802-140">The procedure is provided in the following section:</span></span>

<span data-ttu-id="db802-141">A Microsoft az Avro Library az alábbi eljárás segítségével telepíthető a Visual Studio NuGet-csomag terjesztése:</span><span class="sxs-lookup"><span data-stu-id="db802-141">The Microsoft Avro Library is distributed as a NuGet package that can be installed from Visual Studio via the following procedure:</span></span>

1. <span data-ttu-id="db802-142">Válassza ki a **projekt** lapon -> **NuGet-csomagok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="db802-142">Select the **Project** tab -> **Manage NuGet Packages...**</span></span>
2. <span data-ttu-id="db802-143">Keressen a "Microsoft.Hadoop.Avro" kifejezésre a **keresési Online** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="db802-143">Search for "Microsoft.Hadoop.Avro" in the **Search Online** box.</span></span>
3. <span data-ttu-id="db802-144">Kattintson a **telepítése** gombra **Microsoft Azure HDInsight Avro könyvtár**.</span><span class="sxs-lookup"><span data-stu-id="db802-144">Click the **Install** button next to **Microsoft Azure HDInsight Avro Library**.</span></span>

<span data-ttu-id="db802-145">Vegye figyelembe, hogy a Newtonsoft.Json.dll (> = 6.0.4) függőségi együtt a Microsoft az Avro Library is letöltődik automatikusan.</span><span class="sxs-lookup"><span data-stu-id="db802-145">Note that the Newtonsoft.Json.dll (>=6.0.4) dependency is also downloaded automatically with the Microsoft Avro Library.</span></span>

<span data-ttu-id="db802-146">A Microsoft az Avro Library a forráskód nem érhető el [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="db802-146">The Microsoft Avro Library source code is available at [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

## <a name="compile-schemas-using-avro-library"></a><span data-ttu-id="db802-147">Az Avro szalagtárat használó sémák fordítása</span><span class="sxs-lookup"><span data-stu-id="db802-147">Compile schemas using Avro Library</span></span>
<span data-ttu-id="db802-148">A Microsoft az Avro Library tartalmaz egy kód generálása segédprogram, amely lehetővé teszi, hogy a korábban meghatározott JSON-séma alapján automatikusan C# típusok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="db802-148">The Microsoft Avro Library contains a code generation utility that allows creating C# types automatically based on the previously defined JSON schema.</span></span> <span data-ttu-id="db802-149">A kód generálása segédprogram nem terjesztése egy bináris végrehajtható fájlt, de az alábbi eljárás segítségével könnyen építhetők:</span><span class="sxs-lookup"><span data-stu-id="db802-149">The code generation utility is not distributed as a binary executable, but can be easily built via the following procedure:</span></span>

1. <span data-ttu-id="db802-150">Töltse le a .zip fájlt forráskódjának HDInsight SDK legújabb verziójával <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK a Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="db802-150">Download the .zip file with the latest version of HDInsight SDK source code from <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK For Hadoop</a>.</span></span> <span data-ttu-id="db802-151">(Kattintson a **letöltése** ikonra, nem a **letölti** lapon.)</span><span class="sxs-lookup"><span data-stu-id="db802-151">(Click the **Download** icon, not the **Downloads** tab.)</span></span>
2. <span data-ttu-id="db802-152">A HDInsight SDK a kivonatot egy könyvtár a .NET-keretrendszer 4 gépen telepítve, és csatlakozik az internethez szükséges függőség NuGet-csomagok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="db802-152">Extract the HDInsight SDK to a directory on the machine with .NET Framework 4 installed and connected to the Internet for downloading necessary dependency NuGet packages.</span></span> <span data-ttu-id="db802-153">Az alábbiakban feltételezzük, hogy a forráskód C:\SDK kicsomagolta.</span><span class="sxs-lookup"><span data-stu-id="db802-153">Below, we assume that the source code is extracted to C:\SDK.</span></span>
3. <span data-ttu-id="db802-154">A mappában C:\SDK\src\Microsoft.Hadoop.Avro.Tools, és futtassa a build.bat.</span><span class="sxs-lookup"><span data-stu-id="db802-154">Go to the folder C:\SDK\src\Microsoft.Hadoop.Avro.Tools and run build.bat.</span></span> <span data-ttu-id="db802-155">(A fájl hívások MSBuild a 32 bites terjesztési a .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="db802-155">(The file calls MSBuild from the 32-bit distribution of the .NET Framework.</span></span> <span data-ttu-id="db802-156">Ha azt szeretné, hogy 64 bites verzióját használja, szerkesztése build.bat, a megjegyzéseket, a fájlban a következő.) Győződjön meg arról, hogy a build sikeres.</span><span class="sxs-lookup"><span data-stu-id="db802-156">If you would like to use the 64-bit version, edit build.bat, following the comments inside the file.) Ensure that the build is successful.</span></span> <span data-ttu-id="db802-157">(Egyes rendszerek MSBuild készíthet figyelmeztetéseket.</span><span class="sxs-lookup"><span data-stu-id="db802-157">(On some systems, MSBuild may produce warnings.</span></span> <span data-ttu-id="db802-158">Ezek a figyelmeztetések nem befolyásolják a segédprogram mindaddig, amíg nincsenek build hibák.)</span><span class="sxs-lookup"><span data-stu-id="db802-158">These warnings do not affect the utility as long as there are no build errors.)</span></span>
4. <span data-ttu-id="db802-159">A lefordított segédprogram C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools található.</span><span class="sxs-lookup"><span data-stu-id="db802-159">The compiled utility is located in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span></span>

<span data-ttu-id="db802-160">Ahhoz, hogy ismeri a parancssori szintaxist, a mappából, ahol a kód generálása segédprogram-e végre az alábbi parancsot:`Microsoft.Hadoop.Avro.Tools help /c:codegen`</span><span class="sxs-lookup"><span data-stu-id="db802-160">To get familiar with the command-line syntax, execute the following command from the folder where the code generation utility is located: `Microsoft.Hadoop.Avro.Tools help /c:codegen`</span></span>

<span data-ttu-id="db802-161">A segédprogram teszteléséhez C# osztályokat is generálása a megadott forráskódja minta JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="db802-161">To test the utility, you can generate C# classes from the sample JSON schema file provided with the source code.</span></span> <span data-ttu-id="db802-162">Hajtsa végre a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="db802-162">Execute the following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

<span data-ttu-id="db802-163">Ez az aktuális könyvtárban található fájlok két C# létrehozásához kellene: SensorData.cs és Location.cs.</span><span class="sxs-lookup"><span data-stu-id="db802-163">This is supposed to produce two C# files in the current directory: SensorData.cs and Location.cs.</span></span>

<span data-ttu-id="db802-164">A programot, amely a JSON-séma C# típusú konvertálása során használja a kód generálása segédprogram ismertetése: a fájl GenerationVerification.feature C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc található.</span><span class="sxs-lookup"><span data-stu-id="db802-164">To understand the logic that the code generation utility is using while converting the JSON schema to C# types, see the file GenerationVerification.feature located in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span></span>

<span data-ttu-id="db802-165">Névterek kinyert a JSON-séma, a fájlban, az előző bekezdésben szereplő esetekben ismertetett logika használatával.</span><span class="sxs-lookup"><span data-stu-id="db802-165">Namespaces are extracted from the JSON schema, using the logic described in the file mentioned in the previous paragraph.</span></span> <span data-ttu-id="db802-166">A séma kinyert névterek élveznek függetlenül biztosított parancssori segédprogram, az n paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="db802-166">Namespaces extracted from the schema take precedence over whatever is provided with the /n parameter in the utility command line.</span></span> <span data-ttu-id="db802-167">Ha azt szeretné, felülbírálhatja a névterek, a séma belül található, akkor használja a /nf paramétert.</span><span class="sxs-lookup"><span data-stu-id="db802-167">If you want to override the namespaces contained within the schema, use the /nf parameter.</span></span> <span data-ttu-id="db802-168">Például a SampleJSONSchema.avsc my.own.nspace az összes névtér módosításához hajtsa végre a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="db802-168">For example, to change all namespaces from the SampleJSONSchema.avsc to my.own.nspace, execute the following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a><span data-ttu-id="db802-169">Tudnivalók a minták</span><span class="sxs-lookup"><span data-stu-id="db802-169">About the samples</span></span>
<span data-ttu-id="db802-170">Ebben a témakörben bemutatott hat példák bemutatják a Microsoft az Avro Library által támogatott különböző forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="db802-170">Six examples provided in this topic illustrate different scenarios supported by the Microsoft Avro Library.</span></span> <span data-ttu-id="db802-171">A Microsoft az Avro Library tervezték az adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="db802-171">The Microsoft Avro Library is designed to work with any stream.</span></span> <span data-ttu-id="db802-172">Ezekben a példákban-adatok n keresztül memória adatfolyamok helyett, fájl adatfolyamok vagy az egyszerűség és konzisztenciáját.</span><span class="sxs-lookup"><span data-stu-id="db802-172">In these examples, data is manipulated via memory streams rather than file streams or databases for simplicity and consistency.</span></span> <span data-ttu-id="db802-173">Éles környezetben a megközelítést a pontos forgatókönyv-követelményeinek, az adatforrás és a kötet, a teljesítmény korlátozások és a más tényezőktől függ.</span><span class="sxs-lookup"><span data-stu-id="db802-173">The approach taken in a production environment depends on the exact scenario requirements, data source and volume, performance constraints, and other factors.</span></span>

<span data-ttu-id="db802-174">Az első két példák bemutatják, hogyan szerializálása és deszerializálása adatok memória adatfolyam pufferek az általános és a reflexió használatával.</span><span class="sxs-lookup"><span data-stu-id="db802-174">The first two examples show how to serialize and deserialize data into memory stream buffers by using reflection and generic records.</span></span> <span data-ttu-id="db802-175">Két esetben a séma feltételezett, hogy az olvasók és írók között meg kell osztani a sávon kívüli.</span><span class="sxs-lookup"><span data-stu-id="db802-175">The schema in these two cases is assumed to be shared between the readers and writers out-of-band.</span></span>

<span data-ttu-id="db802-176">A harmadik és negyedik példák bemutatják, hogyan lehet szerializálni, és az Avro objektum tároló fájlokkal adatok deszerializálása.</span><span class="sxs-lookup"><span data-stu-id="db802-176">The third and fourth examples show how to serialize and deserialize data by using the Avro object container files.</span></span> <span data-ttu-id="db802-177">Adatok az Avro-tároló fájl tárolja, amikor a séma mindig tárolása vele, mert a séma meg kell osztani a deszerializáláshoz.</span><span class="sxs-lookup"><span data-stu-id="db802-177">When data is stored in an Avro container file, its schema is always stored with it because the schema must be shared for deserialization.</span></span>

<span data-ttu-id="db802-178">A minta az első négy példák tartalmazó tölthető le: a <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure mintakódok</a> hely.</span><span class="sxs-lookup"><span data-stu-id="db802-178">The sample containing the first four examples can be downloaded from the <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="db802-179">Az ötödik példa bemutatja, hogyan használható egy egyéni tömörítési kodek Avro objektum tárolófájlokba.</span><span class="sxs-lookup"><span data-stu-id="db802-179">The fifth example shows how to use a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="db802-180">Az ebben a példában letölthető szükséges kódot tartalmazó minta a <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure mintakódok</a> hely.</span><span class="sxs-lookup"><span data-stu-id="db802-180">A sample containing the code for this example can be downloaded from the <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="db802-181">A hatodik minta bemutatja, hogyan használható Avro szerializálása feltölteni az adatokat az Azure Blob storage és majd elemezni egy HDInsight (Hadoop) fürthöz való Hive használatával.</span><span class="sxs-lookup"><span data-stu-id="db802-181">The sixth sample shows how to use Avro serialization to upload data to Azure Blob storage and then analyze it by using Hive with an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="db802-182">Le is tölthetők: a <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure mintakódok</a> hely.</span><span class="sxs-lookup"><span data-stu-id="db802-182">It can be downloaded from the <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="db802-183">Az alábbiakban a hat minták, a témakörben tárgyalt mutató hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="db802-183">Here are links to the six samples discussed in the topic:</span></span>

* <span data-ttu-id="db802-184"><a href="#Scenario1">**A reflexió szerializálási** </a> -típusok szerializálandó a JSON-séma automatikusan össze az adatokat szerződés attribútumok.</span><span class="sxs-lookup"><span data-stu-id="db802-184"><a href="#Scenario1">**Serialization with reflection**</a> - The JSON schema for types to be serialized is automatically built from the data contract attributes.</span></span>
* <span data-ttu-id="db802-185"><a href="#Scenario2">**Általános rekordot tartalmazó szerializálási** </a> -a JSON-séma explicit módon megadott rekord elérhető reflexióra nincs .NET típus esetén.</span><span class="sxs-lookup"><span data-stu-id="db802-185"><a href="#Scenario2">**Serialization with generic record**</a> - The JSON schema is explicitly specified in a record when no .NET type is available for reflection.</span></span>
* <span data-ttu-id="db802-186"><a href="#Scenario3">**Objektum tároló fájlok használata a reflexió szerializálási** </a> -a JSON-séma automatikusan összeállítása és megosztott együtt a szerializált adatok keresztül az Avro tároló fájlt.</span><span class="sxs-lookup"><span data-stu-id="db802-186"><a href="#Scenario3">**Serialization using object container files with reflection**</a> - The JSON schema is automatically built and shared together with the serialized data via an Avro object container file.</span></span>
* <span data-ttu-id="db802-187"><a href="#Scenario4">**Általános rekordot tartalmazó objektum tárolófájlokba használatával szerializálási** </a> -a JSON-séma explicit módon megadott előtt a szerializálás és az Avro objektum tárolót fájlon keresztül az adatok és megosztott.</span><span class="sxs-lookup"><span data-stu-id="db802-187"><a href="#Scenario4">**Serialization using object container files with generic record**</a> - The JSON schema is explicitly specified before the serialization and shared together with the data via an Avro object container file.</span></span>
* <span data-ttu-id="db802-188"><a href="#Scenario5">**Objektum tárolófájlokba használata egy egyéni tömörítési kodek szerializálási** </a> -a példa bemutatja, hogyan hozzon létre egy Avro objektum tároló fájlt egy testreszabott .NET megvalósításáról a Deflate adatok tömörítési kodek.</span><span class="sxs-lookup"><span data-stu-id="db802-188"><a href="#Scenario5">**Serialization using object container files with a custom compression codec**</a> - The example shows how to create an Avro object container file with a customized .NET implementation of the Deflate data compression codec.</span></span>
* <span data-ttu-id="db802-189"><a href="#Scenario6">**Az Avro használatával feltölteni az adatokat a Microsoft Azure HDInsight szolgáltatás** </a> -a példa bemutatja, hogyan kommunikál az Avro szerializálási a HDInsight-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="db802-189"><a href="#Scenario6">**Using Avro to upload data for the Microsoft Azure HDInsight service**</a> - The example illustrates how Avro serialization interacts with the HDInsight service.</span></span> <span data-ttu-id="db802-190">Ez a példa futtatásához szükséges egy aktív Azure-előfizetés és Azure HDInsight-fürtök a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="db802-190">An active Azure subscription and access to an Azure HDInsight cluster are required to run this example.</span></span>

## <span data-ttu-id="db802-191"><a name="Scenario1"></a>1. példa: Szerializálási a reflexió</span><span class="sxs-lookup"><span data-stu-id="db802-191"><a name="Scenario1"></a>Sample 1: Serialization with reflection</span></span>
<span data-ttu-id="db802-192">A JSON-séma a következő típusokhoz automatikusan épül fel az adatokat a reflexió használatával a Microsoft az Avro Library a C# objektumokat szerializálni szerződés attribútumait.</span><span class="sxs-lookup"><span data-stu-id="db802-192">The JSON schema for the types can be automatically built by the Microsoft Avro Library via reflection from the data contract attributes of the C# objects to be serialized.</span></span> <span data-ttu-id="db802-193">A Microsoft az Avro Library létrehoz egy [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) szerializálandó mezők azonosításához.</span><span class="sxs-lookup"><span data-stu-id="db802-193">The Microsoft Avro Library creates an [**IAvroSeralizer<T>**](http://msdn.microsoft.com/library/dn627341.aspx) to identify the fields to be serialized.</span></span>

<span data-ttu-id="db802-194">Ebben a példában objektumokat (egy **SensorData** tag osztályra **hely** struct) szerializálva vannak memória adatfolyamba, és az adatfolyam pedig deszerializálva..</span><span class="sxs-lookup"><span data-stu-id="db802-194">In this example, objects (a **SensorData** class with a member **Location** struct) are serialized to a memory stream, and this stream is in turn deserialized.</span></span> <span data-ttu-id="db802-195">Az eredmény ezután annak ellenőrzésére, hogy a kezdeti példányszámnak a rendszer összehasonlítja a **SensorData** az eredeti helyre objektum megegyezik.</span><span class="sxs-lookup"><span data-stu-id="db802-195">The result is then compared to the initial instance to confirm that the **SensorData** object recovered is identical to the original.</span></span>

<span data-ttu-id="db802-196">Ebben a példában a séma feltételezett, hogy között meg kell osztani az olvasók és írók, így nincs szükség az Avro objektum tárolóformátummal.</span><span class="sxs-lookup"><span data-stu-id="db802-196">The schema in this example is assumed to be shared between the readers and writers, so the Avro object container format is not required.</span></span> <span data-ttu-id="db802-197">Példa bemutatja, hogyan szerializálása és deszerializálása adatok a memóriában puffereli használatával reflexiós a tároló objektum formátumban, abban az esetben, ha a séma meg kell osztani az adatokat, lásd: <a href="#Scenario3">objektum tároló fájlok használata a reflexiószerializálási</a>.</span><span class="sxs-lookup"><span data-stu-id="db802-197">For an example of how to serialize and deserialize data into memory buffers by using reflection with the object container format when the schema must be shared with the data, see <a href="#Scenario3">Serialization using object container files with reflection</a>.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-2-serialization-with-a-generic-record"></a><span data-ttu-id="db802-198">2. példa: Szerializálási általános rekordot tartalmazó</span><span class="sxs-lookup"><span data-stu-id="db802-198">Sample 2: Serialization with a generic record</span></span>
<span data-ttu-id="db802-199">A JSON-séma adható explicit módon meg egy általános rekordban Ha reflexiós nem használható, mert az adatok nem reprezentálhatók egy adategyezmény a .NET-osztályok keresztül.</span><span class="sxs-lookup"><span data-stu-id="db802-199">A JSON schema can be explicitly specified in a generic record when reflection cannot be used because the data cannot be represented via .NET classes with a data contract.</span></span> <span data-ttu-id="db802-200">Ez a módszer akkor lassabb, mint a következőt reflexió használatával.</span><span class="sxs-lookup"><span data-stu-id="db802-200">This method is slower than using reflection.</span></span> <span data-ttu-id="db802-201">Ilyen esetben a sémát is lehet dinamikus, ez azt jelenti, hogy nem ismeri a fordítás során.</span><span class="sxs-lookup"><span data-stu-id="db802-201">In such cases, the schema for the data may also be dynamic, that is, not known at compile time.</span></span> <span data-ttu-id="db802-202">Vesszővel tagolt (CSV) fájlok, amelyek séma nem ismeretlen, amíg a futási időben az Avro formátum rendszer átalakítja-ként adata az ilyen dinamikus forgatókönyv egy példát.</span><span class="sxs-lookup"><span data-stu-id="db802-202">Data represented as comma-separated values (CSV) files whose schema is unknown until it is transformed to the Avro format at run time is an example of this sort of dynamic scenario.</span></span>

<span data-ttu-id="db802-203">Ez a példa bemutatja, hogyan létrehozhat és használhat egy [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) való explicit módon adja meg a JSON-séma, hogyan töltse fel adatokkal az adatokat, és majd szerializálása és deszerializálása azt.</span><span class="sxs-lookup"><span data-stu-id="db802-203">This example shows how to create and use an [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) to explicitly specify a JSON schema, how to populate it with the data, and then how to serialize and deserialize it.</span></span> <span data-ttu-id="db802-204">Az eredmény ezután összeveti a kezdeti példányszámnak annak ellenőrzéséhez, hogy a rekord helyreállítani az eredeti azonos.</span><span class="sxs-lookup"><span data-stu-id="db802-204">The result is then compared to the initial instance to confirm that the record recovered is identical to the original.</span></span>

<span data-ttu-id="db802-205">Ebben a példában a séma feltételezett, hogy között meg kell osztani az olvasók és írók, így nincs szükség az Avro objektum tárolóformátummal.</span><span class="sxs-lookup"><span data-stu-id="db802-205">The schema in this example is assumed to be shared between the readers and writers, so the Avro object container format is not required.</span></span> <span data-ttu-id="db802-206">Példa bemutatja, hogyan szerializálása és deszerializálása adatok a memóriában puffereli használatával egy általános rekordban a tároló objektum formátumban, ha a séma szerepelnie kell a szerializált adatok a, tekintse meg a <a href="#Scenario4">objektum tároló fájlokkal rendelkező szerializálási általános rekord</a> példa.</span><span class="sxs-lookup"><span data-stu-id="db802-206">For an example of how to serialize and deserialize data into memory buffers by using a generic record with the object container format when the schema must be included with the serialized data, see the <a href="#Scenario4">Serialization using object container files with generic record</a> example.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a><span data-ttu-id="db802-207">3. példa: Szerializálási a reflexió objektum tároló fájlok és a szerializálási használja</span><span class="sxs-lookup"><span data-stu-id="db802-207">Sample 3: Serialization using object container files and serialization with reflection</span></span>
<span data-ttu-id="db802-208">Ebben a példában a forgatókönyv hasonlít a <a href="#Scenario1"> első példában</a>, ahol a séma implicit módon megadott a reflexió.</span><span class="sxs-lookup"><span data-stu-id="db802-208">This example is similar to the scenario in the <a href="#Scenario1"> first example</a>, where the schema is implicitly specified with reflection.</span></span> <span data-ttu-id="db802-209">A különbség, hogy itt, a séma nem feltételezett, hogy az olvasó deserializes azt, hogy ismert.</span><span class="sxs-lookup"><span data-stu-id="db802-209">The difference is that here, the schema is not assumed to be known to the reader that deserializes it.</span></span> <span data-ttu-id="db802-210">A **SensorData** szerializálható objektumok és azok implicit módon megadott séma fájlban vannak tárolva az Avro objektum tároló által képviselt a [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="db802-210">The **SensorData** objects to be serialized and their implicitly specified schema are stored in an Avro object container file represented by the [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span>

<span data-ttu-id="db802-211">Ebben a példában a szerializált adatok [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) és a deszerializált [ **SequentialReader<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx).</span><span class="sxs-lookup"><span data-stu-id="db802-211">The data is serialized in this example with [**SequentialWriter<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx) and deserialized with [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx).</span></span> <span data-ttu-id="db802-212">Az eredmény ezután a rendszer összehasonlítja a kezdeti példányok identitás biztosításához.</span><span class="sxs-lookup"><span data-stu-id="db802-212">The result then is compared to the initial instances to ensure identity.</span></span>

<span data-ttu-id="db802-213">A rendszer tömöríti az adatokat a objektum tároló fájlban az alapértelmezett keresztül [ **Deflate** ] [ deflate-100] tömörítési kodek a .NET-keretrendszer 4.</span><span class="sxs-lookup"><span data-stu-id="db802-213">The data in the object container file is compressed via the default [**Deflate**][deflate-100] compression codec from .NET Framework 4.</span></span> <span data-ttu-id="db802-214">Tekintse meg a <a href="#Scenario5"> ötödik példa</a> ebben a témakörben további információt az újabb és felső szintű verzióját használja a [ **Deflate** ] [ deflate-110] tömörítési kodek .NET-keretrendszer 4.5 érhető el.</span><span class="sxs-lookup"><span data-stu-id="db802-214">See the <a href="#Scenario5"> fifth example</a> in this topic to learn how to use a more recent and superior version of the [**Deflate**][deflate-110] compression codec available in .NET Framework 4.5.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a><span data-ttu-id="db802-215">4. példa: Szerializálási általános rekordot tartalmazó objektum tárolófájlokba és szerializálási használatával</span><span class="sxs-lookup"><span data-stu-id="db802-215">Sample 4: Serialization using object container files and serialization with generic record</span></span>
<span data-ttu-id="db802-216">Ebben a példában a forgatókönyv hasonlít a <a href="#Scenario2"> második példáját</a>, ahol a séma explicit módon megadott rendelkező JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="db802-216">This example is similar to the scenario in the <a href="#Scenario2"> second example</a>, where the schema is explicitly specified with JSON.</span></span> <span data-ttu-id="db802-217">A különbség, hogy itt, a séma nem feltételezett, hogy az olvasó deserializes azt, hogy ismert.</span><span class="sxs-lookup"><span data-stu-id="db802-217">The difference is that here, the schema is not assumed to be known to the reader that deserializes it.</span></span>

<span data-ttu-id="db802-218">Egy gyűjtött adatok TesztKészlet [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) keresztül explicit módon megadott JSON-séma objektumokat, és egy objektum tároló fájl által képviselt majd tárolja a [  **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="db802-218">The test data set is collected into a list of [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objects via an explicitly defined JSON schema and then stored in an object container file represented by the [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span> <span data-ttu-id="db802-219">A tároló fájlt hoz létre egy író szerializálni az adatokat, majd a fájl mentett memória adatfolyamba tömörítetlen használt.</span><span class="sxs-lookup"><span data-stu-id="db802-219">This container file creates a writer that is used to serialize the data, uncompressed, to a memory stream that is then saved to a file.</span></span> <span data-ttu-id="db802-220">A [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) az olvasó létrehozásakor használt paraméter határozza meg, hogy ezek az adatok nem tömöríti.</span><span class="sxs-lookup"><span data-stu-id="db802-220">The [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter used for creating the reader specifies that this data is not compressed.</span></span>

<span data-ttu-id="db802-221">Az adatok majd kiolvasni a fájlból és az objektumok gyűjteménye deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="db802-221">The data is then read from the file and deserialized into a collection of objects.</span></span> <span data-ttu-id="db802-222">Ez a gyűjtemény a rendszer összehasonlítja a kiindulási lista az Avro rekordok annak ellenőrzéséhez, hogy azok egyezését.</span><span class="sxs-lookup"><span data-stu-id="db802-222">This collection is compared to the initial list of Avro records to confirm that they are identical.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a><span data-ttu-id="db802-223">5. példa: Szerializálási egy egyéni tömörítési kodek objektum tároló fájlok használata</span><span class="sxs-lookup"><span data-stu-id="db802-223">Sample 5: Serialization using object container files with a custom compression codec</span></span>
<span data-ttu-id="db802-224">Az ötödik példa bemutatja, hogyan használható egy egyéni tömörítési kodek Avro objektum tárolófájlokba.</span><span class="sxs-lookup"><span data-stu-id="db802-224">The fifth example shows how to use a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="db802-225">Az ebben a példában letölthető szükséges kódot tartalmazó minta a [Azure mintakódok](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) hely.</span><span class="sxs-lookup"><span data-stu-id="db802-225">A sample containing the code for this example can be downloaded from the [Azure code samples](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.</span></span>

<span data-ttu-id="db802-226">A [Avro specifikációjában](http://avro.apache.org/docs/current/spec.html#Required+Codecs) lehetővé teszi, hogy egy nem kötelező tömörítési kodek használatát (kívül **Null** és **Deflate** alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="db802-226">The [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) allows usage of an optional compression codec (in addition to **Null** and **Deflate** defaults).</span></span> <span data-ttu-id="db802-227">Ebben a példában nem implementálja az Snappy például egy új kodek (egy támogatott választható kodek az említett a [Avro specifikációjában](http://avro.apache.org/docs/current/spec.html#snappy)).</span><span class="sxs-lookup"><span data-stu-id="db802-227">This example is not implementing a new codec such as Snappy (mentioned as a supported optional codec in the [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span></span> <span data-ttu-id="db802-228">Azt illusztrálja, hogyan használata a .NET-keretrendszer 4.5 megvalósítása a [ **Deflate** ] [ deflate-110] kodek, így jobban tömörítési algoritmust alapján a [zlib ](http://zlib.net/) verziójú, mint az alapértelmezett .NET-keretrendszer 4-tömörítési könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="db802-228">It shows how to use the .NET Framework 4.5 implementation of the [**Deflate**][deflate-110] codec, which provides a better compression algorithm based on the [zlib](http://zlib.net/) compression library than the default .NET Framework 4 version.</span></span>

    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It catches the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it relies on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a><span data-ttu-id="db802-229">6. példa: Avro feltölteni az adatokat a Microsoft Azure HDInsight szolgáltatás használatával.</span><span class="sxs-lookup"><span data-stu-id="db802-229">Sample 6: Using Avro to upload data for the Microsoft Azure HDInsight service</span></span>
<span data-ttu-id="db802-230">A hatodik példa bemutatja az egyes programozási módszerek használják az Azure HDInsight szolgáltatással kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="db802-230">The sixth example illustrates some programming techniques related to interacting with the Azure HDInsight service.</span></span> <span data-ttu-id="db802-231">Az ebben a példában letölthető szükséges kódot tartalmazó minta a [Azure mintakódok](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) hely.</span><span class="sxs-lookup"><span data-stu-id="db802-231">A sample containing the code for this example can be downloaded from the [Azure code samples](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.</span></span>

<span data-ttu-id="db802-232">A minta a következő feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="db802-232">The sample does the following tasks:</span></span>

* <span data-ttu-id="db802-233">Csatlakozik egy meglévő HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="db802-233">Connects to an existing HDInsight service cluster.</span></span>
* <span data-ttu-id="db802-234">Rendezi sorba több CSV-fájlt és feltölti az eredményt az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="db802-234">Serializes several CSV files and uploads the result to Azure Blob storage.</span></span> <span data-ttu-id="db802-235">(A CSV-fájlok a minta együtt küld, és megfelelnek-e terjesztve mellékletben készlet előzményadatokat kivonatát [Infochimps](http://www.infochimps.com/) 1970-2010 időszakra.</span><span class="sxs-lookup"><span data-stu-id="db802-235">(The CSV files are distributed together with the sample and represent an extract from AMEX Stock historical data distributed by [Infochimps](http://www.infochimps.com/) for the period 1970-2010.</span></span> <span data-ttu-id="db802-236">A minta CSV-fájl adatokat olvas, alakítja át a rekordok példányait a **készlet** osztály, és ezután rendezi sorba őket reflexió használatával.</span><span class="sxs-lookup"><span data-stu-id="db802-236">The sample reads CSV file data, converts the records to instances of the **Stock** class, and then serializes them by using reflection.</span></span> <span data-ttu-id="db802-237">A JSON-séma segítségével a Microsoft az Avro Library kód generálása segédprogram készlet definíciójának készült.</span><span class="sxs-lookup"><span data-stu-id="db802-237">Stock type definition is created from a JSON schema via the Microsoft Avro Library code generation utility.</span></span>
* <span data-ttu-id="db802-238">Táblázatot hoz létre új külső nevű **készletek** a Hive és a hivatkozásokat úgy, hogy az adatok feltöltése az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="db802-238">Creates a new external table called **Stocks** in Hive, and links it to the data uploaded in the previous step.</span></span>
* <span data-ttu-id="db802-239">A lekérdezés végrehajtja a Hive használatával keresztül a **készletek** tábla.</span><span class="sxs-lookup"><span data-stu-id="db802-239">Executes a query by using Hive over the **Stocks** table.</span></span>

<span data-ttu-id="db802-240">Emellett a minta folyamatot hajt végre a karbantartás előtt és után fő műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="db802-240">In addition, the sample performs a clean-up procedure before and after performing major operations.</span></span> <span data-ttu-id="db802-241">Során a karbantartás minden a kapcsolódó Azure-Blob adatok és a mappák törlődnek, és a Hive tábla megszakad.</span><span class="sxs-lookup"><span data-stu-id="db802-241">During the clean-up, all of the related Azure Blob data and folders are removed, and the Hive table is dropped.</span></span> <span data-ttu-id="db802-242">A karbantartás eljárás a minta parancssorból is hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="db802-242">You can also invoke the clean-up procedure from the sample command line.</span></span>

<span data-ttu-id="db802-243">A minta előfeltételei a következők:</span><span class="sxs-lookup"><span data-stu-id="db802-243">The sample has the following prerequisites:</span></span>

* <span data-ttu-id="db802-244">Egy aktív Microsoft Azure-előfizetést és az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="db802-244">An active Microsoft Azure subscription and its subscription ID.</span></span>
* <span data-ttu-id="db802-245">Egy felügyeleti tanúsítványt, az előfizetés, a megfelelő titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="db802-245">A management certificate for the subscription with the corresponding private key.</span></span> <span data-ttu-id="db802-246">A tanúsítványt kell telepíteni, az aktuális felhasználó titkos tárolási a minta futtatásához használt számítógépen.</span><span class="sxs-lookup"><span data-stu-id="db802-246">The certificate should be installed in the current user private storage on the machine used to run the sample.</span></span>
* <span data-ttu-id="db802-247">Aktív HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="db802-247">An active HDInsight cluster.</span></span>
* <span data-ttu-id="db802-248">A HDInsight-fürthöz az előző előfeltétel, és a megfelelő elsődleges vagy másodlagos elérési kulcsot a kapcsolódó Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="db802-248">An Azure Storage account linked to the HDInsight cluster from the previous prerequisite, together with the corresponding primary, or secondary access key.</span></span>

<span data-ttu-id="db802-249">Minden adatot a Előfeltételek kell adni a minta konfigurációs fájl a minta futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="db802-249">All of the information from the prerequisites should be entered to the sample configuration file before the sample is run.</span></span> <span data-ttu-id="db802-250">Ehhez két lehetséges módja van:</span><span class="sxs-lookup"><span data-stu-id="db802-250">There are two possible ways to do it:</span></span>

* <span data-ttu-id="db802-251">Szerkessze az app.config fájlt, a minta gyökérkönyvtárában, és majd kialakítható a minta</span><span class="sxs-lookup"><span data-stu-id="db802-251">Edit the app.config file in the sample root directory and then build the sample</span></span>
* <span data-ttu-id="db802-252">A minta először létre, és szerkessze a AvroHDISample.exe.config a build könyvtárban</span><span class="sxs-lookup"><span data-stu-id="db802-252">First build the sample, and then edit AvroHDISample.exe.config in the build directory</span></span>

<span data-ttu-id="db802-253">Mindkét esetben minden módosításokat kell végezni a  **<appSettings>**  beállítások szakaszban.</span><span class="sxs-lookup"><span data-stu-id="db802-253">In both cases, all edits should be done in the **<appSettings>** settings section.</span></span> <span data-ttu-id="db802-254">Kövesse a megjegyzéseket, a fájlban.</span><span class="sxs-lookup"><span data-stu-id="db802-254">Follow the comments in the file.</span></span>
<span data-ttu-id="db802-255">A minta futtatása a parancssorból futtassa a következő parancsot (amelyben a .zip-fájlt a mintával lett feltételezi, hogy kibontott C:\AvroHDISample; Ha ellenkező esetben használja a megfelelő elérési útja):</span><span class="sxs-lookup"><span data-stu-id="db802-255">The sample is run from the command line by executing the following command (where the .zip file with the sample was assumed to be extracted to C:\AvroHDISample; if otherwise, use the relevant file path):</span></span>

    AvroHDISample run C:\AvroHDISample\Data

<span data-ttu-id="db802-256">A fürt karbantartása, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="db802-256">To clean up the cluster, run the following command:</span></span>

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
