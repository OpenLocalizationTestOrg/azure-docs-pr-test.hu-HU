---
title: "az Azure MFA és az AD FS aaaSecure felhőalapú erőforrások |} Microsoft Docs"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan tooget lépések az Azure MFA és az AD FS hello felhőben."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: kgremban
ms.openlocfilehash: 8d38d6a4af63ddcaf0fefded0b73d82d5178aa36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>A felhőerőforrások védelme Azure Multi-Factor Authentication hitelesítéssel és AD FS-sel
Ha a szervezet össze van vonva az Azure Active Directory, használja az Azure multi-factor Authentication vagy Active Directory összevonási szolgáltatások (AD FS) toosecure erőforrások az Azure AD által elért. A következő eljárások toosecure Azure Active Directory-erőforrások Azure multi-factor Authentication vagy Active Directory összevonási szolgáltatások hello használata.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Az Azure AD-erőforrások AD FS-sel való védelme
toosecure a felhő erőforrás olyan jogcímeket szabályt beállítani, hogy az Active Directory összevonási szolgáltatások hello multipleauthn jogcím bocsát ki, amikor a felhasználó sikeresen elvégzi a kétlépéses ellenőrzést. A jogcím átadódik tooAzure AD. Ez az eljárás toowalk hello lépéseit követve:


1. Nyissa meg az AD FS felügyeleti konzolt.
2. Hello bal oldalon válassza ki a **függő entitás Megbízhatóságai**.
3. Kattintson a jobb gombbal a **Microsoft Office 365 Identity Platform** elemre, és válassza a **Jogcímszabályok szerkesztése** lehetőséget.

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** elemre.

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. Az átalakítási Jogcímszabály szabály varázsló hello, válassza a **továbbítása vagy szűrése egy bejövő jogcím** hello legördülő listán, és kattintson **következő**.

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. Adjon nevet a szabálynak. 
7. Válassza ki **hitelesítési módszerek hivatkozásai** , hello bejövő jogcím típusa.
8. Válassza **Az összes jogcímérték továbbítása** lehetőséget.
    ![Átalakítási jogcímszabály hozzáadása varázsló](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)
9. Kattintson a **Befejezés** gombra. Zárja be a hello AD FS felügyeleti konzolon.

## <a name="trusted-ips-for-federated-users"></a>Megbízható IP-címek összevont felhasználók számára
Megbízható IP-címek lehetővé teszik a rendszergazdák tooby-fázis kétlépéses ellenőrzés bizonyos IP-címeket vagy összevont felhasználók, amelyek a saját intraneten belüli származó kérelmek. hello alábbi szakaszok azt ismertetik, hogyan tooconfigure Azure multi-factor Authentication megbízható IP-címeket az összevont felhasználók és kihagyva kétlépéses ellenőrzést, ha a kérelem származási egy összevont felhasználók intraneten belül. Ez úgy konfigurálja az AD FS toouse csatlakoztatott vagy szűrő egy bejövő jogcím-sablont a vállalati hálózaton belül jogcímtípus hello érhető el.

Ez a példa az Office 365-öt használja a függőentitás-megbízhatóságokhoz.

### <a name="configure-hello-ad-fs-claims-rules"></a>Az AD FS hello jogcímek szabályok konfigurálása
hello első lépésként szükségünk van toodo tooconfigure hello AD FS-jogcímek. Két jogcímek szabályt hoz létre, egy a vállalati hálózaton belül hello jogcím típusokat, hogy a bejelentkezett felhasználók a további egyikét.

1. Nyissa meg az AD FS felügyeleti konzolt.
2. Hello bal oldalon válassza ki a **függő entitás Megbízhatóságai**.
3. Középen kattintson a jobb gombbal a **Microsoft Office 365 Identity Platform** elemre, és válassza a **Jogcímszabályok szerkesztése…** lehetőséget.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** lehetőségre.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Az átalakítási Jogcímszabály szabály varázsló hello, válassza a **továbbítása vagy szűrése egy bejövő jogcím** hello legördülő listán, és kattintson **következő**.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Hello mezőben következő tooClaim szabály neve nevezze el a szabályt. Például: InsideCorpNet.
7. Hello legördülő listán, a következő tooIncoming jogcím-típust, jelölje be **vállalati hálózaton belül**.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Kattintson a **Finish** (Befejezés) gombra.
9. A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** elemre.
10. Az átalakítási Jogcímszabály szabály varázsló hello, válassza a **jogcímek küldése egyéni szabály segítségével** hello legördülő listán, és kattintson **következő**.
11. Jogcímszabály neve alatt hello mezőben: Adja meg *tartsa felhasználók aláírt a*.
12. Hello egyéni szabály mezőben adja meg:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Kattintson a **Befejezés** gombra.
14. Kattintson az **Alkalmaz** gombra.
15. Kattintson az **OK** gombra.
16. Zárja be az AD FS felügyeleti konzolt.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Azure Multi-Factor Authentication megbízható IP-címeinek konfigurálása összevont felhasználókkal
Most, hogy hello jogcímek helyen, konfigurálhatjuk megbízható IP-címek.

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldalon kattintson **Active Directory**.
3. Könyvtárban jelölje ki a kívánt tooset megbízható IP-címek fel hello címtárát.
4. A kijelölt könyvtár hello, kattintson **konfigurálása**.
5. Hello többtényezős hitelesítés területen kattintson **szolgáltatás beállításainak kezelése**.
6. Hello szolgáltatás beállításai lapon a megbízható IP-címek, jelölje be **több-factor – hitelesítési kérelmek kihagyása az összevont felhasználók intranetről**.  

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. Kattintson a **mentés** gombra.
8. Amikor hello frissítések telepítve lettek, kattintson a **bezárása**.

Készen is van. Ezen a ponton összevont Office 365 felhasználóknak csak rendelkezniük kell toouse többtényezős Hitelesítést, amikor a jogcím külső hello vállalati intranethez származik.
