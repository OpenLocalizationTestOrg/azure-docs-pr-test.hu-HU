---
title: "az Azure MFA használata a hálózati házirend-kiszolgáló bővítmény aaaVPN integrációs |} Microsoft Docs"
description: "A cikk ismerteti a VPN-infrastruktúra integrálása az Azure MFA hello hálózati házirend-kiszolgáló (NPS) bővítmény a Microsoft Azure használatával."
services: active-directory
keywords: "Az Azure MFA integrálja a VPN-, Azure Active Directoryban, hálózati házirend-kiszolgáló bővítmény"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 5e120f7633121385d9cc5d7bec97ecaa1ec7cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-hello-network-policy-server-nps-extension-for-azure"></a>A VPN-infrastruktúra integrálása az Azure multi-factor Authentication (MFA) Azure-beli hello hálózati házirend-kiszolgáló (NPS) bővítményének használatával

## <a name="overview"></a>Áttekintés

az Azure-hálózat szolgáltatás házirend-bővítmény hello segítségével a szervezetek toosafeguard távoli Authentication Dial-In User Service (RADIUS) ügyfél-hitelesítéshez felhőalapú [Azure multi-factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), amely biztosítja a kétlépéses ellenőrzést.

Ez a cikk ismerteti hello NPS infrastruktúra integrálása az Azure MFA hello hálózati házirend-kiszolgáló bővítményében Azure tooenable biztonságos kétlépéses ellenőrzéshez a felhasználóknak tooconnect tooyour hálózathoz egy VPN-kapcsolattal. 

hello hálózati házirend- és hozzáférés-szolgáltatások (NPS) által biztosított szervezetek hello képességek a következő:

* Adja meg a központi helyét hello felügyeleti és vezérelhető a hálózati kérelmek toospecify csatlakozó, mely napszakokban kapcsolatainak engedélyezését, kapcsolatok hello időtartamát és hello szintű biztonságot, hogy az ügyfeleket tooconnect használja, és így tovább. Helyett adja meg, ezek a szabályzatok minden VPN vagy a távoli asztal (RD) átjáró kiszolgálón, ezek a házirendek egyszer adható meg egy központi helyen. hello RADIUS protokollt használja tooprovide hello központosított hitelesítési, engedélyezési és nyilvántartási (AAA). 
* Állítson be és Hálózatvédelem (NAP) ügyfél állapotházirendeket, amelyek meghatározzák, hogy eszközök kapnak korlátozás nélküli vagy korlátozott hozzáférés toonetwork erőforrások kényszerítéséhez.
* Adja meg azt jelenti, hogy tooenforce hitelesítési és engedélyezési hozzáférési too802.1x-kompatibilis vezeték nélküli hozzáférési pontok és Ethernet-kapcsolók.    

További információkért lásd: [hálózati házirend-kiszolgáló (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

tooenhance biztonsági és magas szintű való megfelelőség, a szervezetek integrálhatók a hálózati házirend-kiszolgáló a felhasználók a kétlépéses ellenőrzést használó Azure MFA tooensure toobe képes kapcsolódhat toohello virtuális port hello VPN-kiszolgálón. A felhasználók toobe hozzáférést meg kell adniuk a felhasználónév/jelszó kombináció hello felhasználói adatokkal rendelkezik-e a vezérlő. Ezeket az információkat megbízható kell, és a rendszer egyszerűen nem lettek duplikálva, például a mobiltelefonszám, a vezetékes számát, a kérelem egy mobileszközön, és így tovább.

Előzetes toohello rendelkezésre állását hello hálózati házirend-kiszolgáló, az Azure-bővítmény, az ügyfelek, akik szükséges a kétlépéses ellenőrzést tooimplement integrált hálózati házirend-kiszolgáló, és az Azure MFA-környezetek tooconfigure kellett, és hello a helyszíni környezetben, külön MFA kiszolgáló karbantartása Távoli asztali átjáró és az Azure multi-factor Authentication kiszolgáló RADIUS használata ismertetését.

az Azure-hello hálózati házirend-kiszolgáló bővítmény hello rendelkezésre állását lehetőséget nyújt a szervezetek hello választott toodeploy vagy egy helyszíni MFA-megoldását, vagy felhőalapú MFA megoldás toosecure RADIUS ügyfél-hitelesítés.
 
## <a name="authentication-flow"></a>Hitelesítési folyamat
Amikor egy felhasználó kapcsolódott a virtuális port tooa VPN-kiszolgálón, először hitelesítenie kell protokollok, amelyek lehetővé teszik a felhasználónév és jelszó és a tanúsítványalapú hitelesítési módszerek használatát hello számos használatát. 

Továbbá tooauthenticating és identitás ellenőrzése, a felhasználónak rendelkeznie kell a megfelelő engedélyek betárcsázási hello. Az olyan egyszerű megvalósításokhoz, ezek betárcsázási engedélyek, amelyek lehetővé teszik a hozzáférést legyenek beállítva közvetlenül hello Active Directory-felhasználói objektumok. 

 ![Felhasználói tulajdonságok](./media/nps-extension-vpn/image1.png)

Olyan egyszerű megvalósításokhoz a VPN-kiszolgáló engedélyezi vagy megtagadja a hozzáférést minden helyi VPN-kiszolgálón meghatározott házirendek alapján.

Nagyobb és több méretezhető implementációkban hello házirendeket, hogy engedélyezze vagy tagadhatja meg VPN-hozzáférésre van központi RADIUS-kiszolgálókon. Ebben az esetben hello VPN-kiszolgáló egy kiszolgáló (RADIUS-ügyfél), amely kapcsolatkérelmeket és a fiók üzenetek tooa RADIUS-kiszolgáló működik. tooconnect toohello virtuális port hello VPN-kiszolgálón, a felhasználók kell hitelesíteni, és a RADIUS-kiszolgálók központilag meghatározott hello feltételeknek. 

Ha az Azure NPS-bővítmény hello hello hálózati házirend-kiszolgáló integrálva van, hello sikeres hitelesítési folyamat következőképpen történik:

1. hello VPN-kiszolgáló hitelesítési kérelmet kap, amely tartalmazza az hello felhasználónév és jelszó tooconnect tooa erőforrás, például a távoli asztali munkamenetet VPN felhasználó. 
2. Egy RADIUS-ügyfél működött, VPN-kiszolgáló RADIUS-kérést tooa hello kérelemüzenet alakítja át, és küld hello üzenet (jelszó titkosított) toohello RADIUS (NPS) kiszolgáló, amelyen telepítve van a hálózati házirend-kiszolgáló bővítmény hello. 
3. hello felhasználónév és jelszó kombináció ellenőrzése az Active Directoryban. Ha hello felhasználónév / jelszó nem megfelelő, hello RADIUS-kiszolgáló hozzáférés-utasítsa el az üzenetet küld. 
4. Ha minden feltételek szerint megadva, a hálózati házirend-kiszolgáló kapcsolódási kérelem hello és -e a hálózati házirendek (például időpont vagy csoport tagsági korlátozások), hálózati házirend-kiszolgáló bővítmény hello elindítja az Azure MFA másodlagos hitelesítési kérelmet. 
5. Az Azure MFA kommunikál az Azure Active Directory hello felhasználó adatait kéri le és hello (SMS-üzenet, mobilalkalmazás és így tovább) hello felhasználó által beállított hello metódussal másodlagos hitelesítést hajt végre. 
6. Hello MFA-kérdést sikeres, akkor az Azure MFA hello eredmény toohello hálózati házirend-kiszolgáló bővítmény kommunikál.
7. Miután hello kapcsolódási kísérlet is hitelesítése és engedélyezése, a hello hálózati házirend-kiszolgáló, amelyen telepítve van-e az hello bővítmény elküldi a RADIUS Access-Accept üzenet toohello VPN-kiszolgáló (RADIUS-ügyfél).
8. hello felhasználó hozzáférési toohello virtuális port a VPN-kiszolgáló kap, és egy titkosított VPN-alagutat hoz létre.

## <a name="prerequisites"></a>Előfeltételek
Ez a szakasz részletesen hello Előfeltételek előtt az Azure MFA integrálása hello távoli asztali átjáró szükséges. Mielőtt elkezdené, a következő előfeltételek teljesülése hello kell rendelkeznie.

* VPN-infrastruktúra
* Hálózati házirend- és hozzáférés-szolgáltatások (NPS) szerepkör
* Az Azure MFA-licenc
* Windows Server szoftver
* Szalagtárak
* Az Azure AD szinkronizálni a helyszíni AD 
* Az Azure Active Directory GUID azonosítója

### <a name="vpn-infrastructure"></a>VPN-infrastruktúra
Ez a cikk feltételezi, hogy a Microsoft Windows Server 2016 használatával helyen működő VPN-infrastruktúrával rendelkezik, és hello VPN-kiszolgálóhoz jelenleg nem konfigurált tooforward csatlakozási kérelmek tooa RADIUS-kiszolgáló. A jelen útmutató konfigurál majd hello VPN infrastruktúra toouse központi RADIUS-kiszolgálóra.

Ha nem rendelkeznek olyan működő infrastruktúra helyen, gyorsan létrehozhatja az infrastruktúra számos VPN telepítő oktatóanyagok található hello Microsoft és a külső helyek következő hello útmutató. 

### <a name="network-policy-and-access-services-nps-role"></a>Hálózati házirend- és hozzáférés-szolgáltatások (NPS) szerepkör

hálózati házirend-kiszolgáló szerepkör-szolgáltatás hello hello RADIUS-kiszolgáló és ügyfél biztosít. Ez a cikk feltételezi, hogy telepítette hello NPS szerepkör a tagkiszolgáló vagy tartományvezérlő a környezetben. A jelen útmutató egy VPN-konfiguráció RADIUS konfigurálja. Hello hálózati házirend-kiszolgáló szerepkör telepítése a kiszolgálóra _más_ mint a VPN-kiszolgáló.

Hello hálózati házirend-kiszolgáló szerepkör telepítésével kapcsolatos információt szolgáltatás Windows Server 2012 vagy újabb rendszerre, lásd: [a NAP állapotházirend-kiszolgáló telepítése](https://technet.microsoft.com/library/dd296890.aspx). Hálózati házirend (NAP) elavult a Windows Server 2016. Ajánlott eljárások a hálózati házirend-kiszolgáló, többek között a következőket hello ajánlás tooinstall hálózati házirend-kiszolgáló egy tartományvezérlőn leírását lásd: [ajánlott eljárások a hálózati házirend-kiszolgáló](https://technet.microsoft.com/library/cc771746).

### <a name="licenses"></a>Licencek

A licenc szükség van az Azure MFA számára, amely elérhető az Azure AD Premium, nagyvállalati mobilitási és biztonsági (EMS) vagy az MFA szolgáltatásra. További információkért lásd: [hogyan tooget Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md). Tesztelési célokra használható a próba-előfizetést.

### <a name="software"></a>Szoftver

hálózati házirend-kiszolgáló bővítmény hello van szükség a Windows Server 2008 R2 SP1 vagy újabb a hello NPS szerepkör-szolgáltatás telepítve. Ez az útmutató hello lépéseket a Windows Server 2016 végeztek.

### <a name="libraries"></a>Szalagtárak

a következő két szalagtárak hello szükség:

* [Visual C++ újraterjeszthető csomag a Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
* _Microsoft Active Directory modul Windows Powershellhez készült Azure verzió 1.1.166.0_ vagy újabb verzióját. Hello legújabb kiadására és telepítési utasításokat lásd: [Microsoft Azure Active Directory PowerShell modul kiadási korábbi verzióinak](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx).

Ezek a könyvtárak nem hello hálózati házirend-kiszolgáló bővítmény telepítőfájlokkal (verzió: 0.9.1.2), annak ellenére, hogy a meglévő dokumentáció, amely mást vannak csomagolva. Legalább a Visual Studio 2013 hello Visual C++ újraterjeszthető csomag kell telepítenie. Microsoft Active Directory modul Windows Powershellhez készült Azure hello telepítve van, ha még nincs jelen, keresztül egy konfigurációs parancsfájl hello beállítási folyamatának részeként futtatja. Nincs nincs szükség tooinstall Ez a modul időben Ha még nincs telepítve.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>A helyszíni Active Directoryval szinkronizálva az Azure Active Directory 

toouse hello hálózati házirend-kiszolgáló kiterjesztése a helyszíni felhasználók kell az Azure Active Directoryval szinkronizált és a többtényezős hitelesítés engedélyezve van. Ez az útmutató feltételezi, hogy a helyszíni felhasználók az Azure AD Connect használatával Active Directoryval van szinkronizálva. Így a felhasználók a multi-factor Authentication tartozó utasításokat alatt.
Információk az Azure AD connect című [integrálása a helyszíni címtárakat az Azure Active Directoryval](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Az Azure Active Directory GUID azonosítója 
tooinstall hello hálózati házirend-kiszolgáló esetén kell, hogy tooknow hello hello Azure Active Directory GUID Azonosítóját. A hello GUID-hello Azure Active Directory-kereséshez utasításokat hello a következő szakaszban.

## <a name="configure-radius-for-vpn-connections"></a>A VPN-kapcsolatok RADIUS konfigurálása

Ha telepítette a hello hálózati házirend-kiszolgáló kiszolgálói szerepkör azon a tagkiszolgálón, tooconfigure tooauthenticate kell, és VPN-ügyfél, amely a VPN-kapcsolatok hitelesítéséhez. 

Ez a szakasz azt feltételezi, hogy telepített hello hálózati házirend-kiszolgáló szerepkört, de nem konfigurált, használja a infrastruktúrában.

>[!NOTE]
>Ha már van egy működő VPN-kiszolgáló egy központi RADIUS-kiszolgálót használ, ez a szakasz kihagyhatja.
>

### <a name="register-server-in-active-directory"></a>Kiszolgáló regisztrálása az Active Directoryban
toofunction megfelelően az ebben a forgatókönyvben, meg kell regisztrálni az Active Directory toobe hello hálózati házirend-kiszolgáló.

1. Nyissa meg a Kiszolgálókezelőt.
2. A Kiszolgálókezelőben kattintson **eszközök**, és kattintson a **hálózati házirend-kiszolgáló**. 
3. Hello hálózati házirend-kiszolgáló konzol, kattintson a jobb gombbal **hálózati házirend-kiszolgáló (helyi)**, és kattintson a **kiszolgáló regisztrálása az Active Directoryban**. Kattintson a **OK** kétszer.

 ![Hálózati házirend-kiszolgáló](./media/nps-extension-vpn/image2.png)

4. Hagyja nyitva a következő eljárással hello hello konzol.

### <a name="use-wizard-tooconfigure-radius-server"></a>Használja a varázsló tooconfigure RADIUS-kiszolgáló
Használhatja a szabványos (varázsló-alapú) vagy speciális konfigurációs beállítás tooconfigure hello RADIUS-kiszolgáló. Ez a szakasz azt feltételezi, hogy a hello hello szabványos konfigurációs varázsló alapú beállítás használatát.

1. A hálózati házirend-kiszolgáló konzolján hello, kattintson az **hálózati házirend-kiszolgáló (helyi)**.
2. Standard beállítás csoportban **RADIUS-kiszolgáló telefonos vagy VPN-kapcsolatok**, és kattintson a **konfigurálása VPN vagy a telefonos**.

 ![Konfigurálja a VPN](./media/nps-extension-vpn/image3.png)

3. Hello válassza telefonos vagy virtuális magánhálózati kapcsolatok hálózattípus lapon jelölje be **virtuális magánhálózati kapcsolatok**, és kattintson a **következő**.

 ![Virtuális magánhálózat](./media/nps-extension-vpn/image4.png)

4. Hello meg telefonos vagy VPN-kiszolgáló lapon kattintson **Hozzáadás**.
5. A hello **új RADIUS-ügyfél** párbeszédpanelen adjon meg egy rövid nevet, írja be a hello feloldható nevét vagy IP-cím hello VPN-kiszolgáló, és adjon meg egy megosztott titkos jelszót. Győződjön meg a közös titkos jelszó hosszú és összetett. Jegyezze fel ezt a jelszót, csak a hello a következő szakaszban található lépéseket.

 ![Új RADIUS-ügyfél](./media/nps-extension-vpn/image5.png)

6. Kattintson a **OK**, majd **következő**.
7. A hello **hitelesítési módszerek konfigurálása** lap, fogadja el az alapértelmezésként beállított elemet hello (Microsoft titkosított hitelesítés 2 (MS-CHAPv2) vagy más lehetőséget választ, és kattintson a **következő**.

  >[!NOTE]
  >Ha konfigurálja az Extensible Authentication Protocol (EAP), MS-CHAPv2 vagy PEAP kell használnia. Nincs más EAP esetén támogatott.
 
8. Hello adja meg felhasználói csoportok lapon kattintson a **Hozzáadás** , majd válasszon egy megfelelő, ha van ilyen. Ellenkező esetben hagyja meg hello üres toogrant hozzáférés tooall felhasználók.

 ![Adja meg a felhasználói csoportok](./media/nps-extension-vpn/image7.png)

9. Kattintson a **Tovább** gombra.
10. Hello IP-szűrők megadása lapon kattintson **következő**.
11. Hello adja meg a titkosítási beállítások lapon fogadja el a hello alapértelmezett beállításokat, és kattintson a **következő**.

 ![Adja meg az Encryption](./media/nps-extension-vpn/image8.png)

12. Hello adja meg a tartománynév, a hello neve üresen hagyja, elfogadja hello alapértelmezett beállítást, és kattintson a **következő**.

 ![Tartománynév megadása](./media/nps-extension-vpn/image9.png)

13. Hello új befejezése telefonos vagy virtuális magánhálózati kapcsolatok és a RADIUS-ügyfelek lapot, kattintson a **Befejezés**.

 ![Fejezze be a kapcsolatok száma](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>A RADIUS konfigurálásának ellenőrzése
Ez a szakasz részletesen hello konfigurációs hello varázslóval létrehozott.

1. Hello hálózati házirend-kiszolgálón, hello hálózati házirend-kiszolgáló (helyi) konzolon bontsa ki a RADIUS-ügyfelek, és válassza **RADIUS-ügyfelek**.
2. Hello részleteket tartalmazó ablaktáblában kattintson a jobb gombbal hello RADIUS-ügyfél varázslóval létrehozott, és kattintson a **tulajdonságok**. a RADIUS-ügyfél (hello VPN-kiszolgáló) hello tulajdonságait kell hasonló toothose alább látható.

 ![VPN-tulajdonságai](./media/nps-extension-vpn/image11.png)

3. Kattintson a **Mégse**.
4. Hello hálózati házirend-kiszolgálón, hello hálózati házirend-kiszolgáló (helyi) konzolon bontsa ki a **házirendek**, és válassza ki **kapcsolatkérelem-házirendek**. Az alábbi képen hello levő hello VPN-kapcsolatok házirend kell megjelennie.

 ![Csatlakozási kérések](./media/nps-extension-vpn/image12.png)

5. Válassza ki a házirend, **hálózati házirendek**. Az alábbi képen hello levő virtuális magánhálózati (VPN) kapcsolatok házirend akkor.

 ![Hálózat tulajdonságai](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-toouse-radius-authentication"></a>VPN-kiszolgáló toouse RADIUS-hitelesítés konfigurálása
Ebben a szakaszban hello VPN server toouse RADIUS-hitelesítés konfigurálása. Ez a szakasz azt feltételezi, hogy VPN-kiszolgáló működő konfigurációval rendelkezik, de nem konfigurált hello VPN server toouse RADIUS-hitelesítés. Miután hello VPN-kiszolgáló, akkor győződjön meg arról, hogy a várt módon működik-e a konfigurációs.

>[!NOTE]
>Ha már van egy működő VPN-kiszolgáló beállítása, amely a RADIUS-hitelesítést használ, ez a szakasz kihagyhatja.
>

### <a name="configure-authentication-provider"></a>Hitelesítésszolgáltató konfigurálása
1. Hello VPN-kiszolgálón nyissa meg a Kiszolgálókezelőt.
2. A Kiszolgálókezelőben kattintson **eszközök**, majd **Útválasztás és távelérés**.
3. Hello Útválasztás és távelérés konzolon kattintson a jobb gombbal  **\[kiszolgálónév\] (helyi)**, és kattintson a **tulajdonságok**.

 ![Útválasztás és távelérés](./media/nps-extension-vpn/image14.png)
 
4. A hello **[kiszolgálónév} (helyi) tulajdonságok** párbeszédpanelen kattintson hello **biztonsági** lapon. 
5. A hello **biztonsági** lapon a hitelesítésszolgáltató, kattintson a **RADIUS-hitelesítés**, majd **konfigurálása**.

 ![RADIUS-hitelesítés](./media/nps-extension-vpn/image15.png)
 
6. A RADIUS-hitelesítés hello párbeszédpanel, kattintson **Hozzáadás**.
7. A hello RADIUS-kiszolgáló hozzáadása a kiszolgálónév, hello nevét vagy hello IP-cím az előző szakaszban hello konfigurált hello RADIUS-kiszolgáló hozzáadása.
8. Kattintson a közös titok **módosítása** , és adja hozzá a hello megosztott titkos jelszót létrehozni, és korábban rögzített.
9. Időtúllépés (másodpercben), módosítsa hello tooa érték közötti **30** és **60**. Ez az szükséges tooallow elegendő idő toocomplete hello második hitelesítési tényezőt.
 
 ![RADIUS-kiszolgáló hozzáadása](./media/nps-extension-vpn/image16.png)
 
10. Kattintson a **OK** összes párbeszédpanel bezárásával végrehajtásáig.

### <a name="test-vpn-connectivity"></a>VPN-kapcsolat tesztelése
Ebben a szakaszban, győződjön meg arról, hogy hello VPN-ügyfél hitelesítése és engedélyezése hello RADIUS-kiszolgáló által tooconnect tooVPN virtuális port tett kísérlet során. Ez a szakasz feltételezi, hogy a Windows 10 és a VPN-ügyfélként. 

>[!NOTE]
>Ha már konfigurált egy VPN-ügyfél tooconnect toohello VPN-kiszolgálót, és mentette hello-beállítások, kihagyhatja a hello lépéseket kapcsolódó tooconfiguring és mentése egy VPN-kapcsolat objektumot.
>

1. A VPN-ügyfél számítógépen kattintson **Start**, majd **beállítások** (fogaskerék ikonra).
2. Kattintson az ablak-beállítások **hálózat és Internet**.
3. Kattintson a **VPN**.
4. Kattintson a **egy VPN-kapcsolat hozzáadása**.
5. A VPN-kapcsolat, adjon meg Windows (beépített), a VPN-szolgáltató, akkor a mezőket, szükség esetén hátralévő teljes hello hello, és kattintson a **mentése**. 

 ![VPN-kapcsolat hozzáadása](./media/nps-extension-vpn/image17.png)
 
6. Nyissa meg hello **hálózati és megosztási központ** a Vezérlőpulton.
7. Kattintson a **Adapterbeállítások módosítása**.

 ![Adapterbeállítások módosítása](./media/nps-extension-vpn/image18.png)

8. Kattintson a jobb gombbal a hello VPN-hálózati kapcsolat, és kattintson a Tulajdonságok elemre. 

 ![VPN-hálózati tulajdonságok](./media/nps-extension-vpn/image19.png)

9. Hello VPN tulajdonságai párbeszédpanel, kattintson a hello **biztonsági** fülre. 
10. Hello biztonság lapon győződjön meg arról, hogy csak **Microsoft CHAP 2-es Version (MS-CHAP v2)** van kiválasztva, és kattintson az OK gombra.

 ![Protokollok engedélyezése](./media/nps-extension-vpn/image20.png)

11. Kattintson a jobb gombbal a hello VPN-kapcsolatot, és kattintson a **Connect**.
12. Hello beállítások lapján kattintson a **Connect**.

A sikeres kapcsolat megjelenik hello biztonsági naplóba hello RADIUS-kiszolgáló Event ID 6272, mint a lent látható módon.

 ![Esemény tulajdonságai](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Az útmutató hibaelhárítása
Tegyük fel, hogy a VPN-konfiguráció dolgozott előtt konfigurálta hello VPN-kiszolgáló toouse egy központi RADIUS-kiszolgálót a hitelesítéshez és engedélyezéshez. Ebben az esetben valószínű, hogy hello probléma okozhatja hello RADIUS-kiszolgáló vagy hello használata érvénytelen felhasználónév vagy jelszó helytelen beállítása. Például, ha hello felhasználónév hello alternatív UPN-utótagot használja, hello bejelentkezési kísérlet sikertelen lehet (használjon a legjobb eredmények elérése érdekében azonos fióknév hello). 

Ezeket a problémákat, egy ideális hely toostart megtalálható tooexamine hello and Security event logs tootroubleshoot hello RADIUS-kiszolgáló. toosave idő keresése események, is használhatja hello szerepkör-alapú hálózati házirend- és kiszolgáló egyéni megtekintése az eseménynaplóban, az alábbi megjelenítése. A 6273-as Azonosítójú esemény azt jelzi, hogy hol hello hálózati házirend-kiszolgáló megtagadta a hozzáférést tooa felhasználói események. 

 ![Eseménynapló](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Többtényezős hitelesítés beállítása
A szakasz ismerteti, így a felhasználók a multi-factor Authentication és a kétlépéses ellenőrzéshez fiókok beállításával kapcsolatos utasításokat. 

### <a name="enable-multi-factor-authentication"></a>A többtényezős hitelesítés engedélyezése
Ebben a szakaszban az Azure AD-fiókok a multi-factor Authentication engedélyezése. Használjon hello **klasszikus portál** tooenable felhasználók a multi-factor Authentication. 

1. Nyisson meg egy böngészőt, és keresse meg a túl[https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Hello rendszergazdaként jelentkezzen be.
3. Hello portálon hello bal oldali navigációs, kattintson **ACTIVE DIRECTORY**.

 ![Alapértelmezett könyvtár](./media/nps-extension-vpn/image23.png)

4. A hello neve oszlopban kattintson **alapértelmezett címtárat** (vagy egy másik címtárban, ha szükséges).
5. Hello gyors kezdés lapon kattintson **konfigurálása**.

 ![Alapértelmezett konfigurálása](./media/nps-extension-vpn/image24.png)

6. Hello KONFIGURÁLÁSA lapon görgessen lefelé, és hello többtényezős hitelesítés területen kattintson **szolgáltatás beállításainak kezelése**.

 ![Többtényezős hitelesítés beállításainak kezelése](./media/nps-extension-vpn/image25.png)
 
7. Hello többtényezős hitelesítés oldalon tekintse át hello szolgáltatás alapértelmezett beállításait, majd **felhasználók**. 

 ![MFA Users (MFA-felhasználók)](./media/nps-extension-vpn/image26.png)
 
8. Hello felhasználók lapon válassza ki a hello felhasználók tooenable kívánja az MFA szolgáltatásra, és kattintson **engedélyezése**.

 ![Tulajdonságok](./media/nps-extension-vpn/image27.png)
 
9. Amikor a rendszer kéri, kattintson a **multi-factor auth engedélyezése**.

 ![Többtényezős hitelesítés engedélyezése](./media/nps-extension-vpn/image28.png)
 
10. Kattintson a **Bezárás** gombra. 
11. Frissítse a hello lapot. hello többtényezős hitelesítés állapota megváltozott tooEnabled.

Információ tooenable felhasználók a multi-factor Authentication [Ismerkedés az Azure multi-factor Authentication hello felhőben](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>A kétlépéses ellenőrzéshez fiókok beállítása
Ha a fiók engedélyezve van az MFA szolgáltatásra, felhasználók nem képes toosign hello többtényezős hitelesítési szabályzat szabályozzák, amíg sikeresen konfigurálta a hello második hitelesítési tényező, hogy kétlépéses ellenőrzés egy megbízható eszköz toouse tooresources a.

Ebben a szakaszban egy megbízható eszköz konfigurál a kétlépéses ellenőrzéshez használttal. Többféle módon, tooconfigure elérhető ezek hello következőket beleértve:

* **Mobilalkalmazás**. A Windows Phone, Android vagy iOS eszközön hello Microsoft Authenticator alkalmazás telepítése. Attól függően, hogy a szervezet házirendjeit, amelyek szükséges toouse hello alkalmazás két mód egyikében: kapnak értesítést az ellenőrzések (értesítés fejlesztőre tooyour eszköz) vagy ellenőrző kód használata (a következőket kell végrehajtania tooenter egy ellenőrző kód 30 másodpercenként frissíti). 
* **Mobiltelefon hívást vagy SMS**. Az automatizált telefonhívást vagy SMS-üzenet vagy fogadni. Hello telefonhívás opcióval hello hívás választ, majd nyomja meg a hello # bejelentkezési tooauthenticate. Hello szöveg opcióval toohello szöveges üzenet válasz vagy hello ellenőrzőkódot hello bejelentkezési felületen meg kell adnia.
* **Irodai telefon hívása**. Ez a folyamat ugyanaz, mint az automatikus telefonhívásokat fent leírt van hello.

A következő lépések követésével egy eszköz toouse hello tooreceive leküldéses értesítést a mobilalkalmazásban ellenőrzés beállításához.

1. Jelentkezzen be túl[https://aka.ms/mfasetup](https://aka.ms/mfasetup) vagy egyetlen helyen, például a [https://portal.azure.com](https://portal.azure.com), amely szükséges a tooauthenticate MFA-kompatibilis hitelesítő adataival. 
2. Bejelentkezés a felhasználónevét és jelszavát, akkor lehetősége lesz egy további biztonsági ellenőrzési hello fiókot tooset kérő képernyőn.

 ![További biztonsági](./media/nps-extension-vpn/image29.png)

3. Kattintson a **most beállítása**.
4. Hello további biztonsági ellenőrzési lapot válassza a kapcsolattartó típusa (hitelesítéshez megadott telefonját, irodai telefon vagy mobilalkalmazás). Válasszon olyan országban vagy régióban, majd válasszon ki egy módszert. hello metódus kapcsolattartási típustól függ. Például, ha a mobilalkalmazás lehetőséget választja, kiválaszthatja e tooreceive értesítések ellenőrzése vagy toouse ellenőrző kódot. hello következő lépések azt feltételezik, hogy úgy dönt, **mobilalkalmazás** hello, lépjen kapcsolatba a típusát.

 ![Telefonos hitelesítési](./media/nps-extension-vpn/image30.png)

5. Jelölje ki azt a mobilalkalmazás **értesítéseket az ellenőrzéshez**, majd **beállítása**. 

 ![Mobilalkalmazás ellenőrzése](./media/nps-extension-vpn/image31.png)
 
6. Ha még nem tette meg, a saját eszközére telepített hello authenticator mobilalkalmazás. 
7. Hello mobilalkalmazás tooscan hello jelenik meg a vonalkód hello utasításokat követve vagy hello információk manuális megadása, és kattintson **végzett**.

 ![Mobilalkalmazás konfigurálása](./media/nps-extension-vpn/image32.png)

8. A hello további biztonsági ellenőrzési lapot, kattintson **forduljon me** és választ küldött toonotification tooyour eszköz.
9. Hello további biztonsági ellenőrzési lapon adja meg egy számot, ha elveszti a hozzáférést toohello mobilalkalmazás, és kattintson a **következő**.

 ![Mobiltelefonszám](./media/nps-extension-vpn/image33.png)
 
10. A további biztonsági ellenőrzés hello, kattintson **végzett**.

hello eszköz már konfigurált tooprovide a második ellenőrzési módszert. A kétlépéses ellenőrzéshez fiókok beállításával kapcsolatos információkért lásd: [a kétlépéses ellenőrzéshez a fiók beállítása](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Telepítse és konfigurálja a hálózati házirend-kiszolgáló bővítmény

Ez a szakasz ismerteti az ügyfél-hitelesítéshez az Azure MFA VPN toouse hello VPN-kiszolgáló konfigurálása.

Miután telepítése és konfigurálása a hálózati házirend-kiszolgáló bővítmény hello, ez a kiszolgáló által feldolgozott összes RADIUS-alapú ügyfél-hitelesítés szükséges toouse Azure MFA. Ha nem a VPN-felhasználók az Azure MFA-ban regisztrált, beállíthat egy másik RADIUS kiszolgáló tooauthenticate felhasználókat, akik nem konfigurált toouse MFA. Vagy egy beállításjegyzékbeli bejegyzést, amely lehetővé teszi, hogy a kifogásolt felhasználók tooprovide egy második hitelesítési tényezővel hozhat létre, csak ha léptetheti be többtényezős Hitelesítést. 

Hozzon létre egy új karakterláncértéket _a HKLM\SOFTWARE\Microsoft\AzureMfa REQUIRE_USER_MATCH_, és állítsa be a hello érték tooTRUE vagy HAMIS eredményt ad. 

 ![Felhasználóegyeztetés megkövetelése](./media/nps-extension-vpn/image34.png)
 
Ha hello érték beállítása tooTRUE vagy nincs megadva, minden hitelesítési kérelemre tulajdonos tooan MFA-kérdést. Ha hello értéke tooFALSE, MFA kihívást csak az MFA-ban regisztrált toousers adják ki. Csak a hello FALSE beállítással tesztelési vagy üzemi környezetben egy bevezetési időszakban használható.

### <a name="acquire-azure-active-directory-guid-id"></a>Szerezzen be az Azure Active Directory GUID azonosítója

Hello hálózati házirend-kiszolgáló bővítmény hello konfigurációjának részeként kell toosupply rendszergazdai hitelesítő adataival és hello Azure Active Directory-azonosítója az Azure AD-bérlő. az alábbi hello lépésekből megtudhatja, hogyan tooget hello bérlői azonosító.

1. Az Azure portálon, a bejelentkezés toohello [https://portal.azure.com](https://portal.azure.com) hello globális rendszergazdájaként hello Azure bérlői.
2. A bal oldali navigációs hello, kattintson a hello **Azure Active Directory** ikonra.
3. Kattintson a **Tulajdonságok** elemre.
4. toocopy a könyvtár-azonosítója toohello vágólap, jelölje be hello **másolási** ikonra.
 
 ![Könyvtár-azonosítója](./media/nps-extension-vpn/image35.png)

### <a name="install-hello-nps-extension"></a>Hello NPS-kiterjesztés telepítése
hálózati házirend-kiszolgáló bővítmény hello kell telepítve a kiszolgálón, amelyen a hálózati házirend hello toobe és Access Services (NPS) szerepkör telepítése és a funkciók kialakításában hello RADIUS-kiszolgálóként. Hello hálózati házirend-kiszolgáló bővítmény ne telepítse a távoli asztal-kiszolgálón.

1. Töltse le a hálózati házirend-kiszolgáló bővítmény hello [https://aka.ms/npsmfa](https://aka.ms/npsmfa). 
2. Másolás hello beállítása végrehajtható fájl (NpsExtnForAzureMfaInstaller.exe) toohello hálózati házirend-kiszolgáló.
3. Hello hálózati házirend-kiszolgáló, kattintson duplán a **NpsExtnForAzureMfaInstaller.exe**. Ha a rendszer kéri, kattintson a **futtatása**.
4. A hálózati házirend-kiszolgáló bővítmény hello Azure MFA párbeszédpanel, tekintse át a hello szoftverlicenc-feltételeket, ellenőrizze **elfogadom a licencfeltételeket toohello**, és kattintson a **telepítése**.

 ![Hálózati házirend-kiszolgáló bővítmény](./media/nps-extension-vpn/image36.png)
 
5. A hálózati házirend-kiszolgáló bővítmény hello Azure MFA párbeszédpanel, kattintson **Bezárás**.  

 ![A telepítő sikeres](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-hello-nps-extension-using-a-powershell-script"></a>PowerShell parancsfájl használatával hello hálózati házirend-kiszolgáló kiterjesztésű használt tanúsítványok konfigurálása
tooensure biztonságos kommunikációt, és biztosítani kell tooconfigure tanúsítványok használatra hello NPS-kiterjesztés által. hello hálózati házirend-kiszolgáló összetevői az alábbiak a Windows PowerShell-parancsfájlt, amely beállítja a hálózati házirend-kiszolgáló egy önaláírt tanúsítványt. 

hello parancsfájl hello a következő műveleteket hajtja végre:

* Létrehoz egy önaláírt tanúsítványt
* Társítja az Azure ad-val egyszerű tanúsítvány-tooservice nyilvános kulcs
* Tárolók hello cert hello helyi számítógép tárolójában
* Hozzáférési toohello tanúsítvány titkos kulcs toohello hálózati felhasználó
* Hálózati házirend-kiszolgáló szolgáltatás újraindítása

Ha azt szeretné, toouse saját tanúsítványok, a tanúsítvány toohello szolgáltatás elv tooassociate hello nyilvános Azure ad-val kell, és így tovább.
toouse hello parancsfájl, hello bővítmény biztosít az Azure Active Directory rendszergazdai hitelesítő adatokkal, és hello Azure Active Directory-bérlőazonosító beszerzése korábban kimásolt. Futtassa a parancsfájlt hello minden hálózati házirend-kiszolgálón, ahol hello NPS-bővítményének telepítése.

1. Nyisson meg egy felügyeleti Windows PowerShell-parancssort.
2. Hello PowerShell parancssorába írja be a _cd "c:\Program Files\Microsoft\AzureMfa\Config"_, és nyomja le az ENTER **ENTER**.
3. Típus _.\AzureMfsNpsExtnConfigSetup.ps1_, és nyomja le az ENTER **ENTER**. 
 * hello parancsfájl toosee ellenőrzi, hogy a hello Azure Active Directory PowerShell-modul telepítve van. Ha nincs telepítve, hello parancsfájl telepíti hello modul.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. Miután hello parancsfájl ellenőrzi a hello hello PowerShell-modult, hello Azure Active Directory PowerShell modul párbeszédpanel jelenik meg. Hello párbeszédpanelen adja meg a Azure AD rendszergazdai hitelesítő adatait, és a jelszót, és kattintson **bejelentkezés**. 
 
 ![PowerShell-bejelentkezés](./media/nps-extension-vpn/image39.png)
 
5. Amikor a rendszer kéri, illessze be a korábban kimásolt toohello vágólapra hello Bérlőazonosító, és nyomja le az ENTER **ENTER**. 

 ![Bérlőazonosító](./media/nps-extension-vpn/image40.png)

6. hello parancsfájl létrehoz egy önaláírt tanúsítványt, és más konfigurációs módosításokat hajt végre. hello eredménye például hello kép alább látható.

 ![Önaláírt tanúsítvány](./media/nps-extension-vpn/image41.png)

7. Hello kiszolgáló újraindul.
 
### <a name="verify-configuration"></a>Konfiguráció ellenőrzése
tooverify hello konfigurációban kell tooestablish egy új VPN-kapcsolatot a VPN-kiszolgáló. Belépéskor sikeresen megtörtént a hitelesítő adatait az elsődleges hitelesítéshez, VPN-kapcsolat hello hello másodlagos hitelesítés toosucceed előtt megvárja hello kapcsolat létrejött, alább látható módon. 

 ![Konfiguráció ellenőrzése](./media/nps-extension-vpn/image42.png)

Ha sikeresen hello másodlagos ellenőrzési módszer korábban már konfigurálta az Azure MFA hitelesítést, csatlakoztatott toohello erőforrás áll. Ha másodlagos hitelesítés hello nem sikeres, hozzáférési tooresource sem kap. 

Hello példában az alábbi hello hitelesítő alkalmazást Windows Phone-eszközön használt tooprovide hello másodlagos hitelesítés.

 ![Fiók ellenőrzése](./media/nps-extension-vpn/image43.png)

Miután sikeresen hitelesítette hello másodlagos metódussal, kapnak a hozzáférési toohello virtuális port hello VPN-kiszolgálón. Mivel volt szükséges toouse egy másodlagos hitelesítési módszer használatával, ha megbízható eszközt, hello bejelentkezési folyamat biztonságosabb, mint azt kellene használatával csak a felhasználónév / jelszó kombináció.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Eseménynaplók sikeres bejelentkezési események megtekintése
tooview hello sikeres bejelentkezési események hello Windows Eseménynapló-naplókban, kiadhatja hello a következő Windows PowerShell parancs tooquery hello Windows biztonsági naplóba hello hálózati házirend-kiszolgálón.

tooquery sikeres bejelentkezési események hello biztonsági eseménynapló, használja a következő parancsot, hello
* _Get-WinEvent - naplónév biztonsági_ |} ahol {$_.ID - eq "6272"} |} FL 

 ![Biztonsági eseménynapló](./media/nps-extension-vpn/image44.png)
 
Alább látható módon hello biztonsági naplóban vagy hello hálózati házirend- és elérési szolgáltatások egyéni nézet, is megtekintheti:

 ![Hálózati házirend-hozzáférés](./media/nps-extension-vpn/image45.png)

Hello hálózati házirend-kiszolgáló bővítmény telepítési az Azure MFA hello kiszolgálón található alkalmazás eseménynaplók, adott toohello bővítmény **alkalmazások és szolgáltatások Logs\Microsoft\AzureMfa**. 

* _Get-WinEvent - naplónév biztonsági_ |} ahol {$_.ID - eq "6272"} |} FL

 ![Események (event) száma](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Az útmutató hibaelhárítása
Hello konfigurációs nem a várt módon működik, ha egy remek toostart tootroubleshoot, amely a felhasználó hello tooverify konfigurált toouse Azure MFA. Csatlakozás túl hello felhasználó[https://portal.azure.com](https://portal.azure.com). Ha a rendszer kéri a másodlagos hitelesítés, és sikeresen be tud hitelesíti, megszüntetheti az Azure MFA helytelen konfigurációban.

Ha az Azure MFA hello felhasználó(k) szolgáltatás működik, tekintse át hello vonatkozó eseménynaplók. Ezek közé tartozik a hello biztonsági esemény, az átjáró működik és az Azure MFA naplók hello előző szakaszban tárgyalt. 

Alább egy példa kimenet megjeleníti a sikertelen bejelentkezési esemény (Event ID 6273) a biztonsági napló van:

 ![Biztonsági napló](./media/nps-extension-vpn/image47.png)

Alább az hello AzureMFA naplókból kapcsolódó esemény:

 ![Az Azure MFA-naplók](./media/nps-extension-vpn/image48.png)

Speciális tooperform beállítások, tájékoztatást hello hálózati házirend-kiszolgáló naplófájlok hello NPS szolgáltatást futtató hibaelhárítása. Ezekben a naplófájlokban létrejönnek _%SystemRoot%\System32\Logs_ vesszővel tagolt szövegfájlok mappában. Ezek leírását naplófájlok című [értelmezhetők hálózati házirend-kiszolgáló naplófájlok](https://technet.microsoft.com/library/cc771748.aspx). 

hello ezekben a naplófájlokban bejegyzései nehéz toointerpret nélkül importálja őket egy táblázatot vagy egy adatbázisban. Egy szám található IAS elemzők online tooassist a naplófájlok értelmezése a hello. Az alábbiakban egy a hello kimenet egy ilyen letölthető [shareware alkalmazás](http://www.deepsoftware.com/iasviewer): 

 ![Shareware alkalmazás](./media/nps-extension-vpn/image49.png)

Végezetül, a további beállítások elhárításával kapcsolatos tudnivalókat is használhat, például a Wireshark protokollelemző vagy [Microsoft Message Analyzert](https://technet.microsoft.com/library/jj649776.aspx). hello Wireshark az alábbi képen látható RADIUS köszönőüzenetei hello VPN-kiszolgáló és a hello hálózati házirend-kiszolgáló között.

 ![Microsoft Message Analyzert](./media/nps-extension-vpn/image50.png)

További információkért lásd: [a meglévő hálózati házirend-kiszolgáló infrastruktúra integrálása az Azure multi-factor Authentication](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Következő lépések
[Hogyan tooget Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md)

[Távoli asztali átjáró és RADIUS-t használó Azure Multi-Factor Authentication-kiszolgáló](multi-factor-authentication-get-started-server-rdg.md)

[A helyszíni címtárak integrálása az Azure Active Directoryval](../active-directory/connect/active-directory-aadconnect.md)

