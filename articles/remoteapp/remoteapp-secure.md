---
title: "Biztonságos alkalmazásokhoz és erőforrásokhoz az Azure Remoteappban |} Microsoft Docs"
description: "Megtudhatja, hogyan zárolását, alkalmazásokat és erőforrásokat az Azure Remoteappban"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7fbade87-a453-426d-bfa5-c72227ea83cd
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 1c052906788f0f4fe4ca9fd6d3af63336245174a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Biztonságos alkalmazásokhoz és erőforrásokhoz az Azure Remoteappban
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az Azure RemoteApp központilag felügyelt Windows-alkalmazások, amelyek segítségével szabályozhatja, hogy a felhasználók mit és nem hajthatja végre hozzáférést biztosít a felhasználók számára.  Ez akkor különösen hasznos, ha a felhasználó kapcsolódik (például a személyes Macbook) egy nem felügyelt eszközről, és szabályozhatja a hozzáférést a felhasználónak vagy tapasztalhat.

Például ha az Active Directory használata a felhasználók hitelesítéséhez és azt szeretné, hogy a felhasználók ne az alkalmazásból az adatok másolásának, konfigurálása a távoli asztal csoportházirend a felhasználók blokkolása az adatok másolása

Egy másik példa, hogy szeretné-e a gyűjtemény egy adott alkalmazás internet-hozzáférésének blokkolása. Létrehozhat egy Windows tűzfalszabály, amely blokkolja a hozzáférést, a lemezképet a gyűjtemény létrehozásakor.

## <a name="implementation-options"></a>Végrehajtási beállítások
  Az alábbiakban a kulcs megvalósítási beállításokat, amelyek egyedileg vagy párhuzamosan használható, igény szerint:

1. Ha a RemoteApp-gyűjteménnyel tartományhoz kényszerítheti a [csoportházirend](https://technet.microsoft.com/library/cc725828.aspx) (kivételével leírt üresjáratban és leválasztási időtúllépés házirendek [Itt](../azure-subscription-service-limits.md)).
2. Másik megoldásként a csoportházirend (Ha a gyűjtemény nincs tartományhoz csatlakoztatva, vagy Önnek nincs megfelelő jogosultsága az Active Directory), konfigurálhatja a [helyi házirendek](https://technet.microsoft.com/library/cc775702.aspx) be a sablon lemezképe.  Vegye figyelembe, hogy a csoport adu helyi házirendek házirendeket, ha a ütközés.
3. Egyes operációs rendszer vagy alkalmazás beállításai nincsenek házirend állíthatók be, de keresztül beállításjegyzékbeli kulcs használatával lehet a [RegEdit eszköz](remoteapp-hybridtrouble.md) a sablon lemezképe konfigurálása közben.
4. Használhat [Windows tűzfal](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) hálózati elérés az alkalmazást futtató gépről és. Ne feledje nem tiltja le az URL-címek és portok az itt megadott.
5. Használhat [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) mely alkalmazásokat és fájlokat a felhasználók futtathatják a vezérlőhöz. Például lelkes felhasználók kitalálja, hogy, hogy nem tette közzé, de, amelyek elérhetők a lemezképet a gyűjtemény létrehozásához használt – az AppLocker képes blokkolni a ezt alkalmazások futtatása.

## <a name="detailed-information"></a>Részletes információk
* A távoli asztali szolgáltatások alábbi házirendek várhatóan lehet hasznos:
  * [Eszközök és erőforrások átirányítását](https://technet.microsoft.com/library/ee791794.aspx)
  * [Nyomtatóátirányítás](https://technet.microsoft.com/library/ee791784.aspx)
  * [Profilok](https://technet.microsoft.com/library/ee791865.aspx).
* Vegye figyelembe, hogy konfigurálása a RemoteApp PowerShell-modul segítségével átirányítások (mint [Itt](remoteapp-redirection.md)) az ügyfélszámítógépen. a házirend kikényszerítésére, így ha a biztonság az elsődleges cél szeretné kényszeríteni a szabályzatot, a sablon lemezképe keresztül támaszkodik helyi házirend vagy csoportházirend segítségével.
* [Windows Server 2012 R2 házirendek](https://technet.microsoft.com/library/hh831791.aspx).
* [Office 2013 házirendek](https://technet.microsoft.com/library/cc178969.aspx) (beleértve a [testreszabása az Office eszköztár](https://technet.microsoft.com/library/cc179143.aspx)).

