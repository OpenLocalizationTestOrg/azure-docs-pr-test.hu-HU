---
title: "aaaAzure Active Directory eszközregisztrációs áttekintése |} Microsoft Docs"
description: "Azure Active Directory eszközregisztráció az eszközalapú feltételes hozzáférési forgatókönyvek alapja hello. Amikor regisztrál egy eszközt, az Azure Active Directory eszköz regisztrációs rendelkezések hello eszköz egy identitás, amely használt tooauthenticate hello eszköz hello felhasználó bejelentkezésekor."
services: active-directory
keywords: "eszközregisztráció, eszközregisztráció engedélyezése, eszközregisztráció és MDM"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 1e92c1a2-01b8-4225-950b-373cd601b035
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: b9846e34fe873d271ebe71cbf8e70c6b4768885b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Ismerkedés az Azure Active Directory eszközregisztrációjával
Azure Active Directory eszközregisztráció az eszközalapú feltételes hozzáférési forgatókönyvek alapja hello. Amikor regisztrál egy eszközt, az Azure Active Directory eszközregisztrációs biztosít hello eszköz történik használt tooauthenticate hello hello felhasználó bejelentkezésekor identitással. hello hitelesített eszköz és hello eszköz attribútumai – hello, majd lehet használt tooenforce feltételes hozzáférési házirendek hello felhő és a helyszínen tárolt alkalmazások esetében.

Például a Microsoft Intune mobileszköz-lévő eszközattribútumok megoldás együtt, hello eszközattribútumokon az Azure Active Directoryban frissítődik hello eszközzel kapcsolatos további információk. Ez lehetővé teszi toocreate feltételes hozzáférési szabályok, amelyeket eszközök toomeet való hozzáférést a biztonsági és megfelelőségi szabványoknak. További információk az eszközöknek a Microsoft Intune-ban történő regisztrálásával kapcsolatban: [Eszközök regisztrálása felügyeletre a Microsoft Intune-ban](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Az Azure Active Directory eszközregisztráció által engedélyezett forgatókönyvek
Az Azure Active Directory eszközregisztráció részét képezi az iOS-, Android- és Windows Phone-eszközök támogatása. hello Azure AD eszközregisztrációt használó egyes forgatókönyvek konkrétabb követelményekkel és platformtámogatással rendelkezhetnek. Ezek a forgatókönyvek a következők:

* **Feltételes hozzáférés tooapplications, amelyek helyben tárolt**: használhatja a regisztrált eszközök hozzáférési házirendekkel kapcsolatos alkalmazások, amelyek toouse AD FS a Windows Server 2012 R2. A helyszíni feltételes hozzáférés beállításáról további információért lásd: [Helyszíni feltételes hozzáférés beállítása az Azure Active Directory eszközregisztrációjával](active-directory-device-registration-on-premises-setup.md).
* **Feltételes hozzáférés az Office 365-alkalmazások Microsoft Intune-nal** : IT-rendszergazdák építhető feltételes hozzáférési házirendek toosecure vállalati eszközerőforrások, amíg a hello azonos idő engedélyezése az információkkal dolgozó szakemberek a feltételeknek megfelelő eszközökön tooaccess hello szolgáltatások. További információ: [Feltételes hozzáférés eszközházirendjei Office 365-szolgáltatásokhoz](active-directory-conditional-access-device-policies.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Az Azure Active Directory eszközregisztráció beállítása
Tooenable az Azure AD eszközregisztrációját hello Azure portálon kell, hogy a mobileszközök felderíthessék hello szolgáltatást a jól ismert DNS-rekordok megkeresésével. Konfigurálnia kell a vállalat DNS-ÉT, hogy a Windows 10, Windows 8.1, Windows 7, Android és IOS rendszerű eszközök felderíthessék és használhassák a hello szolgáltatást.
Megtekintheti és engedélyezését vagy letiltását az Azure Active Directory felügyeleti portálon hello regisztrált eszközöket.

> [!NOTE]
> Hogyan tooset be automatikus eszközregisztráció megtekintéséhez legújabb útmutatást [hogyan toosetup az automatikus regisztráció Windows-tartománynak csatlakoztatott eszközök és az Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
> 
> 

### <a name="enable-azure-active-directory-device-registration-service"></a>Az Azure Active Directory eszközregisztrációs szolgáltatás engedélyezése
1. Jelentkezzen be toohello Microsoft Azure portálra rendszergazdaként.
2. Hello bal oldali ablaktáblában jelölje ki **Active Directory**.
3. A hello **Directory** lapra, válassza ki azt a címtárat.
4. Jelölje be hello **konfigurálása** fülre.
5. Görgessen toohello terület **eszközök**.
6. Válassza a **MIND** lehetőséget **A FELHASZNÁLÓK CSATLAKOZTATHATJÁK A MUNKAHELYEN AZ ESZKÖZEIKET** alatt.
7. Válassza ki hello maximális számát, az eszközök felhasználónként tooauthorize szeretné.

> [!NOTE]
> A Microsoft Intune vagy az Office 365 mobileszköz-felügyelet használatával történő beléptetéshez munkahelyi csatlakoztatásra van szükség. Ha a szolgáltatások valamelyikét konfigurálta, mind lesz kiválasztva, és hello egyik sem gomb le van tiltva.
> 
> 

Alapértelmezés szerint a kéttényezős hitelesítés hello szolgáltatás nincs engedélyezve. De a kéttényezős hitelesítés kiválasztása javasolt az eszközök regisztrálásakor.

* Mielőtt kéttényezős hitelesítést igénylő ezt a szolgáltatást, egy kéttényezős hitelesítési szolgáltató konfigurálása az Azure Active Directoryban, és el kell a felhasználói fiókok konfigurálása a multi-factor Authentication, további információ: [multi-factor Authentication hozzáadása Az Active Directory hitelesítési tooAzure](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Ha az AD FS-t a Windows Server 2012 R2 rendszerrel használja, konfigurálnia kell egy kéttényezős hitelesítési modult az AD FS-ben. Lásd: [Multi-Factor Authentication használata az Active Directory összevonási szolgáltatásokkal](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Az Azure Active Directory eszközregisztráció felderítésének konfigurálása
Windows 7 és Windows 8.1-eszközök hello felhasználói fiók nevét egy jól ismert eszközregisztrációs kiszolgáló név kombinálásával hello eszközregisztrációs szolgáltatást deríti fel.

Létre kell hoznia egy DNS CNAME-rekordot, amely az Azure Active Directory eszközregisztrációs szolgáltatással társított toohello A rekord mutat. hello CNAME rekordot kell használnia a hello jól ismert enterpriseregistration előtagot hello hello felhasználói fiókokat a szervezete által használt UPN-utótag követ. Ha a szervezet több UPN-utótagot használ, több CNAME-rekordot kell létrehozni a DNS-ben.

Ha például két UPN-utótagot használ a szervezetben @contoso.com és @region.contoso.com, a következő DNS-rekordok hello hoz létre.

| Bejegyzés | Típus | Cím |
| --- | --- | --- |
| enterpriseregistration.contoso.com |CNAME |enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com |CNAME |enterpriseregistration.windows.net |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Eszközobjektumok megtekintése és kezelése az Azure Active Directoryban
1. Hello Azure felügyeleti portálról megtekintése, letilthatók, és feloldhatja az eszközök blokkolását. Blokkolt eszközök már nem fog rendelkezni, amelyek csak regisztrált eszközök konfigurált tooallow hozzáférés tooapplications.
2. Jelentkezzen be toohello Microsoft Azure portálra rendszergazdaként.
3. Hello bal oldali ablaktáblában jelölje ki **Active Directory**.
4. Válassza ki a címtárát.
5. Jelölje be hello **felhasználók** fülre. Válassza ki a felhasználói tooview eszközeiket
6. Jelölje be hello **eszközök** fülre.
7. Válassza ki **regisztrált eszközöket** hello a legördülő menü.
8. Itt megtekintheti, blokkolhatja, vagy hello felhasználók regisztrált eszközeinek feloldása.

## <a name="additional-topics"></a>További témakörök
Regisztrálhatja a Windows 7- és a Windows 8.1-tartományhoz csatlakozott eszközöket az Azure AD eszközregisztrációval. a következő témakörök hello hello Előfeltételek és hello lépéseket szükséges tooconfigure eszközregisztráció további információt nyújt a Windows 7 és Windows 8.1-eszközökön.

* [Automatikus eszközregisztráció az Azure Active Directoryval Windows-tartományhoz csatlakozott eszközökkel](active-directory-conditional-access-automatic-device-registration.md)
* [Automatikus eszközregisztráció az Azure Active Directoryval Windows 10-tartományhoz csatlakozott eszközökkel](active-directory-azureadjoin-devices-group-policy.md)

