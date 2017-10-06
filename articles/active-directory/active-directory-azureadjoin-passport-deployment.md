---
title: "aaaEnable Microsoft vállalati Windows Hello a szervezet |} Microsoft Docs"
description: "Központi telepítési utasításokat tooenable Microsoft Passport a szervezetében."
services: active-directory
documentationcenter: 
keywords: "a Microsoft Passport, Microsoft Windows Hello-üzleti központi telepítés konfigurálása"
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: 6041f5916f606752bc55844b1b2d0a423b913cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Engedélyezze a Microsoft vállalati Windows Hello a szervezetben
Miután [Windows 10-tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure Active Directoryval](active-directory-azureadjoin-devices-group-policy.md), Microsoft Windows Hello for Business tooenable a szervezet következő hello:

1. A System Center Configuration Manager központi telepítése  
2. Házirend-beállítások konfigurálása
3. Hello tanúsítványprofil konfigurálása  

## <a name="deploy-system-center-configuration-manager"></a>A System Center Configuration Manager központi telepítése
a Windows Hello üzleti kulcsok alapján toodeploy felhasználói tanúsítvány következőkre lesz szüksége hello:

* **A System Center Configuration Manager aktuális ágának** -1606 vagy jobb tooinstall verziója szükséges. További információkért lásd: hello [dokumentáció a System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) és [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
* **Nyilvános kulcsokra épülő infrastruktúra (PKI)** -tooenable Microsoft Windows Hello üzleti felhasználói tanúsítványok használatával, rendelkeznie kell egy nyilvános kulcsokra épülő infrastruktúra helyen. Ha még nincs fiókja, vagy nem szeretné, hogy toouse azt a felhasználói tanúsítványok telepítése egy új tartományvezérlő, amely a Windows Server 2016-os build 10551 (vagy nagyobb frekvenciával) telepítve van. Hello lépésekkel túl[replika tartományvezérlő telepítése meglévő tartományban](https://technet.microsoft.com/library/jj574134.aspx) vagy túl[egy új Active Directory-erdő telepítéséhez, ha hoz létre egy új környezetben](https://technet.microsoft.com/library/jj574166). (hello ISO letölthetők a [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)

## <a name="configure-policy-settings"></a>Házirend-beállítások konfigurálása
Microsoft Windows Hello tooconfigure hello házirend-beállítások, két lehetőség közül választhat:

* Az Active Directory csoportházirend 
* hello System Center Configuration Managerben 

Az Active Directoryban a csoportházirendekkel üzleti házirend-beállítások a Microsoft Windows Hello metódus tooconfigure ajánlott hello. 

A System Center Configuration Manager hello előnyben részesített módszert is használata esetén azt toodeploy tanúsítványokat. Ebben a forgatókönyvben:

* Biztosítja a kompatibilitást hello újabb történő telepítések esetén
* Hello ügyféloldali Windows 10-es verzió 1607 vagy nagyobb frekvenciával igényel.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Konfigurálja a Microsoft vállalati Windows Hello az Active Directory csoportházirend segítségével
**Lépéseket**:

1. Nyissa meg a Kiszolgálókezelőt, és keresse meg a túl**eszközök** > **csoportházirend-kezelő**.
2. A Csoportházirend kezelése keresse meg a használni kívánt Azure AD Join tooenable toohello tartományhoz tartozó tartományi csomópontot toohello.
3. Kattintson a jobb gombbal **csoportházirend-objektumok**, és válassza ki **új**. Nevezze el a csoportházirend-objektum, például engedélyezése vállalati Windows Hello. Kattintson az **OK** gombra.
4. Kattintson a jobb gombbal az új csoportházirend-objektumot, majd válassza ki **szerkesztése**.
5. Keresse meg a túl**számítógép konfigurációja** > **házirendek** > **felügyeleti sablonok** > **Windows Összetevők** > **a vállalati Windows Hello**.
6. Kattintson a jobb gombbal **engedélyezése vállalati Windows Hello**, majd válassza ki **szerkesztése**.
7. Jelölje be hello **engedélyezve** választógomb, és kattintson a **alkalmaz**. Kattintson az **OK** gombra.
8. Most hozzákapcsolhatja hello csoportházirend-objektum tooa tetszőleges helyre. tooenable ezt a házirendet az összes hello tartományhoz csatlakoztatott Windows 10-eszközök a szervezetben, hivatkozás hello csoportházirend toohello tartomány. Példa:
   * Egy adott szervezeti egység (OU) az Active Directoryban, ha Windows 10-es tartományhoz csatlakoztatott számítógépek lesznek elhelyezve
   * Egy adott biztonsági csoport, amely tartalmazza a Windows 10-tartományhoz csatlakoztatott számítógépeket, amelyek automatikusan regisztrálva lesz az Azure ad szolgáltatással

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>A System Center Configuration Manager vállalati Windows Hello konfigurálása
**Lépéseket**:

1. Megnyitás hello **System Center Configuration Manager**, és keresse meg a túl**eszközök és megfelelőség > megfelelőségi beállítások > Vállalati erőforrások elérése > üzleti profilok a Windows Hello**.
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. Hello hello felső eszköztárán kattintson **vállalati profil létrehozása a Windows Hello**.
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. A hello **általános** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    a. A hello **neve** szövegmező, írja be például a profil nevét **WHfB profil**.
   
    b. Kattintson a **Tovább** gombra.
4. A hello **által támogatott platformok** párbeszédpanelen jelölje be hello platformokat, amelyek megkapják a Windows Hello üzleti profil, és kattintson a **következő**.
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. A hello **beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    a. Mint **konfigurálása vállalati Windows Hello**, jelölje be **engedélyezve**.
   
    b. Mint **egy platformmegbízhatósági modul (TPM) használata**, jelölje be **szükséges**. 
   
    c. Mint **hitelesítési módszer**, jelölje be **tanúsítványalapú**.
   
    d. Kattintson a **Tovább** gombra.
6. A hello **összegzés** párbeszédpanel, kattintson a **következő**.
7. A hello **befejezési** párbeszédpanel, kattintson a **Bezárás**.
8. Hello hello felső eszköztárán kattintson **telepítés**.
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-hello-certificate-profile"></a>Hello tanúsítványprofil konfigurálása
Ha a tanúsítvány alapú hitelesítést használ a helyszíni hitelesítéshez, tooconfigure kell, és tanúsítványprofil központi telepítése. Ez a feladat egy NDES-kiszolgáló és a Tanúsítványregisztrációs pont helyrendszer-szerepkör a System Center Configuration Manager hello tooset igényel. További részletekért lásd: hello [előfeltételei a Configuration Manager Tanúsítványprofiljaihoz](https://technet.microsoft.com/library/dn261205.aspx).

1. Megnyitás hello **System Center Configuration Manager**, majd keresse meg a túl**eszközök és megfelelőség > megfelelőségi beállítások > Vállalati erőforrások elérése > Tanúsítványprofilok**.
2. Válasszon olyan sablont, amely rendelkezik az intelligens kártyás bejelentkezési kibővített kulcshasználat (EKU).

A hello **SCEP-igénylés** lap hello tanúsítványprofilt, akkor kell toochoose **tooPassport for Work szolgáltatásba, másként meghibásodás történik telepítése** hello, **kulcstároló-szolgáltató**.

## <a name="next-steps"></a>Következő lépések
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

