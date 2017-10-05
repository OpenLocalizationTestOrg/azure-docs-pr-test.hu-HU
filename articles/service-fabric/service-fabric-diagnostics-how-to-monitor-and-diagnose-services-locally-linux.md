---
title: "A Linux Azure mikroszolgáltatások hibakeresése |} Microsoft Docs"
description: "Megtudhatja, hogyan figyelése és diagnosztizálása a szolgáltatások egy helyi fejlesztési gépen a Microsoft Azure Service Fabric használatával készítettek."
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
ms.openlocfilehash: 4bc73f581f4855ebc724df19dd56fab8bf103854
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a><span data-ttu-id="1fb0a-103">Figyelése és diagnosztizálása szolgáltatásai a helyi számítógép fejlesztési beállítása</span><span class="sxs-lookup"><span data-stu-id="1fb0a-103">Monitor and diagnose services in a local machine development setup</span></span>


> [!div class="op_single_selector"]
> * [<span data-ttu-id="1fb0a-104">Windows</span><span class="sxs-lookup"><span data-stu-id="1fb0a-104">Windows</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [<span data-ttu-id="1fb0a-105">Linux</span><span class="sxs-lookup"><span data-stu-id="1fb0a-105">Linux</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

<span data-ttu-id="1fb0a-106">Figyelés, észlelésére, diagnosztizálása és hibaelhárítási lehetővé teszi a felhasználói élmény minimális megszakadását folytatásához szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-106">Monitoring, detecting, diagnosing, and troubleshooting allow for services to continue with minimal disruption to the user experience.</span></span> <span data-ttu-id="1fb0a-107">Megfigyelési és diagnosztikai kritikusak tényleges telepített éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-107">Monitoring and diagnostics are critical in an actual deployed production environment.</span></span> <span data-ttu-id="1fb0a-108">Egy hasonló modell bevezetése során a szolgáltatások fejlesztéséhez biztosítja, hogy a diagnosztikai csővezeték működik-e éles környezetben való áthelyezésekor.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-108">Adopting a similar model during development of services ensures that the diagnostic pipeline works when you move to a production environment.</span></span> <span data-ttu-id="1fb0a-109">A Service Fabric megkönnyíti a szolgáltatás fejlesztők számára, amely zökkenőmentesen használható legyen a helyi fejlesztési egyszámítógépes beállításokat és a valós üzemi fürt beállítások diagnosztika megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-109">Service Fabric makes it easy for service developers to implement diagnostics that can seamlessly work across both single-machine local development setups and real-world production cluster setups.</span></span>


## <a name="debugging-service-fabric-java-applications"></a><span data-ttu-id="1fb0a-110">Service Fabric Java-alkalmazások hibakeresés</span><span class="sxs-lookup"><span data-stu-id="1fb0a-110">Debugging Service Fabric Java applications</span></span>

<span data-ttu-id="1fb0a-111">Java-alkalmazások [több naplózási keretrendszer](http://en.wikipedia.org/wiki/Java_logging_framework) érhetők el.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-111">For Java applications, [multiple logging frameworks](http://en.wikipedia.org/wiki/Java_logging_framework) are available.</span></span> <span data-ttu-id="1fb0a-112">Mivel a `java.util.logging` az alapértelmezett beállítás a JRE együtt is használható a [github-kódpéldák](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1fb0a-112">Since `java.util.logging` is the default option with the JRE, it is also used for the [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  <span data-ttu-id="1fb0a-113">A következő ismertető ismerteti, hogyan konfigurálhatja a `java.util.logging` keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-113">The following discussion explains how to configure the `java.util.logging` framework.</span></span>

<span data-ttu-id="1fb0a-114">Java.util.logging használatával is átirányítja az alkalmazásnaplók memória, a Kimeneti folyamok, a konzol fájlok vagy a sockets.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-114">Using java.util.logging you can redirect your application logs to memory, output streams, console files, or sockets.</span></span> <span data-ttu-id="1fb0a-115">Mindkét beállítás vonatkoznak-e már szerepel a keretrendszer alapértelmezett kezelők.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-115">For each of these options, there are default handlers already provided in the framework.</span></span> <span data-ttu-id="1fb0a-116">Létrehozhat egy `app.properties` fájl konfigurálása a fájlkezelő összes napló átirányítása egy helyi fájl az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-116">You can create a `app.properties` file to configure the file handler for your application to redirect all logs to a local file.</span></span>

<span data-ttu-id="1fb0a-117">A következő kódrészlet egy példa konfiguráció tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1fb0a-117">The following code snippet contains an example configuration:</span></span>

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

<span data-ttu-id="1fb0a-118">A mappa által hivatkozott a `app.properties` fájlnak léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-118">The folder pointed to by the `app.properties` file must exist.</span></span> <span data-ttu-id="1fb0a-119">Után a `app.properties` jön létre, a belépési pont parancsfájlt is módosítani kell `entrypoint.sh` a a `<applicationfolder>/<servicePkg>/Code/` mappa a tulajdonság beállítása `java.util.logging.config.file` való `app.propertes` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-119">After the `app.properties` file is created, you need to also modify your entry point script, `entrypoint.sh` in the `<applicationfolder>/<servicePkg>/Code/` folder to set the property `java.util.logging.config.file` to `app.propertes` file.</span></span> <span data-ttu-id="1fb0a-120">A bejegyzés a következő kódrészletet hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="1fb0a-120">The entry should look like the following snippet:</span></span>

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


<span data-ttu-id="1fb0a-121">Ez a konfiguráció eredményez, egy forgó módon gyűjtött naplók `/tmp/servicefabric/logs/`.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-121">This configuration results in logs being collected in a rotating fashion at `/tmp/servicefabric/logs/`.</span></span> <span data-ttu-id="1fb0a-122">A naplófájl ebben az esetben nevű mysfapp%u.%g.log ahol:</span><span class="sxs-lookup"><span data-stu-id="1fb0a-122">The log file in this case is named mysfapp%u.%g.log where:</span></span>
* <span data-ttu-id="1fb0a-123">**%u** egyidejű Java-folyamatai közötti ütközések feloldása egyedi szám.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-123">**%u** is a unique number to resolve conflicts between simultaneous Java processes.</span></span>
* <span data-ttu-id="1fb0a-124">**%g** naplók elforgatása megkülönböztetésére generációs száma.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-124">**%g** is the generation number to distinguish between rotating logs.</span></span>

<span data-ttu-id="1fb0a-125">Alapértelmezés szerint ha leíróval explicit módon van konfigurálva, a konzol kezelő regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-125">By default if no handler is explicitly configured, the console handler is registered.</span></span> <span data-ttu-id="1fb0a-126">A naplók /var/log/syslog a syslog egy tekinthető meg.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-126">One can view the logs in syslog under /var/log/syslog.</span></span>

<span data-ttu-id="1fb0a-127">További információkért lásd: a [github-kódpéldák](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1fb0a-127">For more information, see the [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  


## <a name="debugging-service-fabric-c-applications"></a><span data-ttu-id="1fb0a-128">Service Fabric C# alkalmazások hibakeresése</span><span class="sxs-lookup"><span data-stu-id="1fb0a-128">Debugging Service Fabric C# applications</span></span>


<span data-ttu-id="1fb0a-129">Több elérhetők keretrendszerek CoreCLR alkalmazások Linux nyomkövetés.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-129">Multiple frameworks are available for tracing CoreCLR applications on Linux.</span></span> <span data-ttu-id="1fb0a-130">További információkért lásd: [GitHub: naplózás](http:/github.com/aspnet/logging).</span><span class="sxs-lookup"><span data-stu-id="1fb0a-130">For more information, see [GitHub: logging](http:/github.com/aspnet/logging).</span></span>  <span data-ttu-id="1fb0a-131">Mivel a EventSource ismerős a C#-fejlesztők számára "Ebben a cikkben az EventSource a nyomkövetés a Linux CoreCLR mintában.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-131">Since EventSource is familiar to C# developers,\`this article uses EventSource for tracing in CoreCLR samples on Linux.</span></span>

<span data-ttu-id="1fb0a-132">Az első lépés annak System.Diagnostics.Tracing tartalmazza, így a memória, a Kimeneti folyamok vagy a konzol fájlok írhat a naplók.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-132">The first step is to include System.Diagnostics.Tracing so that you can write your logs to memory, output streams, or console files.</span></span>  <span data-ttu-id="1fb0a-133">Az EventSource naplózni, adja hozzá a következő projekt a project.json:</span><span class="sxs-lookup"><span data-stu-id="1fb0a-133">For logging using EventSource, add the following project to your project.json:</span></span>

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

<span data-ttu-id="1fb0a-134">Egy egyéni eseményfigyelő Visszahívásán segítségével a szolgáltatás esemény figyeli, és ezután megfelelően átirányítja őket a nyomkövetési fájlok.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-134">You can use a custom EventListener to listen for the service event and then appropriately redirect them to trace files.</span></span> <span data-ttu-id="1fb0a-135">A következő kódrészletet a minta megvalósításának naplózás EventSource és egy egyéni eseményfigyelő Visszahívásán jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="1fb0a-135">The following code snippet shows a sample implementation of logging using EventSource and a custom EventListener:</span></span>


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

        // TBD: Need to add method for sample event.

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


<span data-ttu-id="1fb0a-136">Az előző részlet kimenete egy fájlra a naplók `/tmp/MyServiceLog.txt`.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-136">The preceding snippet outputs the logs to a file in `/tmp/MyServiceLog.txt`.</span></span> <span data-ttu-id="1fb0a-137">Ezt a fájlnevet megfelelően frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-137">This file name needs to be appropriately updated.</span></span> <span data-ttu-id="1fb0a-138">Abban az esetben, ha át szeretné irányítani a naplókat a konzolon, a következő kódrészletet használja a testreszabott eseményfigyelő Visszahívásán osztályban:</span><span class="sxs-lookup"><span data-stu-id="1fb0a-138">In case you want to redirect the logs to console, use the following snippet in your customized EventListener class:</span></span>

```csharp
public static TextWriter Out = Console.Out;
```

<span data-ttu-id="1fb0a-139">A minták [C# minták](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) EventSource és egy egyéni eseményfigyelő Visszahívásán naplózza az eseményeket egy fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-139">The samples at [C# Samples](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) use EventSource and a custom EventListener to log events to a file.</span></span>



## <a name="next-steps"></a><span data-ttu-id="1fb0a-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fb0a-140">Next steps</span></span>
<span data-ttu-id="1fb0a-141">A diagnosztika az alkalmazás egy Azure fürt ugyanazt a nyomkövetés kódot az alkalmazásba felvett is működik.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-141">The same tracing code added to your application also works with the diagnostics of your application on an Azure cluster.</span></span> <span data-ttu-id="1fb0a-142">Tekintse meg a cikkek ismertetik az eszközök a különböző lehetőségek közül, és bemutatják, hogyan állíthatja be azokat.</span><span class="sxs-lookup"><span data-stu-id="1fb0a-142">Check out these articles that discuss the different options for the tools and describe how to set them up.</span></span>
* [<span data-ttu-id="1fb0a-143">Az Azure diagnosztikai naplók gyűjtéséről</span><span class="sxs-lookup"><span data-stu-id="1fb0a-143">How to collect logs with Azure Diagnostics</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
