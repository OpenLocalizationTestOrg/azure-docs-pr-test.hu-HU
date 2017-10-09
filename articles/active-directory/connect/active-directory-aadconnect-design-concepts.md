---
title: "Az Azure AD Connect: Tervezési alapelvek |} Microsoft Docs"
description: "Ez a témakör részletesen bizonyos megvalósítási terv területek"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1e5d5c6a716ca653fb14fc059e8155124b433732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-design-concepts"></a>Az Azure AD Connect: Tervezési alapelvek
hello Ez a témakör célja, amely során az Azure AD Connect hello megvalósítási terv kell re toodescribe területeket. Ez a témakör egy részletes bemutatója a bizonyos területeken, és ezekről a fogalmakról rövid leírását, valamint további témakörökre mutatnak.

## <a name="sourceanchor"></a>sourceAnchor
hello sourceAnchor attribútum *objektum hello élettartama alatt megváltoztathatatlan attribútum*. Egyedileg azonosít egy objektumot, hogy hello azonos objektumot a helyszíni és az Azure ad-ben. hello attribútum néven is ismert **immutableId** és hello két neveket cserélhető használnak.

hello word nem módosítható, amely "nem módosítható", fontos toothis témakör. Mivel ez az attribútum értéke nem lehet módosítani, beállítása után, célszerű fontos toopick, amely támogatja az adott esetben.

a következő forgatókönyvek hello hello attribútum szolgál:

* Amikor egy új szinkronizálási motor kiszolgálót a beépített, vagy vész-helyreállítási forgatókönyv után úgy, ez az attribútum meglévő objektumokat a helyszíni objektumok Azure AD-ben hivatkozásokat tartalmaz.
* Ha egy csak felhőalapú identitás tooa szinkronizált identitás modellből helyezi át, akkor ez az attribútum lehetővé teszi az objektumoknak túl "rögzített match" meglévő objektumok az Azure AD helyszíni objektumok.
* Ha összevonási, akkor ez az attribútum a hello **userPrincipalName** hello szerepel toouniquely felhasználó azonosításának jogcímet.

Ez a témakör csak beszél sourceAnchor toousers vonatkozik. hello ugyanazok a szabályok alkalmazása tooall objektumtípusok, de csak a felhasználók számára a probléma általában a veszélye.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Egy jó sourceAnchor attribútum kiválasztása
hello értékének igazodnia kell a következő szabályok hello:

* Kisebb, mint 60 karakter hosszú lehet
  * Nem, a – z, A-Z karaktereket vagy 0-9 kódolású és számít 3 karakternél
* Egy speciális karaktert tartalmaz: &#92;! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
* Egyedinek kell lenniük
* Karakterlánc, egész, vagy bináris kell lennie
* Felhasználó nevét, a módosítások nem alapján
* Nem kell a kis-és nagybetűket és értékek, amelyek esetének eltérhetnek elkerülése érdekében
* Hozzá kell rendelni hello objektum létrehozásakor

Hello kijelölt sourceAnchor esetén nem karakterlánc típusú, és az Azure AD Connect Base64Encode hello attribútum értéke tooensure különleges karaktereket nem jelennek meg. Ha az AD FS mint egy másik összevonási kiszolgálót használ, győződjön meg arról is Base64Encode hello attribútumot a kiszolgáló listájában.

hello sourceAnchor attribútum kis-és nagybetűket. A "Kovacsjanos" érték nem hello ugyanaz, mint a "kovacsjanos". De nem kell két különböző objektum csak a kis különbség.

Ha egyetlen erdővel rendelkezik a helyszínen, majd hello attribútum használjon van **objectGUID**. Ez történik akkor is használható, ha a gyorsbeállítások használata az Azure AD Connectben, és a DirSync által használt attribútum is hello hello attribútum.

Ha több erdővel rendelkezik, és nem helyezhető át felhasználók közötti erdők és tartományok, majd **objectGUID** van egy megfelelő attribútumot toouse is ebben az esetben.

Ha a felhasználók az erdők és tartományok között helyezi át, majd meg kell attribútumot, amely nem változik, vagy hogy áthelyezhetők-hello felhasználók hello áthelyezés során. A javasolt megoldás, toointroduce szintetikus attribútum. Olyan attribútum, amely sikerült tartsa valami hasonló a GUID lenne megfelelő. Egy új GUID objektum létrehozása során létrehozott és hello felhasználói nyomtatva. Egy egyéni szinkronizálási szabályt hozhat létre hello szinkronizálási motor server toocreate ezt az értéket a hello alapján **objectGUID** és frissítés hello kijelölt attribútum hozzáadása. Amikor hello objektum helyezi át, győződjön meg arról, hogy tooalso másolási hello tartalma ezt az értéket.

Egy másik megoldás, toopick egyik létező attribútumához ismernie, nem változik. Gyakran használt attribútumokat tartalmaznak **employeeID**. Ha betűket tartalmazó attribútum vegye figyelembe, ellenőrizze, nincs alkalommal hello eset (a kis-és nagybetű) módosíthatja a hello attribútum értéke van. Hibás attribútumok nem használhatók a hello felhasználó hello nevű ezen attribútumait tartalmazza. Házasság vagy felbontására a hello értéke várt toochange, ami nem engedélyezett ehhez az attribútumhoz. Ez egyben az egyik oka miért attribútumok például **userPrincipalName**, **mail**, és **targetAddress** nincsenek akár tooselect hello Azure AD Connect telepítés varázsló. Ezek az attribútumok is tartalmazhat, hello "@" karakter, ami nem engedélyezett a hello sourceAnchor.

### <a name="changing-hello-sourceanchor-attribute"></a>Hello sourceAnchor attribútum módosítása
hello sourceAnchor attribútum értéke nem módosítható, miután hello objektum létrehozása az Azure ad-ben, és hello identitása szinkronizálva.

Emiatt hello a következő korlátozások vonatkoznak tooAzure AD Connect:

* hello sourceAnchor attribútum csak akkor állítható kezdeti telepítése során. Ha Újrafuttatja hello telepítési varázsló, ezt a beállítást csak olvasható. Ha toochange kell ezt a beállítást, majd kell távolítania, és újra kell telepítenie.
* Ha telepít egy másik Azure AD Connect-kiszolgáló, majd válassza ki kell hello korábban már használt azonos sourceAnchor attribútum. Ha korábban használt DirSync, és helyezze át a tooAzure AD csatlakozni, akkor kell használnia **objectGUID** mivel ez a DirSync által használt hello attribútum.
* Ha a sourceAnchor hello érték megváltozott után hello objektum exportált tooAzure AD, majd az Azure AD Connect szinkronizálási hibát jelez, és nem teszi lehetővé a további módosításokat az adott objektumhoz előtt hello a problémát, és hello sourceAnchor vissza a hello módosul a forráskönyvtár.

## <a name="using-msds-consistencyguid-as-sourceanchor"></a>Az msDS-ConsistencyGuid sourceAnchor használatával
Alapértelmezés szerint az Azure AD Connect (1.1.486.0 verziója és a régebbi) objectGUID használja, mint a hello sourceAnchor attribútum. ObjectGUID, a rendszer. Az érték létrehozása során a helyszíni Active Directory-objektumok nem adható meg. A szakaszban leírtak szerint [sourceAnchor](#sourceanchor), forgatókönyv, ahol toospecify hello sourceAnchor értéket kell. Megfelelő tooyou hello forgatókönyvek esetén hello sourceAnchor attribútumaként konfigurálható AD attribútum (például msDS-ConsistencyGuid) kell használnia.

Az Azure AD Connect (1.1.524.0 verziót, és utána) mostantól lehetővé teszi a sourceAnchor attribútum az msDS-ConsistencyGuid hello használni. Ez a funkció használata esetén az Azure AD Connect automatikusan konfigurálja hello szinkronizálási szabályokat:

1. Hello sourceAnchor attribútum esetén a felhasználói objektumok msDS-ConsistencyGuid használják. Más típusú objektumokat ObjectGUID használható.

2. Az összes megadott helyszíni AD-felhasználó objektum, amelynek msDS-ConsistencyGuid attribútum nincs feltöltve, az Azure AD Connect ír az objectGUID érték hátsó toohello msDS-ConsistencyGuid attribútum a helyszíni Active Directoryban. Miután elkészült hello msDS-ConsistencyGuid attribútum, az Azure AD Connect majd exportálja hello objektum tooAzure AD.

>[!NOTE]
> Egyszer egy helyszíni AD-objektum importálja az Azure AD Connect (amelyek, importálja a hello AD kapcsolódási térbe és hello Metaverse képezik le), a sourceAnchor értéke már nem módosítható. toospecify hello sourceAnchor értékét egy adott a helyszíni AD objektumazonosító, a msDS-ConsistencyGuid attribútum konfigurálása az Azure AD Connect az importálás előtt.

### <a name="permission-required"></a>Szükséges engedéllyel
Ez a szolgáltatás toowork a hello AD DS-ben használt fiók toosynchronize a helyszíni Active Directoryval kell biztosítani írási engedéllyel toohello msDS-ConsistencyGuid attribútum a helyszíni Active Directoryban.

### <a name="how-tooenable-hello-consistencyguid-feature---new-installation"></a>Hogyan tooenable hello ConsistencyGuid szolgáltatás - új telepítés
Új telepítése során engedélyezheti a sourceAnchor hello ConsistencyGuid használatát. Ez a fejezet Express és az egyéni telepítés részleteit.

  >[!NOTE]
  > Csak az Azure AD Connect újabb verzióit (1.1.524.0, és utána) támogatja a sourceAnchor ConsistencyGuid használatát hello új telepítése során.

### <a name="how-tooenable-hello-consistencyguid-feature"></a>Hogyan tooenable hello ConsistencyGuid szolgáltatás
Jelenleg hello szolgáltatás csak akkor engedélyezhető, csak új Azure AD Connect telepítése során.

#### <a name="express-installation"></a>Az Expressz telepítés
Az Expressz mód az Azure AD Connect telepítésekor hello Azure AD Connect varázsló automatikusan hello sourceAnchor attribútum használata a következő logikát hello, határozza meg leginkább megfelelő AD hello attribútum toouse:

* Először a hello Azure AD Connect varázsló lekérdezések használja az Azure AD bérlő tooretrieve hello AD attribútum hello előző az Azure AD Connect telepítés sourceAnchor attribútum hello (ha van ilyen). Ez az információ áll rendelkezésre, ha az Azure AD Connect használja ugyanazt az AD hello attribútum.

  >[!NOTE]
  > Csak az Azure AD Connect újabb verzióit (1.1.524.0, és utána) telepítésekor használt információkat tárol az Azure AD-bérlőben kapcsolatos hello sourceAnchor attribútum. Az Azure AD Connect korábbi verziói azonban nem.

* Hello sourceAnchor attribútum használt információ nem érhető el, ha a hello varázsló hello msDS-ConsistencyGuid attribútum a helyszíni Active Directoryban hello állapotát ellenőrzi. Ha hello attribútum nincs konfigurálva minden objektum hello könyvtárban, hello varázsló hello msDS-ConsistencyGuid hello sourceAnchor attribútum használ. Ha egy vagy több objektum hello könyvtárban hello attribútum van beállítva, hello varázsló arra a következtetésre jut hello attribútum más alkalmazások használják, és nem felel meg a sourceAnchor attribútum...

* Ebben az esetben hello varázsló visszavált toousing objectGUID hello sourceAnchor attribútumaként.

* Miután hello sourceAnchor attribútum születik, hello varázsló hello információkat tárol az Azure AD-bérlő. hello adatokat jövőbeni üzembe helyezése az Azure AD Connect használjuk.

Expressz telepítés befejezése után hello varázsló tájékoztatja, hogy mely attribútum válogatott hello Forráshorgony attribútumaként.

![Varázsló tájékoztatja a sourceAnchor kivételezett AD attribútum](./media/active-directory-aadconnect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Egyéni telepítés
Az Azure AD Connect telepítése egyéni módba, ha a hello Azure AD Connect varázsló két lehetőséget biztosít a sourceAnchor attribútum konfigurálásakor:

![Egyéni telepítés – a sourceAnchor konfiguráció](./media/active-directory-aadconnect-design-concepts/consistencyGuid-02.png)

| Beállítás | Leírás |
| --- | --- |
| Hogy az Azure a számomra hello forráshorgony kezelése | Válassza ezt a lehetőséget, ha meg szeretné az Azure AD toopick hello attribútum. Ha ezt a lehetőséget választja, az Azure AD Connect varázsló vonatkozik hello azonos [sourceAnchor attribútum lemezválasztási logika Expressz telepítés során használt](#express-installation). Hasonló tooExpress telepítési hello varázsló tájékoztatja, hogy mely attribútum válogatott, hello Forráshorgony attribútum egyéni telepítés befejezése után. |
| A specific attribute (Egy adott attribútum) | Válassza ezt a lehetőséget, ha egy meglévő AD attribútum hello sourceAnchor attribútumaként toospecify kívánja. |

### <a name="how-tooenable-hello-consistencyguid-feature---existing-deployment"></a>Hogyan tooenable hello ConsistencyGuid szolgáltatás - meglévő telepítéshez
Ha egy meglévő Azure AD Connect a központi telepítési objectGUID használja, mint hello Forráshorgony attribútum, megváltoztathatja azt toousing ConsistencyGuid helyette.

>[!NOTE]
> Csak az Azure AD Connect újabb verzióit (1.1.552.0, és utána) ObjectGuid tooConsistencyGuid hello Forráshorgony attribútumaként való váltás támogatja.

az objectGUID tooConsistencyGuid hello Forráshorgony attribútumaként tooswitch:

1. Hello Azure AD Connect varázsló elindításához, és kattintson a **konfigurálása** toogo toohello feladatok képernyő.

2. Jelölje be hello **Forráshorgony konfigurálása** beállítás feladatot, és kattintson a **következő**.

   ![A meglévő telepítés – 2. lépés ConsistencyGuid engedélyezése](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment01.png)

3. Adja meg Azure AD rendszergazdai hitelesítő adatait, és kattintson a **következő**.

4. Az Azure AD Connect varázsló elemzi a helyszíni Active Directoryban hello msDS-ConsistencyGuid attribútum hello állapotát. Ha hello attribútum nincs beállítva egy objektum a könyvtárban, az Azure AD Connect arra a következtetésre jut, hogy egyetlen másik alkalmazás éppen használja hello attribútum és biztonságos toouse hello azt hello Forráshorgony attribútumaként. Kattintson a **következő** toocontinue.

   ![A meglévő telepítés – 4. lépés ConsistencyGuid engedélyezése](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment02.png)

5. A hello **készen tooConfigure** kattintson **konfigurálása** toomake hello konfiguráció módosítását.

   ![A meglévő telepítés – 5. lépés ConsistencyGuid engedélyezése](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment03.png)

6. Hello konfigurálása után hello varázsló azt jelzi, hogy msDS-ConsistencyGuid most használatos hello Forráshorgony attribútum.

   ![A meglévő telepítés – 6. lépés ConsistencyGuid engedélyezése](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment04.png)

Hello elemzés (4. lépés) Ha hello attribútum konfigurálva van egy vagy több objektum hello könyvtárban hello varázsló arra a következtetésre jut hello attribútum egy másik alkalmazás használja, és az alábbi ábrán hello módon hibát ad vissza. Ha biztos benne, hogy hello attribútum nincs meglévő alkalmazások által használt, hogyan toosuppress hello hiba információt kell toocontact támogatása.

![ConsistencyGuid engedélyezése a meglévő telepítés – hiba](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeploymenterror.png)

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>Active Directory összevonási szolgáltatások vagy harmadik féltől származó összevonási konfiguráció gyakorolt hatás
Ha használ Azure AD Connect toomanage a helyszíni AD FS üzembe helyezése, az Azure AD Connect hello automatikusan frissíti a hello jogcím szabályok toouse hello ugyanazon AD attribútumra, mint a sourceAnchor. Ez biztosítja, hogy az AD FS által generált hello ImmutableID jogcím hello sourceAnchor értékek exportált tooAzure AD konzisztens legyen.

Ha az AD FS az Azure AD Connect kívül kezelt vagy harmadik féltől származó összevonási kiszolgálókat használnak a hitelesítéshez, manuálisan kell frissítenie hello jogcímszabályok ImmutableID jogcím toobe hello sourceAnchor értékek konzisztens exportált tooAzure AD mint az a cikk szakaszban leírt [módosítás az AD FS jogcímszabályokba](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). hello varázsló adja vissza a telepítés befejezése után a következő figyelmeztetés hello:

![Harmadik féltől származó összevonási konfigurációja](./media/active-directory-aadconnect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-tooexisting-deployment"></a>Új könyvtárak tooexisting központi telepítés hozzáadása
Tegyük fel, hogy telepítette az Azure AD Connect hello ConsistencyGuid szolgáltatás engedélyezve van, és most szeretné tooadd directory toohello egy másik telepítés. Amikor tooadd hello directory, az Azure AD Connect varázsló ellenőrzi hello mSDS-ConsistencyGuid attribútum hello könyvtárban hello állapotát. Ha egy vagy több objektum hello könyvtárban hello attribútum van beállítva, hello varázsló arra a következtetésre jut hello attribútum más alkalmazások használják, és az alábbi ábrán hello módon hibát ad vissza. Ha biztos benne, hogy hello attribútum nincs meglévő alkalmazások által használt, hogyan toosuppress hello hiba információt kell toocontact támogatása.

![Új könyvtárak tooexisting központi telepítés hozzáadása](./media/active-directory-aadconnect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Az Azure AD-bejelentkezés
A helyszíni címtár az Azure ad-vel integrálni fontos toounderstand hogyan hello szinkronizálási beállítások hatással lehet hello módon felhasználó hitelesíti magát. Az Azure AD userPrincipalName (UPN) tooauthenticate hello felhasználót használja. A felhasználók szinkronizálásakor hello attribútum toobe használt érték a userPrincipalName gondosan kell választania.

### <a name="choosing-hello-attribute-for-userprincipalname"></a>A userPrincipalName hello attribútum kiválasztása
Amikor kiválasztja a hello attribútum biztosítani azt az Azure egy használt UPN toobe hello értékének biztosítják.

* hello attribútumértékek toohello UPN szintaxis (RFC 822), az hello formátum kötelező felel megusername@domain
* hello utótagjának hello értékek egyezések tooone hello az egyéni tartományok ellenőrzése az Azure ad-ben

Gyorsbeállítások hello feltételezhető a userPrincipalName rendszer hello attribútum. Ha hello userPrincipalName attribútum nem tartalmaz hello érték azt szeretné, hogy a felhasználók toosign a tooAzure, majd ki kell választania **egyéni telepítési**.

### <a name="custom-domain-state-and-upn"></a>Az egyéni tartomány állapotának és az egyszerű felhasználónév
Fontos, hogy van-e az egyszerű Felhasználónévi utótagot hello ellenőrzött tartományt tooensure.

John az a felhasználó a contoso.com webhelyen. John kívánt toouse hello a helyszíni egyszerű Felhasználónévvel john@contoso.com toosign tooAzure követően a felhasználók tooyour az Azure Active directory contoso.onmicrosoft.com. toodo így szinkronizálta, tooadd kell, és ellenőrizze a contoso.com, az egyéni tartománynév az Azure ad-ben megkezdése előtt hello felhasználók szinkronizálása. Ha hello egyszerű Felhasználónévi utótagot János, például contoso.com, az Azure AD nem egyezik meg egy ellenőrzött tartomány, majd az Azure AD lecseréli contoso.onmicrosoft.com hello UPN-utótagot.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Nem átirányítható helyszíni tartományok és az Azure AD egyszerű felhasználónév
Egyes szervezeteknél nem átirányítható tartományok, mint a contoso.local vagy egyszerű egycímkés tartományok, mint például a contoso. Még nem tud tooverify nem átirányítható tartományhoz az Azure ad-ben. Az Azure AD Connect szinkronizálása tooonly ellenőrzött tartományt az Azure ad-ben. Az Azure AD-címtár létrehozásakor az Azure AD például contoso.onmicrosoft.com az alapértelmezett tartomány váló irányítható tartomány hoz létre. Ezért válik szükséges tooverify ilyen esetben más irányítható tartomány abban az esetben, ha nem szeretné, hogy toosync toohello alapértelmezett onmicrosoft.com tartomány.

Olvasási [adja hozzá az egyéni tartomány nevét tooAzure Active Directory](../active-directory-add-domain.md) további információt a hozzáadása és tartományok ellenőrzése.

Az Azure AD Connect azt észleli, ha nem átirányítható tartományi környezetben futtatja, és akkor megfelelően figyelmezteti a folytassa a gyorsbeállítások. Ha nem átirányítható tartományban, akkor valószínű, hogy hello hello felhasználói UPN nem átirányítható utótagok túl kell rendelkeznie. Például contoso.local le, ha az Azure AD Connect javasol gyorsbeállítások használata helyett, toouse egyéni beállításokat. Egyéni beállítások használatával tud toospecify hello attribútum követően hello felhasználók szinkronizált tooAzure AD, a tooAzure UPN toosign használandó áll.

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
