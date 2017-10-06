---
title: "aaaHow Azure kapcsolódnak előfizetések az Azure Active Directory |} Microsoft Docs"
description: "Jelentkezzen be tooMicrosoft Azure és a kapcsolódó problémák, például egy Azure-előfizetés és Azure Active Directory hello kapcsolatát."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4f831cfb972efec57083fcaa63adb43fde7b2faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>How Azure subscriptions are associated with Active Directory? (Hogyan kapcsolódnak az Azure-előfizetések az Azure Active Directory-hoz?)
Ez a cikk ismerteti a hello kapcsolatát egy Azure-előfizetések és Azure Active Directory (Azure AD), és hogyan tooadd meglévő előfizetés tooyour az Azure Active directory.

## <a name="your-azure-subscriptions-relationship-tooazure-ad"></a>Az Azure-előfizetéshez kapcsolat tooAzure AD
Az Azure-előfizetéshez az Azure ad szolgáltatással, ami azt jelenti, hogy megbízik hello directory tooauthenticate felhasználók, szolgáltatások és az eszközök megbízhatósági kapcsolatban áll. Több előfizetés is megbízhat hello ugyanabban a könyvtárban, de az egyes előfizetések csak egy címtárban bízhat. 

hello megbízhatósági kapcsolat, amely egy előfizetés tartozik könyvtár nem hello kapcsolat, amely további erőforrások az Azure (webhelyek, adatbázisok és így tovább) áll. Ha egy előfizetés lejár, a hozzáférési toohello más hello előfizetéshez társított erőforrásokat is leáll. Azonban az Azure AD-címtár az Azure-ban marad, és rendelje hozzá egy másik előfizetésben található könyvtárhoz, és új előfizetést hello hello címtár kezelése.

![az előfizetések társításának módját ábrázoló diagram](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Minden felhasználó egyetlen saját címtárral rendelkezik, amely hitelesíti őt, de más címtárakban lehet vendég is. Az Azure AD-hello könyvtárak, amelyek a felhasználói fiók pedig a tag vagy Vendég tekintheti meg.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Az Azure AD és a felhőszolgáltatásbeli előfizetések
Az Azure AD hello core címtár- és identitáskezelési funkciókat biztosít a Microsoft felhőszolgáltatásai, beleértve a legtöbb mögött:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Ha regisztrál bármelyik fenti Microsoft felhőszolgáltatásra hello Azure AD szolgáltatás szabad kap. Ha azt szeretné, hogy egy további Azure-előfizetés tooan az Azure Active directory tooadd, be kell jelentkeznie be egy Microsoft-fiókkal. Jelentkezzen be tooAzure munkahelyi vagy iskolai fiókkal, ha nem vehet tooan meglévő címtárhoz Azure-előfizetés, mert a munkahelyi vagy iskolai fiókot nem lehet hitelesíteni, közvetlenül az Azure-ban. 

## <a name="tooadd-an-existing-subscription-tooyour-azure-ad-directory"></a>meglévő előfizetés tooyour az Azure Active directory tooadd
Mindkét hello aktuális könyvtárhoz, mely hello előfizetés társítva a fiókkal kell bejelentkeznie, és hello könyvtárban tooadd kívánja azt. 

1. Jelentkezzen be toohello [Azure Account Center](https://account.windowsazure.com/Home/Index) egy olyan fiókkal, amely van hello Fiókadminisztrátort hello előfizetés amelyek tulajdonjoga tootransfer szeretné.
2. Győződjön meg arról, hogy hello felhasználóját, aki toobe hello előfizetés tulajdonosa megcélzott hello a könyvtárban van.
3. Kattintson az **Előfizetés átadása** elemre.
4. Adja meg a hello címzett. hello címzett automatikusan kap egy e-maileket az elfogadási kapcsolaton.
5. hello címzett hello hivatkozásra kattint, és a következő hello utasításokat, beleértve a fizetési adatok megadása. Hello címzett sikeres, hello előfizetés kerül. 
6. hello alapértelmezett könyvtár hello előfizetés toohello directory hello felhasználó van.


## <a name="suggestions-toomanage-both-a-subscription-and-a-directory"></a>Javaslatok toomanage előfizetés, és a könyvtár
Azure-előfizetések rendszergazdai szerepkörei hello Azure-előfizetéssel társított erőforrásokat toohello kezelése. Ez a szakasz ismerteti az Azure-előfizetések rendszergazdái és az Azure AD-címtár rendszergazdái hello különbségei. Rendszergazdai szerepkörök és egyéb javaslatok az előfizetéshez tartoznak, toomanage használatuk [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md).

Alapértelmezés szerint akkor hello szolgáltatás rendszergazdai szerepkörrel feliratkozás. Ha a toosign kell és elérniük a szolgáltatásokat hello ugyanahhoz az előfizetéshez, akkor hozzáadhatja ezeket társadminisztrátorként. 

Az Azure AD rendszergazdai szerepkörei toomanage hello directory és az identitás-kapcsolatos funkciók különböző szabálykészleteket rendelkezik. Például egy címtár globális rendszergazdájának hello hozzáadhat felhasználók és csoportok toohello directory és a felhasználók többtényezős hitelesítését követelheti. A címtárat létrehozó felhasználó toohello globális rendszergazdai szerepkör van rendelve, és rendeljen rendszergazdai szerepkörei tooother felhasználók. Az Azure AD rendszergazdai szerepköreit más szolgáltatások is használják, például az Office 365 és a Microsoft Intune. 

Az Azure-előfizetések rendszergazdái és az Azure AD-címtár rendszergazdái két különböző szerepkörnek számítanak. 
* Azure-előfizetés rendszergazdái kezelhetik az erőforrásokat az Azure-ban, és használhatja az Azure AD hello Azure-portálon (mert hello Azure-portálon magát egy Azure-erőforrás). 
* Címtár rendszergazdái kezelhetik a tulajdonságok csak az Azure AD-címtár hello.

Egy személy mindkét szerepkörrel rendelkezhet, de erre nincs szükség. Egy címtár globális rendszergazdai fiókját nem lehet hozzárendelni az Azure-előfizetés szolgáltatás-rendszergazdai vagy társadminisztrátori szerepköréhez, sem pedig fordítva. Anélkül, hogy a rendszergazda hello előfizetés, hello felhasználó toohello Azure-portálon bejelentkezhet, de nem tudja kezelni a feliratkozásban hello portálon hello könyvtárak. Azonban ez a felhasználó az egyéb eszközökkel, például Azure AD PowerShell vagy az Office 365 felügyeleti központban hello könyvtárak kezelheti.

## <a name="next-steps"></a>Következő lépések
* További részletek toolearn, hogyan toochange rendszergazdák az Azure-előfizetéssel, lásd: [egy Azure-előfizetés tooanother fiók tulajdonjogának átruházása](../billing/billing-subscription-transfer.md)
* toolearn hogyan szabályozott erőforrások elérése a Microsoft Azure-ban kapcsolatos további információkért lásd: [az az Azure-erőforrások hozzáférésének megismerése](active-directory-understanding-resource-access.md)
* További információt a tooassign szerepkörök az Azure ad-ben, lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
