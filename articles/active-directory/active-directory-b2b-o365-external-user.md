---
title: "aaaOffice 365 külső megosztás és az Azure Active Directory B2B együttműködés |} Microsoft Docs"
description: "a jogcímek referencia az Azure Active Directory B2B együttműködés leképezése"
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 60452b27b328453eda729bd839c982b479cb6f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Külső megosztás Office 365 és Azure Active Directory B2B együttműködés

Az Office 365 (OneDrive, SharePoint online-hoz, egységes csoportok stb.), és Azure Active Directory (Azure AD) B2B együttműködés technikailag megosztása külső hello ugyanazt jelentik. Minden külső megosztás (kivéve a onedrive-on vagy a SharePoint Online), beleértve a vendégek Office 365-csoportokat, már használja hello Azure AD B2B együttműködés meghívó API-k megosztása.

A OneDrive vagy a SharePoint Online rendelkezik egy külön meghívó manager. A onedrive-on vagy a SharePoint Online elindítása előtt az Azure AD fejlesztett támogatását külső megosztás támogatása. Idővel OneDrive vagy a SharePoint Online külső megosztás esedékes számos szolgáltatást és hello termék használó felhasználók sok több millió tartozó a-épül megosztása mintát. Van azonban néhány finom eltérések vannak a onedrive-on vagy a SharePoint Online külső megosztás működése, és az Azure AD B2B együttműködés működéséről között:

- A OneDrive vagy a SharePoint Online felhasználók toohello könyvtárat ad után a felhasználók rendelkeznek sikerült beváltani a meghívást. Igen előtt érvényesítési, nem látható hello felhasználói Azure AD portálon. Egy másik hely addig felkéri hello felhasználóként, egy új meghívó jön létre. Azonban Azure AD B2B együttműködés használata esetén felhasználót adnak hozzá közvetlenül a meghívót, hogy azok megjelenni everywhere.

- hello érvényesítési élmény a onedrive-on vagy a SharePoint Online az Azure AD B2B együttműködés hello élmény fog kinézni. Miután egy felhasználó visszaváltja meghívót, hello lép egyformának.

- Az Azure AD B2B együttműködés meghívott felhasználók is tárolható a onedrive-on vagy a SharePoint Online-ból párbeszédpanelek. OneDrive vagy a SharePoint Online meghívott felhasználók is jelenik meg az Azure AD után azok beváltani a meghívást.

- külső toomanage megosztása a onedrive-on vagy a SharePoint Online az Azure AD B2B együttműködés beállítása OneDrive vagy a SharePoint Online hello külső megosztási beállítás túl**csak a hello könyvtárban már külső felhasználókkal való megosztásának engedélyezése**. Felhasználók folytathatja tooexternally megosztott hely, és mentse a külső együttműködők, amely rendszergazdai hello hozzá van adva. Üdvözöljük a rendszergazdákat is hozzáadhat hello külső együttműködők hello B2B együttműködés meghívó API-k használatával.

![hello OneDrive vagy a SharePoint Online külső megosztásának beállítása](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználó tulajdonságai](active-directory-b2b-user-properties.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [B2B együttműködés meghívókat delegálása](active-directory-b2b-delegate-invitations.md)
* [Dinamikus csoportok és a B2B együttműködés](active-directory-b2b-dynamic-groups.md)
* [B2B együttműködés kód és a PowerShell-példák](active-directory-b2b-code-samples.md)
* [B2B együttműködés SaaS-alkalmazások konfigurálása](active-directory-b2b-configure-saas-apps.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés felhasználói jogcímek leképezése](active-directory-b2b-claims-mapping.md)
* [B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)
