---
title: "Hibaelhárítás a RemoteApp - aaaAzure indítási és kapcsolat alkalmazáshibák |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot kezdési és csatlakozás az Azure Remoteappban tooapplications ad ki."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e5cf7171-d1c2-4053-a38b-5af7821305e1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e51d480c9d3fa1f2076f95b63c7a8cd2f6956a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Hibaelhárítás az Azure RemoteApp - alkalmazások indítási és kapcsolat hibái
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure Remoteappban üzemeltetett alkalmazások néhány különböző okokból toolaunch sikertelen lehet. Ez a cikk ismerteti a különböző okokból és a felhasználók fordulhat elő, amikor hibaüzenetek próbált toolaunch alkalmazások. Az beszél esetén kapcsolódási hibákat is. (De ez a cikk nem foglalkozik a problémák hello Azure RemoteApp-ügyfélen történő bejelentkezéskor.)  

Olvassa el a miatt tooapp indítási és a csatlakozási hibák leggyakoribb hibaüzenetek kapcsolatos információkat.

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Elvégezzük a beállításokat beállítása... Próbálkozzon újra 10 perc múlva.
Ez a hiba azt jelenti, hogy az Azure RemoteApp van vertikális felskálázásával toomeet hello kapacitás szükség a felhasználók. Hello háttérben további Azure RemoteApp Virtuálisgép-példányok létrehozása toohandle hello kapacitási igényeihez a felhasználók. Általában ez körülbelül öt percet vesz igénybe, de akár too10 percet is igénybe vehet. Egyes esetekben ez elég gyorsan nem fordulhat elő, és erőforrások azonnal szükség van-e. Például Reggel 9 forgatókönyv ahol sok felhasználó kell toouse az alkalmazást az Azure RemoteAppn hello ugyanannyi időt vesz igénybe. Ha ez történik, tooyou engedélyezhető-e **kapacitás mód** hello háttér. toodo ebben nyissa meg az Azure támogatási jegy. Bizonyos tooinclude a hello kérelemben szereplő előfizetés-azonosító lehet.  

![A Microsoft kihozhatják beállításához](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-tooyour-applications-please-re-launch-your-application"></a>Sikerült nem automatikus-újracsatlakozás tooyour alkalmazások, indítsa újra el az alkalmazást
Ez a hibaüzenet gyakran előforduló, ha Azure RemoteApp használna, és ezután helyezze a számítógép hosszabb, mint 4 óra toosleep és az majd felébresztette a számítógép és hello Azure RemoteApp ügyfél kísérlet tooauto csatlakozzon újra, és túllépte az időkorlátot.  Kérje meg a felhasználók toonavigate hátsó toohello alkalmazás, és próbálja meg tooopen azt hello Azure RemoteApp-ügyfelet.

![Sikerült nem automatikus-újracsatlakozás tooyour alkalmazások](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-hello-temp-profile"></a>Hello-profillal kapcsolatos problémák
Ez a hiba akkor fordul elő, ha a felhasználói profil (felhasználói Profillemeze) nem sikerült toomount és hello felhasználó kapott az ideiglenes profillal.  A rendszergazdák be kell-e toohello gyűjtemény hello Azure-portálon lépjen, és folytassa a toohello **munkamenetek** lapra, és próbálja meg túl**Kijelentkezés** hello felhasználó. Ez a program kényszerítheti ki hello felhasználói munkamenet - napló, majd rendelkezik hello felhasználói kísérlet toolaunch alkalmazást újra. Ha az Azure támogatási kapcsolattartó nem sikerül.

## <a name="azure-remoteapp-has-stopped-working"></a>Az Azure RemoteApp működése leállt
Ez a hibaüzenet azt jelenti, hogy hello Azure RemoteApp-ügyfélen problémát okoz, és újra toobe kell. Kérje meg a felhasználók tooclose: válasszon **program bezárása** majd indítsa újra a hello Azure RemoteApp-ügyfelet.  Ha hello a probléma továbbra is fennáll, nyitó és az Azure támogatási jegy.

![Az Azure RemoteApp működése leállt](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-hello-connection-or-contact-your-system-administrator"></a>Hiba történt a távoli asztali kapcsolat ehhez az erőforráshoz hozzáférő. Ismételje meg a kapcsolat hello, vagy forduljon a rendszergazdához
Ez az általános hibaüzenet - is felülvizsgáljuk, kérje az Azure támogatási. 

![Általános Azure RemoteApp-üzenet](./media/remoteapp-apptrouble/ra-apptrouble4.png) 

