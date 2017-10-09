---
title: "aaaSCP.NET programozási útmutatója |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse SCP.NET toocreate. A NET-alapú Storm-topológiák a HDInsight alatt futó Storm használható."
services: hdinsight
documentationcenter: 
author: raviperi
manager: jhubbard
editor: cgronlun
ms.assetid: 34192ed0-b1d1-4cf7-a3d4-5466301cf307
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: raviperi
ms.openlocfilehash: a57f4217b07e0e82a3f36650308695fbb45d9128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scp-programming-guide"></a>Szolgáltatáskapcsolódási pont programozási útmutató
Szolgáltatáskapcsolódási pont, a platform toobuild valós idejű, megbízható, következetes és magas teljesítmény adatfeldolgozási alkalmazás. Be van építve a [alatt futó Apache Storm](http://storm.incubator.apache.org/) – a streamfeldolgozási rendszer hello OSS Közösségek szerint. A Storm készült Nathan Marz, és nyissa meg által Twitter forrása. Ez a módszer a [Apache ZooKeeper](http://zookeeper.apache.org/), egy másik Apache tooenable nagymértékben megbízható elosztott koordinálásának és állapotkezelés projektre. 

Nem csak hello SCP projekt legelterjedtebb Windows alatt futó Storm, de hello projekt adott bővítmények és hello Windows ökoszisztéma testreszabása. hello bővítmények közé tartozik a .NET-fejlesztők számára, és a könyvtárak, hello testreszabás tartalmazza a Windows-alapú telepítést. 

hello bővítményt, és a Testreszabás úgy, hogy azt nem kell toofork hello OSS-projektek, és azt sikerült kihasználhatják a Storm épülő származtatott ökoszisztéma történik.

## <a name="processing-model"></a>Folyamatmodell
a szolgáltatáskapcsolódási pont hello adatok folyamatos listának van modellezve. Általában hello rekordokat flow néhány várólistán először, majd fel, és át legyenek-e a Storm-topológia belül üzleti logika, végül hello kimeneti sikerült kell adatcsatornán rekordokat tooanother SCP rendszert, vagy véglegesített toostores, például az elosztott fájlrendszer vagy adatbázisok, SQL Server például.

![Adatok tooprocessing, amely adattárat-adatcsatornákat etetési várólista ábrázoló diagram](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

Egy alkalmazás topológia a Storm, egy számítási grafikont határozza meg. Minden csomópont-topológiában feldolgozási logikát tartalmaz, és csomópontok közötti kapcsolatok jelölése. hello csomópontok tooinject bemeneti adatok hello topológia spoutokkal kapcsolatban, amelyek használt toosequence hello adatokat is nevezik. hello bemeneti adatok sikerült található fájl naplókat, a tranzakciós adatbázis, a rendszer teljesítményszámláló stb hello csomópontokat a mindkét bemeneti és kimeneti adatfolyamok Boltokhoz, amelyek tényleges adatok szűrése hello és beállításokat és összesítési beállítások nevezzük.

SCP-JE támogatja az ajánlott azon törekvéseit, a következő legalább egyszeri és pontosan-egyszer adatok feldolgozása. Az elosztott adatfolyam feldolgozása alkalmazásokban több hiba fordulhat elő, során az adatfeldolgozás, például a hálózati kimaradás, a gép hibájának vagy a felhasználói kód hiba stb. A legalább egyszeri feldolgozási biztosítja dolgoz fel az összes adat legalább egyszer által automatikusan visszajátszását hello ugyanazokat az adatokat, ha a hiba akkor fordul elő. A legalább egyszeri feldolgozási egyszerű és megbízható, és számos alkalmazásban is megfelel. Azonban ha hello alkalmazás szükséges a pontos leltár, például: legalább egyszeri feldolgozási nem elegendő óta hello ugyanazokat az adatokat sikertelen potenciálisan lejátszandó hello alkalmazás topológiában. Ebben az esetben, pontosan-feldolgozási úgy van kialakítva, miután toomake meg arról, hogy hello eredménye helyes-e akkor is, ha hello adatokat a rendszer játssza és többször feldolgozása történhet.

Használja ki az hello Java virtuális gép (JVM)-alapú Storm alatt hello SCP bekapcsolása .NET-fejlesztők toodevelop valós idejű adatok folyamat alkalmazások. hello .NET és a JVM TCP helyi szoftvercsatorna keresztül kommunikálnak. Alapvetően minden Spout vagy Bolt .net/Java-folyamat párból hello felhasználói programot futtató .net folyamatban, a beépülő modul.

toobuild egy adatfeldolgozási alkalmazás SCP fölött, több lépésre van szükség:

* Kialakításával és a várólistában lévő adatok hello spoutokkal kapcsolatban toopull megvalósításával kapcsolatban.
* Tervezése és megvalósítása Boltokhoz tooprocess hello bemeneti adatokat, és az adatok mentése tooexternal tárolja, például adatbázis.
* Hello topológia megtervezésére, majd küldje el, és hello topológia futtassa. hello topológia meghatározása csomópontok és hello adatok hello csomópontok közötti forgalom. Szolgáltatáskapcsolódási pont hello topológia meghatározása fogad, majd azt egy Storm-fürt, minden egyes csúcspont futtató egy logikai csomóponton telepítette. hello feladatátvételi és a méretezésről fog kell venni megvagyunk hello Storm Feladatütemező.

Néhány egyszerű példák toowalk azon, hogyan használja majd a dokumentum toobuild adatfeldolgozási alkalmazás SCP-je.

## <a name="scp-plugin-interface"></a>Szolgáltatáskapcsolódási pont beépülő modul felület
Szolgáltatáskapcsolódási pont beépülő modulok (vagy alkalmazások) olyan önálló exe is futtatható belül a Visual Studio hello fejlesztési fázis során, és éles környezetben a telepítés utáni hello Storm csővezeték csatlakoztatva. Szolgáltatáskapcsolódási pont hello beépülő modul írása van csak hello ugyanaz, mint bármely más szabványos Windows konzol biztosító alkalmazások írására. SCP.NET platform deklarál spout vagy bolt néhány felületet, és hello beépülő modul a programkód inkább a konfigurációkezelővel. hello fő Ez a kialakítás célja, hogy hello a felhasználó a saját üzleti logics, és más dolgokat toobe SCP.NET platform által kezelt elhagyása összpontosíthat.

hello beépülő modul a programkód meg kell valósítania az egyik hello következőket adapterhez, hogy hello topológia tranzakciós vagy nem tranzakciós-e, és hogy hello összetevő spout vagy bolt függ.

* ISCPSpout
* ISCPBolt
* ISCPTxSpout
* ISCPBatchBolt

### <a name="iscpplugin"></a>ISCPPlugin
ISCPPlugin hello közös felület különféle beépülő modulok. Azt jelenleg egy üres felületet.

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a>ISCPSpout
ISCPSpout hello felület nem tranzakciós spout.

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

Ha `NextTuple()` nevezik hello C\# felhasználói kód el tudná küldeni egy vagy több rekordokat. Ha semmilyen tooemit, ezt a módszert kell visszaadnia nélkül kibocsátó semmit. Fontos megjegyezni, hogy `NextTuple()`, `Ack()`, és `Fail()` összes nevezzük egy egyetlen szálon c. szoros ismétlődő\# folyamat. Nincsenek nem rekordokat tooemit, esetén udvarias toohave NextTuple alvó a rövid idő (például 10 ezredmásodperc) nem toowaste, így túl sok CPU.

`Ack()`és `Fail()` hívja meg a rendszer csak akkor, ha a nyugtázási mechanizmus fájlmegadásában fájlban engedélyezve van. Hello `seqId` van használt tooidentify hello rekordot, amely korrektúrák, vagy nem sikerült. Így ha nyugtázási engedélyezve van a nem tranzakciós topológia, kibocsátása függvény a következő hello Spout kell használható:

    public abstract void Emit(string streamId, List<object> values, long seqId); 

Ha nyugtázási nem támogatott nem tranzakciós topológia, hello `Ack()` és `Fail()` maradhatnak, üres függvényében.

Hello `parms` ezeket a funkciókat a bemeneti paraméterek csak üres szótár, olyan fenntartja a jövőbeli használatra.

### <a name="iscpbolt"></a>ISCPBolt
ISCPBolt hello felület nem tranzakciós bolt.

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

Új rekord nem érhető el, hello `Execute()` függvényt hívja tooprocess azt.

### <a name="iscptxspout"></a>ISCPTxSpout
ISCPTxSpout tranzakciós spout hello felület.

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

Akárcsak a nem tranzakciós másik részét `NextTx()`, `Ack()`, és `Fail()` összes nevezzük egy egyetlen szálon c. szoros ismétlődő\# folyamat. Ha nincsenek adatok tooemit,-e udvarias toohave `NextTx` rövid időn (10 ezredmásodperc) alvó nem toowaste, így túl sok CPU.

`NextTx()`feltüntettük toostart egy új tranzakció hello paraméter `seqId` is használva van használt tooidentify hello tranzakció `Ack()` és `Fail()`. A `NextTx()`, felhasználói el tudná küldeni adatokat tooJava oldalán. ZooKeeper toosupport ismétlési hello adatokat fog tárolni. ZooKeeper hello kapacitása korlátozott, mert felhasználói csak kell számú metaadat, a tranzakciós spout nem tömeges adatok.

A Storm fog visszajátszásos automatikusan egy tranzakció, ha a hiba, így `Fail()` normál esetben nem szabad meghívni. De ha a szolgáltatáskapcsolódási pont ellenőrizheti a hello metaadatok tranzakciós spout által kibocsátott, akkor meghívhatja `Fail()` hello metaadat érvénytelen esetén.

Hello `parms` ezeket a funkciókat a bemeneti paraméterek csak üres szótár, olyan fenntartja a jövőbeli használatra.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt
ISCPBatchBolt tranzakciós bolt hello felület.

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

`Execute()`Ha bejövő hello bolt új rekordot neve. `FinishBatch()`Amikor befejeződik a tranzakció neve. Hello `parms` bemeneti paraméter későbbi használatra van fenntartva.

Tranzakciós topológia, egy fontos tényező – nincs `StormTxAttempt`. Rendelkezik két mező `TxId` és `AttemptId`. `TxId`használt tooidentify egy bizonyos tranzakció, és a megadott tranzakció nem lehet több kísérlet hello tranzakció sikertelen, és ha a rendszer játssza vissza. Egy másik ISCPBatchBolt objektum tooprocess minden új fog SCP.NET `StormTxAttempt`, csak a például milyen Storm tegye a Java oldalon. hello Ez a kialakítás célja toosupport párhuzamos tranzakciók feldolgozását. Felhasználói tartsa azt szem előtt, ha tranzakció kísérlet befejeződött, hello megfelelő ISCPBatchBolt objektumot meg kell semmisíteni, és szemétgyűjtés.

## <a name="object-model"></a>Hálózatiobjektum-modellje
SCP.NET is biztosít egy egyszerű objektumok az fejlesztők tooprogram. Ezek **környezetben**, **Állapottárolója**, és **SCPRuntime**. Ezek hello többi részében Ez a szakasz tárgyalja.

### <a name="context"></a>Környezet
A környezetben futó környezet toohello alkalmazásokat tartalmaz. Minden egyes ISCPPlugin példány (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) rendelkezik egy megfelelő adatkörnyezet példányához. Környezet által biztosított hello funkciókat lehet osztani két részből áll: (1) hello statikus része, amely található hello teljes C\# feldolgozni, (2) hello dinamikus rész, amelyhez a lehetőség csak a hello adott adatkörnyezet példányához.

### <a name="static-part"></a>Statikus részében szerepel
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

`Logger`napló célra valósul meg.

`pluginType`van tooindicate hello beépülő modul típusú hello C használt\# folyamat. Ha hello C\# folyamat (nélkül Java) tesztcélú helyi módban fut, hello beépülő modul típusa `SCP_NET_LOCAL`.

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

`Config`tooget konfigurációs paraméterek Java oldaláról valósul meg. hello paraméterek át lettek adva, a Java oldaláról amikor C\# beépülő modul inicializálása. Hello `Config` paraméterek oszthatók két részből áll: `stormConf` és `pluginConf`.

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

`stormConf`a Storm által definiált paraméterek és `pluginConf` szolgáltatáskapcsolódási pont által definiált hello paraméterek. Példa:

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

`TopologyContext`tooget hello topológia környezetben, akkor a leghasznosabb, összetevők több párhuzamosság együtt kerül. Például:

    //demo how tooget TopologyContext info
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)                      
    {
        Context.Logger.Info("TopologyContext info:");
        TopologyContext topologyContext = Context.TopologyContext;                    
        Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());          
        taskIndex = topologyContext.GetThisTaskIndex();
        Context.Logger.Info("taskIndex: {0}", taskIndex);
        string componentId = topologyContext.GetThisComponentId();                    
        Context.Logger.Info("componentId: {0}", componentId);
        List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
        Context.Logger.Info("taskNum: {0}", componentTasks.Count);                    
    }

### <a name="dynamic-part"></a>Dinamikus részében szerepel
következő illesztők hello vannak vonatkozó tooa egyes adatkörnyezet példányához. hello környezetben példány SCP.NET platform által létrehozott, az átadott toohello felhasználói kód:

    // Declare hello Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple toodefault stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple toohello specific stream.
    public abstract void Emit(string streamId, List<object> values);  

Nem tranzakciós spout nyugtázási támogatása a következő metódus hello biztosítja:

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

A nem tranzakciós bolt nyugtázási támogató, legyen, vagy ha kifejezetten `Ack()` vagy `Fail()` hello kapott rekordot. És kibocsátó új rekordot, amikor meg kell adnia hello horgonyok hello új rekord. a következő módszerek hello vannak megadva.

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a>Állapottárolója
`StateStore`metaadatok szolgáltatások, a monoton sorrend létrehozása és a várakozási szabad koordinációs biztosít. Elosztott feldolgozási magasabb szintű absztrakciók építhetők `StateStore`, beleértve az elosztott zárolásokat, elosztott várólisták, korlátok és tranzakciós szolgáltatásokat.

Szolgáltatáskapcsolódási pont alkalmazások is használhatnak hello `State` objektum toopersist ZooKeeper, különösen a tranzakciós topológia egyes információk. Ennek a tranzakciós spout összeomlik, és indítsa újra, ha hello szükséges adatok lekérését ZooKeeper, és indítsa újra a hello folyamat során.

Hello `StateStore` főként objektumnak ezen módszerek:

    /// <summary>
    /// Static method tooretrieve a state store of hello given path and connStr 
    /// </summary>
    /// <param name="storePath">StateStore Path</param>
    /// <param name="connStr">StateStore Address</param>
    /// <returns>Instance of StateStore</returns>
    public static StateStore Get(string storePath, string connStr);

    /// <summary>
    /// Create a new state object in this state store instance
    /// </summary>
    /// <returns>State from StateStore</returns>
    public State Create();

    /// <summary>
    /// Retrieve all states that were previously uncommitted, excluding all aborted states 
    /// </summary>
    /// <returns>Uncommited States</returns>
    public IEnumerable<State> GetUnCommitted();

    /// <summary>
    /// Get all hello States in hello StateStore
    /// </summary>
    /// <returns>All hello States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all hello committed states
    /// </summary>
    /// <returns>Registries contain hello Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all hello Aborted State in hello StateStore
    /// </summary>
    /// <returns>Registries contain hello Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of hello State</typeparam>
    public State GetState(long stateId)

Hello `State` főként objektumnak ezen módszerek:

    /// <summary>
    /// Set hello status of hello state object toocommit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set hello status of hello state object tooabort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under hello give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get hello attribute value associated with hello given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

A hello `Commit()` metódust, ha simpleMode tootrue, egyszerűen törli az ZNode ZooKeeper a megfelelő hello. Ellenkező esetben törölni fogja hello aktuális ZNode, és egy új csomópont hozzáadása a hello LEKÖTÖTT\_elérési ÚTJA.

### <a name="scpruntime"></a>SCPRuntime
SCPRuntime hello a következő két módszert biztosít.

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

`Initialize()`van használt tooinitialize hello SCP futtatókörnyezetben. Ezzel a módszerrel hello C\# folyamat toohello Java oldalon, és lekérdezi-konfigurációs paraméterek és topológia környezet csatlakozni fognak.

`LaunchPlugin()`hurok feldolgozása használt tookick üdvözlőüzenetére ki. Ez a ciklus a hello C\# beépülő modul üzenetek űrlap Java ügyféloldali (beleértve a rekordokat és a vezérlő jeleket) kap, és majd folyamat köszönőüzenetei, lehet, hogy a hello felület metódus hívásakor adja meg az hello felhasználói kód. hello metódus bemeneti paramétere `LaunchPlugin()` van olyan delegált esetén, amely ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt felületet megvalósító objektum adhat vissza.

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

A ISCPBatchBolt, beszerezheti a Microsoft `StormTxAttempt` a `parms`, és ezzel toojudge hogy megismételt kísérlet-e. Ez általában hello véglegesítési bolt szabályonkénti, és azt mutatják be hello `HelloWorldTx` példa.

Általánosságban véve a hello SCP beépülő modulok Itt két módban futhat:

1. Helyi tesztelése mód: Ebben a módban hello SCP beépülő modulok (hello C\# felhasználói kód) belül a Visual Studio hello fejlesztési fázis során futtassa. `LocalContext`Ebben a módban, ami metódus tooserialize kibocsátott hello rekordokat toolocal fájlokat, és olvasási toomemory újra használható.
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. Normál mód: Ebben a módban hello SCP beépülő modulok indítja storm java folyamat.
   
    Íme egy példa SCP beépülő modul megnyitása:
   
        namespace Scp.App.HelloWorld
        {
        public class Generator : ISCPSpout
        {
            … …
            public static Generator Get(Context ctx, Dictionary<string, Object> parms)
            {
            return new Generator(ctx);
            }
        }
   
        class HelloWorld
        {
            static void Main(string[] args)
            {
            /* Setting hello environment variable here can change hello log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a>Topológia nyelv
SCP topológia meghatározása az adott nyelvhez és SCP topológiák konfigurálása. A Storm Clojure DSL alapul (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) és az SCP bővített.

Topológia specifikációk küldheti el, közvetlenül keresztül hello végrehajtásra fürt toostorm ***runspec*** parancsot.

SCP.NET kövesse funkciók toodefine hello tranzakciós topológia rendelkezik fel:

| **Új funkciók** | **Paraméterek** | **Leírás** |
| --- | --- | --- |
| **Tx-topolopy** |topológia-név<br />spout-leképezés<br />bolt-leképezés |Adja meg a tranzakciós topológia hello topológia nevű &nbsp;spoutok definition térkép és hello boltokhoz definition térkép |
| **SCP-tx-spout** |Exec-név<br />argumentum<br />Mezők |Adja meg a tranzakciós spout. Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.<br /><br />Hello ***mezők*** hello kimeneti mezők a spout |
| **SCP-tx-batch-bolt** |Exec-név<br />argumentum<br />Mezők |Adja meg egy olyan tranzakciós kötegben Bolthoz. Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum.***<br /><br />hello mezők hello kimeneti mezők a bolt. |
| **SCP-tx-commit-bolt** |Exec-név<br />argumentum<br />Mezők |Adja meg egy olyan tranzakciós Committer Bolthoz. Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.<br /><br />Hello ***mezők*** hello kimeneti mezők a bolt |
| **nontx-topolopy** |topológia-név<br />spout-leképezés<br />bolt-leképezés |Adja meg a nem tranzakciós topológia hello topológia nevű&nbsp; spoutok definition térkép és hello boltokhoz definition térkép |
| **SCP-spout** |Exec-név<br />argumentum<br />Mezők<br />paraméterek |Adja meg egy nem tranzakciós spout. Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.<br /><br />Hello ***mezők*** hello kimeneti mezők a spout<br /><br />Hello ***paraméterek*** nem kötelező, toospecify használja bizonyos paraméterek, például "nontransactional.ack.enabled". |
| **SCP-bolt** |Exec-név<br />argumentum<br />Mezők<br />paraméterek |Adja meg egy nem tranzakciós Bolt. Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.<br /><br />Hello ***mezők*** hello kimeneti mezők a bolt<br /><br />Hello ***paraméterek*** nem kötelező, toospecify használja bizonyos paraméterek, például "nontransactional.ack.enabled". |

SCP.NET rendelkezik hajtsa végre a kulcsok definiálva szavak:

| **Kulcs szavakat** | **Leírás** |
| --- | --- |
| **: neve** |Hello topológia nevének megadása |
| **: topológia** |Adja meg hello topológiáját hello funkciók felett, és azokat a build. |
| **: p** |Adja meg az egyes spout vagy bolt hello párhuzamossági mutató. |
| **: config** |Adja meg konfigurálása paraméter vagy frissítés hello meglévőket |
| **: séma** |Adja meg a hello séma az adatfolyam. |

És a gyakran használt paraméterek:

| **A paraméter** | **Leírás** |
| --- | --- |
| **"plugin.name"** |hello C# beépülő modul exe-fájl neve |
| **"plugin.args"** |beépülő modul argumentum |
| **"output.schema"** |Kimeneti séma |
| **"nontransactional.ack.enabled"** |Nyugtázási engedélyezve van-e a nem tranzakciós topológia |

hello runspec parancs telepíti hello bits együtt, hello használata hasonlít:

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

Hello ***erőforrás-dir*** paraméter nem kötelező, toospecify van szüksége, ha azt szeretné, hogy egy C tooplug\# az alkalmazás és a könyvtár hello alkalmazás, hello függőségeket és konfigurációk fogja tartalmazni.

Hello ***classpath*** paramétert is nem kötelező. Ha hello fájlmegadásában fájl tartalmaz Java Spout vagy Bolt használt toospecify hello Java classpath.

## <a name="miscellaneous-features"></a>Egyéb szolgáltatások
### <a name="input-and-output-schema-declaration"></a>Bemeneti és kimeneti séma nyilatkozat
hello felhasználó el tudná küldeni c. rekordot\# feldolgozni, hello platform kell tooserialize hello rekordot a byte [], átviteli tooJava oldalon, és a Storm a rekord toohello megcélzott át. Eközben alsóbb rétegbeli összetevők C hello\# folyamat rekordot kap vissza java oldaláról, és platform toohello eredeti típusok konvertálja, ezeket a műveleteket rejtettek hello Platform.

toosupport hello szerializálás és a deszerializálás, a felhasználói kód toodeclare hello sémája hello bemenetekhez és kimenetekhez van szüksége.

hello bemeneti/kimeneti adatfolyam séma van definiálva, dictionary, hello kulcs hello StreamId hello értéke pedig hello oszlopok hello típusú. hello összetevő rendelkezhet több adatfolyamok deklarálva.

    public class ComponentStreamSchema
    {
        public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
        public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
        public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
        {
            InputStreamSchema = input;
            OutputStreamSchema = output;
        }
    }


Környezeti objektumot, a következő API hozzáadott hello vezetünk be:

    public void DeclareComponentSchema(ComponentStreamSchema schema)

Biztosítsa, hogy a felhasználói kód, kibocsátott hello rekordokat veszi fel, hogy az adatfolyam definiált hello séma, vagy hello rendszer kivételhibát futásidejű kivételt.

### <a name="multi-stream-support"></a>Több adatfolyam-támogatás
Szolgáltatáskapcsolódási pont által támogatott felhasználó code tooemit, vagy több különböző adatfolyam: hello fogadnak azonos idő. hello kibocsátása metódus egy választható adatfolyam-azonosító paramétert fogad, mert a hello környezeti objektumot a hello támogatási tükrözi.

Két módszer a hello SCP.NET környezeti objektumot hozzá lett adva. Használt tooemit rekordot, vagy rekordokat toospecify StreamId. hello StreamId: karakterlánc, és mindkét c. konzisztens kell toobe\# és topológia meghatározása Spec hello.

        /* Emit tuple toohello specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

hello kibocsátás tooa nem létező adatfolyam futásidejű kivételek miatt.

### <a name="fields-grouping"></a>Mezők csoportosítás
beépített mezők csoportosításának Strom nem működik megfelelően a SCP.NET hello. Hello Java Proxy oldalon, a minden hello mezők adattípusok ténylegesen byte [], és csoportosítási hello mezők hello byte [] objektum kivonatoló kódot tooperform hello csoportosítási használja. hello byte [] objektum kivonatkód az hello cím az objektum a memóriában. Így hello csoportosítás nem megfelelő, a két bájthoz [] objektumokat, hogy megosztás hello azonos tartalmakat, de nem hello ugyanazt a címet.

SCP.NET hozzáadja egy testreszabott csoportosítási módszer, majd hello byte [] toodo hello csoportosítási hello tartalmának fogja használni. A **SPEC** fájl, hello szintaxis hasonlít:

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


itt

1. "scp-mező-csoport": "Testre szabott mező csoportosítási szolgáltatáskapcsolódási pont által megvalósított".
2. ": tx"vagy": nem tx" azt jelenti, hogy tranzakciós topológia esetén. Mivel hello index kezdődően eltér a tx és nem tx topológiák kell ezeket az információkat.
3. [0,1] azt jelenti, hogy a hashset osztály mező azonosítókat, 0-tól kezdődően.

### <a name="hybrid-topology"></a>Hibrid topológia
hello natív Storm Java nyelven van megírva. És SCP.Net továbbfejlesztett azt tooenable a vámügyi toowrite C\# code toohandle az üzleti logikát. De is támogatja a hibrid topológiák, amely tartalmaz, nem csak a C\# spoutokkal kapcsolatban/boltokhoz, emellett pedig Java Spout/Boltokhoz.

### <a name="specify-java-spoutbolt-in-spec-file"></a>Adja meg a Java Spout vagy Bolt fájlmegadásában fájlban
Speciális fájlban használt toospecify Java Spoutok és Boltokhoz is lehet "scp-spout" és "scp-bolt", például:

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

Itt `microsoft.scp.example.HybridTopology.Generator` hello hello Java Spout osztály neve.

### <a name="specify-java-classpath-in-runspec-command"></a>Adja meg a Java Classpath runSpec parancs
Ha azt szeretné, hogy a Java Spoutok vagy Boltokhoz tartalmazó toosubmit topológia, Java Spoutok toofirst fordítási hello vagy Boltokhoz szükséges, és hello Jar fájlok beolvasása. Majd adja meg, amely tartalmazza a hello Jar fájlok topológia elküldésekor hello java classpath. Például:

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

Itt **példák\\HybridTopology\\java\\cél\\**  van hello mappa hello Java Spout vagy Bolt Jar-fájlt tartalmazó.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Szerializálás és a deszerializálás között a Java és C\
A szolgáltatáskapcsolódási pont a következő Java és a C\# oldalán. A sorrend toointeract a natív Java spoutokkal kapcsolatban/Boltokhoz, szerializálással/Deszerializálással el kell végezni között a Java és a C\# oldalán, a következő diagram hello ismertetett módon.

![java-összetevő tooJava összetevő küldése tooSCP összetevő küldése ábrája](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. **A Java és a deszerializálás c. szerializálási\# oldal**
   
   Először igazolnia implementálásához alapértelmezett szerializálás Java oldal és a deszerializálás c.\# oldalán. hello szerializálási metódus a Java ügyféloldali speciális fájl adható meg:
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   Deszerializálási metódus c. hello\# oldalon kell megadni C\# felhasználói kód:
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   Az alapértelmezett implementációja kezelnie kell a legtöbb esetben, ha hello adattípus nem túl összetett. Bizonyos esetekben vagy mert hello felhasználói adattípusnak: túl összetett, vagy mert hello az alapértelmezett implementációja teljesítménye nem éri el hello felhasználói követelményeket, a beépülő modul felhasználó is a saját megvalósítási.
   
   hello szerializálni felület java nyelven ügyféloldali típusúként van definiálva:
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   hello deszerializálni C felülettel\# ügyféloldali típusúként van definiálva:
   
   Nyilvános csatoló ICustomizedInteropCSharpDeserializer
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. **Szerializálási c.\# ügyféloldali és a Java párhuzamosan a deszerializálás**
   
   szerializálási metódus a C hello\# oldalon kell megadni C\# felhasználói kód:
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   hello deszerializálása metódus Java oldal FÁJLMEGADÁSÁBAN fájlt kell megadni:
   
     (scp-spout
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   Itt "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" deszerializáló hello nevét, és "microsoft.scp.example.HybridTopology.Person" hello target osztály hello adatokat a rendszer deszerializálni.
   
   Felhasználó is beépülő modul a következőket teheti a saját C végrehajtásának\# szerializáló és Java deszerializáló. Ez a C hello felület\# szerializáló:
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   Ez az Java deszerializáló hello felület:
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a>Szolgáltatáskapcsolódási pont a gazdagép módja
Ebben a módban a felhasználó lefordítani a kódok tooDLL, és használja a szolgáltatáskapcsolódási pont toosubmit topológia által biztosított SCPHost.exe. hello fájlmegadásában fájl így néz ki:

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

Itt `plugin.name` megadott `SCPHost.exe` SCP SDK által biztosított. Pontosan három paramétert fogad, amely SCPHost.exe:

1. hello először egy hello dll-fájl neve, amely `"HelloWorld.dll"` ebben a példában.
2. hello második hello osztály neve, amely `"Scp.App.HelloWorld.Generator"` ebben a példában.
3. hello harmadik egyik hello nyilvános statikus metódus, amely lehet meghívott tooget ISCPPlugin példányának nevét.

Állomás üzemmódban a felhasználói kód, dll-fájl fordítása, és SCP platform által indított. Szolgáltatáskapcsolódási pont platform, teljes körű hozzáférést engedélyezzenek hello teljes feldolgozó logika kérheti le. Ezért azt javasoljuk ügyfeleink toosubmit topológia SCP-t állomás üzemmódban óta hello fejlesztési élmény egyszerűsítése és velünk, valamint a későbbi kiadásban kapcsolása nagyobb rugalmasságot és jobban visszamenőleges kompatibilitás érdekében.

## <a name="scp-programming-examples"></a>Szolgáltatáskapcsolódási pont programozási példák
### <a name="helloworld"></a>HelloWorld
**HelloWorld** van egy nagyon egyszerű példa tooshow egy SCP.Net ízét. A nem tranzakciós topológia használ egy nevű spout **generátor**, és két boltokhoz nevű **elválasztó** és **számláló**. hello spout **generátor** fog véletlenszerűen generálja néhány mondatot, és ezeket a mondatok túl kibocsátás**elválasztó**. hello bolt **elválasztó** fog hello mondat toowords felosztása és ezeknek a szavaknak túl kibocsátás**számláló** bolt. hello bolt "számláló" szótár toorecord hello előfordulási számú használ minden szó.

Két fájlmegadásában fájlt **HelloWorld.spec** és **HelloWorld\_EnableAck.spec** ehhez a példához. A hello C\# kódot, akkor talál, nyugtázási engedélyezve van-e hello pluginConf lekérésével Java oldaláról.

    /* demo how tooget pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

Hello spout Ha engedélyezve van a nyugtázási, dictionary használt toocache hello rekordokat, amelyek még nincsenek korrektúrák. Ha Fail() nevezik, hello sikertelen rekordot fog bejegyzéseit meg kell ismételni:

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get hello cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay hello failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a>HelloWorldTx
Hello **HelloWorldTx** példa bemutatja, hogyan tooimplement tranzakciós topológia. Van-e egy spout nevű **generátor**, egy kötegelt boltokhoz nevű **részleges-count**, és egy véglegesítési bolt nevű **száma összeg**. Van még három előre létrehozott txt-fájloknál: **DataSource0.txt**, **DataSource1.txt** és **DataSource2.txt**.

Az egyes tranzakciókra, hello spout **generátor** fog véletlenszerűen két fájlt választhat hello előre létrehozott, mindhárom fájlt, majd kiadni hello két fájl nevének toohello **részleges-count** bolt. hello bolt **részleges-count** először beszerezni hello fájlnév kapott hello rekordot, majd nyissa meg hello fájl- és count hello szavak száma ebben a fájlban, és végül a hello word számú toohello kibocsátás **száma összeg**bolthoz. Hello **száma összeg** bolt hello számuk azokat.

tooachieve **pontosan egyszer** szemantikáját, hello véglegesítési bolt **száma összeg** toojudge kell, hogy egy megismételt tranzakció-e. Ebben a példában egy statikus tag változó rendelkezik:

    public static long lastCommittedTxId = -1; 

ISCPBatchBolt példány létrehozásakor kap-e hello `txAttempt` bemeneti paraméterek közül:

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from hello input parms */
        if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
        {
            StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
            return new CountSum(ctx, txAttempt);
        }
        else
        {
            throw new Exception("null txAttempt");
        }
    }

Ha `FinishBatch()` nevezik, hello `lastCommittedTxId` frissítés lesz, ha még nincs megismételt tranzakció.

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update hello toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a>HybridTopology
Ez a topológia tartalmaz egy Java Spout és egy C\# Bolt. Hello alapértelmezett szerializálása és deszerializálása implementációja SCP platform által biztosított használ. Kérjük, ref hello **HybridTopology.spec** a **példák\\HybridTopology** mappában hello fájlmegadásában fájlt, és **SubmitTopology.bat** arról, hogyan toospecify Java classpath.

### <a name="scphostdemo"></a>SCPHostDemo
Ebben a példában az hello ugyanaz, mint a HelloWorld lényegében. hello csak különbség az, hogy hello felhasználói kód lefordított dll-fájl, és hello topológia beküldött SCPHost.exe használatával. Kérjük, ref hello szakasz "SCP-t állomás üzemmódban" részletes ismertetése.

## <a name="next-steps"></a>Következő lépések
A szolgáltatáskapcsolódási pont használatával létrehozott Storm-topológiák, tekintse meg a következő hello:

* [Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Az Azure Event Hubs a HDInsight alatt futó Storm eseményeinek](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [Hozzon létre több adatfolyamokat a C# Storm-topológia](hdinsight-storm-twitter-trending.md)
* [A Power Bi toovisualize adat használata a Storm-topológia](hdinsight-storm-power-bi-topology.md)
* [Az Event Hubs a HDInsight alatt futó Storm használatával vehicle érzékelő adatok feldolgozása](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Kinyerési, átalakítási és betöltési (ETL) az Azure Event Hubs tooHBase](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [A HDInsight alatt futó Storm és HBase használatával összefüggésbe események](hdinsight-storm-correlation-topology.md)

