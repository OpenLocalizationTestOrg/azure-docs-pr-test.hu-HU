---
title: "aaaScheduled események Azure metaadatok szolgáltatással |} Microsoft Docs"
description: "Ahhoz, azok fordulhat elő, reagálni tooImpactful események a virtuális gépen."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/10/2016
ms.author: zivr
ms.openlocfilehash: b5c0849958c3ab48fa9c2cbff7db62f2e9d7daf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service---scheduled-events-preview"></a>Az Azure metaadat-szolgáltatás - ütemezett események (előzetes verzió)

> [!NOTE] 
> Az előzetes verziójú funkciók elérhető tooyou hello feltétel, hogy elfogadja a használati feltételek toohello készülnek el. További részletekért lásd: [Kiegészítő használati feltételek a Microsoft Azure előzetes verziójú termékeihez](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).
>

Ütemezett események egyike hello subservices hello Azure metaadat-szolgáltatás alatt. Felelős felszínre hozza a jövőbeni események kapcsolatos információk (például újraindítás), az alkalmazás előkészítése őket, és korlátozza megszakítása. Érhető el minden Azure virtuális gép esetében, beleértve a PaaS és IaaS. Ütemezett események biztosít a virtuális gép idő tooperform megelőző feladatok toominimize hello hatásának egy eseményt. 

## <a name="introduction---why-scheduled-events"></a>Bevezetés - miért ütemezett eseményeket?

Ütemezett események lépéseket toolimit hello hatása platform-intiated karbantartás vagy felhasználó által kezdeményezett műveletek a szolgáltatás. 

Többpéldányos munkaterhelések esetén, amelyek replikációs technikák toomaintain állapota, lehet, hogy sebezhető toooutages több példánya között történik. Pl. kimaradás költséges feladatokat (például újjáépítése indexek) vagy replika adatvesztést is okozhat. 

Sok más esetben hello a teljes szolgáltatás rendelkezésre állása növelhető a szabályos leállítást sorozat végrehajtásával épp (vagy megszakítás alatt álló) üzenetsoroktól tranzakciók, újbóli feladatok tooother virtuális gépek hello fürt (kézi feladatátvételre), vagy távolítsa el hello A virtuális gép hálózati load balancer készletből. 

Előfordulhatnak olyan esetek, ahol a rendszergazda egy jövőbeli eseményről, amely értesíti, vagy ilyen esemény naplózása érdekében hello szervizelhetőségét hello felhőben üzemeltetett alkalmazások fejlesztése.

Az Azure metaadat-szolgáltatás felületek ütemezett események hello következő esetekben használja:
-   A platform által kezdeményezett karbantartási (például a gazda operációs Rendszerével Bevezetés)
-   A felhasználó által kezdeményezett hívások (például felhasználó újraindítja vagy redeploys a virtuális gépek)


## <a name="scheduled-events---hello-basics"></a>Ütemezett események - hello alapjai  

Azure metaadat-szolgáltatás REST-végpont belülről érhetők el a virtuális gép hello használata virtuális gépek futtatásával kapcsolatos információkat tesz elérhetővé. hello érhető el információ egy nem átirányítható IP-cím segítségével, hogy nincs felfedve hello VM kívül.

### <a name="scope"></a>Hatókör
Ütemezett események illesztve tooall felhőszolgáltatásban lévő virtuális gépek vagy virtuális gépek rendelkezésre állási csoport tooall. Ennek eredményeképpen, jelölje be hello `Resources` hello esemény tooidentify, amely a virtuális gépek is érintett toobe mezőbe. 

### <a name="discovering-hello-endpoint"></a>Hello végpont felderítése
Hello esetében, ahol a rendszer létrehoz egy virtuális gép egy virtuális hálózatot (VNet), hello metaadat-szolgáltatás érhető el a statikus nem irányítható IP-címről `169.254.169.254`.
Ha a virtuális gép hello hello alapértelmezett esetben felhőszolgáltatások és a klasszikus virtuális gépek, virtuális hálózaton belül nem jön létre további logikát szükséges toodiscover hello végpont toouse. Tekintse meg a toothis minta toolearn hogyan túl[hello gazdagép-végpont felderítése](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Versioning 
hello példány metaadat-szolgáltatás rendszerverzióval ellátott. Verziók kötelező, és hello verziószámának `2017-03-01`.

> [!NOTE] 
> Előző előzetes kiadásaiban ütemezett események {legújabb} hello api-verziója támogatott. Ez a formátum már nem támogatott, és a jövőbeli hello elavulttá válik.

### <a name="using-headers"></a>Fejlécek használata
Ha hello metaadat-szolgáltatás, meg kell adnia hello fejléc `Metadata: true` tooensure hello kérést nem volt szándékos átirányítja.

### <a name="enabling-scheduled-events"></a>Ütemezett események engedélyezése
hello ütemezett események kérelmet használhat először Azure implicit módon engedélyezi hello szolgáltatást a virtuális gépen. Ennek eredményeképpen kell késleltetett választ várt tootwo perc fel az első hívásakor.

### <a name="user-initiated-maintenance"></a>Felhasználó által kezdeményezett karbantartás
Felhasználó kezdeményezte a virtuális gép karbantartási hello Azure-portálon API, a parancssori felületen keresztül, vagy PowerShell ütemezett események eredményez. Ez lehetővé teszi az alkalmazás tootest hello karbantartási előkészítése programot, és lehetővé teszi, hogy a felhasználó által kezdeményezett karbantartás alkalmazás tooprepare.

A virtuális gép újraindítása a művelet ütemezi típusú esemény `Reboot`. Újratelepíteni a virtuális gépet a művelet ütemezi típusú esemény `Redeploy`.

> [!NOTE] 
> Jelenleg legfeljebb 10 felhasználó által kezdeményezett karbantartási műveletek egyidejűleg ütemezhető. Ezt a határt fog enyhíteni ütemezett események általánosan rendelkezésre álló előtt.

> [!NOTE] 
> Felhasználó által kezdeményezett karbantartást ütemezett esemény jelenleg nem konfigurálható. Beállíthatóság tervezünk-e egy későbbi kiadásban.

## <a name="using-hello-api"></a>Hello API használatával

### <a name="query-for-events"></a>Lekérdezést eseményekhez.
Ütemezett események egyszerűen azáltal, hogy hello következő hívás lekérdezése:

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

A válasz ütemezett események tömbjét tartalmazza. Üres tömb azt jelenti, hogy nincsenek-e jelenleg ütemezett események.
Hello esetben ahol ütemezett események, hello válasz események tömbjét tartalmazza: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Esemény tulajdonságai
|Tulajdonság  |  Leírás |
| - | - |
| Eseményazonosító | Ez az esemény globálisan egyedi azonosítóját. <br><br> Példa: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Esemény típusa | Ez az esemény hatására hatása. <br><br> Értékek: <br><ul><li> `Freeze`: hello virtuális gép ütemezett toopause néhány másodpercig. hello CPU fel lesz függesztve, de nincs hatással a memória, a megnyitott fájlokat vagy a hálózati kapcsolatok. <li>`Reboot`: virtuális gép az ütemezett újraindítás hello (a nem állandó memória elvész). <li>`Redeploy`: hello virtuális gép ütemezett toomove tooanother csomópont (a rövid élettartamú lemezek elvesznek). |
| ResourceType | Ez az esemény hatással van erőforrás típusát. <br><br> Értékek: <ul><li>`VirtualMachine`|
| Erőforrások| Ez az esemény hatással van erőforrások listáját. Legfeljebb egy ez garantáltan toocontain gépek [frissítési tartomány](windows/manage-availability.md), de nem tartalmazhat a hello UD lévő összes gépen. <br><br> Példa: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Esemény állapota | Ez az esemény állapotát. <br><br> Értékek: <ul><li>`Scheduled`: Ez az esemény az ütemezett toostart hello megadott hello idő múlva `NotBefore` tulajdonság.<li>`Started`: Ez az esemény elindult.</ul> Nem `Completed` vagy hasonló állapot valaha is biztosítja; hello esemény már nem adható vissza, ha hello esemény befejeződött.
| NotBefore| Az idő elteltével kezdheti el ezt az eseményt. <br><br> Példa: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Esemény ütemezése
Minden esemény van ütemezve egy minimális időtartama a jövőbeli hello eseménytípus alapján. Most megjelenik egy esemény `NotBefore` tulajdonság. 

|Esemény típusa  | Minimális értesítés |
| - | - |
| Rögzítése| 15 perc |
| Újraindítás | 15 perc |
| Ismételt üzembe helyezés | 10 perc |

### <a name="starting-an-event-expedite"></a>Az eseményt (gyorsítása)

Miután egy jövőbeli esemény megtanulta, és a szabályos leállítást logikát befejeződött, függőben lévő esemény hello jóváhagyhatja azáltal, hogy egy `POST` toohello metaadat-bA hello hívás `EventId`. Ez azt jelzi, hogy azt lerövidíthető hello minimális értesítési tooAzure (amikor csak lehetséges). 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> Egy esemény igazolása lehetővé teszi hello esemény tooproceed összes `Resources` hello esemény, nem csak hello virtuális gépen, amely elismeri hello esemény. Ezért választhatja, hogy tooelect egy vezető toocoordinate hello nyugtázása, akkor egyszerűen hello első gép hello `Resources` mező.

## <a name="samples"></a>Példák

### <a name="powershell-sample"></a>PowerShell-példa 

hello alábbi mintalekérdezések hello ütemezett események metaadat-szolgáltatás, és hagyja jóvá a függőben lévő események.

```PowerShell
# How tooget scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How tooapprove a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create hello Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert tooJSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with hello following: `n" $approvalString

    # Post approval string tooscheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up hello scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


### <a name="c-sample"></a>C\# minta 

hello következő minta nem egyszerű ügyfél, amely hello metaadatok szolgáltatással kommunikál.

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up hello scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

Ütemezett események jelölhető ki, a következő adatstruktúrák hello használata:

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

hello alábbi mintalekérdezések hello ütemezett események metaadat-szolgáltatás, és hagyja jóvá a függőben lévő események.

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter tooapprove executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter toorepeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

### <a name="python-sample"></a>Python-minta 

hello alábbi mintalekérdezések hello ütemezett események metaadat-szolgáltatás, és hagyja jóvá a függőben lévő események.

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Következő lépések 

- További információk a hello hello elérhető API-k [metaadat-szolgáltatás példány](virtual-machines-instancemetadataservice-overview.md).
- További tudnivalók [a tervezett karbantartások Windows virtuális gépek Azure-ban](windows/planned-maintenance.md).
- További tudnivalók [a tervezett karbantartások Linux virtuális gépek Azure-ban](linux/planned-maintenance.md).
