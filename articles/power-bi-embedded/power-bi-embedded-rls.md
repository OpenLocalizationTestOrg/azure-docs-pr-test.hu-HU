---
title: "a Power BI Embedded aaaRow szintű biztonság"
description: "A Power BI Embedded sorszintű biztonsággal kapcsolatos részletek"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 7936ade5-2c75-435b-8314-ea7ca815867a
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 384f78826ecc710cf8f101b251ae68b074f3e98b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a>Sorszintű biztonság a Power BI Embedded szolgáltatással

Sorszintű biztonság (RLS) használt toorestrict felhasználói hozzáférés tooparticular adatok egy jelentést vagy adatkészletet, így a több különböző felhasználók toouse hello ugyanabban a jelentésben minden megnéz különböző adatainak közben lehet. A Power BI Embedded mostantól támogatja az RLS konfigurált adatkészletek.

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

RLS rendelés tootake előnyeit fontos tisztában három fő alapelv; Felhasználók, szerepkörök és szabályok. Megtudhatja, hogy minden egyes részletes bemutatása:

**Felhasználók** – ezek hello tényleges végfelhasználók tekinti meg jelentéseket. A Power BI Embedded hello username tulajdonság egy alkalmazási jogkivonatot a felhasználók azonosítja.

**Szerepkörök** – felhasználó tooroles tartozik. Egy szerepkör szabályok tárolója, és képes neve például "Értékesítési igazgató" vagy "Értékesítési Rep". A Power BI Embedded felhasználók hello szerepkörök tulajdonságait egy alkalmazási jogkivonatot azonosítják.

**Szabályok** – szerepkörök rendelkezik szabályokat, és ezeket a szabályokat, hogy alkalmazva toobe toohello adatok hello tényleges szűrők. Ennek oka lehet egyszerűen "ország = USA" vagy más sokkal több dinamikus.

### <a name="example"></a>Példa

Ez a cikk többi hello a szerzői RLS, és majd fel, amely egy beágyazott alkalmazásban például következőkhöz nyújtunk. A példában hello [kiskereskedelmi elemzési minta](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-fájl.

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

A kiskereskedelmi elemzési minta értékesítés összes hello tárolóinak egy adott kereskedelmi lánc jeleníti meg. Nélkül RLS függetlenül attól, milyen körzeti manager jelentkezik be, és nézetek jelentés hello, akkor fogja látni hello ugyanazokat az adatokat. Minden körzet manager csak kell megjelennie hello forgalmi kezelnek hello áruházak, és a toodo ez vezető felügyeleti megállapítása szerint, RLS is használhatók.

RLS állította össze a Power BI Desktopban. Megnyitásakor hello adathalmazt és a jelentést, azt átválthatja toosee hello séma toodiagram megtekintése:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

Az alábbiakban néhány dolgot toonotice a sémát:

* Az összes mérték, például **összes értékesítés**, hello tárolódnak **értékesítési** ténytábla.
* Nincsenek négy további kapcsolódó dimenziótáblák: **elem**, **idő**, **tároló**, és **körzeti**.
* hello nyilak hello kapcsolat sorok szűrők áramolhasson az egyik tábla tooanother hogyan jelzi. Például, ha a szűrőt el van helyezve **idő [dátum]**, hello aktuális sémában azt fog csak szűrni le hello értékek **értékesítési** tábla. Más táblák befolyásolhat a szűrő óta az összes hello nyilak hello kapcsolat sorok pont toohello értékesítési táblában, és nem azonnal.
* Hello **körzeti** táblázat azt jelzi, aki hello manager minden körzet:
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

A séma alapján, ha egy szűrő toohello érvénybe lépni **körzeti Manager** oszlopa hello körzeti tábla, és ha ez a szűrő hello a jelentés megtekintése hello felhasználói megegyezik, a szűrő is szűrheti le hello **tároló**és **értékesítési** táblák tooonly jeleníthetők meg, hogy adott körzeti manager.

Itt módját:

1. Hello modellezési lapján, kattintson a **szerepkörök kezelése**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. Hozzon létre egy új szerepkör neve **Manager**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. A hello **körzeti** tábla adja meg a következő DAX-kifejezése hello: **[körzeti Manager] = USERNAME()**  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. toomake meg arról, hogy hello szabályok dolgoznak, hello **modellezési** lapra, majd **szerepkörök nézetként**, majd adja meg a következő hello:  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   hello jelentések most adatait jeleníti meg, ha be volt jelentkezve mint **Andrew Ma**.

Minden rekordot hello le alkalmazása hello szűrőt, akkor azt itt volt hello módon szűrheti **körzeti**, **tároló**, és **értékesítési** táblákat. Hello szűrés irányának a hello közötti kapcsolatok miatt azonban **értékesítési** és **idő**, **értékesítési** és **elem**, és **Elem** és **idő** táblák nem lesznek szűrve le.

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

Amely ehhez a követelményhez ok lehet, azonban kezelők toosee elem, amelynek nem rendelkeznek bármely értékesítési nem szeretnénk, ha azt sikerült kapcsolja be a kétirányú keresztszűrés hello kapcsolat és a folyamat hello biztonsági szűrőhöz mindkét irányban. Hello közötti kapcsolat szerkesztésével ehhez **értékesítési** és **elem**, ez például:

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

Most, szűrők is is haladjanak hello értékesítési tábla toohello **elem** tábla:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> Az adatok használata a DirectQuery mód, tooenable kétirányú-szűrés e két beállítás kiválasztásával határokon lesz szüksége:

1. **Fájl** -> **lehetőségek és beállítások** -> **előzetes verziójú funkciók** -> **a kétirányú keresztszűrés engedélyezése DirectQuery**.
2. **Fájl** -> **lehetőségek és beállítások** -> **DirectQuery** -> **DirectQuerymódbannemkorlátozottmértékengedélyezése**.

További információk a kétirányú keresztszűrés, letöltési hello toolearn [kétirányú keresztszűrés a SQL Server Analysis Services 2016 és a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) tanulmány.

Ez foglalja össze összes hello munka toobe a Power BI Desktop kész, de egy további munkákat, amelyet a kész toobe toomake hello RLS szabályok a Power BI Embedded munkahelyi meghatározott. A felhasználók hitelesítése és az alkalmazás által engedélyezett, és alkalmazási jogkivonatok használt toogrant, amelyek felhasználói hozzáférés tooa adott Power BI Embedded jelentést készítenek. A Power BI Embedded nem rendelkezik a felhasználó ki van információk. RLS toowork kell toopass néhány további környezet az alkalmazások jogkivonat részeként:

* **felhasználónév** (opcionális) – használja az RLS Ez egy használható toohelp hello felhasználó azonosítása, RLS szabályok alkalmazásakor. Lásd a biztonság a Power BI Embedded sor segítségével:
* **szerepkörök** – hello szerepkörök tooselect tartalmazhat a sorszintű biztonság szabályok alkalmazásakor. Ha több szerepkör, át kell őket, egy karakterlánc-tömbben.

Hello segítségével hoz létre hello token [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metódust. Ha hello username tulajdonság meg adva, is át kell legalább egy értéket a szerepkört.

Például megváltoztathatja hello EmbedSample. A DashboardController sor 55 frissítése sikerült

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

erre:

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

hello teljes alkalmazási jogkivonatot fog kinéznie:

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

Most az összes hello darabokat talál együtt, ha valaki be az alkalmazás tooview jelentkezik ez a jelentés azokat csak lesz képes toosee hello adatokat, hogy azok csatlakozhatnának a toosee, meghatározása szerint a sorszintű biztonság.

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Lásd még:

[A sorszintű biztonságot (RLS) rendelkező Power](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)

