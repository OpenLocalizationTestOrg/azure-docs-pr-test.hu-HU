---
title: "Az Azure Active Directory B2C: Az önkiszolgáló jelszó-átállítási |} Microsoft Docs"
description: "A témakör bemutatásához, hogyan tooset fel az önkiszolgáló jelszó alaphelyzetbe állítása a felhasználók az Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: ff87eea42259b610702da73af35d0a38eb7dd33d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Az Azure Active Directory B2C: Az önkiszolgáló jelszó-visszaállítást a felhasználók beállítása
Hello az önkiszolgáló jelszó-átállítási funkció, a felhasználók (akiknek regisztráltak-e helyi fiókok esetében) alaphelyzetbe állíthatja a saját maguk a jelszavukat. Ez jelentősen csökkenti a hello nehezedő a támogató személyzete számára, különösen akkor, ha az alkalmazás rendelkezik több millió fogyasztók rendszeresen használja azt. Jelenleg csak támogatjuk egy helyreállítási módszer az egy ellenőrzött és érvényes e-mail címet. Hello jövőben további helyreállítási módszerek (ellenőrzött telefonszám, a biztonsági kérdések stb.) adunk hozzá.

> [!NOTE]
> Ez a cikk vonatkozik tooself-jelszavának alaphelyzetbe állítása a bejelentkezési házirend hello környezetben használják. Ha módosítania kell meghívni a az alkalmazás teljes mértékben testreszabható jelszó alaphelyzetbe állítása házirendek, lásd: [Ez a cikk](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Alapértelmezés szerint a címtárban nincs önkiszolgáló jelszó-visszaállítási-e kapcsolva. Használjon hello következő lépések tooturn azt a:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello előfizetési rendszergazda. Ez az azonos munkahelyi vagy iskolai fiókkal, vagy ugyanazon Microsoft-fiókkal, amellyel toocreate a címtár hello hello.
2. Keresse meg a hello bal oldali navigációs sávban hello toohello Active Directory-bővítményt.
3. A címtár hello alatt található **Directory** fülre, és kattintson rá.
4. Kattintson a hello **konfigurálása** fülre.
5. Görgessen lefelé toohello **felhasználói jelszó-visszaállítási házirend** szakasz és váltása hello **jelszó-visszaállításhoz engedélyezett felhasználók** beállítás túl**Igen**. Figyelje meg, hogy hello **másodlagos E-mail-cím** beállítást; hagyja, mert az.
   
    ![Új jelszó önkiszolgáló kérése](./media/active-directory-b2c-reference-sspr/sspr.png)
6. Kattintson a **mentése** hello lap hello alján. Elkészült!

tootest, hello "Futtatás most" szolgáltatás bármely bejelentkezési házirend, amely rendelkezik a helyi fiókok identitás-szolgáltatóként. Hello helyi fiók bejelentkezhet a lapon (ha ad meg egy e-mail címet és jelszót, vagy felhasználónév és jelszó), kattintson **nem fér hozzá a fiókjához?** tooverify hello végfelhasználói élmény.

> [!NOTE]
> hello önkiszolgáló jelszó-visszaállítási oldal testreszabható hello segítségével [vállalati arculat megjelenítése a szolgáltatás](../active-directory/active-directory-add-company-branding.md).
> 
> 

