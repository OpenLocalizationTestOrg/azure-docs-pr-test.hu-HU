---
title: "a Linux Azure mikroszolgáltatások aaaDebug |} Microsoft Docs"
description: "Megtudhatja, hogyan toomonitor és diagnosztizálhatja a szolgáltatások egy helyi fejlesztési gépen a Microsoft Azure Service Fabric használatával készítettek."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 4eebe937-ab42-4429-93db-f35c26424321
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: bee47bbabcf6b84ff2da14079e026529e36a198b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a><span data-ttu-id="30e19-103">Figyelése és diagnosztizálása szolgáltatásai a helyi számítógép fejlesztési beállítása</span><span class="sxs-lookup"><span data-stu-id="30e19-103">Monitor and diagnose services in a local machine development setup</span></span>


> [!div class="op_single_selector"]
> * [<span data-ttu-id="30e19-104">Windows</span><span class="sxs-lookup"><span data-stu-id="30e19-104">Windows</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [<span data-ttu-id="30e19-105">Linux</span><span class="sxs-lookup"><span data-stu-id="30e19-105">Linux</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

<span data-ttu-id="30e19-106">Figyelés, észlelésére, diagnosztizálása és hibaelhárítási lehetővé teszi a minimális megszakítás toohello felhasználói élmény szolgáltatások toocontinue.</span><span class="sxs-lookup"><span data-stu-id="30e19-106">Monitoring, detecting, diagnosing, and troubleshooting allow for services toocontinue with minimal disruption toohello user experience.</span></span> <span data-ttu-id="30e19-107">Megfigyelési és diagnosztikai kritikusak tényleges telepített éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="30e19-107">Monitoring and diagnostics are critical in an actual deployed production environment.</span></span> <span data-ttu-id="30e19-108">Hasonló modell bevezetése közben a szolgáltatások fejlesztéséhez biztosítja, hogy hello diagnosztikai csővezeték működik tooa éles környezetben áthelyezésekor.</span><span class="sxs-lookup"><span data-stu-id="30e19-108">Adopting a similar model during development of services ensures that hello diagnostic pipeline works when you move tooa production environment.</span></span> <span data-ttu-id="30e19-109">A Service Fabric megkönnyíti a szolgáltatás a fejlesztők tooimplement diagnosztikai, amely zökkenőmentesen használható legyen a helyi fejlesztési egyszámítógépes beállításokat és a valós üzemi fürt beállításokat.</span><span class="sxs-lookup"><span data-stu-id="30e19-109">Service Fabric makes it easy for service developers tooimplement diagnostics that can seamlessly work across both single-machine local development setups and real-world production cluster setups.</span></span>


## <a name="debugging-service-fabric-java-applications"></a><span data-ttu-id="30e19-110">Service Fabric Java-alkalmazások hibakeresés</span><span class="sxs-lookup"><span data-stu-id="30e19-110">Debugging Service Fabric Java applications</span></span>

<span data-ttu-id="30e19-111">Java-alkalmazások [több naplózási keretrendszer](http://en.wikipedia.org/wiki/Java_logging_framework) érhetők el.</span><span class="sxs-lookup"><span data-stu-id="30e19-111">For Java applications, [multiple logging frameworks](http://en.wikipedia.org/wiki/Java_logging_framework) are available.</span></span> <span data-ttu-id="30e19-112">Mivel a `java.util.logging` hello alapértelmezett beállítás a hello JRE, is használható hello [github-kódpéldák](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="30e19-112">Since `java.util.logging` is hello default option with hello JRE, it is also used for hello [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  <span data-ttu-id="30e19-113">hello következő vitafórum azt ismerteti, hogyan tooconfigure hello `java.util.logging` keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="30e19-113">hello following discussion explains how tooconfigure hello `java.util.logging` framework.</span></span>

<span data-ttu-id="30e19-114">Toomemory, a Kimeneti folyamok, a konzol fájlok vagy a sockets naplók java.util.logging átirányíthatja az alkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="30e19-114">Using java.util.logging you can redirect your application logs toomemory, output streams, console files, or sockets.</span></span> <span data-ttu-id="30e19-115">Mindkét beállítás vonatkoznak-e már szerepel a hello keretrendszer alapértelmezett kezelők.</span><span class="sxs-lookup"><span data-stu-id="30e19-115">For each of these options, there are default handlers already provided in hello framework.</span></span> <span data-ttu-id="30e19-116">Létrehozhat egy `app.properties` fájl tooconfigure hello fájlkezelő a az alkalmazás összes tooredirect tooa helyi fájl naplózza.</span><span class="sxs-lookup"><span data-stu-id="30e19-116">You can create a `app.properties` file tooconfigure hello file handler for your application tooredirect all logs tooa local file.</span></span>

<span data-ttu-id="30e19-117">a következő kódrészletet hello egy példa konfiguráció tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="30e19-117">hello following code snippet contains an example configuration:</span></span>

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

<span data-ttu-id="30e19-118">hello mappa hegyes tooby hello `app.properties` fájlnak léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="30e19-118">hello folder pointed tooby hello `app.properties` file must exist.</span></span> <span data-ttu-id="30e19-119">Hello után `app.properties` jön létre, tooalso kell módosítani a belépési pont parancsfájl `entrypoint.sh` a hello `<applicationfolder>/<servicePkg>/Code/` mappa tooset hello tulajdonság `java.util.logging.config.file` túl`app.propertes` fájl.</span><span class="sxs-lookup"><span data-stu-id="30e19-119">After hello `app.properties` file is created, you need tooalso modify your entry point script, `entrypoint.sh` in hello `<applicationfolder>/<servicePkg>/Code/` folder tooset hello property `java.util.logging.config.file` too`app.propertes` file.</span></span> <span data-ttu-id="30e19-120">hello bejegyzés a következő kódrészletet hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="30e19-120">hello entry should look like hello following snippet:</span></span>

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path tooapp.properties> -jar <service name>.jar
```


<span data-ttu-id="30e19-121">Ez a konfiguráció eredményez, egy forgó módon gyűjtött naplók `/tmp/servicefabric/logs/`.</span><span class="sxs-lookup"><span data-stu-id="30e19-121">This configuration results in logs being collected in a rotating fashion at `/tmp/servicefabric/logs/`.</span></span> <span data-ttu-id="30e19-122">hello naplófájl ebben az esetben nevű mysfapp%u.%g.log ahol:</span><span class="sxs-lookup"><span data-stu-id="30e19-122">hello log file in this case is named mysfapp%u.%g.log where:</span></span>
* <span data-ttu-id="30e19-123">**%u** van egy egyedi szám tooresolve egyidejű Java-folyamatai közötti ütközések.</span><span class="sxs-lookup"><span data-stu-id="30e19-123">**%u** is a unique number tooresolve conflicts between simultaneous Java processes.</span></span>
* <span data-ttu-id="30e19-124">**%g** hello generációs számú toodistinguish naplók elforgatása között van.</span><span class="sxs-lookup"><span data-stu-id="30e19-124">**%g** is hello generation number toodistinguish between rotating logs.</span></span>

<span data-ttu-id="30e19-125">Alapértelmezés szerint ha leíróval explicit módon van konfigurálva, hello konzol kezelő regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="30e19-125">By default if no handler is explicitly configured, hello console handler is registered.</span></span> <span data-ttu-id="30e19-126">Hello naplók /var/log/syslog a syslog egy tekinthető meg.</span><span class="sxs-lookup"><span data-stu-id="30e19-126">One can view hello logs in syslog under /var/log/syslog.</span></span>

<span data-ttu-id="30e19-127">További információkért lásd: hello [github-kódpéldák](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="30e19-127">For more information, see hello [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  


## <a name="debugging-service-fabric-c-applications"></a><span data-ttu-id="30e19-128">Service Fabric C# alkalmazások hibakeresése</span><span class="sxs-lookup"><span data-stu-id="30e19-128">Debugging Service Fabric C# applications</span></span>


<span data-ttu-id="30e19-129">Több elérhetők keretrendszerek CoreCLR alkalmazások Linux nyomkövetés.</span><span class="sxs-lookup"><span data-stu-id="30e19-129">Multiple frameworks are available for tracing CoreCLR applications on Linux.</span></span> <span data-ttu-id="30e19-130">További információkért lásd: [GitHub: naplózás](http:/github.com/aspnet/logging).</span><span class="sxs-lookup"><span data-stu-id="30e19-130">For more information, see [GitHub: logging](http:/github.com/aspnet/logging).</span></span>  <span data-ttu-id="30e19-131">Mivel EventSource megszokott tooC #-fejlesztők számára "Ebben a cikkben az EventSource a nyomkövetés a Linux CoreCLR mintában.</span><span class="sxs-lookup"><span data-stu-id="30e19-131">Since EventSource is familiar tooC# developers,\`this article uses EventSource for tracing in CoreCLR samples on Linux.</span></span>

<span data-ttu-id="30e19-132">hello első lépéseként tooinclude System.Diagnostics.Tracing, úgy, hogy a naplók toomemory, Kimeneti folyamok vagy konzol fájlok írhat.</span><span class="sxs-lookup"><span data-stu-id="30e19-132">hello first step is tooinclude System.Diagnostics.Tracing so that you can write your logs toomemory, output streams, or console files.</span></span>  <span data-ttu-id="30e19-133">Naplózás EventSource használ, adja hozzá a következő projekt tooyour project.json hello:</span><span class="sxs-lookup"><span data-stu-id="30e19-133">For logging using EventSource, add hello following project tooyour project.json:</span></span>

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

<span data-ttu-id="30e19-134">Használjon egy egyéni eseményfigyelő Visszahívásán toolisten hello szolgáltatás eseményhez, és majd megfelelően átirányítja őket tootrace fájlokat.</span><span class="sxs-lookup"><span data-stu-id="30e19-134">You can use a custom EventListener toolisten for hello service event and then appropriately redirect them tootrace files.</span></span> <span data-ttu-id="30e19-135">hello következő kódrészletet mutatja egy EventSource és egy egyéni eseményfigyelő Visszahívásán naplózási minta megvalósításában:</span><span class="sxs-lookup"><span data-stu-id="30e19-135">hello following code snippet shows a sample implementation of logging using EventSource and a custom EventListener:</span></span>


```csharp

 public class ServiceEventSource : EventSource
 {
        public static ServiceEventSource Current = new ServiceEventSource();

        [NonEvent]
        public void Message(string message, params object[] args)
        {
            if (this.IsEnabled())
            {
                var finalMessage = string.Format(message, args);
                this.Message(finalMessage);
            }
        }

        // TBD: Need tooadd method for sample event.

}

```


```csharp
   internal class ServiceEventListener : EventListener
   {

        protected override void OnEventSourceCreated(EventSource eventSource)
        {
            EnableEvents(eventSource, EventLevel.LogAlways, EventKeywords.All);
        }
        protected override void OnEventWritten(EventWrittenEventArgs eventData)
        {
            using (StreamWriter Out = new StreamWriter( new FileStream("/tmp/MyServiceLog.txt", FileMode.Append)))           
        { 
                 // report all event information               
         Out.Write(" {0} ",  Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
                if (eventData.Message != null)              
            Out.WriteLine(eventData.Message, eventData.Payload.ToArray());              
            else             
        { 
                    string[] sargs = eventData.Payload != null ? eventData.Payload.Select(o => o.ToString()).ToArray() : null; 
                    Out.WriteLine("({0}).", sargs != null ? string.Join(", ", sargs) : "");             
        }
           }
        }
    }
```


<span data-ttu-id="30e19-136">hello előző részlet kimenete hello naplók tooa fájl `/tmp/MyServiceLog.txt`.</span><span class="sxs-lookup"><span data-stu-id="30e19-136">hello preceding snippet outputs hello logs tooa file in `/tmp/MyServiceLog.txt`.</span></span> <span data-ttu-id="30e19-137">Ezt a fájlnevet toobe megfelelően frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="30e19-137">This file name needs toobe appropriately updated.</span></span> <span data-ttu-id="30e19-138">Abban az esetben, ha azt szeretné, hogy tooredirect hello naplók tooconsole, használja a következő kódrészletet a testreszabott eseményfigyelő Visszahívásán osztályban hello:</span><span class="sxs-lookup"><span data-stu-id="30e19-138">In case you want tooredirect hello logs tooconsole, use hello following snippet in your customized EventListener class:</span></span>

```csharp
public static TextWriter Out = Console.Out;
```

<span data-ttu-id="30e19-139">minták hello [C# minták](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) EventSource és egy egyéni eseményfigyelő Visszahívásán toolog események tooa fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="30e19-139">hello samples at [C# Samples](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) use EventSource and a custom EventListener toolog events tooa file.</span></span>



## <a name="next-steps"></a><span data-ttu-id="30e19-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30e19-140">Next steps</span></span>
<span data-ttu-id="30e19-141">hello felvett azonos nyomkövetési kód tooyour alkalmazás is együttműködik az alkalmazás Azure fürtökön hello diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="30e19-141">hello same tracing code added tooyour application also works with hello diagnostics of your application on an Azure cluster.</span></span> <span data-ttu-id="30e19-142">Ezek a cikkek, amely különböző lehetőségeket hello hello eszközök tárgyalja, és leírja kivételét hogyan tooset be őket.</span><span class="sxs-lookup"><span data-stu-id="30e19-142">Check out these articles that discuss hello different options for hello tools and describe how tooset them up.</span></span>
* [<span data-ttu-id="30e19-143">Hogyan toocollect naplózza az Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="30e19-143">How toocollect logs with Azure Diagnostics</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
