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
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 8781285cd02d690788056b985b017efd7e4d6b3f
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


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-azure-portal-preview"></a>3. feladat – hello felügyelt tartomány hello Azure portal (előzetes verzió) segítségével biztonságos LDAP engedélyezése
tooenable biztonságos LDAP, hajtsa végre a következő konfigurációs lépéseket hello:

1. Keresse meg a toohello  **[Azure-portálon](https://portal.azure.com)**.

2. Keresési hello tartományi szolgáltatások **keresési erőforrások** keresőmezőbe. Válassza ki **Azure AD tartományi szolgáltatások** hello keresési eredmény alapján. Hello **Azure AD tartományi szolgáltatások** panel a felügyelt tartományok sorolja fel.

    ![Található felügyelt tartomány telepítése folyamatban](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Kattintson hello hello felügyelt tartomány (például "contoso100.com") toosee hello tartománnyal kapcsolatos további részletekért.

    ![Tartományi szolgáltatások - üzembe helyezési állapota](./media/getting-started/domain-services-provisioning-state.png)

3. Kattintson a **biztonságos LDAP** hello navigációs ablaktáblán.

    ![Tartományi szolgáltatások - biztonságos LDAP panel](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Biztonságos LDAP hozzáférés tooyour által kezelt tartomány alapértelmezés szerint le van tiltva. Váltás **biztonságos LDAP** túl**engedélyezése**.

    ![Biztonságos LDAP engedélyezése](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Alapértelmezés szerint biztonságos LDAP hozzáférés tooyour felügyelt tartomány keresztül hello internet le van tiltva. Váltás **biztonságos LDAP hozzáférés engedélyezése keresztül hello internet** túl**engedélyezése**, ha szükséges. 

6. Kattintson a hello mappa ikon következő **. Biztonságos LDAP tanúsítvány PFX-fájl**. Adja meg a hello elérési toohello PFX-fájl hello tanúsítvánnyal biztonságos LDAP hozzáférés toohello kezelt tartományban.

7. Adja meg a hello **jelszó toodecrypt. PFX-fájl**. Adja meg a hello hello tanúsítvány toohello PFX-fájljának exportálásakor használja ugyanazt a jelszót.

8. Amikor elkészült, kattintson a hello **mentése** gombra.

9. Megjelenik egy értesítés, amely arról tájékoztatja, biztonságos LDAP hello felügyelt tartomány van konfigurálva. Amíg ez a művelet be nem fejeződik, hello tartomány számára más beállításokat nem módosíthatja.

    ![Biztonságos LDAP hello felügyelt tartomány beállítása](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Tooenable biztonságos LDAP a felügyelt tartományok körülbelül 10 too15 percet vesz igénybe. Ha hello biztonságos LDAP-tanúsítvány nem megfelelő hello szükséges feltételeket, biztonságos LDAP nincs engedélyezve a címtárban, és hibát látja. Ha például hello tartomány neve nem megfelelő, hello tanúsítvány már lejárt vagy hamarosan lejár. Ebben az esetben próbálkozzon újra egy érvényes tanúsítványt.
>
>

<br>

## <a name="task-4---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a>4. feladat – DNS-tooaccess hello felügyelt tartomány a hello konfigurálása internet
> [!NOTE]
> **Nem kötelező** – Ha nem tervezi tooaccess hello felügyelt tartományra LDAPS több mint hello internet, hagyja ki a konfigurációs feladat.
>
>

Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

Miután engedélyezte a biztonságos LDAP hozzáférést keresztül hello internet a felügyelt tartományok kell tooupdate DNS, hogy a felügyelt tartomány található ügyfélszámítógépek számára. 3. feladat hello végén, külső IP-cím jelenik meg hello **tulajdonságok** panel az **külső IP-cím a LDAPS ELÉRÉST**.

Konfigurálja a külső DNS-szolgáltatónál, úgy, hogy hello hello DNS-nevét (például ldaps.contoso100.com) tartomány pontok toothis külső IP-címet felügyeli. Ebben a példában igazolnia kell a DNS-bejegyzés a következő toocreate hello:

    ldaps.contoso100.com  -> 52.165.38.113

Ennyi az egész – most már áll készen tooconnect toohello felügyelt tartomány biztonságos LDAP protokollt használó hello internet.

> [!WARNING]
> Ne feledje, hogy ügyfélszámítógépek meg kell bíznia hello LDAPS tanúsítvány toobe képes tooconnect hello kiállítójának sikeresen toohello felügyelt tartományba LDAPS használatával. Ha egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell toodo semmit óta az ügyfélszámítógépek számára megbízhatónak ezek tanúsítványkiállítók. Ha önaláírt tanúsítványt használ, telepítse a hello önaláírt tanúsítvány nyilvános részét hello hello ügyfélszámítógépen hello megbízható tanúsítványok tárolójába.
>
>


## <a name="task-5---lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a>5. feladat – lock-le nyilas LDAPS hozzáférési tooyour felügyelt tartomány keresztül hello internet
> [!NOTE]
> **Nem kötelező** – Ha nem engedélyezte a LDAPS hozzáférés toohello felügyelt tartomány több mint hello internet, hagyja ki a konfigurációs feladat.
>
>

Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

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
