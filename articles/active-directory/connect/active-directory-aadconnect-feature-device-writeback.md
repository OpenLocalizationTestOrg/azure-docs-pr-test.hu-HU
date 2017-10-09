---
title: "Az Azure AD Connect: Eszközvisszaírás engedélyezése |} Microsoft Docs"
description: "A dokumentum hogyan részletek Azure AD Connect használatával tooenable eszközvisszaíró"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2566a514137fed85b21929207cf3230e6878ebbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Az Azure AD Connect: Eszközvisszaírás engedélyezése
> [!NOTE]
> Egy előfizetés tooAzure AD Premium szükség az eszközök visszaírásához.
> 
> 

a következő dokumentáció hello bemutatja, hogyan tooenable hello eszközvisszaíró beállítást, az Azure AD Connectben. Eszközvisszaíró hello a következő esetekben használja:

* Feltételes hozzáférési házirend alapján az eszközök tooADFS engedélyezése (2012 R2 vagy újabb) védett alkalmazások (függő entitások megbízhatóságához).

Ez további biztonságot nyújt, és garantálja, hogy hozzáférési tooapplications csak tootrusted eszközök kap. További információ a feltételes hozzáférési: [kockázat kezelése feltételes hozzáféréssel rendelkező](../active-directory-conditional-access.md) és [helyszíni feltételes hozzáférés használata az Azure Active Directory Eszközregisztráció beállítása](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Eszközök objektumának hello hello felhasználóként azonos erdőben kell lennie. Eszközöket úgy kell megírni, vissza tooa egyetlen erdő esetén, mert ez a funkció jelenleg nem támogatja a központi telepítés felhasználói több erdővel.</li>
> <li>Csak egy eszköz regisztrációs konfigurációs objektum toohello a helyszíni Active Directory-erdő lehet hozzáadni. Ez a szolgáltatás nem található a topológia ahol hello a helyszíni Active Directory szinkronizált toomultiple az Azure AD-könyvtárak nem kompatibilis.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>1. lépés: Telepítse az Azure AD Connect
1. Az Azure AD Connect egyéni használatával telepítsen, vagy Gyorsbeállítások. A Microsoft azt javasolja, hogy az összes felhasználók és csoportok sikeresen kell szinkronizálnia a engedélyezése előtt toostart.

## <a name="part-2-prepare-active-directory"></a>2. lépés: Az Active Directory előkészítése
A következő lépéseket tooprepare eszközvisszaíró használatára vonatkozó hello használata.

1. Hello gépen, amelyen telepítve van-e az Azure AD Connect indítsa el a Powershellt emelt jogosultsági szinttel.
2. Ha hello Active Directory PowerShell-modulja nincs telepítve, telepítse a következő parancs hello:
   
   `Add-WindowsFeature RSAT-AD-PowerShell`
3. Ha hello Azure Active Directory PowerShell-modulja nincs telepítve, majd töltse le és telepítse innen [Active Directory modul Windows Powershellhez készült Azure (64 bites változat)](http://go.microsoft.com/fwlink/p/?linkid=236297). Ez az összetevő függőségi hello bejelentkezési segéd, amely telepítve van az Azure AD Connect kapcsolatban van.
4. Vállalati rendszergazdai hitelesítő adataival futtassa a következő parancsok hello, és zárja be a PowerShell.
   
   `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`
   
   `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Vállalati rendszergazda hitelesítő adataival szükség, mert módosítások toohello konfigurációs névtér van szükség. A tartományi rendszergazda nem lesz megfelelő engedélyekkel.

![PowerShell-parancsot az eszközvisszaírás engedélyezése](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Leírás:

* Ha már nem léteznek, hoz létre és konfigurálja az új tárolók és objektumok CN = Device Registration Configuration, CN = Services, CN = Configuration, [erdő-dn].
* Ha már nem léteznek, hoz létre és konfigurálja az új tárolók és objektumok CN = RegisteredDevices, [tartomány-dn]. Eszközobjektumok létrehozza a tárolóban található.
* Egy szükséges engedélyekkel a hello Azure AD-összekötő fiókhoz, az Active Directory-toomanage eszközöket.
* Csak akkor kell egy erdőn toorun, még akkor is, ha az Azure AD Connect van telepítve a több erdőt.

Paraméterek:

* Tartománynév: Active Directory-tartomány ahol eszközobjektumok létrejön. Megjegyzés: Minden eszközök adott Active Directory-erdő egyetlen tartományt létrejön.
* AdConnectorAccount: Active Directory-fiókot, amely az Azure AD Connect toomanage hello directory objektumok által használható. Ez az Azure AD Connect szinkronizálási tooconnect tooAD által használt hello fiók. Ha telepítette a gyorsbeállításokkal, MSOL_ előtagként hello fiókot is.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>3. lépés: Az Azure AD Connect engedélyezése eszközvisszaíró
A következő eljárás tooenable eszközvisszaíró az Azure AD Connectben hello használata.

1. Futtassa újra a hello telepítővarázslóját. Válassza ki **testre szabhatja a szinkronizálási beállítások** hello további feladatok a lapon, majd kattintson **következő**.
   ![Egyéni telepítés testre szabhatja a szinkronizálási beállítások](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2. Hello választható szolgáltatások lapon, az eszközök visszaírásához már nem szürkén jelennek meg. Vegye figyelembe, hogy ha hello előkészítő lépések az Azure AD Connect nem fejeződtek be eszközvisszaíró jelenítette ki hello választható szolgáltatások lapon. Az eszközök visszaírásához hello jelölőnégyzetet, majd kattintson a **következő**. Ha hello jelölőnégyzet továbbra is le van tiltva, lásd: hello [hibaelhárítási szakaszában](#the-writeback-checkbox-is-still-disabled).
   ![Egyéni Eszközvisszaíró választható funkciók telepítése](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3. Hello visszaírási lapon hello megadott tartomány hello alapértelmezett eszköz visszaírási erdő, jelennek meg.
   ![Egyéni telepítés eszköz visszaírási célerdő](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4. További konfigurációs módosítások nélküli varázsló hello hello telepítésének befejezéséhez. Ha szükséges, tekintse meg a túl[az Azure AD Connect egyéni telepítése.](active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Feltételes hozzáférés engedélyezése
Részletes utasítások tooenable érhetők el ebben a forgatókönyvben belül [helyszíni feltételes hozzáférés használata az Azure Active Directory Eszközregisztráció beállítása](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-devices-are-synchronized-tooactive-directory"></a>Ellenőrizze eszköz szinkronizált tooActive könyvtár
Eszközvisszaíró most már megfelelően fog működni. Vegye figyelembe, hogy is eltarthat, too3 órát, hogy az eszköz objektumok toobe írt visszaírt tooAD.  hogy az eszközök megfelelően, hogy szinkronizált tooverify hello hello szinkronizálási szabályok befejezése után a következő:

1. Indítsa el az Active Directory felügyeleti központ.
2. Bontsa ki a RegisteredDevices, belül hello tartományhoz, amely éppen össze van vonva.
   ![Az Active Directory felügyeleti központ regisztrált eszközök](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3. Aktuális regisztrált eszközök nem jelennek meg.
   ![Az Active Directory felügyeleti központ regisztrált eszközök listája](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="hello-writeback-checkbox-is-still-disabled"></a>hello visszaírási jelölőnégyzet továbbra is le van tiltva.
Ha hello jelölőnégyzet eszközvisszaíró használata nem engedélyezett, annak ellenére, hogy a fenti hello lépésekkel hello lépéseket ismerteti keresztül milyen hello telepítési varázsló hello mezőben engedélyezése előtt ellenőrzi.

Első lépések első:

* Győződjön meg arról, hogy legalább egy erdő Windows Server 2012 R2 rendszerben. hello eszköz objektumtípus jelen kell lennie.
* Ha hello telepítővarázsló már fut, majd a módosításokat nem érzékeli. Ebben az esetben hello telepítési varázsló, és futtassa újból.
* Ellenőrizze, hogy megadta a hello inicializálási parancsfájlja hello fiók ténylegesen hello megfelelő felhasználói hello Active Directory-összekötő által használt. tooverify, kövesse az alábbi lépéseket:
  * Hello start menüből nyissa meg a **szinkronizálási szolgáltatás**.
  * Nyissa meg hello **összekötők** fülre.
  * Megkeresi a hello összekötő típusú Active Directory tartományi szolgáltatásokban, és válassza ki azt.
  * A **műveletek**, jelölje be **tulajdonságok**.
  * Nyissa meg túl**tooActive Directory-erdő csatlakozás**. Ellenőrizze, hogy a képernyő egyezés hello fiókját toohello parancsfájlhoz megadott hello tartomány és a felhasználó nevét.
    ![A Szinkronizáló Service Manager-összekötő-fiók](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Ellenőrizze az Active Directory konfigurációját:

* Győződjön meg arról, hogy Eszközregisztrációs szolgáltatás az alábbi hello helyen található hello (CN DeviceRegistrationService, CN = eszköz regisztrációs Services, CN = Device Registration Configuration, CN = Services, CN = = Configuration) konfigurációs névhasználati környezetében.

![A hiba elhárításához DeviceRegistrationService konfigurációs névtér](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Győződjön meg arról csak egy konfigurációs objektumot hello konfigurációs névtér keresve. Ha egynél több, törölje a hello duplikált.

![Elhárításával kapcsolatos tudnivalókat hello duplikált objektumok keresése](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Hello az Eszközregisztrációs szolgáltatás objektumon győződjön meg arról, hello attribútum az msDS-DeviceLocation jelen, és értéke. Keresés a helyét, és győződjön meg arról, hogy telepítve a hello objectType msDS-DeviceContainer.

![A hiba elhárításához msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![A hiba elhárításához RegisteredDevices objektum osztálya](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Ellenőrizze a hello fiók hello által használt Active Directory-összekötő szükséges engedélyekkel rendelkezzen a hello regisztrált eszközök tárolója hello előző lépésben található. Ez az várt hello engedélyekkel a tárolóra vonatkozóan:

![Elhárításával kapcsolatos tudnivalókat tároló engedélyeinek ellenőrzése](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Ellenőrizze a hello Active Directory-fiók megfelelő jogosultságokkal rendelkezik hello CN = Device Registration Configuration, CN = Services, CN = konfigurációs objektum.

![Elhárításával kapcsolatos tudnivalókat Eszközregisztráció konfigurációja az engedélyek ellenőrzése](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>További információ
* [Kockázatkezelés feltételes hozzáférés](../active-directory-conditional-access.md)
* [A helyszíni feltételes hozzáférés használata az Azure Active Directory Eszközregisztráció beállítása](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

