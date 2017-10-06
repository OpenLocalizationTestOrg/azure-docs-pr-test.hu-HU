---
title: "Az Azure Active Directory B2C: E-mail ellenőrzése során a felhasználói regisztrációs letiltása |} Microsoft Docs"
description: "A témakör bemutatásához, hogyan toodisable e-mail-ellenőrzés során az Azure Active Directory B2C-előfizetési fogyasztói"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.openlocfilehash: a8a42eddcb577725f04d70e1b1ebbebf10b5937c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Az Azure Active Directory B2C: Letiltása e-mail ellenőrzése felhasználói regisztráció során
Ha engedélyezve van, Azure Active Directory (Azure AD) B2C által biztosított a fogyasztói hello képességét toosign feliratkozott alkalmazások megadása az e-mail címet és egy helyi fiók létrehozása. Az Azure AD B2C garantálja érvényes e-mail címet azzal, hogy a fogyasztók tooverify őket hello regisztrációs folyamat során. Is megakadályozza, hogy egy rosszindulatú automatikus folyamat hello alkalmazások hamis fiókok létrehozása.

Néhány alkalmazásfejlesztők inkább tooskip e-mail ellenőrzése hello regisztrációs folyamat során, és helyette a fogyasztók hello e-mail cím ellenőrzése később rendelkezik. Ez az Azure AD B2C segítségével toosupport konfigurált toodisable e-mail ellenőrzése sikerült. Ezzel létrehoz egy gördülékenyebb bejelentkezési folyamatot, és lehetőséget nyújt a fejlesztőknek hello rugalmasságot toodifferentiate hello fogyasztók, hogy ellenőrizte az e-mail címüket a fogyasztók, amelyek nem rendelkeznek.

Alapértelmezés szerint előfizetési házirendek rendelkeznek-e kapcsolva e-mail ellenőrzése. Használjon hello következő lépések tooturn azt ki:

1. [Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kattintson a **előfizetési házirendek** vagy **regisztráció vagy bejelentkezés házirendek** attól függően, hogy a konfigurált előfizetési.
3. Kattintson a házirendet (például "B2C_1_SiUp") tooopen azt. Kattintson a **szerkesztése** hello panel hello tetején.
4. Kattintson a **lap felhasználói felületének testreszabása**.
5. Kattintson a **helyi fiók előfizetéshez**.
6. Kattintson a **E-mail cím** a hello **neve** alatti hello **regisztrációs attribútumokat** szakasz.
7. Váltás hello **ellenőrzés megkövetelése** beállítás túl**nem**.
8. Kattintson a **OK** hello alsó hello addig **házirend szerkesztése** panelen.
9. Kattintson a **mentése** hello panel hello tetején. Elkészült!

> [!NOTE]
> E-mail ellenőrzése a regisztrációs folyamat hello letiltása toospam vezethet. Ha letiltja a hello alapértelmezett, ajánlott hozzáadása a saját ellenőrzési rendszerétől.
> 
> 

Azt a rendszer mindig megnyitott toofeedback és javaslatok! Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit hello lap hello alján. A szolgáltatás kéréseket, vegye fel őket túl[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
