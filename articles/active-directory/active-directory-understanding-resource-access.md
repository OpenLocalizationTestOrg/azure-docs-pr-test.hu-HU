---
title: "aaaUnderstanding erőforrások elérése az Azure-ban |} Microsoft Docs"
description: "Ez a témakör ismerteti a fogalmakat előfizetés rendszergazdák toocontrol erőforrás-hozzáférés használata a hello Azure-portálon"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 06b9c4166bdea849faae67cae3146c6c278deb97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-resource-access-in-azure"></a>Az az Azure-erőforrások hozzáférésének megismerése
> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. hello Azure-portált biztosít [szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md) , Azure-erőforrások kezelhetők pontosabban.
> 
> 

2013 októberében hello a klasszikus Azure portálon, és a Service Management API-k lettek integrálva az Azure Active Directoryban rendelés toolay hello áttelepítés tooAzure erőforrások kezelésére szolgáló hello felhasználói élmény javítására. Az Azure Active Directory már például a felhasználók kezelése, a helyszíni címtár-szinkronizálás, a multi-factor authentication és alkalmazás-hozzáférés-vezérlés remek funkciókat biztosít. Természetesen ezek kell is elérhetővé válik az Azure-erőforrások kezelése megfogalmazott.

Hozzáférés-vezérlés az Azure-ban elindul egy számlázási szempontjából. Azure-fiók esetén érhető el hello felkeresésével hello tulajdonosának [Azure Accounts Center](https://account.windowsazure.com/subscriptions), hello AA fiók rendszergazdai. Előfizetések számlázási tárolója, de úgy is működnek, mint a biztonsági határ: minden előfizetés rendelkezik a szolgáltatás rendszergazdai (SA) akik hozzáadása, eltávolítása és módosítása az Azure-erőforrások az adott előfizetés hello használatával [a klasszikus Azure portálon](https://manage.windowsazure.com/). az új előfizetés hello alapértelmezett SA hello AA, de hello AA módosíthatja a hello Azure Accounts Center hello SA.

<br><br>![Azure-fiókra][1]

Előfizetések is könyvtár van társítva. hello directory egy felhasználók csoportját határozza meg. Ezek lehetnek a felhasználók hello munkahelyi vagy iskolai hello directory létrehozott vagy külső felhasználók számára (Ez azt jelenti, hogy a Microsoft Accounts). Az előfizetések által hozzárendelt szolgáltatás-rendszergazdai (SA) vagy Társadminisztrátorának (CA); directory felhasználók egy részhalmazát érhetők el hello csak egyetlen kivételt jelenti, hogy az örökölt összetevők miatt, Microsoft Accounts (korábbi nevén Windows Live ID Azonosítóval) rendelhető rendszergazdai (SA) vagy a CA anélkül, hogy jelen hello könyvtárban.

<br><br>![Hozzáférés-vezérlés az Azure-ban][2]

Hello a klasszikus Azure portálon belül funkciók lehetővé teszik a SAs használatával a Microsoft Account toochange hello könyvtár tartozó előfizetés hello használatával aláírt **könyvtár szerkesztése** hello parancs**Előfizetések** lap **beállítások**. Vegye figyelembe, hogy ez a művelet hatással vannak a hozzáférés-vezérlés hello előfizetésben.

> [!NOTE]
> Hello **könyvtár szerkesztése** parancsot hello klasszikus Azure portál nincs elérhető toousers be voltak jelentkezve egy munkahelyi vagy iskolai fiókhoz, mivel ezek a fiókok bejelentkezhetnek csak toohello directory toowhich tartoznak.
> 
> 

<br><br>![Egyszerű felhasználói bejelentkezési folyamat][3]

Hello egyszerű esetben egy szervezet (Contoso) például számlázási kényszerítése, és hozzáférés-vezérlés hello azonos beállítása előfizetések között. Ez azt jelenti, hogy hello könyvtár egy olyan Azure-fiók tulajdonában lévő társított toosubscriptions. Sikeres bejelentkezés toohello a klasszikus Azure portálon, hogy a felhasználók látni két gyűjtemények erőforrások (az előző ábrán hello narancssárga ábrázolva):

* Könyvtárak ahol a felhasználói fiók létezik-e (vagy fel van véve egy idegen rendszerbiztonsági tag forrása). Vegye figyelembe, hogy a bejelentkezéshez használt hello könyvtár nem megfelelő toothis számítási, így a címtárak mindig megjelennek, függetlenül attól, hol jelentkezett.
* Előfizetések, amelyek hozzárendelve a bejelentkezéshez használt hello könyvtár, és amely hello felhasználói részét képező erőforrásokat érhetik el (Ha olyan rendszergazdai (SA) vagy a CA).

<br><br>![A felhasználó több előfizetések és könyvtárak][4]

Több címtárban előfizetések felhasználóknál hello képességét tooswitch hello jelenlegi kontextusa hello a klasszikus Azure portálon hello előfizetés szűrő használatával. Hello színfalak ez külön bejelentkezési tooa másik címtárhoz eredményez, de mindez zökkenőmentesen egyszeri bejelentkezés (SSO) használatával.

Lehet, hogy a műveletek, például az erőforrások áthelyezése másik előfizetések nehezebb előfizetések egyetlen nézetének miatt. tooperform hello erőforrás átviteli, szükség lehet toofirst hello használata **könyvtár szerkesztése** parancs hello az előfizetések oldalán **beállítások** tooassociate hello előfizetések toohello ugyanabban a könyvtárban .

## <a name="next-steps"></a>Következő lépések
* További részletek toolearn, hogyan toochange rendszergazdák az Azure-előfizetéssel, lásd: [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](../billing/billing-add-change-azure-subscription-administrator.md)
* Hogyan kapcsolódik az Azure Active Directory a tooyour Azure-előfizetés további információkért lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directoryval](active-directory-how-subscriptions-associated-directory.md)
* További információt a tooassign szerepkörök az Azure ad-ben, lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
