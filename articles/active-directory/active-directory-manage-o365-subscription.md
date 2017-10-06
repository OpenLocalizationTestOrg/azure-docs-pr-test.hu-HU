---
title: "aaaManage hello directory az Office 365-előfizetéshez az Azure-ban |} Microsoft Docs"
description: "Egy Azure Active Directory és a klasszikus Azure portálon hello Office 365-előfizetés címtárának kezelése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 746987b7-2dfd-4b35-819d-37c1b65c5c81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4faf9936d7e94b85356a18adfcf3d2a48e5b025f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-directory-for-your-office-365-subscription-in-azure"></a>Az Office 365-előfizetéshez az Azure-ban hello címtár kezelése
Ez a cikk ismerteti, hogyan toomanage egy könyvtárat, amely az Office 365-előfizetéssel, hello klasszikus Azure portál használatával készült. Hello szolgáltatás-rendszergazda vagy egy Azure-előfizetés toosign a klasszikus Azure portálon toohello egy társadminisztrátoraként kell lennie. Ha még nem rendelkezik Azure-előfizetéssel, regisztrálhat egy [30 napos ingyenes próbaverzióra](https://azure.microsoft.com/trial/get-started-active-directory/), és kevesebb mint 5 perc alatt üzembe helyezheti az első felhőalapú megoldást ezen hivatkozás használatával. Lehet, hogy toouse hello munkahelyi vagy iskolai fiók toosign tooOffice 365 használni.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.

Hello Azure-előfizetés elvégzése után jelentkezzen be a klasszikus Azure portálon toohello, és az Azure-szolgáltatások eléréséhez. Kattintson az Active Directory-bővítményt hello toomanage hello az Office 365-felhasználókat hitelesítő címtár.

Ha már rendelkezik Azure-előfizetéssel, hello folyamat toomanage további címtár is egyszerű. Michael Smith például rendelkezhet egy Office 365-előfizetéssel a Contoso.com tartományhoz. Azure-előfizetése is lehet, amelyre msmith@hotmail.com Microsoft-fiókjával regisztrált. Ebben az esetben két címtárat kezel.

| Előfizetés | Office 365 | Azure |
| --- | --- | --- |
|   Megjelenített név |Contoso |Alapértelmezett Azure Active Directory (Azure AD) címtár |
|   Tartománynév |contoso.com |msmithhotmail.onmicrosoft.com |

RON toomanage hello felhasználói identitásokat a Contoso címtárához hello fiókkal van bejelentkezve a saját Microsoft-fiókjával, tooAzure, hogy engedélyezheti az Azure AD-funkciókat, mint a többtényezős hitelesítés. a következő diagram hello segíthet tooillustrate hello folyamat.

![Két független címtár toomanage ábra](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

Ebben az esetben hello két címtár egymástól független.

## <a name="toomanage-two-independent-directories"></a>toomanage két független címtár
A rendezés Michael Smith toomanage a két címtárszolgáltatás tooAzure, mint a fiókkal van bejelentkezve msmith@hotmail.com, ő hello a következő lépéseket kell végrehajtania:

> [!NOTE]
> Ezek a lépések csak akkor végezhetők el, ha egy felhasználó Microsoft-fiókkal van bejelentkezve. Ha hello felhasználó munkahelyi vagy iskolai fiókkal van bejelentkezve, hello beállítás túl**meglévő címtár használata** nem érhető el. A munkahelyi vagy iskolai fiókok csak saját címtárukkal (Ez azt jelenti, hogy hello könyvtárat, ahol hello munkahelyi vagy iskolai fiók tárolva van, adott hello üzleti vagy az iskola a tulajdonosa) hitelesíthetők.
>
>

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) , msmith@hotmail.com.
2. Kattintson az **Új** > **Alkalmazásszolgáltatások** > **Active Directory** > **Címtár** > **Egyéni létrehozása** lehetőségre.
3. Kattintson a meglévő könyvtárhoz, és jelölje be hello **kijelentkezésre készen toobe vagyok** jelölőnégyzetet.
4. Jelentkezzen be toohello klasszikus Azure portálra a Contoso.onmicrosoft.com globális rendszergazdájaként (például msmith@contoso.com).
5. Amikor a rendszer kéri a túl**használata hello Contoso címtár az Azure-ral?**, kattintson a **Folytatás**.
6. Kattintson az **Azonnali kijelentkezés** parancsra.
7. Jelentkezzen be toohello, a klasszikus Azure portálon msmith@hotmail.com. hello Contoso címtárához és hello alapértelmezett címtár megjelenik hello Active Directory-bővítményt.

Ezek a lépések végrehajtását követően msmith@hotmail.com hello Contoso címtár globális rendszergazdája.

## <a name="tooadminister-resources-as-hello-global-admin"></a>globális rendszergazda hello tooadminister erőforráshoz
Most tegyük fel, hogy Jane Doe kell felügyelni a webhelyek és adatbázis-erőforrások társított hello Azure-előfizetéssel msmith@hotmail.com. Mielőtt ezt megtehetné, Michael Smith kell toocomplete ezeket a további lépéseket:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) hello szolgáltatás-rendszergazdai fiók használatával az Azure-előfizetés hello (ebben a példában msmith@hotmail.com).
2. Átviteli hello előfizetés toohello Contoso címtárához: kattintson a **beállítások** > **előfizetések** > Válasszon hello előfizetést > **könyvtár szerkesztése** > Válasszon **Contoso (Contoso.com)**. Részeként hello átviteli, a munkahelyi vagy iskolai fiókokat, amelyek hello előfizetés társadminisztrátora törlődnek.
3. Adja hozzá Jane Doe hello előfizetés társadminisztrátoraként: kattintson a **beállítások** > **rendszergazdák** > Válasszon hello előfizetést > **Hozzáadás** > típusa **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Következő lépések
Az előfizetések és könyvtárak hello kapcsolatát kapcsolatos további információkért lásd: [hogyan előfizetés tartozik könyvtár](active-directory-how-subscriptions-associated-directory.md).
