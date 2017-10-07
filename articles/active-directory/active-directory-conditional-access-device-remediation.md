---
title: "aaaYou nem érheti el a hello itt egy Windows-eszköz az Azure portálon |} Microsoft Docs"
description: "Ismerje meg, ahol nem get van helyről származik és mit ellenőrizheti, ezen a párbeszédpanelen rendszert futtató tooavoid."
services: active-directory
keywords: "eszközalapú feltételes hozzáférés, eszközregisztráció, eszközregisztráció engedélyezése, eszközregisztráció és MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: eda2aa10fbff5b3e515723219f61c1cb6f634e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a>Innen nem érheti el Windows-eszközről

Ha például a szervezet SharePoint Online intranetéhez próbál hozzáférni, megjelenhet egy oldal a következő üzenettel: *Innen nem érheti el*. Ez a lap azért jelent meg, mert a rendszergazda által megadott egy feltételes hozzáférési szabályzatot, amely megakadályozza, hogy bizonyos feltételek hozzáférés tooyour szervezet erőforrásaihoz. Bár szükséges toocontact ügyfélszolgálathoz vagy a rendszergazda tooget, ez a probléma megoldódott, dolgot néhány próbálhatja ki saját kezűleg, először.

Ha használ egy **Windows** eszközt, jelölje be a következő hello:

- Támogatott böngészőt használ?

- A Windows támogatott verzióját futtatja az eszközön?

- Az eszköz megfelelő?






## <a name="supported-browser"></a>Támogatott böngésző

Ha a rendszergazda feltételes hozzáférési szabályzatot állított be, akkor csak támogatott böngészővel férhet hozzá a szervezet erőforrásaihoz. Windows-eszközön csak az **Internet Explorer** és az **Edge** támogatott.

Egyszerűen azonosíthatja, hogy nem fér hozzá egy erőforrás miatt tooan nem támogatott böngészőt hello részletes adatait tartalmazó részben hello hibalap megtekintésével:

![„Innen nem érheti el” üzenetek nem támogatott böngészők esetén](./media/active-directory-conditional-access-device-remediation/02.png "Forgatókönyv")

hello csak szervizelési toouse hello alkalmazás támogatja-e az eszközplatformnál böngészőt. A támogatott böngészők teljes listáját a [támogatott böngészők](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies) című témakörben találja.  


## <a name="supported-versions-of-windows"></a>A Windows támogatott verziói

hello következő hello Windows operációs rendszer eszközére vonatkozó teljesülnie kell: 

- Windows asztali operációs rendszert használ az eszközt, hogy szükséges-toobe Windows 7 vagy újabb.
- Egy Windows server operációs rendszert használ az eszközt, hogy szükséges-e toobe Windows Server 2008 R2 vagy újabb. 


## <a name="compliant-device"></a>Megfelelő eszköz

Előfordulhat, hogy a rendszergazda engedélyezte egy feltételes hozzáférési szabályzatot, amely lehetővé teszi, hogy csak az előírásoknak megfelelő eszközök hozzáférési tooyour szervezet erőforrásaihoz. kompatibilis, az eszköznek meg kell mindkét illesztett tooyour toobe a helyszíni Active Directory vagy tooyour Azure Active Directory tartományhoz.

Könnyen azonosíthatja, hogy miatt tooa eszköz, amely nem kompatibilis hello részletes adatait tartalmazó részben hello hibalap megtekintésével erőforrás nem tud hozzáférni:
 
![„Innen nem érheti el” üzenetek nem regisztrált eszközök esetén](./media/active-directory-conditional-access-device-remediation/01.png "Forgatókönyv")


### <a name="is-your-device-joined-tooan-on-premises-active-directory"></a>A csatlakoztatott eszköz tooan a helyszíni Active Directory?

**Ha az eszköz csatlakoztatva van tooan a helyszíni Active Directory a szervezetében:**

1. Győződjön meg arról, hogy bejelentkezik tooWindows be munkahelyi fiókjával (az Active Directory-fiókot).
2. Csatlakozás tooyour a vállalati hálózathoz a DirectAccess vagy virtuális magánhálózati (VPN) keresztül.
3. Miután csatlakozott, nyomja le az hello Windows billentyű + hello L kulcs toolock a Windows-munkamenet.
4. Oldja fel a Windows-munkamenet zárolását a munkahelyi fiókjához tartozó hitelesítő adatok beírásával.
5. Várjon egy percet, és próbálja meg újból tooaccess hello alkalmazást vagy szolgáltatást.
6. Ha látja hello azonos lapján kattintson a hello **további részleteket** hivatkozásra, és forduljon a rendszergazdához hello adatokkal.


### <a name="is-your-device-not-joined-tooan-on-premises-active-directory"></a>Az eszköz nincs tartományhoz csatlakoztatva tooan a helyszíni Active Directory?

Ha az eszköz nincs tartományhoz csatlakoztatva tooan a helyszíni Active Directory és a Windows 10 rendszerű, két lehetőség közül választhat:

* Futtassa az Azure AD Joint
* Adja hozzá a munkahelyi vagy iskolai fiók tooWindows

További információt a két megoldás közötti különbségekről itt talál: [Windows 10-es eszközök használata a munkahelyen ](active-directory-azureadjoin-windows10-devices.md).  
Ha az eszköz:

- Tooyour szervezet tartozik, az Azure AD Join kell futtatnia.
- Személyes eszköz vagy Windows Phone-eszközön, adja hozzá a munkahelyi vagy iskolai fiók tooWindows kell 



#### <a name="azure-ad-join-on-windows-10"></a>Azure AD Join a Windows 10 rendszeren

hello lépéseket toojoin az eszköz tooAzure AD vannak társítva hello futnak a kiszolgálón Windows 10-es verzióját. a Windows 10 operációs rendszer hello toodetermine hello verziójának **winver** parancs: 

![Windows-verzió](./media/active-directory-conditional-access-device-remediation/03.png )


**Windows 10 évfordulós frissítés (1607-es verzió):**

1. Nyissa meg hello **beállítások** alkalmazást.
2. Kattintson a **Fiókok** > **Hozzáférés munkahelyi vagy iskolai rendszerhez** elemre.
3. Kattintson a **Connect** (Csatlakozás) gombra.
4. Kattintson a **az eszköz tooAzure AD Join**.
5. Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.
6. Jelentkezzen ki, majd jelentkezzen be a munkahelyi fiókjával.
7. Próbálja meg újra tooaccess hello alkalmazás.

**Windows 10, 2015 novemberi frissítés (1511-es verzió):**

1. Nyissa meg hello **beállítások** alkalmazást.
2. Kattintson a **Rendszer** > **Névjegy** elemre.
3. Kattintson a **Csatlakozás az Azure AD-hez** elemre.
4. Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.
5. Jelentkezzen ki, majd jelentkezzen be a munkahelyi fiókjával (Azure AD-fiókjával).
6. Próbálja meg újra tooaccess hello alkalmazás.


#### <a name="workplace-join-on-windows-81"></a>Munkahelyi csatlakoztatás Windows 8.1 rendszeren

Az eszköz nincs tartományhoz és a munkahelyi csatlakoztatás Windows 8.1, toodo fut, és a Microsoft Intune-beli regisztrálásakor, hello a következő lépéseket:

1. Nyissa meg a **Gépházat**.
2. Kattintson a **Hálózat** > **Munkahely** elemre.
3. Kattintson a **Csatlakozás** parancsra.
4. Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.
5. Kattintson a **Bekapcsolás** elemre.
6. Próbálja meg újra tooaccess hello alkalmazás.



#### <a name="add-your-work-or-school-account-toowindows"></a>Adja hozzá a munkahelyi vagy iskolai fiók tooWindows 


**Windows 10 évfordulós frissítés (1607-es verzió):**

1. Nyissa meg hello **beállítások** alkalmazást.
2. Kattintson a **Fiókok** > **Hozzáférés munkahelyi vagy iskolai rendszerhez** elemre.
3. Kattintson a **Connect** (Csatlakozás) gombra.
4. Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.
5. Próbálja meg újra tooaccess hello alkalmazás.


**Windows 10, 2015 novemberi frissítés (1511-es verzió):**

1. Nyissa meg hello **beállítások** alkalmazást.
2. Kattintson a **Fiókok** > **Saját fiókok** elemre.
3. Kattintson a **Munkahelyi vagy iskolai fiók beállítása** elemre.
4. Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.
5. Próbálja meg újra tooaccess hello alkalmazás.





## <a name="next-steps"></a>Következő lépések
[Azure Active Directory feltételes hozzáférés](active-directory-conditional-access.md)

