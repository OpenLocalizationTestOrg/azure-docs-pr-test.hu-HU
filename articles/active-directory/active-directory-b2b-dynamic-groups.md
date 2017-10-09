---
title: "aaaDynamic csoportok és az Azure Active Directory B2B együttműködés |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés használható az Azure AD dinamikus csoportok"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: b011298de5fd2c851c6d9caaf5c2b257807ef0a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dinamikus csoportok és az Azure Active Directory B2B együttműködés

## <a name="what-are-dynamic-groups"></a>Mik azok a dinamikus csoportok?
Az Azure Active Directory (Azure AD), biztonsági csoporttagság dinamikus beállítási lehetőségek érhetők el a [hello Azure-portálon](https://portal.azure.com). A rendszergazdák az Azure Active Directory címtárban létrehozott toopopulate csoportok alapján, felhasználói attribútumok (például userType, részleg vagy ország) állíthatja be. Tagok automatikusan hozzáadhatók tooor attribútumaik alapján egy biztonsági csoportot eltávolítani. Ezek a csoportok hozzáférést biztosíthat tooapplications vagy a felhőbeli erőforrások (SharePoint-webhelyekhez, dokumentumok), és tooassign licencek toomembers. További információk a dinamikus csoportok [dedikált csoportok az Azure Active Directoryban](active-directory-accessmanagement-dedicated-groups.md).

megfelelő hello [Azure AD Premium P1 és P2 licencelési](https://azure.microsoft.com/pricing/details/active-directory/) szükséges toocreate és -felhasználási dinamikus csoportok van. További információ: hello cikk [dinamikus csoporttagság Attribútumalapú szabályok létrehozása az Azure Active Directoryban](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-hello-built-in-dynamic-groups"></a>Mik azok a hello beépített dinamikus csoportot?
Hello **minden felhasználó** dinamikus csoport lehetővé teszi, hogy a bérlői rendszergazdák toocreate kattintson egy hello bérlő egyetlen összes felhasználót tartalmazó csoport. Alapértelmezés szerint hello **minden felhasználó** csoport hello könyvtárban, beleértve a tagok és a vendégek felhasználókat tartalmazza.
Hello új Azure Active Directory felügyeleti portálon választhat tooenable hello **minden felhasználó** csoporthoz hello beállítások megtekintéséhez.

![beépített csoportok](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-hello-all-users-dynamic-group"></a>Korlátozási hello minden felhasználók dinamikus csoport
Alapértelmezés szerint hello **minden felhasználó** csoport tartalmazza a B2B együttműködés (vendég) felhasználókat is. További biztonságossá teheti a **minden felhasználó** használatával egy szabály tooremove vendég felhasználók csoportba. hello alábbi ábrán látható hello **minden felhasználó** csoport tooexclude vendégek módosítva.

![minden felhasználói csoport engedélyezése](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

Is találhat, hasznos toocreate tartalmazó új dinamikus csoportok csak a vendégfelhasználók számára, hogy a házirendek (például az Azure AD feltételes hozzáférési házirendek) toothem is alkalmazhat.
Milyen ilyen csoporthoz nézhet ki:

![vendégfelhasználók kizárása](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználó tulajdonságai](active-directory-b2b-user-properties.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [B2B együttműködés meghívókat delegálása](active-directory-b2b-delegate-invitations.md)
* [B2B együttműködés kód és a PowerShell-példák](active-directory-b2b-code-samples.md)
* [B2B együttműködés SaaS-alkalmazások konfigurálása](active-directory-b2b-configure-saas-apps.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés felhasználói jogcímek leképezése](active-directory-b2b-claims-mapping.md)
* [Külső Office 365-megosztás](active-directory-b2b-o365-external-user.md)
* [B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)
