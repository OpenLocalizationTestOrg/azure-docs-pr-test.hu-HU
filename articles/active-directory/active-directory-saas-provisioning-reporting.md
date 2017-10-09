---
title: "Azure Active Directory automatikus felhasználói fiók kiépítése tooSaaS alkalmazások Reporting |} Microsoft Docs"
description: "Megtudhatja, hogyan toocheck hello automatikus felhasználói fiók kiépítése a feladatok állapotát, és hogyan tootroubleshoot hello kiépítés az egyéni felhasználók számára."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: asmalser-msft
ms.openlocfilehash: 5dcf9e5dbaacf3a2c81183c5d81e331858671b86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Oktatóanyag: Jelentések automatikus felhasználói fiók kiépítése


Az Azure Active Directory tartalmaz egy [szolgáltatás kiépítését felhasználói fiók](active-directory-saas-app-provisioning.md) , amelynek segítségével automatizálhatja hello kiépítés való üzembe helyezési SaaS-alkalmazásokhoz és más rendszerek végpont identitás-életciklus hello célra felhasználói fiókok felügyeleti. Az Azure AD támogatja az összes hello alkalmazások és a rendszeren a "Kiemelt" szakasz hello hello összekötők kiépítés előre integrált felhasználói [az Azure AD application gallery](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

Ez a cikk ismerteti, hogyan toocheck hello állapotának kiépítés feladatokat azok megadása után működik, és hogyan tootroubleshoot hello kiépítése egyes felhasználókat és csoportokat.

## <a name="overview"></a>Áttekintés

Üzembe helyezési összekötők elsősorban beállítása és hello segítségével konfigurálható: [Azure felügyeleti portálján](https://portal.azure.com), következő hello által [biztosított dokumentáció](active-directory-saas-tutorial-list.md) ahol felhasználóifiók-létesítési hello alkalmazáshoz van szükség. Konfigurálni és futtatni, ha egy alkalmazás feladatok kiépítés jelenthetők a két módszer egyikével:

* **Azure felügyeleti portálján** -Ez a cikk elsődlegesen ismerteti jelentések adatainak lekérése hello [Azure felügyeleti portálján](https://portal.azure.com), amely biztosítja, hogy mind a kiépítés összefoglaló jelentést, valamint részletes kiépítése a naplók egy adott alkalmazáshoz.

* **Naplózási API** -Azure Active Directory is biztosít a naplózási API lehetővé teszi a programozott beolvasása a hello részletes kiépítés naplók. Lásd: [Azure Active Directory naplózási API-referencia](active-directory-reporting-api-audit-reference.md) a dokumentáció adott toousing az API. Amíg ez a cikk nem kifejezetten megírásának módját toouse hello API, azt hello naplóban rögzített események kiépítés hello típusú adatok találhatók.

### <a name="definitions"></a>Meghatározások

Ebben a cikkben az alábbi feltételeket, az alábbiakban meghatározott hello:

* **Forrás-rendszer** -hello tárház, amely az Azure AD létesítési szolgáltatás hello szinkronizálja. Az Azure Active Directory hello forrásrendszerben hello többségének előre integrálva az összekötők kiépítés, azonban nincsenek kivételek (például: Workday bejövő szinkronizálás).

* **Cél rendszer** -hello tárház, amely az Azure AD létesítési szolgáltatás hello szinkronizálja. Ez általában az SaaS-alkalmazás (példák: Salesforce, a ServiceNow, a Google Apps, a Dropbox vállalati), azonban bizonyos esetekben lehet például az Active Directory egy a helyszíni rendszer (Példa: Workday bejövő szinkronizálási tooActive Directory).


## <a name="getting-provisioning-reports-from-hello-azure-management-portal"></a>Kiépítés hello Azure felügyeleti portálról jelentések beolvasása

kiépítési információinak jelentés egy adott alkalmazás tooget hello indításával start [Azure felügyeleti portálján](https://portal.azure.com) és böngészés toohello vállalati alkalmazás, amelynek kiépítés van konfigurálva. Például ha a felhasználók tooLinkedIn jogosultságszint-emelés kiépíteni, hello navigációs elérési toohello alkalmazás részletei a következő:

**Az Azure Active Directory > Vállalati alkalmazások > összes alkalmazás > LinkedIn jogosultságszint-emelés**

Itt hello kiépítési összefoglaló jelentést és hello létesítési naplókat, mind az alábbiakban.


### <a name="provisioning-summary-report"></a>Üzembe helyezési összegzési jelentése

hello kiépítés összefoglaló jelentést is elérhetővé válik a hello **kiépítési** megadott alkalmazás lapján. Hello szinkronizálás részletei szakasz alatt található **beállítások**, és biztosítja a következő információ hello:

* azon felhasználók száma hello és / csoportok, amely nincs szinkronizálva, és jelenleg a hatókör hello forrás és cél rendszer hello között történő üzembe helyezéséhez.

* hello utolsó hello időszinkronizálást futtatták. Minden 20-40 perc, a teljes szinkronizálás befejeződése után általában történjen szinkronizálás.

* Egy kezdeti teljes szinkronizálás befejeződött-e.

* -E a kiépítési folyamat hello karanténba helyezés került, és milyen hello hello karantén állapotának oka, hogy pl. (toocommunicate a célrendszeren miatt tooinvalid rendszergazdai hitelesítő adatokat. hiba)

kiépítés összesítő jelentés hello kell hello első helyi rendszergazdák megjelenését toocheck a hello kiosztási feladatának hello működési állapotát.

 ![Összegzési jelentése](./media/active-directory-saas-provisioning-reporting/summary_report.PNG)

### <a name="provisioning-audit-logs"></a>Telepítési naplófájlok
Hello szolgáltatás kiépítését által végzett tevékenységek tárolja, amely hello Azure AD naplókat, amelyen megtekinthetők az hello **naplók** lapon a hello **fiók** kategóriát. Naplózott esemény tevékenységtípusok a következők:

* **Események importálása** -"importálás" esemény rögzítése minden alkalommal, amikor hello Azure AD létesítési szolgáltatás lekérdezi egy adott felhasználó vagy csoport kapcsolatos információkat a forrásrendszerben vagy a célrendszeren. A szinkronizálás során felhasználók lekért hello forrásrendszerben először "importálása" események rögzített hello eredményekkel. hello beolvasott hello felhasználók egyező azonosítójú lekérdezést végzett hello cél rendszer toocheck majd vannak, ha vannak ilyenek, "importálása" események is rögzített hello eredményekkel. Ezeket az eseményeket rögzítse az összes csatlakoztatott felhasználói attribútumokat és azok értékei, hello esemény hello idején hello Azure AD létesítési szolgáltatás által látott volt. 

* **Szinkronizálási szabály események** – hello attribútum leképezési szabályokat hello eredményeit jelentés ezeket az eseményeket, és bármelyik konfigurált hatókörének meghatározásához szűrők, felhasználói adatok importálása és hello forrása és célja rendszerekből kiértékelése után. Például ha egy felhasználó a forrásrendszerben akkor számít elértnek toobe hatókörében történő üzembe helyezéséhez, és feltételezett toonot hello célrendszerben létezik, majd ez az esemény azt jelzi, hogy a felhasználó hello kiépítendő hello célrendszerben. 

* **Események exportálását** -"export" esemény rögzítése minden alkalommal, amikor üzembe helyezési hello Azure AD szolgáltatás egy felhasználói fiókot vagy csoportot objektum tooa célrendszeren írja. Ezeket az eseményeket rögzítse az összes felhasználói attribútumokat és azok értékei, írt által hello Azure AD létesítési szolgáltatás hello esemény hello idején. Ha hello felhasználói fiókot vagy csoportot objektum toohello célrendszeren írása közben hiba történt, akkor megjelenik itt.

* **Feldolgozni az eseményeket a letéti** -folyamat escrows fordulhat elő, ha hello szolgáltatás kiépítését közben művelet hiba lép fel, és tooretry hello művelet vissza az indító intervallumban idő kezdődik. Egy "letéti" esemény rögzítése minden alkalommal, amikor a telepítési művelet lett visszavonva.

Amikor megtekint egy adott felhasználó létesítési eseményeket, a hello események általában történik, az itt megadott sorrendben:

1. Importálás esemény: felhasználói hello forrásrendszerben kéri le a rendszer.

2. Importálás esemény: cél rendszer lekérdezett toocheck hello beolvasott hello felhasználó létezésének ellenőrzésével.

3. Szinkronizálási szabály esemény: a forrás-és cél felhasználó adatait rendszer összehasonlítja a konfigurált hello attribútum leképezési szabályok és a hatókör szűrők toodetermine Mi a teendő, ha van ilyen kell elvégezni.

4. Esemény exportálása: Ha hello szinkronizálási szabály eseményt meghatározni, hogy egy műveletet kell végrehajtani (pl. hozzáadás, frissítés, Törlés), majd hello művelet eredményeinek hello tárolja, amely az Exportálás esemény.

![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-provisioning-reporting/audit_logs.PNG)


### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Kiépítés események egy adott felhasználó keresése

hello leggyakoribb használati eset a naplók kiépítés hello toocheck hello előkészítési állapotát az egyes felhasználói fiókkal. egy adott felhasználó létesítési események utolsó hello mentése toolook:

1. Nyissa meg toohello **naplók** szakasz.

2. A hello **kategória** menü **fiók**.

3. A hello **dátumtartomány** menü, azt szeretné, hogy toosearch, jelölje be hello dátumtartományt

4. A hello **keresési** sáv, adja meg a toosearch kívánja hello felhasználó hello felhasználói Azonosítóját. hello azonosító érték formátuma bármilyen kiválasztott hello elsődleges egyező azonosító hello attribútum leképezési konfigurációban (pl. userPrincipalName vagy alkalmazott azonosítóját), meg kell egyeznie. hello azonosító értékének megadása kötelező, fog megjelenni a hello cél(ok) oszlop.

5. Nyomja le az Enter toosearch. először az eredmény hello megtartott legutóbbi események kiépítés.

6. Ha események ad vissza, vegye figyelembe a hello tevékenységtípusok, és hogy azok sikeres vagy sikertelen volt. Ha nincs találat, majd akkor hello felhasználó nem létezik, vagy még nem észlelt által hello létesítésének folyamatát kell használnia, ha a teljes szinkronizálás még nem fejeződött be.

7. Kattintson az egyes események kiterjesztett tooview részleteit, beleértve az összes felhasználó tulajdonság lekérése, értékeli ki, vagy hello esemény részeként írása.


### <a name="tips-for-viewing-hello-provisioning-audit-logs"></a>Tippek az hello kiépítés naplók megtekintése

Az olvashatóság érdekében ajánlott a hello Azure-portálon, válassza ki a hello **oszlopok** gombra, majd válassza ki a következő oszlopot:

* **Dátum** -látható hello dátum hello esemény történt.
* **Cél(ok)** -hello alkalmazást és egy felhasználói Azonosítót, amelyek hello témák hello esemény mutatja.
* **Tevékenység** -hello tevékenységtípus, korábban leírt módon.
* **Állapot** – akár hello esemény sikeres volt-e.
* **Állapotok** – Mi történt a kiépítés esemény hello összegzését.


## <a name="troubleshooting"></a>Hibaelhárítás

kiépítés összegző jelentés és a naplózási naplók hello segíti a rendszergazdák különböző felhasználói fiók kiépítése kapcsolatos problémák elhárítása kulcsfontosságú szerepet játszanak.

Kapcsolatos útmutatás a forgatókönyv-alapú tootroubleshoot automatikus felhasználók átadásához, lásd: [problémák konfigurálása és a felhasználók tooan alkalmazás kiépítés](active-directory-application-provisioning-content-map.md).


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
