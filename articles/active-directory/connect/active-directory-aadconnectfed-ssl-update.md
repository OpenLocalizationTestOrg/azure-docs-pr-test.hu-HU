---
title: "Az Azure AD Connect: Frissítés hello SSL-tanúsítványa egy Active Directory összevonási szolgáltatások (AD FS) farm |} Microsoft Docs"
description: "Ez a dokumentum részletek hello lépéseket tooupdate hello SSL-tanúsítvány egy AD FS-farm, az Azure AD Connect használatával."
services: active-directory
keywords: "az Azure ad connect, az AD FS ssl-frissítés, az AD FS-tanúsítványt frissítés, módosítsa az AD FS-tanúsítványt, új AD FS-tanúsítványt, az AD FS tanúsítványt, a frissítés AD FS ssl-tanúsítványt, a frissítés ssl tanúsítvány adfs konfigurálása az AD FS ssl-tanúsítvány, az AD FS, ssl, tanúsítvány, az AD FS szolgáltatás közötti kommunikációs tanúsítványt, a frissítés összevonási, összevonás konfigurálása, aad-csatlakozás"
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: anandy
ms.openlocfilehash: bce7f75aab83b6abacb8472a6895054d137e10e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-hello-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Az Active Directory összevonási szolgáltatások (AD FS) farm hello SSL-tanúsítvány frissítése

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan használhatja az Azure AD Connect tooupdate hello SSL-tanúsítványt az Active Directory összevonási szolgáltatások (AD FS) farmhoz. Akkor is, ha hello felhasználói bejelentkezési módszer kijelölt nem AD FS, hello Azure AD Connect eszköz tooeasily frissítés hello SSL-tanúsítvány használható hello AD FS-farm.

Az SSL-tanúsítványa hello AD FS-farm összes összevonási és három egyszerű lépését a webalkalmazás-proxykiszolgálóként (WAP) kiszolgálók frissítése hello a teljes műveletet végezheti el:

![Három lépést](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
>További információ az AD FS által használt tanúsítványok toolearn lásd [AD FS által használt tanúsítványok ismertetése](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Előfeltételek

* **AD FS-Farm**: Győződjön meg arról, hogy az AD FS farm Windows Server 2012 R2-alapú vagy újabb.
* **Az Azure AD Connect**: Győződjön meg arról, hogy hello Azure AD Connect verziója 1.1.443.0 vagy újabb. Hello feladat fogjuk **frissítés az AD FS SSL-tanúsítvány**.

![SSL-tevékenység frissítése](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>1. lépés: Az AD FS farm adatok megadása

Az Azure AD Connect megkísérli automatikusan hello AD FS-farm tooobtain információt:
1. Active Directory összevonási szolgáltatások (Windows Server 2016 vagy újabb) hello farm adatainak lekérdezésekor.
2. Hivatkozási hello adatait az Azure AD Connect helyileg tárolt korábbi futtatásakor.

Módosíthatja a kiszolgálók hozzáadásával vagy eltávolításával hello kiszolgálók tooreflect hello aktuális konfigurációja hello AD FS-farm hello listája. Amint hello server információt, az Azure AD Connect hello kapcsolat és az SSL-tanúsítvány aktuális állapotát jeleníti meg.

![AD FS-kiszolgáló adatai](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

Ha hello lista tartalmaz egy kiszolgálót, amely már nem hello AD FS-farm része, kattintson a **eltávolítása** toodelete hello kiszolgálót a kiszolgálók az AD FS farmon hello listája.

![Kapcsolat nélküli server listában](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> A kiszolgáló eltávolításával hello listából az AD FS-kiszolgálók az Azure AD Connectben farm helyi művelet, és frissítések hello hello helyileg tárolja az Azure AD Connect AD FS-farm adatait. Az Azure AD Connect nem módosítja az AD FS tooreflect hello változás hello konfigurációs.    

## <a name="step-2-provide-a-new-ssl-certificate"></a>2. lépés:, Adjon meg egy új SSL-tanúsítvány

Az AD FS farm kiszolgálói hello információ megerősítését követően az Azure AD Connect hello új SSL-tanúsítványt kér. Tanúsítvány jelszóval védett PFX toocontinue hello telepítést biztosítanak.

![SSL-tanúsítvány](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

Miután megadta a hello tanúsítványt, az Azure AD Connect végighalad az Előfeltételek sorozata. Győződjön meg arról, hogy a tanúsítvány hello hello tanúsítvány tooensure megfelelő hello AD FS-farm:

-   hello tulajdonos neve vagy másodlagos tulajdonosneve hello tanúsítvány vagy hello ugyanaz, mint hello összevonási szolgáltatás neve, vagy helyettesítő tanúsítványt.
-   több mint 30 napig hello tanúsítvány érvénytelen.
-   Érvénytelen a hello tanúsítvány megbízhatósági láncában.
-   hello tanúsítvány jelszóval védve.

## <a name="step-3-select-servers-for-hello-update"></a>3. lépés: Válassza a kiszolgálók hello frissítés

Hello a következő lépésben válassza ki a hello kiszolgálók toohave SSL-tanúsítvány hello frissíteni kell. Az offline kiszolgálók hello frissítés nem állítható be.

![Válassza ki a kiszolgálók tooupdate](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

Hello konfiguráció befejezése után az Azure AD Connect hello frissítés hello állapotát jelzi, és egy beállítás tooverify hello AD FS-bejelentkezés biztosít hello üzenetet jeleníti meg.

![Konfigurálás befejezése](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>Gyakori kérdések

* **Milyen kell hello hello tanúsítvány tulajdonosnevének hello új AD FS SSL-tanúsítvány?**

    Az Azure AD Connect ellenőrzi, hogy hello tulajdonos neve vagy másodlagos hello tanúsítvány tulajdonosnevének tartalmazza-e a hello összevonási szolgáltatás neve. Például ha az összevonási szolgáltatás nevének fs.contoso.com, a tulajdonos neve vagy másodlagos tulajdonosnév hello kell fs.contoso.com.  Helyettesítő tanúsítványok is használhatók.

* **Miért kérdezi meg az alkalmazás hitelesítő adatainak újra hello WAP-kiszolgáló lapon?**

    Ha a hello hitelesítő adatokat ad meg a tooAD FS kiszolgálókat is nincs hello jogosultság toomanage hello WAP-kiszolgálókkal, majd az Azure AD Connect hitelesítő adatokat kér, amely rendszergazdai jogosultságokkal rendelkezik a hello WAP-kiszolgálókkal.

* **hello kiszolgáló offline állapotúként jelenik meg. Mit tegyek?**

    Az Azure AD Connect nem hajtható végre semmilyen műveletet, ha hello kiszolgáló offline állapotban. Ha hello kiszolgáló hello AD FS-farm része, akkor ellenőrizze a hello kapcsolat toohello kiszolgáló. Miután hello problémát már megoldott, nyomja le az ENTER hello frissítés ikon tooupdate hello állapotának hello varázslóban. Ha hello kiszolgáló része volt a hello farm korábban, de most már nem létezik, kattintson a **eltávolítása** toodelete hello listából az Azure AD Connect-kiszolgálók tart fenn. Az Azure AD Connect hello listájából eltávolításakor hello server hello maga az AD FS konfigurációs nem módosítható. Ha az AD FS a Windows Server 2016 vagy újabb, illetve hello server marad hello konfigurációs beállításokat használ, és megjelenik újra hello legközelebb hello feladat fut.

* **Frissíthetem a farm kiszolgálói részhalmazának hello új SSL-tanúsítvány?**

    Igen. Mindig hello feladatot futtathatja **frissítés SSL-tanúsítvány** újra tooupdate hello fennmaradó kiszolgálók. A hello **válassza kiszolgálók SSL tanúsítvány frissítés** lapon rendezheti hello azon kiszolgálók listájára, **SSL lejárati** tooeasily hello kiszolgálók, amelyek még nincsenek frissítve.

* **Hello-kiszolgálónak az előző hello eltávolított, de továbbra is alatt megjelenő, a kapcsolat nélküli és a felsorolt hello AD FS-kiszolgálók lapon. Miért van hello még a kapcsolat nélküli kiszolgáló, az eltávolított után is?**

    Eltávolításakor hello server hello listájából az Azure AD Connect nem távolítható el az AD FS-konfiguráció hello. Az Azure AD Connect semmilyen információt hello farm AD FS (Windows Server 2016 vagy újabb) hivatkozik. Ha hello kiszolgáló továbbra is megtalálhatók hello AD FS konfigurációt, az jelenik vissza hello listájában.  

## <a name="next-steps"></a>Következő lépések

- [Azure AD Connect és összevonás](active-directory-aadconnectfed-whatis.md)
- [Active Directory összevonási szolgáltatások kezelése és testreszabása az Azure AD Connecttel](active-directory-aadconnect-federation-management.md)
