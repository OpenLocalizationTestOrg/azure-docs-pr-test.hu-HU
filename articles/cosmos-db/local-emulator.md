---
title: "helyileg a hello Azure Cosmos DB emulátor aaaDevelop |} Microsoft Docs"
description: "Hello Azure Cosmos DB Emulator használatával, fejlesztése és tesztelése az alkalmazás helyileg szabad, Azure-előfizetés létrehozása nélkül."
services: cosmos-db
documentationcenter: 
keywords: "Az Azure Cosmos DB emulátor"
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: arramac
ms.openlocfilehash: fb5449489e5f71664e72d8e11e583315be371bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Hello Azure Cosmos DB emulátor használja a helyi fejlesztéshez és teszteléshez

<table>
<tr>
  <td><strong>Bináris fájlok</strong></td>
  <td>[MSI-fájl letöltése](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><strong>Docker</strong></td>
  <td>[Docker központ](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><strong>Docker-forrás</strong></td>
  <td>[Github](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
hello Azure Cosmos DB Emulator emulálja hello Azure Cosmos DB szolgáltatás fejlesztési célokra szolgál egy helyi környezet biztosít. Hello Azure Cosmos DB Emulator használatával, létrehozhatja és helyileg, az alkalmazás tesztelése egy Azure-előfizetés létrehozása, és ezzel járó költségeket nélkül. Ha elégedett az alkalmazás a hello Azure Cosmos DB emulátor alakulását, toousing hello felhőben Azure Cosmos DB fiók válthat.

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Hello emulátor telepítése
> * A Docker a Windows hello emulátor fut
> * Kérések hitelesítése
> * Az emulátor hello adatkezelő hello használata
> * SSL-tanúsítványok exportálása
> * Hívása hello emulátor hello parancssorból
> * Nyomkövetési fájlok összegyűjtése

Azt javasoljuk, hogy Kezdésként tekintse a következő videót, amely Kirill Gavrylyuk mutatja, hogyan tooget hello Azure Cosmos DB emulátor lépések hello. Vegye figyelembe, hogy hello videó hello DocumentDB emulátor toohello emulátor néven hivatkozik, de maga hello eszköz át lett nevezve hello Azure Cosmos DB emulátor hello videó taping óta. Videó hello összes adata hello Azure Cosmos DB emulátor pontosak még. 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-hello-emulator-works"></a>Hello emulátor működése
hello Azure Cosmos DB emulátor egy valósághű emulációját hello Azure Cosmos DB szolgáltatást biztosít. Azure Cosmos DB, például létrehozása és a JSON-dokumentumok lekérdezését kiépítés azonos funkciókat támogatja, és gyűjtemények skálázás, és végrehajtása tárolt eljárásokként és eseményindítókként. Fejlesztés és hello Azure Cosmos DB emulátor használó alkalmazások tesztelése, és telepítheti őket az Azure Cosmos DB toohello csatlakozási végpont megváltoztatásához egyetlen konfigurációja csak tételével globális léptékű tooAzure.

Létrehoztunk egy valósághű helyi emuláció hello tényleges Azure Cosmos DB szolgáltatás, miközben hello végrehajtásának hello Azure Cosmos DB emulátor hello szolgáltatást, amely eltér. Például hello Azure Cosmos DB emulátor használja például a helyi fájlrendszer hello szabványos operációs rendszer összetevőit adatmegőrzési, és a HTTPS protokollhoz tartozó, a hálózati kapcsolatot. Ez azt jelenti, hogy bizonyos funkciók, például globális replikációs, egyjegyű ezredmásodperces késési olvasása/írása, és beállítható konzisztenciaszinteket hello Azure Cosmos DB emulátor keresztül nem érhetők el az Azure infrastruktúra támaszkodik.

> [!NOTE]
> Ez idő hello adatkezelő hello: emulátor csak DocumentDB API gyűjtemények és a MongoDB-gyűjtemény hello létrehozását támogatja. hello adatkezelő hello emulátorban jelenleg nem támogatja a hello létrehozását táblázatokat és diagramokat. 

## <a name="system-requirements"></a>Rendszerkövetelmények
hello Azure Cosmos DB emulátor hardver- és szoftverkövetelmények a következő hello rendelkezik:

* Szoftverkövetelmények
  * Windows Server 2012 R2, Windows Server 2016 vagy Windows 10
*   Minimális hardverkövetelmények
  * 2 GB RAM
  * 10 GB szabad lemezterület

## <a name="installation"></a>Telepítés
Töltse le, és telepítse a hello Azure Cosmos DB emulátor hello a [Microsoft Download Center](https://aka.ms/cosmosdb-emulator). 

> [!NOTE]
> tooinstall, konfigurálásához és futtatásához hello Azure Cosmos DB emulátor, hello számítógépen rendszergazdai jogosultsággal kell rendelkeznie.

## <a name="running-on-docker-for-windows"></a>A Windows Docker fut

Docker a Windows hello Azure Cosmos DB emulátor futtatható. hello emulátor nem működnek a Docker az Oracle Linux.

Ha már [Docker for Windows](https://www.docker.com/docker-windows) telepítve, akkor is lekérés hello emulátor rendszerképe a Docker Hub futtassa a következő parancsot a kedvenc rendszerhéjból hello (cmd.exe, PowerShell, stb.).

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
toostart hello kép, futtassa a következő parancsok hello.

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

hello válasz a következőhöz hasonló toohello következő:

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import hello SSL certificate from an administrator command prompt on hello host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

Hello interaktív rendszerhéj bezárása, ha emulátor elindult hello leállítási hello emulátor tároló.

Hello végpont és a hello válaszban található főkulcs használata az ügyfél és hello SSL-tanúsítvány importálása a gazdagépen. SSL-tanúsítvány, tooimport hello hello egy rendszergazdai parancssorból a következő:

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-hello-emulator"></a>Indítsa el a hello emulátor

toostart hello Azure Cosmos DB emulátor hello Start gombra, vagy hello Windows billentyűt. Írja be a szöveget **Azure Cosmos DB emulátor**, és jelölje be hello emulátor hello alkalmazások listája. 

![Hello Start gombra, vagy nyomja le az hello Windows kulcs kiválasztásához, írja be a szöveget ** Azure Cosmos DB emulátor **, és jelölje be hello emulátor hello alkalmazások listája](./media/local-emulator/database-local-emulator-start.png)

Hello emulátor futtatásakor a Windows tálca értesítési területén hello egy ikon látható. ![Az Azure Cosmos DB helyi emulátor értesítési](./media/local-emulator/database-local-emulator-taskbar.png)

hello Azure Cosmos DB emulátor alapértelmezés szerint a hello helyi számítógépen ("localhost") porton 8081 fut.

hello Azure Cosmos DB emulátor szerint telepíti a rendszer alapértelmezett toohello `C:\Program Files\Azure Cosmos DB Emulator` könyvtár. Is elindíthatja, és leállítja a parancssori hello hello emulátor. Lásd: [parancssori eszköz](#command-line) további információt.

## <a name="start-data-explorer"></a>Adatok Explorer elindítása

Amikor hello Azure Cosmos DB emulátor elindítja azt automatikusan megnyílik hello Azure Cosmos DB adatkezelő a böngészőben. hello cím jelenik meg [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Ha akkor zárja be hello explorer, és szeretné toore-nyissa meg azt később hello URL-cím megnyitása a böngészőben, vagy indítsa el a hello Azure Cosmos DB emulátor tálcaikon Windows hello a lent látható módon.

![Az Azure Cosmos DB helyi emulátor data explorer indítója](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Frissítések keresése
Adatkezelő azt jelzi, ha van egy új frissítés elérhető letöltésre. 

> [!NOTE]
> Hello Azure Cosmos DB emulátor egy verziójában létrehozott adatokat nem garantált toobe érhető el egy másik verzió használata esetén. Ha toopersist az adatok hosszú távú hello, ajánlott, hogy egy Cosmos-DB Azure-fiók, nem pedig a hello Azure Cosmos DB emulátor tárolja ezeket az adatokat. 

## <a name="authenticating-requests"></a>Kérések hitelesítése
Ahogy az Azure Cosmos DB hello felhőben hello Azure Cosmos DB emulátor képest végrehajtott kérelmek hitelesíteni kell. hello Azure Cosmos DB emulátor főkulcs hitelesítéshez ugyanazt a rögzített fiókot és egy jól ismert hitelesítési kulcs támogatja. A fiók és a kulcs nem hello csak hitelesítő adatokkal való használathoz az Azure Cosmos DB emulátor hello engedélyezett. Ezek a következők:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> hello Azure Cosmos DB emulátor által támogatott hello főkulcs csak hello emulátor való használatra készült. Az éles Azure Cosmos DB fiókot és kulcsot hello Azure Cosmos DB emulátor nem használható. 

> [!NOTE] 
> Hello emulátor indította hello /Key lehetőséget, ha létrehozott hello kulcsa használja "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Továbbá, ugyanúgy, mint a hello Azure Cosmos DB service, Azure Cosmos DB emulátor hello támogatja az SSL-en keresztüli csak biztonságos kommunikációt.

## <a name="running-hello-emulator-on-a-local-network"></a>A helyi hálózaton futó hello emulátor

Hello emulator egy helyi hálózaton is futtathatja. tooenable hálózati hozzáférés, adja meg a hello /AllowNetworkAccess kapcsolót hello [parancssori](#command-line-syntax), ami megköveteli azt is meg kell adnia /Key = key_string vagy /KeyFile = fájlnév. Használhatja a /GenKeyFile = fájlnév toogenerate fájl véletlenszerű kulccsal előzetes megfizetése esetén.  Ezt követően, hogy túl/KeyFile átadhatók = fájlnév vagy /Key = contents_of_file.

tooenable hálózati hozzáférés hello első alkalommal hello felhasználó leállítási hello emulátor kell, és törölje a hello emulátor adatkönyvtára (C:\Users\user_name\AppData\Local\CosmosDBEmulator).

## <a name="developing-with-hello-emulator"></a>Fejlesztés az emulátor hello
Ha Ön rendelkezik hello Azure Cosmos DB-emulátort az asztalon, használata támogatott [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) vagy hello [Azure Cosmos DB REST API](/rest/api/documentdb/) az emulátor hello toointeract. hello Azure Cosmos DB emulátor egy beépített adatkezelő, amely lehetővé teszi, hogy a gyűjtemények hello DocumentDB és MongoDB API-kat, és tekintse meg a dokumentumok létrehozásához és szerkesztéséhez programozás nélkül is.   

    // Connect toohello Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Ha használ [Azure Cosmos DB protokoll támogatása mongodb](mongodb-introduction.md), használja a következő kapcsolati karakterlánc hello:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

Használhatja például a meglévő eszközöket [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello Azure Cosmos DB emulátor. Is áttelepítheti az adatokat közötti hello Azure Cosmos DB emulátor és hello Azure Cosmos DB szolgáltatást hello [Azure Cosmos DB adatáttelepítési eszköz](https://github.com/azure/azure-documentdb-datamigrationtool).

> [!NOTE] 
> Hello emulátor indította hello /Key lehetőséget, ha létrehozott hello kulcsa használja "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Hello Azure Cosmos DB emulátor, alapértelmezés szerint használva létrehozhat too25 az egypartíciós gyűjtemények vagy 1 particionált gyűjtemény. Ez az érték módosításával kapcsolatos további információkért lásd: [hello PartitionCount érték](#set-partitioncount).

## <a name="export-hello-ssl-certificate"></a>Hello SSL-tanúsítvány exportálása

.NET-nyelveket és futásidejű használata hello Windows tanúsítványtároló toosecurely csatlakozás toohello Azure Cosmos DB helyi emulátor. Nyelvek kezelése és tanúsítványok segítségével a saját metódus rendelkezik. Java használ a saját [tanúsítványtároló](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) mivel Python használ [burkolók szoftvercsatorna](https://docs.python.org/2/library/ssl.html).

A sorrend tooobtain nyelvet és a futtatókörnyezetek, amely integrálható a tanúsítványtároló Windows hello, egy tanúsítvány toouse kell tooexport Tanúsítványkezelő Windows hello segítségével. Indítsa el a certlm.msc futtatásával, vagy hello részletes utasításokat követve [hello Azure Cosmos DB emulátor tanúsítványok exportálása](./local-emulator-export-ssl-certificates.md). Hello Tanúsítványkezelő futásakor, hello nyissa meg a személyes tanúsítványok alább látható módon és exportálási hello hello rövid nevű "DocumentDBEmulatorCertificate" tanúsítvány, egy BASE-64 kódolású X.509 (.cer) fájlba.

![Az Azure Cosmos DB helyi emulátor SSL-tanúsítvány](./media/local-emulator/database-local-emulator-ssl_certificate.png)

hello Java tanúsítványtároló hello X.509 tanúsítvány importálható hello utasításait követve [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtárolójában tanúsítvány toohello](https://docs.microsoft.com/azure/java-add-certificate-ca-store). Hello tanúsítvány hello tanúsítványtárolóba importálása, Java és a MongoDB alkalmazások képesek tooconnect toohello Azure Cosmos DB emulátor állnak.

A Python és a Node.js SDK-k toohello emulátor kapcsolódáskor SSL ellenőrzés le van tiltva.

## <a id="command-line"></a>Parancssori eszköz
Hello telepítési helyéről hello parancssori toostart használjon és hello emulátor leállítása, beállításokat adhat meg, és egyéb műveleteket végezhet.

### <a name="command-line-syntax"></a>Parancssori szintaxis

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

beállítások, típus tooview hello listájának `CosmosDB.Emulator.exe /?` hello parancssorban.

<table>
<tr>
  <td><strong>A beállítás</strong></td>
  <td><strong>Leírás</strong></td>
  <td><strong>A parancs</strong></td>
  <td><strong>Argumentumok</strong></td>
</tr>
<tr>
  <td>[Nincs argumentumok]</td>
  <td>Hello Azure Cosmos DB emulátor alapértelmezett beállításokkal indul.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Súgó]</td>
  <td>Hello listájának megjelenítése támogatott parancssori argumentumokat.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>Leállítás</td>
  <td>Hello Azure Cosmos DB emulátor leáll.</td>
  <td>CosmosDB.Emulator.exe shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>A toostore adatfájlok hello elérési út megadása Alapértelmezett érték a % LocalAppdata%\CosmosDBEmulator.</td>
  <td>CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</td>
  <td>&lt;DataPath&gt;: egy elérhető elérési útja</td>
</tr>
<tr>
  <td>Port</td>
  <td>Hello port száma toouse hello emulátor határozza meg.  Alapértelmezés szerint 8081.</td>
  <td>CosmosDB.Emulator.exe/port =&lt;port&gt;</td>
  <td>&lt;port&gt;: egy portszám</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>Megadja a hello port száma toouse MongoDB kompatibilitás API. Alapértelmezés szerint 10255.</td>
  <td>CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</td>
  <td>&lt;mongoport&gt;: egy portszám</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Megadja a hello portok toouse közvetlen kapcsolat. Alapértelmezett 10251,10252,10253,10254.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: 4 portok vesszővel tagolt listája</td>
</tr>
<tr>
  <td>Kulcs</td>
  <td>Hello emulátor hitelesítési kulcs. Kulcs hello base 64 kódolás egy 64 bájtos vektor kell lennie.</td>
  <td>CosmosDB.Emulator.exe /Key:&lt;kulcs&gt;</td>
  <td>&lt;kulcs&gt;: hello base 64 kódolás egy 64 bájtos vektor kulcsnak kell lennie</td>
</tr>
<tr>
  <td>EnableRateLimiting</td>
  <td>Megadja, hogy korlátozza az viselkedés kérelmek aránya engedélyezve van.</td>
  <td>CosmosDB.Emulator.exe /EnableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>DisableRateLimiting</td>
  <td>Meghatározza, hogy korlátozza az viselkedés kérelmek aránya le van tiltva.</td>
  <td>CosmosDB.Emulator.exe /DisableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>NoUI</td>
  <td>Ne jelenjen meg hello emulator felhasználói felületét.</td>
  <td>CosmosDB.Emulator.exe/noui</td>
  <td></td>
</tr>
<tr>
  <td>NoExplorer</td>
  <td>Ne jelenjen meg indításkor dokumentumkezelő.</td>
  <td>CosmosDB.Emulator.exe /NoExplorer</td>
  <td></td>
</tr>
<tr>
  <td>PartitionCount</td>
  <td>A particionált gyűjtemények a hello maximális számát határozza meg. Lásd: [gyűjtemények hello számának módosításához](#set-partitioncount) további információt.</td>
  <td>CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</td>
  <td>&lt;partitioncount&gt;: maximális száma az egypartíciós gyűjtemények engedélyezett. Alapértelmezett érték 25. Megengedett legnagyobb érték 250.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>A particionált gyűjtemény partíciók hello alapértelmezett számát adja meg.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</td>
  <td>&lt;defaultpartitioncount&gt; alapértelmezett érték 25.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Lehetővé teszi, hogy toohello emulátor hozzáfér a hálózaton. Is át kell /Key =&lt;key_string&gt; vagy /KeyFile =&lt;Fájlnév&gt; tooenable hálózati hozzáférést.</td>
  <td>CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;<br><br>vagy<br><br>CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile =&lt;fájlnév&gt;</td>
  <td></td>
</tr>
<tr>
  <td>NoFirewall</td>
  <td>Ne állítsa be úgy a tűzfalszabályok /AllowNetworkAccess használata esetén.</td>
  <td>CosmosDB.Emulator.exe /NoFirewall</td>
  <td></td>
</tr>
<tr>
  <td>GenKeyFile</td>
  <td>Hozzon létre egy új hitelesítési kulcsot, és mentse toohello megadott fájlt. hello létrehozott kulcs hello /Key vagy /KeyFile lehetőséggel használható.</td>
  <td>CosmosDB.Emulator.exe /GenKeyFile =&lt;tookey fájl elérési útja&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Konzisztencia</td>
  <td>A fiók hello hello alapértelmezett konzisztencia szintjének beállítása.</td>
  <td>CosmosDB.Emulator.exe /Consistency =&lt;konzisztencia&gt;</td>
  <td>&lt;konzisztencia&gt;: értéknek kell lennie a következő hello [konzisztenciaszintek](consistency-levels.md): munkamenet, erős, Eventual vagy BoundedStaleness.  hello alapértelmezett értéke munkamenet.</td>
</tr>
<tr>
  <td>?</td>
  <td>Üdvözlőüzenetére Súgó megjelenítése.</td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-hello-azure-cosmos-db-emulator-and-azure-cosmos-db"></a>Hello Azure Cosmos DB emulátor és Azure Cosmos DB közötti különbségek 
Hello Azure Cosmos DB Emulator egy helyi fejlesztői munkaállomáson fut emulált környezetet biztosít, mert vannak eltérések a funkciók között hello emulátor és Azure Cosmos DB fiók hello felhőben:

* hello Azure Cosmos DB emulátor csak egyetlen rögzített fiók és a jól ismert főkulcs támogatja.  Kulcs újragenerálása erre nincs lehetőség hello Azure Cosmos DB emulátor.
* hello Azure Cosmos DB emulátor nem egy méretezhető szolgáltatás, és nem fogja támogatni a nagy mennyiségű gyűjteményhez.
* hello Azure Cosmos DB emulátor nem szimulálása különböző [Azure Cosmos DB konzisztenciaszintek](consistency-levels.md).
* hello Azure Cosmos DB emulátor nem szimulálása [több területi replikációs](distribute-data-globally.md).
* hello Azure Cosmos DB Emulator nem támogatja a hello szolgáltatás kvóta felülbírálások által biztosított hello Azure Cosmos DB szolgáltatás (pl. dokumentum méretkorlátait, nagyobb particionált gyűjtemény tároló).
* Kérjük, mert hello Azure Cosmos DB emulátor a példány nem lehet másolatot toodate hello Azure Cosmos DB szolgáltatással hello legújabb módosításokkal, [Azure Cosmos DB kapacitás planner](https://www.documentdb.com/capacityplanner) tooaccurately becsült éles átviteli sebesség (RUs) az alkalmazás igényeinek.

## <a id="set-partitioncount"></a>Gyűjtemények hello számának módosítása

Alapértelmezés szerint too25 az egypartíciós gyűjtemények vagy 1 particionált gyűjtemény hello Azure Cosmos DB Emulator használatával hozhat létre. Hello módosításával **PartitionCount** érték, létrehozhat too250 az egypartíciós gyűjtemények vagy 10 particionált gyűjtemények, vagy tetszőleges kombinációja hello legfeljebb 250 egyetlen két partíciók (ahol 1 particionálva gyűjtemény = 25 egypartíciós gyűjtemény).

Ha után hello aktuális partíciók száma túl lett lépve. Próbálja toocreate egy gyűjtemény, hello emulátor ServiceUnavailable kivételt, a következő üzenet hello okoz.

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    toobring more and more capacity online, and encourage you tootry again. 
    Please do not hesitate tooemail docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

gyűjtemények elérhető toohello Azure Cosmos DB emulátor toochange hello száma hello a következő:

1. Kattintson a jobb gombbal a hello Azure Cosmos DB emulátor összes helyi adat törlése **Azure Cosmos DB emulátor** hello tálcán, majd kattintson az ikonra **adatok alaphelyzetbe állítása...** .
2. Ez a mappa C:\Users\user_name\AppData\Local\CosmosDBEmulator összes emulátor adatok törlése.
3. Minden nyitott példányokat kilépéshez kattintson a jobb gombbal a hello **Azure Cosmos DB emulátor** hello tálcán, majd kattintson az ikonra **kilépési**. Minden példány tooexit percet is igénybe vehet.
4. Hello hello legújabb verziójának telepítéséhez [Azure Cosmos DB emulátor](https://aka.ms/cosmosdb-emulator).
5. Indítsa el a hello PartitionCount jelző hello emulátor egy < = 250. Például: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="troubleshooting"></a>Hibaelhárítás

Használja a következő toohelp elhárítása hello Azure Cosmos DB emulátorral tapasztal hello:

- Ha telepített hello emulátor új verziója, és hibákat tapasztal, győződjön meg arról, alaphelyzetbe állítja az adatokat. Visszaállíthatja az adatokat, kattintson a jobb gombbal a hello Azure Cosmos DB emulátor ikonjára hello rendszerterületén, és kattintson az adatok alaphelyzetbe állítása... Ha ez nem segít hello hibák, távolítsa el, és telepítse újra az hello alkalmazást. Lásd: [hello helyi emulátor eltávolítása](#uninstall) utasításokat.

- Ha hello Azure Cosmos DB emulátor összeomlik, memóriaképek összegyűjtése c:\Users\user_name\AppData\Local\CrashDumps mappából, tömörítéssel és csatolja őket túl tooan e-mail[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Ha összeomlik a CosmosDB.StartupEntryPoint.exe, futtassa a következő parancsot egy rendszergazdai parancssorból hello:`lodctr /R` 

- Ha egy hálózati probléma, [nyomkövetési fájlok összegyűjtése](#trace-files), tömörítéssel, és csatolja őket túl tooan e-mail[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Ha megjelenik egy **szolgáltatás nem érhető el** üzenet, hello emulátor valószínűleg hibás tooinitialize hello hálózati vermet. Ellenőrizze a toosee, ha nem rendelkezik hello Pulse secure ügyfél vagy Juniper hálózatok telepített, mint a hálózati fájlrendszerszűrő-illesztőprogramok hello problémát okozhatja. Harmadik féltől származó hálózati fájlrendszerszűrő-illesztőprogramok eltávolítása általában javít hello hibát.

### <a id="trace-files"></a>Nyomkövetési fájlok gyűjtése

hibakeresési nyomkövetés, futtassa a következő parancsokat egy rendszergazdai parancssorból hello toocollect:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. A Watch hello rendszer tálca toomake meg arról, hogy hello program leállt, egy percig is eltarthat. Csak is kattinthat **kilépési** hello Azure Cosmos DB emulator felhasználói felületén.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Hello probléma reprodukálásához szükséges. Data Explorer nem működik, ha elegendő toowait hello böngésző tooopen néhány másodperc toocatch hello hibához.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Keresse meg a túl`%ProgramFiles%\Azure Cosmos DB Emulator` hello docdbemulator_000001.etl fájlt.
7. Hello .etl fájl együtt Reprodukálja lépéseket túl küldése[ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) a hibakereséshez.

### <a id="uninstall"></a>Távolítsa el hello helyi emulátor

1. Hello minden nyitott példányai való kilépéshez kattintson a jobb gombbal a hello tálcán hello Azure Cosmos DB emulátor ikonjára, majd a kilépési helyi emulátor. Minden példány tooexit percet is igénybe vehet.
2. Írja be a Windows hello keresőmezőbe, **alkalmazások és szolgáltatások** , majd kattintson a hello **alkalmazások és szolgáltatások (rendszerbeállítások)** eredménye.
3. Hello alkalmazások listájának megtekintéséhez görgessen túl**Azure Cosmos DB emulátor**, válassza ki azt, kattintson a **Eltávolítás**, majd erősítse meg, és kattintson a **Eltávolítás** újra.
4. Hello alkalmazás eltávolításakor, keresse meg a tooC:\Users\<felhasználó > \AppData\Local\CosmosDBEmulator és delete hello mappában. 

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Telepített helyi emulátor hello
> * VÉL hello emulátor Docker a Windows rendszeren
> * Hitelesített kéréseknél
> * Az emulátor hello adatkezelő hello használt
> * Exportált SSL-tanúsítványok
> * Hello emulátor nevű hello parancssorból
> * Összegyűjtött nyomkövetési fájlokat

Ebben az oktatóanyagban korábban megtanulta, hogyan toouse hello szabad helyi fejlesztési helyi emulátor. Ezután folytassa a következő oktatóanyag toohello és megtudhatja, hogyan tooexport emulátor SSL-tanúsítványokat. 

> [!div class="nextstepaction"]
> [Hello Azure Cosmos DB emulátor tanúsítványok exportálása](local-emulator-export-ssl-certificates.md)
