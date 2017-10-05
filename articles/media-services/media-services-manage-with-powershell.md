---
title: "A PowerShell segítségével az Azure Media Services fiókok kezelése"
description: "Útmutató: Azure Media Services-fiókok a PowerShell-parancsmagokkal kezelheti."
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
ms.openlocfilehash: 3d999d9e27844bc0164cc3572522b9ec022118a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a>A PowerShell segítségével az Azure Media Services fiókok kezelése
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> Nem fogja tudni az Azure Media Services-fiók létrehozása, rendelkeznie kell egy Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Ingyenes Azure-fiók létrehozása</a>.
> 
> 

## <a name="overview"></a>Áttekintés
Ez a cikk az Azure Media Services (AMS) tartalmazza az Azure PowerShell-parancsmagok az Azure Resource Manager keretében. A parancsmagok szerepel a **Microsoft.Azure.Commands.Media** névtér.

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

Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a media Services nevét.

| Aliasok | Név |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

**-Hely &lt;karakterlánc&gt;**

A media Services erőforrás helyét adja meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-StorageAccountId &lt;karakterlánc&gt;**

Meghatározza, hogy a media Services társított elsődleges tárfiók.

* Új tárfiók (Resource Manager API-val létrehozott) csak a támogatott.
* A tárfiók már léteznie kell, és ugyanazon a helyen, a media szolgáltatással rendelkezik.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |3 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |StorageAccountIdParamSet |
| Helyettesítő karakterek elfogadása? |hamis |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Meghatározza, hogy a media Services társított tárfiókokat.

* Új tárfiók (Resource Manager API-val létrehozott) csak a támogatott.
* A tárfiók már léteznie kell, és ugyanazon a helyen, a media szolgáltatással rendelkezik.
* Csak egy tárfiók elsődleges adhat meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |3 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |StorageAccountsParamSet |
| Helyettesítő karakterek elfogadása? |hamis |

**-Címkéket &lt;Hashtable&gt;**

Megadja egy kivonattáblát a címkék a media Services társított.

* Példa: @{"" ="érték1 tag1";" tag2 "=: Érték2"}

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |hamis |
| Hová kerüljön? |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.

### <a name="inputs"></a>Bemenetek
A bemeneti típus a parancsmagnak átadható objektumok típusa.

### <a name="outputs"></a>kimenetek
A kimeneti típus a parancsmag által létrehozott objektumok típusa.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService
Egy media Services frissíti.

### <a name="syntax"></a>Szintaxis
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a media Services nevét.

| Aliasok | Név |
| --- | --- |
| Kötelező megadni? |True (Igaz) |
| Hová kerüljön? |1 |
| Alapértelmezett érték |None |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |False (Hamis) |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Meghatározza, hogy a media Services társított tárfiókokat.

* Új tárfiók (Resource Manager API-val létrehozott) csak a támogatott.
* A tárfiók már léteznie kell, és ugyanazon a helyen, a media szolgáltatással rendelkezik.
* Csak egy tárfiók elsődleges adhat meg.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |hamis |
| Hová kerüljön? |nevű |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |StorageAccountsParamSet |
| Helyettesítő karakterek elfogadása? |hamis |

**-Címkéket &lt;Hashtable&gt;**

Megadja egy kivonattáblát a címkék a media Services társított.

* A media Services társított címkék értékkel az ügyfél által megadott helyett.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |False (Hamis) |
| Hová kerüljön? |nevű |
| Alapértelmezett érték |None |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.

### <a name="inputs"></a>Bemenetek
A bemeneti típus a parancsmagnak átadható objektumok típusa.

### <a name="outputs"></a>kimenetek
A kimeneti típus a parancsmag által létrehozott objektumok típusa.

## <a name="remove-azurermmediaservice"></a>Remove-AzureRmMediaService
Egy media Services eltávolítja.

### <a name="syntax"></a>Szintaxis
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a media Services nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |None |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |False (Hamis) |

**&lt;CommandParameters&gt;**

Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.

### <a name="inputs"></a>Bemenetek
A bemeneti típus a parancsmagnak átadható objektumok típusa.

### <a name="outputs"></a>kimenetek
A kimeneti típus a parancsmag által létrehozott objektumok típusa.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService
Lekérdezi egy erőforráscsoportban található összes media services vagy egy media Services a megadott névvel.

### <a name="syntax"></a>Szintaxis
ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |ResourceGroupParameterSet, AccountNameParameterSet |

Helyettesítő karakterek elfogadása?   hamis

**-AccountName &lt;karakterlánc&gt;**

Megadja a media Services nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| A paraméter neve |AccountNameParameterSet |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.

### <a name="inputs"></a>Bemenetek
A bemeneti típus a parancsmagnak átadható objektumok típusa.

### <a name="outputs"></a>kimenetek
A kimeneti típus a parancsmag által létrehozott objektumok típusa.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys
Lekérdezi a médiaszolgáltatás kulcsokat.

### <a name="syntax"></a>Szintaxis
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a media Services nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.

### <a name="inputs"></a>Bemenetek
A bemeneti típus a parancsmagnak átadható objektumok típusa.

### <a name="outputs"></a>kimenetek
A kimeneti típus a parancsmag által létrehozott objektumok típusa.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey
Egy media Services elsődleges vagy másodlagos kulcs újragenerálása.

### <a name="syntax"></a>Szintaxis
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a media Services nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**KeyType – &lt;KeyType&gt;**

Megadja a media Services kulcs típusát.

* Elsődleges vagy másodlagos

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |hamis |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.

### <a name="inputs"></a>Bemenetek
A bemeneti típus a parancsmagnak átadható objektumok típusa.

### <a name="outputs"></a>kimenetek
A kimeneti típus a parancsmag által létrehozott objektumok típusa.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sync-AzureRmMediaServiceStorageKeys
A media Services társított storage-fiók tárfiókkulcsainak szinkronizálása.

### <a name="syntax"></a>Szintaxis
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek
**-ResourceGroupName &lt;karakterlánc&gt;**

Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |0 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-AccountName &lt;karakterlánc&gt;**

Megadja a media Services nevét.

| Aliasok | Egyik sem |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |1 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**-StorageAccountId &lt;karakterlánc&gt;**

Adja meg a tárfiókot, a media Services társított.

| Aliasok | Azonosító |
| --- | --- |
| Kötelező megadni? |Igaz |
| Hová kerüljön? |2 |
| Alapértelmezett érték |Egyik sem |
| Fogadja el a feldolgozási sor beviteli? |TRUE(ByPropertyName) |
| Helyettesítő karakterek elfogadása? |hamis |

**&lt;CommandParameters&gt;**

Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.

### <a name="inputs"></a>Bemenetek
A bemeneti típus a parancsmagnak átadható objektumok típusa.

### <a name="outputs"></a>kimenetek
A kimeneti típus a parancsmag által létrehozott objektumok típusa.

## <a name="next-step"></a>Következő lépés
Tekintse meg a Media Services tanulási útvonalai.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

