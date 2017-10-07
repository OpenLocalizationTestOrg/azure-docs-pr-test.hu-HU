---
title: aaaPublish-WebApplicationVM |} Microsoft Docs
description: "Megtudhatja, hogyan toodeploy egy webes alkalmazás tooa virtuális gépet. Ez a parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: e4b52b620bebf44b87ddfc3b19c155bb65111814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>(A Windows PowerShell-parancsfájl) közzététele-WebApplicationVM
Telepít a webes alkalmazás tooa virtuális gépet. hello parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguráció
hello elérési toohello JSON-konfigurációs fájlt, amely hello hello központi telepítés részleteit ismerteti.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

### <a name="subscriptionname"></a>SubscriptionName
hello neve hello toocreate hello virtuális géphez használni kívánt Azure-előfizetés.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |Hello első előfizetés használ egy hello előfizetési fájl: |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

### <a name="webdeploypackage"></a>WebDeployPackage
hello elérési toohello webes telepítési csomag toopublish toohello virtuális gép. Ezt a csomagot a Visual Studio hello webhely közzététele varázsló használatával hozhat létre. Lásd: [Útmutató: webes telepítési csomag létrehozása a Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

### <a name="allowuntrusted"></a>AllowUntrusted
Amennyiben az értéke igaz, a tanúsítványokat, amelyek nem megbízható legfelső szintű hatóság aláírásával hello használatának engedélyezése.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |hamis |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

### <a name="vmpassword"></a>VMPassword
hello hello virtuális gép fiók hitelesítő adatait. Példa: - VMPassword @{név = "rendszergazda"; Jelszó = a "password"}

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
az Azure SQL-adatbázis hello hello hitelesítő adatokat. Példa: - DatabaseServerPassword @{név = "rendszergazda"; Jelszó = a "password"}

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Igaz értéke esetén a nyomtatási üzeneteit hello parancsfájl toohello kimeneti adatfolyam.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |hamis |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

## <a name="remarks"></a>Megjegyzések
Hogyan toouse hello parancsfájl toocreate fejlesztési és tesztkörnyezetek: teljes leírását [Windows PowerShell-parancsfájlok használatával tooPublish tooDev és a tesztkörnyezetek](vs-azure-tools-publishing-using-powershell-scripts.md).

hello JSON-konfigurációs fájlt határozza meg, hogy mit telepített toobe hello részletei. Ez magában foglalja a hello projekt, például hello nevét, az affinitáscsoport, a Virtuálismerevlemez-kép és a hello virtuális gép mérete létrehozásakor megadott hello adatokat. Azt is magában foglalja a hello végpontok hello virtuális gépen, hello adatbázisok tooprovision, ha van ilyen, webes és üzembe helyezéshez megadott paraméterek. a következő kód hello látható egy példa JSON-konfigurációs fájlt:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Szerkesztheti a hello JSON konfigurációs fájl toochange mi lett kiépítve. Szükség egy virtuális gép és egy felhőalapú szolgáltatás, de hello adatbázis szakasz nem kötelező megadni.

