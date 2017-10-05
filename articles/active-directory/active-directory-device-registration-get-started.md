---
title: "Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása konfigurálása az Azure Active Directoryval |} Microsoft Docs"
description: "A tartományhoz csatlakoztatott Windows-eszközök beállítása automatikusan és a csendes regisztrálása az Azure Active Directoryban."
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
ms.openlocfilehash: 38750050e8525272079e1f3a5509da1e8f0a557b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Ismerkedés az Azure Active Directory eszközregisztrációjával

Az Azure Active Directory eszközregisztráció az eszközalapú feltételes hozzáférési forgatókönyvek alapja. Amikor regisztrál egy eszközt, az Azure Active Directory eszközregisztráció egy identitással látja el az eszközt, amely az eszköz hitelesítésére használható a felhasználó bejelentkezésekor. A hitelesített eszköz és az eszköz attribútumai ezután a feltételes hozzáférési házirendek betartatásához használhatók a felhőben és a helyszínen tárolt alkalmazások esetében.

Amikor mobileszköz-kezelési (MDM) megoldással, például a Microsoft Intune-nal ötvözi, frissülnek az Azure Active Directoryban lévő eszközattribútumok az eszköz további információival. Ez lehetővé teszi további feltételes hozzáférési szabályok létrehozását, amelyek arra kényszerítik az eszközhozzáféréseket, hogy megfeleljenek a biztonsági és megfelelőségi szabványoknak. A Microsoft Intune-beli regisztrálásának eszközökön további információkért lásd: eszközök regisztrálása felügyeletre a Intune-ban.
Azure Active Directory eszköz regisztrációs Azure Active Directory Eszközregisztráció által engedélyezett forgatókönyvek támogatja az iOS, Android és Windows eszközökhöz. Az Azure AD eszközregisztrációt használó egyes forgatókönyvek konkrétabb követelményekkel és platformtámogatással rendelkezhetnek. 

Ezek a forgatókönyvek a következők:

- **Feltételes hozzáférés az Office 365-alkalmazások Microsoft Intune-nal:** IT-rendszergazdák hozhat létre feltételes hozzáférési szabályzatok, lehetővé téve az információkkal dolgozó szakemberek a szolgáltatásokat a feltételeknek megfelelő eszközökön a vállalati erőforrások biztonságossá tételére. További információ: Feltételes hozzáférés eszközházirendjei Office 365-szolgáltatásokhoz.

- **Feltételes hozzáférés az alkalmazásokhoz a helyszínen szolgáltatott:** használhat regisztrált eszközöket hozzáférési házirendekkel alkalmazásokat, amelyek a Windows Server 2012 R2 AD FS használatára van konfigurálva. A helyszíni feltételes hozzáférés beállításáról további információért lásd: [Helyszíni feltételes hozzáférés beállítása az Azure Active Directory eszközregisztrációjával](active-directory-device-registration-on-premises-setup.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Az Azure Active Directory eszközregisztráció beállítása

Eszközregisztráció beállítása, hogy több lehetőség közül választhat:

- Eszközöket regisztrálhatja, amikor az Azure AD-tartományhoz csatlakozik. A témakörrel kapcsolatban bővebben részletesebb Azure AD Join és a szükséges felhasználói beállításokat az Azure AD-tartományhoz való csatlakozáshoz.

- Ha a felhasználók fel a munkahelyi vagy iskolai fiókok Windows személyes eszköz esetén a mobileszközök csatlakozni a munkahelyi erőforrásokhoz regisztrációs igénylő eszközöket regisztrálni lehet. Győződjön meg arról, ez, engedélyeznie kell az Azure AD Eszközregisztrációját az Azure portálon. 

- Eszközök automatikus regisztrálásának használatával hagyományos tartományhoz csatlakozó gépek regisztrálható. Ennek biztosítására kell első beállítása az Azure AD Connectet automatikus eszközregisztráció folytatása előtt.

Legújabb útmutatásért lásd: [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md).  
Is tekintse át és engedélyezheti vagy letilthatja az Azure Active Directory felügyeleti portálján regisztrált eszközöket.

## <a name="enable-the-azure-active-directory-device-registration-service"></a>Az Azure Active Directory eszközregisztrációs szolgáltatás engedélyezése

**Az Azure Active Directory eszközregisztrációs szolgáltatás engedélyezése:**

1.  Jelentkezzen be a Microsoft Azure portálra rendszergazdaként.

2.  A bal oldali panelen válassza az **Active Directory** elemet.

3.  A könyvtár lapon válassza ki a címtárát.

4.  Kattintson a **Configure** (Konfigurálás) elemre.

5.  Görgessen **eszközök**.

6.  A felhasználók számára az összes lehetséges, hogy REGISZTRÁLJÁK az ESZKÖZEIKET az AZURE ad-val.

7.  Válassza ki a felhasználónként regisztrálni kívánt eszközök maximális számát.

Az Office 365 a Microsoft Intune-vagy mobileszköz-kezelés regisztrációs eszközregisztráció szükséges. Ha konfigurálta a szolgáltatások valamelyikét **összes** van kiválasztva, és **NONE** le van tiltva. Győződjön meg arról, hogy azok helyesen vannak konfigurálva, és rendelkezik a megfelelő licencelés.

Alapértelmezés szerint a kéttényezős hitelesítés nem engedélyezett a szolgáltatáshoz. De a kéttényezős hitelesítés kiválasztása javasolt az eszközök regisztrálásakor.

- Mielőtt kéttényezős hitelesítést igénylő ezt a szolgáltatást, kell konfigurálni egy kéttényezős hitelesítésszolgáltatót az Azure Active Directoryban és a felhasználói fiókok konfigurálása a multi-factor Authentication. további tekintse meg a multi-factor Authentication felvétele az Azure Active Directoryhoz

- Ha a Windows Server 2012 R2 AD FS használ, akkor egy kéttényezős hitelesítési modult az AD FS konfigurálása, tekintse meg a multi-factor Authentication használata az Active Directory összevonási szolgáltatások.

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Eszközobjektumok megtekintése és kezelése az Azure Active Directoryban

Az Azure felügyeleti portálon megtekintheti, blokkolhatja és feloldhatja az eszközök blokkolását. A blokkolt eszközök már nem érhetik el azon alkalmazásokat, amelyek csak regisztrált eszközök engedélyezésére vannak konfigurálva.

**Megtekintheti és kezelheti az eszközt az Azure Active Directory-objektumokat:**
 
1.  Jelentkezzen be a Microsoft Azure portálra rendszergazdaként.

2.  A bal oldali panelen válassza az **Active Directory** elemet.

3.  Válassza ki a címtárát.

4.  Válassza ki **felhasználók**. 

5.  Kattintson arra a felhasználóra, amelynek meg szeretné tekinteni az eszközöket.

6.  Válassza ki **eszközök**.

7.  Válassza ki **regisztrált eszközök**.

Most tekintse át, letiltása, vagy a felhasználó regisztrált eszközök feloldása.
A helyszínen a tartományhoz, és automatikusan regisztrált Windows 10-eszközök nem jelennek meg a felhasználók lapon. A Get-MsolDevice PowerShell-parancs segítségével a vállalati eszközök keresése. 


## <a name="next-steps"></a>Következő lépések

Automatikus eszközregisztráció beállítása, lásd: [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md).


