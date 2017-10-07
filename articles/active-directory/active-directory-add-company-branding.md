---
title: "aaaAdd vállalati arculat megjelenítése a tooyour bejelentkezési és hozzáférési Panel oldalakon"
description: "Ismerje meg, hogyan tooadd egy vállalati arculat Azure bejelentkezés toohello lap és hello hozzáférés panel oldalon"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/23/2017
ms.author: curtand
ms.openlocfilehash: b1c6442028393552bff3d380312c8cd1bfda5d31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-and-access-panel-pages"></a>Vállalati arculat megjelenítése a tooyour bejelentkezési és hozzáférési Panel oldalakon
tooavoid zavart, számos vállalat szeretné, hogy egy egységes megjelenést és működést tooapply összes hello webhelyek és szolgáltatások kezelnek. Az Azure Active Directory lehetővé teszik, hogy a következő webhelyek megjelenését a vállalat emblémájának elhelyezésével és egyéni színsémák hello toocustomize hello megjelenésének biztosítja ezt a lehetőséget:

* **Bejelentkezési oldal** -hello oldal jelenik meg, amikor bejelentkezik tooOffice 365 vagy más webes alkalmazások által használt Azure AD az identitás-szolgáltatóként. Ön a szolgáltatóosztályokkal ezen a lapon, a Hitelesítőtartomány vagy tooenter közben a hitelesítő adatait. hello Hitelesítőtartományának feltárási lehetővé teszi, hogy a hello rendszer tooredirect összevont felhasználók tootheir a helyszíni STS (például az AD FS).
* **Hozzáférési Panel oldala** – hello hozzáférési Panel egy webes portál, amely lehetővé teszi a tooview és indítási hello felhőalapú alkalmazások az Azure AD rendszergazdai jogokat engedélyezett Önnek a hozzáférést. tooaccess hello hozzáférési panelre, a következő URL-cím használata hello: [https://myapps.microsoft.com](https://myapps.microsoft.com).

Ez a témakör azt ismerteti, hogyan szabhatja testre a hello bejelentkezési oldal és hello hozzáférési panel oldala.

> [!NOTE]
> * Vállalati arculat megjelenítése egy szolgáltatás, amely csak akkor, ha frissített toohello prémium vagy alapszintű kiadására az Azure Active Directory, vagy az Office 365-felhasználó. További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).
> * Az Azure Active Directory prémium és alapszintű kiadásai érhetők el a kínai ügyfelek számára az Azure Active Directory hello világszerte példányát használja. Az Azure Active Directory prémium és alapszintű kiadásai jelenleg nem támogatottak Kínában a 21Vianet által működtetett hello Microsoft Azure szolgáltatásban. További információkért lépjen velünk kapcsolatba: hello [Azure Active Directory fórumán](https://feedback.azure.com/forums/169401-azure-active-directory/).
>
>

## <a name="customizing-hello-sign-in-page"></a>Hello bejelentkezési oldal testreszabása
Általában ha böngészőalapú hozzáférésre tooyour felhőalapú alkalmazások és szolgáltatások, amelyek a szervezet által előfizetett, van szüksége, használja hello bejelentkezési oldalára.

Módosítások tooyour bejelentkezési oldal alkalmazta, hello módosítások tooappear tooan órát is eltarthat.

A vállalati arculattal ellátott bejelentkezési oldal csak akkor jelenik meg, ha bérlőspecifikus URL-címmel (például https://outlook.com/**contoso**.com vagy https://mail.**contoso**.com) rendelkező szolgáltatást keres fel.

Nem bérlőspecifikus URL-címmel (például https://mail.office365.com) ellátott szolgáltatás felkeresésekor nem vállalati arculattal rendelkező bejelentkezési oldal jelenik meg. Ebben az esetben a vállalati arculat a felhasználói azonosító megadása és a felhasználói csempe kiválasztása után jelenik meg.

> [!NOTE]
> * A tartománynév szerepelnie kell "Aktív" hello **Active Directory** > **Directory** > **tartományok** hello a klasszikus Azure portálon szakasza Ha vállalati arculatot konfigurálta.
> * Bejelentkezési oldal vállalati arculata toohello ügyfél-bejelentkezési Microsoft oldalán nem jelenik. Ha személyes Microsoft-fiókkal jelentkezik be, láthatja az Azure AD által nyújtott felhasználói csempék vállalati arculattal ellátott listáját, de a szervezet branding hello nem érvényes toohello Microsoft-fiókja bejelentkezési oldalára.
>
>

Ha azt szeretné, hogy tooshow vállalatának arculatát, színeit és egyéb testreszabható elemeit ezen a lapon lásd a következő lemezképek toounderstand hello különbségének hello két megközelítés hello.

következő képernyőfelvételen látható és példán hello Office 365 bejelentkezési oldala egy asztali számítógépen hello **előtt** testreszabás:

![Az Office 365 bejelentkezési oldala a testreszabást megelőzően][1]

következő képernyőfelvételen látható és példán hello Office 365 bejelentkezési oldala egy asztali számítógépen hello **után** testreszabás:

![Az Office 365 bejelentkezési oldala a testreszabást követően][2]

hello alábbi képernyőfelvételen látható példa hello Office 365 bejelentkezési oldala a mobileszköz **előtt** testreszabás:

![Az Office 365 bejelentkezési oldala a testreszabást megelőzően][3]

hello alábbi képernyőfelvételen látható példa hello Office 365 bejelentkezési oldala a mobileszköz **után** testreszabás:

![Az Office 365 bejelentkezési oldala a testreszabást követően][4]

A böngészőablak átméretezésekor hello nagy méretű ábrát, például a hello egyik látható korábban, a rendszer gyakran levágja tooaccommodate különböző képernyő-méretarányoknak megfelelően. Ennek tudatában meg kell tookeep hello fontos vizuális elemeket hello ábrán látható, hogy mindig hello bal felső sarokban (jobb felső jobbról balra író nyelvek esetén). Ez azért fontos, mert az átméretezés jellemzően akkor fordul elő, hello jobb alsó sarok folyamatos felé hello felső / bal vagy a hello felső hello felfelé.

hello következő kép bemutatja, hogyan hello ábra levágását szemléltetjük hello böngésző balra átméretezett toohello:

![][6]

Íme, ahogy után hello böngésző átméretezték hello felső felé:

![][7]

## <a name="what-elements-on-hello-page-can-i-customize"></a>Hello oldalon elemeket testre szabhatók?
Testre szabható elemek hello bejelentkezési lapon a következő hello:

![][5]

| Oldalelem | Hello lap |
|:--- | --- |
| Szalagcímembléma |Hello jobb felső részén hello lapon látható. A felváltja hello embléma hello célhelyre bejelentkezés toodisplays (például. Office 365 vagy Azure) emblémáját. |
| Nagy méretű ábra/háttérszín |Hello lap hello balra látható. A felváltja hello kép hello célhelyre toodisplays bejelentkezés. hello háttérszínét hello nagy méretű ábrát alacsony sávszélességű kapcsolat vagy keskeny képernyő helyett előfordulhat, hogy jelennek meg. |
| Bejelentkezve szeretnék maradni |Hello jelszó szövegmezőjét mezőben látható. |
| A bejelentkezési oldal szövege |Fent hello oldal lábléce látható Ha tooconvey hasznos információkat, mielőtt egy jelentkezzen be munkahelyi vagy iskolai fiókkal van szükség. Például érdemes lehet tooinclude hello telefon száma tooyour támogatási szolgálat, vagy egy jogi nyilatkozatot. |

> [!NOTE]
> Ezen elemek egyike sem kötelező. Például ha megad Szalagcímemblémát, de nagy méretű ábrát nem, hello bejelentkezési oldala látható, az embléma és hello illusztráció a következőhöz: hello célhelyre (Ez azt jelenti, hogy hello Office 365 kaliforniai főutat ábrázoló képe).
>
>

A bejelentkezési oldalon hello **bejelentkezve szeretnék maradni** jelölőnégyzet lehetővé teszi, hogy egy felhasználó tooremain bejelentkezett zárja be, és nyissa meg újra a böngészőt. Ez nincs hatással a munkamenet élettartamára. Hello Azure Active Directory bejelentkezési oldal hello jelölőnégyzet elrejthetők.

Hello beállítása hello jelölőnégyzet megjelenítését függ **elrejtése KMSI**.

![][9]

toohide hello jelölőnégyzet, konfigurálja ezt a beállítást túl**rejtett**.

> [!NOTE]
> Néhány funkciójának Office 2010 és SharePoint Online attól függnek, hogy a felhasználók képesek toocheck ebben a mezőben. Ha ez a beállítás toohidden konfigurál, előfordulhat, hogy jelennek meg a felhasználók további és váratlan toosign a megjelenő utasításokat.
>
>

Az oldalon szereplő összes elem honosítható. A testreszabási összetevők „alapértelmezett” készletének összeállítását követően a különböző területi beállításokhoz további verziókat is konfigurálhat. A különböző elemek szabadon kombinálhatók. Megteheti például a következőt:

* Létrehozhat egy „alapértelmezett” nagy méretű ábrát, amely minden országban használható, majd hozza létre ennek angol és francia verzióját. Ha úgy állítja be a böngészők tooone két nyelv valamelyikére, hello adott kép jelenik meg, míg minden más nyelv hello alapértelmezett ábra jelenik meg.
* A vállalat eltérő verziójú (például japán vagy héber) emblémáit is beállíthatja.

## <a name="access-panel-page-customization"></a>A hozzáférési panel oldalának testreszabása
a gyors hozzáférés toohello felhőalkalmazások rendelkezik hozzáférési tooby a rendszergazda hello hozzáférési Panel oldalára alapvetően a portál lapján. Ezen az oldalon az alkalmazások kattintható alkalmazáscsempékként jelennek meg.

a következő képernyőkép hello testreszabást követően egy példát a hozzáférési panel oldalát mutatja.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>A címtár konfigurálása a vállalati arculattal
Címtáranként egy testreszabható elemek egy alapértelmezett készletét a hello a klasszikus Azure portálon konfigurálhatja. Hello alapértelmezett mentése után a rendszergazda adhat hozzá honosított az egyes elemek különböző nyelvekhez / területi beállításokhoz. A testre szabható elemek egyikének sem kötelező a használata.

Például ha konfigurál egy alapértelmezett Szalagcímemblémát, de nagy méretű ábrát nem, hello bejelentkezési oldal megjeleníti az embléma hello jobb felső sarkában található. Azonban hello hely hello alapértelmezett ábra jelenik meg.

Képzelje el a következő konfigurációs hello:

* A szalagcím alapértelmezett emblémája és a bejelentkezési oldal szövege angolul
* Nyelvspecifikus bejelentkezési oldal német nyelven

Ha a nyelvi beállításai német, hello alapértelmezett Szalagcímemblémát, de hello német szöveggel kap.

Technikailag konfigurálhatja egy másik készlet minden Azure AD által támogatott nyelv, de javasolt, hogy maradjon hello túl sok változat alacsony, a karbantartási és teljesítmény érdekében.

> [!IMPORTANT]
> Yammer nem az Azure AD-ig bejelentkezési oldal vállalati arculattal, hello felhasználó bejelentkezése után az megjelenítése hello végzi. először hello általános Office 365 bejelentkezési oldala, és ezután hello vállalati arculattal lap, amely után hello felhasználónál.   
 
 
**tooadd vállalati arculat tooyour könyvtár, hajtsa végre az alábbi lépésekkel hello:**

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) hello könyvtár rendszergazdaként toocustomize szeretné.
2. Válassza ki a kívánt toocustomize hello könyvtár.
3. Hello hello felső eszköztárán kattintson **konfigurálása**.
4. Kattintson a **Customize Branding** (Márkajelzés testreszabása) lehetőségre.
5. Módosítsa a kívánt toocustomize hello elemek. A mezők egyike sem kötelező.
6. Kattintson a **Save** (Mentés) gombra.

Is eltarthat, tooan órát, toohello bejelentkezési oldal vállalati arculatán alkalmazott tooappear végzett.

**tooadd nyelvspecifikus vállalati arculat megjelenítése, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) hello könyvtár rendszergazdaként toocustomize szeretné.
2. Válassza ki a kívánt toocustomize hello könyvtár.
fs3. Hello hello felső eszköztárán kattintson **konfigurálása**.
4. Kattintson a **Customize Branding** (Márkajelzés testreszabása) lehetőségre.
5. Kattintson az **Add branding for a specific language** (Márkajelzés hozzáadása adott nyelven) lehetőségre.
6. Válassza ki a hello kívánt nyelvre toocustomize hello embléma, majd kattintson a **következő**.
7. Csak a kívánt tooconfigure nyelvspecifikus felülbírálások hello elemek szerkesztése. A mezők egyike sem kötelező. Ha egy mező üresen marad, majd hello egyéni alapértelmezett érték kerül (vagy hello Microsoft alapértelmezett, ha egy egyéni nincsen konfigurálva).
8. Kattintson a **Save** (Mentés) gombra.

**tooremove vállalati arculat megjelenítése a könyvtárból, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) hello könyvtár rendszergazdaként toocustomize szeretné.
2. Válassza ki a kívánt toocustomize hello könyvtár.
3. Hello hello felső eszköztárán kattintson **konfigurálása**.
4. Kattintson a **Customize Branding** (Márkajelzés testreszabása) lehetőségre.
5. Hello testreszabásával foglalkozó oldalon válassza **meglévő márkajelzési beállítások szerkesztése** és folytassa a következő oldalon toohello.
6. Attól függően, hogy mely elemek szeretné, hogy tooremove, hajtson végre egy vagy több hello következő:

    a. A **Banner Logo** (Szalagcímembléma) területen válassza a **Remove uploaded logo** (Feltöltött embléma eltávolítása) lehetőséget.

    b. A **Tile Logo** (Csempe emblémája) területen válassza a **Remove uploaded logo** (Feltöltött embléma eltávolítása) lehetőséget.

    c. Távolítsa el a hello szöveget az összes szövegmezőből.

    d. Kattintson a **Tovább** gombra.

    e. Távolítsa el a hello szöveget az összes szövegmezőből.
7. Kattintson a **mentése** tooremove hello elemek.
8. Szükség esetén kattintson **testreszabásával foglalkozó** újra, majd ismételje meg ezeket a lépéseket az összes nyelvspecifikus márkajelzés esetében toobe eltávolítva.
    Összes márkajelzési beállítás el lettek távolítva kattintva **testreszabásával foglalkozó** és hello **testreszabásával foglalkozó alapértelmezett** nincs konfigurálva proxybeállításokkal űrlap.

## <a name="testing-and-examples"></a>Tesztelés és példák
Javasoljuk, hogy mielőtt éles környezetben hajtana végre módosításokat, próbálja ki azokat egy tesztelési bérlőn.

**tooverify e a márkajelzési beállításokat telepítve van:**

1. Nyisson meg egy InPrivate vagy Incognito böngésző-munkamenetet.
2. Látogasson el a https://outlook.com/contoso.com, ahol a contoso.com helyére a testreszabott hello tartomány.

Ez a contoso.onmicrosoft.com formátumú tartományok esetében is működik.

hatékony és megfelelő testreszabási készletek létrehozása toohelp, azt testreszabott a következő két fiktív bejelentkezési lapok hello:

* [http://aka.ms/aaddemo001](http://aka.ms/aaddemo001)
* [http://aka.ms/aaddemo002](http://aka.ms/aaddemo002)

tootest hello nyelvspecifikus beállítások, a webes böngésző tooa nyelven, a testreszabás során beállított szüksége toomodify hello alapértelmezett nyelvi beállítások. Az Internet Explorerben, konfigurálja a hello **Internetbeállítások** menü.

## <a name="customizable-elements"></a>Testreszabható elemek
Az Azure AD egyes testreszabható elemei többféleképpen is használhatók. Címtáranként egy vállalati emblémát konfigurálhat, és mind, hello bejelentkezési és hozzáférési Panel oldalakon használják. Bizonyos testreszabható elemek adott csak toohello bejelentkezési oldalára. hello alábbi táblázat részleteket hello különböző testreszabható elemek.

| Név | Leírás | Korlátozások | Javaslatok |
| --- | --- | --- | --- |
| Szalagcímembléma |hello szalagcím emblémájának hello bejelentkezési oldal és hello hozzáférési panel jelenik meg. |<p>JPG vagy PNG</p><p>60x280 képpont</p><p>10 kB</p> |<p>Használja szervezete teljes emblémáját (a piktogramot és az emblémát is beleértve).</p><p>Legyen 30 képpont magas tooavoid mobileszközökön görgetősávok bemutatása</p><p>A mérete maradjon 4 kB alatt.</p><p>Átlátszó PNG Formátumot használjon (nem érdemes feltételezni, hogy hello bejelentkezési oldal mindenhol fehér)</p> |
| Csempeembléma |(jelenleg nem szerepel hello bejelentkezési oldal) Hello jövőbeli Ez a szöveg használt tooreplace hello általános "munkahelyi vagy iskolai fiók" piktogramot hello élmény különböző helyeken lehet. |<p>JPG vagy PNG</p><p>120x120 képpont</p><p>10 kB</p> |<p>Legyen egyszerű (kisméretű szöveg nélkül), a lemezkép lehetnek átméretezett too50 % |
| </p> | | | |
| A felhasználónév címkéje a bejelentkezési oldalon |(jelenleg nem szerepel hello bejelentkezési oldal) Hello jövőbeli Ez a szöveg használt tooreplace hello hello élmény különböző helyeken az általános "munkahelyi vagy iskolai fiók" karakterlánc lehet. Olyasmire állíthatja be, például "Contoso-fiók" vagy "Contoso-azonosító" toosomething |<p>Unicode szöveg, amelyet fel too50 karakter</p><p>Csak egyszerű szöveg (hivatkozások és HTML-címkék nélkül)</p> |<p>Legyen rövid és egyszerű.</p><p>Kérje meg a felhasználók hogyan általában vonatkoznak toohello munkahelyi vagy iskolai fiók nekik biztosított.</p> |
| A bejelentkezési oldal szövege |A "bolierplate" szöveg hello bejelentkezési űrlaptól alatt jelenik meg, és a használt toocommunicate további utasításokat, vagy ha tooget Súgó és támogatás is. |<p>Unicode szöveg, amelyet fel too256 karakter</p><p>Csak egyszerű szöveg (hivatkozások és HTML-címkék nélkül)</p> |Legyen 250 karakternél kevesebb (nagyjából 3 soros szöveg). |
| A bejelentkezési oldal ábrája |hello ábra hello bejelentkezési oldal toohello hello bejelentkezési űrlaptól balra látható nagy képet. |<p>JPG vagy PNG</p><p>1420x1200</p><p>500 kB</p> |<p>1420x1200 képpont.</p><p>Fontos: a mérete legyen a lehető legkisebb, ideálisan 200 kB alatti. Ha a kép túl nagy, hello hello bejelentkezési lap van a teljesítményre amikor hello kép nincs gyorsítótárazva.</p><p>Ez a rendszerkép rendszer gyakran levágja, a különböző képarányoknak tooaccommodate. Bal felső sarokban (jobbra író nyelvek esetén), mert átméretezése keskenyebb formára hello alsó résztől/jobb saroktól felé hello felső / bal, mivel hello böngészőablakban zsugorítja hello felső hello elsődleges látványelemeket ne.</p> |
| A bejelentkezési oldal háttérszíne |hello bejelentkezési oldal háttérszíne hello terület toohello hello bejelentkezési űrlaptól balra használatban van. |Hexadecimális formátumú RGB-színnek kell lennie (például: #FFFFFF) |<p>hello háttérszínét is látható, helyett hello nagy méretű ábrát alacsony sávszélességű kapcsolatok használata esetén</p><p>Javasoljuk, hogy hello hello szalagcím emblémájának elsődleges színét válassza háttérszínnek.</p> |

## <a name="next-steps"></a>Következő lépések
* [Bevezetés a Prémium szintű Azure Active Directory használatába](active-directory-get-started-premium.md)
* [A hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png
