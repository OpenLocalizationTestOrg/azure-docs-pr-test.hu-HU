---
title: "aaaUse toomanage hozzáférés tooresources az Azure Active Directory groups |} Microsoft Docs"
description: "Azure Active Directory toomanage felhasználói csoportok toouse miként férhetnek hozzá a tooon helyszíni és felhőalapú alkalmazásokhoz és erőforrásokhoz."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 876a356c8095505432e9346721f35c7943819e9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-tooresources-with-azure-active-directory-groups"></a>Hozzáférés tooresources kezelése az Azure Active Directoryval
Azure Active Directory (Azure AD) egy olyan átfogó identitás- és hozzáférés felügyeleti megoldás által biztosított képességek toomanage hozzáférés tooon helyszíni és felhőalapú alkalmazások és erőforrások például az Office 365-höz hasonló Microsoft online szolgáltatások robusztus és Microsoft SaaS-alkalmazások használatának világában. Ez a cikk áttekintést nyújt, de ha azt szeretné, az Azure AD toostart most csoportosítja, hello utasításait követve [biztonsági csoportok kezelése az Azure AD](active-directory-accessmanagement-manage-groups.md). Ha azt szeretné, hogy toosee további, az Azure Active Directory PowerShell toomanage használatát csoportok [eszközcsoport-kezelés Azure Active Directory-parancsmagjai](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

> [!NOTE]
> toouse Azure Active Directory, egy Azure-fiók szükséges. Ha nincs fiókja, akkor [regisztráljon egy ingyenes Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).
>
>

Az Azure AD belül hello fő szolgáltatásainak egyik hello képességét toomanage hozzáférés tooresources. Ezeket az erőforrásokat hello könyvtár, mint hello engedélyek toomanage objektumok keresztül hello könyvtárban, vagy külső toohello címtár, például az SaaS-alkalmazásokhoz, Azure-szolgáltatásokat, és a SharePoint-webhelyek vagy a helyszíni erőforrásokhoz szerepkörök része lehet. erőforrások. A felhasználó hozzáférési jogok tooa erőforrás rendelhető négy módja van:

1. Közvetlen hozzárendelés

    Felhasználók rendelhetők hozzá közvetlenül tooa erőforrás hello tulajdonosa erőforrás.
2. Csoporttagság

    Egy csoport társítható tooa erőforrás hello erőforrás tulajdonosa, és ezzel a módszerrel, hello tagok toohello erőforrás csoport hozzáférés biztosítása. Hello csoport tagságának majd hello csoport hello tulajdonosa kell kezelnie. Gyakorlatilag hello erőforrás tulajdonosa delegáltak hello engedély tooassign felhasználók tootheir erőforrás toohello hello csoport tulajdonosa.
3. Szabály alapú

    hello erőforrás tulajdonosa használhatja olyan szabály tooexpress, mely felhasználók hozzáférési tooa erőforrás legyen hozzárendelve. hello szabály hello eredményét függ hello attribútum szerepel az adott felhasználókra vonatkozóan, hogy a szabály és azok értékeit, és így hello erőforrás tulajdonosa hatékonyan ad hello jobb toomanage hozzáférés tootheir erőforrás toohello mérvadó forrása hello hello szabály használt attribútumok. hello erőforrás tulajdonosa továbbra is maga hello szabály kezeli, és határozza meg, melyik attribútumai és értékei biztosítanak hozzáférést tootheir erőforrás.
4. Külső hatóság

    hello hozzáférés tooa erőforrás származtatott külső forrásból; Ha például egy csoportot, amely egy mérvadó forrás, például egy helyszíni directory vagy egy SaaS-alkalmazást például a WorkDay szinkronizálva. hello erőforrás tulajdonosa hello tooprovide hozzáférés toohello csoporterőforrás rendeli hozzá, és hello külső adatforrás kezeli hello hello csoport tagjai.

   ![Access management diagram áttekintése](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Bemutató videó ismerteti a hozzáférés-kezelés
Egy rövid videót a magyarázó figyelemmel követheti:

**Az Azure AD: Bevezetés toodynamic tagsági csoportok**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Hogyan hozzáférés-kezelés az Azure Active Directory munkahelyi?
Az Azure AD hozzáférési felügyeleti megoldás hello közepére hello hello biztonsági csoport. A biztonsági csoport toomanage hozzáférés tooresources használata egy jól ismert összeállítást, amely lehetővé teszi, hogy egy rugalmas és könnyen érthető módon tooprovide hozzáférés tooa erőforrás szánt hello csoport számára. hello erőforrás tulajdonosa (vagy hello címtár rendszergazdájának hello) rendelhet hozzá egy csoporthoz tooprovide egy bizonyos jobb toohello erőforrások eléréséhez saját. hello hello csoport tagjai a rendszer hello hozzáférést, és hello erőforrás tulajdonosa hello jobb toomanage hello tagok listája egy csoport toosomeone más, például a részleg vezetője vagy a segélyszolgálat rendszergazdák számára delegálhatják.

![Az Azure Active Directory elérés felügyeleti diagramja](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

a csoport tulajdonosa hello is elérhetővé teheti, hogy a csoport önkiszolgáló kérelmek. Ennek során a felhasználó is keresse meg és található hello csoport és a kérelem toojoin hatékony keresést az engedély tooaccess hello erőforrások hello csoport keresztül. hello csoport hello tulajdonosa hello csoportot állíthat be, hogy a csatlakozási kérelmek automatikus jóváhagyása vagy hello csoport hello tulajdonos jóváhagyása szükséges. Amikor egy felhasználó esetén a kérelem toojoin egy csoportot, hello csatlakozási kérelmét hello csoport tulajdonosainak toohello továbbítja. Ha hello kérelmet jóváhagyja hello tulajdonosok közül, hello kérelmező felhasználó értesítést kap és hello felhasználó illesztett toohello csoport. Hello kérelem megtagadja hello tulajdonosok közül, ha hello kérelmező felhasználó értesítést kap, de nem a toohello csoport tagja.

## <a name="getting-started-with-access-management"></a>Ismerkedés a hozzáférés-kezelés
Készen áll a tooget el? Próbálkozzon meg néhány hello alapvető feladatokat hajthat végre az Azure AD-csoporthoz. Használja ezeket a képességeket tooprovide kifejezetten toodifferent csoportok a szervezet különböző erőforrások eléréséhez. Alapszintű első lépések listáját alább láthatók.

* [Egy egyszerű szabályt tooconfigure dinamikus csoporttagságok a csoport létrehozása](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)
* [Egy csoport toomanage tooSaaS alkalmazásokat használatával](active-directory-accessmanagement-group-saasapps.md)
* [Csoport elérhetővé tétele a felhasználók önkiszolgáló](active-directory-accessmanagement-self-service-group-management.md)
* [Az Azure AD Connect használatával a helyszíni csoport tooAzure szinkronizálása](active-directory-aadconnect.md)
* [Egy csoport tulajdonosainak kezelése](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Következő lépések
Most, hogy a megértettem kezelési hello alapjait itt érhetők el néhány további speciális funkciókat az Azure Active Directory hozzáférési tooyour alkalmazások és erőforrások kezelése.

* [Használata speciális szabályok toocreate attribútumok](active-directory-accessmanagement-groups-with-advanced-rules.md)
* [Biztonsági csoportok kezelése az Azure ad-ben](active-directory-accessmanagement-manage-groups.md)
* [Dedikált csoportok beállítása az Azure ad-ben](active-directory-accessmanagement-dedicated-groups.md)
* [A csoportok Graph API-referencia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
