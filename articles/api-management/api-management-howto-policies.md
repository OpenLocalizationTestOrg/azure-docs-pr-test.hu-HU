---
title: az Azure API Management aaaPolicies |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate, szerkesztése és API-felügyeleti szabályzatok konfigurálására."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 9ab0f884a655004cb10c05085034df1795f512e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="policies-in-azure-api-management"></a>Házirendek az Azure API Management
Az Azure API Management a házirendek, amelyek lehetővé teszik a hello publisher toochange hello viselkedését hello API konfigurálással hello rendszer hatékony képesség. Házirendek olyan hello kérésre egymás után végrehajtott utasítások gyűjteménye vagy egy API-t adott válaszokat. Népszerű utasítások XML tooJSON formátumú konverzió tartalmazza, és hívja meg a fejlesztők a bejövő hívások toorestrict hello mennyisége sebességével. Számos további házirendeket hello kezdő verzióról érhetők el.

Lásd: hello [szabályzataihoz] [ Policy Reference] házirend-utasításoknál és a beállítások teljes listáját.

Házirendek hello átjárón, amely hello API fogyasztói és felügyelt hello API között helyezkedik el belül érvényesek. hello átjáró összes kéréseket fogad, és általában továbbítja őket a változatlan toohello az alapul szolgáló API. A házirend azonban módosítások tooboth hello bejövő kérés- és kimenő is alkalmazhatja.

Házirend-kifejezések használható attribútumértékek vagy szöveges értékek bármely hello API-felügyeleti házirendek, kivéve, ha hello házirend ellenkező esetben adja meg. Egyes házirendek, például a hello [folyamatot szabályozhatja] [ Control flow] és [Set változó] [ Set variable] házirendek a házirend-kifejezések alapulnak. További információkért lásd: [házirendek speciális] [ Advanced policies] és [házirend-kifejezések][Policy expressions].

## <a name="scopes"></a>Hogyan tooconfigure házirendek
Házirendek úgy is konfigurálhatók, globálisan vagy hello hatókörét egy [termék][Product], [API] [ API] vagy [művelet] [Operation]. tooconfigure egy házirendet, keresse meg a toohello házirendek szerkesztő hello publisher portálon.

![Házirendek menü][policies-menu]

hello házirendek szerkesztő három fő részből áll: hello házirend hatókör (felső), hello házirend-definíció ahol házirendek szerkesztése (bal oldali) és hello utasítások listában (jobbra):

![Házirendek szerkesztő][policies-editor]

a toobegin először válassza ki, mely hello házirend vonatkozzon hello hatókör házirend beállítása. Hello képernyőfelvételen látható, alább hello **alapszintű** termék van kiválasztva. Vegye figyelembe, hogy hello négyzetes szimbólum következő toohello házirendnév azt jelzi, hogy egy házirend már alkalmazva van ezen a szinten.

![Hatókör][policies-scope]

Egy házirend már telepítve van, mert hello konfigurációs hello típusdefiníció nézet jelenik meg.

![Konfigurálás][policies-configure]

hello házirend írásvédett jelenik meg először. A sorrend tooedit hello definition kattintson hello **házirend konfigurálása** művelet.

![Szerkesztés][policies-edit]

hello házirend-definíció egy egyszerű XML-dokumentumban, amely leírja a bejövő és kimenő utasítás sorozata. hello XML közvetlenül hello definition ablak szerkeszthetők. Utasítások a listáját, feltéve toohello jobb utasítások alkalmazható toohello aktuális hatókör engedélyezett és a kiemelt; Amint azt a hello **korlát hívás arány** hello a képernyőfelvételen látható a fenti utasítást.

Kattintson egy engedélyezett utasítás felveszi hello megfelelő XML hello helyen hello kurzor hello definition nézetben. 

> [!NOTE]
> Ha hello házirend, amelyet az tooadd nincs engedélyezve, ellenőrizze, hogy hello a házirendhez tartozó megfelelő hatókörben. Minden egyes házirend-utasítás az egyes hatókörök és házirend szakaszok használatra szolgál. tooreview hello házirend szakaszok és egy házirend hatókörök ellenőrizze hello **használati** hello a házirendhez tartozó szakasz [szabályzataihoz][Policy Reference].
> 
> 

A házirend-utasításoknál teljes listáját és a beállítások érhetők el a hello [szabályzataihoz][Policy Reference].

Például tooadd egy új utasítás toorestrict bejövő kérelmek toospecified IP-címek, hello kurzorral hello hello tartalmának belülre `inbound` XML-elem, és kattintson hello **IP-címek korlátozása hívó** utasítást.

![A szoftverkorlátozó házirendek][policies-restrict]

Ez hozzáadja egy XML-részlet toohello `inbound` elem, hogyan tooconfigure hello utasítás nyújt útmutatást.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

toolimit a bejövő kéréseket, és fogadja el az alábbiak szerint módosítsa 1.2.3.4 IP-cím csak az hello XML:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Mentés][policies-save]

Amikor végzett hello utasítások hello házirend konfigurálásával, kattintson **mentése** és hello módosítások azonnal lesz propagált toohello API adatkezelési átjáró.

## <a name="sections"></a>Ismertetése házirend konfigurálása
A házirend utasításokat, amelyek ahhoz, hogy egy kérés- és a végrehajtása több. hello konfigurációs oszlik megfelelően `inbound`, `backend`, `outbound`, és `on-error` látható módon hello a következő konfigurációs szakasz.

```xml
<policies>
  <inbound>
    <!-- statements toobe applied toohello request go here -->
  </inbound>
  <backend>
    <!-- statements toobe applied before hello request is forwarded too
         hello backend service go here -->
  </backend>
  <outbound>
    <!-- statements toobe applied toohello response go here -->
  </outbound>
  <on-error>
    <!-- statements toobe applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Hello egy kérelem feldolgozása során hiba történik, ha a többi szükséges lépések hello `inbound`, `backend`, vagy `outbound` szakaszok kimarad, és végrehajtási áttérés toohello utasítások a hello `on-error` szakasz. Házirend-utasításoknál hello úgy `on-error` hello segítségével áttekintheti a hello hiba szakasz `context.LastError` tulajdonság, nézze meg, és testre szabhatja a hello hibaválaszba hello használata `set-body` házirend, és konfigurálása, mi történik, ha a hiba akkor fordul elő. Nincsenek hibakódok beépített lépéseket és a házirend-utasításoknál hello feldolgozása során előforduló hibákat. További információkért lásd: [hiba történt az API-felügyeleti házirendek kezelése](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Mivel a házirendek (globális, termék, api és művelet) különböző szinteken adható meg hello konfigurációs lehetőséget biztosít az Ön toospecify hello sorrendben, amelyben hello házirend-definíció utasításokat illetően toohello szülő irányelvnek hajtható végre. 

Házirend hatókörök sorrend hello kiértékelése.

1. Globális hatókörű
2. A termék hatókör
3. API-hatókör
4. Műveleti hatókör

hello rajtuk utasítás kiértékelése hello toohello elhelyezését szerint `base` elem, ha telepítve. Globális-házirendben engedélyezve van, nincs szülő házirend és hello segítségével `<base>` az elem nincs hatása.

Például ha egy házirend hello globális szinten és az API-k konfigurált házirendek, majd, hogy adott API-t igénybe mindkét házirendeket a rendszer alkalmazza. API-kezelés lehetővé teszi a kombinált házirend kimutatások keresztül hello a(z) determinisztikus rendezéshez. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

A fenti hello példa házirend definíció, hello `cross-domain` utasítás futtatása előtt minden magasabb házirendet, amely viszont hello követi `find-and-replace` házirend. 

toosee hello házirendek a jelenlegi hatókörben hello hello csoportházirend-szerkesztőben kattintson **számítsa ki újra a kijelölt hatókör hatékony házirend**.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő videót a házirend-kifejezések.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
