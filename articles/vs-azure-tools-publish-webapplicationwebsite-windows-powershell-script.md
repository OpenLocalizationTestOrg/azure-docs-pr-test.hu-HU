---
title: "(a Windows PowerShell-parancsfájl) aaaPublish-WebApplicationWebSite |} Microsoft Docs"
description: "Ismerje meg, hogyan toopublish egy webes projekt tooan Azure-webhelyen. Ez a parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d46904e30e3c2e040e57888fa31543e8e366527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>(A Windows PowerShell-parancsfájl) közzététele-WebApplicationWebSite
## <a name="syntax"></a>Szintaxis
Közzéteszi a webes projekt tooan Azure-webhelyen. hello parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguráció
hello elérési toohello JSON-konfigurációs fájlt, amely hello hello központi telepítés részleteit ismerteti.

| Paraméter | Alapértelmezett érték |
| --- | --- |
| Aliasok |Egyik sem |
| Kötelező? |Igaz |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

## <a name="subscriptionname"></a>SubscriptionName
hello hello Azure-előfizetést, amelyet szeretne toocreate hello webhely neve.

| Paraméter | Alapértelmezett érték |
| --- | --- |
| Aliasok |Egyik sem |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

## <a name="webdeploypackage"></a>WebDeployPackage
hello elérési toohello webes telepítési csomag toopublish toohello webhelyet. Ezt a csomagot a Visual Studio hello webhely közzététele varázsló használatával hozhat létre. További információkért lásd: [Ismerkedés az Azure felhőalapú szolgáltatásairól és ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Paraméter | Alapértelmezett érték |
| --- | --- |
| Aliasok |Egyik sem |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
hello felhasználónév és jelszó hello SQL-adatbázis az Azure-ban.

| Paraméter | Alapértelmezett érték |
| --- | --- |
| Aliasok |Egyik sem |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Igaz értéke esetén a nyomtatási üzeneteit hello parancsfájl toohello kimeneti adatfolyam.

| Paraméter | Alapértelmezett érték |
| --- | --- |
| Aliasok |Egyik sem |
| Kötelező? |hamis |
| Beosztás |nevű |
| Alapértelmezett érték |hamis |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

## <a name="remarks"></a>Megjegyzések
Hogyan toouse hello parancsfájl toocreate fejlesztési és tesztkörnyezetek: teljes leírását [Windows PowerShell-parancsfájlok használatával tooPublish tooDev és a tesztkörnyezetek](vs-azure-tools-publishing-using-powershell-scripts.md).

hello JSON-konfigurációs fájlt határozza meg, hogy mit telepített toobe hello részletei. Ez magában foglalja a hello projekt, például hello nevét és a felhasználónév hello webhely létrehozásakor megadott hello adatokat. Is hello adatbázis tooprovision, ha van ilyen. a következő kód hello látható egy példa JSON-konfigurációs fájlt:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Szerkesztheti a hello JSON konfigurációs fájl toochange telepítve van. A webhely szakaszban szükség, de hello adatbázis szakasz nem kötelező megadni.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: [Publish-WebApplicationVM (a Windows PowerShell-parancsfájl)](vs-azure-tools-publish-webapplicationvm.md)

