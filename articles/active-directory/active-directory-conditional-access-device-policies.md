---
title: "aaaAzure Active Directory feltételes hozzáférési eszközházirendek Office 365-szolgáltatásokhoz |} Microsoft Docs"
description: "További információk a hogyan tooprovision feltételes hozzáférést biztosító eszköz házirendek toohelp biztonságosabbá a vállalati erőforrásokhoz, felhasználói megfelelőség és a hozzáférés tooservices megőrzésével."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8664c0bb-bba1-4012-b321-e9c8363080a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 38229a8d9766efbf0e6b17875b3018011c6b4ea3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="active-directory-conditional-access-device-policies-for-office-365-services"></a>Active Directory feltételes hozzáférési eszközházirendek Office 365-szolgáltatásokhoz

Feltételes hozzáférés több adatra toowork igényel. A multi-factor Authentication magában foglalja a hitelesített felhasználó, egy hitelesített eszköz és a megfelelő eszköz, többek között. Ebben a cikkben azt elsősorban összpontosítania, hogy a szervezet használhatja szabályozhatja a hozzáférést tooOffice 365 szolgáltatások toohelp eszköz-alapú feltételek. 

A vállalati felhasználók szeretné tooaccess Office 365 szolgáltatások például az Exchange és SharePoint Online munkahelyi és iskolai a személyes eszközeikről. Érdemes hello hozzáférés toobe biztonságos. Feltételes hozzáférési házirendek toohelp ellenőrizze vállalati eszközerőforrások nagyobb biztonságot nyújt, a felhasználóknak, akik az előírásoknak megfelelő eszközök hozzáférési tooservices vezérlését építhető ki. Feltételes hozzáférési házirendek tooOffice 365 hello a Microsoft Intune feltételes hozzáférési portálon állíthatja be.

Azure Active Directory (Azure AD) érvénybe lépteti a feltételes hozzáférési házirendek toohelp biztonságos hozzáférést tooOffice 365 szolgáltatások. Létrehozhat egy feltételes hozzáférési szabályzatot, amely blokkolja a hozzáférését az Office 365-szolgáltatás egy nem megfelelő eszközt használó felhasználó. hello felhasználói toohello szolgáltatás megadása előtt meg kell felelnie toohello vállalati eszközökre vonatkozó házirendeket. Alternatív megoldásként házirendet hozhat létre a felhasználók tooenroll igénylő az eszközök toogain hozzáférés tooan Office 365 szolgáltatás. Házirendek lehet a szervezet felhasználói alkalmazott tooall, vagy korlátozott tooa néhány célcsoportok. Adott idő alatt több cél csoportok tooa házirend is hozzáadhat.

Eszköz-házirendek érvényesítéséről előfeltétele az, hogy felhasználók regisztrálnia kell az eszközeiket, az Azure AD hello eszközregisztrációs szolgáltatással. Az eszközök, amelyeket az eszközregisztrációs szolgáltatás az Azure AD hello regisztrálni a többtényezős hitelesítést tooturn dönthet úgy is. A multi-factor authentication hello Azure Active Directory eszközregisztrációs szolgáltatás ajánlott. Többtényezős hitelesítés be van kapcsolva, amikor a felhasználók regisztrálják az eszközeiket az Azure AD eszközregisztrációs szolgáltatás hello kéttényezős hitelesítéshez sor kerül.

## <a name="how-does-a-conditional-access-policy-work"></a>Hogyan működik a feltételes hozzáférési házirendet?

Ha a felhasználó hozzáférési tooan Office 365 szolgáltatás kér egy támogatott eszközplatform, az Azure AD akkor hitelesíti az hello felhasználó- és hello. Az Azure AD biztosít toohello szolgáltatás csak akkor, ha hello felhasználói toohello házirend hello szolgáltatás értéke megfelel. A nem regisztrált eszközökön felhasználók útmutatást hogyan tooenroll és megfelelő tooaccess vállalati Office 365-szolgáltatásokhoz. IOS és Android-eszközök a felhasználók eszközein szükséges tooenroll vannak hello Intune vállalati portál alkalmazás használatával. Amikor a felhasználó regisztrál egy eszközt, hello eszköz regisztrálása az Azure ad-val, és regisztrálva van a felügyeleti eszközök és megfelelőség. A Microsoft Intune-nal hello Azure AD eszközregisztrációs szolgáltatás kell használnia a mobileszközök felügyeletéhez Office 365-szolgáltatásokhoz. Eszközök beléptetése szabályzatok érvényesítik szükség a felhasználók tooaccess Office 365-szolgáltatásokhoz.

Amikor a felhasználó sikeresen regisztrál egy eszközt, az eszköz hello lesz megbízható. Az Azure AD hello hitelesített felhasználó egyszeri bejelentkezéses hozzáférést toocompany alkalmazások biztosít. Az Azure AD érvénybe lépteti a feltételes hozzáférési házirend toogrant tooa szolgáltatás nem csak hello első alkalommal hello felhasználó hozzáférést kér, de minden alkalommal hello felhasználói megújítja hozzáférési kérelmet. hello felhasználótól megtagadja a hozzáférést tooservices bejelentkezési hitelesítő adatok megváltoznak, hello eszköz elvesztésekor vagy ellopásakor vagy hello házirend feltételeinek hello időpontban hello kérelem megújítási nem teljesülnek.

## <a name="deployment-considerations"></a>Telepítési szempontok

Hello Azure AD eszköz regisztrációs szolgáltatás tooregister eszközöket kell használnia.

Ha a helyszíni felhasználók készül toobe hitelesítése az Active Directory összevonási szolgáltatások (AD FS) (1.0-s verziója és újabb verziók) szükséges. Többtényezős hitelesítés a munkahelyi csatlakoztatás sikertelen lesz, ha hello identitásszolgáltató nem képes a többtényezős hitelesítés. Például többtényezős hitelesítés nem használható az AD FS 2.0-s. Győződjön meg arról, hogy hello a helyszíni AD FS működik a multi-factor Authentication hitelesítéshez, és hogy egy érvényes multi-factor authentication módszer helyen hello Azure AD eszközregisztrációs szolgáltatással többtényezős hitelesítés bekapcsolása előtt. Például a Windows Server 2012 R2 AD FS többtényezős hitelesítés képességekkel rendelkezik. Is be kell egy további érvényes hitelesítés (többtényezős hitelesítés) metódus hello AD FS-kiszolgálón az Azure AD hello eszközregisztrációs szolgáltatással többtényezős hitelesítés bekapcsolása előtt. Az AD FS-ben támogatott többtényezős hitelesítési módszerekkel kapcsolatos további információkért lásd: [további hitelesítési módszerek konfigurálása az AD FS](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs).

## <a name="next-steps"></a>Következő lépések

*   A válaszok toocommon kérdésekhez lásd: [Azure Active Directory feltételes hozzáférés – gyakori kérdések](active-directory-conditional-faqs.md).
