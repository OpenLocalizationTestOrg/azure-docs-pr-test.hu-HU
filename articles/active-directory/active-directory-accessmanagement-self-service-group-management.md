---
title: "aaaSetting Azure Active Directory fel az önkiszolgáló alkalmazáshozzáférés-kezeléshez |} Microsoft Docs"
description: "Önkiszolgáló csoportfelügyelet lehetővé teszi, hogy a felhasználók toocreate és biztonsági csoportok vagy az Azure Active Directory és ajánlatok felhasználók hello lehetőségét toorequest biztonsági vagy Office 365-csoporttagságot Office 365-csoportok kezelése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 2a73f4ed2532d41143fe5abe2fef1322d971a5c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Az Azure Active Directory beállítása önkiszolgáló csoportkezelésre
Önkiszolgáló csoportfelügyelet lehetővé teszi, hogy a felhasználók toocreate, és kezelheti a biztonsági vagy Office 365-csoportokat az Azure Active Directory (Azure AD). A felhasználó biztonsági csoport vagy az Office 365-csoporttagságot is kérhet, és majd hello hello csoport tulajdonosa is jóváhagyására vagy visszautasítására tagságát. Ily módon a csoporttagság napi szintű felügyelete meghatalmazott toopeople ki, akik az adott tagság hello az üzleti környezet tisztában lehet. Az önkiszolgáló csoportkezelési szolgáltatások kizárólag biztonsági és Office 365-csoportok esetében érhetőek el, levelezési címmel rendelkező biztonsági csoportok vagy terjesztési listák esetében nem.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.

Az önkiszolgáló csoportkezelési szolgáltatás jelenleg két alapvető alkalmazási helyzetet tartalmaz: a delegált és az önkiszolgáló csoportkezelést.

* **Delegált csoportkezelés** példa, hogy a rendszergazda hozzáférést tooa SaaS-alkalmazás hello vállalat által használt kezelő. E hozzáférési jogosultságok kezelése nehézkessé válik, így a rendszergazda kéri hello üzleti tulajdonosa toocreate egy új csoportot. hello rendszergazda hello toohello új alkalmazáscsoportba tartozó hozzáférési jogosultságot rendel, és hozzáadja a toohello csoport összes személyek toohello alkalmazás már elérésének. hello tulajdonos majd adhat hozzá további felhasználókat, és ezek a felhasználók automatikusan kiosztott toohello alkalmazás. hello üzleti tulajdonosa nem szükséges toowait hello rendszergazdai toomanage hozzáférés a felhasználók számára. Ha engedélyezi a hello rendszergazda hello azonos engedély tooa manager egy másik üzleti csoportban, majd az adott személyt is kezelhetők a saját felhasználók hozzáférésének. Hello tulajdonos sem hello manager megtekintheti vagy egymás felhasználók kezeléséhez. hello rendszergazdai hozzáférés toohello alkalmazás és a blokk hozzáférési jogok szükség esetén rendelkező felhasználók is látható.
* **Önkiszolgáló csoportkezelés** – Jellemző példa rá két olyan felhasználó, akik egyaránt rendelkeznek egymástól függetlenül üzembe helyezett SharePoint Online-webhelyekkel. Minden saját csapatának hozzáférést tootheir helyek toogive kívánják. tooaccomplish, egy csoportot az Azure AD hozhatnak létre, és a SharePoint Online azok választja ki, hogy csoport tooprovide hozzáférés tootheir helyek. Ha valaki hozzáférést igényel, a hozzáférési Panel hello kérnek, és a jóváhagyás után hozzáférhetnének tooboth SharePoint Online-webhelyhez automatikusan. Később az egyik legyen úgy dönt, hogy minden férnek hozzá a hello webhely is szeretne küldeni hozzáférés tooa adott SaaS-alkalmazáshoz. SaaS-alkalmazás hello hello rendszergazdája adhat hozzá hozzáférési jogosultságokat a(z) hello alkalmazás toohello SharePoint Online-webhelyhez. Ettől a jóváhagyott beolvasása kéréseit lehetőséget nyújt az access toohello két SharePoint Online-webhelyhez, és is toothis SaaS-alkalmazás.

## <a name="making-a-group-available-for-end-user-self-service"></a>Csoport elérhetővé tétele önkiszolgáló végfelhasználói tevékenységhez
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), nyissa meg az Azure AD-címtár.
2. A hello **konfigurálása** lapon **delegált csoportkezelés** tooEnabled.
3. Állítsa be **felhasználók létrehozhatnak biztonsági csoportokat** vagy **felhasználók létrehozhatnak Office-csoportokat** tooEnabled.

Ha **felhasználók létrehozhatnak biztonsági csoportokat** van engedélyezve van, a címtárban szereplő összes felhasználó számára engedélyezett toocreate új biztonsági csoportokat és adja hozzá toothese csoportok tagjai. Ezek az új csoportok is tenné jelennek meg az hello hozzáférési Panel összes többi felhasználó számára. Ha hello csoport hello csoportházirend-beállítás engedélyezi, más felhasználók hozhatnak létre kérelmek toojoin ezeket a csoportokat. Ha a **Users can create security groups** (A felhasználók létrehozhatnak biztonsági csoportokat) beállítás le van tiltva, a felhasználók nem hozhatnak létre csoportokat, és nem módosíthatják azokat a meglévő csoportokat, amelyeknek a tulajdonosai. Azonban ugyanakkor továbbra is kezelheti hello ilyen csoportok tagságát, és hagyja jóvá a más felhasználók toojoin érkező kéréseket a csoporthoz.

Is **önkiszolgáló biztonsági csoportok használó felhasználók** tooachieve még hozzáférés-vezérlés keresztül önkiszolgáló csoportkezelési a felhasználók számára. Ha **felhasználók létrehozhatnak csoportokat** van engedélyezve van, a címtárban szereplő összes felhasználó számára engedélyezett toocreate új csoportokat és adja hozzá toothese csoportok tagjai. Úgy is **önkiszolgáló biztonsági csoportok használó felhasználók** tooSome, áll csoportkezelést csoport felügyeleti tooonly korlátozott csoport számára. Ha a kapcsoló értéke tooSome, hozzá kell adnia felhasználók toohello csoport SSGMSecurityGroupsUsers ahhoz, hogy hozzon létre új csoportokat, és adja hozzá a tagok toothem. Úgy, hogy **önkiszolgáló biztonsági csoportok használó felhasználók** tooAll, az összes felhasználó a directory toocreate új csoportok.

Is használhatja a hello **csoport, a biztonsági csoportokat is használhatják az önkiszolgáló** toospecify egy egyéni nevet a csoport tagjai használhatják az önkiszolgáló mezőben.

## <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
