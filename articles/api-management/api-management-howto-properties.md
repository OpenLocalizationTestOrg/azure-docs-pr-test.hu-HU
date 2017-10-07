---
title: "az Azure API-felügyeleti házirendek aaaHow toouse tulajdonságai"
description: "Megtudhatja, hogyan toouse tulajdonságokat az Azure API-felügyeleti házirendek."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 1ff096deeb97543b48dcf1f40be9dbfcbcd09542
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-properties-in-azure-api-management-policies"></a>Hogyan toouse tulajdonságokat az Azure API-felügyeleti házirendek
API-felügyeleti házirendek, amelyek lehetővé teszik a hello publisher toochange hello viselkedését hello API konfigurálással hello rendszer hatékony képesség. Házirendek olyan hello kérésre egymás után végrehajtott utasítások gyűjteménye vagy egy API-t adott válaszokat. Házirend-utasításoknál szövegkonstans értékek, a házirend-kifejezések és a tulajdonságok használatával lehet létrehozni. 

Minden API Management szolgáltatáspéldány kulcs/érték párok, amelyek globális toohello szolgáltatáspéldány tulajdonságok gyűjteményével rendelkezik. Ezek a tulajdonságok összes API konfigurálása és házirendek lehet használt toomanage állandó karakterlánc-értékek. Minden egyes tulajdonsága a következő attribútumok hello rendelkezik.

| Attribútum | Típus | Leírás |
| --- | --- | --- |
| Név |Karakterlánc |hello tulajdonság hello neve. Ez előfordulhat, hogy csak betűket, számokat, időszak, kötőjelet tartalmazhat, és aláhúzás karaktereket tartalmazhat. |
| Érték |Karakterlánc |hello hello tulajdonság értéke. Nem lehet üres vagy csak szóközök állhatnak. |
| Titkos |Logikai érték |Meghatározza, hogy hello érték titkos kulcs-e, és hogy titkosítani kell-e. |
| Címkék |A karakterlánc tömbje |Nem kötelező, hogy címkéket, amikor megadja a használt toofilter hello keresésitulajdonság-lista lehet. |

Van beállítva a hello hello publisher portálról **tulajdonságok** fülre. A következő példa hello három tulajdonságainak vannak konfigurálva.

![Tulajdonságok][api-management-properties]

A tulajdonság értékek tartalmazhatnak szövegkonstansok és [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx). hello következő táblázatban hello előző három minta tulajdonságok és attribútumaik. hello értékének `ExpressionProperty` a házirend-kifejezést, amely visszaadja a karakterláncot, amely tartalmazza az aktuális dátum és idő hello. hello tulajdonság `ContosoHeaderValue` titkos kulcs, van megjelölve, ezért az értéke nem jelenik meg.

| Név | Érték | Titkos | Címkék |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False (Hamis) |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True (Igaz) |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False (Hamis) | |

## <a name="toouse-a-property"></a>toouse tulajdonság
toouse házirend tulajdonság, hely hello tulajdonságnév belül dupla párban kell használni, például `{{ContosoHeader}}`, ahogy az alábbi példa hello.

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

Ebben a példában `ContosoHeader` a fejléc neveként hello szolgál egy `set-header` házirend, és `ContosoHeaderValue` , hogy a fejléc értékeként hello szolgál. Ezzel a házirend-kérés vagy válasz toohello API Management az átjáró során kiértékelésekor `{{ContosoHeader}}` és `{{ContosoHeaderValue}}` váltják fel az adott tulajdonságot értékekre.

Tulajdonságok teljes attribútumot vagy elemet értékek hello előző példában látható módon használható, de is lehet beszúrt vagy együtt egy szövegkonstans kifejezés része, ahogy az alábbi példa hello:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Tulajdonságok házirend kifejezést is tartalmazhat. A következő példa hello, hello `ExpressionProperty` szolgál.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Ez a házirend kiértékeléséhez `{{ExpressionProperty}}` értéke helyére: `@(DateTime.Now.ToString())`. Hello értéke egy házirend-kifejezést, mert hello van kiértékeli, és a végrehajtása a hello házirendet, majd.

Tesztelheti a kimenő hello developer portálon egy műveletet, amelynek hatókörében szerepel egy házirend tulajdonságokkal meghívásával. A következő példa hello, egy művelet hívása hello két előző példa `set-header` házirendek tulajdonságokkal. Vegye figyelembe, hogy a hello válasz házirendekkel tulajdonságokkal konfigurált, két egyéni fejléc tartalmazza.

![Fejlesztői portál][api-management-send-results]

Ha megnézzük hello [API Inspector nyomkövetési](api-management-howto-api-inspector.md) hello két korábbi minta szabályzatok tulajdonságait tartalmazó hívásra, lásd: a két hello `set-header` beszúrni, valamint hello házirend-kifejezést hello tulajdonságértékek szabályzatok hello tulajdonság hello házirend-kifejezést tartalmazó kiértékelését.

![API-Inspector nyomkövetési][api-management-api-inspector-trace]

Vegye figyelembe, hogy közben a tulajdonságértékek házirend-kifejezést tartalmazhat, tulajdonságértékek nem tartalmazhat más tulajdonságok. Ha a szöveg tulajdonság a hivatkozást használja a tulajdonság értéke, például a `Property value text {{MyProperty}}`, hogy tulajdonsághivatkozás nem lehet írni, és tartalmazzák a hello tulajdonság értéke lesz.

## <a name="toocreate-a-property"></a>toocreate tulajdonság
toocreate tulajdonság, kattintson a **tulajdonság hozzáadása** a hello **tulajdonságok** fülre.

![Tulajdonság hozzáadása][api-management-properties-add-property-menu]

**Név** és **érték** szükséges érték. Ha ez a tulajdonság értéke egy titkos kulcsot, ellenőrizze a hello **Ez a titkos kulcs** jelölőnégyzetet. Adjon meg egy vagy több választható címkék toohelp való rendszerezését a tulajdonságokat, majd kattintson **mentése**.

![Tulajdonság hozzáadása][api-management-properties-add-property]

Új tulajdonság mentésekor a rendszer hello **tulajdonság keresése** szövegmező hello hello új tulajdonság neve a telepítéskor és hello új tulajdonság jelenik meg. toodisplay összes tulajdonság, törölje a jelet hello **tulajdonság keresése** szövegmező, és nyomja le az adja meg.

![Tulajdonságok][api-management-properties-property-saved]

Hello REST API használatával tulajdonság létrehozásával kapcsolatos további információkért lásd: [hello REST API használatával tulajdonság létrehozása](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="tooedit-a-property"></a>tooedit tulajdonság
egy tulajdonság tooedit kattintson **szerkesztése** hello tulajdonság tooedit mellett.

![Tulajdonság szerkesztése][api-management-properties-edit]

Végezze el a szükséges módosításokat, és kattintson a **mentése**. Hello tulajdonságnév módosításakor a házirendekben, amelyek az adott tulajdonsághoz hivatkoznak automatikusan frissített toouse hello új nevet.

![Tulajdonság szerkesztése][api-management-properties-edit-property]

Hello REST API használatával tulajdonság szerkesztési információkért lásd: [hello REST API használatával tulajdonság módosítása](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="toodelete-a-property"></a>toodelete tulajdonság
egy tulajdonság toodelete kattintson **törlése** hello tulajdonság toodelete mellett.

![Tulajdonság törlése][api-management-properties-delete]

Kattintson a **Igen, törölje azt** tooconfirm.

![Törlés megerősítése][api-management-delete-confirm]

> [!IMPORTANT]
> Ha hello tulajdonság házirendekben hivatkozik, akkor nem lehet toosuccessfully törlése amíg hello tulajdonság távolít el, az azt használó összes házirendet.
> 
> 

Hello REST API használatával tulajdonság törléséről további információkért lásd: [törölni egy tulajdonságot hello REST API használatával](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="toosearch-and-filter-properties"></a>toosearch és szűrő tulajdonságai
Hello **tulajdonságok** lapján található, a Keresés és szűrés képességek toohelp kezelheti a tulajdonságokat. toofilter hello keresésitulajdonság-lista tulajdonság nevét, adja meg a kívánt keresőkifejezést hello **tulajdonság keresése** szövegmező. toodisplay összes tulajdonság, törölje a jelet hello **tulajdonság keresése** szövegmező, és nyomja le az adja meg.

![Keresés][api-management-properties-search]

toofilter hello keresésitulajdonság-lista által előfizetéscímkék értékeit, adjon meg egy vagy több címke hello **címkék szűrés** szövegmező. toodisplay összes tulajdonság, törölje a jelet hello **címkék szűrés** szövegmező, és nyomja le az adja meg.

![Szűrés][api-management-properties-filter]

## <a name="next-steps"></a>Következő lépések
* További információ a házirendek használata
  * [Az API Management házirendek](api-management-howto-policies.md)
  * [Házirend-referencia](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Áttekintő videó megtekintése
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

