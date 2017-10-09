---
title: a Hadoop - Microsoft Avro Library - Azure aaaSerialize adatok |} Microsoft Docs
description: "Megtudhatja, hogyan tooserialize és hello Microsoft Avro Library toopersist toomemory, adatbázis, vagy fájl segítségével hdinsight Hadoop az adatokat."
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
ms.openlocfilehash: f364f8e855a54c0fc160e9a106ec8d5b30c6db23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="serialize-data-in-hadoop-with-hello-microsoft-avro-library"></a><span data-ttu-id="b3e4e-104">Hello Microsoft Avro Library a Hadoop adatok szerializálása</span><span class="sxs-lookup"><span data-stu-id="b3e4e-104">Serialize data in Hadoop with hello Microsoft Avro Library</span></span>

>[!NOTE]
><span data-ttu-id="b3e4e-105">a Microsoft hello Avro SDK már nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-105">hello Avro SDK is no longer supported by Microsoft.</span></span> <span data-ttu-id="b3e4e-106">hello könyvtár támogatott nyílt forráskódú közösségi.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-106">hello library is open source community supported.</span></span> <span data-ttu-id="b3e4e-107">hello szalagtár hello források találhatók [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="b3e4e-107">hello sources for hello library are located in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

<span data-ttu-id="b3e4e-108">Ez a témakör bemutatja, hogyan toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objektumok és egyéb adatok szerkezet adatfolyamok toopersist be őket toomemory, adatbázis vagy egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-108">This topic shows how toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objects and other data structures into streams toopersist them toomemory, a database, or a file.</span></span> <span data-ttu-id="b3e4e-109">Azt is bemutatja, hogyan toodeserialize toorecover hello eredeti objektumok őket.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-109">It also shows how toodeserialize them toorecover hello original objects.</span></span>

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a><span data-ttu-id="b3e4e-110">Apache Avro</span><span class="sxs-lookup"><span data-stu-id="b3e4e-110">Apache Avro</span></span>
<span data-ttu-id="b3e4e-111">Hello <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> valósít meg hello Apache Avro adatok szerializálási rendszer hello Microsoft.NET környezethez.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-111">hello <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implements hello Apache Avro data serialization system for hello Microsoft.NET environment.</span></span> <span data-ttu-id="b3e4e-112">Apache Avro kompakt bináris adatcsere adatformátum szerializálási biztosít.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-112">Apache Avro provides a compact binary data interchange format for serialization.</span></span> <span data-ttu-id="b3e4e-113">Használja <a href="http://www.json.org" target="_blank">JSON</a> toodefine egy nyelvtől független sémát, amely lehetővé teszi a nyelvi együttműködést.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-113">It uses <a href="http://www.json.org" target="_blank">JSON</a> toodefine a language-agnostic schema that underwrites language interoperability.</span></span> <span data-ttu-id="b3e4e-114">Több nyelven érhetőek szerializált adatok olvashatók egy másik.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-114">Data serialized in one language can be read in another.</span></span> <span data-ttu-id="b3e4e-115">Jelenleg a C, C++, C#, Java, PHP, Python és Ruby támogatottak.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-115">Currently C, C++, C#, Java, PHP, Python, and Ruby are supported.</span></span> <span data-ttu-id="b3e4e-116">Hello részletes információk találhatók hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro specifikációjában</a>.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-116">Detailed information on hello format can be found in hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span></span> 

>[!NOTE]
><span data-ttu-id="b3e4e-117">az Avro Library Microsoft hello nem támogatja a hello távoli eljáráshívásnak (RPC) hívások részét ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-117">hello Microsoft Avro Library does not support hello remote procedure calls (RPCs) part of this specification.</span></span>
>

<span data-ttu-id="b3e4e-118">az objektumok hello Avro rendszer szerializált hello ábrázolását két részből áll: séma, a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-118">hello serialized representation of an object in hello Avro system consists of two parts: schema and actual value.</span></span> <span data-ttu-id="b3e4e-119">az Avro-séma hello hello nyelvfüggetlen adatmodell az hello szerializált adatok JSON ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-119">hello Avro schema describes hello language-independent data model of hello serialized data with JSON.</span></span> <span data-ttu-id="b3e4e-120">A bináris adatok megjelenítése és jelölőnégyzetként jelenik.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-120">It is presented side by side with a binary representation of data.</span></span> <span data-ttu-id="b3e4e-121">Minden objektum toobe készült nincs érték terhek, így szerializálási gyors és hello ábrázolását kis hello séma elkülönül a bináris megjelenítése hello rendelkező lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-121">Having hello schema separate from hello binary representation permits each object toobe written with no per-value overheads, making serialization fast, and hello representation small.</span></span>

## <a name="hello-hadoop-scenario"></a><span data-ttu-id="b3e4e-122">hello Hadoop forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="b3e4e-122">hello Hadoop scenario</span></span>
<span data-ttu-id="b3e4e-123">hello Apache Avro szerializálási formátum széles körben használt Azure HDInsight és egyéb Apache Hadoop-környezetekben.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-123">hello Apache Avro serialization format is widely used in Azure HDInsight and other Apache Hadoop environments.</span></span> <span data-ttu-id="b3e4e-124">Az Avro biztosít egy kényelmes módszert arra toorepresent összetett adatstruktúra Hadoop MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-124">Avro provides a convenient way toorepresent complex data structures within a Hadoop MapReduce job.</span></span> <span data-ttu-id="b3e4e-125">az Avro-fájlok (Avro tároló fájlt) hello formátuma lett tervezett toosupport hello elosztott MapReduce programozási modellt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-125">hello format of Avro files (Avro object container file) has been designed toosupport hello distributed MapReduce programming model.</span></span> <span data-ttu-id="b3e4e-126">hello kulcs szolgáltatása, amely lehetővé teszi, hogy a hello terjesztési, az, hogy hello fájlok "feloszthatók" hello értelemben, hogy egy fájl bármely pontot kikereshet, és az egy adott blokktól kezdheti-e.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-126">hello key feature that enables hello distribution is that hello files are “splittable” in hello sense that one can seek any point in a file and start reading from a particular block.</span></span>

## <a name="serialization-in-avro-library"></a><span data-ttu-id="b3e4e-127">Az Avro könyvtárban szerializálási</span><span class="sxs-lookup"><span data-stu-id="b3e4e-127">Serialization in Avro Library</span></span>
<span data-ttu-id="b3e4e-128">hello .NET könyvtár az Avro támogatja a szerializálási objektumok két módon:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-128">hello .NET Library for Avro supports two ways of serializing objects:</span></span>

* <span data-ttu-id="b3e4e-129">**Fejlécreflexiós** -hello JSON-séma hello típusok automatikusan beépített hello adatokból hello .NET típusok toobe szerializált szerződés attribútumait.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-129">**reflection** - hello JSON schema for hello types is automatically built from hello data contract attributes of hello .NET types toobe serialized.</span></span>
* <span data-ttu-id="b3e4e-130">**általános rekord** -A JSON-séma explicit módon megadott hello által képviselt rekordban [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) indulásakor nem .NET típusok a következők hello adatok toobe jelen toodescribe hello séma szerializálni.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-130">**generic record** - A JSON schema is explicitly specified in a record represented by hello [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) class when no .NET types are present toodescribe hello schema for hello data toobe serialized.</span></span>

<span data-ttu-id="b3e4e-131">Ha hello adatkulcsokat ismert tooboth hello író és ahhoz való olvasóra hello adatfolyam, hello adatküldés sémájú nélkül.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-131">When hello data schema is known tooboth hello writer and reader of hello stream, hello data can be sent without its schema.</span></span> <span data-ttu-id="b3e4e-132">Azokban az esetekben az Avro objektum tároló fájllal, hello séma tárolja hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-132">In cases when an Avro object container file is used, hello schema is stored within hello file.</span></span> <span data-ttu-id="b3e4e-133">Más paraméterek, például az adatok tömörítésének használt hello kodek adható meg.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-133">Other parameters, such as hello codec used for data compression, can be specified.</span></span> <span data-ttu-id="b3e4e-134">Ezek a forgatókönyvek részletesen ismertetett és mutatja be a következő példák hello:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-134">These scenarios are outlined in more detail and illustrated in hello following code examples:</span></span>

## <a name="install-avro-library"></a><span data-ttu-id="b3e4e-135">Telepítse az Avro-könyvtár</span><span class="sxs-lookup"><span data-stu-id="b3e4e-135">Install Avro Library</span></span>
<span data-ttu-id="b3e4e-136">hello következők szükségesek, hello könyvtár telepítése előtt:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-136">hello following are required before you install hello library:</span></span>

* <span data-ttu-id="b3e4e-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">A Microsoft .NET-keretrendszer 4</a></span><span class="sxs-lookup"><span data-stu-id="b3e4e-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span></span>
* <span data-ttu-id="b3e4e-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="b3e4e-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 or later)</span></span>

<span data-ttu-id="b3e4e-139">Vegye figyelembe, hogy hello Newtonsoft.Json.dll függőségi automatikusan letöltött hello Microsoft Avro Library hello összetevőnek.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-139">Note that hello Newtonsoft.Json.dll dependency is downloaded automatically with hello installation of hello Microsoft Avro Library.</span></span> <span data-ttu-id="b3e4e-140">hello eljárás hello a következő szakaszban találhatók:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-140">hello procedure is provided in hello following section:</span></span>

<span data-ttu-id="b3e4e-141">az Avro Library Microsoft hello eljárást követő hello keresztül telepíthető a Visual Studio NuGet-csomag terjesztése:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-141">hello Microsoft Avro Library is distributed as a NuGet package that can be installed from Visual Studio via hello following procedure:</span></span>

1. <span data-ttu-id="b3e4e-142">Jelölje be hello **projekt** lapon -> **NuGet-csomagok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="b3e4e-142">Select hello **Project** tab -> **Manage NuGet Packages...**</span></span>
2. <span data-ttu-id="b3e4e-143">Keresse meg a "Microsoft.Hadoop.Avro" hello a **keresési Online** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-143">Search for "Microsoft.Hadoop.Avro" in hello **Search Online** box.</span></span>
3. <span data-ttu-id="b3e4e-144">Kattintson a hello **telepítése** gomb melletti túl**Microsoft Azure HDInsight Avro könyvtár**.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-144">Click hello **Install** button next too**Microsoft Azure HDInsight Avro Library**.</span></span>

<span data-ttu-id="b3e4e-145">Vegye figyelembe, hogy hello Newtonsoft.Json.dll (> = 6.0.4) függőségi hello Microsoft Avro Library együtt is letöltődik automatikusan.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-145">Note that hello Newtonsoft.Json.dll (>=6.0.4) dependency is also downloaded automatically with hello Microsoft Avro Library.</span></span>

<span data-ttu-id="b3e4e-146">Microsoft Avro Library forráskód hello érhető el: [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="b3e4e-146">hello Microsoft Avro Library source code is available at [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

## <a name="compile-schemas-using-avro-library"></a><span data-ttu-id="b3e4e-147">Az Avro szalagtárat használó sémák fordítása</span><span class="sxs-lookup"><span data-stu-id="b3e4e-147">Compile schemas using Avro Library</span></span>
<span data-ttu-id="b3e4e-148">az Avro Library Microsoft hello egy segédprogram, amely lehetővé teszi, hogy létrehozása a C# típusok hello alapján automatikusan korábban megadott JSON-séma kódgenerálás tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-148">hello Microsoft Avro Library contains a code generation utility that allows creating C# types automatically based on hello previously defined JSON schema.</span></span> <span data-ttu-id="b3e4e-149">hello kód generálása segédprogram nem winphonesspbootstrapper.exe végrehajtható bináris, de könnyen építhetők eljárást követő hello keresztül:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-149">hello code generation utility is not distributed as a binary executable, but can be easily built via hello following procedure:</span></span>

1. <span data-ttu-id="b3e4e-150">A HDInsight SDK forráskódjának hello legújabb verziójával hello .zip fájl letöltése <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK a Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-150">Download hello .zip file with hello latest version of HDInsight SDK source code from <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK For Hadoop</a>.</span></span> <span data-ttu-id="b3e4e-151">(Kattintson a hello **letöltése** ikonra, nem hello **letölti** lapon.)</span><span class="sxs-lookup"><span data-stu-id="b3e4e-151">(Click hello **Download** icon, not hello **Downloads** tab.)</span></span>
2. <span data-ttu-id="b3e4e-152">Bontsa ki a HDInsight SDK tooa directory hello gépen a .NET-keretrendszer 4 telepítve, és a szükséges függőség NuGet-csomagok letöltése a toohello Internet kapcsolat hello.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-152">Extract hello HDInsight SDK tooa directory on hello machine with .NET Framework 4 installed and connected toohello Internet for downloading necessary dependency NuGet packages.</span></span> <span data-ttu-id="b3e4e-153">Az alábbiakban azt feltételezik, hogy hello forráskód kibontott tooC:\SDK.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-153">Below, we assume that hello source code is extracted tooC:\SDK.</span></span>
3. <span data-ttu-id="b3e4e-154">Nyissa meg toohello mappa C:\SDK\src\Microsoft.Hadoop.Avro.Tools, és futtassa a build.bat.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-154">Go toohello folder C:\SDK\src\Microsoft.Hadoop.Avro.Tools and run build.bat.</span></span> <span data-ttu-id="b3e4e-155">(hello fájl hívásait MSBuild hello 32 bites terjesztése hello .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-155">(hello file calls MSBuild from hello 32-bit distribution of hello .NET Framework.</span></span> <span data-ttu-id="b3e4e-156">Ha szeretné toouse hello 64 bites verziója, szerkesztése build.bat, a következő hello megjegyzések hello fájl.) Győződjön meg arról, hogy hello build sikeres.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-156">If you would like toouse hello 64-bit version, edit build.bat, following hello comments inside hello file.) Ensure that hello build is successful.</span></span> <span data-ttu-id="b3e4e-157">(Egyes rendszerek MSBuild készíthet figyelmeztetéseket.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-157">(On some systems, MSBuild may produce warnings.</span></span> <span data-ttu-id="b3e4e-158">Ezek a figyelmeztetések nem befolyásolják hello segédprogram mindaddig, amíg nincsenek build hibák.)</span><span class="sxs-lookup"><span data-stu-id="b3e4e-158">These warnings do not affect hello utility as long as there are no build errors.)</span></span>
4. <span data-ttu-id="b3e4e-159">lefordított hello segédprogram C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools található.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-159">hello compiled utility is located in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span></span>

<span data-ttu-id="b3e4e-160">hello parancssori szintaxist, ismernie tooget parancs követően – hello mappáját hello kód generálása segédprogram hello hajtható végre:`Microsoft.Hadoop.Avro.Tools help /c:codegen`</span><span class="sxs-lookup"><span data-stu-id="b3e4e-160">tooget familiar with hello command-line syntax, execute hello following command from hello folder where hello code generation utility is located: `Microsoft.Hadoop.Avro.Tools help /c:codegen`</span></span>

<span data-ttu-id="b3e4e-161">tootest hello segédprogram, a C# osztályok hozhat létre a hello minta JSON sémafájl hello forráskód biztosított.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-161">tootest hello utility, you can generate C# classes from hello sample JSON schema file provided with hello source code.</span></span> <span data-ttu-id="b3e4e-162">A következő parancs hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-162">Execute hello following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

<span data-ttu-id="b3e4e-163">Hello aktuális könyvtárban található a várt tooproduce két C#-fájlok: SensorData.cs és Location.cs.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-163">This is supposed tooproduce two C# files in hello current directory: SensorData.cs and Location.cs.</span></span>

<span data-ttu-id="b3e4e-164">toounderstand hello logikát használó hello kód generálása segédprogram konvertálásakor tooC # hello JSON sématípusok, tekintse meg a GenerationVerification.feature található C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-164">toounderstand hello logic that hello code generation utility is using while converting hello JSON schema tooC# types, see hello file GenerationVerification.feature located in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span></span>

<span data-ttu-id="b3e4e-165">Névterek kinyert hello JSON-séma hello előző bekezdésben szereplő esetekben hello fájlban leírt hello logika használatával.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-165">Namespaces are extracted from hello JSON schema, using hello logic described in hello file mentioned in hello previous paragraph.</span></span> <span data-ttu-id="b3e4e-166">Hello séma kinyert névterek élveznek függetlenül biztosított hello n paraméterrel hello segédprogram parancssorban.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-166">Namespaces extracted from hello schema take precedence over whatever is provided with hello /n parameter in hello utility command line.</span></span> <span data-ttu-id="b3e4e-167">Ha azt szeretné, hogy toooverride hello névterek hello séma belül található, használja a hello /nf paramétert.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-167">If you want toooverride hello namespaces contained within hello schema, use hello /nf parameter.</span></span> <span data-ttu-id="b3e4e-168">Például toochange hello SampleJSONSchema.avsc toomy.own.nspace, az összes névtér hajtható végre hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-168">For example, toochange all namespaces from hello SampleJSONSchema.avsc toomy.own.nspace, execute hello following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-hello-samples"></a><span data-ttu-id="b3e4e-169">Kapcsolatos hello minták</span><span class="sxs-lookup"><span data-stu-id="b3e4e-169">About hello samples</span></span>
<span data-ttu-id="b3e4e-170">Ebben a témakörben bemutatott hat példák bemutatják a Microsoft Avro Library hello által támogatott különböző forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-170">Six examples provided in this topic illustrate different scenarios supported by hello Microsoft Avro Library.</span></span> <span data-ttu-id="b3e4e-171">az Avro Library Microsoft hello az adatfolyam-tervezett toowork.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-171">hello Microsoft Avro Library is designed toowork with any stream.</span></span> <span data-ttu-id="b3e4e-172">Ezekben a példákban-adatok n keresztül memória adatfolyamok helyett, fájl adatfolyamok vagy az egyszerűség és konzisztenciáját.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-172">In these examples, data is manipulated via memory streams rather than file streams or databases for simplicity and consistency.</span></span> <span data-ttu-id="b3e4e-173">éles környezetben hello megközelítést hello pontos forgatókönyv-követelményeinek, az adatforrás és a kötet, a teljesítmény korlátozások és a más tényezőktől függ.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-173">hello approach taken in a production environment depends on hello exact scenario requirements, data source and volume, performance constraints, and other factors.</span></span>

<span data-ttu-id="b3e4e-174">Hogyan hello első két példa megjelenítése tooserialize és adatok deszerializálása memória adatfolyam pufferek az általános és a reflexió használatával.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-174">hello first two examples show how tooserialize and deserialize data into memory stream buffers by using reflection and generic records.</span></span> <span data-ttu-id="b3e4e-175">hello két esetben a séma feltételezett hello olvasók és írók között megosztott toobe sávon kívüli.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-175">hello schema in these two cases is assumed toobe shared between hello readers and writers out-of-band.</span></span>

<span data-ttu-id="b3e4e-176">hello harmadik és negyedik példák megjelenítése hogyan tooserialize és adatok deszerializálása hello Avro objektum tároló fájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-176">hello third and fourth examples show how tooserialize and deserialize data by using hello Avro object container files.</span></span> <span data-ttu-id="b3e4e-177">Adatok az Avro-tároló fájl tárolja, amikor a séma mindig tárolása vele, mert hello séma meg kell osztani a deszerializáláshoz.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-177">When data is stored in an Avro container file, its schema is always stored with it because hello schema must be shared for deserialization.</span></span>

<span data-ttu-id="b3e4e-178">hello mintát tartalmazó hello első négy példák tölthető le: hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure mintakódok</a> hely.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-178">hello sample containing hello first four examples can be downloaded from hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="b3e4e-179">ötödik példa azt mutatja meg hello hogyan toouse az avro-hoz egy egyéni tömörítési kodek objektum tárolófájlokba.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-179">hello fifth example shows how toouse a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="b3e4e-180">Egy minta hello kódot tartalmazó, az ebben a példában tölthető le: hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure mintakódok</a> hely.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-180">A sample containing hello code for this example can be downloaded from hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="b3e4e-181">hello hatodik példa bemutatja, hogyan toouse Avro szerializálási tooupload adatok tooAzure Blob-tároló, és majd elemezni egy HDInsight (Hadoop) fürthöz való Hive használatával.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-181">hello sixth sample shows how toouse Avro serialization tooupload data tooAzure Blob storage and then analyze it by using Hive with an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="b3e4e-182">Le is tölthetők: hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure mintakódok</a> hely.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-182">It can be downloaded from hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="b3e4e-183">Az alábbiakban hivatkozások toohello hat minták hello témakör ismertet:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-183">Here are links toohello six samples discussed in hello topic:</span></span>

* <span data-ttu-id="b3e4e-184"><a href="#Scenario1">**A reflexió szerializálási** </a> -hello JSON-séma szerializált típusok toobe automatikusan beépített hello adatokból szerződés attribútumok.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-184"><a href="#Scenario1">**Serialization with reflection**</a> - hello JSON schema for types toobe serialized is automatically built from hello data contract attributes.</span></span>
* <span data-ttu-id="b3e4e-185"><a href="#Scenario2">**Általános rekordot tartalmazó szerializálási** </a> -hello JSON-séma explicit módon megadott rekord elérhető reflexióra nincs .NET típus esetén.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-185"><a href="#Scenario2">**Serialization with generic record**</a> - hello JSON schema is explicitly specified in a record when no .NET type is available for reflection.</span></span>
* <span data-ttu-id="b3e4e-186"><a href="#Scenario3">**Objektum tároló fájlok használata a reflexió szerializálási** </a> -hello JSON-séma automatikusan összeállítása és megosztott együtt hello szerializált adatok keresztül az Avro tároló fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-186"><a href="#Scenario3">**Serialization using object container files with reflection**</a> - hello JSON schema is automatically built and shared together with hello serialized data via an Avro object container file.</span></span>
* <span data-ttu-id="b3e4e-187"><a href="#Scenario4">**Általános rekordot tartalmazó objektum tárolófájlokba használatával szerializálási** </a> -hello JSON-séma explicit módon hello szerializálási előtt meg és megosztott együtt hello adatokat egy Avro tároló fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-187"><a href="#Scenario4">**Serialization using object container files with generic record**</a> - hello JSON schema is explicitly specified before hello serialization and shared together with hello data via an Avro object container file.</span></span>
* <span data-ttu-id="b3e4e-188"><a href="#Scenario5">**Objektum tárolófájlokba használata egy egyéni tömörítési kodek szerializálási** </a> -hello példa bemutatja, hogyan toocreate egy Avro objektum tároló fájl egy testreszabott .NET végrehajtásának hello Deflate adatok tömörítési kodek.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-188"><a href="#Scenario5">**Serialization using object container files with a custom compression codec**</a> - hello example shows how toocreate an Avro object container file with a customized .NET implementation of hello Deflate data compression codec.</span></span>
* <span data-ttu-id="b3e4e-189"><a href="#Scenario6">**Az Avro tooupload adatok használatával a Microsoft Azure HDInsight szolgáltatás hello** </a> -hello példa azt mutatja be, hogyan kommunikál az Avro szerializálási hello HDInsight-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-189"><a href="#Scenario6">**Using Avro tooupload data for hello Microsoft Azure HDInsight service**</a> - hello example illustrates how Avro serialization interacts with hello HDInsight service.</span></span> <span data-ttu-id="b3e4e-190">Egy aktív Azure előfizetés és a hozzáférés tooan Azure HDInsight fürt vannak szükséges toorun ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-190">An active Azure subscription and access tooan Azure HDInsight cluster are required toorun this example.</span></span>

## <span data-ttu-id="b3e4e-191"><a name="Scenario1"></a>1. példa: Szerializálási a reflexió</span><span class="sxs-lookup"><span data-stu-id="b3e4e-191"><a name="Scenario1"></a>Sample 1: Serialization with reflection</span></span>
<span data-ttu-id="b3e4e-192">hello JSON-séma hello típusok automatikusan épül fel Microsoft Avro Library hello hello adatokból reflexió útján hello C# objektumok toobe szerializált szerződés attribútumait.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-192">hello JSON schema for hello types can be automatically built by hello Microsoft Avro Library via reflection from hello data contract attributes of hello C# objects toobe serialized.</span></span> <span data-ttu-id="b3e4e-193">hello Microsoft Avro Library létrehoz egy [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello mezők toobe szerializálni.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-193">hello Microsoft Avro Library creates an [**IAvroSeralizer<T>**](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello fields toobe serialized.</span></span>

<span data-ttu-id="b3e4e-194">Ebben a példában objektumokat (egy **SensorData** tag osztályra **hely** struct) szerializált tooa memóriafolyam, és ez az adatfolyam pedig deszerializálva..</span><span class="sxs-lookup"><span data-stu-id="b3e4e-194">In this example, objects (a **SensorData** class with a member **Location** struct) are serialized tooa memory stream, and this stream is in turn deserialized.</span></span> <span data-ttu-id="b3e4e-195">hello eredménye akkor összehasonlított toohello kezdeti példány tooconfirm adott hello **SensorData** objektum helyre az eredeti azonos toohello.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-195">hello result is then compared toohello initial instance tooconfirm that hello **SensorData** object recovered is identical toohello original.</span></span>

<span data-ttu-id="b3e4e-196">hello séma ebben a példában feltételezzük, hogy megosztott hello olvasók és írók, között, így nincs szükség hello Avro objektum tárolóformátummal toobe.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-196">hello schema in this example is assumed toobe shared between hello readers and writers, so hello Avro object container format is not required.</span></span> <span data-ttu-id="b3e4e-197">A példa bemutatja, hogyan tooserialize deszerializálni az adatokat, és a memória pufferek használatával reflexiós hello objektum tároló formátumú hello séma meg kell osztani a hello adatokat, lásd: <a href="#Scenario3">szerializálási reflexiósobjektumtárolófájlokhasználata</a>.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-197">For an example of how tooserialize and deserialize data into memory buffers by using reflection with hello object container format when hello schema must be shared with hello data, see <a href="#Scenario3">Serialization using object container files with reflection</a>.</span></span>

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
        //hello usage of Microsoft Avro Library
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

                    //Serialize hello data toohello specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from hello stream and cast it toohello same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches hello original one
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

                //Serialization toomemory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-2-serialization-with-a-generic-record"></a><span data-ttu-id="b3e4e-198">2. példa: Szerializálási általános rekordot tartalmazó</span><span class="sxs-lookup"><span data-stu-id="b3e4e-198">Sample 2: Serialization with a generic record</span></span>
<span data-ttu-id="b3e4e-199">A JSON-séma adható explicit módon meg egy általános rekordban Ha reflexiós nem használható, mert hello adatok nem reprezentálhatók egy adategyezmény a .NET-osztályok keresztül.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-199">A JSON schema can be explicitly specified in a generic record when reflection cannot be used because hello data cannot be represented via .NET classes with a data contract.</span></span> <span data-ttu-id="b3e4e-200">Ez a módszer akkor lassabb, mint a következőt reflexió használatával.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-200">This method is slower than using reflection.</span></span> <span data-ttu-id="b3e4e-201">Ilyen esetekben hello séma hello adatok is lehet dinamikus, ez azt jelenti, hogy nem ismeri a fordítás során.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-201">In such cases, hello schema for hello data may also be dynamic, that is, not known at compile time.</span></span> <span data-ttu-id="b3e4e-202">Vesszővel tagolt (CSV) fájlt, amelynek séma ismeretlen amíg átalakítására kerül toohello az Avro formátum futás közben látható egy példa az ilyen dinamikus forgatókönyv ábrázolva adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-202">Data represented as comma-separated values (CSV) files whose schema is unknown until it is transformed toohello Avro format at run time is an example of this sort of dynamic scenario.</span></span>

<span data-ttu-id="b3e4e-203">A példa bemutatja, hogyan toocreate, és egy [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly adja meg a JSON-séma, hogyan toopopulate azt hello adatokat, és ezután hogyan tooserialize és deszerializálhatja azt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-203">This example shows how toocreate and use an [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly specify a JSON schema, how toopopulate it with hello data, and then how tooserialize and deserialize it.</span></span> <span data-ttu-id="b3e4e-204">hello eredmény majd össze lesz hasonlítva toohello a kezdeti példányszámnak tooconfirm, amely hello rekord helyre az eredeti azonos toohello.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-204">hello result is then compared toohello initial instance tooconfirm that hello record recovered is identical toohello original.</span></span>

<span data-ttu-id="b3e4e-205">hello séma ebben a példában feltételezzük, hogy megosztott hello olvasók és írók, között, így nincs szükség hello Avro objektum tárolóformátummal toobe.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-205">hello schema in this example is assumed toobe shared between hello readers and writers, so hello Avro object container format is not required.</span></span> <span data-ttu-id="b3e4e-206">A példa bemutatja, hogyan tooserialize deszerializálni az adatokat, és a memória pufferek használatával egy általános rekordban hello objektum tároló formátumú hello séma szerepelnie kell a szerializált hello adatokat, lásd: hello <a href="#Scenario4">szerializálási objektum tároló használatával általános rekordot tartalmazó fájlok</a> példa.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-206">For an example of how tooserialize and deserialize data into memory buffers by using a generic record with hello object container format when hello schema must be included with hello serialized data, see hello <a href="#Scenario4">Serialization using object container files with generic record</a> example.</span></span>

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
    //hello usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with hello schema explicitly defined in JSON.
        //All serialized data should be mapped toohello fields of hello generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining hello Schema and creating Sample Data Set...");

            //Define hello schema in JSON
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

            //Create a generic serializer based on hello schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record toorepresent hello data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize hello data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize hello data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify hello results
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

            //Serialization toomemory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key tooexit.");
            Console.Read();
        }
    }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining hello Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a><span data-ttu-id="b3e4e-207">3. példa: Szerializálási a reflexió objektum tároló fájlok és a szerializálási használja</span><span class="sxs-lookup"><span data-stu-id="b3e4e-207">Sample 3: Serialization using object container files and serialization with reflection</span></span>
<span data-ttu-id="b3e4e-208">Ebben a példában a hasonló toohello lehetőséget a hello <a href="#Scenario1"> első példában</a>, ahol hello séma implicit módon megadott a licencjelentésben.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-208">This example is similar toohello scenario in hello <a href="#Scenario1"> first example</a>, where hello schema is implicitly specified with reflection.</span></span> <span data-ttu-id="b3e4e-209">hello különbség az, hogy itt hello séma nem feltételezi azt deserializes toohello olvasó ismert toobe.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-209">hello difference is that here, hello schema is not assumed toobe known toohello reader that deserializes it.</span></span> <span data-ttu-id="b3e4e-210">Hello **SensorData** szerializált objektumok toobe és implicit módon megadott sémák fájlban vannak tárolva az Avro objektum tároló hello által képviselt [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) az osztály.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-210">hello **SensorData** objects toobe serialized and their implicitly specified schema are stored in an Avro object container file represented by hello [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span>

<span data-ttu-id="b3e4e-211">Ebben a példában a szerializált adatok hello [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) és a deszerializált [ **SequentialReader<SensorData>** ](http://msdn.microsoft.com/library/dn627340.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3e4e-211">hello data is serialized in this example with [**SequentialWriter<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx) and deserialized with [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx).</span></span> <span data-ttu-id="b3e4e-212">hello majd eredménye összehasonlított toohello kezdeti példányok tooensure identitás.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-212">hello result then is compared toohello initial instances tooensure identity.</span></span>

<span data-ttu-id="b3e4e-213">hello a hello adatobjektumot tároló fájl tömörített keresztül hello alapértelmezett [ **Deflate** ] [ deflate-100] tömörítési kodek a .NET-keretrendszer 4.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-213">hello data in hello object container file is compressed via hello default [**Deflate**][deflate-100] compression codec from .NET Framework 4.</span></span> <span data-ttu-id="b3e4e-214">Lásd: hello <a href="#Scenario5"> ötödik példa</a> az ebben a témakörben toolearn hogyan toouse hello újabb és felső szintű verziója [ **Deflate** ] [ deflate-110] tömörítés a kodek .NET-keretrendszer 4.5 érhető el.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-214">See hello <a href="#Scenario5"> fifth example</a> in this topic toolearn how toouse a more recent and superior version of hello [**Deflate**][deflate-110] compression codec available in .NET Framework 4.5.</span></span>

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
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes hello sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with hello Deflate codec.
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

                //Serializing and saving data toofile.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Data is compressed using hello Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize hello data toostream by using hello sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream toofile
                    Console.WriteLine("Saving serialized data toofile...");
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

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches hello original one
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

                //Delete hello file
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

            //Saving memory stream tooa new file with hello given path
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
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
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

            //Reading a file content by using hello given path tooa memory stream
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
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
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
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
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

                //Serialization using reflection tooAvro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a><span data-ttu-id="b3e4e-215">4. példa: Szerializálási általános rekordot tartalmazó objektum tárolófájlokba és szerializálási használatával</span><span class="sxs-lookup"><span data-stu-id="b3e4e-215">Sample 4: Serialization using object container files and serialization with generic record</span></span>
<span data-ttu-id="b3e4e-216">Ebben a példában a hasonló toohello lehetőséget a hello <a href="#Scenario2"> második példáját</a>, ahol hello séma explicit módon megadott rendelkező JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-216">This example is similar toohello scenario in hello <a href="#Scenario2"> second example</a>, where hello schema is explicitly specified with JSON.</span></span> <span data-ttu-id="b3e4e-217">hello különbség az, hogy itt hello séma nem feltételezi azt deserializes toohello olvasó ismert toobe.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-217">hello difference is that here, hello schema is not assumed toobe known toohello reader that deserializes it.</span></span>

<span data-ttu-id="b3e4e-218">hello TesztKészlet adatokat gyűjt egy [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) explicit módon megadott JSON-séma segítségével objektumokat, és ott hello által képviselt objektum tároló fájlban [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-218">hello test data set is collected into a list of [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objects via an explicitly defined JSON schema and then stored in an object container file represented by hello [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span> <span data-ttu-id="b3e4e-219">A tároló fájlt hoz létre egy író használt tooserialize hello adatok tömörítetlen, majd mentett tooa fájl tooa memóriafolyam.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-219">This container file creates a writer that is used tooserialize hello data, uncompressed, tooa memory stream that is then saved tooa file.</span></span> <span data-ttu-id="b3e4e-220">Hello [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) hello-olvasó létrehozásakor használt paraméter határozza meg, hogy ezek az adatok nem tömöríti.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-220">hello [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter used for creating hello reader specifies that this data is not compressed.</span></span>

<span data-ttu-id="b3e4e-221">hello adatokat, majd hello fájl olvasásának és deszerializálni az objektumok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-221">hello data is then read from hello file and deserialized into a collection of objects.</span></span> <span data-ttu-id="b3e4e-222">Ez a gyűjtemény rendszer összehasonlított toohello kezdeti Avro rekordok tooconfirm azok egyezését.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-222">This collection is compared toohello initial list of Avro records tooconfirm that they are identical.</span></span>

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
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining hello Schema and creating Sample Data Set...");

                //Define hello schema in JSON
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

                //Create a generic serializer based on hello schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record toorepresent hello data
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

                //Serializing and saving data toofile.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize hello data toostream by using hello sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data toofile...");

                    //Save stream toofile
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing hello data.
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

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify hello results
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

                //Delete hello file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream tooa new file with hello given path
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
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
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

            //Reading a file content by using hello given path tooa memory stream
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
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using hello given path
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
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
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

                //Create an instance of hello AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record tooAvro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining hello Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a><span data-ttu-id="b3e4e-223">5. példa: Szerializálási egy egyéni tömörítési kodek objektum tároló fájlok használata</span><span class="sxs-lookup"><span data-stu-id="b3e4e-223">Sample 5: Serialization using object container files with a custom compression codec</span></span>
<span data-ttu-id="b3e4e-224">ötödik példa azt mutatja meg hello hogyan toouse az avro-hoz egy egyéni tömörítési kodek objektum tárolófájlokba.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-224">hello fifth example shows how toouse a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="b3e4e-225">Egy minta hello kódot tartalmazó, az ebben a példában tölthető le: hello [Azure mintakódok](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) hely.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-225">A sample containing hello code for this example can be downloaded from hello [Azure code samples](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.</span></span>

<span data-ttu-id="b3e4e-226">Hello [Avro specifikációjában](http://avro.apache.org/docs/current/spec.html#Required+Codecs) lehetővé teszi, hogy egy nem kötelező tömörítési kodek használatát (továbbá túl**Null** és **Deflate** alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="b3e4e-226">hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) allows usage of an optional compression codec (in addition too**Null** and **Deflate** defaults).</span></span> <span data-ttu-id="b3e4e-227">Ebben a példában nem implementálja az Snappy például egy új kodek (egy támogatott választható kodek hello az említett [Avro specifikációjában](http://avro.apache.org/docs/current/spec.html#snappy)).</span><span class="sxs-lookup"><span data-stu-id="b3e4e-227">This example is not implementing a new codec such as Snappy (mentioned as a supported optional codec in hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span></span> <span data-ttu-id="b3e4e-228">Azt illusztrálja, hogyan toouse hello hello .NET-keretrendszer 4.5 végrehajtásának [ **Deflate** ] [ deflate-110] kodek, amely hello alapjánjobbalgoritmustbiztosít[zlib](http://zlib.net/) hello alapértelmezett .NET-keretrendszer 4 verziónál tömörítési könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-228">It shows how toouse hello .NET Framework 4.5 implementation of hello [**Deflate**][deflate-110] codec, which provides a better compression algorithm based on hello [zlib](http://zlib.net/) compression library than hello default .NET Framework 4 version.</span></span>

    //
    // This code needs toobe compiled with hello parameter Target Framework set as ".NET Framework 4.5"
    // tooensure hello desired implementation of hello Deflate compression algorithm is used.
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
        //which are hello key ones for data compression.
        //tooenable a custom codec, one needs tooimplement these methods for hello required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs tooimplement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed toobe null.");

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
        //Define hello actual codec class containing hello required methods for manipulating streams:
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
        //Define modified codec factory toobe used in hello reader.
        //It catches hello attempt toouse "Deflate" and provide  a custom codec.
        //For all other cases, it relies on hello base class (CodecFactory).
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
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with hello custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related tooserialization, deserialization and
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

                //Serializing and saving data toofile.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Here hello custom codec is introduced. For convenience, hello next commented code line shows how toouse built-in Deflate.
                    //Note that because hello sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize hello data toostream using hello sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream toofile
                    Console.WriteLine("Saving serialized data toofile...");
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

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment hello line below if you want toouse built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    //Here hello custom codec factory is introduced.
                    //For convenience, hello next commented code line shows how toouse built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because hello sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches hello original one
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

                //Delete hello file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream tooa new file with hello given path
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
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
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

            //Reading file content by using hello given path tooa memory stream
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
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
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
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
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

                //Serialization using reflection tooAvro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.

## <a name="sample-6-using-avro-tooupload-data-for-hello-microsoft-azure-hdinsight-service"></a><span data-ttu-id="b3e4e-229">6. példa: Avro tooupload adatok használata a Microsoft Azure HDInsight szolgáltatás hello</span><span class="sxs-lookup"><span data-stu-id="b3e4e-229">Sample 6: Using Avro tooupload data for hello Microsoft Azure HDInsight service</span></span>
<span data-ttu-id="b3e4e-230">hello hatodik példa bemutatja az egyes programozási módszerek kapcsolódó toointeracting hello Azure HDInsight szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-230">hello sixth example illustrates some programming techniques related toointeracting with hello Azure HDInsight service.</span></span> <span data-ttu-id="b3e4e-231">Egy minta hello kódot tartalmazó, az ebben a példában tölthető le: hello [Azure mintakódok](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) hely.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-231">A sample containing hello code for this example can be downloaded from hello [Azure code samples](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.</span></span>

<span data-ttu-id="b3e4e-232">hello minta a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-232">hello sample does hello following tasks:</span></span>

* <span data-ttu-id="b3e4e-233">Tooan meglévő HDInsight-szolgáltatás fürthöz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-233">Connects tooan existing HDInsight service cluster.</span></span>
* <span data-ttu-id="b3e4e-234">Rendezi sorba több CSV-fájlt, és feltölti a hello eredmény tooAzure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-234">Serializes several CSV files and uploads hello result tooAzure Blob storage.</span></span> <span data-ttu-id="b3e4e-235">(hello CSV-fájlok együtt hello minta küld, és megfelelnek-e terjesztve mellékletben készlet előzményadatokat kivonatát [Infochimps](http://www.infochimps.com/) 1970-2010 hello időszakra.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-235">(hello CSV files are distributed together with hello sample and represent an extract from AMEX Stock historical data distributed by [Infochimps](http://www.infochimps.com/) for hello period 1970-2010.</span></span> <span data-ttu-id="b3e4e-236">hello minta CSV-fájl adatokat olvas, konvertálja a hello rekordok tooinstances hello **készlet** osztály, és ezután rendezi sorba őket reflexió használatával.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-236">hello sample reads CSV file data, converts hello records tooinstances of hello **Stock** class, and then serializes them by using reflection.</span></span> <span data-ttu-id="b3e4e-237">Készlet definíciójának egy JSON-séma hello Microsoft Avro Library kód generálása segédprogram használatával készült.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-237">Stock type definition is created from a JSON schema via hello Microsoft Avro Library code generation utility.</span></span>
* <span data-ttu-id="b3e4e-238">Táblázatot hoz létre új külső nevű **készletek** a Hive, és csatolja azt toohello adatok hello előző lépésben feltöltve.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-238">Creates a new external table called **Stocks** in Hive, and links it toohello data uploaded in hello previous step.</span></span>
* <span data-ttu-id="b3e4e-239">A lekérdezés végrehajtja a Hive használatával hello keresztül **készletek** tábla.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-239">Executes a query by using Hive over hello **Stocks** table.</span></span>

<span data-ttu-id="b3e4e-240">Ezenkívül hello minta folyamatot hajt végre a karbantartás előtt és után fő műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-240">In addition, hello sample performs a clean-up procedure before and after performing major operations.</span></span> <span data-ttu-id="b3e4e-241">Során hello karbantartás minden hello kapcsolódó Azure-Blob adatok és mappákat eltávolítsa, és hello Hive tábla megszakad.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-241">During hello clean-up, all of hello related Azure Blob data and folders are removed, and hello Hive table is dropped.</span></span> <span data-ttu-id="b3e4e-242">Hello karbantartás eljárás hello minta parancssorból is hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-242">You can also invoke hello clean-up procedure from hello sample command line.</span></span>

<span data-ttu-id="b3e4e-243">hello minta a következő előfeltételek hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-243">hello sample has hello following prerequisites:</span></span>

* <span data-ttu-id="b3e4e-244">Egy aktív Microsoft Azure-előfizetést és az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-244">An active Microsoft Azure subscription and its subscription ID.</span></span>
* <span data-ttu-id="b3e4e-245">Felügyeleti tanúsítvány hello előfizetés hello megfelelő titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-245">A management certificate for hello subscription with hello corresponding private key.</span></span> <span data-ttu-id="b3e4e-246">hello aktuális felhasználói személyes tárolót a hello használt géppel toorun hello minta hello tanúsítványt kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-246">hello certificate should be installed in hello current user private storage on hello machine used toorun hello sample.</span></span>
* <span data-ttu-id="b3e4e-247">Aktív HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-247">An active HDInsight cluster.</span></span>
* <span data-ttu-id="b3e4e-248">Egy Azure Storage-fiók kapcsolódó toohello HDInsight fürt hello előző előfeltétel együtt hello megfelelő elsődleges vagy másodlagos elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-248">An Azure Storage account linked toohello HDInsight cluster from hello previous prerequisite, together with hello corresponding primary, or secondary access key.</span></span>

<span data-ttu-id="b3e4e-249">Minden hello Előfeltételek hello információt kell megadott toohello minta konfigurációs fájlt, hello minta futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-249">All of hello information from hello prerequisites should be entered toohello sample configuration file before hello sample is run.</span></span> <span data-ttu-id="b3e4e-250">Nincsenek a két lehetséges módjait toodo azt:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-250">There are two possible ways toodo it:</span></span>

* <span data-ttu-id="b3e4e-251">Hello minta gyökérkönyvtár hello app.config fájl szerkesztése, és majd kialakítható hello minta</span><span class="sxs-lookup"><span data-stu-id="b3e4e-251">Edit hello app.config file in hello sample root directory and then build hello sample</span></span>
* <span data-ttu-id="b3e4e-252">Először a hello minta létrehozása, és szerkessze a AvroHDISample.exe.config hello build könyvtárban</span><span class="sxs-lookup"><span data-stu-id="b3e4e-252">First build hello sample, and then edit AvroHDISample.exe.config in hello build directory</span></span>

<span data-ttu-id="b3e4e-253">Mindkét esetben minden módosításokat kell végezni a hello  **<appSettings>**  beállítások szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-253">In both cases, all edits should be done in hello **<appSettings>** settings section.</span></span> <span data-ttu-id="b3e4e-254">Hajtsa végre a hello megjegyzések hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="b3e4e-254">Follow hello comments in hello file.</span></span>
<span data-ttu-id="b3e4e-255">hello minta fut hello parancssorból futtassa a következő parancsot (amelyben hello hello mintát tartalmazó .zip fájl volt feltételezve, hogy a kibontott toobe tooC:\AvroHDISample; Ha vonatkozó hello-e ellenkező esetben használja fájl elérési útja) hello:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-255">hello sample is run from hello command line by executing hello following command (where hello .zip file with hello sample was assumed toobe extracted tooC:\AvroHDISample; if otherwise, use hello relevant file path):</span></span>

    AvroHDISample run C:\AvroHDISample\Data

<span data-ttu-id="b3e4e-256">hello fürt, futtassa a következő parancs hello tooclean:</span><span class="sxs-lookup"><span data-stu-id="b3e4e-256">tooclean up hello cluster, run hello following command:</span></span>

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
