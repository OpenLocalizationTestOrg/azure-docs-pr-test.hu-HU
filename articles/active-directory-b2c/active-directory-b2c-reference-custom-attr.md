---
title: "Az Azure Active Directory B2C: Egyéni attribútumok |} Microsoft Docs"
description: "Az egyéni attribútumok használata az Azure Active Directory B2C gyűjthet a felhasználókról"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 356aaeff3a78fc7b682d621e8e0de9312582b2fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Az Azure Active Directory B2C: Gyűjthet a felhasználókról egyéni attribútumok használata
Azure Active Directory (Azure AD) B2C-címtárban tartalmaz egy beépített információk (attribútumok): az Utónév, Vezetéknév, város, irányítószámát és egyéb attribútumai. Azonban minden egyes felhasználók felé néző alkalmazás követelményei egyedi attribútumok, mi a fogyasztói gyűjtéséhez. Az Azure AD B2C-ben kiterjesztheti az egyes felhasználói fiókjában tárolt attribútumokat. Létrehozhat egyéni attribútumok a [Azure-portálon](https://portal.azure.com/) és az előfizetési házirendek alább látható módon használhatja. Is olvasási és írási ezek az attribútumok használatával a [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Egyéni attribútumok használatát [az Azure AD Graph API Directory-séma bővítményei](https://msdn.microsoft.com/library/azure/dn720459.aspx).
> 
> 

## <a name="create-a-custom-attribute"></a>Egyéni attribútum létrehozása
1. [Kövesse az alábbi lépéseket az Azure portálon a B2C funkciók panelje navigáljon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kattintson a **felhasználói attribútumok**.
3. A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.
4. Adjon meg egy **neve** az egyéni attribútum (például "ShoeSize"), és szükség esetén egy **leírás**. Kattintson a **Create** (Létrehozás) gombra.
   
   > [!NOTE]
   > Csak a "karakterlánc" **adattípus** jelenleg rendelkezésre áll.
   > 
   > 

Az egyéni attribútum már elérhető a közül **felhasználói attribútumok**, és az előfizetési házirendek használható.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Egyéni attribútumot használja a regisztrációs szabályzatban
1. [Kövesse az alábbi lépéseket az Azure portálon a B2C funkciók panelje navigáljon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kattintson a **Regisztrálási szabályzatok** lehetőségre.
3. Kattintson a regisztrációs szabályzatban (például "B2C_1_SiUp") való megnyitásához. Kattintson a **szerkesztése** a panel tetején.
4. Kattintson a **regisztrációs attribútumokat** válassza ki az egyéni attribútum (például "ShoeSize"). Kattintson az **OK** gombra.
5. Kattintson a **alkalmazási jogcímet** , és válassza ki az egyéni attribútumot. Kattintson az **OK** gombra.
6. Kattintson a **mentése** a panel tetején.

A házirend a "Futtatás most" szolgáltatás használatával ellenőrizze a felhasználói élmény. Inkább most "ShoeSize" című felhasználói regisztráció során gyűjtött attribútumok listáját, és látja, a jogkivonat vissza az alkalmazás számára.

## <a name="notes"></a>Megjegyzések
* Előfizetési házirendek, valamint az egyéni attribútumok használhatók a regisztráció vagy bejelentkezés házirendek és Profilszerkesztési házirendek.
* Egy ismert korlátozás egyéni attribútum van. Csak jön létre, bármely házirend használatban van, és nem, amikor hozzáadja a listája **felhasználói attribútumok**.

