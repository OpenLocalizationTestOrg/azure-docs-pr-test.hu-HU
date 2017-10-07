---
title: "aaaWindows hitelesítés és az Azure MFA kiszolgáló |} Microsoft Docs"
description: "Ez a hello Azure multi-factor Authentication hitelesítés oldal, amely segítséget nyújt a Windows-hitelesítés és az Azure multi-factor Authentication kiszolgáló üzembe helyezése."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 19a4043f-c4ce-43c0-80e7-2548ee92cb74
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 0fc38fd751966bf883d4eae7c48055988922af80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-hitelesítés és Azure Multi-Factor Authentication-kiszolgáló
Hello Azure multi-factor Authentication kiszolgáló tooenable szakasza hello Windows-hitelesítés használatára, és az alkalmazások a Windows-hitelesítés konfigurálása. Windows-hitelesítés beállítása előtt tartsa szem előtt a lista a következő hello:

* A telepítő, a rendszer újraindítása után hello Azure multi-factor Authentication hitelesítés Terminálszolgáltatások tootake hatás.
* Ha "Azure multi-factor Authentication felhasználói egyeztetés megkövetelése" jelölőnégyzet be van jelölve, és nem hello felhasználói listán, nem lesz képes toolog hello géppé a rendszer újraindítása után.
* Megbízható IP-címek függ, hogy hello alkalmazás képes biztosítani a hello ügyfél IP-Címét hello hitelesítés-e. Jelenleg csak a Terminálszolgáltatások támogatott.  

> [!NOTE]
> Ez a funkció nem támogatott toosecure Terminálszolgáltatások a Windows Server 2012 R2 rendszeren.

## <a name="toosecure-an-application-with-windows-authentication-use-hello-following-procedure"></a>az alkalmazás Windows-hitelesítést, a következő eljárás használható hello toosecure.
1. Kattintson az Azure multi-factor Authentication kiszolgáló hello hello Windows-hitelesítés ikonra.
   ![Windows-hitelesítés](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Ellenőrizze a hello **Windows-hitelesítés engedélyezése** jelölőnégyzetet. Alapértelmezés szerint a jelölőnégyzet nincs bejelölve.
3. hello alkalmazások lap révén hello rendszergazda tooconfigure egy vagy több alkalmazást a Windows-hitelesítés.
4. Jelöljön ki egy kiszolgáló vagy az alkalmazás – adja meg, hogy a hello kiszolgáló/alkalmazás engedélyezve van-e. Kattintson az **OK** gombra.
5. Kattintson a **Hozzáadás…** gombra.
6. hello megbízható IP-címek lap lehetővé teszi a megadott IP-címekről tooskip többtényezős hitelesítés a Windows Azure-munkamenetek. Például ha az alkalmazottak hello alkalmazás használatához hello irodából és otthonról, előfordulhat, hogy dönthet úgy, hogy az Azure multi-factor Authentication hello irodában mellőzi. Ehhez a hello irodai alhálózatot megbízható IP-címek bejegyzésének kellene megadnia.
7. Kattintson a **Hozzáadás…** gombra.
8. Válassza ki **egyetlen IP-cím** Ha azt szeretné, hogy tooskip egyetlen IP-címnek.
9. Válassza ki **IP-címtartomány** Ha azt szeretné, hogy tooskip egy teljes IP-címtartományt. Példa: 10.63.193.1-10.63.193.100.
10. Válassza ki **alhálózati** Ha szeretné toospecify IP-címtartományra alhálózati jelöléssel. Hello alhálózat kezdő IP-Címét adja meg, és válasszon hello hello legördülő listából válassza ki a megfelelő alhálózati maszkot.
11. Kattintson az **OK** gombra.

## <a name="next-steps"></a>Következő lépések

- [Külső felektől származó VPN-készülékek konfigurálása Azure MFA-kiszolgálóhoz](multi-factor-authentication-advanced-vpn-configurations.md)

- [A meglévő hitelesítési infrastruktúrát a hálózati házirend-kiszolgáló bővítmény hello kiegészítheti az Azure MFA számára](multi-factor-authentication-nps-extension.md)
