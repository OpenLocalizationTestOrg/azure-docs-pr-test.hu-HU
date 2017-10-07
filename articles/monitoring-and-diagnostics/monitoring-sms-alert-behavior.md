---
title: "Riasztási viselkedés aaaSMS művelet csoportok |} Microsoft Docs"
description: "SMS-üzenet formátuma és tooSMS üzenetek toounsubscribe válaszol, resubscribe, illetve segítséget kérni."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 3cd09b1903e3472f6402f62b74409d97e7e7ea97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS riasztási művelet csoportok működése
## <a name="overview"></a>Áttekintés ##
A művelet csoportok lehetővé teszik a tooconfigure fogadók listáját. Ezek a csoportok majd kihasználható, napló tevékenységriasztásokat; meghatározásakor Győződjön meg arról, hogy egy adott művelet csoport értesítést hello tevékenység napló figyelmeztetés jelenik meg. Riasztási mechanizmusok támogatott hello egyik SMS; hello riasztások kétirányú kommunikáció támogatja. A felhasználók válaszolhassanak tooan riasztást:

- **Riasztások lemondani:** A felhasználó az összes művelet csoportot, vagy egyes számú művelet csoport összes SMS riasztásokból lemondhatja.  
- **Resubscribe tooalerts:** egy felhasználó is resubscribe tooall SMS riasztások összes művelet csoportot, vagy egyes számú művelet csoport.  
- **Segítség kérése:** egy felhasználó hello SMS további információ teheti fel. Átirányított toothis cikk fogja

Ez a cikk ismerteti hello SMS riasztások hello viselkedését, és hello válasz műveletek hello felhasználói vehet igénybe alapján hello területi hello felhasználó:

## <a name="usacanada-sms-behavior"></a>Amerikai Egyesült Államok vagy Kanada SMS viselkedése
### <a name="receiving-an-sms-alert"></a>SMS riasztást fogadása
Az SMS fogadó, akik egy művelet csoport részeként van konfigurálva, az SMS kap, ha a riasztás akkor következik be. hello SMS fogja hajtani a következő információ hello:
* Hello művelet csoport rövid_név ezt a figyelmeztetést küldött
- Cím hello riasztás

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Az SMS riasztásokból egy művelet csoport unsubscribing
A felhasználó leiratkozhat SMS az értesítések egy-egy művelettel csoport által válaszol toohello shortcode 20873 hello kulcsszavakkal: "LETILTÁSA &lt;művelet csoport rövid_név&gt;".

Példa Az SMS-toohello shortcode 20873, amely szerint "Azure tiltsa le a" hello rövid_név "Azure", a művelet csoportok riasztásaiból toounsubscribe kívánó felhasználó elküldése

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Az SMS riasztásokból az összes művelet csoport unsubscribing
A felhasználó által válaszol toohello shortcode 20873 sem a következő kulcsszavak hello leiratkozhat az összes művelet csoport összes SMS riasztás:
* ÁLLJ

Példa A felhasználó toounsubscribe az összes SMS riasztásokból az összes művelet csoport, amely az SMS-toohello shortcode 20873, amely szerint a "STOP" elküldése

>[!NOTE]
>Ha egy felhasználó leiratkozott a SMS riasztást küld, de kerül tooa új művelet csoport; RENDSZER SMS figyelmeztetést a művelet új csoport, hanem marad a művelet minden olyan csoportból korábbi művelet.
>
>

### <a name="resubscribing-toosms-alerts-for-one-action-group"></a>Egy művelet csoport resubscribing tooSMS riasztások
A felhasználó által válaszol toohello shortcode 20873 hello kulcsszavakkal is resubscribe tooSMS az értesítések egy-egy művelettel csoport: "engedélyezése &lt;művelet csoport rövid_név&gt;".

Példa A felhasználó tooresubscribe tooalerts egy művelet csoport a hello rövid_név "Azure", amely az SMS-toohello shortcode 20873, amely szerint "Engedélyezése Azure" elküldése

### <a name="resubscribing-toosms-alerts-for-all-action-groups"></a>Az összes művelet csoport resubscribing tooSMS riasztások
A felhasználó által válaszol toohello shortcode 20873 sem a következő kulcsszavak hello is resubscribe tooall SMS az összes művelet csoport riasztásokhoz:

* INDÍTSA EL

Példa A felhasználó toounsubscribe az összes SMS riasztásokból az összes művelet csoport, amely az SMS-toohello shortcode 20873, amely szerint a "START" elküldése

### <a name="requesting-help-via-sms"></a>SMS segítségkérés
A felhasználó hello SMS további információ teheti fel, kaptak a következő kulcsszavak hello egyik válaszol toohello shortcode 20873 által:
* SEGÍTSÉG

A hivatkozás toothis cikk toohello felhasználó küld egy választ.

## <a name="next-steps"></a>Következő lépések
Első egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md) és megtudhatja, hogyan tooget riasztást  
További információ [SMS sebesség korlátozása](monitoring-alerts-rate-limiting.md)  
További információ [művelet csoportok](monitoring-action-groups.md)
