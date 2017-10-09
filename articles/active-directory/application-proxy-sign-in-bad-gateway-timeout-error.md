---
title: "AAA \"nem tud hozzáférni a vállalati alkalmazás hiba alkalmazásproxy alkalmazás használatakor |} Microsoft dokumentumok\""
description: "Hogyan tooresolve közös hozzáférési állít ki az Azure AD alkalmazásproxy alkalmazásokkal."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 490b106b7d774ee43fc076cc5d082997a1df85e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>"Nem tud hozzáférni a vállalati alkalmazás" hiba az alkalmazásproxy alkalmazás használatakor

Ez a cikk segítséget tootroubleshoot "a vállalati alkalmazás nem érhető el" hiba megjelenik az Azure AD alkalmazásproxy alkalmazás tapasztalt gyakori problémákat.

## <a name="overview"></a>Áttekintés
Ha ezt a hibaüzenetet látja, a hello lap is megosztott állapotkódot. Ezt a kódot, valószínűleg egy olyan hello következő:

-   **Átjáró időtúllépése**: hello alkalmazásproxy-szolgáltatás egy nem tooreach hello összekötő. Ez általában hello összekötő hozzárendelés, a maga összekötő vagy a hálózati szabályok körül hello összekötő hello hibáját jelzi.

-   **Hibás átjáró**: hello összekötő nem tooreach hello a háttéralkalmazás. Ennek oka lehet egy helytelen konfiguráció hello alkalmazás.

-   **Tiltott**: hello felhasználó: nem engedélyezett tooaccess hello alkalmazás. Ez akkor fordulhat elő, ha hello felhasználó nincs hozzárendelve toohello alkalmazás Azure Active Directoryban, vagy ha hello háttérkiszolgálón hello felhasználónak nincs engedélye tooaccess hello alkalmazás.

toofind hello kódot, nézze meg hello hibaüzenet hello "Állapotkód:" mező a bal alsó hello hello szöveg. Hello nagyon alsó részén további tippek hello lapon megjegyzések is keres.

   ![Átjáró időtúllépési hiba](./media/application-proxy/connection-problem.png)

Hogyan tootroubleshoot hello ezeket a hibákat, és további részleteket a javasolt javítások okozza a részletekért lásd: hello megfelelő című részhez.

## <a name="gateway-timeout-errors"></a>Átjáró-időtúllépést

Átjáró időtúllépése következik be, amikor hello szolgáltatás megpróbál tooreach hello összekötő, és nem toowithin hello időkereten. Ennek oka általában egy adott alkalmazás tooa összekötő csoportban található, nem működő összekötők, vagy bizonyos hello összekötő szükséges portok nincsenek megnyitva.


## <a name="bad-gateway-errors"></a>Hibás átjáró hibái

A hibás átjáró hiba azt jelzi, hogy hello connector nem tooreach hello a háttéralkalmazás. Győződjön meg arról, hogy miután közzétette hello megfelelő alkalmazás. Gyakori hibák, amely a hiba oka:

-   Egy elírás vagy hello belső URL-címben hiba

-   Nem teszik közzé a hello alkalmazás hello gyökérmappájában. Például közzétételi <http://expenses/reimbursement> tooaccess próbált, de <http://expenses>

-   Hello Kerberos által korlátozott delegálás (KCD) konfigurációjával kapcsolatos problémák

-   A háttéralkalmazás hello kapcsolatos problémák

## <a name="forbidden-errors"></a>Tiltott hibák

Ha egy tiltott hibát látja, hello felhasználó nincs hozzárendelve toohello alkalmazás. Ez lehet az Azure Active Directoryban, vagy a hello a háttéralkalmazás.

toolearn hogyan tooassign felhasználók toohello alkalmazás, az Azure-ban: hello [konfigurációs dokumentáció](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Ha meggyőződött róla, hogy hello felhasználói toohello alkalmazás az Azure-ban van hozzárendelve, hello felhasználói konfigurációjának a hello a háttéralkalmazás ellenőrzése. Ha a Kerberos által korlátozott delegálás vagy integrált Windows-hitelesítést használ, egy útmutatót a Kerberos által korlátozott Delegálás hibaelhárítása oldalunkon láthatja.

## <a name="check-hello-applications-internal-url"></a>Hello alkalmazás belső URL-cím ellenőrzése

Az első Kész lépés, ellenőrizze ellenőrizze és javítsa ki a hello belső URL-cím keresztül hello alkalmazás megnyitásával **vállalati alkalmazások**, majd hello kiválasztásával **alkalmazásproxy** menü. Ellenőrizze a hello megfelelő belső URL-cím, egy a helyszíni hálózati tooaccess hello alkalmazásból használt hello.

## <a name="check-hello-application-is-assigned-tooa-working-connector-group"></a>Ellenőrizze a hello alkalmazás hozzá van rendelve a tooa összekötő csoport használata

tooverify hello alkalmazás hozzá van rendelve tooa összekötő csoport használata:

1.  Nyissa meg a hello alkalmazás hello portálon túl címen**Azure Active Directory**a gombra, majd **vállalati alkalmazások**, majd **összes alkalmazást.** Nyissa meg a hello alkalmazást, majd válasszon **alkalmazásproxy** hello bal oldali menüből.

2.  Tekintse meg hello összekötő csoport mező. Ha nincs aktív összekötők hello csoport, egy figyelmeztetés megjelenítése. Ha nem látja a figyelmeztetéseket, helyezze át, amely túl "Ellenőrizze az összes szükséges portok szerepel az engedélyezési listán".

3.  Ha ez hello megfelelő összekötő csoporthoz hello legördülő lista tooselect hello megfelelő csoport használja, és győződjön meg róla, többé nem láthatja a figyelmeztetéseket. Ha ez hello szánt összekötő csoport hello figyelmeztető üzenet tooopen hello lap összekötő felügyeleti parancsra.

4.  Itt van néhány módon toodrill a további:

  * Helyezze át egy aktív összekötő hello csoportba: Ha egy aktív összekötő, amely toothis csoporthoz kell tartoznia, és nem sor a láthatáron toohello célalkalmazás háttér, hello összekötő áthelyezheti hozzárendelt hello csoportjához. toodo kattintson hello összekötő. A hello "Összekötő csoport" mezőben hello legördülő tooselect hello megfelelő csoportot, és kattintson a Mentés gombra.

  * A csoport új összekötő letöltése: ezen az oldalon kaphat hello hivatkozás túl[új összekötő letöltése](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). hello összekötő igények toobe közvetlen toohello a háttéralkalmazás a gépre telepítve, és általában el van helyezve hello hello alkalmazás ugyanarra a kiszolgálóra. Használjon hello összekötő hivatkozás toodownload alakzatot hello célgépen összekötő letöltése. A következő hello összekötő kattintson, és használja a hello "Összekötő csoport" legördülő toomake meg arról, hogy toohello megfelelő csoport tartozik.

  * Vizsgálja meg az inaktív csatlakozó: összekötő inaktívként jeleníti meg, akkor nem tooreach hello szolgáltatást. Ez általában miatt toosome szükséges portok blokkolva van. toosolve ezt a problémát, áthelyezés túl "Ellenőrizze az összes szükséges portjait szerepel az engedélyezési listán".

Az alábbi lépések végrehajtásával után tooensure hello alkalmazás hozzárendelt tooa csoportban található, összekötők, hello tesztalkalmazás újra működik. Ha az eszköz még nem működik, továbbra is toohello a következő szakaszban.

## <a name="check-all-required-ports-are-whitelisted"></a>Ellenőrizze az összes szükséges portok szerepel az engedélyezési listán

tooverify, hogy az összes szükséges portok nyitva, lásd a dokumentációban a portok megnyitása. Ha az összes hello szükséges portok nyitva, helyezze át a következő szakaszban toohello.

## <a name="check-for-other-connector-errors"></a>A többi összekötőt hibák

Ha nincs a fenti hello hello probléma megoldása érdekében hello következő lépésre toolook hello összekötő magát a hibák és problémák. Láthatja, hogy néhány gyakori hibáinak hello [kapcsolatos problémák elhárítása a dokumentum](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

Is megtalálhatja közvetlenül hello összekötő naplók tooidentify ki a hibákat. A hibaüzenetek sok kell tudni tooshare több konkrét javaslatokkal szolgál a javításokat. Hogyan tooview hello naplókat, lásd: toolearn [összekötők dokumentációban](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="additional-resolutions"></a>További megoldások

Ha a fenti hello nem hello probléma megoldása érdekében néhány másik lehetséges oka is van. tooidentify hello problémát:

Ha az alkalmazáshoz beállított toouse integrált Windows-hitelesítéssel (IWA), hello tesztalkalmazást az egyszeri bejelentkezés nélkül. Ha nem, akkor helyezze át a következő bekezdés toohello. toocheck hello alkalmazás nélkül egyszeri bejelentkezést, nyissa meg az alkalmazás keresztül **vállalati alkalmazások** és toohello **egyszeri bejelentkezés** menü. Változás hello legördülő listán a "Integrált Windows-hitelesítés" túl "az Azure AD az egyszeri bejelentkezés le van tiltva". 

Most nyisson meg egy böngészőt, és próbálja meg újból a tooaccess hello alkalmazást. A hitelesítéshez a rendszer kéri, és feltölti a hello alkalmazás. Ha minden megfelelően működik, hello hiba, amely lehetővé teszi, hogy hello egyszeri bejelentkezés hello Kerberos által korlátozott delegálás (KCD) beállításában van. Lásd: hello Kerberos által korlátozott Delegálás hibaelhárítás lap.

Ha toosee hello hiba továbbra is, nyissa meg toohello gép ahol hello összekötő telepítve van, nyisson meg egy böngészőt, és megpróbál tooreach hello belső használt URL-cím hello alkalmazás. hello összekötő úgy viselkedik, mint egy másik ügyfél hello egyazon számítógépen. Ha hello alkalmazás nem érhető el, vizsgálja meg, hogy a gép nem tooreach hello alkalmazás oka vagy az összekötő olyan kiszolgálóra, amely képes tooaccess hello alkalmazás.

Ha az, hogy a gép hello összekötő magát a hibák és problémák toolook hello alkalmazás érhető el. Láthatja, hogy néhány gyakori hibáinak hello [kapcsolatos problémák elhárítása a dokumentum](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). Is megtalálhatja közvetlenül hello összekötő naplók tooidentify ki a hibákat. A hibaüzenetek sok kell tudni tooshare több konkrét javaslatokkal szolgál a javításokat. Hogyan tooview hello naplókat, lásd: toolearn [összekötők dokumentációban](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="next-steps"></a>Következő lépések
[Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)
