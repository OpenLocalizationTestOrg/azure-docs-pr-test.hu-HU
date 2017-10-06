---
title: "Ismételt futtatásával hello Azure AD Connect varázsló |} Microsoft Docs"
description: "Ismerteti, hello telepítési varázsló működése hello még egyszer kell futtatnia."
keywords: "hello Azure AD Connect telepítővarázsló lehetővé teszi a karbantartási beállítások hello konfigurálása még egyszer kell futtatnia"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 83cc74aca471ef9b4f65f7f3582e3e48d3d81cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-running-hello-installation-wizard-a-second-time"></a>Azure AD Connect szinkronizálása: hello telepítővarázsló futtatása még egyszer
hello hello Azure AD Connect telepítővarázsló első futtatásakor az végigvezeti tooconfigure a telepítést. Ha újra hello telepítési varázsló, karbantartási lehetőségeinek kínál.

Hello telepítővarázsló nevű hello start menüben található **az Azure AD Connect**.

![Start menü](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Hello telepítővarázsló indításakor, beállítások lap jelenik meg:

![További feladatok listáját tartalmazó lap](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Ha már telepítette az AD FS az Azure AD Connect, még akkor is több lehetőség van. az AD FS ismertetett rendelkezik további beállítások hello [az AD FS felügyeleti](active-directory-aadconnect-federation-management.md#manage-ad-fs).

Válassza ki valamelyik hello feladatot, és kattintson a **következő** toocontinue.

> [!IMPORTANT]
> Miközben hello telepítési varázsló megnyitása, a szinkronizálási motor hello szereplő összes művelet fel vannak függesztve. Ellenőrizze, hogy hello telepítővarázsló bezárása, amint befejezte a konfigurációs módosításokat.
>
>

## <a name="view-current-configuration"></a>Aktuális konfiguráció megtekintése
Ez a beállítás lehetővé teszi a jelenleg konfigurált beállítások gyors áttekintést.

![Az összes beállítás és állapotukra listáját lap](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Kattintson a **előző** toogo vissza. Ha **kilépési**, hello telepítési varázsló bezárásához.

## <a name="customize-synchronization-options"></a>Szinkronizálási beállítások testreszabása
Ez a beállítás akkor használt toomake módosítások toohello sync konfigurációja. Megjelenik egy részhalmazát hello egyéni konfigurációs telepítési elérési utat a beállításai. Látja ezt a beállítást, akkor is, ha az Expressz telepítés eredetileg használt.

* [Adja hozzá a további címtárakat](active-directory-aadconnect-get-started-custom.md#connect-your-directories). A könyvtár törlése, lásd: [összekötő törlése](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
* [Módosítsa a tartomány és szervezeti egységek szűrése](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
* Szűrés csoport eltávolítása.
* [Választható szolgáltatások módosítása](active-directory-aadconnect-get-started-custom.md#optional-features).

hello egyéb beállítások és a kezdeti telepítés hello nem módosítható és nem érhetők el. Ezek a beállítások a következők:

* A userPrincipalName és sourceAnchor hello attribútum toouse módosítása.
* Csatlakozás másik erdőből származó objektumok metódus hello módosítása.
* Csoport-alapú szűrés engedélyezése.

## <a name="refresh-directory-schema"></a>Directory-séma frissítése
Ez a beállítás használható, ha hello séma módosította az egyik a helyszíni AD DS-erdőkből. Például előfordulhat, hogy telepített Exchange, vagy frissített Windows Server 2012 tooa séma eszköz objektummal. Ebben az esetben kell tooinstruct az Azure AD Connect tooread hello séma újra az Active Directory tartományi Szolgáltatásokban, és frissítse gyorsítótárát. Ezzel a művelettel újragenerálja a hello szinkronizálási szabályokat is. Példa felvételekor hello Exchange-séma, az Exchange hello szinkronizálási szabályok hozzáadása történik meg toohello konfigurációs.

Ha ezt a lehetőséget választja, a konfiguráció összes hello könyvtárak jelennek meg. Hello alapértelmezett megtartásához és frissítse az összes erdőben, és némelyikük törölje.

![Az összes könyvtár hello környezetben listáját lap](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Átmeneti környezetű üzemmód konfigurálása
Ez a beállítás lehetővé teszi tooenable, és tiltsa le az átmeneti környezetű üzemmód hello kiszolgálón. További információt az átmeneti módot, és hogyan használja fel azokat a található [műveletek](active-directory-aadconnectsync-operations.md#staging-mode).

hello beállítást jeleníti meg, ha átmeneti jelenleg engedélyezve vagy letiltva:  
![Lehetőség, amely is láthatók az átmeneti környezetű üzemmód hello aktuális állapota](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

toochange hello állapotát, jelölje be ezt a beállítást és válassza ki, vagy visszavonása hello jelölőnégyzetet.  
![Lehetőség, amely is láthatók az átmeneti környezetű üzemmód hello aktuális állapota](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Felhasználói bejelentkezés módosítása
Ez a beállítás lehetővé teszi a jelszó-szinkronizálás toofederation vagy megfordítva már hello toochange. Nem módosítható túl**ne konfiguráljon**.

Ez a beállítás további információkért lásd: [felhasználói bejelentkezés](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Következő lépések
* További tudnivalók az Azure AD Connect szinkronizálási által használt hello konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
