---
title: "aaaSet be a StorSimple eszköz webes proxy |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Windows PowerShell StorSimple tooconfigure a webalkalmazás-proxy-beállításaiban a StorSimple eszköz."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6c2ca351-a7c6-4da6-ab5e-c081e6d08261
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: c0213eb2a1902ff994147bb16593f0ff300eb70c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>A StorSimple eszköz webalkalmazás-proxy konfigurálása
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag leírja, hogyan toouse Windows PowerShell StorSimple tooconfigure, és tekintse meg a webalkalmazás-proxy-beállításaiban a StorSimple eszközt. hello webproxy beállításai hello StorSimple eszköz által használt hello felhő való kommunikáció során. Proxy-webkiszolgáló használt tooadd, biztonsági réteget tartalom szűréséhez, gyorsítótár tooease sávszélesség-követelményekkel, és az analytics is segíthet.

Webalkalmazás-proxy a StorSimple eszköz választható konfigurációs. StorSimple webproxy csak a Windows PowerShell segítségével konfigurálható. hello konfigurációs az alábbiak szerint lépésből áll:

1. Először konfigurálnia webproxy beállításai hello beállítása varázslót vagy a Windows Powershellen keresztül StorSimple parancsmagok.
2. Ezután engedélyeznie konfigurált hello webproxy beállításai Windows PowerShell parancsmagok StorSimple.

Webproxy-konfigurációja hello befejezése után StorSimple hello Microsoft Azure StorSimple Manager szolgáltatás és a Windows PowerShell hello is megtekintheti konfigurált hello webproxy beállításai. 

Ez az oktatóanyag elolvasása, után fogja tudni:

* Webalkalmazás-proxy konfigurálása telepítővarázslóját, és a parancsmagok használatával
* Engedélyezze a webproxy-parancsmagok használatával
* Webes proxy beállításainak megtekintése a klasszikus Azure portálon hello
* Webproxy konfigurálásakor felmerülő hibák elhárítása

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Windows PowerShell segítségével a webproxy konfigurálása a StorSimple
Használja a következő tooconfigure webproxy beállításai hello:

* A telepítő varázsló tooguide hello konfigurációs lépéseit.
* Parancsmagok a Windows PowerShell-lel.

Mindkét módszerhez a következő szakaszok hello ismertet.

## <a name="configure-web-proxy-via-hello-setup-wizard"></a>Hello beállítása varázsló segítségével webalkalmazás-proxy konfigurálása
Webproxy-konfigurációja le a hello lépések hello beállítása varázsló tooguide használható. Hajtsa végre a hello követő lépéseket tooconfigure webproxy az eszközön.

#### <a name="tooconfigure-web-proxy-via-hello-setup-wizard"></a>webproxy tooconfigure keresztül hello beállítása varázsló
1. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési** , és adja meg a hello **eszköz rendszergazdai jelszava**. Típus hello következő parancsot a telepítő varázsló munkamenet toostart:
   
    `Invoke-HcsSetupWizard`
2. Ha hello első alkalommal használja a hello beállítása varázslót az eszközregisztrációhoz tartozó, szüksége lesz tooconfigure összes hello szükséges hálózati beállításokat addig hello webproxy-konfigurációja. Ha az eszköz már regisztrálva van, elfogadhatja összes hello konfigurált hálózati beállításokat, amíg el nem éri hello webproxy-konfigurációja. Hello beállítása varázslóban, amikor a kért tooconfigure webalkalmazás-proxy beállításait, írja be **Igen**.
3. A hello **webes Proxy URL-címe**, adja meg a hello IP-címet, vagy hello teljesen minősített tartományneve (FQDN) a webes proxy server és hello TCP-port számát, hogy milyen a eszköz toouse hello felhő való kommunikáció során. A következő formátumban hello használata:
   
    `http://<IP address or FQDN of hello web proxy server>:<TCP port number>`
   
    Alapértelmezés szerint a TCP-portszámot 8080 van megadva.
4. Válassza ki a hello hitelesítési típus szerint **NTLM**, **alapvető**, vagy **nincs**. Alapszintű hello legkevésbé biztonságos hitelesítést hello proxykiszolgáló-konfiguráció van. NT LAN Manager-(NTLM-) értéke magas biztonságos és összetett hitelesítés használt protokoll, amely egy háromutas üzenetkezelési rendszeréhez (néha négy sértetlenségét szükség esetén) a felhasználó tooauthenticate. hello alapértelmezett hitelesítés NTLM. További információkért lásd: [alapvető](http://hc.apache.org/httpclient-3.x/authentication.html) és [NTLM-hitelesítés](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **A StorSimple Manager szolgáltatás hello hello eszköz figyelési diagramok nem működnek a Basic, vagy az NTLM-hitelesítés nincs engedélyezve az hello proxykiszolgálót hello eszközhöz. Hello diagramok toowork figyelését, tooensure kell, hogy a hitelesítési tooNONE van beállítva.**
   > 
   > 
5. Ha hitelesítést használ, adja meg a **webes Proxy-felhasználónév** és egy **webes Proxy jelszó**. Tooconfirm hello jelszó is szüksége lesz.
   
    ![Webalkalmazás-Proxy konfigurálása a StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Ha regisztrál az eszközhöz tartozó hello először, folytassa a hello regisztrációs. Ha az eszköz már regisztrálva van, hello varázsló kilép. hello konfigurált beállítások a rendszer menti.

Webalkalmazás-proxy most is engedélyezve lesz. Kihagyhatja hello [webes proxy engedélyezése](#enable-web-proxy) . lépés:, és lépjen közvetlenül túl[webes proxy beállításainak megtekintése a klasszikus Azure portálon hello](#view-web-proxy-settings-in-the-azure-classic-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>StorSimple-parancsmagokhoz tartozó Windows PowerShell segítségével webalkalmazás-proxy konfigurálása
Egy másik módja tooconfigure webproxy beállításai hello Windows PowerShell parancsmagok StorSimple keresztül van. Hajtsa végre a következő lépéseket tooconfigure webproxy hello.

#### <a name="tooconfigure-web-proxy-via-cmdlets"></a>webproxy tooconfigure parancsmagok segítségével
1. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. Amikor a rendszer kéri, adja meg a hello **eszköz rendszergazdai jelszava**. hello alapértelmezett jelszó `Password1`.
2. Hello parancsot, írja be a parancssorba:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Adja meg, és hello jelszót, erősítse meg, alább látható módon.
   
    ![Webalkalmazás-Proxy konfigurálása a StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

hello webproxy van konfigurálva, és toobe engedélyezni kell.

## <a name="enable-web-proxy"></a>Webalkalmazás-proxy engedélyezése
Webalkalmazás-proxy alapértelmezés szerint le van tiltva. A StorSimple eszköz hello webes proxy beállításainak konfigurálása után szükséges toouse hello Windows PowerShell StorSimple tooenable hello webproxy beállításai.

> [!NOTE]
> **Ezt a lépést nem lesz szükség, ha hello beállítása varázsló tooconfigure webproxy használt. Webalkalmazás-proxy automatikusan alapértelmezés szerint engedélyezve van a telepítő varázsló munkamenet után.**
> 
> 

Hajtsa végre a következő lépéseket a Windows PowerShell StorSimple tooenable webes proxy eszközén hello:

#### <a name="tooenable-web-proxy"></a>webalkalmazás-proxy tooenable
1. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. Amikor a rendszer kéri, adja meg a hello **eszköz rendszergazdai jelszava**. hello alapértelmezett jelszó `Password1`.
2. Hello parancsot, írja be a parancssorba:
   
    `Enable-HcsWebProxy`
   
    Most már engedélyezte a hello webproxy konfigurálása a StorSimple eszköz.
   
    ![Webalkalmazás-Proxy konfigurálása a StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-hello-azure-classic-portal"></a>Webes proxy beállításainak megtekintése a klasszikus Azure portálon hello
hello webproxy beállításai hello Windows PowerShell felületén keresztül konfigurált, és nem módosítható hello klasszikus portálon. Azonban megtekintheti a konfigurált beállítások hello a klasszikus portálon. Hajtsa végre a következő lépéseket tooview webproxy hello.

#### <a name="tooview-web-proxy-settings"></a>tooview webproxy beállításai
1. Keresse meg a túl**StorSimple Manager szolgáltatás > eszközök**. Válassza ki, és kattintson arra az eszközre, és keresse meg a túl**konfigurálása**.
2. Görgessen lefelé a hello **konfigurálása** túl lapon**webalkalmazás-proxy beállításainak** szakasz. Konfigurált hello webproxy beállításai megtekintheti a StorSimple eszköz alább látható módon.
   
    ![Nézet webes Proxy felügyeleti portálon](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)

## <a name="errors-during-web-proxy-configuration"></a>Webproxy konfigurálásakor felmerülő hibák
Ha hello webproxy beállításai helytelenül vannak konfigurálva, hibaüzenetek nem megjelenített toohello felhasználó a Windows PowerShell-lel. hello a következő táblázat ismerteti, ezeket a hibaüzeneteket, a lehetséges okairól és a javasolt művelet.

| Sorszám. | HRESULT hibakód | Probléma lehetséges kiváltó okai | Javasolt művelet |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Hello passzív vezérlőből parancs fut, és nem tudja toocommunicate hello aktív vezérlővel. |Hello parancs futtatható hello aktív vezérlő. toorun hello parancs hello passzív vezérlőből, szüksége lesz a passzív tooactive vezérlő toofix hello kapcsolat. Microsoft Support tooengage szüksége lesz, ha a kapcsolat megszakad. |
| 2. |0x800710dd - hello művelet azonosítója érvénytelen |Proxybeállítások a StorSimple virtuális eszköz nem támogatottak. |Proxybeállítások a StorSimple virtuális eszköz nem támogatottak. Ezek a fizikai StorSimple eszközön csak konfigurálható. |
| 3. |0x80070057 - paraméter érvénytelen |Hello proxybeállítások megadott hello paraméterek egyike nem érvényes. |hello URI azonosítója nincs megadva, a megfelelő formátumban. A következő formátumban hello használata:`http://<IP address or FQDN of hello web proxy server>:<TCP port number>` |
| 4. |0x800706BA jelű - RPC-kiszolgáló nem érhető el |hello okozza-e hello a következők egyikét:</br></br>Fürt nem működik-e.</br></br>DataPath szolgáltatás nem fut.</br></br>Hello parancsot futtatja a passzív vezérlőről, és nem tudja toocommunicate hello aktív vezérlővel. |Adjon vállalhat Microsoft Support tooensure, amely a fürt hello működik-e, és datapath szolgáltatás fut..</br></br>Hello parancsot a hello aktív vezérlő. Ha azt szeretné, hogy toorun hello parancs hello passzív vezérlőből, szüksége lesz a tooensure hello passzív vezérlő kommunikálhatnak hello aktív vezérlő. Microsoft Support tooengage szüksége lesz, ha a kapcsolat megszakad. |
| 5. |0X800706be - RPC-hívása sikertelen volt |Fürt nem működik. |Adjon megszólítása Microsoft Support, amely a fürt hello tooensure működik-e. |
| 6. |0x8007138f - fürt erőforrás nem található |Platform szolgáltatás fürt erőforrás nem található. Ez akkor fordulhat elő, amikor hello telepítés nem volt megfelelő. |Szükség lehet tooperform a gyári beállításokat az eszközön. Szükség lehet toocreate platform erőforrás. Forduljon a Microsoft Support további lépéseket. |
| 7. |0x8007138c - fürt erőforrás nincs online állapotban |Platform vagy datapath fürt erőforrás nincs online állapotban. |Forduljon a Microsoft Support toohelp győződjön meg arról, hogy hello datapath és platform szolgáltatás erőforrás online-e. |

> [!NOTE]
> * nincs teljes körű hello fent hibaüzenetek listáját. 
> * Hibák kapcsolódó tooweb proxybeállítások a hello a StorSimple Manager szolgáltatás a klasszikus Azure portálon nem lesznek megjelenítve. Ha a webalkalmazás-proxy probléma van a hello konfiguráció befejezése után, hello eszköz állapota megváltozik a túl**Offline** hello a klasszikus portálon. |}
> 
> 

## <a name="next-steps"></a>Következő lépések
* Ha probléma merül fel az eszköz üzembe helyezése vagy webes proxy beállításainak konfigurálása során, tekintse meg a túl[hibaelhárítása a StorSimple eszköztelepítő](storsimple-troubleshoot-deployment.md).
* Hogyan toouse hello StorSimple Manager szolgáltatás, túl Ugrás toolearn[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

