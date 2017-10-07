---
title: "aaaAzure Active Directory eszköz felügyeleti kapcsolatos gyakori kérdések |} Microsoft Docs"
description: "Az Azure Active Directory kezelése – gyakori kérdések."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 000eb6a930187e13cb24cf628793afd06813be23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory eszköz felügyeleti kapcsolatos gyakori kérdések

**K: I hello eszköz nemrég regisztrálva. Miért nem látom hello eszköz hello Azure-portálon a saját felhasználói adatok alapján?**

**V:** , amelyek automatikus eszközregisztráció tartományhoz csatlakoztatott Windows 10-eszközök nem jelennek meg hello felhasználói adatok alapján.
Minden eszköz kell toouse PowerShell toosee. 

Csak hello következő eszköz szerepel-e hello felhasználói adatok alapján:

- Az összes személyes eszközök, amelyek nem csatlakoztatott vállalati 
- Az összes nem - Windows 10 vagy Windows Server 2016 
- Minden-Windows eszköz 

---

**K: Miért nem látom minden hello eszköz hello Azure-portálon az Azure Active Directoryban regisztrálva?** 

**V:** jelenleg nincs módja toosee minden regisztrált eszközöket a hello Azure-portálon. Azure PowerShell toofind minden eszköz használható. További részletekért lásd: hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) parancsmag.

--- 

**K: Hogyan állapítható meg, milyen hello eszköz regisztrációs állapotát hello ügyfél van?**

**V:** hello eszköz regisztrációs állapotától függ:

- Milyen hello eszköz
- Hogyan regisztrálták 
- Minden adatát tooit kapcsolatos. 
 

---

**K: Miért történik szeretnék, hogy törölték a hello Azure portál vagy a Windows PowerShell használatával továbbra is szerepel a regisztrált?**

**V:** Ez az elvárt működés. hello eszköz nem fog hozzáférést tooresources hello felhőben. Ha szeretné, hogy tooremove hello eszközt, és regisztrálja újra, a manuális műveletet végzett hello eszköz toobe kell lennie. 

A Windows 10 és Windows Server 2016-os, amely a helyszíni AD-tartományhoz:

1.  Nyissa meg a hello parancssort rendszergazdaként.

2.  Típusa`dsregcmd.exe /debug /leave`

3.  Jelentkezzen ki, és jelentkezzen be tootrigger hello ütemezett feladatot, amely regisztrálja újra a hello eszközt. 

Az egyéb Windows platformra, amelyek a helyszíni AD-tartományhoz:

1.  Nyissa meg a hello parancssort rendszergazdaként.
2.  Gépelje be: `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Gépelje be: `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**Kérdés: Miért látom azt ismétlődő eszközbevitelek Azure-portálon?**

**V:**

-   A Windows 10 és Windows Server 2016, ha az ismételt kísérletek toounjoin és újracsatlakozhatnak hello ugyanarra az eszközre, előfordulhat ismétlődő bejegyzések. 

-   Ha használta, adja hozzá a munkahelyi vagy iskolai fiókkal, minden windows-felhasználó hozzáadása munkahelyi vagy iskolai fiókhoz használó fog hozzon létre egy új eszköz rekordot hello ugyanazon eszköznév.

-   Egyéb Windows-platformokat, amelyek a helyszíni AD tartományhoz az automatikus regisztráció használatával hoz létre egy új eszközrekordhoz hello minden tartományi felhasználó bejelentkezik az hello eszköz azonos eszköznevet. 

-   Az, hogy törölték, AADJ gép újratelepítése, és újra csatlakoztatni a hello azonos nevet, egy másik rekord, hello állapotúként jelenik meg ugyanazon eszköznév.

---

**K: Miért egy felhasználó erőforrásaihoz is hozzáférjenek az eszközről szeretnék letiltotta a hello Azure-portálon?**

**V:** be egy alkalmazott a visszavonási toobe tooan órát is igénybe vehet.

>[!Note] 
>Az elveszett eszközök ajánlott hello eszköz tooensure, hogy a felhasználók nem férhetnek hozzá a hello eszköz törlése. További részletekért lásd: [eszközök regisztrálása az Intune-ban felügyeletre](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**K: Miért látnak a felhasználók "Nem érheti el innen"?**

**V:** konfigurált egyes feltételes hozzáférési szabályok toorequire egy adott eszköz állapotát, és hello eszköz nem felel meg a hello feltételek, ha a felhasználók le vannak tiltva, és ez az üzenet megjelenik. Értékelje ki a hello szabályok, és győződjön meg arról, hogy hello eszköz képes toomeet hello feltételek tooavoid van ezt az üzenetet.

---


**K: I lásd: hello rekordokat az Azure-portálon hello hello felhasználói adatok alapján, és hello ügyfélen regisztrált hello állapota látható. A feltételes hozzáférés használatának megfelelően beállítani a perceké 'M?**

**V:** hello eszközrekordhoz (deviceID) és az állapotot a hello Azure-portálon kell hello ügyfél felel meg, és bármely értékelési feltételeknek a feltételes hozzáférés. További részletekért lásd: [Ismerkedés az Azure Active Directory Eszközregisztrációs](active-directory-device-registration.md).

---

**K: Miért kapok "felhasználónév vagy jelszó nem megfelelő" üzenet egy eszközhöz csak csatlakozott, I tooAzure AD?**

**V:** Ez a forgatókönyv gyakori okai a következők:

- A felhasználói hitelesítő adatok nem érvényesek.

- A számítógép nem toocommunicate az Azure Active Directoryval. Ellenőrizze, hogy minden hálózati kapcsolati problémáit.

- hello Azure AD Join előfeltételek nem teljesültek. Győződjön meg arról, hogy elvégezte-e hello lépései [kiterjesztése a felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási](active-directory-azureadjoin-overview.md).  

- Összevont bejelentkezések igényel az összevonási kiszolgáló toosupport egy WS-Trust aktív végpontot. 

---

**K: Miért látom azt hello "Oops... hiba!" párbeszédpanel jelenik meg a számítógép csatlakozni?**

**V:** Ez az Azure Active Directory-regisztráció az Intune-nal beállításának eredménye. További részletekért lásd: [Windows-eszközök kezelésének beállítása](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**K: Miért a kísérlet toojoin egy PC sikertelen lesz, bár kaptam bármely hibainformációk?**

**V:** ennek valószínű oka az, hogy hello felhasználó van bejelentkezve toohello eszköz hello beépített rendszergazdai fiók használatával. Hozzon létre egy másik helyi fiók használata az Azure Active Directory csatlakozási toocomplete hello beállítása előtt. 

---

**K: hol található hello beállítása automatikus eszközregisztráció utasításait?**

**V:** részletes útmutatásért lásd: [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**K: hol található a hibaelhárítási információkat hello automatikus eszközregisztráció?**

**V:** a hibaelhárítással kapcsolatos információkért, lásd:

- [Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md)

- [Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD Windows régebbi ügyfelek](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

