---
title: "Linux virtuális gépek Azure-ban események aaaScheduled |} Microsoft Docs"
description: "Ütemezett események hello Azure metaadatok szolgáltatás használata a Linux virtuális gépeken."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: e153c8854725330acefd67d0ef542f6149170a7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a>Az Azure metaadat-szolgáltatás: Ütemezett események (előzetes verzió) Linux virtuális gépekhez

> [!NOTE] 
> Az előzetes verziójú funkciók elérhető tooyou hello feltétel, hogy elfogadja a használati feltételek toohello készülnek el. További részletekért lásd: [Kiegészítő használati feltételek a Microsoft Azure előzetes verziójú termékeihez](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Ütemezett események egyike hello subservices hello Azure metaadat-szolgáltatás alatt. Felelős felszínre hozza a jövőbeni események kapcsolatos információk (például újraindítás), az alkalmazás előkészítése őket, és korlátozza megszakítása. Érhető el minden Azure virtuális gép esetében, beleértve a PaaS és IaaS. Ütemezett események biztosít a virtuális gép idő tooperform megelőző feladatok toominimize hello hatásának egy eseményt. 

A Windows és a Linux virtuális gépek ütemezett események érhető el. További információk a Windows az ütemezett eseményekről: [ütemezett eseményeket a Windows virtuális gépek](../windows/scheduled-events.md).

## <a name="why-scheduled-events"></a>Miért ütemezett eseményeket?

Ütemezett események lépéseket toolimit hello hatása platform-intiated karbantartás vagy felhasználó által kezdeményezett műveletek a szolgáltatás. 

Többpéldányos munkaterhelések esetén, amelyek replikációs technikák toomaintain állapota, lehet, hogy sebezhető toooutages több példánya között történik. Pl. kimaradás költséges feladatokat (például újjáépítése indexek) vagy replika adatvesztést is okozhat. 

Sok más esetben hello a teljes szolgáltatás rendelkezésre állása növelhető a szabályos leállítást sorozat végrehajtásával épp (vagy megszakítás alatt álló) üzenetsoroktól tranzakciók, újbóli feladatok tooother virtuális gépek hello fürt (kézi feladatátvételre), vagy távolítsa el hello A virtuális gép hálózati load balancer készletből. 

Előfordulhatnak olyan esetek, ahol a rendszergazda egy jövőbeli eseményről, amely értesíti, vagy ilyen esemény naplózása érdekében hello szervizelhetőségét hello felhőben üzemeltetett alkalmazások fejlesztése.

Az Azure metaadat-szolgáltatás felületek ütemezett események hello következő esetekben használja:
-   A platform által kezdeményezett karbantartási (például a gazda operációs Rendszerével Bevezetés)
-   A felhasználó által kezdeményezett hívások (például felhasználó újraindítja vagy redeploys a virtuális gépek)


## <a name="hello-basics"></a>hello alapjai  

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
Felhasználó kezdeményezte a virtuális gép karbantartási hello Azure-portálon API, a parancssori felületen keresztül, vagy PowerShell ütemezett esemény eredményez. Ez lehetővé teszi az alkalmazás tootest hello karbantartási előkészítése programot, és lehetővé teszi, hogy a felhasználó által kezdeményezett karbantartás alkalmazás tooprepare.

A virtuális gép újraindítása ütemezi típusú esemény `Reboot`. Újratelepíteni a virtuális gépet ütemezi a következő típusú eseményre `Redeploy`.

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
| Esemény típusa | Ez az esemény hatására hatása. <br><br> Értékek: <br><ul><li> `Freeze`: hello virtuális gép ütemezett toopause néhány másodpercig. hello CPU fel van függesztve, de nincs hatással a memória, a megnyitott fájlokat vagy a hálózati kapcsolatok. <li>`Reboot`: virtuális gép az ütemezett újraindítás hello (a nem állandó memória elvész). <li>`Redeploy`: hello virtuális gép ütemezett toomove tooanother csomópont (a rövid élettartamú lemezek elvesznek). |
| ResourceType | Ez az esemény hatással van erőforrás típusát. <br><br> Értékek: <ul><li>`VirtualMachine`|
| Erőforrások| Ez az esemény hatással van erőforrások listáját. Legfeljebb egy ez garantáltan toocontain gépek [frissítési tartomány](manage-availability.md), de nem tartalmazhat a hello UD lévő összes gépen. <br><br> Példa: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Esemény állapota | Ez az esemény állapotát. <br><br> Értékek: <ul><li>`Scheduled`: Ez az esemény az ütemezett toostart hello megadott hello idő múlva `NotBefore` tulajdonság.<li>`Started`: Ez az esemény elindult.</ul> Nem `Completed` vagy hasonló állapot valaha is biztosítja; hello esemény már nem adható vissza, ha hello esemény befejeződött.
| NotBefore| Az idő elteltével kezdheti el ezt az eseményt. <br><br> Példa: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Esemény ütemezése
Minden esemény van ütemezve egy minimális időtartama a jövőbeli hello eseménytípus alapján. Most megjelenik egy esemény `NotBefore` tulajdonság. 

|Esemény típusa  | Minimális értesítés |
| - | - |
| Rögzítése| 15 perc |
| Újraindítás | 15 perc |
| Ismételt üzembe helyezés | 10 perc |

### <a name="starting-an-event"></a>Egy esemény indítása 

Miután egy jövőbeli esemény megtanulta, és a szabályos leállítást logikát befejeződött, függőben lévő esemény hello jóváhagyhatja azáltal, hogy egy `POST` toohello metaadat-bA hello hívás `EventId`. Ez azt jelzi, hogy azt lerövidíthető hello minimális értesítési tooAzure (amikor csak lehetséges). 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> Egy esemény igazolása lehetővé teszi, hogy hello esemény tooproceed összes `Resources` hello esemény, nem csak hello virtuális gépen, amely elismeri hello esemény. Ezért választhatja, hogy tooelect egy vezető toocoordinate hello nyugtázása, akkor egyszerűen hello első gép hello `Resources` mező.




## <a name="python-sample"></a>Python-minta 

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

- További információk a hello hello elérhető API-k [példány metaadat-szolgáltatás](instance-metadata-service.md).
- További tudnivalók [a tervezett karbantartások Linux virtuális gépek Azure-ban](planned-maintenance.md).
