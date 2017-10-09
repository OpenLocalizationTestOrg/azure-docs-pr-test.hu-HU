---
title: "aaaHow tooconfigure automatikus regisztrálása az Azure Active Directoryval Windows tartományhoz csatlakozó eszközök |} Microsoft Docs"
description: "Állítsa be a tartományhoz csatlakoztatott Windows-eszközök tooregister automatikusan és értesítések nélkül történik az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1d736eba734418231f12e23a8fc1a93405f129c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Ismerkedés az Azure Active Directory eszközregisztrációjával

Azure Active Directory eszközregisztráció az eszközalapú feltételes hozzáférési forgatókönyvek alapja hello. Amikor regisztrál egy eszközt, az Azure Active Directory eszközregisztrációs biztosít hello eszköz történik használt tooauthenticate hello hello felhasználó bejelentkezésekor identitással. hello hitelesített eszköz és hello eszköz attribútumai – hello, majd lehet használt tooenforce feltételes hozzáférési házirendek hello felhő és a helyszínen tárolt alkalmazások esetében.

Például a Microsoft Intune mobileszköz-lévő eszközattribútumok megoldás együtt, hello eszközattribútumokon az Azure Active Directoryban frissítődik hello eszközzel kapcsolatos további információk. Ez lehetővé teszi toocreate feltételes hozzáférési szabályok, amelyeket eszközök toomeet való hozzáférést a biztonsági és megfelelőségi szabványoknak. A Microsoft Intune-beli regisztrálásának eszközökön további információkért lásd: eszközök regisztrálása felügyeletre a Intune-ban.
Azure Active Directory eszköz regisztrációs Azure Active Directory Eszközregisztráció által engedélyezett forgatókönyvek támogatja az iOS, Android és Windows eszközökhöz. hello Azure AD Eszközregisztrációt használó egyes forgatókönyvek konkrétabb követelményekkel és platformtámogatással rendelkezhetnek. 

Ezek a forgatókönyvek a következők:

- **Feltételes hozzáférés az Office 365-alkalmazások Microsoft Intune-nal:** IT-rendszergazdák építhető feltételes hozzáférési házirendek toosecure vállalati eszközerőforrások, amíg a hello azonos idő engedélyezése az információkkal dolgozó szakemberek a feltételeknek megfelelő eszközökön tooaccess hello szolgáltatások. További információ: Feltételes hozzáférés eszközházirendjei Office 365-szolgáltatásokhoz.

- **Feltételes hozzáférés tooapplications, amelyek helyben tárolt:** használhatja a regisztrált eszközök hozzáférési házirendekkel kapcsolatos alkalmazások, amelyek toouse AD FS a Windows Server 2012 R2. A helyszíni feltételes hozzáférés beállításáról további információért lásd: [Helyszíni feltételes hozzáférés beállítása az Azure Active Directory eszközregisztrációjával](active-directory-device-registration-on-premises-setup.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Az Azure Active Directory eszközregisztráció beállítása

toosetup eszközregisztráció, több lehetősége van:

- Eszközöket regisztrálhatja, mikor illesztett tooAzure AD-tartományhoz. A témakörrel kapcsolatban bővebben részletesebb toojoin az Azure AD tartományi felhasználók számára szükséges az Azure AD Join és hello beállításról.

- Eszközök regisztrálható, amikor a felhasználók hozzáadása a munkahelyi vagy iskolai fiókok tooWindows személyes eszköz, vagy ha a mobil eszközök csatlakoznak a regisztrációs igénylő tooa munkahelyi erőforrásokhoz. tooensure, engedélyeznie kell az Azure AD Eszközregisztrációját hello Azure portálon. 

- Eszközök automatikus regisztrálásának használatával hagyományos tartományhoz csatlakozó gépek regisztrálható. tooensure, először a telepítő az Azure AD Connectet kell az automatikus eszközregisztráció folytatása előtt.

Legújabb útmutatásért lásd: [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-conditional-access-automatic-device-registration-setup.md).  
Is tekintse át és engedélyezheti vagy letilthatja az Azure Active Directory felügyeleti portálon hello regisztrált eszközöket.

## <a name="enable-hello-azure-active-directory-device-registration-service"></a>Hello Azure Active Directory eszközregisztrációs szolgáltatás engedélyezése

**tooenable hello Azure Active Directory eszközregisztrációs szolgáltatás:**

1.  Jelentkezzen be toohello Microsoft Azure portálra rendszergazdaként.

2.  Hello bal oldali ablaktáblában jelölje ki **Active Directory**.

3.  Hello könyvtár lapján válassza ki a címtárát.

4.  Kattintson a **Configure** (Konfigurálás) elemre.

5.  Görgessen túl**eszközök**.

6.  A felhasználók számára az összes lehetséges, hogy REGISZTRÁLJÁK az ESZKÖZEIKET az AZURE ad-val.

7.  Válassza ki hello maximális számát, az eszközök felhasználónként tooauthorize szeretné.

az Office 365 a Microsoft Intune- vagy mobileszköz-kezelés hello beléptetési eszközregisztráció szükséges. Ha konfigurálta a szolgáltatások valamelyikét **összes** van kiválasztva, és **NONE** le van tiltva. Győződjön meg arról, hogy azok helyesen vannak konfigurálva, és rendelkezik a megfelelő licencelési hello.

Alapértelmezés szerint a kéttényezős hitelesítés hello szolgáltatás nincs engedélyezve. De a kéttényezős hitelesítés kiválasztása javasolt az eszközök regisztrálásakor.

- Mielőtt kéttényezős hitelesítést igénylő ezt a szolgáltatást, egy kéttényezős hitelesítési szolgáltató konfigurálása az Azure Active Directoryban és el kell a felhasználói fiókok konfigurálása a multi-factor Authentication, további információ: a multi-factor Authentication felvétele az Active Directory tooAzure

- Ha a Windows Server 2012 R2 AD FS használ, akkor egy kéttényezős hitelesítési modult az AD FS konfigurálása, tekintse meg a multi-factor Authentication használata az Active Directory összevonási szolgáltatások.

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Eszközobjektumok megtekintése és kezelése az Azure Active Directoryban

Hello Azure felügyeleti portálról megtekintése, letilthatók, és feloldhatja az eszközök blokkolását. Blokkolt eszközök már nem fog rendelkezni, amelyek csak regisztrált eszközök konfigurált tooallow hozzáférés tooapplications.

**tooview és eszközobjektumot az Azure Active Directoryban kezelése:**
 
1.  Jelentkezzen be toohello Microsoft Azure portálra rendszergazdaként.

2.  Hello bal oldali ablaktáblában jelölje ki **Active Directory**.

3.  Válassza ki a címtárát.

4.  Válassza ki **felhasználók**. 

5.  Kattintson a kívánt toosee hello eszközök hello felhasználó.

6.  Válassza ki **eszközök**.

7.  Válassza ki **regisztrált eszközök**.

Most tekintse át, letiltása, vagy feloldása hello felhasználó regisztrált eszközöket.
A helyszínen a tartományhoz, és automatikusan regisztrált Windows 10-eszközök hello felhasználók lapján nem jelennek meg. Használja a Get-MsolDevice PowerShell-parancs toofind hello minden vállalati eszköz. 


## <a name="next-steps"></a>Következő lépések

toosetup automatikus eszközregisztráció, lásd: [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-conditional-access-automatic-device-registration-setup.md).


