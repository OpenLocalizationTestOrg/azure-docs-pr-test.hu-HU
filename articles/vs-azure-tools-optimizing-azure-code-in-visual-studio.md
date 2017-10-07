---
title: az Azure a Visual Studio code aaaOptimizing |} Microsoft Docs
description: "Ismerje meg, az Azure kód optimalizálási eszközök Visual Studio érdekében a kód megbízhatóbb és jobban végrehajtása."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 7df932def9dc16c93de29fc6a77c8fc121fda338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-your-azure-code"></a>Az Azure kód optimalizálása
Ha a Microsoft Azure használó alkalmazások által programozási, van néhány toohelp ajánlott eljárásit app méretezhetőséget, viselkedését, egy felhőalapú környezetben a teljesítményt és a problémák elkerülése érdekében. A Microsoft biztosít egy Azure kód elemző eszköz, amely ismeri fel és azonosítja a leggyakrabban észlelt problémák számos, és segítséget nyújt a megoldásukkal együtt. A Visual Studio NuGet útján hello eszköz töltheti le.

## <a name="azure-code-analysis-rules"></a>Az Azure Analysis kód szabályok
hello Azure kód elemző eszköz tooautomatically jelzőt az Azure kódot, ha talál teljesítményt érintő ismert problémákat szabályainak hello használja. Észlelt problémák jelenhetnek meg a figyelmeztetéseket vagy fordítási hibákat. A villanykörte ikonnal gyakran kipróbálni a kód javítások és a javaslatok tooresolve hello figyelmeztetés vagy hibaüzenet.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Ne használja az alapértelmezett (a folyamat) munkamenet-állapot módját
### <a name="id"></a>ID (Azonosító)
AP0000

### <a name="description"></a>Leírás
Ha hello alapértelmezett (a folyamat) munkamenet-állapot módját használja a felhőalapú alkalmazásokhoz, a munkamenet-állapot elveszhet.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Alapértelmezés szerint hello web.config fájlban megadott hello munkamenet-állapot módját a folyamatban. Is ha hello konfigurációs fájlban megadott bejegyzés, a munkamenet-állapot módját a hello tooin-folyamat alapértelmezés szerint. hello folyamaton belüli módot a munkamenet-állapot hello webkiszolgáló a memóriában tárolja. Amikor egy újraindítása, vagy egy új példányt a terheléselosztás és feladatátvétel támogatásáról szolgál, hello hello webkiszolgáló a memóriában tárolt munkamenet-állapot nem menti a rendszer. Ez a helyzet meggátolja, hogy hello nem méretezhető hello felhő.

Az ASP.NET munkamenet-állapot munkamenet-állapot adatainak támogatja a több, eltérő tárolási lehetőség: InProc, StateServer, SQL Server, egyéni, és ki. Ajánlott a használata egyéni mód toohost adatait a munkamenet-állapot külső áruházban, például a [Azure munkamenetállapot-szolgáltató az Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Megoldás
Egy ajánlott megoldás, egy felügyelt gyorsítótár szolgáltatásra toostore munkamenet-állapot. Megtudhatja, hogyan toouse [Azure munkamenetállapot-szolgáltató az Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore a munkamenet-állapot. Akkor is is tároló munkamenet állapot, a más helyek tooensure az alkalmazás méretezhető hello felhő. olvassa el az alternatív megoldások további toolearn [munkamenet-állapot módok](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Futtatási mód nem lehet aszinkron
### <a name="id"></a>ID (Azonosító)
AP1000

### <a name="description"></a>Leírás
Aszinkron metódusok létrehozása (például [await](https://msdn.microsoft.com/library/hh156528.aspx)) kívül hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer, majd a hívás hello aszinkron metódusok a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Hello deklaráló [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus aszinkron módon hatására hello feldolgozói szerepkör tooenter újraindítás hurkot.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Aszinkron metódusok hello belüli meghívása [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus okoz hello felhő futásidejű toorecycle hello feldolgozói szerepkör-szolgáltatás. A feldolgozói szerepkör indulásakor az összes program végrehajtását belül kerül sor hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust. A meglévő hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus okoz hello feldolgozói szerepkör toorestart. Hello feldolgozói szerepkör futásidejű találatok hello aszinkron metódussal, ha minden műveletnél kiszállítja hello aszinkron metódus után, és adja vissza. Ennek hatására hello feldolgozói szerepkör tooexit a hello [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust, és indítsa újra. Hello következő munkamenetben végrehajtás a hello feldolgozói szerepkör találatok hello aszinkron metódus újra és újraindul, okozó hello feldolgozói szerepkör toorecycle újra is.

### <a name="solution"></a>Megoldás
Helyezze el az összes aszinkron művelet kívül hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust. Majd, meghívják a átkerült hello aszinkron metódusnak belül hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer, például RunAsync () .wait. hello Azure kód elemzőeszköz segíthet a probléma megoldásához.

a következő kódrészletet hello hello kód a hiba javítása mutatja be:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Service Bus közös hozzáférésű Jogosultságkód-hitelesítés használata
### <a name="id"></a>ID (Azonosító)
AP2000

### <a name="description"></a>Leírás
Hitelesítéshez használandó közös hozzáférésű Jogosultságkód (SAS). A service bus hitelesítéshez Access Control Service (ACS) is elavult.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
A fokozott biztonság érdekében Azure Active Directory SAS hitelesítési ACS hitelesítési lecseréli. Lásd: [Azure Active Directory rendszer hello jövőbeli az ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) hello áttűnés terv olvashat.

### <a name="solution"></a>Megoldás
SAS-hitelesítés használata az alkalmazások. hello a következő példa bemutatja, hogyan toouse egy meglévő SAS-token tooaccess egy service bus névtér vagy entitás.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Tekintse meg a következő témaköröket további tudnivalókért hello.

* Megtudhatja, [megosztott hozzáférési aláírást hitelesítést a Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)
* [Hogyan toouse megosztott hozzáférési aláírása hitelesítés a Service busszal](https://msdn.microsoft.com/library/dn205161.aspx)
* Egy minta-projekt lásd [használatával közös hozzáférésű Jogosultságkód (SAS) hitelesítés a Service Bus-előfizetések](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-tooavoid-receive-loop"></a>Érdemes lehet OnMessage metódus tooavoid "az üzenetfogadási hurok"
### <a name="id"></a>ID (Azonosító)
AP2002

### <a name="description"></a>Leírás
tooavoid, amelyek egy "az üzenetfogadási hurok," hívó hello **OnMessage** metódus egy jobb megoldás, mint a hívó hello üzenetek fogadására **Receive** metódust. Azonban hello használata **Receive** metódust, és egy nem alapértelmezett server várakozási idő megadni, akkor győződjön meg arról, hogy hello server várakozási idő több mint egy percig.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Meghívásakor **OnMessage**, hello ügyfél elindul egy belső üzenet szivattyú, amely folyamatosan kérdezze le az hello várólista vagy az előfizetéshez. Az üzenet szivattyú által kiállított hívás tooreceive üzenetek végtelen hurkot tartalmaz. Hello hívás túllépi az időkorlátot, ha egy új hívás ad ki. hello időkorlátja hello hello értéke határozza meg [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) hello tulajdonságának [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)használt.

hello használatának előnye **OnMessage** képest túl**Receive** , hogy a felhasználóknak nem kell toomanually kérdezze le az üzenetek, kivételek kezelése, több üzenetet párhuzamosan feldolgozásához, és végezze el a hello üzenetek.

Ha meghívja a **Receive** nélkül használja az alapértelmezett értékét, lehet, hogy hello *ServerWaitTime* értéke nagyobb, mint egy perc. Beállítás *ServerWaitTime* toomore egy percnél megakadályozza, hogy a hello kiszolgáló köszönőüzenetei teljesen megérkezése előtt időtúllépés miatt.

### <a name="solution"></a>Megoldás
Tekintse meg a következő ajánlott módjait hitelesítésikód-példák hello. További részletekért lásd: [QueueClient.OnMessage metódus (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)és [QueueClient.Receive metódus (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

tooimprove hello teljesítményének hello Azure üzenetcsere infrastruktúra esetén, lásd: hello kialakításban [aszinkron üzenetkezelési ismertetése](https://msdn.microsoft.com/library/dn589781.aspx).

hello az alábbiakban látható egy példa segítségével **OnMessage** tooreceive üzeneteket.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if hello message-pump should call complete on messages after hello callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates hello maximum number of concurrent calls toohello callback hello pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you tooget notified of any errors encountered by hello message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates hello message pump and callback is invoked for each message that is recieved, calling close on hello client will stop hello pump.
    {
        // Process hello message.
    }, options);
    Console.WriteLine("Press any key tooexit.");
    Console.ReadKey();
```

hello az alábbiakban látható egy példa segítségével **Receive** hello alapértelmezett kiszolgálóval várakozási idő.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

hello az alábbiakban látható egy példa segítségével **Receive** egy nem alapértelmezett kiszolgálóval várakozási idő.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Érdemes lehet aszinkron Service Bus-metódusok
### <a name="id"></a>ID (Azonosító)
AP2003

### <a name="description"></a>Leírás
Aszinkron Service Bus módszerek tooimprove teljesítmény használata közvetítőalapú üzenettovábbítás.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Alkalmazás program egyidejű aszinkron metódusok használatával lehetővé teszi, mert minden hívás végrehajtása nem tiltja a hello fő szálnak. A Service Bus üzenetkezelés módszerek, egy olyan műveletet hajt használatakor (Küldés, kapni, törlés, stb.) időt vesz igénybe. Most hello Service Bus szolgáltatás által hello feldolgozási hello művelet szerepel továbbá toohello késését hello helykérelemmel és válasszal hello. műveletek másodpercenkénti idő tooincrease hello száma műveletek végre kell hajtani egyidejűleg. További információt lásd túl[gyakorlati tanácsok a teljesítmény javítását használatával Service Bus Közvetítőalapú üzenetkezelés](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Megoldás
Lásd: [QueueClient osztály (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) hogyan toouse hello ajánlott aszinkron metódus kapcsolatos információkat.

tooimprove hello teljesítményének hello Azure üzenetcsere infrastruktúra esetén, lásd: hello kialakításban [aszinkron üzenetkezelési ismertetése](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Vegye figyelembe a particionálási Service Bus-üzenetsorok és témakörök
### <a name="id"></a>ID (Azonosító)
AP2004

### <a name="description"></a>Leírás
Partíció Service Bus-üzenetsorok és témakörök a jobb teljesítmény érdekében a Service Bus üzenetkezelés.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Particionálás a Service Bus-üzenetsorok és témakörök növeli a teljesítményt átviteli sebesség és a szolgáltatás rendelkezésre állási, mivel a hello teljes átviteli sebességgel particionált üzenetsor vagy témakör már nem korlátozzák egyetlen üzenet broker vagy üzenetküldési tárolóban hello teljesítmény szerint. Ezenkívül átmenetileg nem működik az üzenetküldési tárolóban nem elérhetetlenné particionált üzenetsor vagy témakör. További információkért lásd: [üzenetküldési entitások particionálás](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Megoldás
a következő kódrészletben látható kód hogyan hello üzenetküldési entitások toopartition.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

További információkért lásd: [particionálva Service Bus-üzenetsorok és témakörök |} A Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) hello, valamint [Microsoft Azure Service Bus particionálva várólista](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) minta.

## <a name="do-not-set-sharedaccessstarttime"></a>Ne adja meg az SharedAccessStartTime
### <a name="id"></a>ID (Azonosító)
AP3001

### <a name="description"></a>Leírás
Ne SharedAccessStartTimeset toohello aktuális indításakor tooimmediately hello megosztott hozzáférési házirend használatával. Csak akkor kell tooset ezt a tulajdonságot, ha egy későbbi időpontban szeretné toostart hello megosztott hozzáférési házirend.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Számítógépóra-szinkronizálás adatközpontok között enyhe időeltérése okoz. Például úgy szeretné logikailag gondolja, hogy beállítás hello kezdési idejét a tároló SAS-házirend szerint hello aktuális idő DateTime.Now vagy egy hasonló módon, akkor hello SAS házirend tootake hatás azonnal. Azonban hello enyhe időeltérést adatközpont között problémákat okozhat a mivel lehet, hogy néhány datacenter eset némileg későbbi, mint hello kezdési ideje, míg mások azt előre. Ennek eredményeképpen hello SAS házirend lejárhat gyorsan (vagy akár azonnal) Ha hello házirend élettartam értéke túl rövid.

Az Azure storage közös hozzáférésű Jogosultságkód használatával további útmutatást lásd: [bevezetéséről tábla SAS (közös hozzáférésű Jogosultságkód), az üzenetsor SAS és a frissítés tooBlob SAS - Microsoft Azure tárolás fejlesztői Blog - hely kezdőlap - MSDN-blogok](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Megoldás
Távolítsa el a hello utasításon belül, amely beállítja a hello megosztott hozzáférési házirend hello kezdési idejét. hello Azure kód elemző eszközt biztosít a probléma egy javítást. Biztonságkezelés a további információkért lásd: hello kialakításban [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).

a következő kódrészletet hello hello kód a hiba javítása mutatja be.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Megosztott hozzáférési házirend lejárati idő több mint öt perc kell lennie.
### <a name="id"></a>ID (Azonosító)
AP3002

### <a name="description"></a>Leírás
Lehet, mint amennyit az adatközpontok különböző helyeken "óra eltérésére." néven ismert tooa feltétel miatt között órák eltérés öt perc tooprevent hello SAS házirend jogkivonat lejárjanak korábbi, mint a tervezett, állítsa be hello lejárati idő toobe több mint öt perc.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Hello világ különböző helyeken adatközpontok egy órajel szinkronizálni. Óra jel tootravel toodifferent helyeket időt vesz igénybe, mert lehet a különböző földrajzi helyen adatközpontokat közötti idő eltérés Bár minden támaszkodnak szinkronizálva. Ez eltérést hatással lehet a hello megosztott hozzáférési házirend kezdési időt és a lejárati időközt. Ezért tooensure megosztott hozzáférési házirend azonnal érvénybe lép, hello kezdete nem adja meg. Ezenkívül ellenőrizze, hogy hello lejárati idő több mint 5 perc tooprevent korai időtúllépés.

Az Azure storage közös hozzáférésű Jogosultságkód használatával kapcsolatos további információkért lásd: [bevezetéséről tábla SAS (közös hozzáférésű Jogosultságkód), az üzenetsor SAS és a frissítés tooBlob SAS - Microsoft Azure tárolás fejlesztői Blog - hely kezdőlap - MSDN-blogok](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Megoldás
Biztonságkezelés további információkért lásd: hello kialakításban [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).

hello az alábbiakban látható egy példa nem adja meg a megosztott hozzáférési házirend kezdő időpont.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

hello például a megosztott hozzáférési házirend kezdési idő megadása a öt percnél nagyobb házirend lejárati időtartam látható.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

További információkért lásd: [létrehozhat és használhat egy közös hozzáférésű Jogosultságkód](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>CloudConfigurationManager használata
### <a name="id"></a>ID (Azonosító)
AP4000

### <a name="description"></a>Leírás
Hello segítségével [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) projektek osztály, például az Azure Websites és az Azure mobile services nem vezetnek be futásidejű problémákat. Ajánlott eljárásként, célszerű egy jó ötlet toouse felhő[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) minden Azure felhőalapú alkalmazásokhoz konfigurációk kezelése egyesített módja.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
CloudConfigurationManager hello konfigurációs fájl megfelelő toohello alkalmazás környezetet olvassa be.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Megoldás
A kód toouse hello refactor [CloudConfigurationManager osztály](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Hiba a kód javítása hello Azure kód elemző eszköz által biztosított.

a következő kódrészletet hello hello kód a hiba javítása mutatja be. Csere

`var settings = ConfigurationManager.AppSettings["mySettings"];`

a

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Íme egy példa hogyan toostore hello konfigurációs beállítás egy App.config vagy a Web.config fájlban. Adja hozzá a hello beállítások toohello appSettings szakaszt hello konfigurációs fájl. hello az alábbiakban az előző példakódban hello hello Web.config fájlt.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Kerülje a kódolt kapcsolati karakterláncok
### <a name="id"></a>ID (Azonosító)
AP4001

### <a name="description"></a>Leírás
Ha a kódolt kapcsolati karakterláncokat használ, és tooupdate kell őket, később lesz toomake módosítások tooyour forráskód rendelkezik, és fordítsa újra hello alkalmazást. Azonban ha egy konfigurációs fájlban tárolja a kapcsolati karakterláncokat, később bármikor módosíthatja azokat hello konfigurációs fájl egyszerűen frissítésével.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Rögzített megadás kapcsolati karakterláncok a hibás célszerű, mert problémák amikor kapcsolati karakterláncok toobe gyorsan módosítani kell. Ezenkívül ha hello projekt toobe toosource vezérlőben be van jelölve, kódolt kapcsolati karakterláncok vezethet biztonsági rések óta hello karakterláncok hello forráskód lehet megtekinteni.

### <a name="solution"></a>Megoldás
Kapcsolati karakterláncok tárolni hello konfigurációs fájlok vagy az Azure környezetben.

* Önálló alkalmazások esetén használja az app.config toostore kapcsolatikarakterlánc-beállításokat.
* IIS kiszolgálón futó webes alkalmazásokhoz használja a web.config toostore kapcsolati karakterláncokat.
* Az ASP.NET-alkalmazások vNext használja a configuration.json toostore kapcsolati karakterláncokat.

Konfigurációk fájlok – például a web.config vagy az App.config fájlt használatáról információkért lásd: [ASP.NET webes beállítási útmutatója](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx). Az Azure környezeti változók munkahelyi információkért lásd: [webhelyek Azure: hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok működik](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). A kapcsolati karakterlánc tárolása verziókezelő információkért lásd: [ne tegye a bizalmas adatokat például kapcsolati karakterláncok forráskódraktárban tárolt fájlok](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Diagnosztika konfigurációs fájl használata
### <a name="id"></a>ID (Azonosító)
AP5000

### <a name="description"></a>Leírás
Diagnosztikai beállítások konfigurálására a kódban, többek között a hello API programozási Microsoft.WindowsAzure.Diagnostics, hello diagnostics.wadcfg fájlban konfigurálni kell diagnosztika beállításait. (Vagy, ha Azure SDK 2.5 diagnostics.wadcfgx). Ezzel az eljárással diagnosztikai beállítások módosításával anélkül, hogy toorecompile a kódot.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
Mielőtt az Azure SDK 2.5-ös (amely az Azure diagnostics 1.3), Azure Diagnostics (ÜVEGVATTA) sikerült konfigurálni a számos különböző módszer használatával: hozzáadná toohello konfigurációs blob Storage, feltétlenül szükséges kódot, deklaratív konfigurációs vagy hello alapértelmezett használatával konfiguráció. Azonban hello elsődleges módon tooconfigure diagnosztika toouse egy XML-konfigurációs fájl (diagnostics.wadcfg vagy diagnositcs.wadcfgx SDK 2.5 és újabb) hello alkalmazás projektben. Ez a megközelítés a hello diagnostics.wadcfg fájl teljesen definiálja a konfigurációt hello és frissíthető és újra telepíteni fogja a. Hello diagnostics.wadcfg konfigurációs fájljának hello használatát keverése hello programozási módszerek konfigurációk hello segítségével állítjuk [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)vagy [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx) osztályok tooconfusion vezethet. Lásd: [inicializálása vagy Azure Diagnostics konfigurációjának módosítása](https://msdn.microsoft.com/library/azure/hh411537.aspx) további információt.

(Az Azure SDK 2.5 része) ÜVEGVATTA 1.3 kezdve már nem lehetséges toouse kód tooconfigure diagnosztika. Ennek eredményeképpen csak biztosíthat hello konfiguráció alkalmazása vagy hello diagnosztika bővítmény frissítése.

### <a name="solution"></a>Megoldás
Hello diagnosztika configuration designer toomove diagnosztikai beállítások toohello diagnosztika konfigurációs fájlt használja (diagnositcs.wadcfg vagy diagnositcs.wadcfgx SDK 2.5 és újabb). Emellett ajánlott telepíteni [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) hello legújabb diagnosztikai funkciót használja.

1. A megjeleníteni kívánt tooconfigure hello szerepkör hello helyi menüben kattintson a Tulajdonságok parancsra, és válassza a hello konfiguráció lapon.
2. A hello **diagnosztika** területen győződjön meg arról, hogy hello **engedélyezése diagnosztikai** jelölőnégyzet be van jelölve.
3. Válassza ki a hello **konfigurálása** gombra.

   ![Hello diagnosztika engedélyezése beállítás elérése](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   Lásd: [diagnosztika konfigurálása az Azure Cloud Services és a virtuális gépek](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) további információt.

## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Kerülje a DbContext objektumokat statikus deklaráló
### <a name="id"></a>ID (Azonosító)
AP6000

### <a name="description"></a>Leírás
toosave memória elkerülése deklaráló statikus DBContext objektumokat.

Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Ok
DBContext objektumok tárolására hello minden hívás lekérdezés eredményei. Statikus DBContext objektumok nem értékesítik, amíg a hello alkalmazástartomány eltávolítva. Ezért a statikus DBContext objektum nagy mennyiségű memóriát is felhasználhatnak.

### <a name="solution"></a>Megoldás
DBContext deklarálható lokális változó vagy nem statikus mezőn, a feladat használni, és engedélyezze azt követően ártalmatlanítani.

a következő példa MVC-vezérlő osztályhoz hello bemutatja, hogyan toouse hello DBContext objektum.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass tooDbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Következő lépések
További információ az optimalizálás, és az Azure-alkalmazások esetén hibaelhárítási toolearn lásd [hibaelhárítása a webes alkalmazás az Azure App Service szolgáltatásban a Visual Studio használatával](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
