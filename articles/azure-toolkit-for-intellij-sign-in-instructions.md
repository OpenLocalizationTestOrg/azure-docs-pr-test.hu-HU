---
title: "Bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ |} Microsoft Docs"
description: "Útmutató az intellij-t az Azure-eszközkészlet használatával a Microsoft Azure bejelentkezni."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 4e2ed072bdaea0a71fef042c0c72b7656a42bbe8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t

Az Azure-eszközkészlet az intellij-t az Azure-fiókjába történő bejelentkezés két módszert biztosít:

  * **Interaktív**: Azure hitelesítő adatait az Azure-fiókjával bejelentkezik minden alkalommal megadniuk.
  * **Automatikus**: hoz létre egy hitelesítőadat-fájlt, amely segítségével automatikusan jelentkezzen be az Azure-fiókjával.

Az alábbi szakaszok azt ismertetik, hogyan minden módszer használatát.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-interactively"></a>Interaktív bejelentkezés az Azure-fiókjába

Jelentkezzen be Azure hitelesítő adatait az Azure manuális megadásával, tegye a következőket:

1. Nyissa meg a projektet az IntelliJ IDEA.

2. Kattintson a **eszközök**, mutasson a **Azure**, és kattintson a **Azure bejelentkezés**.

   ![Az intellij-t Azure bejelentkezés parancs][I01]

3. Az a **Azure bejelentkezés** ablakban válassza ki **interaktív**, és kattintson a **bejelentkezés**.

   ![A kiválasztott interaktív Azure bejelentkezés ablakot][I02]

4. Az a **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![Az Azure bejelentkezési párbeszédpanel][I03]

5. Az a **előfizetések kiválasztása** párbeszédpanel megnyitásához, válassza ki a használni kívánt előfizetést, és végül **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a>Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett interaktív módon

Miután konfigurálta a fiókját a fenti lépések segítségével, akkor rendszer automatikusan kijelentkezteti az Azure-fiókjával IntelliJ IDEA minden újraindításakor. Azonban hogy szeretné-e az Azure-fiókjával kijelentkezni *nélkül* újraindítása IntelliJ IDEA, tegye a következőket.

1. Az IntelliJ IDEA a a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure Kijelentkezés**.

   ![Az intellij-t Azure Kijelentkezés parancs][L01]

2. Az a **Azure Kijelentkezés** ablak, kattintson a **Igen**.

   ![Az Azure kijelentkezés ablak][L02]

## <a name="sign-in-to-your-azure-account-automatically"></a>Automatikus bejelentkezés az Azure-fiókjába

Ez a szakasz végigvezeti egy hitelesítő adatait tartalmazó fájlt, a szolgáltatás egyszerű adatainak létrehozása. Ez a folyamat befejezését követően eclipse-ben a hitelesítőadat-fájlt használatával automatikusan bejelentkezés az Azure a projekt minden egyes megnyitásakor.

1. Nyissa meg a projektet az IntelliJ IDEA.

2. Az a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure bejelentkezés**.

   ![Az intellij-t Azure bejelentkezés parancs][A01]

3. Az a **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **új**.

   ![A kiválasztott automatikus Azure bejelentkezés ablakot][A02]

4. Az a **Azure bejelentkezési párbeszédpanel** ablakban adja meg Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![Az Azure bejelentkezési párbeszédpanel][A03]

5. A a **hitelesítési fájlok létrehozása** ablakban válassza ki, amelyet szeretne használni, válassza ki a célkönyvtárat, és kattintson az előfizetések **Start**.

   ![A hitelesítés-fájlok létrehozása ablak][A04]

6. Az a **egyszerű szolgáltatás létrehozása állapota** párbeszédpanel, miután a fájlok sikeresen létrejött, kattintson a **OK**.

   ![Az egyszerű szolgáltatás létrehozása párbeszédpanel][A05]

7. Az a **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A06]

8. Az a **előfizetések kiválasztása** párbeszédpanel megnyitásához, válassza ki a használni kívánt előfizetést, és végül **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a>Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett automatikusan

Miután konfigurálta a fiókját a fenti lépések használatával, az Azure-eszközkészlet automatikusan bejelentkezik, az Azure-fiókjával IntelliJ IDEA minden újraindításakor. Azonban jelentkezzen ki az Azure-fiókjával, és az Azure-eszközkészlet megakadályozza az automatikus bejelentkezés, tegye a következőket:

1. Az IntelliJ IDEA a a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure Kijelentkezés**.

   ![Az intellij-t Azure Kijelentkezés parancs][L01]

2. Az a **Azure Kijelentkezés** ablak, kattintson a **Igen**.

   ![Az Azure kijelentkezés ablak][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a>Jelentkezzen be az Azure-fiókjával automatikusan meglévő hitelesítő adatok fájl segítségével

IntelliJ IDEA használatakor kijelentkezni az Azure-fiókjával, ha egy meglévő hitelesítő adatok fájl automatikusan jelentkezzen be újra a a fiókot kell használnia. A meglévő hitelesítő adatok fájl az eclipse-ben az Azure eszközkészlet konfigurálásához tegye a következőket:

1. Nyissa meg a projektet az IntelliJ IDEA.

2. Az a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure bejelentkezés**.

   ![Az intellij-t Azure bejelentkezés parancs][A01]

3. Az a **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **Tallózás**.

   ![A kiválasztott automatikus Azure bejelentkezés ablakot][A02]

4. Az a **hitelesítési fájl kiválasztása** párbeszédpanelen válassza ki a korábban létrehozott hitelesítő adatokat tartalmazó fájlt, és kattintson **válasszon**.

   ![A hitelesítési fájl kiválasztása párbeszédpanel][A08]

5. Az a **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.

   ![A kiválasztott automatikus Azure bejelentkezés ablakot][A06]

6. Az a **előfizetések kiválasztása** párbeszédpanel megnyitásához, válassza ki a használni kívánt előfizetést, és végül **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="next-steps"></a>Következő lépések
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse Azure eszköztára]
  * [What's new in Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * [Az Eclipse Azure eszköztára bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in IntelliJ Azure eszköztára]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * *Bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ* (Ez a cikk)
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Az Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
