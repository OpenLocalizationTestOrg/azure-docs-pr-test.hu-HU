---
title: "Active Directory jelentéskészítési értesítései aaaAzure"
description: "Hogyan toouse hello gyanús bejelentkezési az Azure Active Directory jelentéskészítési értesítéseket moduljainak."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: 3843c45eaf9d68e671943bfdbc7ab68933f38fbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-notifications"></a>Az Azure Active Directory jelentéskészítési értesítései
## <a name="what-reports-generate-email-notifications"></a>Milyen jelentések készítése az értesítő e-mailek
Jelenleg csak hello szabálytalan bejelentkezési tevékenység jelentés eseményindítók az e-mail értesítések.

## <a name="what-is-an-irregular-sign-in"></a>Mi az az "szabálytalan bejelentkezési"?
Szabálytalan bejelentkezések megegyeznek a gépi tanulási algoritmusok, hello alapon "lehetetlen odautazás" feltétel egy rendellenes bejelentkezési helye és az eszköz által azonosított. Ez azt jelezheti, hogy egy támadó próbálkozott toosign az ezzel a fiókkal.

## <a name="who-receives-hello-email-notifications"></a>Hello e-mail értesítéseket fogad számára?
hello e-mailt küld tooall globális rendszergazdák számára egy Active Directory Premium licenc van rendelve. tooensure biztosítását, hogy elküldi a toohello rendszergazdák másodlagos E-mail-cím is. Tartalmaznia kell a rendszergazdák aad-alerts-noreply@mail.windowsazure.com a saját megbízható feladók listájához, így azok nem teljesíti az üdvözlő e-mail.

## <a name="how-often-are-these-emails-sent"></a>Milyen gyakran történik a küldi az e-maileket?
hello e-mailt küld, ha a 10 új szabálytalan bejelentkezési tevékenység történik hello a 30 napnál, vagy mert hello utolsó e-mailt küldték, attól függően kisebb.

## <a name="how-do-i-access-hello-report-mentioned-in-hello-email"></a>Hogyan érhetem el az üdvözlő e-mail említett hello jelentés?
Amikor hello hivatkozásra kattint, lehetősége lesz átirányított toohello jelentés oldalról hello a klasszikus Azure portálon belül. Sorrendben tooaccess hello jelentés, mindkét toobe van szüksége:

* Rendszergazdaként vagy az Azure-előfizetéshez társadminisztrátornak
* Hello címtár globális rendszergazdája és az Active Directory Premium licenc hozzárendelése. További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).

## <a name="can-i-turn-off-these-emails"></a>Lehet kikapcsolni az e-maileket?
Igen, rendszerértesítőket tooturn kapcsolódó tooanomalous bejelentkezések hello a klasszikus Azure portálon belül, kattintson a **konfigurálása**, majd válassza ki **letiltott** alatt hello **értesítések**szakasz.

## <a name="whats-next"></a>A következő lépések
* Fejezetét milyen biztonsági, naplózási és tevékenységjelentéseket állnak rendelkezésre? Tekintse meg [az Azure AD biztonsági, naplózási és tevékenységjelentéseket](active-directory-view-access-usage-reports.md)
* [Bevezetés a Prémium szintű Azure Active Directory használatába](active-directory-get-started-premium.md)
* [Vállalati arculat megjelenítése a tooyour bejelentkezés és a hozzáférési Panel oldalon](active-directory-add-company-branding.md)

