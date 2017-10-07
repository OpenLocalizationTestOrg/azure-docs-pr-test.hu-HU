---
title: "Biztonságos LDAP (LDAPS) az Azure AD tartományi szolgáltatásokban aaaConfigure |} Microsoft Docs"
description: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 356b28f8392b0e203df9c81177ec842d52866c4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása

## <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy befejezte [1. feladat – tanúsítvány beszerzése biztonságos LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="task-2---export-hello-secure-ldap-certificate-tooa-pfx-file"></a>2. feladat – hello biztonságos LDAP tanúsítvány tooa exportálása. PFX-fájlból
Ez a feladat megkezdése előtt győződjön meg arról, beszerezte a hello biztonságos LDAP tanúsítványt nyilvános hitelesítésszolgáltatótól származó, vagy önaláírt tanúsítványt hozott létre.

Hajtsa végre az alábbi lépésekkel hello, tooexport hello LDAPS tooa tanúsítvány. PFX-fájlt.

1. Nyomja le az hello **Start** gombra, és írja be **R**. A hello **futtatása** párbeszédpanel, írja be **mmc** kattintson **OK**.

    ![Hello MMC konzol indítása](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. A hello **felhasználói fiókok felügyelete** kérdés, kattintson a **Igen** toolaunch MMC (Microsoft Management Console) rendszergazdaként.
3. A hello **fájl** menüben kattintson a **beépülő modul hozzáadása/eltávolítása...** .

    ![Adja hozzá a beépülő modul tooMMC konzol](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. A hello **hozzáadása vagy eltávolítása a beépülő modulok** párbeszédpanelen jelölje be hello **tanúsítványok** beépülő modul hello kattintson **Hozzáadás >** gombra.

    ![Adja hozzá a tanúsítványok beépülő modul tooMMC konzol](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. A hello **tanúsítványok beépülő modul** varázslóban válassza **számítógépfiók** kattintson **következő**.

    ![Tanúsítványok beépülő modul számítógép fiók hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. A hello **számítógép kijelölése** lapon, hogy melyik **helyi számítógép: (hello számítógépet a konzol fut)** kattintson **Befejezés**.

    ![Tanúsítványok beépülő modul – jelölje be a számítógép hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. A hello **hozzáadása vagy eltávolítása a beépülő modulok** párbeszédpanel, kattintson a **OK** tooadd hello tanúsítványok beépülő modul tooMMC.

    ![Adja hozzá a tanúsítványok beépülő modul tooMMC - kész](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. Hello MMC ablak, kattintson a tooexpand **konzolgyökér**. Meg kell jelennie a tanúsítványok beépülő modul hello betöltve. Kattintson a **tanúsítványok (helyi számítógép)** tooexpand. Kattintson a tooexpand hello **személyes** csomópontot, majd hello **tanúsítványok** csomópont.

    ![Nyissa meg személyes tanúsítványok tárolójában](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Hello létrehozott önaláírt tanúsítványt kell megjelennie. Láthatja, hogy hello tulajdonságainak hello tanúsítvány tooensure hello ujjlenyomat megfelelő találat jelentett hello PowerShell windows hello-tanúsítvány létrehozásakor.
10. Válassza ki az önaláírt tanúsítvány hello és **kattintson a jobb gombbal**. Hello helyi menüből válassza ki a **feladataival** válassza **exportálása...** .

    ![Tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. A hello **Tanúsítványexportáló varázsló**, kattintson a **következő**.

    ![Exportálja a tanúsítványt varázsló](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. A hello **titkos kulcs exportálása** lapon jelölje be **Igen, hello titkos kulcs exportálását választom**, és kattintson a **következő**.

    ![Tanúsítvány titkos kulcs exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > Exportálnia kell a titkos kulcs hello hello tanúsítvány együtt. Ha megad egy PFX hello hello tanúsítvány titkos kulcsa nem tartalmazó, a felügyelt tartományok biztonságos LDAP engedélyezése sikertelen.
    >
    >
13. A hello **Exportfájlformátum** lapon jelölje be **személyes információcsere - PKCS #12 (. PFX)** hello hello formátumban exportálja a tanúsítványt.

    ![A tanúsítvány fájlformátuma exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > Csak hello. PFX-fájl formátuma támogatott. Hello tanúsítvány toohello nem exportálja. CER-fájlformátum.
    >
    >
14. A hello **biztonsági** lapra, jelölje be hello **jelszó** lehetőséget és a jelszó tooprotect hello írja be. PFX-fájlt. Ne felejtse el ezt a jelszót, mert a következő feladathoz hello lesz szükség. Kattintson a **következő** tooproceed.

    ![A Tanúsítványexportálás jelszó ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Jegyezze fel ezt a jelszót. A felügyelt tartomány biztonságos LDAP engedélyezése során szüksége [3. feladat – hello felügyelt tartomány biztonságos LDAP engedélyezése](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >
15. A hello **fájl tooExport** hello nevét és helyét, hol szeretné tooexport hello tanúsítvány adja meg azokat.

    ![A Tanúsítványexportálás elérési útja](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Hello a következő lapon kattintson **Befejezés** tooexport hello tooa PFX tanúsítványfájl. Hello tanúsítvány exportálásakor meg kell jelennie a megerősítő párbeszédpanelen.

    ![Kész tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a>Következő lépés
[3. feladat – hello felügyelt tartomány biztonságos LDAP engedélyezése](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
