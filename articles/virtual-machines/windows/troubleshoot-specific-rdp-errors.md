---
title: "Azure virtuális gépek aaaSpecific RDP hibaüzenetekben |} Microsoft Docs"
description: "Megérteni a vonatkozó hibaüzeneteket, amelyek jelenhet meg tett kísérlet során használja a távoli asztali kapcsolat tooa Windows rendszerű virtuális gép az Azure-ban"
keywords: "Távoli asztali hiba, a távoli asztali kapcsolat hiba, nem lehet kapcsolódni a tooVM, távoli asztal – hibaelhárítás"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 8e1551be23e696bd60adbd76c3e1ea86d9dd11aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-specific-rdp-error-messages-tooa-windows-vm-in-azure"></a>Hibaelhárítás az adott RDP hiba üzenetek tooa Windows Azure-ban
Egy adott hibaüzenet jelenhet meg, az Azure-ban a távoli asztali kapcsolat tooa Windows rendszerű virtuális gép (VM) használatakor. Ez a cikk részletek néhány gyakori hibaüzenetek észlelt, és hibaelhárítási lépéseket tooresolve hello őket. Ha a Kapcsolódás a virtuális gép tooyour problémák RDP Funkciót használnak a do azonban nem egy adott hibaüzenetet kapja, lásd: hello [hibaelhárítási útmutatója a távoli asztal](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Információ a vonatkozó hibaüzeneteket: hello következő:

* [hello távoli munkamenet meg lett szakítva, mert nincs elérhető távoli asztali licenckiszolgálókat tooprovide licenc](#rdplicense).
* [A távoli asztal nem találja a hello "számítógépnév"](#rdpname).
* [Hitelesítési hiba történt. hello helyi biztonsági szervezet nem érhető el](#rdpauth).
* [Windows biztonsági hiba: A hitelesítő adatok nem működött](#wincred).
* [Ezen a számítógépen nem lehet kapcsolódni a távoli számítógép toohello](#rdpconnect).

<a id="rdplicense"></a>

## <a name="hello-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-tooprovide-a-license"></a>hello távoli munkamenet meg lett szakítva, mert nincs távoli asztali licenckiszolgálókat elérhető tooprovide egy licencet.
OK: lejárt hello 120 napos türelmi időszak hello távoli asztal-kiszolgálói szerepkör, és tooinstall licencekre van szüksége.

A probléma megoldásához hello RDP-fájlt a helyi másolatot mentenek hello portálról, és futtassa a parancsot egy PowerShell-parancssorba tooconnect. Ezzel a lépéssel letiltja az adott kapcsolathoz csak licencelési:

        mstsc <File name>.RDP /admin

Ha több mint két egyidejű távoli asztali kapcsolatok toohello VM ténylegesen nem szükséges, a Kiszolgálókezelő tooremove hello távoli asztali kiszolgáló szerepkör is használhatja.

További információkért lásd: hello blogbejegyzésben [Azure virtuális Gépen sikertelen, és "Nincs távoli asztali licenckiszolgálókat érhető el"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-hello-computer-name"></a>A távoli asztal hello számítógép "name" nem található.
OK: hello távoli asztali ügyfél a számítógép neve nem oldható fel hello hello számítógép hello beállítások hello RDP-fájlt.

A lehetséges megoldásokról:

* Ha egy szervezet intraneten, ellenőrizze, hogy a számítógép hozzáférés toohello proxykiszolgálóval rendelkezik-e, és HTTPS-forgalom tooit küldhet.
* Ha helyileg tárolt RDP-fájl használata esetén próbálja hello egy hello portál által létrehozott használatával. Ez a lépés biztosítja, hogy rendelkezik-e helyes DNS-névvel hello hello virtuális gépet, vagy hello felhőalapú szolgáltatás és virtuális gép hello hello végpont portja. Íme egy minta hello portál által előállított RDP-fájlt:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

hello cím része az RDP-fájlt tartalmaz:

* hello teljesen minősített tartománynevét, amely tartalmazza a virtuális gép ("Dejójáték-azdatatier.cloudapp.net" Ebben a példában) hello hello felhőalapú szolgáltatás.
* hello külső TCP-port hello végpont a távoli asztal forgalmat (55919).

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-hello-local-security-authority-cannot-be-contacted"></a>Hitelesítési hiba történt. nem lehet kapcsolódni a helyi biztonsági szervezet hello.
OK: hello cél virtuális gép nem található hello biztonsági hatóság hello felhasználói részét a hitelesítő adatait.

A felhasználónév esetén hello formában *SecurityAuthority*\\*felhasználónév* (Példa: corp\felhasználó1), hello *SecurityAuthority* részére vagy hello VM számítógép neve (a helyi biztonsági szervezet hello) vagy egy Active Directory-tartomány nevét.

A lehetséges megoldásokról:

* Ha hello fiók helyi toohello VM, győződjön meg arról, hogy hello virtuális gép nevét helyesen írta-e.
* Ha hello fiók Active Directory-tartomány, helyesírás hello hello tartománynév.
* Ha egy Active Directory tartományi fiók, és hello tartomány nevét helyesen írta-e, ellenőrizze, hogy a tartományvezérlő elérhető az adott tartományban. Ez a jelenség a tartományvezérlőnek, hogy a tartományvezérlő nem érhető el, mivel nincs elindítva az Azure virtuális hálózatokhoz. A probléma megoldásához használhatja egy helyi rendszergazdai fiók helyett egy olyan tartományi fiók.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows biztonsági hiba: A hitelesítő adatok nem működik.
OK: a hello cél virtuális gép nem lehet érvényesíteni a fióknevet és jelszót.

A Windows-alapú számítógép ellenőrzéséhez hello vagy helyi fiók, vagy egy olyan tartományi fiók hitelesítő adatait.

* A helyi fiókok használata a hello *számítógépnév*\\*felhasználónév* szintaxis (Példa: SQL1\Admin4798).
* A tartományi fiókokat, használja a hello *tartománynév*\\*felhasználónév* szintaxis (Példa: CONTOSO\peterodman).

Hello jelentkezett be helyi rendszergazdai fiók alakítja át a virtuális gép tooa tartományvezérlő egy új Active Directory-erdő léptette be, ha ugyanaz a hello fiók tooan egyenértékű jelszó hello új erdőben és tartományban. hello helyi fiókot majd törölték.

Például ha hello helyi fiókkal DC1\DCAdmin bejelentkezve, és majd előléptetett hello virtuális gép hello corp.contoso.com tartományhoz tartozó új erdő tartományvezérlőjeként, hello helyi fiók törlése DC1\DCAdmin és egy új (CORP\DCAdmin tartományi fiókot. ) jön létre a hello azonos jelszót.

Győződjön meg arról, hogy hello fióknév hello virtuális gép ellenőrizheti egy érvényes fiókot, és adott hello jelszava helyes nevét.

Ha szüksége toochange hello hello helyi rendszergazdai fiók jelszavát, tekintse meg [hogyan tooreset jelszó vagy a távoli asztal hello szolgáltatás fut a Windows virtuális gépek](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-toohello-remote-computer"></a>Ezen a számítógépen nem lehet kapcsolódni a távoli számítógép toohello.
OK: hello tooconnect által használt fiók nem jogosult a távoli asztal bejelentkezés.

Minden Windows-számítógép a távoli asztali felhasználók helyi csoport, amely tartalmazza a hello fiókokat és csoportokat, amelyek jelentkezhet be távolról rendelkezik. Hello helyi Rendszergazdák csoport tagjai is hozzáférhetnek, annak ellenére, hogy ezek a fiókok nem szereplő hello távoli asztali felhasználók helyi csoportot. Tartományhoz csatlakozó gépek hello helyi Rendszergazdák csoport hello tartomány hello tartományi rendszergazdák is tartalmaz.

Győződjön meg arról, hogy a tooconnect használata hello fiókja rendelkezik-e a távoli asztal bejelentkezési jogokkal. A probléma megoldásához használja a tartomány vagy helyi rendszergazdai fiók tooconnect távoli asztali kapcsolaton keresztül. tooadd hello kívánt fiókot toohello távoli asztali felhasználók helyi csoport, hello Microsoft Management Console beépülő modullal (**Rendszereszközök > helyi felhasználók és csoportok > csoportok > Remote Desktop Users**).

## <a name="next-steps"></a>Következő lépések
Ha ezek a hibák egyike sem történt, és csatlakozás RDP Funkciót használnak ismeretlen hibája, lásd: hello [hibaelhárítási útmutatója a távoli asztal](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Című témakörben leírt lépéseket a virtuális gép futó alkalmazásokhoz való hozzáférés [kapcsolatos problémák elhárítása access tooan alkalmazást egy Azure virtuális Gépen futó](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Ha a Secure Shell (SSH) tooconnect tooa Linux virtuális gép használata az Azure-ban problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooa Linux virtuális gép az Azure-ban](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

