---
title: "Példány metaadat-szolgáltatás Windows aaaAzure |} Microsoft Docs"
description: "RESTful felület tooget információ Windows virtuális gép számítási, hálózati, és a közelgő karbantartásokról események."
services: virtual-machines-windows
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: harijay
ms.openlocfilehash: a33c26b5e9ed650be639598cdb6895fc19ccb605
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-instance-metadata-service-for-windows-vms"></a>Windows virtuális gépek Azure példány metaadat-szolgáltatás


hello Azure példány metaadat-szolgáltatás fut a virtuális gépet állíthat be és használt toomanage lehet virtuálisgép-példányok információkat biztosít.
Ide tartoznak például Termékváltozat, a hálózati konfigurációs és a közelgő karbantartásokról események. Milyen típusú információkat érhető el a további információkért lásd: [metaadat-kategóriák](#instance-metadata-data-categories).

Azure példány metaadatok szolgáltatás nem érhető el tooall IaaS virtuális gépeket hello segítségével létrehozott REST-végpont [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). hello végpont egy jól ismert nem irányítható IP-címen érhető el (`169.254.169.254`), amely elérhető csak hello VM belül.



> [!IMPORTANT]
> Ez a szolgáltatás **általánosan elérhető** globális Azure-régióban. Nyilvános előzetes kormányzati, Kína és német Azure felhőben van. Rendszeresen kap frissítéseket tooexpose vonatkozó új információt virtuálisgép-példánya. Ezen a lapon tükrözi naprakész hello [az adatkategóriákat](#instance-metadata-data-categories) érhető el.



## <a name="service-availability"></a>Szolgáltatás rendelkezésre állása
hello szolgáltatás minden általánosan elérhető globális Azure területen érhető el. hello szolgáltatás nyilvános előzetes hello kormányzati, Kína vagy Németország régióban van.

Régiók                                        | Rendelkezésre állási?
-----------------------------------------------|-----------------------------------------------
[Minden általánosan elérhető globális Azure-régió](https://azure.microsoft.com/regions/)     | Általánosan elérhető 
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | Az előzetes verzió 
[Az Azure Kínában](https://www.azure.cn/)                                                           | Az előzetes verzió
[Az Azure-Németország](https://azure.microsoft.com/overview/clouds/germany/)                    | Az előzetes verzió

Ez a táblázat frissülni fog, amikor más Azure felhőben hello szolgáltatás elérhetővé válik.

tootry kimenő hello példány metaadat-szolgáltatás, a virtuális gép létrehozása [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) vagy hello [Azure-portálon](http://portal.azure.com) a hello régiók és kövesse hello példák az alábbi felett.

## <a name="usage"></a>Használat

### <a name="versioning"></a>Versioning
hello példány metaadat-szolgáltatás rendszerverzióval ellátott. Verziók kötelező, és hello verziószámának `2017-04-02`.

> [!NOTE] 
> Előző előzetes kiadásaiban ütemezett események {legújabb} hello api-verziója támogatott. Ez a formátum már nem támogatott, és a jövőbeli hello elavulttá válik.

Azt hozzáadja az újabb verziók, régebbi verziók továbbra is elérhető kompatibilitás, ha a parancsfájlok tartalmazhat függőségeket, meghatározott formátumban. Vegye figyelembe azonban, hogy hello aktuális előzetes version(2017-03-01) nem érhető el hello szolgáltatás általánosan elérhetővé válik.

### <a name="using-headers"></a>Fejlécek használata
Ha hello példány metaadat-szolgáltatás, meg kell adnia hello fejléc `Metadata: true` tooensure hello kérést nem volt szándékos átirányítja.

### <a name="retrieving-metadata"></a>Metaadatainak lekérése során

Példány metaadatai nem érhető el a futó virtuális gépek létrehozása/segítségével kezelt [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Elérhető összes az adatkategóriákat a virtuális gép példány hello kérelem a következő használatával:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> Minden példány metaadatok lekérdezés-és nagybetűk.

### <a name="data-output"></a>Kimeneti adatok
Alapértelmezés szerint hello példány metaadat-szolgáltatás adatokat adja vissza JSON formátumban (`Content-Type: application/json`). Azonban más API-k adhatnak vissza adatokat különböző formátumokban kérésre.
hello következő tábla más adatok formátumú API-k támogathatja a hivatkozást.

API | Alapértelmezett adatformátum | Eltérő formátumban
--------|---------------------|--------------
/instance | JSON-ban | Szöveg
/scheduledevents | JSON-ban | Egyik sem

egy nem alapértelmezett válaszformátum tooaccess hello kért formátum hello kérelem lekérdezési karakterlánc paraméterként adja meg. Példa:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a>Biztonság
hello példány metaadatok szolgáltatás végpontjának értéke csak nem irányítható IP-címnek a virtuálisgép-példányt futtató hello belülről érhetők el. Emellett a kérelmet egy `X-Forwarded-For` fejléc hello szolgáltatás elutasította.
Azt is igénylik kérelmek toocontain egy `Metadata: true` fejléc tooensure, amelyek tényleges kérelem hello közvetlenül volt a tervezett és nem szándékos átirányítási része. 

### <a name="error"></a>Hiba
Ha nem található egy adatelem vagy hibás formátumú kérelem, a példány metaadat-szolgáltatás hello szabványos HTTP-hibák adja vissza. Példa:

HTTP-állapotkód | Ok
----------------|-------
200 OK |
400 Hibás kérés | Hiányzó `Metadata: true` fejléc
404 – Nem található | hello kért elem does't létezik 
405 metódus nem engedélyezett | Csak `GET` és `POST` kérelmek támogatottak.
429 túl sok kérelem | hello API jelenleg legfeljebb 5 lekérdezések száma másodpercenként
500 szolgáltatási hiba     | Némi várakozás után próbálja meg újra

### <a name="examples"></a>Példák

> [!NOTE] 
> Minden API-válasz olyan JSON-karakterláncok. Minden, a következő példa válaszok pretty nyomtatott olvashatóság érdekében.

#### <a name="retrieving-network-information"></a>Hálózati adatok beolvasása

**Kérés**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

**Válasz**

> [!NOTE] 
> a rendszer hello választ, egy JSON-karakterláncban. a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.

```
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Nyilvános IP-cím beolvasása

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Egy példányát minden metaadatainak beolvasása

**Kérés**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

**Válasz**

> [!NOTE] 
> a rendszer hello választ, egy JSON-karakterláncban. a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.

```
{
  "compute": {
    "location": "westcentralus",
    "name": "IMDSSample",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "Canonical",
    "sku": "16.04.0-LTS",
    "version": "16.04.201610200",
    "vmId": "5d33a910-a7a0-4443-9f01-6a807801b29b",
    "vmSize": "Standard_A1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.0.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.0.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3AF806EC"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>A Windows rendszerű virtuális gép metaadatainak lekérése során

**Kérés**

Példány metaadatok lehet beolvasni a Windows hello PowerShell segédprogram segítségével `curl`: 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

Vagy hello `Invoke-RestMethod` parancsmagot:
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

**Válasz**

> [!NOTE] 
> a rendszer hello választ, egy JSON-karakterláncban. a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.

```
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Példány metaadat az adatkategóriákat
az adatkategóriákat a következő hello hello példány metaadat-szolgáltatás keresztül érhetők el:

Adatok | Leírás
-----|------------
location | Az Azure régióban hello virtuális gép fut.
név | Hello virtuális gép neve 
az ajánlat | Hello VM-lemezkép információkat nyújtanak. Kép: Azure-galériából telepített lemezképek nincs csak ezt az értéket.
Közzétevő | Hello Virtuálisgép-lemezkép kiadója
Termékváltozat | Adott SKU hello VM-lemezkép  
Verzió | Hello Virtuálisgép-lemezkép verziója 
osType | Linux- vagy Windows 
platformUpdateDomain |  [Frissítési tartományok](manage-availability.md) hello virtuális gép fut.
platformFaultDomain | [Tartalék tartomány](manage-availability.md) hello virtuális gép fut.
vmId | [Egyedi azonosító](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) hello virtuális gép számára
vmSize | [Virtuálisgép-mérettel](sizes.md)
IPv4/privateipaddress tulajdonságot | Virtuális gép hello helyi IPv4-címe 
IPv4-vagy nyilvános | Virtuális gép hello nyilvános IPv4-címe
alhálózat és címét | Virtuális gép hello alhálózati címe
/ előtagot. | Példa 24 alhálózati előtag
IPv6/IP-cím | Virtuális gép hello helyi IPv6-cím
MacAddress | Virtuális gép mac-cím 
scheduledevents | Jelenleg a nyilvános előzetes lásd: [scheduledevents](scheduled-events.md)

## <a name="example-scenarios-for-usage"></a>Példa használati forgatókönyvek  

### <a name="tracking-vm-running-on-azure"></a>Azure-on futó virtuális gép nyomon követése

Szolgáltató előfordulhat, hogy a szoftver futó virtuális gépek száma tootrack hello, vagy hello VM tootrack egyediségének igénylő ügynökök. toobe képes tooget egyedi Azonosítót a virtuális gépek használata hello `vmId` példány metaadat-szolgáltatásának mezőjét.

**Kérés**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

**Válasz**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Tárolók elhelyezését, adat-partíciók alapú tartalék/frissítési tartomány 

Az egyes forgatókönyvek, más adatok replikák elhelyezését van elsődleges fontosságú. Például [HDFS replika elhelyezésének](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) vagy tároló elhelyezési keresztül egy [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) igényelheti tooknow hello `platformFaultDomain` és `platformUpdateDomain` hello futó virtuális gép.
Ezek az adatok közvetlenül hello példány metaadat-szolgáltatás segítségével lekérdezheti.

**Kérés**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

**Válasz**

```
0
```

### <a name="getting-more-information-about-hello-vm-during-support-case"></a>További információ a virtuális gép hello során támogatási esetet beolvasása 

Szolgáltató előfordulhat, hogy kap egy támogatási hívás tooknow helyének hello VM további információt. Hello ügyfél tooshare kérése a hello számítási metaadatok alapvető információkat a támogatási szakemberek tooknow hello hello típusú virtuális gép Azure biztosíthat. 

**Kérés**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

**Válasz**

> [!NOTE] 
> a rendszer hello választ, egy JSON-karakterláncban. a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.

```
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-hello-vm"></a>Példák hello VM belül különböző nyelveken használó metaadatok szolgáltatás hívása 

Nyelv | Példa 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB
Nyissa meg a helyi hálózat   | https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go            
python   | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY
C++      | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp
C#       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs
Javascript | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH
    

## <a name="faq"></a>GYIK
1. Hello hiba azért kapom `400 Bad Request, Required metadata header not specified`. Ez mit jelent?
   * hello példány metaadat-szolgáltatás hello fejlécet igényel `Metadata: true` toobe átadott hello kérelemben. Ezt a fejlécet benyújtása hello REST-hívást lehetővé teszi, hogy a hozzáférés toohello példány metaadat-szolgáltatás. 
2. Miért nem jelenik meg a virtuális gép számítási adatai?
   * Hello példány metaadat-szolgáltatás jelenleg csak az Azure Resource Manager eszközzel létrehozott példányokat támogatja. A jövőbeli hello azt is támogatásáról Cloud Service virtuális gépeken.
3. A virtuális gép Azure Resource Manager használatával létrehozott egy while vissza. Miért nem tudok lásd számítási metaadat-információ?
   * A 2016. szeptember után létrehozott virtuális gépek hozzáadása egy [címke](../../azure-resource-manager/resource-group-using-tags.md) toostart megnéz számítási metaadatok. (Szeptember 2016 előtt létrehozott) régebbi virtuális gépek hozzáadása extensions vagy adatok lemezek toohello VM toorefresh metaadatok.
4. Miért jelenik meg hello hiba `500 Internal Server Error`?
   * Próbálja megismételni a kérést a rendszer ki exponenciális vissza alapján. Ha nem szűnik hello Azure ügyfélszolgálatához.
5. Ha ugyanazt a további kérdéseit vagy megjegyzéseit?
   * A Megjegyzések küldése a http://feedback.azure.com.
7. Ez használható lenne a virtuálisgép-méretezési beállítása példányt?
   * Igen metaadat-szolgáltatás esetén méretezési beállítása példányok érhető el. 
6. Hogyan kérhet támogatást hello szolgáltatás?
   * tooget támogatása hello szolgáltatást, hozzon létre egy támogatási probléma Azure-portál a virtuális gép, amelyen még nem tud tooget metaadatok válasz hosszú újrapróbálkozás után hello 

   ![Példány metaadatok támogatása](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Következő lépések

- További tudnivalók hello [ütemezett események](scheduled-events.md) API **nyilvános előzetes verziójában** hello példány metaadat-szolgáltatás által biztosított.
