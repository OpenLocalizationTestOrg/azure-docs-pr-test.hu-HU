---
title: "aaaAzure API Management áttekintése és a kulcs fogalmak |} Microsoft Docs"
description: "Ismerje meg az API-kat, a termékeket, a szerepköröket, a csoportokat és az API Management többi alapfogalmát."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: e71da405-835a-48f3-956f-45c1a85698d7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a77b24b4632d868afa15bd6cf88060982046cb38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-api-management"></a>Mi az API Management?
Az API Management segít a szervezeteknek API-k tooexternal, a partner és a belső fejlesztők toounlock hello lehetséges az adatok és a szolgáltatások közzététele. Vállalatok mindenhol digitális platformként a műveletek tooextend keresése, új csatorna létrehozása, új ügyfelek keresése és befolyásoló tényezők mélyebb engagement a már meglévőket. Az API Management biztosít hello core szakértelem tooensure keresztül fejlesztői engagement, üzleti elemzéseket, elemzés, biztonság és védelem sikeres API programot.

A következő videó az Azure API Management áttekintése hello figyelje, és megtudhatja, hogyan toouse API Management tooadd sok szolgáltatások tooyour API-t, beleértve a hozzáférés-vezérlés, értékelje korlátozása, figyelés, eseménynaplózás és válasz gyorsítótárazást, ha meg a minimális munka.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-API-Management-Overview/player]
> 
> 

az API Management toouse, a rendszergazdák létrehozhatnak API-k. Egy vagy több műveletet minden egyes API áll, és minden API tooone vagy több terméket lehet hozzáadni. az API-k toouse, fejlesztők előfizetés tooa termék, amely tartalmazza az adott API, és majd akkor hívja az hello API műveletet, a tulajdonos tooany használati házirendek az érvényben.

Ez a témakör áttekintést nyújt az API Management alapfogalmairól.

> [!NOTE]
> További információkért lásd: hello [felhőalapú API Management: kifejlesztése hello API-k Power](http://j.mp/ms-apim-whitepaper) PDF tanulmány. A CITO Research által írt, az API Management szolgáltatást bemutató tanulmány az alábbiakat tárgyalja: 
> 
> * Gyakori API-követelmények és kihívások
> * Az API-k leválasztása és a homlokzatok bemutatása
> * A fejlesztők számára gyors és egyszerű munkakezdés
> * A hozzáférés biztonságossá tétele
> * Elemzések és mérőszámok
> * Ellenőrzés és áttekintés az API Management-platformmal
> * Felhőalapú és helyszíni megoldások használata
> * Azure API Management
> 
> 

## <a name="apis"></a>API-k és műveletek
API-jainak hello foundation API-kezelés szolgáltatás példány. Minden API műveletek elérhető toodevelopers készletét reprezentálja. Minden API tartalmaz egy hivatkozást toohello háttér-szolgáltatás, amely megvalósítja az hello API, és a műveletek hozzárendelését az hello háttér-szolgáltatás által megvalósított toohello műveletek. Az API Management műveletei részletesen konfigurálhatók, szabályozni lehet az URL-címmegfeleltetést, a lekérdezések és útvonalak paramétereit, a kérelmek és válaszok tartalmát, valamint a művelet válaszainak gyorsítótárazását. Sávszélesség-korlátjának, kvóták és IP-korlátozási házirendek hello API vagy egyedi művelet szintjén is végrehajtható.

További információkért lásd: [hogyan toocreate API-k] [ How toocreate APIs] és [hogyan tooadd műveletek tooan API][How tooadd operations tooan API].

## <a name="products"></a> Termékek
Hogyan API-jainak illesztve toodevelopers termékeket. Az API Management szolgáltatásban a termékek egy vagy több API-val rendelkeznek, emellett címmel, leírással és használati feltételekkel vannak konfigurálva. A termékeknek két típusa létezik: **Nyílt** és **Védett**. Védett termékek kell is használhatók, miközben a Megnyitás előfizetett toobefore termékek előfizetés nélkül is használható. Amikor egy termék készen áll a fejlesztők általi használatra, közzé lehet azt tenni. Miután közzé van téve, is megtekinthető (és a hello védett termékekkel előfizetett) fejlesztők. Előfizetés jóváhagyási hello termék szintjén állítható be és rendszergazdai jóváhagyás megkövetelése, vagy automatikusan jóváhagyta lehet.

Csoportok olyan termékek toodevelopers használt toomanage hello láthatóságát. Termékek adjon látható toogroups és fejlesztők tekintheti meg és előfizetés toohello terméket, amelyre látható toohello csoportok, amelyben tartoznak. 

További információkért lásd: [hogyan toocreate és közzététele egy termék] [ How toocreate and publish a product] és a következő videó hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="groups"></a> Csoportok
Csoportok olyan termékek toodevelopers használt toomanage hello láthatóságát. Az API Management követően nem módosítható rendszer csoportok hello rendelkezik.

* **Rendszergazdák** – A csoportot Azure-előfizető rendszergazdák alkotják. Rendszergazdák kezelése az API Management szolgáltatáspéldány, API-k, műveletek és a fejlesztők által használt termékek létrehozása hello.
* **Fejlesztők** – A fejlesztői portál hitelesített felhasználói tartoznak ebbe a csoportba. A fejlesztők olyan alkalmazásokat, az API-k használatával készíthetnek hello ügyfelek. A fejlesztők kapnak hozzáférést toohello fejlesztői portálján, és alkalmazások összeállítását, amelyek hívja az API-k hello működésére.
* **A vendégek** -hitelesítés nélküli developer portálon a felhasználóknak, például a lehetséges ügyfelek látogató hello fejlesztői portálján az API Management példány alá esik az ebbe a csoportba. Ezek is hozzáférést bizonyos csak olvasható, például a hello képességét tooview API-k, de nem hívható meg őket.

Toothese rendszer csoportok hozzáadása, a rendszergazdák egyéni csoportot hozhat létre vagy [kihasználja a társított Azure Active Directory-bérlők külső csoportok](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Egyéni és külső csoportok rendszer csoportok jogosultságot ad a fejlesztők számára látható együtt is használható, és tooAPI terméket. Például létrehozhat egy egyéni csoport tagja-e egy adott fejlesztők fiókpartner szervezetében dolgozó és a hozzáférés engedélyezése csak megfelelő API-k tartalmazó az API-k toohello. Egy felhasználó egyszerre több csoport tagja is lehet.

További információkért lásd: [hogyan toocreate és -felhasználási csoportok][How toocreate and use groups].

## <a name="developers"></a> Fejlesztők
A fejlesztők hello felhasználói fiókokat az API Management szolgáltatáspéldány jelölik. A fejlesztők hozhatók létre, vagy toojoin rendszergazdák által meghívott, vagy azok regisztrálhat a hello [fejlesztői portálján][Developer portal]. Minden fejlesztői egy vagy több csoport tagja, és lehet, amely adja meg a láthatóság toothose csoportok toohello termékek előfizetés.

Amikor a fejlesztők előfizetés tooa termék hello elsődleges és másodlagos kulcsot következő hello termék kapnak. Ezt a kulcsot használja az hello termék API-k hívása esetén.

További információkért lásd: [hogyan toocreate vagy a meghívott felhasználó fejlesztők] [ How toocreate or invite developers] és [hogyan tooassociate csoportok fejlesztőkkel][How tooassociate groups with developers].

## <a name="policies"></a> Házirendek
Házirendek, amelyek lehetővé teszik a hello publisher toochange hello viselkedését hello API keresztül konfigurációs API-kezelési hatékony képesség. Házirendek olyan hello kérésre egymás után végrehajtott utasítások gyűjteménye vagy egy API-t adott válaszokat. Népszerű kimutatások XML tooJSON és hívás sebességével toorestrict hello mennyiségű bejövő hívások a fejlesztőtől származó formátum konverzió tartalmaznak, és számos egyéb házirendek állnak rendelkezésre.

Házirend-kifejezések használható attribútumértékek vagy szöveges értékek bármely hello API-felügyeleti házirendek, kivéve, ha hello házirend ellenkező esetben adja meg. Egyes házirendek, például a hello [folyamatot szabályozhatja](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) és [Set változó](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) házirendek a házirend-kifejezések alapulnak. További információkért lásd: [házirendek speciális](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx), és ezáltal hello a következő videó.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

Az API Management házirendek teljes listájáért tekintse meg a [Házirend-referencia][Policy reference] szakaszt. További információ a házirendek használatáról és konfigurálásáról: [API Management házirendek][API Management policies]. Ha egy sebességkorlát- és kvótaházirendekkel rendelkező termék létrehozásához keres oktatóanyagot, tekintse meg a [Speciális termékbeállítások létrehozása és konfigurálása][How create and configure advanced product settings] című szakaszt. A bemutató tekintse meg a következő videó hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="developer-portal"></a> Fejlesztői portálon
hello fejlesztői portálján, ahol a fejlesztők további tudnivalók az API-k, megtekintése és hívás műveleteket, és feliratkozhat tooproducts. Lehetséges ügyfelek által felkereshető hello fejlesztői portálján, API-k és műveletek megtekintése, és regisztráljon. a fejlesztői portálján hello URL-címe a klasszikus Azure portál hello az API Management szolgáltatáspéldány hello irányítópulton található.

Testre szabhatja a fejlesztői portálján, hello megjelenését és működését egyéni tartalom-Hozzáadás stílusok testreszabása és a vállalati arculat hozzáadása.

## <a name="api-management-and-hello-api-economy"></a>Az API Management és hello API gazdaság
További információ az API Management, bemutató követően – hello Microsoft ignite-on 2015 konferencia figyelési hello toolearn.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3708/player]
> 
> 

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[How toocreate APIs]: api-management-howto-create-apis.md
[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[How create and configure advanced product settings]: api-management-howto-product-with-rules.md
[How toocreate or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance




