---
title: "Az Azure Active Directory B2C: Egyéni attribútumok |} Microsoft Docs"
description: "Hogyan toouse egyéni attribútumok a toocollect Azure Active Directory B2C a felhasználókról"
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
ms.openlocfilehash: fb1bff77ad54c246c6d2f69f39c03eafb76fe6bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-toocollect-information-about-your-consumers"></a>Az Azure Active Directory B2C: Toocollect egyéni attribútumok a felhasználókról használata
Azure Active Directory (Azure AD) B2C-címtárban tartalmaz egy beépített információk (attribútumok): az Utónév, Vezetéknév, város, irányítószámát és egyéb attribútumai. Minden egyes felhasználók felé néző alkalmazás azonban egyedi követelményeket támaszt a fogyasztói milyen attribútumok toogather. Az Azure AD B2C-ben minden felhasználói fiókhoz tárolt attribútumok készletét hello bővítheti. Létrehozhat egyéni attribútumok hello [Azure-portálon](https://portal.azure.com/) és az előfizetési házirendek alább látható módon használhatja. Is olvasási és írási ezek az attribútumok hello segítségével [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Egyéni attribútumok használatát [az Azure AD Graph API Directory-séma bővítményei](https://msdn.microsoft.com/library/azure/dn720459.aspx).
> 
> 

## <a name="create-a-custom-attribute"></a>Egyéni attribútum létrehozása
1. [Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kattintson a **felhasználói attribútumok**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy **neve** hello egyéni attribútum (például "ShoeSize"), és szükség esetén egy **leírás**. Kattintson a **Create** (Létrehozás) gombra.
   
   > [!NOTE]
   > Csak a "String" hello **adattípus** jelenleg rendelkezésre áll.
   > 
   > 

hello egyéni attribútum már elérhető a hello listája **felhasználói attribútumok**, és az előfizetési házirendek használható.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Egyéni attribútumot használja a regisztrációs szabályzatban
1. [Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kattintson a **Regisztrálási szabályzatok** lehetőségre.
3. Kattintson a regisztrációs szabályzatban (például "B2C_1_SiUp") tooopen azt. Kattintson a **szerkesztése** hello panel hello tetején.
4. Kattintson a **regisztrációs attribútumokat** és select hello egyedi attribútum (például "ShoeSize"). Kattintson az **OK** gombra.
5. Kattintson a **alkalmazási jogcímet** és select hello egyéni attribútum. Kattintson az **OK** gombra.
6. Kattintson a **mentése** hello panel hello tetején.

Hello házirend tooverify hello végfelhasználói élmény hello "Futtatás most" funkciót is használja. Kell most "ShoeSize" című felhasználói regisztráció során gyűjtött attribútumok hello listáját, és látja, hello token küldött vissza tooyour alkalmazás.

## <a name="notes"></a>Megjegyzések
* Előfizetési házirendek, valamint az egyéni attribútumok használhatók a regisztráció vagy bejelentkezés házirendek és Profilszerkesztési házirendek.
* Egy ismert korlátozás egyéni attribútum van. Csak a létrehozott hello használják első alkalommal a házirendet, és nem Ha hozzáadja toohello listája **felhasználói attribútumok**.

