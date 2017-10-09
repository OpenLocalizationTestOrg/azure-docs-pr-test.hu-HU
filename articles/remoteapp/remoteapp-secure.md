---
title: "aaaSecure alkalmazásokat és erőforrásokat az Azure Remoteappban |} Microsoft Docs"
description: "Megtudhatja, hogyan toolock le az alkalmazásokat és erőforrásokat az Azure Remoteappban"
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
ms.openlocfilehash: 26325826e92855a12a0087f19a3e32cbe1116449
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Biztonságos alkalmazásokhoz és erőforrásokhoz az Azure Remoteappban
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp biztosít a felhasználók számára hozzáférést toocentrally által felügyelt Windows-alkalmazások, amelyek segítségével szabályozhatja, hogy a felhasználók mit és nem hajthatja végre.  Ez akkor különösen hasznos, ha hello felhasználó egy nem felügyelt eszközön (például a személyes Macbook), és csatlakozás kívánt toocontrol hello felhasználói hozzáférés vagy tapasztalhat.

Például ha a felhasználók az alkalmazásból az adatok másolásának szeretné tooprevent Active Directory használata a felhasználók hitelesítéséhez, konfigurálhatja az adatok másolása egy távoli asztali csoportházirend tooblock felhasználókat.

Egy másik példa: Ha azt szeretné, tooblock internet-hozzáféréssel egy adott alkalmazás a gyűjteményben. A Windows tűzfal szabályt, hogy a blokkok hello lemezképet a gyűjtemény létrehozásakor hello hozzáférés hozhat létre.

## <a name="implementation-options"></a>Végrehajtási beállítások
  Az alábbiakban hello kulcs végrehajtási beállítások, egyénileg vagy párhuzamosan használható, igény szerint:

1. Ha a RemoteApp-gyűjteménnyel tartományhoz kényszerítheti a [csoportházirend](https://technet.microsoft.com/library/cc725828.aspx) (hello üresjáratban és a kapcsolat bontása időtúllépés házirendek leírt hello kivétellel [Itt](../azure-subscription-service-limits.md)).
2. Alternatív tooGroup (Ha a gyűjtemény nincs tartományhoz csatlakoztatva, vagy Önnek nincs megfelelő jogosultsága hello az Active Directory), mint konfigurálható [helyi házirendek](https://technet.microsoft.com/library/cc775702.aspx) be a sablon lemezképe.  Vegye figyelembe, hogy a csoport adu helyi házirendek házirendeket, ha a ütközés.
3. Egyes operációs rendszer vagy alkalmazás beállításai nincsenek házirend állíthatók be, de lehet, beállításkulcs használatával hello keresztül [RegEdit eszköz](remoteapp-hybridtrouble.md) a sablon lemezképe konfigurálása közben.
4. Használhat [Windows tűzfal](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) toocontrol hálózati hozzáférési tooand hello hello alkalmazást futtató számítógépről. Ne feledje hello URL-címek és portok az itt megadott nincs blokkolás.
5. Használhat [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) toocontrol, amely a felhasználók alkalmazásokat és fájlokat futtathatják. Például lelkes felhasználók ismerhetik hogyan toorun alkalmazások, amelyek nem tette közzé, de elérhető az hello kép akkor használt toocreate hello gyűjtemény, mert az AppLocker képes blokkolni a ezt.

## <a name="detailed-information"></a>Részletes információk
* a következő távoli asztali szolgáltatások házirendek hello valószínűleg toobe leghasznosabb:
  * [Eszközök és erőforrások átirányítását](https://technet.microsoft.com/library/ee791794.aspx)
  * [Nyomtatóátirányítás](https://technet.microsoft.com/library/ee791784.aspx)
  * [Profilok](https://technet.microsoft.com/library/ee791865.aspx).
* Vegye figyelembe, hogy a konfigurálás átirányítások keresztül RemoteApp PowerShell-modul hello (mint [Itt](remoteapp-redirection.md)) támaszkodik hello gép tooenforce hello ügyfélházirend, így ha a biztonság hello elsődleges célja tooenforce hello házirend segítségével hello sablonházirendjének kép helyi vagy csoportházirend segítségével.
* [Windows Server 2012 R2 házirendek](https://technet.microsoft.com/library/hh831791.aspx).
* [Office 2013 házirendek](https://technet.microsoft.com/library/cc178969.aspx) (beleértve a [hogyan toocustomize hello Office eszköztár](https://technet.microsoft.com/library/cc179143.aspx)).

