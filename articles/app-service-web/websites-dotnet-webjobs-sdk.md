---
title: Mi az Azure WebJobs SDK?
description: "Bevezetés az Azure WebJobs SDK-val. Megtudhatja, mi az SDK-t, akkor hasznos, ha, és Kódminták jellemző forgatókönyvek."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 8281267b-572b-4b14-a328-6704493ea682
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 8eb05b7cbfb4505f2e94c5b8e6d367ec63a2f033
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-the-azure-webjobs-sdk"></a>Mi az Azure WebJobs SDK?
## <a id="overview"></a>– Áttekintés
Ez a cikk azt ismerteti, mi az a WebJobs SDK, ellenőrzi, hogy olyan gyakori forgatókönyveket tartalmaz, akkor hasznos, ha, és áttekintést használatának a kódban.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

[Webjobs-feladatok](websites-webjobs-resources.md) Azure App Service lehetővé teszi egy program vagy parancsfájl futtatását ugyanabban a környezetben, egy webalkalmazás, az API-alkalmazás vagy a mobilalkalmazás-szolgáltatása. A célja a [WebJobs SDK](websites-webjobs-resources.md) egyszerűbbé teheti a webjobs-feladat hajthat végre, például képek feldolgozását, a várólista feldolgozása, a RSS összesítési, a fájlkarbantartás, kapcsolatos általános feladatok írt kódot, és e-mailek küldésekor. A WebJobs SDK az Azure Storage és a Service Bus, a feladatütemezés és a hibák kezelése és sok más szabhatják beépített szolgáltatásai rendelkezik. Emellett úgy van kialakítva, fogva bővíthető. A [WebJobs SDK nyílt forráskódú](https://github.com/Azure/azure-webjobs-sdk/), és nincs olyan [nyílt forráskódú extensions tárháza](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

A WebJobs SDK a következő összetevőket tartalmazza:

* **NuGet-csomagok**. A Visual Studio-Konzolalkalmazás projekt hozzáadni kívánt NuGet-csomagok adjon meg egy keretrendszer, amely szerint a WebJobs SDK attribútumokkal módszerek dekoráció használja a kódot.
* **Irányítópult**. A WebJobs SDK részét tartalmazza az Azure App Service-ben, és hatékony monitoring és diagnosztikai biztosít a NuGet-csomagok használó programokban. A figyelés és diagnosztika szolgáltatások használatának kód írását, nem rendelkezik.

## <a id="scenarios"></a>Forgatókönyvek
Az alábbiakban néhány tipikus forgatókönyvet az Azure WebJobs SDK-val könnyebben kezelheti:

* Kép feldolgozásra vagy más processzorigényes feladat. A webhelyek gyakran elérhető szolgáltatás azt a képességet, képeket vagy videók feltöltéséhez. Gyakran érdemes segítségével kezelheti a tartalom feltöltése van, de nem szeretné a felhasználó, várjon, amíg ezt követően.
* Várólista feldolgozása. Közös így a háttérszolgáltatás kommunikálni egy webes előtér-üzenetsorok használata. Ha a webhely munkavégzésben, egy üzenetsort, egy üzenet leküldéses értesítések. Háttérszolgáltatás lekéri az üzenetsorból érkezett üzeneteket, és nem a munkát. Várólisták használata kép feldolgozásra: például után a felhasználó feltölt egy fájlok száma, helyezze a fájlnevek észlelnie kell a háttérbeli feldolgozás várólista üzenetben. Vagy várólisták segítségével javítása hely válaszképességét. Például ahelyett, hogy közvetlenül egy SQL-adatbázis írásakor, írni a várólista elkészült, a felhasználó értesítése, és lehetővé teszik a háttér szolgáltatás leíró nagy késleltetésű relációs adatbázis működik. Kép folyamat feldolgozáson várólista példáért lásd: a [WebJobs SDK első lépéseket bemutató oktatóanyaghoz](websites-dotnet-webjobs-sdk-get-started.md).
* RSS-összesítési. Ha egy helyet, amely listát készít az RSS-hírcsatornák, akkor az összes háttérfolyamatként hírcsatornák a cikkek sikerült lekéréses.
* Fájlkarbantartás, például összesítése, vagy törölje a rendszernapló fájljaiban.  Lehetséges, hogy több helyek vagy kívánja kombinálni rajtuk feladatok futtatásához külön időtartamainak az éppen létrehozott naplófájlokat. Vagy előfordulhat, hogy szeretné futtatni a régi naplófájlokat karbantartása heti feladat ütemezése.
* Az Azure-táblákban érkező. Előfordulhat, hogy tárolt fájlok és a blobok, és szeretné elemzi őket, és az adatok tárolása táblák. A bejövő adatok függvény a nagy mennyiségű sort (bizonyos esetekben több millió) írása sikerült, és a WebJobs SDK megkönnyíti könnyen valósíthat meg ezt a funkciót. Az SDK-t is biztosít, például a sorokat a tábla írása a folyamatjelző a valós idejű figyelését.
* Más hosszan futó feladatokat, mint a háttérszálon futtatni kívánt [e-mailek küldésekor](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 
* Bármilyen feladat ütemezés szerint, például minden biztonsági mentése műveletet futtatni szeretné.

Ezek a forgatókönyvek számos érdemes lehet a több virtuális gépeken, amelyek futna egyidejűleg több webjobs-feladatok futtatásához egy webalkalmazás skálázása. Bizonyos esetekben emiatt kérelmének feldolgozása többször ugyanazokat az adatokat, de ez nem probléma használata esetén a beépített várólista, a blob és a Service Bus-eseményindítók a WebJobs SDK. Az SDK biztosítja, hogy a funkciók dolgoz fel csak egyszer minden üzenet vagy a blob.

A WebJobs SDK is könnyen közös hibakezelési helyzetek kezelésére. Riasztásokat állíthat be az értesítések küldése a parancs nem működik, és időtúllépések beállíthatja, hogy egy függvény automatikusan megszűnik, ha azt nem fejezi be a megadott időkorláton belül.

## <a id="code"></a>Kódminták
A szokásos feladatokat, amelyek az Azure Storage használatához kezelő kód felettébb egyszerű. A Konzolalkalmazás `Main` metódus hoz létre egy `JobHost` objektum, amely koordinálja a hívások módszerek írhatók. A WebJobs SDK-keretrendszer tudja, mikor legyen meghívva a módszereket, és milyen paraméterértékeket kíván használni a WebJobs SDK attribútumok alapján használja őket. Az SDK-t biztosít *eseményindítók* , amelyek meghatározzák, milyen feltételek okozhat a függvény hívása, és *kötőanyagok* , amelyek meghatározzák, hogyan kérhet információt, és a metódusok paramétereihez kimenő.

Például a [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) attribútum okoz hívható meg abban az esetben, ha egy üzenet jelenik meg a várólista, és az üzenet formátuma JSON egy bájttömböt vagy egy egyéni típusa, az üzenet akkor automatikusan deszerializált függvényt. A [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) attribútum elindítja a folyamat minden egyes új blob az Azure Storage-fiók létrehozásakor.

Íme egy egyszerű programot, amely lekérdezi a várólista hoz létre minden egyes sor üzenet érkezett a blob:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

A `JobHost` objektum egy olyan tároló, a háttér-funkciók. A `JobHost` objektum figyeli a Funkciók, kiváltó eseményeket figyeli, és végrehajtja a funkciók eseményindító események bekövetkezésekor. Meghívja a `JobHost` módszer annak jelzésére, hogy kívánja-e a tároló folyamat az aktuális szál vagy egy háttérszálon futtatni. A példában a `RunAndBlock` metódus folyamatosan futó a folyamat az aktuális szál.

Mivel a `ProcessQueueMessage` ebben a példában a metódusnak egy `QueueTrigger` attribútum az eseményindító a funkcióval egy új várólista-üzenet fogadása. A `JobHost` objektum a megadott várólista (Ez a példa "webjobsqueue" jelöli) az új üzeneteit figyeli, és ha talál olyat, meghívja `ProcessQueueMessage`. 

A `QueueTrigger` kötések attribútumot a `inputText` paraméter értékének az üzenetsorban lévő üzenetet. És a `Blob` kötések attribútum egy `TextWriter` egy blobba "containername" nevű tárolót "blobnév" nevű objektum.  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

A függvény használatával ezek a paraméterek értékét az üzenetsorban lévő üzenetet írni a blob:

        writer.WriteLine(inputText);

A WebJobs SDK eseményindító és kötő részeit, nagy mértékben egyszerűsítheti, a kódot kell írni. Az alacsony szintű feldolgozására, üzenetsorok, blobok vagy fájlokat, vagy ütemezett feladatok kezdeményezéséhez szükséges kód a WebJobs SDK-keretrendszer történik meg. Például a keretrendszer várólisták esetében, amelyek nem léteznek, megnyílik a várólista létrehozása, olvasása várólista üzenetek, törlések üzenetek feldolgozási sor, amikor feldolgozása befejeződött, hoz létre, amely nem létezik blob tárolók még ír, blobok és így tovább.

Az alábbi példakód mutatja egy webjobs-feladat eseményindítók számos: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, és `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Ütemezés
A `TimerTrigger` attribútum lehetővé teszi az eseményindító funkciók ütemezés futtatásához. Ütemezheti az Azure vagy az ütemezés a webjobs-feladat sem. a WebJobs SDK használatával egyéni függvényei keresztül egész webjobs-feladat `TimerTrigger`. Ez a kódminta.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

További mintakód, lásd: [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) a github.com azure-webjobs-sdk-bővítmények tárházban.

## <a name="extensibility"></a>Bővíthetőség
Nemcsak a beépített funkcióval--a WebJobs SDK lehetővé teszi egyéni eseményindítók és kötőanyagok írni.  Például a gyorsítótár-események és rendszeres ütemezés eseményindítók írhat. Egy [nyílt forráskódú tárház](https://github.com/Azure/azure-webjobs-sdk-extensions) tartalmaz egy [részletes útmutató a WebJobs SDK bővítési](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) és mintakódot az első lépések a saját eseményindítók és kötőanyagok írásához.

## <a id="workerrole"></a>A webjobs-feladatok kívül WebJobs SDK használatával
A program, amely használja a a WebJobs SDK egy szabványos Konzolalkalmazás, és bárhol futhatnak – a webjobs-feladat futtatása nem kell. A program helyileg is tesztelheti, a fejlesztési számítógépen, és éles környezetben futtatható egy felhőalapú szolgáltatás feldolgozói szerepkör vagy egy Windows-szolgáltatás Ha jobban szeret egyik adott környezetben. 

Az irányítópult azonban az Azure App Service webalkalmazás bővítményként csak érhető el. Ha azt szeretné, kívül webjobs-feladat futtatásához, és továbbra is használhatják az irányítópultot, konfigurálhatja egy webalkalmazást a használja ugyanazt a tárfiókot, amely a WebJobs SDK irányítópult kapcsolati karakterlánc hivatkozik, és hogy a webes alkalmazás WebJobs-irányítópulttal megjelennek a programból, hogy fut-e valahol máshol függvény végrehajtása adatait. Az URL-cím https:// használatával kaphat az irányítópulton*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions. További információkért lásd: [első irányítópult a WebJobs SDK-val helyi fejlesztési](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), de vegye figyelembe, hogy a blogbejegyzést egy régi kapcsolati karakterlánc nevét jeleníti meg. 

## <a id="nostorage"></a>Irányítópult-funkciók
A WebJobs SDK számos előnyt biztosít, még akkor is, ha nem adja meg WebJobs SDK eseményindítókat vagy kötőanyagok:

* Az irányítópult funkciókat hívhat meg.
* Az irányítópultról funkciók is visszajátszásos.
* Megtekintheti a naplók az irányítópulton, az adott webjobs-feladat (alkalmazásnaplók, írhatók Console.Out, Console.Error, Trace, stb.) kapcsolódó vagy csatolva az őket létrehozó adott függvény hívása (használatával írt naplók egy `TextWriter` objektum, a függvény paraméterként kapott az SDK-val). 

További információkért lásd: [hogyan manuális hívása a következő függvényt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) és [írásával naplók](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Következő lépések
A WebJobs SDK kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

A legújabb fejlesztések a WebJobs SDK-val kapcsolatos információkért tekintse meg a [kibocsátási megjegyzések](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).

