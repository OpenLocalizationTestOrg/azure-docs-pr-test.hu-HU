---
title: "Biztonságos LDAP (LDAPS) az Azure AD tartományi szolgáltatásokban aaaConfigure |} Microsoft Docs"
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
ms.openlocfilehash: a0d6e2faf474b1f0cbe157fb4ae2754b1d521ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása

## <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy befejezte [2. feladat – exportálás hello biztonságos LDAP tanúsítvány tooa. PFX-fájl](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).

Válassza ki e toouse hello Azure portál felhasználói élmény preview vagy hello Azure klasszikus portál toocomplete ezt a feladatot.
> [!div class="op_single_selector"]
> * **Azure-portálon (előzetes verzió)**: [engedélyezése biztonságos LDAP hello Azure-portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
> * **A klasszikus Azure portálon**: [engedélyezése biztonságos LDAP hello a klasszikus Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-classic-azure-portal"></a>3. feladat – hello felügyelt tartomány hello a klasszikus Azure portál használatával biztonságos LDAP engedélyezése
tooenable biztonságos LDAP, hajtsa végre a következő konfigurációs lépéseket hello:

1. Keresse meg a toohello  **[a klasszikus Azure portálon](https://manage.windowsazure.com)**.
2. Jelölje be hello **Active Directory** csomópontot a bal oldali panelen hello.
3. Válassza ki a hello Azure Active directory (is hivatkozott tooas "tenant"), amelyhez engedélyezte az Azure AD tartományi szolgáltatásokat.

    ![Azure AD címtár kiválasztása](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Kattintson a hello **konfigurálása** fülre.

    ![Címtár lapjának konfigurálása](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Görgessen lefelé toohello című **tartományi szolgáltatások**. Megjelenik egy lehetőség, című **biztonságos LDAP (LDAPS)** látható módon a következő képernyőkép hello:

    ![Tartományi szolgáltatások konfigurációja szakasz](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. Kattintson a hello **tanúsítvány konfigurálása...**  hello fel gomb toobring **tanúsítvány konfigurálása a biztonságos LDAP** párbeszédpanel.

    ![Biztonságos LDAP-tanúsítvány konfigurálása](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. Kattintson a hello mappa ikon következő **PFX-fájl a tanúsítvány** toospecify hello PFX-fájl, a biztonságos LDAP hozzáférés toohello toouse kívánja hello tanúsítványt tartalmazó felügyelt tartomány. Hello tanúsítvány toohello PFX-fájl exportálása során megadott hello jelszó is beírhatja. Kattintson a Kész gombra a hello alsó hello.

    ![Biztonságos LDAP PFX-fájlt és a jelszó megadása](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. Hello **tartományi szolgáltatások** hello szakasza **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a hello **folyamatban...**  állapot néhány percig. Ebben az időszakban hello LDAPS tanúsítvány pontosságát ellenőrizve, és biztonságos LDAP konfigurálva van a felügyelt tartományok.

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > Tooenable biztonságos LDAP a felügyelt tartományok körülbelül 10 too15 percet vesz igénybe. Ha hello biztonságos LDAP-tanúsítvány nem megfelelő hello szükséges feltételeket, biztonságos LDAP nincs engedélyezve a címtárban, és hibát látja. Ha például hello tartomány neve nem megfelelő, hello tanúsítvány már lejárt vagy hamarosan lejár.
   >
   >

9. Ha biztonságos LDAP sikeresen engedélyezve van a felügyelt tartományok, hello **folyamatban...**  üzenet kell eltűnik. Meg kell jelennie hello tanúsítvány ujjlenyomata látható hello jelenik meg.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-hello-internet"></a>4. feladat – hello biztonságos LDAP-hozzáférés engedélyezése az internet
**Nem kötelező** – Ha nem tervezi tooaccess hello felügyelt tartományra LDAPS több mint hello internet, hagyja ki a konfigurációs feladat.

Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).

1. Megjelenik egy lehetőség túl**ENGEDÉLYEZNI biztonságos LDAP hozzáférés keresztül hello INTERNET** a hello **tartományi szolgáltatások** hello szakasza **konfigurálása** lap. Ez a beállítás túl**nem** alapértelmezés szerint óta internet access toohello felügyelt tartomány keresztül biztonságos LDAP alapértelmezésben le van tiltva.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. Váltás **ENGEDÉLYEZNI biztonságos LDAP hozzáférés keresztül hello INTERNET** túl**Igen**. Kattintson a hello **mentése** hello alsó panelje gombjára.
    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)
3. Hello **tartományi szolgáltatások** hello szakasza **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a hello **folyamatban...**  állapot néhány percig. Némi várakozás után internet access tooyour által kezelt tartomány keresztül biztonságos LDAP engedélyezve van.

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > Körülbelül 10 percet vesz igénybe tooenable internet-hozzáférés biztonságos LDAP keresztül a felügyelt tartományok.
   >
   >
4. Biztonságos LDAP hozzáférés tooyour felügyelt tartomány keresztül hello internet sikeresen engedélyezve van, hello **folyamatban...**  üzenet kell eltűnik. Hello külső IP-címet, amelyet látni lehet használt tooaccess a címtár keresztül hello mezőben LDAPS **külső IP-cím a LDAPS ELÉRÉST**.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a>5. feladat – DNS-tooaccess hello felügyelt tartomány a hello konfigurálása internet
**Nem kötelező** – Ha nem tervezi tooaccess hello felügyelt tartományra LDAPS több mint hello internet, hagyja ki a konfigurációs feladat.

Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).

Miután engedélyezte a biztonságos LDAP hozzáférést keresztül hello internet a felügyelt tartományok kell tooupdate DNS, hogy a felügyelt tartomány található ügyfélszámítógépek számára. 4. feladat hello végén, külső IP-cím jelenik meg hello **konfigurálása** lapján **külső IP-cím a LDAPS ELÉRÉST**.

Konfigurálja a külső DNS-szolgáltatónál, úgy, hogy hello hello DNS-nevét (például ldaps.contoso100.com) tartomány pontok toothis külső IP-címet felügyeli. Ebben a példában igazolnia kell a DNS-bejegyzés a következő toocreate hello:

    ldaps.contoso100.com  -> 52.165.38.113

Ennyi az egész – most már áll készen tooconnect toohello felügyelt tartomány biztonságos LDAP protokollt használó hello internet.

> [!WARNING]
> Ne feledje, hogy ügyfélszámítógépek meg kell bíznia hello LDAPS tanúsítvány toobe képes tooconnect hello kiállítójának sikeresen toohello felügyelt tartományba LDAPS használatával. Ha egy vállalati hitelesítésszolgáltatótól vagy egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell toodo semmit ügyfélszámítógépek számára megbízhatónak ezek tanúsítványkiállítók óta. Ha önaláírt tanúsítványt használ, tooinstall hello nyilvános részét hello önaláírt tanúsítvány kell hello ügyfélszámítógépen hello megbízható tanúsítványok tárolójába.
>
>


## <a name="lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a>Zárolási-le nyilas LDAPS tooyour által kezelt tartomány érje el hello internet
> [!NOTE]
> **Nem kötelező** – Ha nem engedélyezte a LDAPS hozzáférés toohello felügyelt tartomány több mint hello internet, hagyja ki a konfigurációs feladat.
>
>

Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).

A felügyelt tartományok LDAPS hozzáférési kitettségének keresztül hello internetes biztonsági kockázatot jelentenek jelöli. hello által kezelt tartomány érhető el a hello interneten, a használt biztonságos LDAP hello port (Ez azt jelenti, hogy a 636-os port). Ezért dönthet úgy toorestrict hozzáférés toohello felügyelt tartomány toospecific ismert IP-címeket. A nagyobb biztonság érdekében hozzon létre egy hálózati biztonsági csoportot (NSG), és társítsa azt az hello alhálózatot, ha engedélyezte az Azure AD tartományi szolgáltatásokat.

hello alábbi táblázat mutatja be egy minta NSG is konfigurálhat, biztonságos LDAP hozzáférés keresztül hello toolock internet. hello NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat. hello alapértelmezett "DenyAll" szabály tooall egyéb bejövő forgalmát hello az interneten. hello NSG szabály tooallow LDAPS hozzáférés hello keresztül megadott IP-címekről érkező internetes rendelkezik magasabb prioritású, mint hello DenyAll NSG-szabály.

![A minta NSG toosecure LDAPS hozzáférés keresztül hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**További információ** - [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
* [Csoportházirend a egy Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-group-policy.md)
* [Hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md)
* [Hálózati biztonsági csoport létrehozása](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
