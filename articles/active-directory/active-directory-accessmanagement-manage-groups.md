---
title: az Azure Active Directoryban aaaManaging csoportok |} Microsoft Docs
description: "Hogyan toocreate és csoportok toomanage Azure kezelése Azure Active Directory használatával felhasználókat."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 9bee224655639740b3dd99983892b30c3c537aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-groups-in-azure-active-directory"></a>Csoportkezelés az Azure Active Directoryban
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [klasszikus Azure portál](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Hello szolgáltatások az Azure Active Directory (Azure AD) felhasználó felügyeleti egyik hello képességét toocreate felhasználók csoportját. Egy csoport tooperform felügyeleti feladatokhoz, mint az hozzárendelése licenccel, illetve engedélyeket felhasználók száma tooa egyszerre használja. Csoportok tooassign engedéllyel is használható

* Erőforrások, például a hello directory objektumok
* Erőforrások külső toohello címtár, például az SaaS-alkalmazásokhoz, Azure-szolgáltatásokkal, SharePoint-webhelyek vagy a helyszíni erőforrások

Ezenkívül erőforrás tulajdonosa is hozzárendelhetők hozzáférés tooa erőforrás tooan az Azure AD-csoport valaki más a tulajdonosa. Ez a hozzárendelés tagjai hello csoport hozzáférés toohello erőforrás. Ezt követően hello csoport hello tulajdonosa kezeli hello csoport tagjának kell lennie. Gyakorlatilag hello erőforrás tulajdonosa delegáltak toohello hello csoport hello engedély tooassign felhasználók tootheir erőforrás tulajdonosa.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan toomanage hello Azure AD felügyeleti központban csoportosítja, lásd: [hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](active-directory-groups-create-azure-portal.md).

## <a name="how-do-i-create-a-group"></a>Hogyan hozható létre csoport?
Attól függően, hogy hello szolgáltatások toowhich a szervezet kér le létrehozhat egy csoportot a következő hello:

* hello a klasszikus Azure portálon
* hello Office 365 fiókportálon
* hello Windows Intune-fiókportál

Feladatok, a klasszikus Azure portálon hello végre ismerteti. Az Azure AD-címtár nem Azure portálon toomanage használatáról további információk: [az Azure AD-címtár felügyelete](active-directory-administer.md).

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.
2. Jelölje be hello **csoportok** fülre.
3. Válassza ki a **Csoport hozzáadása** lehetőséget.
4. A hello **csoport hozzáadása** ablakban hello nevét adja meg, és a csoport leírása hello.

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Hogyan lehet adott felhasználókat felvenni egy biztonsági csoportba vagy eltávolítani onnan?
**egy adott felhasználó tooa csoport tooadd**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.
2. Jelölje be hello **csoportok** fülre.
3. Nyissa meg a kívánt tooadd tagok hello csoport toowhich. Nyissa meg hello **tagok** hello lapján csoport ki, ha azt már nem megjelenítése.
4. Válassza ki a **Tagok hozzáadása** lehetőséget.
5. A hello **tagok hozzáadása** lap, hello felhasználó vagy egy csoport, amelyet az tooadd, ez a csoport tagjaként válassza hello nevét. Győződjön meg arról, hogy ez a név szerepel-e toohello **kijelölt** ablaktáblán.

**tooremove egy felhasználót egy csoportból**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.
2. Jelölje be hello **csoportok** fülre.
3. Nyissa meg a hello csoportot, amelyből el kívánja tooremove tagokat.
4. Jelölje be hello **tagok** lap, válassza hello neve hello tag szeretné, hogy tooremove ebből a csoportból, és kattintson **eltávolítása**.
5. Erősítse meg hello parancssorba tooremove a hello csoport tagja.

## <a name="how-can-i-manage-hello-membership-of-a-group-dynamically"></a>Hogyan kezelhetők a csoportok tagsága hello dinamikusan?
Az Azure AD-nagyon egyszerűen állíthat be egy egyszerű szabályt toodetermine, mely felhasználók toobe hello csoport tagjai. Az egyszerű szabály egy olyan szabály, amely csak egyszeres összehasonlítást végez. Például ha egy csoport tooa SaaS-alkalmazáshoz van hozzárendelve, állíthatja be a szabály tooadd "Értékesítési Rep." beosztással rendelkező felhasználók Ez a szabály majd hozzáférést toothis SaaS alkalmazás tooall felhasználóinak, hogy a címtárban beosztás.

Amikor egy felhasználó módosítása semmilyen attribútumot hello rendszer kiértékeli egy könyvtár toosee hello attribútum módosítása hello felhasználó bármely csoport kiváltották, ha az összes dinamikus csoport szabály hozzáadása vagy eltávolítása. Ha egy felhasználói csoporton szabály megfelel, hozzáadásuk után tag toothat csoportként. Azok a csoport tagjai hello szabály már nem felel meg, ha eltávolítja a tagjai a csoportból.

> [!NOTE]
> Biztonsági vagy Office 365-csoportok esetében dinamikustagság-szabály beállítására is lehetőség van. A beágyazott csoporttagság biztonságicsoport-alapú hozzárendelés tooapplications jelenleg nem támogatott.
>
> Dinamikus csoporttagság az Azure AD Premium licenc toobe rendelt
>
> * hello felügyelő rendszergazdához; csoporton hello szabály
> * Hello csoport minden tagja
>
>

**tooenable dinamikus csoporttagság**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.
2. Jelölje be hello **csoportok** fülre, és azt szeretné, hogy tooedit nyitott hello csoport.
3. Jelölje be hello **konfigurálása** lapot, és utána állítsa be **dinamikus csoporttagságok engedélyezése** túl**Igen**.
4. Állítson be egy egyszerű szabályt hello csoport toocontrol dinamikus csoporttagság működését. Győződjön meg arról, hogy hello **felhasználók hozzáadása az adott** beállítás van kiválasztva, majd válassza ki egy felhasználói tulajdonságot (például részleg, beosztás, stb.), hello listából
5. Ezt követően válasszon ki egy állapotot (nem egyenlő, egyenlő, nem ezzel kezdődik, ezzel kezdődik, nem tartalmazza, tartalmazza, nem egyezik, megegyezik).
6. Adja meg az összehasonlítási értéket hello kiválasztott felhasználói tulajdonságnak.

arról, hogyan toolearn toocreate *speciális* (szabályok többszörös összehasonlítást is tartalmazó) szabályok dinamikus csoporttagság, lásd: [toocreate attribútumok használata speciális szabályok](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>További információ
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
