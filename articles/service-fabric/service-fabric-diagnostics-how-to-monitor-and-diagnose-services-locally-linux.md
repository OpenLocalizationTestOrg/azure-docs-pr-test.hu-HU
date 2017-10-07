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
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Figyelése és diagnosztizálása szolgáltatásai a helyi számítógép fejlesztési beállítása


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

Figyelés, észlelésére, diagnosztizálása és hibaelhárítási lehetővé teszi a minimális megszakítás toohello felhasználói élmény szolgáltatások toocontinue. Megfigyelési és diagnosztikai kritikusak tényleges telepített éles környezetben. Hasonló modell bevezetése közben a szolgáltatások fejlesztéséhez biztosítja, hogy hello diagnosztikai csővezeték működik tooa éles környezetben áthelyezésekor. A Service Fabric megkönnyíti a szolgáltatás a fejlesztők tooimplement diagnosztikai, amely zökkenőmentesen használható legyen a helyi fejlesztési egyszámítógépes beállításokat és a valós üzemi fürt beállításokat.


## <a name="debugging-service-fabric-java-applications"></a>Service Fabric Java-alkalmazások hibakeresés

Java-alkalmazások [több naplózási keretrendszer](http://en.wikipedia.org/wiki/Java_logging_framework) érhetők el. Mivel a `java.util.logging` hello alapértelmezett beállítás a hello JRE, is használható hello [github-kódpéldák](http://github.com/Azure-Samples/service-fabric-java-getting-started).  hello következő vitafórum azt ismerteti, hogyan tooconfigure hello `java.util.logging` keretrendszer.

Toomemory, a Kimeneti folyamok, a konzol fájlok vagy a sockets naplók java.util.logging átirányíthatja az alkalmazás segítségével. Mindkét beállítás vonatkoznak-e már szerepel a hello keretrendszer alapértelmezett kezelők. Létrehozhat egy `app.properties` fájl tooconfigure hello fájlkezelő a az alkalmazás összes tooredirect tooa helyi fájl naplózza.

a következő kódrészletet hello egy példa konfiguráció tartalmazza:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

hello mappa hegyes tooby hello `app.properties` fájlnak léteznie kell. Hello után `app.properties` jön létre, tooalso kell módosítani a belépési pont parancsfájl `entrypoint.sh` a hello `<applicationfolder>/<servicePkg>/Code/` mappa tooset hello tulajdonság `java.util.logging.config.file` túl`app.propertes` fájl. hello bejegyzés a következő kódrészletet hello hasonlóan kell kinéznie:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path tooapp.properties> -jar <service name>.jar
```


Ez a konfiguráció eredményez, egy forgó módon gyűjtött naplók `/tmp/servicefabric/logs/`. hello naplófájl ebben az esetben nevű mysfapp%u.%g.log ahol:
* **%u** van egy egyedi szám tooresolve egyidejű Java-folyamatai közötti ütközések.
* **%g** hello generációs számú toodistinguish naplók elforgatása között van.

Alapértelmezés szerint ha leíróval explicit módon van konfigurálva, hello konzol kezelő regisztrálva van. Hello naplók /var/log/syslog a syslog egy tekinthető meg.

További információkért lásd: hello [github-kódpéldák](http://github.com/Azure-Samples/service-fabric-java-getting-started).  


## <a name="debugging-service-fabric-c-applications"></a>Service Fabric C# alkalmazások hibakeresése


Több elérhetők keretrendszerek CoreCLR alkalmazások Linux nyomkövetés. További információkért lásd: [GitHub: naplózás](http:/github.com/aspnet/logging).  Mivel EventSource megszokott tooC #-fejlesztők számára "Ebben a cikkben az EventSource a nyomkövetés a Linux CoreCLR mintában.

hello első lépéseként tooinclude System.Diagnostics.Tracing, úgy, hogy a naplók toomemory, Kimeneti folyamok vagy konzol fájlok írhat.  Naplózás EventSource használ, adja hozzá a következő projekt tooyour project.json hello:

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

Használjon egy egyéni eseményfigyelő Visszahívásán toolisten hello szolgáltatás eseményhez, és majd megfelelően átirányítja őket tootrace fájlokat. hello következő kódrészletet mutatja egy EventSource és egy egyéni eseményfigyelő Visszahívásán naplózási minta megvalósításában:


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


hello előző részlet kimenete hello naplók tooa fájl `/tmp/MyServiceLog.txt`. Ezt a fájlnevet toobe megfelelően frissíteni kell. Abban az esetben, ha azt szeretné, hogy tooredirect hello naplók tooconsole, használja a következő kódrészletet a testreszabott eseményfigyelő Visszahívásán osztályban hello:

```csharp
public static TextWriter Out = Console.Out;
```

minták hello [C# minták](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) EventSource és egy egyéni eseményfigyelő Visszahívásán toolog események tooa fájlt használja.



## <a name="next-steps"></a>Következő lépések
hello felvett azonos nyomkövetési kód tooyour alkalmazás is együttműködik az alkalmazás Azure fürtökön hello diagnosztika. Ezek a cikkek, amely különböző lehetőségeket hello hello eszközök tárgyalja, és leírja kivételét hogyan tooset be őket.
* [Hogyan toocollect naplózza az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md)
