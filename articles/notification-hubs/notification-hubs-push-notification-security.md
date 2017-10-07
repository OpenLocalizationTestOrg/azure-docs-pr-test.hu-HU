---
title: a Notification Hubs aaaSecurity
description: "Ez a témakör ismerteti az Azure notification hubs használatával biztonságát."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f59ad4594c2c0a2e2b22ab0b6d6bad53825a4dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security"></a>Biztonság
## <a name="overview"></a>Áttekintés
Ez a témakör ismerteti az Azure Notification Hubs hello biztonsági modellt. Mivel a Notification Hubs egy Szolgáltatásbusz-entitás, valósítják meg hello mint a Service Bus ugyanazt biztonsági modellt. További információkért lásd: hello [Service Bus hitelesítési](https://msdn.microsoft.com/library/azure/dn155925.aspx) témaköröket.

## <a name="shared-access-signature-security-sas"></a>Megosztott hozzáférési aláírást biztonsági (SAS)
A Notification Hubs megvalósítja az entitás szintű biztonsági rendszer SAS (közös hozzáférésű Jogosultságkód) néven ismert. Ez a séma lehetővé teszi, hogy az üzenetküldési entitások toodeclare too12 engedélyezési szabályok a leírásában, amely jogot adni, hogy az entitás.

Minden egyes szabály egy nevet, egy kulcs-érték (közös titkos kulcs) és olyan jogokkal, tartalmaz "Biztonsági jogcímeinek." hello szakaszban leírtak szerint Ha létrehoz egy értesítési központot, két szabály automatikusan létrejönnek: egy figyelési jogokkal (azaz a hello ügyfél alkalmazás által használt), és egy, az összes jogokkal (hello app háttér használ).

Végrehajtásakor regisztrációs felügyeleti ügyfél alkalmazásokból, ha hello információk keresztül küldött értesítések nem érzékeny (például időjárási frissítések), a közös módon tooaccess egy értesítési központot toogive hello kulcs értéke hello szabály-figyelési hozzáférés toohello ügyfélalkalmazás és toogive hello kulcs értéke hello szabály teljes körű hozzáférési toohello háttéralkalmazás.

Nem ajánlott, hogy az ügyfél a Windows Áruházbeli alkalmazások beágyazása hello kulcs értékét. Egy módon tooavoid beágyazás hello kulcsérték van toohave hello ügyfélalkalmazás le azt hello háttéralkalmazás indításkor.

Fontos, hogy a figyelési hozzáféréssel rendelkező kulcs hello toounderstand lehetővé teszi, hogy egy ügyfél app tooregister bármely címke. Ha az alkalmazás korlátoznia kell a regisztrációkat toospecific címkék toospecific ügyfelek (például, ha a címkék képviselő felhasználói azonosítók), majd a háttéralkalmazás hello regisztrációk kell elvégeznie. További információkért tekintse meg a regisztrációs felügyeleti. Ne feledje, hogy ezzel a módszerrel hello ügyfélalkalmazás fog nem közvetlen hozzáférést tooNotification hubok.

## <a name="security-claims"></a>Biztonsági jogcímek
Hasonló tooother entitások, értesítési központ műveletek végezhetők a három biztonsági jogcímek: figyelésére, küldése és kezelését.

| Jogcím | Leírás | Engedélyezett műveletek |
| --- | --- | --- |
| Figyelés |Létrehozása/frissítése, olvassa el és egyetlen regisztráció törlése |Regisztrációs létrehozása/frissítése<br><br>Olvassa el a regisztrációs<br><br>Olvassa el az összes regisztrációját a számára egy.<br><br>Regisztráció törlése |
| Küldés |Küldjön üzeneteket toohello értesítési központ |Üzenet küldése |
| Kezelés |A Notification Hubs (beleértve a PNS hitelesítő adatokat, és a biztonsági kulcsok frissítése), és a címkék alapján olvasható regisztráció cRUDs |Létrehozás/frissítés/olvasás/törlés értesítési központok<br><br>Olvassa el a regisztrációk címke szerint |

Notification hubs használatával a Microsoft Azure Access Control jogkivonatokat, és közvetlenül hello értesítési központ konfigurálva megosztott kulccsal rendelkező előállított aláírás jogkivonatokat kap jogcímeket elfogadni.

