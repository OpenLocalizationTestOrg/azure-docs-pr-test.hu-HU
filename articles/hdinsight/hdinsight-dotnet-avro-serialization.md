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
# <a name="serialize-data-in-hadoop-with-hello-microsoft-avro-library"></a>Hello Microsoft Avro Library a Hadoop adatok szerializálása

>[!NOTE]
>a Microsoft hello Avro SDK már nem támogatott. hello könyvtár támogatott nyílt forráskódú közösségi. hello szalagtár hello források találhatók [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

Ez a témakör bemutatja, hogyan toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objektumok és egyéb adatok szerkezet adatfolyamok toopersist be őket toomemory, adatbázis vagy egy fájlt. Azt is bemutatja, hogyan toodeserialize toorecover hello eredeti objektumok őket.

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
Hello <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> valósít meg hello Apache Avro adatok szerializálási rendszer hello Microsoft.NET környezethez. Apache Avro kompakt bináris adatcsere adatformátum szerializálási biztosít. Használja <a href="http://www.json.org" target="_blank">JSON</a> toodefine egy nyelvtől független sémát, amely lehetővé teszi a nyelvi együttműködést. Több nyelven érhetőek szerializált adatok olvashatók egy másik. Jelenleg a C, C++, C#, Java, PHP, Python és Ruby támogatottak. Hello részletes információk találhatók hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro specifikációjában</a>. 

>[!NOTE]
>az Avro Library Microsoft hello nem támogatja a hello távoli eljáráshívásnak (RPC) hívások részét ezt a beállítást.
>

az objektumok hello Avro rendszer szerializált hello ábrázolását két részből áll: séma, a tényleges érték. az Avro-séma hello hello nyelvfüggetlen adatmodell az hello szerializált adatok JSON ismerteti. A bináris adatok megjelenítése és jelölőnégyzetként jelenik. Minden objektum toobe készült nincs érték terhek, így szerializálási gyors és hello ábrázolását kis hello séma elkülönül a bináris megjelenítése hello rendelkező lehetővé teszi.

## <a name="hello-hadoop-scenario"></a>hello Hadoop forgatókönyv
hello Apache Avro szerializálási formátum széles körben használt Azure HDInsight és egyéb Apache Hadoop-környezetekben. Az Avro biztosít egy kényelmes módszert arra toorepresent összetett adatstruktúra Hadoop MapReduce feladatot. az Avro-fájlok (Avro tároló fájlt) hello formátuma lett tervezett toosupport hello elosztott MapReduce programozási modellt. hello kulcs szolgáltatása, amely lehetővé teszi, hogy a hello terjesztési, az, hogy hello fájlok "feloszthatók" hello értelemben, hogy egy fájl bármely pontot kikereshet, és az egy adott blokktól kezdheti-e.

## <a name="serialization-in-avro-library"></a>Az Avro könyvtárban szerializálási
hello .NET könyvtár az Avro támogatja a szerializálási objektumok két módon:

* **Fejlécreflexiós** -hello JSON-séma hello típusok automatikusan beépített hello adatokból hello .NET típusok toobe szerializált szerződés attribútumait.
* **általános rekord** -A JSON-séma explicit módon megadott hello által képviselt rekordban [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) indulásakor nem .NET típusok a következők hello adatok toobe jelen toodescribe hello séma szerializálni.

Ha hello adatkulcsokat ismert tooboth hello író és ahhoz való olvasóra hello adatfolyam, hello adatküldés sémájú nélkül. Azokban az esetekben az Avro objektum tároló fájllal, hello séma tárolja hello fájlban. Más paraméterek, például az adatok tömörítésének használt hello kodek adható meg. Ezek a forgatókönyvek részletesen ismertetett és mutatja be a következő példák hello:

## <a name="install-avro-library"></a>Telepítse az Avro-könyvtár
hello következők szükségesek, hello könyvtár telepítése előtt:

* <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">A Microsoft .NET-keretrendszer 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 vagy újabb)

Vegye figyelembe, hogy hello Newtonsoft.Json.dll függőségi automatikusan letöltött hello Microsoft Avro Library hello összetevőnek. hello eljárás hello a következő szakaszban találhatók:

az Avro Library Microsoft hello eljárást követő hello keresztül telepíthető a Visual Studio NuGet-csomag terjesztése:

1. Jelölje be hello **projekt** lapon -> **NuGet-csomagok kezelése...**
2. Keresse meg a "Microsoft.Hadoop.Avro" hello a **keresési Online** mezőbe.
3. Kattintson a hello **telepítése** gomb melletti túl**Microsoft Azure HDInsight Avro könyvtár**.

Vegye figyelembe, hogy hello Newtonsoft.Json.dll (> = 6.0.4) függőségi hello Microsoft Avro Library együtt is letöltődik automatikusan.

Microsoft Avro Library forráskód hello érhető el: [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

## <a name="compile-schemas-using-avro-library"></a>Az Avro szalagtárat használó sémák fordítása
az Avro Library Microsoft hello egy segédprogram, amely lehetővé teszi, hogy létrehozása a C# típusok hello alapján automatikusan korábban megadott JSON-séma kódgenerálás tartalmazza. hello kód generálása segédprogram nem winphonesspbootstrapper.exe végrehajtható bináris, de könnyen építhetők eljárást követő hello keresztül:

1. A HDInsight SDK forráskódjának hello legújabb verziójával hello .zip fájl letöltése <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK a Hadoop</a>. (Kattintson a hello **letöltése** ikonra, nem hello **letölti** lapon.)
2. Bontsa ki a HDInsight SDK tooa directory hello gépen a .NET-keretrendszer 4 telepítve, és a szükséges függőség NuGet-csomagok letöltése a toohello Internet kapcsolat hello. Az alábbiakban azt feltételezik, hogy hello forráskód kibontott tooC:\SDK.
3. Nyissa meg toohello mappa C:\SDK\src\Microsoft.Hadoop.Avro.Tools, és futtassa a build.bat. (hello fájl hívásait MSBuild hello 32 bites terjesztése hello .NET-keretrendszer. Ha szeretné toouse hello 64 bites verziója, szerkesztése build.bat, a következő hello megjegyzések hello fájl.) Győződjön meg arról, hogy hello build sikeres. (Egyes rendszerek MSBuild készíthet figyelmeztetéseket. Ezek a figyelmeztetések nem befolyásolják hello segédprogram mindaddig, amíg nincsenek build hibák.)
4. lefordított hello segédprogram C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools található.

hello parancssori szintaxist, ismernie tooget parancs követően – hello mappáját hello kód generálása segédprogram hello hajtható végre:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

tootest hello segédprogram, a C# osztályok hozhat létre a hello minta JSON sémafájl hello forráskód biztosított. A következő parancs hello hajtható végre:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Hello aktuális könyvtárban található a várt tooproduce két C#-fájlok: SensorData.cs és Location.cs.

toounderstand hello logikát használó hello kód generálása segédprogram konvertálásakor tooC # hello JSON sématípusok, tekintse meg a GenerationVerification.feature található C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc hello fájlt.

Névterek kinyert hello JSON-séma hello előző bekezdésben szereplő esetekben hello fájlban leírt hello logika használatával. Hello séma kinyert névterek élveznek függetlenül biztosított hello n paraméterrel hello segédprogram parancssorban. Ha azt szeretné, hogy toooverride hello névterek hello séma belül található, használja a hello /nf paramétert. Például toochange hello SampleJSONSchema.avsc toomy.own.nspace, az összes névtér hajtható végre hello a következő parancsot:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-hello-samples"></a>Kapcsolatos hello minták
Ebben a témakörben bemutatott hat példák bemutatják a Microsoft Avro Library hello által támogatott különböző forgatókönyveket. az Avro Library Microsoft hello az adatfolyam-tervezett toowork. Ezekben a példákban-adatok n keresztül memória adatfolyamok helyett, fájl adatfolyamok vagy az egyszerűség és konzisztenciáját. éles környezetben hello megközelítést hello pontos forgatókönyv-követelményeinek, az adatforrás és a kötet, a teljesítmény korlátozások és a más tényezőktől függ.

Hogyan hello első két példa megjelenítése tooserialize és adatok deszerializálása memória adatfolyam pufferek az általános és a reflexió használatával. hello két esetben a séma feltételezett hello olvasók és írók között megosztott toobe sávon kívüli.

hello harmadik és negyedik példák megjelenítése hogyan tooserialize és adatok deszerializálása hello Avro objektum tároló fájlok használatával. Adatok az Avro-tároló fájl tárolja, amikor a séma mindig tárolása vele, mert hello séma meg kell osztani a deszerializáláshoz.

hello mintát tartalmazó hello első négy példák tölthető le: hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure mintakódok</a> hely.

ötödik példa azt mutatja meg hello hogyan toouse az avro-hoz egy egyéni tömörítési kodek objektum tárolófájlokba. Egy minta hello kódot tartalmazó, az ebben a példában tölthető le: hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure mintakódok</a> hely.

hello hatodik példa bemutatja, hogyan toouse Avro szerializálási tooupload adatok tooAzure Blob-tároló, és majd elemezni egy HDInsight (Hadoop) fürthöz való Hive használatával. Le is tölthetők: hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure mintakódok</a> hely.

Az alábbiakban hivatkozások toohello hat minták hello témakör ismertet:

* <a href="#Scenario1">**A reflexió szerializálási** </a> -hello JSON-séma szerializált típusok toobe automatikusan beépített hello adatokból szerződés attribútumok.
* <a href="#Scenario2">**Általános rekordot tartalmazó szerializálási** </a> -hello JSON-séma explicit módon megadott rekord elérhető reflexióra nincs .NET típus esetén.
* <a href="#Scenario3">**Objektum tároló fájlok használata a reflexió szerializálási** </a> -hello JSON-séma automatikusan összeállítása és megosztott együtt hello szerializált adatok keresztül az Avro tároló fájlt.
* <a href="#Scenario4">**Általános rekordot tartalmazó objektum tárolófájlokba használatával szerializálási** </a> -hello JSON-séma explicit módon hello szerializálási előtt meg és megosztott együtt hello adatokat egy Avro tároló fájlt.
* <a href="#Scenario5">**Objektum tárolófájlokba használata egy egyéni tömörítési kodek szerializálási** </a> -hello példa bemutatja, hogyan toocreate egy Avro objektum tároló fájl egy testreszabott .NET végrehajtásának hello Deflate adatok tömörítési kodek.
* <a href="#Scenario6">**Az Avro tooupload adatok használatával a Microsoft Azure HDInsight szolgáltatás hello** </a> -hello példa azt mutatja be, hogyan kommunikál az Avro szerializálási hello HDInsight-szolgáltatás. Egy aktív Azure előfizetés és a hozzáférés tooan Azure HDInsight fürt vannak szükséges toorun ebben a példában.

## <a name="Scenario1"></a>1. példa: Szerializálási a reflexió
hello JSON-séma hello típusok automatikusan épül fel Microsoft Avro Library hello hello adatokból reflexió útján hello C# objektumok toobe szerializált szerződés attribútumait. hello Microsoft Avro Library létrehoz egy [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello mezők toobe szerializálni.

Ebben a példában objektumokat (egy **SensorData** tag osztályra **hely** struct) szerializált tooa memóriafolyam, és ez az adatfolyam pedig deszerializálva.. hello eredménye akkor összehasonlított toohello kezdeti példány tooconfirm adott hello **SensorData** objektum helyre az eredeti azonos toohello.

hello séma ebben a példában feltételezzük, hogy megosztott hello olvasók és írók, között, így nincs szükség hello Avro objektum tárolóformátummal toobe. A példa bemutatja, hogyan tooserialize deszerializálni az adatokat, és a memória pufferek használatával reflexiós hello objektum tároló formátumú hello séma meg kell osztani a hello adatokat, lásd: <a href="#Scenario3">szerializálási reflexiósobjektumtárolófájlokhasználata</a>.

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


## <a name="sample-2-serialization-with-a-generic-record"></a>2. példa: Szerializálási általános rekordot tartalmazó
A JSON-séma adható explicit módon meg egy általános rekordban Ha reflexiós nem használható, mert hello adatok nem reprezentálhatók egy adategyezmény a .NET-osztályok keresztül. Ez a módszer akkor lassabb, mint a következőt reflexió használatával. Ilyen esetekben hello séma hello adatok is lehet dinamikus, ez azt jelenti, hogy nem ismeri a fordítás során. Vesszővel tagolt (CSV) fájlt, amelynek séma ismeretlen amíg átalakítására kerül toohello az Avro formátum futás közben látható egy példa az ilyen dinamikus forgatókönyv ábrázolva adatokat.

A példa bemutatja, hogyan toocreate, és egy [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly adja meg a JSON-séma, hogyan toopopulate azt hello adatokat, és ezután hogyan tooserialize és deszerializálhatja azt. hello eredmény majd össze lesz hasonlítva toohello a kezdeti példányszámnak tooconfirm, amely hello rekord helyre az eredeti azonos toohello.

hello séma ebben a példában feltételezzük, hogy megosztott hello olvasók és írók, között, így nincs szükség hello Avro objektum tárolóformátummal toobe. A példa bemutatja, hogyan tooserialize deszerializálni az adatokat, és a memória pufferek használatával egy általános rekordban hello objektum tároló formátumú hello séma szerepelnie kell a szerializált hello adatokat, lásd: hello <a href="#Scenario4">szerializálási objektum tároló használatával általános rekordot tartalmazó fájlok</a> példa.

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


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>3. példa: Szerializálási a reflexió objektum tároló fájlok és a szerializálási használja
Ebben a példában a hasonló toohello lehetőséget a hello <a href="#Scenario1"> első példában</a>, ahol hello séma implicit módon megadott a licencjelentésben. hello különbség az, hogy itt hello séma nem feltételezi azt deserializes toohello olvasó ismert toobe. Hello **SensorData** szerializált objektumok toobe és implicit módon megadott sémák fájlban vannak tárolva az Avro objektum tároló hello által képviselt [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) az osztály.

Ebben a példában a szerializált adatok hello [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) és a deszerializált [ **SequentialReader<SensorData>** ](http://msdn.microsoft.com/library/dn627340.aspx). hello majd eredménye összehasonlított toohello kezdeti példányok tooensure identitás.

hello a hello adatobjektumot tároló fájl tömörített keresztül hello alapértelmezett [ **Deflate** ] [ deflate-100] tömörítési kodek a .NET-keretrendszer 4. Lásd: hello <a href="#Scenario5"> ötödik példa</a> az ebben a témakörben toolearn hogyan toouse hello újabb és felső szintű verziója [ **Deflate** ] [ deflate-110] tömörítés a kodek .NET-keretrendszer 4.5 érhető el.

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


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>4. példa: Szerializálási általános rekordot tartalmazó objektum tárolófájlokba és szerializálási használatával
Ebben a példában a hasonló toohello lehetőséget a hello <a href="#Scenario2"> második példáját</a>, ahol hello séma explicit módon megadott rendelkező JSON-NÁ. hello különbség az, hogy itt hello séma nem feltételezi azt deserializes toohello olvasó ismert toobe.

hello TesztKészlet adatokat gyűjt egy [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) explicit módon megadott JSON-séma segítségével objektumokat, és ott hello által képviselt objektum tároló fájlban [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) osztály. A tároló fájlt hoz létre egy író használt tooserialize hello adatok tömörítetlen, majd mentett tooa fájl tooa memóriafolyam. Hello [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) hello-olvasó létrehozásakor használt paraméter határozza meg, hogy ezek az adatok nem tömöríti.

hello adatokat, majd hello fájl olvasásának és deszerializálni az objektumok gyűjteménye. Ez a gyűjtemény rendszer összehasonlított toohello kezdeti Avro rekordok tooconfirm azok egyezését.

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




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>5. példa: Szerializálási egy egyéni tömörítési kodek objektum tároló fájlok használata
ötödik példa azt mutatja meg hello hogyan toouse az avro-hoz egy egyéni tömörítési kodek objektum tárolófájlokba. Egy minta hello kódot tartalmazó, az ebben a példában tölthető le: hello [Azure mintakódok](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) hely.

Hello [Avro specifikációjában](http://avro.apache.org/docs/current/spec.html#Required+Codecs) lehetővé teszi, hogy egy nem kötelező tömörítési kodek használatát (továbbá túl**Null** és **Deflate** alapértelmezett). Ebben a példában nem implementálja az Snappy például egy új kodek (egy támogatott választható kodek hello az említett [Avro specifikációjában](http://avro.apache.org/docs/current/spec.html#snappy)). Azt illusztrálja, hogyan toouse hello hello .NET-keretrendszer 4.5 végrehajtásának [ **Deflate** ] [ deflate-110] kodek, amely hello alapjánjobbalgoritmustbiztosít[zlib](http://zlib.net/) hello alapértelmezett .NET-keretrendszer 4 verziónál tömörítési könyvtárat.

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

## <a name="sample-6-using-avro-tooupload-data-for-hello-microsoft-azure-hdinsight-service"></a>6. példa: Avro tooupload adatok használata a Microsoft Azure HDInsight szolgáltatás hello
hello hatodik példa bemutatja az egyes programozási módszerek kapcsolódó toointeracting hello Azure HDInsight szolgáltatással. Egy minta hello kódot tartalmazó, az ebben a példában tölthető le: hello [Azure mintakódok](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) hely.

hello minta a következő feladatok hello:

* Tooan meglévő HDInsight-szolgáltatás fürthöz csatlakozik.
* Rendezi sorba több CSV-fájlt, és feltölti a hello eredmény tooAzure Blob Storage tárolóban. (hello CSV-fájlok együtt hello minta küld, és megfelelnek-e terjesztve mellékletben készlet előzményadatokat kivonatát [Infochimps](http://www.infochimps.com/) 1970-2010 hello időszakra. hello minta CSV-fájl adatokat olvas, konvertálja a hello rekordok tooinstances hello **készlet** osztály, és ezután rendezi sorba őket reflexió használatával. Készlet definíciójának egy JSON-séma hello Microsoft Avro Library kód generálása segédprogram használatával készült.
* Táblázatot hoz létre új külső nevű **készletek** a Hive, és csatolja azt toohello adatok hello előző lépésben feltöltve.
* A lekérdezés végrehajtja a Hive használatával hello keresztül **készletek** tábla.

Ezenkívül hello minta folyamatot hajt végre a karbantartás előtt és után fő műveletet hajt végre. Során hello karbantartás minden hello kapcsolódó Azure-Blob adatok és mappákat eltávolítsa, és hello Hive tábla megszakad. Hello karbantartás eljárás hello minta parancssorból is hívhat meg.

hello minta a következő előfeltételek hello rendelkezik:

* Egy aktív Microsoft Azure-előfizetést és az előfizetés-azonosító.
* Felügyeleti tanúsítvány hello előfizetés hello megfelelő titkos kulccsal. hello aktuális felhasználói személyes tárolót a hello használt géppel toorun hello minta hello tanúsítványt kell telepíteni.
* Aktív HDInsight-fürtöt.
* Egy Azure Storage-fiók kapcsolódó toohello HDInsight fürt hello előző előfeltétel együtt hello megfelelő elsődleges vagy másodlagos elérési kulcsot.

Minden hello Előfeltételek hello információt kell megadott toohello minta konfigurációs fájlt, hello minta futtatása előtt. Nincsenek a két lehetséges módjait toodo azt:

* Hello minta gyökérkönyvtár hello app.config fájl szerkesztése, és majd kialakítható hello minta
* Először a hello minta létrehozása, és szerkessze a AvroHDISample.exe.config hello build könyvtárban

Mindkét esetben minden módosításokat kell végezni a hello  **<appSettings>**  beállítások szakaszban. Hajtsa végre a hello megjegyzések hello fájlban.
hello minta fut hello parancssorból futtassa a következő parancsot (amelyben hello hello mintát tartalmazó .zip fájl volt feltételezve, hogy a kibontott toobe tooC:\AvroHDISample; Ha vonatkozó hello-e ellenkező esetben használja fájl elérési útja) hello:

    AvroHDISample run C:\AvroHDISample\Data

hello fürt, futtassa a következő parancs hello tooclean:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
