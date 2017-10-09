---
title: "olyan objektum, amely nem szinkronizál tooAzure AD aaaTroubleshoot |} Microsoft Docs"
description: "Végezzen hibaelhárítást, ezért az objektum nem szinkronizál tooAzure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 81e0a0793a1d5ec76cfcaec6e974726d7854f58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-tooazure-ad"></a>Olyan objektum, amely nem szinkronizál tooAzure AD hibaelhárítása

Ha egy objektum nem szinkronizál a várt tooAzure AD, mint, akkor több oka lehet. Ha hiba e-mailben kapott az Azure AD vagy az Azure AD Connect Health hello hibát látja, majd beolvassa [exportálási hibák elhárítása](active-directory-aadconnect-troubleshoot-sync-errors.md) helyette. De ha a probléma, ahol hello objektum nincs az Azure AD hibaelhárítási, majd a témakör, hogy a felhasználó. Leírja, hogyan toofind hibák hello helyszíni összetevő az Azure AD Connect szinkronizálása.

toofind hello hibák, helyen néhány sorrend hello toolook fog:

1. Hello [műveletnaplók](#operations) importálása és szinkronizálás során hello szinkronizálási motor által azonosított hibák kereséséhez.
2. Hello [kapcsolódási térbe](#connector-space-object-properties) hiányzó objektumokhoz és szinkronizálási hibák kereséséhez.
3. Hello [metaverse](#metaverse-object-properties) problémák kapcsolódó adatok kereséséhez.

Start [Synchronization Service Managert](active-directory-aadconnectsync-service-manager-ui.md) lépések megkezdése előtt.

## <a name="operations"></a>Műveletek
a Synchronization Service Managert hello hello műveletek lapján, ahol el kell kezdenie a hibaelhárítást. hello műveletek lapon hello legutóbbi műveletek hello eredményei láthatók.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/operations.png)  

hello felső félig minden futtatásakor krónikus sorrendben jeleníti meg. Alapértelmezés szerint hello műveleti napló tartja hello információ elmúlt hét napban, de ez a beállítás használatával módosítható hello [Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md). Bármely futtatása, amelyek nem szerepelnek egy sikeres állapotnak toolook használni szeretne. Hello rendezés hello fejlécek kattintva módosíthatja.

Hello **állapot** oszlop hello legfontosabb adatokat, és látható hello futtató legsúlyosabb károkat okozó problémát. Hello leggyakoribb állapotokról tooinvestigate prioritási sorrendben gyors összegzése (ahol * több lehetséges hiba karakterláncok jelzi).

| status | Megjegyzés |
| --- | --- |
| leállított-* |hello futtatása nem sikerült befejezni. Például ha hello távoli rendszer le, és nem érhető el. |
| leállt hiba-korlát |Több mint 5000 hibák vannak. Futtatás hello automatikusan toohello hibák nagy száma miatt le lett állítva. |
| Befejezett -\*-hibák |hello futtatása befejeződött, de hibák (kevesebb mint 5000) meg kell vizsgálni. |
| Befejezett -\*-figyelmeztetések |hello futtatása befejeződött, de bizonyos adatok nem várt hello állapotban van. Ha hibákat, majd ezt az üzenetet az általában csak az egyik oka. Mindaddig, amíg meg kell oldani hibák, figyelmeztetések nem ki kell vizsgálni. |
| sikeres |Nincs probléma. |

Amikor kiválaszt egy sorra, akkor hello alsó tooshow hello részleteit futtató frissíti. toohello sokkal bal hello alsó részén, előfordulhat, hogy egy lista bármelyiket **lépés #**. Ez a lista csak akkor jelenik meg, ha több tartomány az erdőben, ahol minden egyes tartományhoz egy lépés képviseli. hello címszó alatt található hello tartománynév **partíció**. A **szinkronizálási statisztika**, található további információ a feldolgozott változtatások hello száma. Kattinthat hello hivatkozások tooget hello megváltozott objektumok listája. Ha hibákkal objektummal rendelkezik, azokat a hibákat az Eseménynaplón **szinkronizálási hibák**.

### <a name="troubleshoot-errors-in-operations-tab"></a>A műveletek lapon hibák elhárítása
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorsync.png)  
Ha a hibák, mindkét hello objektum hiba és maga hello hiba hivatkozásokat tartalmaz, amelyek további információval.

Hello hibakarakterlánc kattintva Start (**szinkronizálási szabály-hiba – függvény-indított** hello kép). Először lehetősége lesz a hello objektum áttekintését. toosee hello tényleges hiba hello gombra **Veremkivonat**. A nyomkövetési hello hiba hibakeresési szintű információkat nyújt.

Kattintson a jobb egérgombbal a hello **hívásverem-információk** válassza **válassza ki az összes**, és **másolási**. Ezután másolja hello verem, és tekintse meg a kedvenc szerkesztő, például a Jegyzettömbben hello hiba.

* Ha hello hiba érkezett **SyncRulesEngine**, majd hello hívásverem-információk először rendelkezik az összes attribútumok listája hello objektumon. Görgessen lefelé, amíg megjelenik a hello címsor **belső kivétel leírásában olvasható = >**.  
  ![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorinnerexception.png)  
  hello sor látható hello hiba után. A fenti hello kép hello hiba létrehozott egyéni szinkronizálási szabály Fabrikam származik.

Hello hiba maga nem ad elegendő információ áll rendelkezésre, majd esetén idő toolook: hello adatokat mozgatná. Kattintson a hivatkozás hello hello objektumazonosító, és folytassa a hello [összekötő tér importált objektum](#cs-import).

## <a name="connector-space-object-properties"></a>Összekötőtér-objektum tulajdonságai
Ha Ön nem rendelkezik a hello észlelt hibák [műveletek](#operations) lapon, akkor hello tovább toofollow hello összekötő terület objektumot az Active Directoryból, toohello metaverse és tooAzure AD. Az ehhez az elérési úthoz keresse meg hello probléma esetén.

### <a name="search-for-an-object-in-hello-cs"></a>Keresse meg az objektumok hello CS

A **Synchronization Service Managert**, kattintson a **összekötők**, válassza ki az Active Directory-összekötőt, hello és **Összekötőtér keresési**.

A **hatókör**, jelölje be **RDN** (Ha azt szeretné toosearch hello CN attribútum) vagy **megkülönböztető név vagy a horgony** (Ha azt szeretné toosearch hello distinguishedName attribútum). Adjon meg egy értéket, és kattintson a **keresési**.  
![Összekötőtér keresése](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearch.png)  

Ha nem találja a hello objektum keres, majd, előfordulhat, hogy a rendszer kiszűrt rendelkező [tartományalapú szűrés](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) vagy [OU-alapú szűrés](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). Olvasási hello [szűrésének beállítása](active-directory-aadconnectsync-configure-filtering.md) témakör tooverify, amely a szűrés hello elvártak szerint van konfigurálva.

Egy másik keresési tooselect hello Azure AD-összekötőt, **hatókör** válassza ki **függőben lévő importálás**, és jelölje be hello **Hozzáadás** jelölőnégyzetet. Ez a keresés lehetővé teszi minden szinkronizált objektumnak az Azure ad-ben, amely nem rendelhető hozzá a helyi objektum.  
![Összekötő terület keresési árva](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearchorphan.png)  
Azokat az objektumokat egy másik szinkronizálási motor vagy a szinkronizálási motor a különböző szűrési konfigurációval rendelkezik hozott létre. Ez a nézet olvashat egy listát **árva** már nem kezelt objektumok. Át kell tekintenie a ezen a listán, és fontolja meg ezek az objektumok hello eltávolítását [Azure AD PowerShell](http://aka.ms/aadposh) parancsmagok.

### <a name="cs-import"></a>CS importálása
A tanúsítványszolgáltatások objektum megnyitásakor nincsenek több lapot hello tetején. Hello **importálása** lapon láthatók az importálás után előkészített hello adatok.  
![CS objektum](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/csobject.png)    
Hello **régi érték** jeleníti meg, mi jelenleg tárolt Connect és hello **új érték** mi hello forrásrendszerben érkezett, és nem alkalmazták. Ha nem sikerül hello objektumra, majd változtatások nincsenek feldolgozva.

**Hiba történt**  
![CS objektum](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssyncerror.png)  
Hello **szinkronizálási hiba** lap csak akkor látható, ha a probléma hello objektummal. További információkért lásd: [szinkronizálási hibák elhárítása](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS Leszármaztatás
hello Leszármaztatás lapon látható, hogyan hello összekötő terület objektum kapcsolódó toohello metaverzum-objektum. Importálásakor hello összekötő utolsó eltérő hello csatlakoztatott rendszer, és az alkalmazott szabályok toopopulate adatok hello metaverse tekintheti meg.  
![CS Leszármaztatás](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineage.png)  
A hello **művelet** oszlopban láthatja van egy **bejövő** szinkronizálási szabály hello művelettel **rendelkezés**. Azt jelzi, hogy, hogy mindaddig, amíg az összekötő terület objektum jelen, hello metaverzum-objektum továbbra is. Ha hello szinkronizálási szabályok helyette jeleníti meg egy irányban szinkronizálási szabály **kimenő** és **rendelkezés**, azt jelzi, hogy ez az objektum törölve, hello metaverzum-objektum törlésekor.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineageout.png)  
Azt is láthatja hello **PasswordSync** oszlopot, amely a bejövő kapcsolódási térbe hello járulhat módosítások toohello jelszó, mivel hello értéke egy szinkronizálási szabály **igaz**. Ezt a jelszót AD tooAzure hello kimenő forgalomra vonatkozó szabály keresztül elküldi.

Hello Leszármaztatás lapon kaphatunk toohello metaverse kattintva [Metaverzum-objektum tulajdonságai](#mv-attributes).

Az összes fül hello alján van két gomb: **előzetes** és **napló**.

### <a name="preview"></a>Előzetes verzió
hello villámnézeti lap használt toosynchronize egy adott objektum. Ez akkor hasznos, ha néhány egyéni szinkronizálási szabály kapcsolatos hibaelhárítást végez, és szeretné, hogy egyetlen objektum változás toosee hello hatása. Választhat **Full sync** és **különbözeti szinkronizálási**. Is választhat **készítése Preview**, amely csak végleg hello módosítása a memória, és **véglegesítése Preview**, amely hello metaverse frissítése, és minden szakaszból vált tootarget összekötőterek.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/preview.png)  
Hello objektum, és melyik szabály alkalmazása az adott Attribútumfolyam vizsgálhatja meg.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/previewresult.png)

### <a name="log"></a>Napló
hello oldalon használt toosee hello jelszó-szinkronizálás állapota és előzményei. További információkért lásd: [jelszó-szinkronizálás hibaelhárítása](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="metaverse-object-properties"></a>Metaverzum-objektum tulajdonságai
Keresés Active Directory hello forrásból általában jobb toostart [kapcsolódási térbe](#connector-space). De a keresést a hello metaverse is elindíthatja.

### <a name="search-for-an-object-in-hello-mv"></a>Keresse meg az objektumok hello MV
A **Synchronization Service Managert**, kattintson a **Metaverzum-keresés**. Hozzon létre egy talál hello felhasználói tudja lekérdezést. Általános attribútumokkal rendelkeznek, például a fióknév (sAMAccountName) és a userPrincipalName is kereshet. További információkért lásd: [Metaverzum-keresés](active-directory-aadconnectsync-service-manager-ui-mvsearch.md).
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvsearch.png)  

A hello **keresési eredmények** ablakában kattintson hello objektumra.

Ha nem talált hello objektumot, majd nem még elérte hello metaverse. Továbbra is toosearch hello objektum az Active Directory hello [kapcsolódási térbe](#connector-space-object-properties). Előfordulhat, hogy a szinkronizálást, amely blokkolja a következő toohello metaverzum-objektum hello hiba, vagy előfordulhat, hogy egy szűrőt alkalmazza.

### <a name="mv-attributes"></a>MV-attribútumok
Hello attribútumok lapon hello érték látható, és melyik összekötő hozzájárult azt.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvobject.png)  

Ha egy objektum nem szinkronizál, majd tekintse meg a következő attribútumok hello metaverse hello:
- Hello attribútum **cloudFiltered** jelent-e, és állítsa be a túl**igaz**? Ha van, akkor a rendszer kiszűrt toohello lépéseit szerint [attribútum alapú szűrés](active-directory-aadconnectsync-configure-filtering.md#attribute-based-filtering).
- Hello attribútum **sourceAnchor** jelen? Ha nem rendelkezik egy fiók-erőforrás szolgának? Ha egy objektum hivatkozott postafiókkal azonosítottak (hello attribútum **msExchRecipientTypeDetails** hello értéke 2), majd hello sourceAnchor hello erdő engedélyezve van az Active Directory-fiókkal van járulnak hozzá. Győződjön meg arról, hello fő fiók importálva, és megfelelően szinkronizálva. hello fő fióknak szerepelnie kell a hello [összekötők](#mv-connectors) hello objektum.

### <a name="mv-connectors"></a>MV-összekötők
hello összekötők lapon láthatók, amelyek rendelkeznek egy ábrázolása hello objektum összes összekötőterek.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvconnectors.png)  
Az összekötő kell rendelkezniük:

- Minden Active Directory-erdő hello felhasználó jelennek meg. Ez a megjelenítési módja foreignSecurityPrincipals és ügyfél objektumok lehetnek.
- Az Azure AD-összekötő.

Ha hello összekötő tooAzure AD hiányzik, majd olvassa el [MV attribútumok](#MV-attributes) tooverify hello feltételeket ahhoz, hogy meg kiépített tooAzure AD.

Ezen a lapon is lehetővé teszi toonavigate toohello [összekötő terület objektum](#connector-space-object-properties). Jelöljön ki egy sort, és kattintson a **tulajdonságok**.

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
