---
title: "Az Azure Active Directory B2C: Azure Active Directory B2C-bérlő létrehozása |} Microsoft Docs"
description: "Témakör: hogyan toocreate egy Azure Active Directory B2C-bérlőben"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: patricka
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/07/2017
ms.author: swkrish
ms.openlocfilehash: e8b257d66c1f66ffb84f5d3d21b30b42eddcbac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-hello-azure-portal"></a>Hozzon létre egy Azure Active Directory B2C-bérlő hello Azure-portálon

Sipi szerkesztését.

A gyors üzembe helyezés segítséget nyújt a Microsoft Azure Active Directory (Azure AD) B2C-bérlő létrehozása néhány perc múlva. Amikor végzett, hogy a B2C bérlő toouse B2C alkalmazások regisztrálásához.

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C bérlő létrehozása

B2C funkciók nem engedélyezhetők a a meglévő bérlők számára. Azure AD B2C-bérlő toocreate van szüksége.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Gratulálunk, az Azure Active Directory B2C-bérlő hozott létre. Biztosan hello bérlő globális rendszergazdája. Szükség szerint más globális rendszergazdákat is hozzáadhat. új tooswitch tooyour bérlői, kattintson a hello *kezelése az új bérlő hivatkozás*.

![Az új bérlő hivatkozás kezelése](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Ha azt tervezi, a B2C-bérlő éles alkalmazások toouse, hello a cikk elolvasása a [éles méretű és a B2C előzetes bérlők](active-directory-b2c-reference-tenant-type.md). Vannak, ismert problémákat, ha töröl egy meglévő B2C bérlői, és hozza létre újból hello ugyanazon a néven. A B2C-bérlő egy másik tartománynév toocreate van szüksége.
>
>

## <a name="link-your-tenant-tooyour-subscription"></a>A bérlői tooyour előfizetés hivatkozás

Toolink van szüksége az Azure AD B2C bérlői tooyour Azure-előfizetés tooenable a B2C-funkciókat és használati díjat kell fizetnie. több, olvassa el az toolearn [Ez a cikk](active-directory-b2c-how-to-enable-billing.md). Ha nem az Azure AD B2C bérlő tooyour Azure-előfizetése, néhány funkció le van tiltva, és megjelenik egy figyelmeztető üzenet ("Nincs előfizetés csatolt toothis B2C-bérlőben vagy előfizetés hello kell a figyelmet.") a hello B2C beállítások. Fontos, hogy ezzel a lépéssel előtt az alkalmazások éles környezetben.

## <a name="easy-access-toosettings"></a>Könnyű hozzáférés toosettings

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Emellett hello panel megadásával `Azure AD B2C` a **keresési erőforrások** hello portal hello tetején. Hello eredmények listában válassza ki a **az Azure AD B2C** tooaccess hello B2C beállítások panelen.

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [A B2C alkalmazás regisztrálása a B2C-bérlőben](active-directory-b2c-app-registration.md)
