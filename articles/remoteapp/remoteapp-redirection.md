---
title: "az Azure Remoteappban aaaUsing átirányítási |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure és -felhasználási átirányítás a RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 2c8c867f-4907-4f2e-9ccd-2eb82bb5b837
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d5739a75cf606bd971268da67b2c5ff0fe5fe19b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a>Az Azure Remoteappban átirányítással
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Eszközátirányítás lehetővé teszi, hogy a felhasználók távoli alkalmazások hello eszközök csatolt tootheir helyi számítógépen, telefonját vagy táblagépét kommunikál. Például hogy megadta az Azure RemoteApp Skype, a felhasználó számára szükséges hello kamera telepítve a számítógépen toowork a Skype. Ez akkor is igaz, nyomtatók, hangszórók, figyelők és a tartomány az USB-csatlakozóval csatlakoztatott perifériák.

RemoteApp hello Remote Desktop Protocol (RDP) és a RemoteFX tooprovide átirányítási kihasználja.

## <a name="what-redirection-is-enabled-by-default"></a>Milyen átirányítás alapértelmezés szerint engedélyezve van?
RemoteApp használata esetén a következő átirányítások hello alapértelmezés szerint engedélyezve vannak. hello információk zárójelben hello RDP-beállítás megjelenítése.

* Hang lejátszása hello helyi számítógépen (**ezen a számítógépen lejátszása**). (audiomode:i:0)
* A hello helyi számítógép és a küldési toohello távoli számítógépről hang rögzítése (**erről a számítógépről rögzítse**). (audiocapturemode:i:1)
* Nyomtatási toolocal nyomtatók (redirectprinters:i:1)
* COM-portokhoz (redirectcomports:i:1)
* Intelligens kártya eszköz (redirectsmartcards:i:1)
* Vágólap (képességét toocopy és a Beillesztés) (redirectclipboard:i:1)
* Törölje a típus betűsimítás (betűsimítás engedélyezése: i:1)
* Támogatott Plug and Play eszközök átirányítása. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Milyen más átirányítási érhető el?
Két átirányítási alapesetben le vannak tiltva:

* Átirányítás (meghajtó-hozzárendeléssel): A helyi számítógép meghajtók hello távoli munkamenetben csatlakoztatott meghajtók válnak. Ez lehetővé teszi, hogy menti vagy a helyi meghajtók a megnyitott fájlok munka hello távoli munkamenet közben.
* USB-átirányítása: hello távoli munkamenetben hello USB eszközök csatolt tooyour helyi számítógépen is használhatja.

## <a name="change-your-redirection-settings-in-remoteapp"></a>A RemoteApp a mappaátirányítási beállítások módosítása
SDK-val hello Microsoft Azure PowerShell használatával módosíthatja a hello eszköz Mappaátirányítási beállítások gyűjteménye. Telepítése után új PowerShell és az SDK hello, először konfigurálja előfizetése kezeléséhez ismertetett toomanage [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

Kövesse a következő tooset hello egyéni RDP tulajdonságai parancs hasonló toohello:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Vegye figyelembe, hogy  *`n*  használt elválasztó egyes tulajdonságai között.)

tooget mely egyéni RDP-tulajdonságok vannak konfigurálva, futtassa a következő parancsmag hello listáját. Vegye figyelembe, hogy csak egyéni tulajdonságok jelennek meg a kimeneti eredmények, és nem az alapértelmezett tulajdonságokat hello:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Ha úgy állítja be az egyéni tulajdonságokat meg kell adnia minden egyéni tulajdonságok minden alkalommal, amikor; Ellenkező esetben a hello beállítás toodisabled visszaállítja.   

### <a name="common-examples"></a>Gyakori példák
A következő parancsmag tooenable átirányítás hello használata:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

Ez a parancsmag tooenable használja USB és meghajtó átirányítás:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Használja a parancsmag toodisable vágólap megosztása:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> Lehet, hogy toocompletely kijelentkezés hello gyűjteményben lévő összes felhasználók (és nem csak válassza le őket) hello módosítás tesztelése előtt. tooensure felhasználók teljesen kilépéséig, nyissa meg toohello **munkamenetek** hello gyűjtemény hello Azure-portálon a lapra, és jelentkezzen ki minden leválasztása és bejelentkezett felhasználóknak. Egyes esetekben is igénybe vehet pár másodpercig hello helyi meghajtók tooshow Explorer hello-munkameneten belül.
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>A Windows-ügyfeleken USB-átirányítása beállításainak módosítása
Ha azt szeretné, hogy a számítógépen, amely a tooRemoteApp toouse USB-átirányítása, nincsenek 2 toohappen szükséges műveleteket. 1 – a rendszergazdának, hello gyűjtemény szintjén tooenable USB-átirányítása Azure PowerShell használatával. 2 - az egyes eszközökön, ha azt szeretné, hogy toouse USB-átirányítása tooenable olyan csoportházirenddel, amely lehetővé teszi, hogy szüksége. Ezzel a lépéssel kell toobe minden olyan felhasználóhoz, szeretne toouse USB-átirányítása történik.

> [!NOTE]
> Az Azure RemoteApp USB-átirányítása csak a Windows rendszerű számítógépeken támogatott.
> 
> 

### <a name="enable-usb-redirection-for-hello-remoteapp-collection"></a>A RemoteApp-gyűjtemény hello USB-átirányítása engedélyezése
A következő parancsmag tooenable USB-átirányítása hello gyűjtemény szintjén hello használata:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-hello-client-computer"></a>USB-átirányítása hello ügyfélszámítógép engedélyezése
tooconfigure USB Mappaátirányítási beállítások a számítógépen:

1. Nyissa meg a Helyicsoportházirend-szerkesztő (GPEDIT hello. MSC). (Gpedit.msc futtassa a parancssorba.)
2. Nyissa meg **számítógép Configuration\Policies\Administrative Templates\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali kapcsolat Client\RemoteFX USB eszközátirányítás**.
3. Kattintson duplán a **RDP engedélyezése átirányítása egyéb támogatott RemoteFX USB-eszközök erről a számítógépről**.
4. Válassza ki **engedélyezve**, majd válassza ki **hello RemoteFX USB-átirányítása hozzáférési jogok lévő Rendszergazdák és felhasználók**.
5. Nyisson meg egy parancssort rendszergazdai engedélyekkel, és futtassa a következő parancs hello:
   
        gpupdate /force
6. Indítsa újra a hello számítógépet.

Is használhat hello Csoportházirend kezelése eszköz toocreate és hello USB átirányítási házirend az összes számítógép alkalmazni a tartományban:

1. Jelentkezzen be a tartományvezérlő hello hello tartományi rendszergazdaként.
2. Nyissa meg a Csoportházirend kezelése konzol hello. (Kattintson **Start > Felügyeleti eszközök > csoportházirend-kezelés**.)
3. Keresse meg a toohello tartomány vagy szervezeti egység legyen toocreate hello házirend.
4. Kattintson a jobb gombbal **alapértelmezett tartományházirend**, és kattintson a **szerkesztése**.
5. Nyissa meg **számítógép Configuration\Policies\Administrative Templates\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali kapcsolat Client\RemoteFX USB eszközátirányítás**.
6. Kattintson duplán a **RDP engedélyezése átirányítása egyéb támogatott RemoteFX USB-eszközök erről a számítógépről**.
7. Válassza ki **engedélyezve**, majd válassza ki **hello RemoteFX USB-átirányítása hozzáférési jogok lévő Rendszergazdák és felhasználók**.
8. Kattintson az **OK** gombra.  

