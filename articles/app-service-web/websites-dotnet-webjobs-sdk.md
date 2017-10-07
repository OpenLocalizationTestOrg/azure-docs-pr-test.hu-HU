---
title: aaaWhat hello Azure WebJobs SDK
description: "Egy bevezető toohello Azure WebJobs SDK-t. Milyen hello SDK, akkor hasznos, a jellemző forgatókönyvek és mintakódok mutatja be."
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
ms.openlocfilehash: efac7a75c3b68a6a6601fb298f2ccac9bd71709d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-azure-webjobs-sdk"></a>Mi az Azure WebJobs SDK hello
## <a id="overview"></a>– Áttekintés
Ez a cikk azt ismerteti, milyen hello WebJobs SDK ellenőrzi, hogy olyan gyakori forgatókönyveket tartalmaz, akkor hasznos, ha, és áttekintést használatának a kódban, a.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

[Webjobs-feladatok](websites-webjobs-resources.md) lehetővé teszi az Azure App Service, amely lehetővé teszi toorun egy program vagy parancsfájl hello egy webalkalmazást, API-alkalmazás vagy mobilalkalmazás ugyanabban a környezetben. hello célját hello [WebJobs SDK](websites-webjobs-resources.md) közös elvégezhető webjobs-feladat, például képek feldolgozását, a várólista feldolgozása, a RSS összesítési, a fájlkarbantartás, írt toosimplify hello kódot, és e-mailek küldésekor. hello WebJobs SDK az Azure Storage és a Service Bus, a feladatütemezés és a hibák kezelése és sok más szabhatják beépített szolgáltatásai rendelkezik. Emellett arra tervezték, toobe bővíthető. Hello [WebJobs SDK nyílt forráskódú](https://github.com/Azure/azure-webjobs-sdk/), és nincs olyan [nyílt forráskódú extensions tárháza](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

hello WebJobs SDK hello a következő összetevőket tartalmazza:

* **NuGet-csomagok**. NuGet-csomagok hozzáadása tooa Visual Studio-Konzolalkalmazás projekt adjon meg egy keretrendszer, amely szerint a WebJobs SDK attribútumokkal módszerek dekoráció használja a kódot.
* **Irányítópult**. Hello WebJobs SDK részét tartalmazza az Azure App Service-ben, és hatékony monitoring és diagnosztikai biztosít hello NuGet-csomagok használó programokban. Toowrite kód toouse nincs figyelés és diagnosztika felhasználásokhoz.

## <a id="scenarios"></a>Forgatókönyvek
Az alábbiakban néhány tipikus forgatókönyvet az Azure WebJobs SDK hello könnyebben kezelheti:

* Kép feldolgozásra vagy más processzorigényes feladat. A webhelyek gyakran elérhető szolgáltatás hello képességét tooupload képeket vagy videókat. Gyakran érdemes toomanipulate hello tartalom feltöltése van, de nem kívánt toomake hello felhasználói Várjon, amíg ezt követően.
* Várólista feldolgozása. Egy webes előtér toocommunicate a háttérszolgáltatás közös módja toouse várólisták. Ha a webhely hello tooget dolgozott, egy üzenetsort, egy üzenet leküldéses értesítések. Háttérszolgáltatás hello várólistában lévő üzenetek kéri le, és a munka hello. Várólisták használata kép feldolgozásra: például hello felhasználó feltölt egy fájlok száma, miután be hello fájlnevek egy várólista üzenet toobe hello háttérbeli feldolgozás észlelnie. Vagy használhat várólisták tooimprove hely válaszképességét. Például ahelyett közvetlenül tooa SQL-adatbázis, tooa várólista írási, hello felhasználó értesítése, kész van, és lehetővé teszik a hello háttér szolgáltatás leíró nagy késleltetésű relációs adatbázis működik. Kép folyamat feldolgozáson várólista példáért lásd: hello [WebJobs SDK első lépéseket bemutató oktatóanyaghoz](websites-dotnet-webjobs-sdk-get-started.md).
* RSS-összesítési. Ha egy helyet, amely listát készít az RSS-hírcsatornák, akkor az összes hello cikkeket a hello hírcsatornák háttérfolyamatként sikerült lekéréses.
* Fájlkarbantartás, például összesítése, vagy törölje a rendszernapló fájljaiban.  Előfordulhat, hogy több helynek vagy a külön létrehozott naplófájlok időtartamainak, amely a kívánt toocombine sorrendben toorun feladatok rajtuk. Vagy a tooschedule egy feladat toorun heti tooclean mentése régi naplófájlokat.
* Az Azure-táblákban érkező. Előfordulhat, hogy tárolt fájlok és a blobok, és szeretné tooparse őket és hello adat tárolása táblákat. hello érkező függvény sikerült írása nagy mennyiségű sort (bizonyos esetekben több millió), és a WebJobs SDK hello teszi lehetséges tooimplement Ez a funkció könnyen. hello SDK is biztosít, például írt hello tábla sorainak száma hello folyamatjelző valós idejű figyelését.
* Más hosszan futó feladatokat, amelyet a háttérszálon toorun például [e-mailek küldésekor](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 
* Minden feladatot, amelyet az ütemezés szerint, például a biztonsági mentése műveletet minden toorun.

Ezek a forgatókönyvek számos érdemes lehet egy webes alkalmazás toorun több virtuális gépeken, amelyek futna egyidejűleg több WebJobs tooscale. Bizonyos esetekben emiatt hello azonos adatfeldolgozási első többször, de ez nem probléma hello beépített várólista, a blob és a hello WebJobs SDK a Service Bus-eseményindítók használata esetén. hello SDK biztosítja, hogy a funkciók dolgoz fel csak egyszer minden üzenet vagy a blob.

hello WebJobs SDK megkönnyíti a könnyen toohandle hibakezelés szabhatják. Állíthat be riasztásokat toosend értesítések egy parancs nem működik, és időtúllépések beállíthatja, hogy egy függvény automatikusan megszűnik, ha azt nem fejezi be a megadott időkorláton belül.

## <a id="code"></a>Kódminták
szokásos feladatokat, amelyek az Azure Storage használatához kezelő hello kód felettébb egyszerű. A Konzolalkalmazás `Main` metódus hoz létre egy `JobHost` hello koordinálására objektum meghívja toomethods írhatók. hello WebJobs SDK keretrendszer tudja, amikor a módszereket, és milyen paraméter toouse értékei alapján hello WebJobs SDK toocall attribútumokra van a bennük foglalt használja. hello SDK biztosít *eseményindítók* , amelyek meghatározzák, milyen feltételek következtében hello függvény toobe nevezik, és *kötőanyagok* , amelyek meghatározzák milyen tooget információkat és a metódusok paramétereihez kimenő.

Például hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) attribútum egy függvény toobe hívható meg, ha egy üzenet jelenik meg a várólista, és ha hello üzenetformátum JSON egy bájttömböt vagy egy egyéni típusa, üdvözlőüzenetére automatikusan deszerializált okoz. Hello [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) attribútum elindítja a folyamat minden egyes új blob az Azure Storage-fiók létrehozásakor.

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

Hello `JobHost` objektum egy olyan tároló, a háttér-funkciók. Hello `JobHost` objektum figyelők hello működik, azok kiváltó eseményeket figyeli, és eseményindító események előfordulásakor végrehajtandó hello funkciók. Meghívja a `JobHost` metódus tooindicate hello tároló folyamat toorun a kíván-e hello aktuális szál vagy egy háttérszálon. Hello példában hello `RunAndBlock` metódus folyamatosan futó hello folyamat hello aktuális szálat.

Mivel hello `ProcessQueueMessage` ebben a példában a metódusnak egy `QueueTrigger` attribútum hello eseményindító funkcióval egy új várólista-üzenet fogadása hello érték. Hello `JobHost` objektum hello megadott várakozási (Ez a példa "webjobsqueue" jelöli) új üzeneteit figyeli, és ha talál olyat, meghívja `ProcessQueueMessage`. 

Hello `QueueTrigger` attribútum köti hello `inputText` toohello paraméterérték hello üzenetsorban lévő üzenetet. És hello `Blob` kötések attribútum egy `TextWriter` objektum tooa blob neve "blobnév" a "containername" nevű tárolót.  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

hello függvény akkor használja a paraméterek toowrite hello érték hello várólista üzenet toohello BLOB:

        writer.WriteLine(inputText);

hello eseményindító és kötő szolgáltatásait hello WebJobs SDK nagy mértékben egyszerűsítheti, hello kód toowrite rendelkezik. hello alacsony szintű kódot tooprocess üzenetsorok, blobok, vagy fájlokat vagy tooinitiate ütemezett feladatok, végezhető el az Ön hello WebJobs SDK keretrendszer. Például hello keretrendszer hoz létre a várólisták esetében, amelyek még nem léteznek, megnyílik hello várólista, olvasási várólista üzenetek, törléseket üzenetek feldolgozási sor, amikor feldolgozása befejeződött, hoz létre, amely nem létezik blob tárolók még írja tooblobs, és így tovább.

hello alábbi példakód mutatja eseményindítók számos egy webjobs-feladatot: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, és `ErrorTrigger`. 

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
            too= "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors toohello Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Ütemezés
Hello `TimerTrigger` attribútum által biztosított, hello képességét tootrigger funkciók toorun ütemezés szerint. Ütemezheti a webjobs-feladat, egy teljes osztályig Azure vagy az ütemezés egyedi funkciók használatával webjobs-feladat sem. a WebJobs SDK hello `TimerTrigger`. Ez a kódminta.

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

További mintakód, lásd: [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) hello azure-webjobs-sdk-bővítmények tárházban github.com.

## <a name="extensibility"></a>Bővíthetőség
Nem szab toobuilt a--WebJobs SDK hello funkcióval toowrite egyéni eseményindítók és kötőanyagok.  Például a gyorsítótár-események és rendszeres ütemezés eseményindítók írhat. Egy [nyílt forráskódú tárház](https://github.com/Azure/azure-webjobs-sdk-extensions) tartalmaz egy [részletes útmutató a WebJobs SDK bővítési](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) és minta kód toohelp megismerheti a saját eseményindítók és kötőanyagok írásához.

## <a id="workerrole"></a>Hello kívül WebJobs WebJobs SDK használatával
Hello hello WebJobs SDK a szabványos Konzolalkalmazás és bárhol – futtathat használó, webjobs-feladat nem tartalmaz toorun. A fejlesztési számítógépen, és akkor is futtathatja egy felhőalapú szolgáltatás feldolgozói szerepkör vagy egy Windows-szolgáltatás Ha egy adott környezetekben inkább éles környezetben, helyben tesztelheti hello program. 

Hello irányítópult azonban az Azure App Service webalkalmazás bővítményként csak érhető el. Ha szeretné, hogy toorun kívül webjobs-feladat, és továbbra is használhatják az irányítópult hello, konfigurálhatja a webes alkalmazás toouse hello ugyanazt a tárfiókot, hogy a WebJobs SDK irányítópult kapcsolati karakterlánc hivatkozik, és a webes alkalmazás WebJobs-irányítópulttal majd jelennek meg a függvény az adatait a program, hogy fut-e valahol máshol végrehajtás. Hello URL-cím https:// használatával kaphat a toohello irányítópult*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions. További információkért lásd: [irányítópult első hello WebJobs SDK a helyi fejlesztési](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), de vegye figyelembe, hogy a hello blogbejegyzés egy régi kapcsolati karakterlánc nevét jeleníti meg. 

## <a id="nostorage"></a>Irányítópult-funkciók
hello WebJobs SDK számos előnyt biztosít, még akkor is, ha nem adja meg WebJobs SDK eseményindítókat vagy kötőanyagok:

* Hello irányítópult funkciókat hívhat meg.
* Hello irányítópult funkciókat is visszajátszásos.
* Hello csatolt toohello az irányítópult meg tudja tekinteni a naplók adott webjobs-feladat (alkalmazásnaplók, írhatók Console.Out, Console.Error, Trace, stb.) vagy csatolva az őket létrehozó toohello adott függvény meghívása (használatával írt naplók egy `TextWriter` az objektum adott hello SDK paraméterként átadja a toohello függvény). 

További információkért lásd: [hogyan toomanually meghívni a függvényt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) és [hogyan naplózza az toowrite](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Következő lépések
Hello WebJobs SDK kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

Hello legújabb fejlesztések toohello WebJobs SDK kapcsolatos információkért lásd: hello [kibocsátási megjegyzések](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).

