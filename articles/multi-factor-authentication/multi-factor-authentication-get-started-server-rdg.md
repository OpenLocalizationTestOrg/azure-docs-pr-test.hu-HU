---
title: "aaaRDG és az Azure MFA kiszolgáló RADIUS használata |} Microsoft Docs"
description: "Ez a távoli asztal (RD) átjáró és az Azure multi-factor Authentication kiszolgáló RADIUS használata segít hello Azure multi-factor Authentication hitelesítési lapot."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f2354ac4-a3a7-48e5-a86d-84a9e5682b42
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: fd280e9b6ff90c82927cffe593c4f1fda7047842
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Távoli asztali átjáró és RADIUS-t használó Azure Multi-Factor Authentication-kiszolgáló
Gyakran a távoli asztal (RD) átjáró hello helyi hálózati szolgáltatások házirend-tooauthenticate felhasználók használja. Ez a cikk ismerteti, hogyan tooroute RADIUS kér kimenő hello távoli asztali átjáró (keresztül hello helyi hálózati házirend-kiszolgáló) toohello multi-factor Authentication kiszolgáló. hello Azure MFA és a távoli asztali átjáró azt jelenti, hogy a felhasználó hozzáférhet-e a munkahelyi környezetben bárhonnan erős hitelesítés végrehajtása közben. 

Terminálszolgáltatások a Windows-hitelesítés nem támogatott Server 2012 R2-höz, mert a távoli asztali átjáró és a RADIUS-toointegrate használata MFA kiszolgáló. 

Hello Azure multi-factor Authentication kiszolgáló telepítése egy különálló kiszolgálón, mely hello RADIUS-kérelmek proxyk toohello hálózati házirend-kiszolgáló biztonsági hello távoli asztali átjárókiszolgáló. Miután a hálózati házirend-kiszolgáló érvényesítése hello felhasználónevét és jelszavát, a válasz toohello multi-factor Authentication kiszolgáló adja vissza. Ezután hello MFA kiszolgáló hello második tényezős hitelesítést hajt végre, és toohello átjáró eredményt adja vissza.

## <a name="prerequisites"></a>Előfeltételek

- Tartományhoz csatlakoztatott Azure MFA-kiszolgáló. Ha nincs ilyen már telepítve van, kövesse hello [Ismerkedés az Azure multi-factor Authentication kiszolgáló hello](multi-factor-authentication-get-started-server.md).
- A hitelesítést az NPS szolgáltatásokkal végző távoli asztali átjáró.

## <a name="configure-hello-remote-desktop-gateway"></a>Távoli asztali átjáró hello konfigurálása
Hello távoli asztali átjáró toosend RADIUS hitelesítési tooan Azure multi-factor Authentication kiszolgáló konfigurálása. 

1. Az RD Átjárókezelőt, kattintson a jobb gombbal a hello kiszolgáló nevét, és válassza ki **tulajdonságok**.
2. Nyissa meg toohello **távoli asztali házirendjeihez** lapot, és válasszon **központi házirend-kiszolgálót futtató**. 
3. Adja hozzá egy vagy több Azure multi-factor Authentication kiszolgáló RADIUS-kiszolgálók hello nevét vagy IP-cím az egyes kiszolgálók megadásával. 
4. Hozzon létre egy közös titkos kulcsot mindegyik kiszolgálóhoz.

## <a name="configure-nps"></a>Az NPS konfigurálása
Távoli asztali átjáró hello hálózati házirend-kiszolgáló toosend hello RADIUS kérelem tooAzure többtényezős hitelesítést használ. hálózati házirend-kiszolgáló tooconfigure, először hello időtúllépés beállításokat módosíthatja tooprevent hello túllépné a távoli asztali átjáró hello kétlépéses ellenőrzés befejezése előtt. Ezt követően frissítse a hálózati házirend-kiszolgáló tooreceive RADIUS-hitelesítés a multi-factor Authentication kiszolgálóról. A következő eljárás tooconfigure hálózati házirend-kiszolgáló hello használata:

### <a name="modify-hello-timeout-policy"></a>Hello időtúllépés házirend módosítása

1. Az NPS, nyissa meg a hello **RADIUS-ügyfél és kiszolgáló** hello bal oldali oszlopban, és válassza a menü **távoli RADIUS-kiszolgálócsoportok**. 
2. Jelölje be hello **TS ÁTJÁRÓ kiszolgáló csoport**. 
3. Nyissa meg toohello **terheléselosztás** fülre. 
4. Mindkét hello módosítása **eldobása előtt válasz nélkül másodpercben** és hello **kiszolgáló nem érhető el, amelynél kérelmek között eltelt másodpercek száma** toobetween 30 és 60 másodperc. (Ha hello kiszolgálón még mindig időtúllépése hitelesítés során, lépjen vissza ide, és növelje a másodpercek száma hello.)
5. Nyissa meg toohello **Authentication-fiókjának** lapra, és ellenőrizze, hogy hello RADIUS-portok megadott egyezés hello portok, hogy a multi-factor Authentication kiszolgáló figyel a következőn hello.

### <a name="prepare-nps-tooreceive-authentications-from-hello-mfa-server"></a>Hálózati házirend-kiszolgáló tooreceive hitelesítések a hello MFA kiszolgáló előkészítése

1. Kattintson a jobb gombbal **RADIUS-ügyfelek** RADIUS-ügyfelek és kiszolgálók hello bal oldali oszlopban, és válassza ki a **új**.
2. Adja hozzá hello Azure multi-factor Authentication kiszolgáló RADIUS-ügyfél. Válasszon egy rövid nevet, és adjon meg egy közös titkos kulcsot.
3. Nyissa meg hello **házirendek** hello bal oldali oszlopban, és válassza a menü **kapcsolatkérelem-házirendek**. Ekkor meg kell jelennie egy TS GATEWAY AUTHORIZATION POLICY nevű házirendnek, amely a távoli asztali átjáró konfigurálásakor lett létrehozva. Ez a házirend a RADIUS-kérelmeket toohello multi-factor Authentication kiszolgáló továbbítja.
4. Kattintson a jobb gombbal a **TS GATEWAY AUTHORIZATION POLICY** elemre, és válassza a **Házirend duplikálása** parancsot. 
5. Nyissa meg a hello új házirendet, majd a toohello **feltételek** fülre.
6. Adjon hozzá egy feltételt, amely megfelel a hello ügyfél rövid nevet a 2. lépésben hello Azure multi-factor Authentication kiszolgáló RADIUS-ügyfél beállítása hello rövid nevét. 
7. Nyissa meg toohello **beállítások** lapot, és válasszon **hitelesítési**.
8. Módosítsa a hitelesítésszolgáltató hello túl**a kiszolgáló-kérelmek hitelesítéséhez szükséges**. Ez a házirend biztosítja, hogy ha a hálózati házirend-kiszolgáló a hello Azure MFA kiszolgáló RADIUS kérelmet kap, hello történik hitelesítés helyileg egy RADIUS kérelem hátsó toohello Azure multi-factor Authentication kiszolgáló, amely egy hurok feltétele küldése helyett. 
9. tooprevent hurok feltétele, győződjön meg arról, hogy fent hello-szabályzat eredeti hello hello új házirend mutassa **kapcsolatkérelem-házirendek** ablaktáblán.

## <a name="configure-azure-multi-factor-authentication"></a>Az Azure Multi-Factor Authentication konfigurálása

hello Azure multi-factor Authentication kiszolgáló RADIUS-proxyként a távoli asztali átjáró és a hálózati házirend-kiszolgáló között van konfigurálva.  Ez egy tartományhoz csatlakoztatott kiszolgálóra végzi, amely távoli asztali átjárókiszolgáló hello nem telepíthető. A következő eljárás tooconfigure hello Azure multi-factor Authentication kiszolgáló hello használata.

1. Nyissa meg a hello Azure multi-factor Authentication kiszolgálót, majd válassza a hello RADIUS-hitelesítés ikonra. 
2. Ellenőrizze a hello **RADIUS-hitelesítés engedélyezése** jelölőnégyzetet.
3. A hello ügyfelek lapon győződjön meg arról, hello portok felel meg, mi van konfigurálva a hálózati házirend-kiszolgáló, majd válassza ki **Hozzáadás**.
4. Adja hozzá a hello távoli asztali átjáró kiszolgáló IP-címe, alkalmazás neve (nem kötelező) és egy közös titkos kulcsot. hello megosztott titkos igények toobe hello azonos hello Azure multi-factor Authentication kiszolgáló és a távoli asztali átjáró.
3. Nyissa meg toohello **cél** lapra, és jelölje be hello **RADIUS-kiszolgálók** választógombot.
4. Válassza ki **Hozzáadás** , és írja be a hello IP-cím, közös titok és portok hello hálózati házirend-kiszolgáló. Ha használja a központi hálózati házirend-kiszolgáló, hello RADIUS-ügyfél és a RADIUS-cél vannak hello azonos. hello közös titkos kulcsot meg kell egyeznie egy telepítő hello hello hello hálózati házirend-kiszolgáló RADIUS-ügyfél szakasz.

![Radius-hitelesítés](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="next-steps"></a>Következő lépések

- Az Azure MFA és az [IIS-webalkalmazások](multi-factor-authentication-get-started-server-iis.md) integrálása

- Választ kaphat a hello [Azure multi-factor Authentication – gyakori kérdések](multi-factor-authentication-faq.md)
