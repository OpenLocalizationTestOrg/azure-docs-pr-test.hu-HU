---
title: "Logic Apps-összekötők aaaOverview |} Microsoft Docs"
description: "A logikai alkalmazás használható összekötők áttekintése"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: ca8dab2e-9b69-4b1e-865d-1facd9f0cdac
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan
ms.openlocfilehash: dc4cac4c0dfaa2f9fd218ffc04414fa8e52a91d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-in-a-logic-app"></a>A logikai alkalmazás összekötők használata
Összekötők szolgáltatások, a protokollok és a platformok közötti adja meg a gyors hozzáférés tooevents, az adatok és a műveletek.  hello teljes listáját, amely támogatja a Logic Apps összekötők is [itt található](apis-list.md).  Összekötők változtatás nélkül használhatók egy eseményindító vagy a logikai alkalmazás egy műveletet, és előfordulhat, hogy a konfigurált *kapcsolat* toouse (például: egy Twitter engedélyező tooaccess fiókot, vagy az Ön nevében post).

## <a name="basics"></a>Alapvető beállítások
Összekötők a legtöbb esetben érheti el más szolgáltatásokkal, Dynamics, az Azure, a Salesforce, például a logic app toointegrate részeként üzemeltetett szolgáltatások [és további](apis-list.md).  Ezek telepített és Microsoft által felügyelt, a méretezés, a teljesítményt és a biztonsági áll integrációs munkafolyamatok hozhat létre.  Indítás alatt vagy a Keresés és egy összekötő művelet kiválasztása összekötő tooa logikai alkalmazás hozzá **megjelenítése Microsoft felügyelt API-k**.

![Az eseményindító kiválasztásáról művelet menü][1]

Minden egyes összekötőt művelet vagy az eseményindító lesz a Tulajdonságok tooconfigure készletét.  Kattintson a hello info gombok toolearn további információk a művelet, vagy a dokumentáció [további toolearn](apis-list.md).

Ha azt szeretné, hogy a szolgáltatás vagy API-t, amely nem még összekötő toointegrate, a logic apps segítségével is kiterjesztheti a [egyéni összekötő](../logic-apps/logic-apps-create-api-app.md) vagy csak hívja közvetlenül toohello szolgáltatás egy például a HTTP protokollon keresztül.

## <a name="triggers"></a>Eseményindítók
Összekötők rendelkezik egy eseményindítót, ami azt jelenti, egy eseményt, amelyek a rendszer logikai alkalmazás tűz- és hello eseményindító részeként adjon át adatokat.  Egy eseményindító nem mindig hello első lépése a logikai alkalmazást.  Népszerű eseményindítók olyan műveleteket, például:

* Ismétlődés - minden órában futtatva
* HTTP-kérés fogadásakor
* Amikor egy elem szerepel-e tooa várólista
* Amikor egy e-mailt érkezik.

Néhány eseményindítók érvényesítést fog hello azonnali esemény értesítési toohello logikai alkalmazás keresztül történik, és mások egy ismétlési időköz, milyen gyakran hello logikai alkalmazás ellenőrzi az esemény (felfelé tooevery 15 másodperc) hello szolgáltatást konfigurálni kell.  

Miután egy esemény érkezik, hello logikai alkalmazást futtatni fogja érvényesítést, és hello műveletek hello munkafolyamat elindul.  Hello munkafolyamaton keresztül indítás hello összes adatát képes tooaccess fogja (az "On egy új tweetet" Példa hello eseményindító fogja továbbítani hello tweetet történő futtatása hello).

## <a name="actions"></a>Műveletek
A legtöbb összekötők hello munkafolyamat részeként hajt végre egy vagy több műveletet kell végrehajtani.  A műveletek olyan lépéseket hello futtatása után kerül sor egy elindult.  tooadd művelet kattintson hello **új lépés** gombra, majd keresse meg a hello toouse kívánt összekötőt.  Egyszer kiválasztva (és bármely konfigurálása után [kapcsolatok](#connections) , amely segítségére lesz szüksége) látni fogja a hello művelet kártya is beállíthat.  Előző lépéseiből adatok bármely kimenetek hello jogkivonatainak kattintva válassza ki, vagy igény szerint adjon meg más beállításokat.

![Egy összekötő művelet konfigurálása][2]

## <a name="connections"></a>Kapcsolatok
A legtöbb összekötők kell tooconfigure egy *kapcsolat* hello összekötő használata előtt.  A *kapcsolat* bármely bejelentkezés vagy kapcsolat konfigurációs tooaccess hello összekötő szükséges.  Az összekötők, amely OAuth használja, hozza létre a kapcsolat azt jelenti, hogy bejelentkezik hello szolgáltatást (például Office 365, a Salesforce vagy a Githubon) Ha titkosított és biztonságosan tárolhatók a Azure titkos tárolóban a a hozzáférési jogkivonat.  Más összekötők (például FTP- és SQL) konfigurációt, például a kiszolgáló címét, a felhasználónevet és jelszót tartalmazó kapcsolat szükséges.  Ezen kapcsolat konfigurációs adatok is titkosítást kapnak, és biztonságosan tárolhatók.  Kapcsolat lesz képes tooaccess hello szolgáltatást, amíg hello szolgáltatás lehetővé teszi.  Az Azure Active Directory OAuth-kapcsolatok (például Office 365 és a Dynamics) Folytatás toorefresh hello hozzáférési jogkivonat határozatlan ideig.  Egyéb szolgáltatások korlátok elhelyezése lehetséges, hogy mennyi ideig használhatjuk jogkivonat frissítése nélkül.  Általában bizonyos műveletek, például a jelszó módosítása az összes hozzáférési jogkivonatok érvényteleníti.  

Kapcsolatok tekinthetők meg és kattintson az Azure-ban kezelt **Tallózás** választja **API kapcsolatok**.  Az API-kapcsolatok erőforrás hello megtekintése, szerkesztése, frissíteni vagy újra engedélyezni, a létrehozott kapcsolatokat.

## <a name="next-steps"></a>Következő lépések
* [Az első logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)
* [Ismerje meg a közös használ és példákat a logic Apps alkalmazások](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Ismerkedés a vállalati integrációs eseményindítók és műveletek](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png
