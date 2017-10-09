---
title: "az Active Directory biztonságicsoport-alapú további forgatókönyvek licencelési aaaAzure |} Microsoft Docs"
description: "Azure Active Directory biztonságicsoport-alapú licencelési további forgatókönyvei"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: piotrci
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/02/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 782b7c9aa1c062a55c1241d69af673466f7a849c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-limitations-and-known-issues-using-groups-toomanage-licensing-in-azure-active-directory"></a>Forgatókönyvek, korlátozások és ismert problémák csoportok toomanage licencelés az Azure Active Directory használata

A következő útmutatást és példákat toogain tájékozottabbak Azure Active Directory (Azure AD) Csoportalapú licencelés hello használata.

## <a name="usage-location"></a>Használat helye

Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen. Licenc tooa felhasználói rendelhető, mielőtt hello rendszergazda toospecify hello **felhasználási hely** hello felhasználói tulajdonságát. A [hello Azure-portálon](https://portal.azure.com), megadhat **felhasználói** &gt; **profil** &gt; **beállítások**.

A licenc-hozzárendelést nélkül a megadott felhasználási hely összes felhasználója örökli hello könyvtár hello helye. Ha a felhasználók több helyen van, győződjön meg arról, hogy tooreflect, hogy megfelelően a felhasználói objektumok licenccel rendelkező felhasználók toogroups hozzáadása előtt.

> [!NOTE]
> Licenc-hozzárendelést soha nem módosítja a felhasználó meglévő használati hely érték. Azt javasoljuk, hogy mindig használat helye a felhasználó-létrehozási folyamat részeként az Azure AD (pl. keresztül AAD Connect konfigurációjának) – Ez biztosítja a hello licenc-hozzárendelést eredménye mindig megfelelő, és a felhasználók nem kapják meg a helyeken, amelyek nem szolgáltatások engedélyezett.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>A dinamikus csoportok a csoport-alapú licenc

A biztonsági csoporttal, ami azt jelenti, hogy az Azure AD dinamikus csoportok kombinálható Csoportalapú licencelési használhatja. Feladat futtatása ellen felhasználói objektum attribútumok tooautomatically dinamikus csoportok hozzáadása, és távolítsa el a felhasználók csoportokból.

Például létrehozhat egy dinamikus csoport egyes termékek készlete tooassign toousers keresi. Felhasználók hozzáadása az attribútumok szabályok által a minden egyes csoport akkor töltődik fel tagokkal, és minden egyes csoport, amelyeket ki szeretne tooreceive hozzárendelt hello licenceket. Hello attribútum helyszíni rendelhet és szinkronizálása az Azure AD-val, vagy kezelheti közvetlenül hello felhőben hello attribútum.

Licencek hozzárendelésének toohello felhasználói hozzáadásuk után toohello csoport követően rövid időn belül. Hello attribútum módosításakor hello felhasználó elhagyja hello csoportok, és a hello licencek eltávolítását.

### <a name="example"></a>Példa

Fontolja meg egy a helyszíni identitáskezelési megoldás, amely úgy dönt, hogy mely felhasználók rendelkezzenek hozzáféréssel tooMicrosoft webszolgáltatások hello példát. Használja **extensionAttribute1** toostore karakterlánc értékét képviselő hello licencek hello felhasználónak rendelkeznie kell. Az Azure AD Connect szinkronizálásának az Azure ad-val.

Felhasználók szükség lehet licenc, de nem egy másik, vagy mindkettőt kell. Íme egy példa, terjeszti Office 365 nagyvállalati E5 csomag és a nagyvállalati mobilitási + biztonsági (EMS) licencek, a csoportok toousers:

#### <a name="office-365-enterprise-e5-base-services"></a>Az Office 365 nagyvállalati E5 csomag: base szolgáltatások

![Képernyőkép az Office 365 nagyvállalati E5 csomag alap szolgáltatások](media/active-directory-licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: licenccel rendelkező felhasználók

![Képernyőkép az Enterprise Mobility + Security licenccel rendelkező felhasználók](media/active-directory-licensing-group-advanced/o365-e5-licensed-users.png)

Ebben a példában egy felhasználók módosításához, és állítsa be a extensionAttribute1 toohello `EMS;E5_baseservices;` Ha azt szeretné, hello felhasználói toohave mindkét licenceket. Ez a módosítás végezheti el a helyszínen. Hello után módosítsa a hello felhő szinkronizál, hello felhasználó automatikusan fel lesz véve tooboth csoportok és licencek vannak rendelve.

![Hogyan tooset hello felhasználó extensionAttribute1 ábrázoló képernyőfelvétel](media/active-directory-licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Körültekintően járjon el, ha módosítja egy meglévő csoport tagsági szabályt. Ha egy szabály módosul, hello csoport tagságának hello újra kell értékelni, és nem megfelelő felhasználók hello új szabály eltávolítja (felhasználók, akik továbbra is egyezik az új szabály hello nem érinti a folyamat során). Azoknak a felhasználóknak kell eltávolítása során hello szolgáltatáskimaradást, illetve bizonyos esetekben előfordulhat, hogy eredmények adatvesztés licencét.

> Ha egy nagy dinamikus csoport függ a licenc-hozzárendelést, fontolja meg, mielőtt alkalmazná őket toohello fő csoport lényegesen módosul a kisebb Tesztcsoport ellenőrzése.

## <a name="multiple-groups-and-multiple-licenses"></a>Több csoportot, és több licenc

A felhasználói licencek több csoport tagja lehet. Az alábbiakban néhány dolgot tooconsider:

- Egyszerre több licencet hello átfedhetik a ugyanazt a terméket, és az összes engedélyezve szolgáltatások alkalmazott toohello felhasználói alatt. hello következő példában két licencelési csoportok: *E3 alap szolgáltatások* hello foundation toodeploy első, tooall felhasználóinak tartalmazza. És *kibővített szolgáltatások E3* kiegészítő szolgáltatásokat (lengése és Planner) toodeploy csak toosome felhasználókat tartalmazza. Ebben a példában a hello felhasználó hozzá lett adva tooboth csoportok:

  ![Képernyőkép a engedélyezett szolgáltatások](media/active-directory-licensing-group-advanced/view-enabled-services.png)

  Hello felhasználó rendelkezik-emiatt hello 12 szolgáltatások 7 hello termék engedélyezve van, csak egy-egy licencet a termékhez tartozó használata során.

- Kiválasztásával hello *E3* licenc további adatait, többek között információkat arról, hogy mely csoportok milyen szolgáltatások toobe hello felhasználó okozott jeleníti meg.

  ![Képernyőfelvétel a csoport által engedélyezett szolgáltatások](media/active-directory-licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Csoport-licenccel rendelkező lehet közvetlen licencek

Ha egy felhasználó egy licencet a csoportból, nem közvetlenül távolítsa el és nem módosíthatók, hogy a licenc-hozzárendelést hello felhasználói tulajdonságok. Hello csoportba kell tenni, majd propagálása tooall felhasználók módosításokat.

Lehetséges, azonban tooassign hello-e azonos terméklicenc közvetlen toohello felhasználó, továbbá toohello örökölt licenc. Engedélyezheti a további szolgáltatások hello terméket csak egy felhasználó, az nem befolyásolja a többi felhasználó.

Közvetlenül a hozzárendelt licenceket távolítható el, és nincsenek hatással a örökölt licenceket. Fontolja meg az Office 365 nagyvállalati E3 csomag licencek örököl egy csoport hello felhasználó.

1. Kezdetben hello felhasználói örököl hello licenc csak hello *E3 alapvető szolgáltatások* csoport, amely lehetővé teszi, hogy négy service-csomagokról látható módon:

  ![Képernyőfelvétel a E3 csoport engedélyezett szolgáltatások](media/active-directory-licensing-group-advanced/e3-group-enabled-services.png)

2. Kiválaszthatja **hozzárendelése** toodirectly rendelje hozzá egy E3 licenc toohello felhasználót. Ebben az esetben is toodisable minden service-csomagok Yammer vállalati kivételével:

  ![Képernyőkép a hogyan tooassign licenc közvetlenül tooa felhasználó](media/active-directory-licensing-group-advanced/assign-license-to-user.png)

3. Ennek eredményeképpen a hello felhasználó továbbra is használja a hello E3 termék csak egy-egy licencet. De hello rendelését lehetővé teszi, hogy a vállalati Yammer-szolgáltatás az adott felhasználó csak hello. Láthatja, hogy melyik szolgáltatás be legyen kapcsolva hello csoport tagsága hello rendelését és:

  ![Képernyőkép a örökölt és közvetlen hozzárendelés](media/active-directory-licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Közvetlen hozzárendelés használatakor a következő műveletek hello engedélyezettek:

  - Yammer vállalati kikapcsolható hello user objektum közvetlenül. Hello **be- / kikapcsolása** hello ábra váltás ehhez a szolgáltatáshoz, más megakadályozását toohello engedélyezett szolgáltatás be-és kikapcsolása. Hello szolgáltatás közvetlenül hello felhasználó engedélyezett, mert módosítható.
  - További szolgáltatásokat, valamint közvetlenül a licenc hozzárendelése hello részeként is engedélyezhető.
  - Hello **eltávolítása** gomb használt tooremove hello közvetlen licencét hello felhasználó lehet. Láthatja, hogy hello felhasználói most már csak hello örökölt csoport licenccel rendelkezik, és csak hello eredeti szolgáltatások továbbra is engedélyezett:

    ![Hogyan tooremove közvetlen hozzárendelés ábrázoló képernyőfelvétel](media/active-directory-licensing-group-advanced/remove-direct-license.png)

## <a name="managing-new-services-added-tooproducts"></a>Új szolgáltatások kezelése hozzáadott tooproducts
Ha Microsoft hozzáadja az új szolgáltatás tooa terméket, lesz engedélyezve alapértelmezés szerint a hozzárendelt összes csoportok toowhich hello termék licencét. Felhasználók az Ön bérelt szolgáltatásának termék változásaira vonatkozó előfizetett toonotifications előre hello jövőbeli szolgáltatás kiegészítéseket kapcsolatos értesítő e-mailt fog kapni.

Rendszergazdaként nézze át az hello változás által érintett összes csoportot, és a szükséges műveletek, például hello új szolgáltatás minden csoport letiltása. Például ha létrehozott csoportok célzó központi telepítés csak meghatározott szolgáltatásokat, akkor is le újra azokat a csoportokat és győződjön meg arról, hogy újonnan hozzáadott összes szolgáltatások le vannak tiltva.

Íme egy példa mi nézhet ki ezt a folyamatot:

1. Eredetileg, hozzárendelt hello *Office 365 nagyvállalati E5 csomag* termék tooseveral csoportok. Ezek között nevű *O365 E5 - csak Exchange* tervezett tooenable csak hello lett *Exchange Online (megtervezése 2)* szolgáltatás a tagjai.

2. Értesítést kapott, amely a termék egy új szolgáltatással - kiterjesztése E5 hello Microsoft *Microsoft Stream*. Amikor hello szolgáltatás az Ön bérelt szolgáltatásának elérhetővé válik, megteheti hello következő:

3. Nyissa meg toohello [ **Azure Active Directory > licencek > összes** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) panelhez, és válassza *Office 365 nagyvállalati E5 csomag*, majd jelölje be **licenccel rendelkező csoportok**  tooview a terméket az összes csoport listája.

4. Kattintson a hello csoport tooreview szeretné (ebben az esetben *O365 E5 - csak Exchange*). Ekkor megnyílik hello **licencek** fülre. Hello E5 licenc kattintva megnyílik egy panel, az összes engedélyezett szolgáltatások listázása.
> [!NOTE]
> Hello *Microsoft Stream* szolgáltatás automatikusan hozzáadott és engedélyezve az ebbe a csoportba, továbbá toohello *Exchange Online* szolgáltatás:

  ![Képernyőkép a új szolgáltatás hozzáadott tooa csoport licenc](media/active-directory-licensing-group-advanced/manage-new-services.png)

5. Toodisable hello új szolgáltatás ennek a csoportnak, kattintson a hello **be- / kikapcsolása** Váltás a következő toohello szolgáltatást, majd kattintson a hello **mentése** tooconfirm hello módosítása gombra. Az Azure AD mostantól feldolgozza hello csoport tooapply hello módosítása; a minden felhasználó új felhasználók hozzáadott toohello a csoport nem rendelkezik hello *Microsoft Stream* szolgáltatás engedélyezve van.

  > [!NOTE]
  > Előfordulhat, hogy a felhasználók továbbra is fennáll hello szolgáltatás néhány más licenc-hozzárendelést (egy másik csoport tagjai, vagy egy közvetlen licenc-hozzárendelést) keresztül engedélyezhető.

6. Ha szükséges, hajtsa végre a hello ugyanezekkel a lépésekkel más, a termék csoportok hozzárendelve.

## <a name="use-powershell-toosee-who-has-inherited-and-direct-licenses"></a>PowerShell toosee, akik örökölt használja, és közvetlen licencek
A PowerShell parancsfájl toocheck is használhatja, ha a felhasználók jogosultak hozzárendelt közvetlenül vagy öröklés útján egy csoporttól származik.

1. Futtassa a hello `connect-msolservice` parancsmag tooauthenticate és tooyour bérlői csatlakozzon.

2. `Get-MsolAccountSku`lehet használt toodiscover hello bérlő összes kiosztott terméklicencek.

  ![Képernyőfelvétel a hello Get-Msolaccountsku parancsmag](media/active-directory-licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. Használjon hello *AccountSkuId* hello licenc érdekli, azzal a következő [a PowerShell parancsfájl](./active-directory-licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group). Ez a művelet létrehoz az ezt a licencet, hogyan rendel hozzá licencet hello hello adatait tartalmazó rendelkező felhasználók listáját.

## <a name="use-audit-logs-toomonitor-group-based-licensing-activity"></a>Naplózási naplók toomonitor Csoportalapú licencelési tevékenységbe

Használhat [az Azure AD-naplók](./active-directory-reporting-activity-audit-logs.md#audit-logs) toosee összes tevékenységhez kapcsolódó toogroup-alapú licencelési, beleértve:
- ki módosította a csoportok a licenceket
- licenc csoport módosítása hello rendszer indításakor, és befejezésekor,
- licenc változásait tooa felhasználói eredményeként egy licenc-hozzárendelést.

>[!NOTE]
> Naplók hello hello portál Azure Active Directory szakaszban szereplő legtöbb panelen érhetők el. Attól függően, hogy elérhetik őket szűrők lehet előre alkalmazott tooonly megjelenítése tevékenység megfelelő toohello környezetben hello panelről. Ha nem Ön hello várt eredményt, ellenőrizze [szűrési beállítások hello](./active-directory-reporting-activity-audit-logs.md#filtering-audit-logs) vagy nem érhető el a szűrés hello naplókat [ **Azure Active Directory > tevékenység > Naplók** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit).

### <a name="find-out-who-modified-a-group-license"></a>A csoport licenc módosítások megállapítása

1. Set hello **tevékenység** túl szűrése*Set csoport licenc* kattintson **alkalmaz**.
2. hello eredmények tartalmazzák a licencek alatt állítsa be, illetve módosítása az csoportok minden esetben.
>[!TIP]
> Hello hello csoport nevét írja be a hello *cél* tooscope hello eredmények szűrésére.

3. Kattintson egy hello lista toosee hello részletes adatainak megtekintése elemre, hogy mi változott. A *módosított tulajdonságok* hello licenc-hozzárendelést a régi és az új értékek találhatók.

Íme egy példa a legutóbbi csoport licenc módosításait, adatokkal:

![Képernyőkép csoport licenc módosításai](media/active-directory-licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Megállapítása csoport különböző módosításai elindult és feldolgozása befejeződött

A licenc módosítja a csoport, az Azure AD megkezdi hello módosítások tooall felhasználók alkalmazása.

1. csoportok indításakor feldolgozása, toosee hello beállítása **tevékenység** túl szűrése*indítsa el a csoport licenc toousers alkalmazása*. Vegye figyelembe, hogy hello szereplő hello művelet van *Microsoft Azure AD Csoportalapú licencelés* -a system fiók, amely az összes csoport licenc módosítások használt tooexecute van.
>[!TIP]
> Hello lista toosee hello elemeire *módosított tulajdonságok* mező - feldolgozás céljából kivett hello licenc módosítások azt jeleníti meg. Ez akkor hasznos, ha több változások tooa csoport végzett, és nem biztos abban, hogy melyik lett feldolgozva.

2. Hasonlóképpen, a feldolgozást, használjon hello szűrőérték csoportok befejezésekor toosee *Befejezés csoport licenc toousers alkalmazása*.
>[!TIP]
> Ebben az esetben hello *módosított tulajdonságok* mező hello eredmények összegzését tartalmazza – ez hasznos tooquickly ellenőrizze akkor, ha a feldolgozási hibák eredményezett. Példa a kimenetre:
> ```
Modified Properties
...
Name : Result
Old Value : []
New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
> ```

3. toosee hello teljes keresse meg a csoport lett feldolgozásának módja, beleértve az összes felhasználót módosítások, állítsa be a következő szűrők hello:
  - **(Aktor) által kezdeményezett**: "Microsoft Azure AD Csoportalapú licencelési"
  - **Dátumtartomány** (nem kötelező): Ha egy adott felhasználócsoportnak tudja az egyéni tartomány elindult és feldolgozása befejeződött

A minta az alábbiakat mutatja be hello start feldolgozási, minden eredményül kapott felhasználói érintő változtatások, valamint hello Befejezés feldolgozása.

![Képernyőkép csoport licenc módosításai](media/active-directory-licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> Túl kapcsolódó elemeket*módosítás felhasználói licenc* licenc alkalmazott módosítások tooeach egyes felhasználói az információk jelennek meg.

## <a name="limitations-and-known-issues"></a>A korlátozásokkal, valamint az ismert problémák

Csoportalapú licencelési használatához esetén a jó ötlet toofamiliarize saját magának az ismert problémák és korlátozások listáját a következő hello.

- Jelenleg Csoportalapú licencelési nem támogatja a más (beágyazott csoportok) tartalmazó csoportok. Ha alkalmaz egy tooa beágyazott licenccsoportot, csak hello azonnali első szintű felhasználó hello csoport tagjai rendelkeznek hello licencek alkalmazza.

- hello szolgáltatás csak a biztonsági csoportok használatával használható. Office-csoportok jelenleg nem támogatottak és nem fogja tudni toouse hello licenc hozzárendelési folyamat őket.

- Hello [Office 365 felügyeleti portál](https://portal.office.com ) jelenleg nem támogatja a licencelési biztonságicsoport-alapú. Ha a felhasználó licenc örököl egy csoportot, ez a licenc jelenik meg hello Office felügyeleti portál normál felhasználói licencet. Ha licenc, vagy próbálja tooremove hello licenc toomodify, hello portal hibaüzenetet ad vissza. Örökölt csoport licencek nem módosíthatók közvetlenül a felhasználó.

- Ha a felhasználó el lesz távolítva a csoportból, és elveszíti hello licenc, hello service-csomagokról (például a SharePoint Online) licencből, hogy megfelelőek-e tooa **felfüggesztett** állapotát. hello service-csomagok nincsenek beállítva az tooa végleges, letiltott állapotban. Ez okokból véletlen eltávolítását felhasználói adatokat, elkerülheti, ha egy rendszergazda hibásan tagsági kezelése.

- Licencek hozzárendelése vagy módosításakor a nagy méretű csoport (például 100 000 felhasználó), jelentős hatással lehet a teljesítmény. Pontosabban, az Azure AD-automatizálás által végrehajtott módosítások hello mennyiségű negatív hatással lehet a hello teljesítményét a címtár-szinkronizálás az Azure AD között és a helyszíni rendszereket.

- Licenc Szolgáltatáskezelés-automatizálás automatikusan reagál tooall típusú hello környezet változásait. Például akkor valószínűleg elfogyott licencek, egyes felhasználók toobe okozó hibás állapotú. toofree mentése hello elérhető ülések száma, néhány közvetlenül hozzárendelt licencekkel eltávolíthatja a más felhasználóktól. Azonban hello rendszer automatikusan reagálni toothis módosítása, és javítsa ki a felhasználók az adott hiba állapotban.

  Korlátozások, a megoldás toothese típusú toohello lépjen **csoport** panel az Azure ad-ben, és kattintson a **újból feldolgozza**. Ez a parancs dolgozza fel, hogy a csoport összes felhasználója, és oldja fel a hello hiba állapotokat, ha lehetséges.

- Csoportalapú licencelési nem rögzíti a hibákat, ha a licenc nem rendelhető tooa felhasználói miatt tooa ismétlődő proxy címének konfigurációja, az Exchange Online; Ezek a felhasználók rendszer eltekint a licenc-hozzárendelést. További információ tooidentify és a probléma megoldásához, lásd: [ebben a szakaszban](./active-directory-licensing-group-problem-resolution-azure-portal.md#license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online).

## <a name="next-steps"></a>Következő lépések

További információ az egyéb forgatókönyvek Licenckezelés Csoportalapú licenceléssel, toolearn lásd:

* [Mi az a csoport-alapú licencelése az Azure Active Directoryban?](active-directory-licensing-whatis-azure-portal.md)
* [Az Azure Active Directoryban licencek tooa csoportok átjáróalhálózathoz való hozzárendelése](active-directory-licensing-group-assignment-azure-portal.md)
* [Majd azonosítani és megoldani az Azure Active Directory csoport licenc problémák](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Hogyan toomigrate személy licenccel rendelkező felhasználók toogroup alapú licencelése az Azure Active Directoryban](active-directory-licensing-group-migration-azure-portal.md)
