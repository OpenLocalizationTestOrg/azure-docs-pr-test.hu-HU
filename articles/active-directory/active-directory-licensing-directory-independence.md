---
title: "az Azure Active Directory aaaCharacteristics bérlői intercaction |} Microsoft Docs"
description: "A bérlők teljesen független erőforrásként megismerni az Azure Active bérlői bérlők kezelése"
services: active-tenant
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-tenant
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 57b677665c7cb4aee63f518c39d26754fe71a999
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Hogyan több Azure Active Directory-bérlő működjön ismertetése

Az Azure Active Directory (Azure AD), minden egyes bérlő teljesen független erőforrásként: logikailag független a társ hello más bérlők, amelyeket Ön kezel. Nincs szülő-gyermek kapcsolat bérlők között. Ez a függetlenség bérlők tartalmaz erőforrás-függetlenség, a felügyelet és a szinkronizálás függetlenségét is jelenti.

## <a name="resource-independence"></a>Erőforrás-függetlenség
* Hoz létre, vagy egy bérlői erőforrás törlése, ha nincs hatással van a valamilyen más bérlőket, a külső felhasználók hello részleges kivétellel erőforrás. 
* Ha a tartomány neveinek és egy bérlő használ, azt semmilyen más bérlővel nem használható.

## <a name="administrative-independence"></a>Felügyeleti függetlenség
Ha a "Contoso" bérlő nem rendszergazda jogosultságú felhasználók létrehoz egy tesztelési bérlőn "Test", majd:

* Alapértelmezés szerint hello létrehozó felhasználó esetleg a bérlők külső felhasználónak számít, hogy új bérlőt, és a hozzárendelt hello globális rendszergazdai szerepkör bérlőre van adva.
* Bérlői "Contoso" rendszergazdái hello nem lesznek közvetlen rendszergazdai jogosultságai tootenant "Teszt" rendelkeznek, kivéve, ha a "Teszt" rendszergazdája kifejezetten megadja számára ezeket a jogokat. Azonban a "Contoso" rendszergazdái szabályozhatja hozzáférés tootenant "Teszt" Ha irányítanak hello felhasználói fiók létrehozása a "Teszt".
* Ha Ön hozzáadása egy egy felhasználó egy Bérlői rendszergazda szerepkör, hello módosítása nem nem befolyásolják hello rendszergazdai szerepkörök adott hello felhasználó rendelkezik-e egy másik-bérlőben.

## <a name="synchronization-independence"></a>Szinkronizálási függetlenség
Beállíthatja, hogy minden Azure AD bérlői egymástól függetlenül tooget származó adatok szinkronizálásához a következők egyetlen példányából:

* hello Azure AD Connect eszközt, egyetlen AD-erdővel rendelkező toosynchronize adatokat.
* hello Azure Active bérlő összekötő a Forefront Identity Manager toosynchronize adatokat egy vagy több helyszíni erdővel és/vagy a nem Azure AD-adatforrásokhoz.

## <a name="add-an-azure-ad-tenant"></a>Az Azure AD-bérlő hozzáadása
túl bejelentkezés tooadd hello Azure-portálon az Azure AD-bérlő[hello Azure-portálon](https://portal.azure.com) az Azure AD globális rendszergazdai fiókkal, és hello bal oldali, válassza ki a **új**.

> [!NOTE]
> Egyéb Azure-erőforrásokat, eltérően a bérlők csak egy Azure-előfizetés alsóbb szintű erőforrásai. Ha az Azure-előfizetéshez megszakítva, vagy lejárt, továbbra is elérheti a bérlő adatokat az Azure PowerShell, hello Azure Graph API vagy hello Office 365 felügyeleti központban. Hello bérlővel másik előfizetést is rendelhet.
>

## <a name="next-steps"></a>Következő lépések
Az Azure AD licencelési problémák és ajánlott eljárások általános áttekintést, lásd: [Mi az Azure Active bérlői licencelése?](active-directory-licensing-whatis-azure-portal.md)
