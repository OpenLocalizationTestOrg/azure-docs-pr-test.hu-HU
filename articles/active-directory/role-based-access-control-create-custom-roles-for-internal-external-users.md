---
title: "egyéni szerepkörök aaaCreate a szerepköralapú hozzáférés-vezérlés, és rendelje hozzá a toointernal és a külső felhasználók az Azure-ban |} Microsoft Docs"
description: "PowerShell és a parancssori felület használatával a belső és külső felhasználók számára létrehozott egyéni RBAC-szerepkörök hozzárendelése"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: 26793a66d6ca2f771338eed87d10ce2b3b431841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="intro-on-role-based-access-control"></a>A szerepköralapú hozzáférés-vezérlés – bevezetés

Szerepköralapú hozzáférés-vezérlés az Azure portál csak szolgáltatása: lehetővé előfizetés hello tulajdonosainak tooassign részletes szerepkörök tooother felügyelheti az adott erőforrás hatókörök a környezetükben.

Szerepalapú lehetővé teszi, hogy jobb biztonságkezelés nagy méretű szervezeteknek, és az SMB-khez külső közreműködő, a szállítók vagy freelancers, amelyhez kell hozzáférhet a környezetben, de nem feltétlenül toohello teljes infrastruktúrát és bármely toospecific erőforrások használata a számlázással kapcsolatos hatókörök. Szerepalapú lehetővé teszi, hogy a hello rugalmasan hello rendszergazdai fiókot (szolgáltatás-rendszergazda szerepkörrel egy előfizetés szintjén) által kezelt egy Azure-előfizetéssel rendelkező és rendelkezik a több meghívott felhasználók toowork hello ugyanahhoz az előfizetéshez de minden felügyeleti jogosultságok az. Felügyeleti és számlázási szempontból hello RBAC szolgáltatás egy idő és a felügyeleti hatékony beállítása Azure használatával a különböző forgatókönyvekben bizonyítja toobe.

## <a name="prerequisites"></a>Előfeltételek
Az RBAC használatát hello Azure-környezetéhez szükséges:

* Önálló rendelkezik Azure-előfizetés hozzárendelt toohello felhasználó tulajdonosa (előfizetés szerepkör)
* Hello tulajdonos szerepe hello Azure-előfizetéssel rendelkezik
* Hozzáférés toohello rendelkezik [Azure-portálon](https://portal.azure.com)
* Ellenőrizze, hogy a következő erőforrás-szolgáltató toohave hello hello felhasználói előfizetés regisztrálva: **Microsoft.Authorization**. Hogyan tooregister hello erőforrás-szolgáltató további információkért lásd: [Resource Manager szolgáltatók, régiók, API verziók és sémák](/azure-resource-manager/resource-manager-supported-services.md).

> [!NOTE]
> Office 365-előfizetéssel, vagy az Azure Active Directory-licencek (például: tooAzure Active Directory eléréséhez) kiosztott hello Office 365 portálon nem minőségi az RBAC használata.

## <a name="how-can-rbac-be-used"></a>Hogyan lehet használni az RBAC
Az RBAC alkalmazni lehet az Azure-ban három különböző hatóköröket. A hello legmagasabb hatókör toohello legalacsonyabb egyet akkor a következők:

* Előfizetés (legmagasabb)
* Erőforráscsoport
* Erőforrás hatókör (hello legalacsonyabb hozzáférési szint ajánlat célzott engedélyek tooan egyes Azure-erőforrás hatókör)

## <a name="assign-rbac-roles-at-hello-subscription-scope"></a>Hello előfizetés hatókörből RBAC-szerepkörök hozzárendelése
Nincsenek két gyakori példán RBAC használja (de nem kizárólagosan):

* Külső felhasználók hello szervezetek (nem hello rendszergazdai jogú felhasználó Azure Active Directory-bérlő része) rendelkező meghívott toomanage, bizonyos erőforrások vagy hello teljes előfizetés
* A felhasználók hello szervezet (részét képezik hello felhasználó Azure Active Directory-bérlő), de a különböző csapatok vagy csoportokat, amelyek részletes hozzáférésre van szükségük belső használata vagy toohello teljes előfizetés vagy toocertain erőforráscsoportok vagy az erőforrás hatóköröket a hello környezet

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Hozzáférés egy felhasználó Azure Active Directory kívül egy előfizetés szintjén
Az RBAC-szerepkörök csak akkor adhatók **tulajdonosok** hello előfizetés ezért hello rendszergazdai jogú felhasználó kell bejelentkeznie az előzetes szerepkörrel vagy hello Azure-előfizetést hozott létre egy felhasználónévvel.

Hello Azure-portálon, a követően bejelentkezhet rendszergazdaként, válassza ki "Előfizetések" és a kiválasztott hello kívánt egyet.
![előfizetés panel az Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) alapértelmezés szerint ha hello rendszergazdai jogú felhasználó megvásárolta hello Azure-előfizetésre, hello felhasználói állapotúként jelenik meg **Fiókadminisztrátor**, a hello előfizetés szerepkör alatt. Hello Azure-előfizetés szerepkörök további részletekért lásd: [kezelése hello előfizetés vagy szolgáltatások hozzáadása vagy módosítása Azure-rendszergazdai szerepkörök](/billing/billing-add-change-azure-subscription-administrator.md).

Ebben a példában a felhasználó hello "alflanigan@outlook.com" hello van **tulajdonos** az "Ingyenes" hello hello aad-ben az előfizetéshez bérlői "Alapértelmezett bérlőt Azure". Mivel ez a felhasználó hello Azure-előfizetéssel rendelkező hello hello létrehozója kezdeti Microsoft Account "Outlook" (Microsoft Account = Outlook, a működés közbeni stb.) hello alapértelmezett tartomány nevét ennél a bérlőnél a hozzáadott összes többi felhasználó számára lesz **"@alflaniganuoutlook.onmicrosoft.com"**. Úgy lett kialakítva, hello szintaxis hello új tartomány által létrehozott hello bérlői és hozzáadását hello bővítmény hello felhasználó felhasználónevét és tartományát neve hello üzembe formátuma **". onmicrosoft.com"**.
Ezenkívül felhasználók is jelentkezzen be egy egyéni tartománynevet hello bérlői hozzáadása és az új bérlő hello ellenőrzése után. Hogyan tooverify egy egyéni tartománynevet, Azure Active Directory-bérlő: vonatkozó részletes információért [adja hozzá ezeket az egyéni tartomány nevét tooyour](/active-directory/active-directory-add-domain).

Ebben a példában hello "alapértelmezett bérlőt Azure" címtár csak a felhasználók hello tartománynévvel tartalmaz "@alflanigan.onmicrosoft.com".

Hello előfizetés kiválasztása után kattintson kell az hello rendszergazdai jogú felhasználó **hozzáférés-vezérlés (IAM)** , majd **új szerepkör hozzáadása**.





![hozzáférés-vezérlési IAM funkciója Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Új felhasználó hozzáadása a hozzáférés-vezérlési IAM funkciója Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

hello következő lépésre tooselect hello szerepkör toobe hozzárendelt és hello felhasználó, akinek hello RBAC szerepkör rendeli hozzá. A hello **szerepkör** legördülő menü hello rendszergazdai jogú felhasználó számára megjelenített csak hello beépített RBAC szerepkörök, amelyek elérhetők az Azure-ban. Részletesebb ismereteket szeretnének elsajátítani a minden egyes szerepkör és a hozzárendelhető hatókörök, lásd: [átruházásához hozzáférés-vezérlés beépített szerepkörök](/active-directory/role-based-access-built-in-roles.md).

hello rendszergazdai jogú felhasználó majd kell hello külső felhasználó tooadd hello e-mail címét. hello várt viselkedés hello külső felhasználói toonot belül megjelennek a hello meglévő bérlő számára. Után hello külső felhasználói kérték, ő lesz látható a **előfizetések > hozzáférés-vezérlés (IAM)** már hozzá vannak rendelve egy Szerepalapú szerepkör hello előfizetési hatókört, amely minden hello aktuális felhasználókkal.





![engedélyek toonew RBAC-szerepkör hozzáadása](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![előfizetés szintjén RBAC-szerepkörök listája](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

hello felhasználói "chessercarlton@gmail.com" meghívott toobe lett egy **tulajdonos** hello "Ingyenes" előfizetés esetében. Hello meghívót küld, miután hello külső felhasználó kap egy e-mailes megerősítés egy aktiválási hivatkozást tartalmazó.
![e-mailek meghívó RBAC-szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Külső toohello szervezet folyamatban, hello új felhasználó nem rendelkezik meglévő attribútuma hello "Alapértelmezett bérlőt Azure" könyvtárban. Miután hello külső felhasználói hozzájárulás toobe hello előfizetéshez, amely rendelt szerepkör hello directory rögzített adott létrejön.





![meghívót e-mailt az RBAC-szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

hello Azure Active Directory-bérlő mostantól külső felhasználóként hello külső felhasználó látható, és ez hello Azure-portál és a klasszikus portálon hello lehet megtekinteni.





![felhasználók panel az azure active Directoryval Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![felhasználók panel az azure active Directoryval a klasszikus Azure portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

A hello **felhasználók** mindkét portálok hello külső felhasználók nézet felismeri:

* hello más ikon típusának hello Azure-portálon
* hello különböző forrás pont hello klasszikus portál

Azonban biztosítása **tulajdonos** vagy **közreműködő** hozzáférés felhasználói tooan-külső hello **előfizetés** hatókörét, nem engedélyezi a hello hozzáférés toohello rendszergazda felhasználó mappájába, Ha hello **globális rendszergazda** lehetővé teszi. Hello felhasználói tulajdonságokat, a hello **felhasználótípust** két közös paramétert tartalmaz **tag** és **vendég** azonosítható legyen. A tagja a felhasználó, amely hello könyvtárban regisztrálva van, míg a Vendég a meghívott felhasználó toohello directory külső forrásból. További információkért lásd: [hogyan Azure Active Directory rendszergazdák hozzá B2B együttműködés felhasználók](/active-directory/active-directory-b2b-admin-add-users).

> [!NOTE]
> Győződjön meg arról, hogy után hello hitelesítő adatok megadása hello portálon, hello külső felhasználó hello megfelelő directory toosign a választ. hello ugyanahhoz a felhasználóhoz is hozzáférés toomultiple könyvtárai és is hello felhasználónév a hello Azure-portálon jobb oldalán felső hello kattintva válassza ki bármelyik egyik és válassza hello megfelelő directory hello legördülő listából.

A Vendég hello könyvtárban, miközben hello külső felhasználó hello Azure-előfizetéshez tartozó összes erőforrást is kezelheti, de hello könyvtár nem hozzáférhető.





![hozzáférés tiltott tooazure active-directory Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Az Azure Active Directory és az Azure-előfizetés nem rendelkezik egy szülő-gyermek kapcsolat, például a más Azure-erőforrások (például: virtuális gépek, virtuális hálózatok, a webalkalmazások, tárolási stb.) az Azure-előfizetéssel rendelkezik. Ez utóbbi összes hello létrehozott, felügyelt, és amíg Azure-előfizetés használt toomanage hello hozzáférés tooan Azure-címtár az Azure-előfizetés számlázva. További részletekért lásd: [hogyan Azure-előfizetés, kapcsolódó tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Az összes hello beépített RBAC szerepkörből **tulajdonos** és **közreműködő** kínálnak a teljes felügyeleti hozzáférés tooall erőforrások hello környezetben, hello különbség folyamatban, amely egy közreműködői nem hozható létre, és új törlése Az RBAC-szerepkörök. hello más beépített szerepkörök, például **virtuális gép közreműködő** összes felügyeleti hozzáférés csak a megjelölt hello neve, függetlenül attól, hello toohello erőforrásokat kínálnak **erőforráscsoport** azok létrehozása .

Hozzárendelése hello beépített RBAC szerepe **virtuális gép közreműködő** egy előfizetés szintjén azt jelenti, hogy hello felhasználó lehet hozzárendelve hello szerepkör:

* Megtekintheti az összes virtuális gép függetlenül azok telepítési dátumát és hello erőforráscsoportok részét képezik
* Összes felügyeleti hozzáférés toohello virtuális gépe van, a hello előfizetés
* Nem lehet megtekinteni, bármilyen más típusú erőforrások hello az előfizetéshez
* A módosításokat a számlázási szempontjából nem tud működni.

> [!NOTE]
> Az RBAC alatt az Azure portál csak szolgáltatásai, azt nem adja meg hozzáférés toohello klasszikus portálon.

## <a name="assign-a-built-in-rbac-role-tooan-external-user"></a>Beépített RBAC szerepkör tooan külső felhasználó hozzárendelése
Ez a vizsgálat a különböző forgatókönyvek esetében a külső felhasználó hello "alflanigan@gmail.com" meg van adva egy **virtuális gép közreműködő**.




![virtuális gép közreműködő beépített szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

hello ezek normál viselkedése a külső felhasználó beépített szerepkörrel rendelkező toosee, és csak a virtuális gépek és a szomszédos erőforrás-kezelő csak szükséges erőforrások másikban kezelése. Úgy lett kialakítva, a korlátozott szerepkörök kínálnak hozzáférés csak tootheir levelező erőforrások hello Azure-portálon létrehozott, attól függetlenül történik néhány továbbra is telepíthető a hello, valamint a klasszikus portálon (például: virtuális gépek).





![virtuális gép közreműködői szerepkör áttekintése azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-hello-same-directory"></a>Azonos hello felhasználójának előfizetés szintű hozzáférés GRANT könyvtár
hello folyamata azonos tooadding külső felhasználó, mind hello admin perspektíva támogatást nyújtó hello RBAC szerepkör, valamint a hozzáférés toohello szerepkör engedély hello felhasználó. hello itt különbség, hogy hello meghívott felhasználó nem kap minden e-mailek meghívókat, a bejelentkezés után minden hello erőforrás hatókör hello előfizetésen belül hello irányítópult elérhető lesz.

## <a name="assign-rbac-roles-at-hello-resource-group-scope"></a>Hello erőforrás csoport hatókörű RBAC-szerepkörök hozzárendelése
Hozzárendelése egy Szerepalapú szerepkör a(z) egy **erőforráscsoport** hatókörben van egy azonos folyamata hello előfizetés szintjén, a felhasználók – külső vagy belső mindkét típusú hello szerepkör hozzárendelése (hello része könyvtárába). hello hello RBAC szerepkörhöz hozzárendelt felhasználók csak a környezetükben toosee hello erőforráscsoportban van hozzájuk rendelve hozzáférés a hello **erőforráscsoportok** hello Azure-portálon ikonra.

## <a name="assign-rbac-roles-at-hello-resource-scope"></a>Hello erőforrás hatókörből RBAC-szerepkörök hozzárendelése
Az Azure erőforrás-hatókörben egy Szerepalapú szerepkör hozzárendelése hello előfizetés szintjén vagy hello erőforráscsoport szintjén hello szerepkör hozzárendelése egy azonos folyamata, a következő hello azonos mindkét forgatókönyvet munkafolyamata. Ebben az esetben hello hello RBAC szerepkörhöz hozzárendelt értesülhet, hogy azok hozzárendelt hozzáférés, vagy a hello csak hello elemek **összes erőforrás** lapon vagy közvetlenül az irányítópulton.

Az RBAC egyaránt erőforrás csoporthatókör vagy erőforrás hatókör fontos eleme hello felhasználók toomake toohello meg arról, hogy toosign-a megfelelő könyvtárban van.





![Directory bejelentkezés az Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Egy Azure Active Directory csoport RBAC-szerepkörök hozzárendelése
Az RBAC használata a következő három különböző hatókörök hello Azure-ajánlat hello jogosultság kezelése, a központi telepítését és a különböző erőforrások felügyelete nélkül hello hozzárendelt felhasználóként az összes hello forgatókönyv kell a személyes előfizetés kezelésére. Függetlenül attól, hogy hello RBAC szerepkör tartozik előfizetés, erőforráscsoporthoz vagy erőforrás-hatókör, tovább hello hozzárendelt felhasználók által létrehozott összes hello erőforrások számlázása a hello egy Azure-előfizetéssel ahol hello felhasználók férhetnek hozzá. Ezzel a módszerrel hello számlázási a teljes Azure-előfizetéshez rendszergazdai jogosultságokkal rendelkező felhasználók rendelkezik teljes áttekintése hello fogyasztás, függetlenül attól, hogy a kezelő hello erőforrásokat.

Nagyobb szervezetek számára az RBAC-szerepkörök alkalmazhatók a hello annak eldöntéséhez, hogy a hello perspektíva hello rendszergazda felhasználó Active Directory-csoportok ugyanúgy toogrant hello részletes hozzáférést igényel a csapatok vagy teljes, nem különállóan minden felhasználó részlegei, így annak eldöntéséhez, hogy ez rendkívül idő és a felügyeleti hatékony lehetőség. tooillustrate ezt a példát, hello **közreműködő** szerepkör hozzá lett adva a hello csoportok tooone hello bérlői hello előfizetés szintjén.





![az AAD-csoportokat RBAC-szerepkör hozzáadása](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Ezek a csoportok üzembe helyezve, és csak az Azure Active Directoryban felügyelni biztonsági csoportokat.

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-powershell"></a>Hozzon létre egyéni RBAC szerepkör tooopen támogatási kérelmek PowerShell használatával
hello beépített RBAC szerepkörök az Azure-ban rendelkezésre álló gondoskodjon arról, hogy bizonyos jogosultsági szintek hello a hello környezetben elérhető erőforrások alapján. Azonban ezek a szerepkörök egyike hello rendszergazdai jogú felhasználó saját igényeinek megfelelően, ha nincs hello beállítás toolimit hozzáférés még több egyéni RBAC-szerepkörök létrehozásával.

Egyéni RBAC-szerepkörök létrehozásához szükséges tootake egy beépített szerepkör, szerkesztheti, majd importálja vissza hello környezetben. hello letöltése és a feltöltés hello szerepkör kezelése a PowerShell vagy a parancssori felület.

Fontos toounderstand hello Előfeltételek létre egy egyéni biztonsági szerepkört is, hogy a támogatási kérelmek megnyitása hello meghívott felhasználó hello rugalmasságát és részletes hozzáférést hello előfizetés szintjén.

A példa hello beépített szerepkör **olvasó** amely lehetővé teszi a felhasználók hozzáférési tooview összes hello erőforrás hatóköröket azonban nem tooedit őket, vagy hozzon létre újakat lett testre szabott tooallow hello felhasználói hello lehetőség a támogatási kérelmek megnyitása.

első művelet hello exportáló hello **olvasó** emelt jogosultsági szintű rendszergazdaként futtatott szerepkör igények toobe PowerShell befejeződött.

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Az olvasó RBAC szerepkör PowerShell képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

Majd tooextract hello JSON-sablon hello szerepkör van szüksége.





![Egyéni olvasó RBAC szerepkör JSON-sablon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

Egy tipikus RBAC szerepkör áll kívül három fő szakasz **műveletek**, **NotActions** és **AssignableScopes**.

A hello **művelet** szakaszban felsorolt összes hello a szerepkörhöz tartozó engedélyezett műveletek. Minden egyes művelethez hozzárendelt erőforrás-szolgáltató fontos toounderstand. Ebben az esetben a támogatási jegyeket hello létrehozására vonatkozó **Microsoft.Support** szerepelnie kell az erőforrás-szolgáltató.

toobe képes toosee összes hello érhető el, és az előfizetéshez regisztrált erőforrás-szolgáltató, a PowerShell segítségével.
```
Get-AzureRMResourceProvider

```
Ezenkívül ellenőrizheti a hello összes hello elérhető PowerShell parancsmagok toomanage hello erőforrás-szolgáltató.
    ![Az erőforrás-szolgáltató management PowerShell képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)

egy adott RBAC-szerepkör erőforrás-szolgáltatók hello szakaszban felsorolt műveleteket hello összes toorestrict **NotActions**.
Utolsó akkor kötelező hello RBAC szerepkörhöz hello explicit előfizetési azonosítók felhasználási tartalmazza. hello előfizetési azonosítók a hello **AssignableScopes**, egyéb, nem engedélyezett tooimport hello szerepkör az előfizetésben.

Létrehozása és testreszabása hello RBAC szerepkör, után toobe importált hátsó hello környezet szükséges.

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

Ebben a példában a Szerepalapú szerepkör hello egyéni nevét "Olvasó támogatási jegyek hozzáférési szint" lehetővé tevő hello felhasználói tooview mindent hello előfizetés, és tooopen támogatási kérelmeket.

> [!NOTE]
> hello a támogatási kérelmek nyitó hello művelet engedélyezése csak két beépített RBAC szerepkörök vannak **tulajdonos** és **közreműködő**. Egy felhasználói toobe képes tooopen támogatási kérelmek ő szerepkört kell hozzárendelni egy Szerepalapú csak hatókörben hello előfizetés, mert minden támogatási kérelmek létrehozása az Azure-előfizetés alapján.

Az új egyéni szerepkör van hozzárendelve a hello tooan felhasználói ugyanabban a könyvtárban.





![képernyőfelvétel a hello Azure-portálon az importált egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![Képernyőkép a hozzárendelése egyéni importált RBAC szerepkör toouser hello az ugyanabban a könyvtárban](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![egyéni importált RBAC szerepkör engedélyeinek képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

hello példa lett további részletes tooemphasize hello korlátok egyéni RBAC-szerepkörhöz az alábbiak szerint:
* Új támogatási kérelmek hozhat létre.
* Nem hozható létre új erőforrás hatókörök (például: virtuális gép)
* Nem hozható létre új erőforrás-csoportok





![Képernyőkép a támogatási kérelmek létrehozása egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![Képernyőkép a Szerepalapú egyéni szerepkör nem tudja toocreate virtuális gépek](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![Képernyőkép a Szerepalapú egyéni szerepkör nem tudja toocreate új RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-azure-cli"></a>Hozzon létre egy egyéni RBAC szerepkör tooopen támogatási kérelmek Azure parancssori felület használatával
Mac számítógépen, és anélkül, hogy hozzáférési tooPowerShell fut, az Azure parancssori felület hello módon toogo.

hello lépéseket toocreate egy egyéni biztonsági szerepkört, hello egyedüli kivétel, hogy parancssori felület használatával hello szerepkör nem lehet letölteni egy JSON-sablon, de megtekinthetők az hello CLI hello.

Ehhez a példához I hello beépített szerepkör a kiválasztott **biztonsági mentés olvasó**.

```

azure role show "backup reader" --json

```





![Parancssori felület Képernyőkép a biztonsági mentési olvasó szerepkört megjelenítése](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

A Visual Studio hello szerepkör módosítása hello tulajdonságokat a JSON-sablon másolása után hello **Microsoft.Support** erőforrás-szolgáltató hozzá lett adva hello **műveletek** , hogy a felhasználó megnyithatja részek támogatási kérelmek toobe hello mentési tárolók olvasójának folytatása közben. Újra szükség tooadd hello előfizetés-azonosító amelyeken ezt a szerepkört kell használni a hello **AssignableScopes** szakasz.

```

azure role create --inputfile <path>

```





![Egyéni RBAC szerepkör importálása CLI képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

hello új szerepkör most hello Azure-portálon elérhető és hello hozzárendeléseket folyamat van hello ugyanaz, mint hello előző példák.





![Az Azure portál Képernyőkép a CLI 1.0 használatával létrehozott egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

Hello frissítésétől legújabb Build 2017, hello Azure Cloud rendszerhéj általánosan elérhető. Azure Cloud rendszerhéj egy a komplemens számnak tooIDE és hello Azure portálon. Ezzel a szolgáltatással hitelesítése és Azure-ban üzemeltetett egy webböngésző-alapú rendszerhéj kap, és használhatja a számítógépen telepített parancssori felület helyett.





![Azure-felhőbe rendszerhéj](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
