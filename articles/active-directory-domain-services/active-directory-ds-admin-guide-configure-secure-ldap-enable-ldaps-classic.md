---
title: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatásokban |} Microsoft Docs"
description: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3aafe209aad7383cd0610d147b5fdba673023c93
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása

## <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy befejezte [2. feladat – a biztonságos LDAP tanúsítvány exportálása a. PFX-fájl](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).

Válassza ki, hogy az Azure portál felhasználói élmény előzetes vagy a klasszikus Azure portálon segítségével befejezheti a feladatot.
> [!div class="op_single_selector"]
> * **Azure-portálon (előzetes verzió)**: [engedélyezése biztonságos LDAP az Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
> * **A klasszikus Azure portálon**: [engedélyezése biztonságos LDAP a klasszikus Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal"></a>3. feladat – a felügyelt tartomány, a klasszikus Azure portál használatával biztonságos LDAP engedélyezése
Ahhoz, hogy a biztonságos LDAP, hajtsa végre az alábbi konfigurációs lépéseket:

1. Keresse meg a  **[a klasszikus Azure portálon](https://manage.windowsazure.com)**.
2. Válassza az **Active Directory** csomópontot a bal oldali panelen.
3. Válassza ki az Azure AD-címtár (más néven "tenant"), amelyhez engedélyezte az Azure AD tartományi szolgáltatásokat.

    ![Azure AD címtár kiválasztása](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Kattintson a **Configure** (Konfigurálás) lapra.

    ![Címtár lapjának konfigurálása](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Görgessen le a című **tartományi szolgáltatások**. Megjelenik egy lehetőség, című **biztonságos LDAP (LDAPS)** az alábbi képernyőfelvételen látható módon:

    ![Tartományi szolgáltatások konfigurációja szakasz](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. Kattintson a **tanúsítvány konfigurálása...**  gombra kattintva nyissa meg a **tanúsítvány konfigurálása a biztonságos LDAP** párbeszédpanel.

    ![Biztonságos LDAP-tanúsítvány konfigurálása](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. Kattintson a mappa ikon következő **PFX-fájl a tanúsítvány** adhatja meg a PFX-fájlt, amely tartalmazza a felügyelt tartományra biztonságos LDAP eléréséhez használni kívánt tanúsítványt. Ha a tanúsítvány exportálása PFX-fájl a megadott jelszó is beírhatja. Kattintson az alul a Kész gombra.

    ![Biztonságos LDAP PFX-fájlt és a jelszó megadása](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. A **tartományi szolgáltatások** szakasza a **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a **folyamatban...**  állapot néhány percig. Ebben az időszakban a LDAPS tanúsítvány pontosságát ellenőrizve, és biztonságos LDAP konfigurálva van a felügyelt tartományok.

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > Ahhoz, hogy a felügyelt tartományok biztonságos LDAP körülbelül 10 – 15 percet vesz igénybe. A megadott biztonságos LDAP-tanúsítvány nem felel meg a szükséges feltételeknek, ha biztonságos LDAP nincs engedélyezve a címtárban, és láthatja a hibát. Ha például a tartomány neve nem megfelelő, a tanúsítvány már lejárt vagy hamarosan lejár.
   >
   >

9. Ha a felügyelt tartományok sikeresen engedélyezve a biztonságos LDAP a **folyamatban...**  üzenet kell eltűnik. Meg kell jelennie jelenik meg a tanúsítvány ujjlenyomatát.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>4. feladat – az interneten keresztül biztonságos LDAP-hozzáférés engedélyezése
**Nem kötelező** – Ha nem tervezi, hogy a felügyelt tartományra LDAPS az interneten keresztül elérni, hagyja ki a konfigurációs feladat.

Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).

1. Megjelenik egy lehetőség, hogy **ENGEDÉLYEZNI biztonságos LDAP hozzáférés keresztül interneten keresztül** a a **tartományi szolgáltatások** szakasza a **konfigurálása** lap. Ez a beállítás értéke **nem** mivel internet-hozzáféréssel a felügyelt tartományra keresztül biztonságos LDAP alapértelmezés szerint le van tiltva alapértelmezés szerint.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. Váltás **biztonságos LDAP-hozzáférés engedélyezése az INTERNETEN keresztül** való **Igen**. Kattintson a **mentése** gomb alsó panelje.
    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)
3. A **tartományi szolgáltatások** szakasza a **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a **folyamatban...**  állapot néhány percig. Némi várakozás után internet-hozzáféréssel a felügyelt tartományra keresztül biztonságos LDAP engedélyezve van.

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > Internet-hozzáférés engedélyezése a felügyelt tartományok biztonságos LDAP körülbelül 10 percig tart.
   >
   >
4. Ha az interneten keresztül a felügyelt tartományra biztonságos LDAP hozzáférés sikeresen engedélyezve van, a **folyamatban...**  üzenet kell eltűnik. A külső IP-cím, a mezőben LDAPS keresztül a könyvtár eléréséhez használható kell megjelennie **külső IP-cím a LDAPS ELÉRÉST**.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>5. feladat – az internetről a felügyelt tartomány eléréséhez DNS konfigurálása
**Nem kötelező** – Ha nem tervezi, hogy a felügyelt tartományra LDAPS az interneten keresztül elérni, hagyja ki a konfigurációs feladat.

Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).

Miután engedélyezte biztonságos LDAP hozzáférést az interneten keresztül a felügyelt tartományok, módosítania DNS, hogy a felügyelt tartomány található ügyfélszámítógépek számára. 4. feladat végén külső IP-cím jelenik meg a **konfigurálása** lapján **külső IP-cím a LDAPS ELÉRÉST**.

Konfigurálja a külső DNS-szolgáltatónál, hogy a külső IP-címre mutat a felügyelt tartomány (például ldaps.contoso100.com) DNS-nevét. A jelen példában kell a következő DNS-bejegyzés létrehozása:

    ldaps.contoso100.com  -> 52.165.38.113

Ennyi az egész – most már készen áll a felügyelt tartományra biztonságos LDAP az interneten keresztül csatlakozni.

> [!WARNING]
> Ne feledje, hogy az ügyfél számára megbízhatónak kell lennie a LDAPS tanúsítvány kiállítója tud sikeresen csatlakozni a felügyelt tartományra LDAPS kell. Ha egy vállalati hitelesítésszolgáltatótól vagy egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell semmit, mivel az ügyfélszámítógépek számára megbízhatónak ezek tanúsítványkiállítók. Ha önaláírt tanúsítványt használ, telepítendő önaláírt tanúsítvány nyilvános részét az ügyfélszámítógépen, a megbízható tanúsítványok tárolójába.
>
>


## <a name="lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a>Zárolási-le nyilas LDAPS hozzáféréssel a felügyelt tartományok az interneten keresztül
> [!NOTE]
> **Nem kötelező** - LDAPS hozzáféréssel a felügyelt tartományra nincs engedélyezve az interneten keresztül, ha a konfigurációs feladat kihagyása.
>
>

Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).

A felügyelt tartományok LDAPS hozzáférési közzéteszi az interneten keresztül jelenti a biztonsági kockázatot jelentenek. A felügyelt tartományra érhető el az interneten, a biztonságos LDAP használt port (Ez azt jelenti, hogy a 636-os port). Ezért ha szeretné, korlátozza a hozzáférést a felügyelt tartományra ismert IP-címek. A nagyobb biztonság érdekében hozzon létre egy hálózati biztonsági csoportot (NSG), és rendelje hozzá azt az alhálózatot, ha engedélyezte az Azure AD tartományi szolgáltatásokat.

Az alábbi táblázat mutatja be egy minta NSG-t is konfigurálhat, biztonságos LDAP hozzáférést zárolását az interneten keresztül. Az NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat. Minden bejövő forgalom az internetről az alapértelmezett "DenyAll" szabály vonatkozik. Az NSG-szabály LDAPS hozzáférést a megadott IP-címeket az interneten keresztül rendelkezik, magasabb prioritású, mint a DenyAll NSG-szabály.

![Minta NSG LDAPS hozzáférés biztonságossá az interneten keresztül](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**További információ** - [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
* [Csoportházirend a egy Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-group-policy.md)
* [Hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md)
* [Hálózati biztonsági csoport létrehozása](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
