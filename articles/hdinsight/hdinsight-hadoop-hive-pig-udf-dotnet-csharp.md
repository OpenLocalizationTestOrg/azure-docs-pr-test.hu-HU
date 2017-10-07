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
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a>C# felhasználó által definiált függvények használata a Hive és a HDInsight hadoop streamelési Pig használatával

Ismerje meg, hogyan toouse C# felhasználói függvény (UDF) Apache Hive és a Pig a hdinsight platformon.

> [!IMPORTANT]
> hello ebben a dokumentumban a lépések a Linux-alapú, és a Windows-alapú HDInsight-fürtökkel. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).

Mind a Hive és a Pig teljen tooexternal alkalmazások feldolgozásához. Ezt a folyamatot nevezik _streaming_. A .NET hatóságuknál használatakor hello adatok toohello alkalmazás átadódik STDIN, és hello alkalmazás STDOUT-on hello eredményeket ad vissza. tooread és STDIN és a STDOUT írási, használhat `Console.ReadLine()` és `Console.WriteLine()` konzol alkalmazás.

## <a name="prerequisites"></a>Előfeltételek

* Egy írást, és C#-kódban, amelynek célpontja a .NET-keretrendszer 4.5 felépítése ismeretét.

    * Bármilyen IDE szeretne használni. Ajánlott [Visual Studio](https://www.visualstudio.com/vs) 2015-ös, 2017, vagy [Visual Studio Code](https://code.visualstudio.com/). Ez a dokumentum használatát a Visual Studio 2017 hello szükséges lépések.

* Egy módon tooupload .exe fájlok toohello fürt, és futtassa a Pig és Hive feladat. Hello Data Lake Tools for Visual Studio, Azure PowerShell és az Azure parancssori felület javasoljuk. hello jelen dokumentumban leírt lépések hello Data Lake Tools használni a Visual Studio tooupload hello fájlokhoz és hello példa Hive-lekérdezések futtatása.

    Más módokon toorun Hive-lekérdezések és a Pig-feladatokhoz kapcsolatos tudnivalókat lásd: a következő dokumentumok hello:

    * [Apache Hive használata a Hdinsightban](hdinsight-use-hive.md)

    * [Apache Pig használata a HDInsight](hdinsight-use-pig.md)

* A Hadoop on HDInsight-fürt. A fürtök létrehozásáról további információk: [HDInsight-fürtök létrehozása](hdinsight-provision-clusters.md).

## <a name="net-on-hdinsight"></a>A HDInsight .NET

* __Linux-alapú HDInsight__ használó fürtök [monó (https://mono-project.com)](https://mono-project.com) toorun .NET-alkalmazások. Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója.

    A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).

    Monó, egy adott verziójához toouse lásd: hello [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.

* __Windows-alapú HDInsight__ fürtök hello Microsoft .NET CLR toorun .NET alkalmazásokat használnak.

Hello verzióján hello .NET-keretrendszer és monó részét képező HDInsight-verziókról további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md).

## <a name="create-hello-c-projects"></a>Hozzon létre hello C\# projektek

### <a name="hive-udf"></a>Hive UDF-ben

1. Nyissa meg a Visual Studio és a megoldás létrehozása. Hello projekttípus, válassza a **Konzolalkalmazás (.NET-keretrendszer)**, és a név hello új projekt **HiveCSharp**.

    > [!IMPORTANT]
    > Válassza ki __.NET-keretrendszer 4.5__ egy Linux-alapú HDInsight-fürt használata. A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).

2. Cserélje le a hello tartalmát **Program.cs** hello alábbira:

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

3. Hello projekt felépítéséhez.

### <a name="pig-udf"></a>Pig UDF-ben

1. Nyissa meg a Visual Studio és a megoldás létrehozása. Hello projekttípus, válassza a **Konzolalkalmazás**, és a név hello új projekt **PigUDF**.

2. Cserélje le a hello hello tartalmát **Program.cs** hello kód a következő fájlt:

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

    Ez az alkalmazás elemez hello Pig által küldött és újraformázása sorokat kezdődő `java.lang.Exception`.

3. Mentés **Program.cs**, majd a hello projekt felépítéséhez és.

## <a name="upload-toostorage"></a>Toostorage feltöltése

1. A Visual Studióban nyissa meg a **Server Explorer**.

2. Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.

3. Ha a rendszer kéri, adja meg Azure-előfizetés hitelesítő adatait, és kattintson **bejelentkezés**.

4. Bontsa ki, hogy az alkalmazás kívánja toodeploy hello HDInsight-fürthöz. Hello szöveg bejegyzés __(alapértelmezett Tárfiók)__ szerepel.

    ![Server Explorer hello tárfiók hello fürt megjelenítése](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Ez a bejegyzés bővíthetők, ha az egy __Azure Storage-fiók__ hello fürt alapértelmezett tárolóként. tooview hello fájlok hello alapértelmezett tároló hello fürt, bontsa ki a hello bejegyzést, és kattintson duplán a hello __(alapértelmezett tároló)__.

    * Ez a bejegyzés nem bonthatók ki, ha az __Azure Data Lake Store__ hello fürt hello alapértelmezett tárolóként. tooview hello fájlok hello alapértelmezett tároló hello fürt, kattintson duplán a hello __(alapértelmezett Tárfiók)__ bejegyzés.

6. tooupload hello .exe fájlok, a hello a következő módszerek valamelyikével:

    * Ha használ egy __Azure Storage-fiók__, hello Feltöltés ikonra, és tallózással keressen toohello **bin\debug** hello mappa **HiveCSharp** projekt. Végül válassza ki a hello **HiveCSharp.exe** fájlt, és kattintson a **Ok**.

        ![Töltse fel az ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Ha használ __Azure Data Lake Store__, kattintson a jobb gombbal egy üres területre a hello felsoroló, és válassza __feltöltése__. Végül válassza ki a hello **HiveCSharp.exe** fájlt, és kattintson a **nyitott**.

    Egyszer hello __HiveCSharp.exe__ feltöltése befejeződött, ismétlődő hello feltöltési folyamat a hello __PigUDF.exe__ fájlt.

## <a name="run-a-hive-query"></a>Hive-lekérdezések futtatása

1. A Visual Studióban nyissa meg a **Server Explorer**.

2. Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.

3. Kattintson a jobb gombbal hello fürt hello telepített **HiveCSharp** alkalmazás számára, és adja **Hive lekérdezés írása**.

4. A következő szöveg hello Hive lekérdezés hello használata:

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
    > Állítsa vissza a hello `add file` utasítást, amely megfelel a fürthöz használt alapértelmezett tárolási hello típusának.

    Ez a lekérdezés kiválasztja hello `clientid`, `devicemake`, és `devicemodel` mezőit a `hivesampletable`, és továbbadja hello mezők toohello HiveCSharp.exe alkalmazás. hello lekérdezést vár hello alkalmazás tooreturn három mezőt, mint a tárolt `clientid`, `phoneLabel`, és `phoneHash`. hello lekérdezés is vár toofind HiveCSharp.exe hello alapértelmezett tároló hello gyökerében található.

5. Kattintson a **Submit** toosubmit hello feladat toohello HDInsight-fürthöz. Hello **Hive feladat összegzése** ablak nyílik meg.

6. Kattintson a **frissítése** toorefresh hello összefoglaló, amíg **feladatállapot** túl változik**befejezve**. tooview hello feladat kimeneti, kattintson a **Job Output**.

## <a name="run-a-pig-job"></a>A Pig-feladat futtatása

1. A következő módszerek tooconnect tooyour HDInsight-fürt hello egyikét használja:

    * Ha használ egy __Linux-alapú__ HDInsight fürt esetén az SSH használata. Például: `ssh sshuser@mycluster-ssh.azurehdinsight.net`. További információkért lásd: [az SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * Ha használ egy __Windows-alapú__ HDInsight-fürtjéhez, [Connect toohello fürt távoli asztali kapcsolattal](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)

2. A következő parancs toostart hello Pig parancssori egy hello használata:

        pig

    > [!IMPORTANT]
    > Ha egy Windows-alapú fürtöt használ, használja inkább a következő parancsok hello:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    A `grunt>` kérdés megjelenik.

3. Adja meg a következő toorun hello .NET-keretrendszer alkalmazás által a Pig feladatot hello:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    Hello `DEFINE` utasítás hoz létre az alias `streamer` hello pigudf.exe alkalmazásokhoz, és `CACHE` betölti azt az alapértelmezett hello fürt tárolóhelyét. Később `streamer` hello használt `STREAM` operátor tooprocess hello szereplő oszlopok sorozataként NAPLÓHOZ és visszatérési hello az egyetlen sort.

    > [!NOTE]
    > az adatfolyamként történő használt alkalmazásnév hello kell lennie.%n hello \` (backtick) karakter mikor aliasnevet, és a "(szimpla idézőjel) használata esetén `SHIP`.

4. Után utolsó sora hello bevitelét, hello feladat elindul. Azt jelzi, hogy a kimeneti hasonló toohello szöveg a következő:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Következő lépések

Ebből a dokumentumból megtanulta, hogyan toouse a Hive és a Pig a HDInsight .NET-keretrendszer alkalmazás. Ha azt szeretné, hogy hogyan toouse a Hive és a Pig, Python: toolearn [a Hive és a hdinsight a Pig használata Python](hdinsight-python.md).

Más módokon toouse Pig és a Hive és a toolearn MapReduce használatáról tekintse meg a következő dokumentumok hello:

* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md)
