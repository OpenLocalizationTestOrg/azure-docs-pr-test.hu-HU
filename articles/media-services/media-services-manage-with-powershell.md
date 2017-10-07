---
title: "aaaManage Azure Media Services fiókok a PowerShell használatával"
description: "Ismerje meg, hogyan toomanage Azure Media Services fiókok a PowerShell-parancsmagokkal."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 17a10c25-d94f-421c-b6bc-ae0958e2ac96
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: juliako
ms.openlocfilehash: e8f97bb2393343e45fabf9c437b4fc09f2525dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a>A PowerShell segítségével az Azure Media Services fiókok kezelése
> [!div class="op_single_selector"]
> * [Portál](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> toobe képes toocreate Azure Media Services-fiók, rendelkeznie kell egy Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Ingyenes Azure-fiók létrehozása</a>.
> 
> 

## <a name="overview"></a>Áttekintés
Ez a cikk az Azure Media Services (AMS) felsorolja hello Azure PowerShell-parancsmagok hello Azure Resource Manager keretében. hello parancsmagok szerepel hello **Microsoft.Azure.Commands.Media** névtér.

## <a name="versions"></a>Verziók
**ApiVersion**: "2015-10-01"

## <a name="new-azurermmediaservice"></a>New-AzureRmMediaService
Media szolgáltatás létrehozása.

### <a name="syntax"></a>Szintaxis
A paraméterhalmaz: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

A paraméterhalmaz: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a hello hello médiaszolgáltatás nevére.

| Aliasok | Név |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

**-Hely &lt;karakterlánc&gt;**

Hello media service hello erőforrás helyét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-StorageAccountId &lt;karakterlánc&gt;**

Adja meg, amely hello media service társított elsődleges tárfiók.

* Új tárfiók (Resource Manager API hello létre) csak a támogatott.
* hello tárfiók létezése és rendelkezik hello hello media Service ugyanazon a helyen.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |3 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |StorageAccountIdParamSet |
| Helyettesítő karakterek elfogadása? |hamis |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Adja meg a társított hello media Services, storage-fiókok.

* Új tárfiók (Resource Manager API hello létre) csak a támogatott.
* hello tárfiók létezése és rendelkezik hello hello media Service ugyanazon a helyen.
* Csak egy tárfiók elsődleges adhat meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |3 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |StorageAccountsParamSet |
| Helyettesítő karakterek elfogadása? |hamis |

**-Címkéket &lt;Hashtable&gt;**

Megadja egy kivonattáblát a hello media service társított hello címkék.

* Példa: @{"" ="érték1 tag1";" tag2 "=: Érték2"}

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Hová kerüljön? |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.

### <a name="inputs"></a>Bemenetek
hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.

### <a name="outputs"></a>kimenetek
hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService
Egy media Services frissíti.

### <a name="syntax"></a>Szintaxis
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a hello hello médiaszolgáltatás nevére.

| Aliasok | Név |
| --- | --- |
| Kötelező? |True (Igaz) |
| Hová kerüljön? |1 |
| Alapértelmezett érték |None |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |False (Hamis) |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Adja meg a társított hello media Services, storage-fiókok.

* Új tárfiók (Resource Manager API hello létre) csak a támogatott.
* hello tárfiók létezése és rendelkezik hello hello media Service ugyanazon a helyen.
* Csak egy tárfiók elsődleges adhat meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |hamis |
| Hová kerüljön? |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |StorageAccountsParamSet |
| Helyettesítő karakterek elfogadása? |hamis |

**-Címkéket &lt;Hashtable&gt;**

Megadja egy kivonattáblát a media Services társított hello címkék.

* hello címkék hello media service társított cserélése hello ügyfél által megadott értéket.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |False (Hamis) |
| Hová kerüljön? |nevű |
| Alapértelmezett érték |None |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.

### <a name="inputs"></a>Bemenetek
hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.

### <a name="outputs"></a>kimenetek
hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.

## <a name="remove-azurermmediaservice"></a>Remove-AzureRmMediaService
Egy media Services eltávolítja.

### <a name="syntax"></a>Szintaxis
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a hello hello médiaszolgáltatás nevére.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |None |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |False (Hamis) |

**&lt;CommandParameters&gt;**

Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.

### <a name="inputs"></a>Bemenetek
hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.

### <a name="outputs"></a>kimenetek
hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService
Lekérdezi egy erőforráscsoportban található összes media services vagy egy media Services a megadott névvel.

### <a name="syntax"></a>Szintaxis
ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |ResourceGroupParameterSet, AccountNameParameterSet |

Helyettesítő karakterek elfogadása?   hamis

**-AccountName &lt;karakterlánc&gt;**

Megadja a hello hello médiaszolgáltatás nevére.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |AccountNameParameterSet |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.

### <a name="inputs"></a>Bemenetek
hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.

### <a name="outputs"></a>kimenetek
hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys
Lekérdezi a médiaszolgáltatás kulcsokat.

### <a name="syntax"></a>Szintaxis
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a hello hello médiaszolgáltatás nevére.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.

### <a name="inputs"></a>Bemenetek
hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.

### <a name="outputs"></a>kimenetek
hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey
Egy media Services elsődleges vagy másodlagos kulcs újragenerálása.

### <a name="syntax"></a>Szintaxis
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a hello hello médiaszolgáltatás nevére.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**KeyType – &lt;KeyType&gt;**

Adja meg a hello media service hello kulcstípusával.

* Elsődleges vagy másodlagos

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.

### <a name="inputs"></a>Bemenetek
hello bemeneti típus hello hello típusú objektumokat toothe parancsmag is átadhatja.

### <a name="outputs"></a>kimenetek
hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sync-AzureRmMediaServiceStorageKeys
Hello media service társított storage-fiók tárfiókkulcsainak szinkronizálása.

### <a name="syntax"></a>Szintaxis
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a hello hello médiaszolgáltatás nevére.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-StorageAccountId &lt;karakterlánc&gt;**

Megadja a hello hello media szolgáltatáshoz tartozó tárfiók.

| Aliasok | Azonosító |
| --- | --- |
| Kötelező? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.

### <a name="inputs"></a>Bemenetek
hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.

### <a name="outputs"></a>kimenetek
hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.

## <a name="next-step"></a>Következő lépés
Tekintse meg a Media Services tanulási útvonalai.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

