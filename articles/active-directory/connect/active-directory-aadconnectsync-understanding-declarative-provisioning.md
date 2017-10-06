---
title: "Az Azure AD Connect: Understanding deklaratív kiépítés |} Microsoft Docs"
description: "Hello deklaratív üzembe helyezési modell az Azure AD Connectben ismerteti."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f11e078f0aafacf94d69f0726ae41629a8470336
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect szinkronizálása: Understanding deklaratív kiépítés
Ez a témakör ismerteti az Azure AD Connectben hello konfigurációs modell. hello modell deklaratív kiépítés nevezik, és a konfiguráció változása kihívásokra toomake lehetővé teszi. A jelen témakörben ismertetett számos elemet speciális, és a legtöbb ügyfél forgatókönyvhöz nem szükséges.

## <a name="overview"></a>Áttekintés
Deklaratív kiépítés csatlakoztatott forráskönyvtárat érkező objektumok feldolgozása, és határozza meg, hogyan hello objektumok és attribútumok kell kell alakítani egy forrás tooa céltól. Az objektum feldolgozása a szinkronizálási folyamat, és hello csővezeték van hello ugyanazt a bejövő és kimenő szabályok. Egy bejövő forgalomra vonatkozó szabály egy összekötő terület toohello metaverse származik, és egy kimenő forgalomra vonatkozó szabály hello metaverse tooa kapcsolódási térbe származik.

![Szinkronizálási folyamat](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

hello csővezeték van több különböző modult. Minden egyes felelős szinkronizációs egy fogalom.

![Szinkronizálási folyamat](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

* Forrás, hello adatforrás-objektum
* [Hatókör](#scope), megkeresi az összes szinkronizálási szabály hatókörébe
* [Csatlakozás](#join), határozza meg a kapcsolódási térbe és metaverzum közötti kapcsolat
* [Átalakítás](#transform), számítja ki, hogyan kell alakítani attribútumok és folyamata
* [Sorrend](#precedence), oldja fel a rendszer ütköző attribútum hozzájárulások
* Cél, hello célobjektum

## <a name="scope"></a>Hatókör
hello hatókör modul objektum értékeli, és határozza meg a hatókörön, és fel kell venni hello feldolgozási hello szabályokat. Attól függően, hogy hello attribútumok értékek hello objektumon más szinkronizálási szabályok kiértékelt toobe hatókörében. A letiltott felhasználó nem Exchange postaládával például különböző szabályok, mint az engedélyezett felhasználók egy postaládával rendelkezik.  
![Hatókör](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

hello hatókört csoportok és a kikötéseket típusúként van definiálva. hello záradékok egy csoporton belül vannak. A logikai és a csoport összes záradékok közötti szolgál. Például (részleg informatikai és ország = = Dánia). Logikai vagy csoportok közötti szolgál.

![Hatókör](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
hello hatókör képen értelmezhető (részleg informatikai és ország = = Dánia) vagy (ország = Svédország). Ha 1 vagy csoport 2 kiértékelt tootrue, hello szabály hatóköre van.

hello hatókör modul támogatja a következő műveletek hello.

| Művelet | Leírás |
| --- | --- |
| EGYENLŐ, NOTEQUAL |Egy karakterlánc-összehasonlítási, amely kiértékeli, ha az érték egyenlő toohello hello attribútum értékének. Többértékű attribútumok ISIN és ISNOTIN témakörben talál. |
| LESSTHAN, LESSTHAN_OR_EQUAL |Egy karakterlánc-összehasonlítási, amely kiértékeli, ha az érték kevesebb hello attribútum értékének hello. |
| TARTALMAZZA, NOTCONTAINS |Egy karakterlánc-összehasonlítási, amely kiértékeli, ha az érték megtalálható valahol belüli értéket hello attribútum. |
| MEGADOTT MÓDON KEZDŐDŐ, NOTSTARTSWITH |Egy karakterlánc hasonlítsa össze, amely kiértékeli, ha érték hello hello hello attribútum értékének elejére. |
| MEGADOTT MÓDON VÉGZŐDŐ, NOTENDSWITH |Egy karakterlánc-összehasonlítási, amely kiértékeli a hello végén hello attribútum értékének hello érték esetén. |
| GREATERTHAN, GREATERTHAN_OR_EQUAL |Egy karakterlánc hasonlítsa össze, amely kiértékeli, ha az érték nagyobb, mint a hello attribútum értékének hello. |
| ISNULL, ISNOTNULL |Kiértékeli, ha hello attribútum hiányzik hello objektumból. Ha hello attribútum nincs jelen, és ezért null értékű, hello szabály hatóköre van. |
| ISIN, ISNOTIN |Ha hello érték megtalálható-e definiálva hello attribútum kiértékeli. A művelet nem egyenlő és NOTEQUAL többértékű változata hello. hello attribútum kellene toobe többértékű attribútum, valamint ha hello érték hello attribútumértékek valamelyikében található, majd hello szabály hatókörében. |
| ISBITSET, ISNOTBITSET |Értékeli ki, ha egy adott bit be van állítva. Például lehet használt tooevaluate userAccountControl toosee hello bit, ha a felhasználó engedélyezve van vagy le van tiltva. |
| ISMEMBEROF, ISNOTMEMBEROF |hello érték hello kapcsolódási térbe DN tooa csoporthoz kell tartalmaznia. Ha hello objektum megadott hello csoport tagja, hello szabály hatókörében van. |

## <a name="join"></a>Csatlakozás
hello illesztési modul hello szinkronizálási folyamat keresése hello objektum hello forrásból és objektum hello kapcsolatát hello cél felelős. Egy bejövő forgalomra vonatkozó szabály, a ezt a kapcsolatot a kapcsolódási térbe hello metaverse tooan kapcsolatobjektum keresése objektum lehet.  
![Csatlakozás cs és mv között](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
hello cél toosee, ha már létezik egy objektum a hello metaverse, egy másik-összekötő által létrehozott, azt kell társítani. Például egy fiók-erőforrást az erdő hello felhasználót hello fiókerdő keresztül illeszthető hello felhasználói hello erőforráserdőből.

Illesztések többnyire bejövő szabályok toojoin összekötő terület objektumok együtt toohello használt azonos metaverzum-objektum.

hello csatlakozik egy vagy több csoportot is meg van adva. Egy csoporton belüli záradékok rendelkezik. A logikai és a csoport összes záradékok közötti szolgál. Logikai vagy csoportok közötti szolgál. hello csoportok felső toobottom sorrendben feldolgozása. Egy csoport hello cél talált található objektum pontosan egy megfelelő, ha nincs más illesztési szabály kell kiértékelni. Ha nulla vagy egynél több objektum található, a feldolgozási toohello következő szabálycsoport továbbra is. Emiatt hello szabályok létre kell hozni a explicit hello sorrendjének első és több intelligens hello végén.  
![Csatlakozás meghatározása](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
a felső toobottom hello illesztések képen dolgoznak fel. Első hello szinkronizálási folyamat látja-e az employeeID egyezés. Ha nem, a második szabály hello látja, hogy hello fióknév használt toojoin hello objektumok együtt. Amely nem egyező vagy, hello harmadik és végső szabály akkor több egyeztetésű felhasználó hello nevének megadásával.

Illesztés az összes szabály értékelése, és nem pontosan egy egyezés, ha hello **kapcsolattípus** a hello **leírás** lap szolgál. Ha a beállítás értéke túl**rendelkezés**, majd egy új objektum hello cél jön létre.  
![Kiépítés, vagy csatlakozzon](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Objektum csak kell rendelkeznie az illesztés szabállyal egy egyetlen szinkronizálási szabály hatókörében. Ha több szinkronizálási szabály ahol illesztés van definiálva, hiba történik. Sorrendje nincs használt tooresolve illesztési ütközések. Egy objektum rendelkeznie kell egy illesztési szabály hello rendelkező attribútumok tooflow hatókörébe ugyanazt a bejövő/kimenő irányban. Ha tooflow kell attribútumok bejövő és kimenő toohello ugyanaz az objektum, rendelkeznie kell egy bejövő és egy illesztési kimenő szinkronizálási szabály.

Kimenő illesztési van egy különleges viselkedését, ha egy objektum tooa cél kapcsolódási térbe tooprovision megkísérli. hello megkülönböztető név attribútum használt toofirst próbálkozzon a névkeresési csatlakozzon. Ha már létezik egy objektum a hello cél kapcsolódási térbe a hello azonos DN, hello objektumok kapcsolódnak-e.

hello illesztési modul csak akkor értékeli ki, amikor egy új szinkronizálási szabály ismét elérhető, a hatókör. Ha csatlakozott egy objektumot, a rendszer nem disjoining akkor is, ha hello illesztési feltétel nem teljesül. Ha azt szeretné, hogy az objektum toodisjoin, be kell lépnie hello szinkronizálási szabály, amely hello objektumok csatlakoztatott hatókörén kívül.

### <a name="metaverse-delete"></a>Metaverse törlése
A metaverzum-objektum marad, amíg nincs egy szinkronizálási szabály hatóköre, **kapcsolattípus** túl beállítása**rendelkezés** vagy **StickyJoin**. Egy StickyJoin használatos, ha egy összekötő nem tud új tooprovision toohello metaverse object, de csatlakozott, amikor kell törölni a forrásban: hello hello metaverzum-objektum törlése előtt.

A törölt metaverzum-objektum egy kimenő szinkronizálási szabályt társított összes objektumot megjelölve **rendelkezés** törlési vannak megjelölve.

## <a name="transformations"></a>Átalakítás
hello átalakítások hogyan attribútumok kell hello forrás toohello cél haladjanak használt toodefine. hello adatfolyamok rendelkezhet hello következő **típusok flow**: közvetlen, állandó vagy kifejezés. A közvetlen folyamat zajlik egy attribútum értékét,-átalakítás nélküli további van. Egy konstans érték beállítása hello megadott értéket. Egy kifejezés hello deklaratív létesítési kifejezés nyelvi tooexpress hogyan hello átalakítása kell használja. Részletek hello a hello kifejezés nyelvi hello találhatók [deklaratív létesítési kifejezés nyelvi ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) témakör.

![Kiépítés, vagy csatlakozzon](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Hello **egyszer alkalmazása** jelölőnégyzet határozza meg, hogy hello attribútum csak kell beállítani, hello objektum létrehozásakor. Ez a konfiguráció például használt tooset egy kezdeti jelszó egy új felhasználói objektum lehet.

### <a name="merging-attribute-values"></a>Az egyesítés attribútumértékek
Az attribútumfolyamok hello van egy beállítás toodetermine Ha többértékű attribútumok össze kell vonni az számos különböző összekötők. hello alapértelmezett értéke **frissítés**, ami azt jelenti, hogy hello szinkronizálási szabály, legmagasabb prioritású win kell.

![Egyesítési típusok](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Is **egyesítése** és **MergeCaseInsensitive**. Ezek a beállítások lehetővé teszik különböző forrásokból származó toomerge értékeket. Például lehet használt toomerge hello tag vagy a proxyAddresses számos különböző erdőkből származó attribútum. Ezt a beállítást használja, ha az összes szinkronizálni szabályok hatókörében, objektum hello kell használniuk azonos Egyesítési típus. Nem adhat meg **frissítés** a egy összekötőt és **egyesítése** másik. Ha megpróbál, hibaüzenet.

hello közötti különbség **egyesítése** és **MergeCaseInsensitive** hogyan tooprocess ismétlődő attribútumértékekkel van. hello szinkronizálási motor biztosítja, hogy az ismétlődő értékek nem kerülnek hello target attribútuma. A **MergeCaseInsensitive**, ismétlődő értékek csak különbség, abban az esetben nem fog toobe található. Például nem kell megjelennie mindkét "SMTP:bob@contoso.com"és"smtp:bob@contoso.com" hello target attribútum. **Egyesítési** csak megnézi hello pontos és több értékek esetén csak a különbségek eset jelen lehet.

beállítás hello **cserélje le** azonos hello **frissítés**, de semmi nem használja.

### <a name="control-hello-attribute-flow-process"></a>Vezérlő hello attribútum áramlási folyamat
Ha több bejövő szinkronizálási szabály konfigurált toocontribute toohello azonos metaverzum-attribútum, majd érvénybe használt toodetermine hello győztes. hello szinkronizálási szabály legmagasabb prioritású (legkisebb numerikus értéket) toocontribute hello érték lesz. hello azonos kimenő szabályok történik. hello szinkronizálási szabály legmagasabb prioritású wins, és hozzájárul hello érték toohello csatlakoztatott címtárhoz.

Bizonyos esetekben helyett közre értéket hello szinkronizálási szabályt kell meghatározni, más szabályok működése kell. Nincsenek ebben az esetben használt néhány speciális literálok.

A bejövő szinkronizálási szabályok hello literális **NULL** lehet, hogy hello folyamata rendelkezik-e a nem érték toocontribute használt tooindicate. Egy másik szabály alacsonyabb fontossági sorrendű értéket vehet részt. Ha nincs szabály értéket hozzájárult, hello metaverzum-attribútum eltávolítása. Az egy kimenő forgalomra vonatkozó szabály Ha **NULL** érték hello végső szinkronizálási szabályok feldolgozása után, majd a rendszer eltávolítja hello értéket hello csatlakoztatott könyvtárban.

literális hello **AuthoritativeNull** túl hasonlít**NULL** , de hello különbség az, hogy nincs alsó végrehajtására vonatkozó elsőbbségi szabályok értéket vehet részt.

Az Attribútumfolyam is használható **IgnoreThisFlow**. Azt jelzi semmilyen hello értelemben hasonló tooNULL toocontribute. hello különbség, hogy nem távolítja el egy már meglévő érték hello célkiszolgálón. Van például hello Attribútumfolyam soha nem létezik.

Például:

A *tooAD - felhasználó Exchange hibrid kimenő* folyamata a következő hello található:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Ebben a kifejezésben értelmezhető: Ha hello felhasználói postaláda az Azure ad-ben található, majd flow az Azure AD tooAD hello attribútumot. Ha nem, nem flow semmi hátsó tooActive könyvtár. Ebben az esetben azt megakadályozná hello meglévő értéket az ad-ben.

### <a name="importedvalue"></a>ImportedValue
hello függvény ImportedValue eltér a más funkciók, mivel hello attribútum neve szögletes zárójelbe helyett idézőjelek között kell tenni:  
`ImportedValue("proxyAddresses")`.

A szinkronizálás során általában attribútum hello várt érték, használja, akkor is, ha az még nem lett még exportálása, vagy hiba történt ("tetején hello torony") exportálása során. Egy bejövő szinkronizálási azt feltételezi, hogy olyan attribútum, amely még nem ért egy csatlakoztatott címtárhoz idővel eléri azt. Néhány esetben fontos tooonly szinkronizálja egy érték, amely megerősítette hello csatlakoztatott címtárhoz ("hologram és a különbözeti importálás torony").

Ez a függvény egy példa található hello out-of-box szinkronizálási szabály *található az Active Directoryból – az Exchange-ből közös felhasználói*. A hibrid Exchange-ben hozzáadott Exchange online hello érték csak szinkronizálnia kell, amikor hello érték sikeresen exportált visszaigazolását:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Sorrend
Ha több szinkronizálási szabályok próbálja toocontribute hello attribútum értéke toohello egyazon célobjektum, hello sorrend érték használt toodetermine hello győztes. hello szabály legmagasabb prioritású legkisebb numerikus értéket, toocontribute hello attribútum ütközést lesz.

![Egyesítési típusok](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Ebben a rendezése használt toodefine pontosabb lehet attribútum-adatfolyamok objektumok egy kis részhalmaza. Például a hello out-a-mezőben-szabályok győződjön meg arról, amelyek egy engedélyezett fiók attribútumok (**felhasználói AccountEnabled**) prioritást élveznek a más fiókokhoz.

Sorrend összekötők között definiálhatók. Amely lehetővé teszi, hogy összekötők jobb adatok toocontribute értékekkel először.

### <a name="multiple-objects-from-hello-same-connector-space"></a>A több objektum hello ugyanazt a kapcsolódási térbe
Ha több objektummal rendelkezik a hello azonos összekötő tér illesztett toohello azonos metaverzum-objektum érvénybe kell beállítani. Ha több objektum hatókörének hello az azonos szinkronizálása szabály, majd hello szinkronizálási motor nem tudja toodetermine elsőbbséget. Ez nem egyértelmű melyik adatforrás-objektum hozzá kell járulnia hello érték toohello metaverse. Ez a konfiguráció jelentése szerint nem egyértelmű, még akkor is, ha a forrásban: hello hello attribútumok rendelkezik hello ugyanazt az értéket.  
![Több objektum csatlakoztatott toohello azonos mv-objektum](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

Ebben az esetben szüksége hello szinkronizálási szabályok toochange hello hatókörének így hello adatforrás-objektumok különböző szinkronizálási szabályok hatókörében. Amely lehetővé teszi különböző toodefine elsőbbséget.  
![Több objektum csatlakoztatott toohello azonos mv-objektum](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Következő lépések
* További információk a hello kifejezés nyelvi [ismertetése deklaratív kiépítés kifejezések](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Lásd: hogyan deklaratív használt out-of-box a [ismertetése hello alapértelmezett konfiguráció](active-directory-aadconnectsync-understanding-default-configuration.md).
* Tekintse meg, hogyan toomake egy gyakorlati módosítani, használja a deklaratív kiépítés [hogyan toomake egy módosítás toohello alapértelmezett konfiguráció](active-directory-aadconnectsync-change-the-configuration.md).
* Továbbra is a felhasználók és névjegyek működése együtt az tooread [felhasználók és névjegyek](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

**Referencia-témakörei**

* [Azure AD Connect szinkronizálása: funkciók referencia](active-directory-aadconnectsync-functions-reference.md)
