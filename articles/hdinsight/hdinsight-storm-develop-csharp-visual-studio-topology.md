---
title: "a Visual Studio és a C# - Azure HDInsight Storm-topológiák aaaApache |} Microsoft Docs"
description: "Ismerje meg, hogy a C# Storm-topológiák toocreate. Hozzon létre egy egyszerű word-count topológiához a Visual Studio által hello Hadoop tools for Visual Studio használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: larryfr
ms.openlocfilehash: b3fb01a4dda144fd7fb4141e624e31e667f93753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-hello-data-lake-tools-for-visual-studio"></a>C#-topológiák fejlesztése az Apache Storm által hello Data Lake tools for Visual Studio használatával

Ismerje meg, hogyan toocreate a C# Storm-topológia hello Azure Data Lake (Hadoop) használatával tools for Visual Studio. Ebből a dokumentumból hello folyamatot, amely egy Storm-projekt létrehozása a Visual Studio helyi tesztelés és tooan alatt futó Apache Storm on Azure HDInsight-fürt üzembe helyezése.

Azt is megtudhatja, hogyan C#-és Java használó hibrid topológiák toocreate.

> [!NOTE]
> Hello jelen dokumentumban leírt lépések a Visual Studio Windows fejlesztői környezetre támaszkodnak, amíg a hello lefordított projekt lehet elküldött tooeither egy Linux vagy a Windows-alapú HDInsight-fürt. Csak a Linux-alapú fürtök létrehozása után 28 2016 októberétől kezdve, SCP.NET topológiákat támogatja.

a Linux-alapú fürt toouse egy C# topológia, frissítenie kell a hello Microsoft.SCP.Net.SDK NuGet-csomagot a projekt tooversion 0.10.0.6 által használt vagy újabb. hello csomag hello verziójának is egyeznie kell telepíteni a HDInsight alatt futó Storm hello főverziója.

| HDInsight-verzió | A Storm verzióját | SCP.NET verzió | Alapértelmezett monó verzió |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.3 |0.10.x |0.10.x.x</br>(csak a Windows-alapú HDInsight) | NA |
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3.5 | 1.0.2.x | 1.0.0.x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0.x | 4.2.8 |

> [!IMPORTANT]
> C#-topológiák Linux-alapú fürtökön kell használni a .NET 4.5, és monó toorun hello HDInsight-fürt használatára. Ellenőrizze [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) az esetleges kompatibilitási problémák.

## <a name="install-visual-studio"></a>A Visual Studio telepítése

C#-topológiák rendelkező SCP.NET fejleszthet követően a Visual Studio verziójának hello egyikének használatával:

* A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)

* A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)

* Visual Studio 2015-öt vagy [Visual Studio 2015-Közösség](https://go.microsoft.com/fwlink/?LinkId=532606)

* A Visual Studio 2017 (minden kiadás)

## <a name="install-data-lake-tools-for-visual-studio"></a>Telepítse a Data Lake Visual Studio eszközök

tooinstall Data Lake tools for Visual Studio kövesse hello [Ismerkedés a Data Lake tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

## <a name="install-java"></a>Java telepítése

A Storm-topológia a Visual Studio eszközből elküldésekor SCP.NET hello topológia és a függőségek tartalmazó zip-fájl állít elő. Java használt toocreate ezek zip-fájlokhoz, mert több kompatibilis a Linux-alapú fürtökön formátuma nem használ.

1. Hello Java fejlesztői készlet (JDK) 7 vagy újabb a fejlesztési környezet telepítése. Kaphat az Oracle JDK hello [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html). Is [más Java terjesztéseket](http://openjdk.java.net/).

2. Hello `JAVA_HOME` környezeti változó kell pont toohello tartalmazó könyvtár Java.

3. Hello `PATH` környezeti változó tartalmaznia kell hello `%JAVA_HOME%\bin` könyvtár.

C# console application tooverify, hogy Java és hello JDK helyesen vannak-e telepítve a következő hello használhatja:

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a>A Storm-sablonok

hello Data Lake tools for Visual Studio adja meg a következő sablonok hello:

| Projekt típusa | Azt mutatja be |
| --- | --- |
| Storm-alkalmazás |Egy üres Storm-topológia projektet. |
| A Storm Azure SQL Writer minta |Hogyan toowrite tooAzure SQL-adatbázis. |
| A Storm Azure Cosmos DB olvasó minta |Hogyan tooread a Azure Cosmos-Adatbázisból. |
| A Storm Azure Cosmos DB író minta |Hogyan toowrite tooAzure Cosmos DB. |
| A Storm EventHub olvasó minta |Hogyan Azure Event hubs tooread. |
| A Storm EventHub író minta |Hogyan toowrite tooAzure Event Hubs. |
| A Storm HBase olvasó minta |Hogyan tooread a HBase a HDInsight-fürtök. |
| A Storm HBase író minta |Hogyan toowrite tooHBase a HDInsight-fürtök. |
| A Storm hibrid minta |Hogyan toouse egy Java-összetevő. |
| A Storm-minta |Alapszintű word-count topológiához. |

> [!WARNING]
> Nem minden sablonok a Linux-alapú hdinsight eszközzel fog működni. Hello sablonok által használt Nuget-csomagok nem lehet monó kompatibilis. Ellenőrizze a hello [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) hello használata és dokumentálása [.NET hordozhatóság Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potenciális problémákat.

A jelen dokumentumban leírt lépések hello hello alapszintű Storm-alkalmazás projekt típus toocreate a topológia használja.

### <a name="hbase-templates-notes"></a>A HBase sablonok megjegyzések

hello HBase olvasó és író sablonok használata hello HBase REST API-t nem hello HBase Java API-val a HDInsight-fürt HBase toocommunicate.

### <a name="eventhub-templates-notes"></a>Az EventHub sablonok megjegyzések

> [!IMPORTANT]
> Java-alapú EventHub hello spout hello alatt futó Storm példatopológiái 3.5-ös vagy újabb verziója nem működik az EventHub-olvasó sablon részét képező összetevő. Ez az összetevő frissített verziója érhető el, [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).

Ezt példatopológia összetevő, és együttműködik Storm on HDInsight 3.5, lásd: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>Hozzon létre egy C#-topológiák

1. Nyissa meg a Visual Studio, válassza ki **fájl** > **új**, majd válassza ki **projekt**.

2. A hello **új projekt** ablakában bontsa ki a **telepített** > **sablonok**, és válassza ki **Azure Data Lake**. Válassza ki a sablonok hello listáról **Storm-alkalmazás**. Hello a hello képernyő aljára, adja meg a **WordCount** hello alkalmazás hello neveként.

    ![Képernyőfelvétel az új projekt ablakról](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. Hello projekt létrehozása után rendelkeznie kell a következő fájlok hello:

   * **Program.cs**: ezt a fájlt a projekthez hello topológia meghatározása. Alapértelmezés szerint létrejön egy alapértelmezett topológia, amely egy spout és egy bolt áll.

   * **Spout.cs**: egy példa spout, amely véletlenszerű számok bocsát ki.

   * **Bolt.cs**: egy példa bolthoz, amely hello spout által kibocsátott számok számát követi.

     Hello projekt létrehozásakor NuGet letölti a legújabb hello [SCP.NET csomag](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-hello-spout"></a>Alkalmazzon hello spout

1. Nyissa meg **Spout.cs**. Spoutokkal kapcsolatban állnak a használt tooread adatok külső forrásból topológiában. egy spout hello fő összetevőket a következők:

   * **NextTuple**: amikor hello spout engedélyezett tooemit új rekordokat Storm hívja.

   * **Nyugtázási** (csak tranzakciós topológia): a nyugtázás hello topológia hello spout küldött rekordokat más összetevői által kezdeményezett kezeli. Egy rekord igazolása lehetővé teszi, hogy tudja, hogy megtörtént a feldolgozása sikeresen alsóbb rétegbeli összetevők hello spout.

   * **Sikertelen** (csak tranzakciós topológia): rekordokat, amelyek vannak sikertelen – a feldolgozás más összetevői hello topológia kezeli. A sikertelen metódusának lehetővé teszi a toore-kibocsátás hello rekordot, hogy újból feldolgozva.

2. Cserélje le a hello hello tartalmát **Spout** a következő szöveg hello osztályt. A spout véletlenszerűen hello topológia be bocsát ki egy mondat helyett szerepel.

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "hello cow jumped over hello moon",
        "an apple a day keeps hello doctor away",
        "four score and seven years ago",
        "snow white and hello seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set hello instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // hello schema for hello default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of hello spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // hello sentence toobe emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-hello-bolts"></a>Hello boltokhoz megvalósítása

1. Törölje a meglévő hello **Bolt.cs** fájl hello projektből.

2. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **Hozzáadás** > **új elem**. Hello listájából válassza ki **Storm Bolt**, és írja be **Splitter.cs** hello neveként. Ismételje meg a második bolt nevű folyamat toocreate **Counter.cs**.

   * **Splitter.cs**: egy olyan bolthoz, amely felosztja a mondatok egyedi szót, és megfelelően kibocsát egy új adatfolyam szavak valósítja meg.

   * **Counter.cs**: valósít meg olyan bolthoz, amely megjeleníti minden szó, és megfelelően kibocsát egy új adatfolyam szavak és minden szó hello száma.

     > [!NOTE]
     > Ezek a boltokhoz írási és olvasási toostreams, de egy bolt toocommunicate is használható adatforrások, például egy adatbázis vagy a szolgáltatás.

3. Nyissa meg **Splitter.cs**. Alapértelmezés szerint csak egyetlen módszer van: **Execute**. hello Execute metódus neve feldolgozásra rekordot hello bolt fogadásakor. Itt el tudja olvasni és feldolgozni a bejövő rekordokat, és a kibocsátás kimenő rekordokat.

4. Cserélje le a hello hello tartalmát **elválasztó** hello kód a következő osztályra:

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (hello sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (hello word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of hello bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello sentence from hello tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. Nyissa meg **Counter.cs**, és cserélje ki hello osztály tartalmát hello következőre:

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - hello word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - hello word and hello word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello word from hello tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for hello word in hello dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment hello count
        count++;
        // Update hello count in hello dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit hello word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-hello-topology"></a>Hello topológia meghatározása

A grafikus, amely meghatározza, hogyan hello közötti adatáramlás összetevők spoutokkal kapcsolatban és boltokhoz vannak rendezve. Az ebben a topológiában hello graph a következőképpen történik:

![Hogyan vannak elrendezve összetevők ábrája](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

A mondatok hello spout a kibocsátott, és a hello elválasztó bolt elosztott tooinstances. hello elválasztó bolt bontja szavakat, amelyek elosztott toohello számláló bolt hello mondatokat.

Szószámot helyileg tárolt hello teljesítményszámláló-példány, mert azt szeretnénk, ha meg arról, hogy a szavakat toohello flow toomake számláló bolt példányt. Minden példány nyomon követi a szavakat. Hello elválasztó bolt nem tart fenn, mert tényleg mindegy hello elválasztó példányok kap mely mondat helyett szerepel.

Nyissa meg **Program.cs**. hello fontos módszer **GetTopologyBuilder**, ami által használt toodefine hello topológia beküldött tooStorm. Cserélje le a hello tartalmát **GetTopologyBuilder** a következő kód tooimplement hello topológia a fentiekben ismertetett hello:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add hello spout toohello topology.
// Name hello component 'sentences'
// Name hello field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add hello splitter bolt toohello topology.
// Name hello component 'splitter'
// Name hello field that is emitted 'word'
// Use suffleGrouping toodistribute incoming tuples
//   from hello 'sentences' spout across instances
//   of hello splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add hello counter bolt toohello topology.
// Name hello component 'counter'
// Name hello fields that are emitted 'word' and 'count'
// Use fieldsGrouping tooensure that tuples are routed
//   toocounter instances based on hello contents of field
//   position 0 (hello word). This could also have been
//   List<string>(){"word"}.
//   This ensures that hello word 'jumped', for example, will always
//   go toohello same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-hello-topology"></a>Küldje el a hello topológia

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**.

   > [!NOTE]
   > Ha a rendszer kéri, adja meg hello hitelesítő adatait az Azure-előfizetéshez. Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.

2. Válassza ki a Storm on HDInsight-fürt hello **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**. Ha hello elküldése sikeres hello segítségével figyelheti **kimeneti** ablak.

3. Amikor hello topológia sikeresen elküldte, hello **Storm-topológiák** a hello fürt megjelenjen-e. Jelölje be hello **WordCount** hello lista tooview információt a futtató topológia hello topológia.

   > [!NOTE]
   > Megtekintheti továbbá **Storm-topológiák** a **Server Explorer**. Bontsa ki a **Azure** > **HDInsight**, kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza ki **nézet Storm-topológiák**.

    hello összetevők hello topológia tooview adatait hello összetevő hello diagramban kattintson duplán.

4. A hello **topológia összegzése** megtekintéséhez kattintson az **Kill** toostop hello topológia.

   > [!NOTE]
   > Storm-topológiák folytatható toorun, amíg azok inaktiválása vagy hello fürtök törlése.

## <a name="transactional-topology"></a>Tranzakciós topológia

hello előző topológia nem tranzakciós. hello összetevők hello topológiában nem valósítja meg a funkciók tooreplaying üzeneteket. Például egy tranzakciós topológia, hozzon létre egy projektet, és válassza **Storm minta** hello projekt típusként.

Tranzakciós topológiák valósítja meg a következő adatok toosupport ismétlés hello:

* **Metaadatok gyorsítótárazása**: hello spout kell tárolnia, így hello adatokat beolvasni, és újra kibocsátott, ha hiba történik a kibocsátott hello adatokkal kapcsolatos metaadatokat. Mert hello minta által kibocsátott hello adatok kicsi, a táblakonstruktor minden rekordjának hello nyers adatok a visszajátszás dictionary tárolja.

* **Nyugtázási**: hello topológiában minden bolt meghívhatja `this.ctx.Ack(tuple)` tooacknowledge, hogy sikeresen feldolgozta-rekordot. Ha az összes boltokhoz korrektúrák hello rekordot, hello `Ack` hello spout metódusában meghívták. Hello `Ack` módszer lehetővé teszi, hogy a hello spout tooremove adatok, amelyek a visszajátszás gyorsítótárazva lett.

* **Sikertelen**: minden bolt meghívhatja `this.ctx.Fail(tuple)` tooindicate a feldolgozása sikertelen volt a rekordot. hello hiba lejjebb toohello `Fail` metódusában hello spout, ahol hello rekordot is bejegyzéseit meg kell ismételni a gyorsítótárazott metaadatok.

* **A feladatütemezési azonosító**: egy rekord kibocsátó, amikor egy egyedi Sorozatazonosító adható meg. Ez az érték hello rekordot a visszajátszás (Ack és sikertelen) feldolgozás azonosítja. Például hello a hello spout **Storm minta** projekt hello következő használja, amikor adatokat kibocsátó:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Ez a kód megfelelően kibocsát egy rekord, amely tartalmazza a mondatok toohello alapértelmezett adatfolyam hello feladatütemezési azonosítóérték található **lastSeqId**. Ehhez a példához **lastSeqId** értéke eggyel növekszik, a kibocsátott minden rekordot.

Ahogyan az hello **Storm minta** projekt, a tranzakciós-e egy összetevő lehet futásidőben, a konfiguráció alapján állítsa be.

## <a name="hybrid-topology-with-c-and-java"></a>Hibrid C# és Java topológia

Data Lake tools is használhatja a Visual Studio toocreate hibrid topológiák, ha néhány összetevőt C# és Java.

Példa egy hibrid topológia, hozzon létre egy projektet, és jelölje ki **Storm hibrid minta**. Ez a minta-típus a következő fogalmak hello mutatja be:

* **Java spout** és **C# bolt**: az **HybridTopology_javaSpout_csharpBolt**.

    * A tranzakciós verzió van megadva **HybridTopologyTx_javaSpout_csharpBolt**.

* **C# spout** és **Java bolt**: az **HybridTopology_csharpSpout_javaBolt**.

    * A tranzakciós verzió van megadva **HybridTopologyTx_csharpSpout_javaBolt**.

  > [!NOTE]
  > Ebben a verzióban is bemutatja, hogyan toouse Clojure code Java összetevőjeként szövegfájlból.


hello projekt elküldésekor használt tooswitch hello topológia egyszerűen át hello `[Active(true)]` utasítás toohello topológiáját toouse, mielőtt elküldi azt toohello fürt.

> [!NOTE]
> A projekt hello részeként biztosított összes hello Java szükséges fájlok **JavaDependency** mappa.

Vegye figyelembe hello következőket létrehozása és elküldése a hibrid topológia:

* Használjon **JavaComponentConstructor** toocreate hello adott spout vagy bolt a Java-osztály példánya.

* Használjon **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize virtuális gépbe vagy onnan Java-összetevőt Java-adatobjektumok tooJSON.

* Hello topológia toohello server elküldésekor használnia kell a hello **további konfigurációs** beállítás toospecify hello **Java elérési utat**. hello elérési út van megadva a Java-osztályok tartalmazó JAR fájlok hello tartalmazó hello könyvtár kell lennie.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.NET 0.9.4.203 tartalmazza egy új osztályt és metódus kifejezetten a hello Event Hub spout (a Java spout az Event Hubs olvasó) használata. A topológia egy Event Hub spout használó létrehozásakor a következő módszerek hello használata:

* **EventHubSpoutConfig** osztály: hoz létre egy objektumot, amely hello spout összetevő hello konfigurációját tartalmazza.

* **TopologyBuilder.SetEventHubSpout** metódus: hello Event Hub spout összetevő toohello topológia hozzáadja.

> [!NOTE]
> Továbbra is használnia kell hello **CustomizedInteropJSONSerializer** hello spout által létrehozott tooserialize adatok.

## <a id="configurationmanager"></a>ConfigurationManager használata

Ne használjon **ConfigurationManager** tooretrieve konfigurációs értékei a boltok és összetevők spout. Így okozhat, null mutató által-kivétel. Ehelyett a projekthez hello konfigurációs átad a Storm-topológia hello hello topológia környezetben kulcs-érték párban. Minden egyes összetevő, amely támaszkodik a konfigurációs értékeket kell kérheti le azokat a hello környezetből inicializálása során.

hello következő kód bemutatja, hogyan tooretrieve ezeket az értékeket:

```csharp
public class MyComponent : ISCPBolt
{
    // toohold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of hello context for this component instance
        this.ctx = ctx;
        // If it exists, load hello configuration for hello component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve hello value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Ha egy `Get` metódus tooreturn egy példányát az összetevőt, győződjön meg arról, hogy mindkét hello haladnak `Context` és `Dictionary<string, Object>` paraméterek toohello konstruktor. hello alábbi példa az alapvető `Get` módszer, amelynek megfelelően továbbítja ezeket az értékeket:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-tooupdate-scpnet"></a>Hogyan tooupdate SCP.NET

SCP.NET újabb kiadásaiban a Nugeten keresztül Csomagfrissítés támogatja. Ha egy új frissítés érhető el, egy frissítési értesítést kap. a frissítés keresése toomanually kövesse az alábbi lépéseket:

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **NuGet-csomagok kezelése**.

2. Hello Csomagkezelő, válassza ki **frissítések**. Frissítés érhető el, ha szerepel. Kattintson a **frissítés** a hello csomag tooinstall azt.

> [!IMPORTANT]
> Ha a projekt, amely nem NuGet SCP.NET korábbi verziójával lett létrehozva, a következő lépéseket tooupdate tooa újabb verzióra hello kell elvégeznie:
>
> 1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **NuGet-csomagok kezelése**.
> 2. Hello segítségével **keresési** mezőben, keresse meg, és adja hozzá, **Microsoft.SCP.Net.SDK** toohello projekt.

## <a name="troubleshoot-common-issues-with-topologies"></a>Topológiák termékkel kapcsolatos gyakori hibák elhárítása

### <a name="null-pointer-exceptions"></a>NULL mutatót kivételek

C#-topológiák használatakor a Linux-alapú HDInsight-fürthöz boltok és összetevőket spout **ConfigurationManager** tooread konfigurációs beállításokat futásidőben adhat vissza null mutatót kivételek.

a projekt hello konfigurációs hello Storm-topológia átadott hello topológia környezetben kulcs-érték párban. Tooyour összetevők átadása akkor inicializálásakor hello szótárobjektum lekérhetők.

További információkért lásd: hello [ConfigurationManager](#configurationmanager) szakasz ebben a dokumentumban.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

C#-topológiák használatakor a Linux-alapú HDInsight-fürthöz felmerülhet a következő hiba hello:

    System.TypeLoadException: Failure has occurred while loading a type.

Ez a hiba akkor fordul elő, egy bináris, amely nem kompatibilis a monó támogató .NET hello verziójának használatakor.

Linux-alapú HDInsight-fürtök győződjön meg arról, hogy a projekt használ-e a bináris fájlok a .NET 4.5 számára.

### <a name="test-a-topology-locally"></a>A topológia helyi tesztelése

Könnyen toodeploy topológia tooa fürtökhöz, néhány esetben azonban szükség lehet a topológia helyileg tootest. A következő lépéseket toorun hello használja, és tesztelje a hello példa topológia ebben az oktatóanyagban helyileg a fejlesztői környezetben.

> [!WARNING]
> Helyi tesztelése csak akkor működik, a basic, a C#-topológiák csak. A hibrid topológiák vagy topológiák több adatfolyam használó helyi tesztelése nem használható.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **tulajdonságok**. A projekt tulajdonságai hello módosítása hello **kimeneti típus** túl**Konzolalkalmazás**.

    ![Képernyőfelvétel a projekt tulajdonságait, a kiemelt kimeneti típusú](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > Ne feledje toochange hello **kimeneti típus** túl biztonsági**Class Library** hello topológia tooa fürt központi telepítése előtt.

2. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza **Hozzáadás** > **új elem**. Válassza ki **osztály**, és írja be **LocalTest.cs** hello osztály neve. Végezetül kattintson **Hozzáadás**.

3. Nyissa meg **LocalTest.cs**, és adja hozzá a következő hello **használatával** hello felső utasítást:

    ```csharp
    using Microsoft.SCP;
    ```

4. Hello hello tartalmát kód használata hello következő **LocalTest** osztály:

    ```csharp
    // Drives hello topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test hello spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of hello spout, using hello local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext toopersist hello data stream toofile
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test hello splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set hello data stream toohello data created by hello spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test hello counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set hello data stream toohello data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    Eltarthat egy rövid ideig tooread keresztül hello kód megjegyzéseket. Ezt a kódot használja **LocalContext** toorun hello összetevők hello fejlesztési környezetet, és továbbra is fennáll, hello adatfolyam hello helyi meghajtón lévő összetevők tootext fájlok között.

1. Nyissa meg **Program.cs**, és adja hozzá a következő toohello hello **fő** módszert:

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize hello runtime
    SCPRuntime.Initialize();

    //If we are not running under hello local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. Hello módosítások mentéséhez, és kattintson a **F5** , vagy válasszon **Debug** > **Start Debugging** toostart hello projekt. A konzolablakban kell jelenik meg, és állapot naplózása hello tesztek állapotát. Ha **tesztek befejeződött** jelenik meg, nyomja meg bármelyik kulcs tooclose hello ablakban.

3. Használjon **Windows Explorer** toolocate hello a projektet tartalmazó könyvtárat. Például: **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Ebben a könyvtárban, nyissa meg a **Bin**, és kattintson a **Debug**. Hello szöveges fájlok, amelyeket a hello tesztek futtatásakor kell megjelennie: sentences.txt counter.txt és splitter.txt. Nyissa meg minden egyes szöveges fájlt, és vizsgálja meg a hello adatok.

   > [!NOTE]
   > Karakterlánc típusú adatok továbbra is fennáll, ezeket a fájlokat a decimális értékek ábrázolva. Például \[[97,103,111]] hello a **splitter.txt** fájl hello word *és*.

> [!NOTE]
> Lehet, hogy tooset hello **projekttípus** túl biztonsági**Class Library** tooa Storm on HDInsight-fürt üzembe helyezése előtt.

### <a name="log-information"></a>Naplóadatok

Könnyen bejelentkezhet információkat a topológia összetevői használatával `Context.Logger`. Hello következő például egy tájékoztató naplóbejegyzés hoz létre:

```csharp
Context.Logger.Info("Component started");
```

A naplózott információk is megtekinthetők hello **Hadoop Szolgáltatásnaplót**, amely megtalálható **Server Explorer**. Bontsa ki a hello bejegyzést a Storm on HDInsight-fürt számára, és végül **Hadoop Szolgáltatásnaplót**. Végül válassza ki a hello napló fájl tooview.

> [!NOTE]
> hello naplók hello Azure storage-fiók, amely a fürt által használt vannak tárolva. tooview hello naplók a Visual Studio alkalmazásban, be kell jelentkeznie toohello Azure-előfizetéssel, amely hello tárfiók tulajdonosa.

### <a name="view-error-information"></a>Hiba történt adatainak megtekintése

a futó topológiát az előfordult tooview hibák lépések hello használata:

1. A **Server Explorer**, kattintson a jobb gombbal a hello Storm on HDInsight-fürt, és válassza ki **nézet Storm-topológiák**.

2. A hello **Spout** és **boltok**, hello **utolsó hiba** oszlop hello utolsó hiba információkat tartalmaz.

3. Jelölje be hello **Spout azonosító** vagy **Bolt azonosító** hello összetevő, amely rendelkezik a felsorolt hiba. Hello részletes lapon, amely hiba jelenik meg, további információt hello szerepel **hibák** alján hello hello szakasz.

4. tooobtain további információt, jelölje be a **Port** a hello **végrehajtója** hello lap részében, toosee hello Storm munkavégző naplóban hello utolsó néhány percig.

### <a name="errors-submitting-topologies"></a>Hibák elküldése a topológiák

Ha hibákba ütközik egy topológia tooHDInsight elküldése, a naplók hello kiszolgálóoldali összetevők, amelyek kezelik a HDInsight-fürt topológiája küldésének is megtalálhatja. tooretrieve ezek naplózza, használjon hello következő parancsot a parancssorból:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

Cserélje le __sshuser__ a hello hello fürthöz SSH-felhasználói fiókhoz. Cserélje le __clustername__ hello nevű hello HDInsight-fürt. További információk az `scp` és `ssh` a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

Több okból sikertelen lehet a jelentések:

* JDK nincs telepítve, vagy nincs hello elérési úton.
* Java szükséges függőség nem szerepelnek a hello küldésének.
* Nem kompatibilis függőségek.
* Ismétlődő topológia nevek.

Ha hello `hdinsight-scpwebapi.out` a napló tartalmaz egy `FileNotFoundException`, ennek oka lehet a következő feltételek hello:

* hello JDK nincs hello fejlesztőkörnyezet hello elérési útját. Győződjön meg arról, hogy hello fejlesztési környezetet, és hogy telepítve van a JDK hello `%JAVA_HOME%/bin` hello elérési út része.
* Java függősége hiányzik. Győződjön meg arról, beleértve minden szükséges .jar fájlt hello küldésének részeként is.

## <a name="next-steps"></a>Következő lépések

Például az adatok feldolgozása az Event Hubs, [feldolgozni az eseményeket az Azure Event Hubs a HDInsight alatt futó Storm](hdinsight-storm-develop-csharp-event-hub-topology.md).

Például egy C#-topológiák, amely felosztja a több adatfolyam adatfolyam-adatokat, lásd: [C# Storm példa](https://github.com/Blackmist/csharp-storm-example).

toodiscover C#-topológiák, létrehozásával kapcsolatos további információért lásd: [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

További módszereket toowork a hdinsight eszközzel, és minták a HDInsight alatt futó Storm további tekintse meg a következő dokumentumok hello:

**Microsoft SCP.NET**

* [Szolgáltatáskapcsolódási pont programozási útmutató](hdinsight-storm-scp-programming-guide.md)

**A HDInsight alatt futó Apache Storm**

* [Telepítheti és figyelheti a HDInsight alatt futó Apache Storm topológiák](hdinsight-storm-deploy-monitor-topology.md)
* [HDInsight alatt futó Storm példatopológiái](hdinsight-storm-example-topology.md)

**Apache Hadoop on HDInsight**

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

**Apache HBase a HDInsight-on**

* [Bevezetés a hbase a HDInsight eszközzel](hdinsight-hbase-tutorial-get-started.md)
