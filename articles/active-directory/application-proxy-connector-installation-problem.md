---
title: "Alkalmazásproxy-összekötő ügynök aaaProblem telepítése hello |} Microsoft Docs"
description: "Hogyan tootroubleshoot kapcsolatban, hogy előfordulhat, hogy telepítésekor arcfelismerési hello alkalmazásproxy-összekötő ügynök"
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
ms.openlocfilehash: 07ac366a429083af0c9b87aa9df9cf3876132b90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-proxy-agent-connector"></a>A probléma hello ügynök Proxy összekötőjének telepítése

Microsoft AAD alkalmazásproxy-összekötő egy belső tartomány-összetevő által használt kimenő kapcsolatok tooestablish hello kapcsolat hello felhő elérhető végpontot toohello belső tartomány.

## <a name="general-problem-areas-with-connector-installation"></a>Általános problémás területek Connector telepítése

Ha egy összekötő hello telepítése nem sikerül, hello okozza-e általában hello a következő területek közül:

1.  **Kapcsolat** – toocomplete a sikeres telepítés hello új összekötő igények tooregister, és létrehozza a jövőbeli megbízhatósági tulajdonságok. Csatlakozás toohello AAD alkalmazásproxy felhőalapú szolgáltatás végzi.

2.  **Megbízhatósági kapcsolat kialakítása** – hello új összekötőt hoz létre egy önaláírt tanúsítványt, és regisztrálja a toohello felhőalapú szolgáltatás.

3.  **Üdvözöljük a rendszergazdákat a hitelesítési** – a telepítés során hello felhasználónak meg kell adnia rendszergazdai hitelesítő adataival toocomplete hello összekötő telepítése.

## <a name="verify-connectivity-toohello-cloud-application-proxy-service-and-microsoft-login-page"></a>Ellenőrizze a kapcsolat toohello Felhőbeli Proxy szolgáltatás és a Microsoft Login lap

**Cél:** győződjön meg arról, hogy hello összekötő gép is kapcsolódnak toohello AAD alkalmazásproxy regisztrációs végpont, valamint a Microsoft bejelentkezési oldalt.

1.  Nyisson meg egy böngészőt, majd a toohello, a következő weblapot: <https://aadap-portcheck.connectorporttest.msappproxy.net> , és győződjön meg arról, hogy hello kapcsolat tooCentral USA és az USA keleti régiója adatközpontok porttal 9090 és 9091 megfelelően működik.

2.  Ha ezeket a portokat bármelyike nem sikeres (zöld pipa nem rendelkezik), győződjön meg arról, hogy hello tűzfal vagy a háttér-proxyra \*. msappproxy.net 9090 és helyesen definiálva 9091 porttal.

3.  Nyisson meg egy böngészőt (külön lapon), és nyissa meg a következő weblapot toohello: <https://login.microsoftonline.com>, győződjön meg arról, hogy toothat lap bejelentkezhet.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Ellenőrizze a gép és a háttérkiszolgáló összetevői támogatják az alkalmazásproxy megbízható tanúsítvány

**Cél:** ellenőrizze hello összekötő számítógéphez, a háttér-proxy és a tűzfal jövőbeli megbízhatósági hello-összekötő által létrehozott hello tanúsítvány is támogatja.

>[!NOTE]
>hello összekötő megkísérli toocreate TLS1.2 által támogatott SHA512 tanúsítványt. Ha hello gép vagy a hello háttértűzfalat és a proxy nem támogatja a TLS1.2, hello telepítése sikertelen.
>
>

**tooresolve hello problémát:**

1.  Ellenőrizze a gép hello támogatja-e TLS1.2 – 2012 R2 után minden Windows-verziók támogatnia kell a TLS 1.2-es. Ha az összekötő gép 2012 R2 verzióját vagy előtt, győződjön meg arról, hogy hello hello számítógépen telepítve vannak a következő KB: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  A hálózati rendszergazdától, és tooverify kérje meg, hogy hello háttér proxy és tűzfal nem blokkolja SHA512 a kimenő forgalmat.

## <a name="verify-admin-is-used-tooinstall-hello-connector"></a>Ellenőrizze a használt tooinstall hello összekötő feladata

**Cél:** győződjön meg arról, hogy hello felhasználó megpróbál tooinstall hello összekötő rendszergazda helyes hitelesítő adatokkal. Jelenleg hello felhasználói hello telepítési toosucceed egy globális rendszergazdának kell lennie.

**tooverify hello hitelesítő adatai helyesek:**

Csatlakozás túl<https://login.microsoftonline.com> és hello ugyanazokat a hitelesítő adatokat használja. Ellenőrizze, hogy hello bejelentkezési sikeres. Hello felhasználói szerepkör túl megnyitásával ellenőrizheti**Azure Active Directory**  - &gt; **felhasználók és csoportok**  - &gt; **minden felhasználó**. 

Válassza ki a felhasználói fiókot, majd "Directory szerepkör" hello eredményül kapott menüben. Győződjön meg arról, hogy hello kijelölt szerepkör "Globális rendszergazda". Ha bármelyik hello lépések mentén lapok nem tooaccess, nem egy globális rendszergazdaként.

## <a name="next-steps"></a>Következő lépések
[Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)
