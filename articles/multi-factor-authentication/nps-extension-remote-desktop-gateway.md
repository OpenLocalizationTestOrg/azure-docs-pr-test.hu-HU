---
title: "Azure MFA NPS bővítmény asztali átjáró integrációja aaaRemote |} Microsoft Docs"
description: "A cikk ismerteti a távoli asztali átjáró infrastruktúra integrálása az Azure MFA hello hálózati házirend-kiszolgáló (NPS) bővítmény a Microsoft Azure használatával."
services: active-directory
keywords: "Az Azure MFA integrálja a távoli asztali átjáró, Azure Active Directoryban, hálózati házirend-kiszolgáló bővítmény"
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
ms.openlocfilehash: ae5f6864416582bd82b787005ca787988d23a813
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="integrate-your-remote-desktop-gateway-infrastructure-using-hello-network-policy-server-nps-extension-and-azure-ad"></a>A távoli asztali átjáró infrastruktúra használatával hello hálózati házirend-kiszolgáló (NPS) bővítményt, és az Azure AD integrálása

Ez a cikk részletesen a távoli asztali átjáró infrastruktúra Azure multi-factor Authentication (MFA) integrálására hello hálózati házirend-kiszolgáló (NPS) bővítmény a Microsoft Azure használatával. 

hello az Azure-hálózat szolgáltatás házirend-bővítmény lehetővé teszi, hogy az ügyfelek toosafeguard távoli Authentication Dial-In User Service (RADIUS) ügyfél-hitelesítéshez Azure tartozó felhőalapú [multi-factor Authentication (MFA)](multi-factor-authentication.md). A megoldás biztosít a kétlépéses ellenőrzést, a második réteg biztonsági toouser bejelentkezéseket és tranzakciókat.

Ez a cikk részletesen hello NPS infrastruktúra integrálása az Azure MFA hello hálózati házirend-kiszolgáló bővítmény használatával az Azure-bA. Ez lehetővé teszi a felhasználóknak a távoli asztali átjáró tooa toolog biztonságos ellenőrzése. 

hello hálózati házirend- és hozzáférés-szolgáltatások (NPS) által biztosított szervezetek képességét toodo hello következő hello:
* Hello felügyeleti és a hálózati kérelmek vezérlő központi helyét adja meg a kapcsolódást megadásával, a kapcsolatok engedélyezve legyenek, mely napszakokban hello kapcsolatok időtartamát és hello szintű biztonságot, hogy az ügyfeleket tooconnect használja, és így tovább. Ahelyett, hogy ezek a házirendek megadása minden VPN vagy a távoli asztal (RD) átjáró kiszolgálón, ezek a házirendek egyszer adható meg egy központi helyen. hello RADIUS protokollt biztosít hello központosított hitelesítési, engedélyezési és nyilvántartási (AAA). 
* Állítson be és Hálózatvédelem (NAP) ügyfél állapotházirendeket, amelyek meghatározzák, hogy eszközök kapnak korlátozás nélküli vagy korlátozott hozzáférés toonetwork erőforrások kényszerítéséhez.
* Adja meg azt jelenti, hogy tooenforce hitelesítési és engedélyezési hozzáférési too802.1x-kompatibilis vezeték nélküli hozzáférési pontok és Ethernet-kapcsolók.    

Általában a szervezet használja a hálózati házirend-kiszolgáló (RADIUS) toosimplify, VPN hello kezelésének központosítása házirendeket. Számos szervezet azonban is használja a hálózati házirend-kiszolgáló toosimplify, és a távoli asztali asztali kapcsolatengedélyezési házirendek (RD CAP-ok) hello kezelésének központosítása. 

A szervezetek hálózati házirend-kiszolgáló integrálása az Azure MFA tooenhance biztonsági és megfelelőségi egy magas szintű is. Ezzel biztosíthatja, hogy a felhasználók a távoli asztali átjáró toohello kétlépéses ellenőrzés toolog létrehozásához. A felhasználók toobe hozzáférést meg kell adniuk a felhasználónév/jelszó kombináció hello felhasználói adatokkal rendelkezik-e a vezérlő. Ezeket az információkat megbízható kell, és a rendszer egyszerűen nem lettek duplikálva, például a mobiltelefonszám, a vezetékes számát, a kérelem egy mobileszközön, és így tovább.

Előzetes toohello rendelkezésre állását hello hálózati házirend-kiszolgáló, az Azure-bővítmény, az ügyfelek, akik szükséges a kétlépéses ellenőrzést tooimplement integrált hálózati házirend-kiszolgáló, és az Azure MFA-környezetek tooconfigure kellett, és hello a helyszíni környezetben, külön MFA kiszolgáló karbantartása részletes ismertetését lásd: [távoli asztali átjáró és az Azure multi-factor Authentication kiszolgáló RADIUS használata](multi-factor-authentication-get-started-server-rdg.md).

az Azure-hello hálózati házirend-kiszolgáló bővítmény hello rendelkezésre állását lehetőséget nyújt a szervezetek hello választott toodeploy vagy egy helyszíni MFA-megoldását, vagy felhőalapú MFA megoldás toosecure RADIUS ügyfél-hitelesítés.

## <a name="authentication-flow"></a>Hitelesítési folyamat

A felhasználók számára az toobe nyújtott toonetwork erőforrások eléréséhez távoli asztali átjárón keresztül egy távoli asztali kapcsolat engedélyezési házirendje (RD CAP) és egy távoli asztali erőforrás engedélyezési házirendje (RD RAP) megadott hello feltételeknek kell megfelelniük. RD CAP-OK adja meg, akik meghatalmazott tooconnect tooRD átjárók. RD RAP-k hello hálózati erőforrások, például a távoli asztali vagy a távoli alkalmazások esetén adja meg, hogy a felhasználó hello engedélyezett tooconnect toothrough hello távoli asztali átjáró. 

Egy távoli asztali átjáró lehet konfigurált toouse az RD CAP-OK központi házirendtárolóból. Feldolgozásuk a távoli asztali átjáró hello RD RAP-k egyik központi házirend, nem használhatja. Például egy távoli asztali átjáró konfigurálva toouse az RD CAP-OK központi házirendtárolót egy RADIUS ügyfél tooanother hálózati házirend-kiszolgáló, amely hello központi házirend tárolóként szolgál.

Ha az Azure NPS-bővítmény hello hello hálózati házirend-kiszolgáló és a távoli asztali átjáró integrálva van, a hello sikeres hitelesítési folyamat következőképpen történik:

1. hello távoli asztali átjárókiszolgáló hitelesítési kérelmet kap a távoli asztal felhasználójának tooconnect tooa erőforrás, például a távoli asztali munkamenetet. Egy RADIUS-ügyfél működött, hello távoli asztali átjáró kiszolgálójának alakítja át a hello kérelemüzenet tooa RADIUS-kérést, és elküldi a hello üzenet toohello RADIUS (NPS) kiszolgáló hello hálózati házirend-kiszolgáló bővítményt futtató. 
2. hello felhasználónév és jelszó kombináció ellenőrzése az Active Directoryban, és hello felhasználó hitelesítése.
3. Ha az összes megadott feltételek hello hello hálózati házirend-kiszolgáló kapcsolódási kérelem és hello hálózati házirendek-e (például időpont vagy csoport korlátozása), hálózati házirend-kiszolgáló bővítmény hello elindítja az Azure MFA másodlagos hitelesítési kérelmet. 
4. Az Azure MFA az Azure ad-val kommunikál, hello felhasználó adatait kéri le és hello (SMS-üzenet, mobilalkalmazás és így tovább) hello felhasználó által beállított hello metódussal másodlagos hitelesítést hajt végre. 
5. Hello MFA-kérdést sikeres, akkor az Azure MFA hello eredmény toohello hálózati házirend-kiszolgáló bővítmény kommunikál.
6. hello hálózati házirend-kiszolgáló, amelyen telepítve van-e az hello bővítmény hello RD CAP toohello távoli asztali átjáró-kiszolgáló RADIUS Access-Accept üzenetet küld.
7. hello kap hozzáférést a kért toohello hello távoli asztali átjáró hálózati erőforráshoz.

## <a name="prerequisites"></a>Előfeltételek
Ez a szakasz részletesen hello Előfeltételek előtt az Azure MFA integrálása hello távoli asztali átjáró szükséges. Mielőtt elkezdené, a következő előfeltételek teljesülése hello kell rendelkeznie.  

* Távoli asztali szolgáltatások (RDS) infrastruktúra
* Az Azure MFA-licenc
* Windows Server szoftver
* Hálózati házirend- és hozzáférés-szolgáltatások (NPS) szerepkör
* Az Azure AD szinkronizálni a helyszíni AD 
* Az Azure Active Directory GUID azonosítója

### <a name="remote-desktop-services-rds-infrastructure"></a>Távoli asztali szolgáltatások (RDS) infrastruktúra
Távoli asztali szolgáltatások (RDS) infrastruktúra működő helyen kell rendelkeznie. Ha nem, akkor gyorsan hozhat létre az Azure-ban ez az infrastruktúra hello alábbi gyors üzembe helyezési sablon: [távoli asztali munkamenet-gyűjtemény létrehozása központi telepítési](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment). 

Ha toomanually létrehozhat egy helyszíni távoli asztali szolgáltatások infrastruktúrát gyorsan tesztelésre, kövesse hello lépéseket toodeploy egy. 
**További**: [távoli asztali szolgáltatások telepítése az Azure gyors üzembe helyezési](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) és [alapszintű RDS-infrastruktúra telepítése](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure). 

### <a name="licenses"></a>Licencek
A licenc szükség van az Azure MFA számára, amely elérhető az Azure AD Premium, nagyvállalati mobilitási és biztonsági (EMS) vagy az MFA szolgáltatásra. További információkért lásd: [hogyan tooget Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md). Tesztelési célokra használható a próba-előfizetést.

### <a name="software"></a>Szoftver
hálózati házirend-kiszolgáló bővítmény hello van szükség a Windows Server 2008 R2 SP1 vagy újabb a hello NPS szerepkör-szolgáltatás telepítve. Ebben a szakaszban ismertetett összes hello Windows Server 2016 végeztek.

### <a name="network-policy-and-access-services-nps-role"></a>Hálózati házirend- és hozzáférés-szolgáltatások (NPS) szerepkör
hello NPS szerepkör-szolgáltatás funkcióit, valamint a hálózati házirend-állapotszolgáltatás hello RADIUS-kiszolgáló és az ügyfél biztosítja. Ezt a szerepkört a infrastruktúrában legalább két számítógépre telepítenie kell: távoli asztali átjáró és egy másik hello tagkiszolgáló vagy tartományvezérlő. Alapértelmezés szerint hello szerepkör már létezik a távoli asztali átjáró hello konfigurált hello számítógép.  Telepítenie kell hello NPS szerepkör a legalább egy másik számítógépre, például egy tartományvezérlő vagy tagkiszolgáló.

Szolgáltatás hello hálózati házirend-kiszolgáló szerepkör telepítésével kapcsolatos információkat a Windows Server 2012 vagy régebbi című [a NAP állapotházirend-kiszolgáló telepítése](https://technet.microsoft.com/library/dd296890.aspx). Ajánlott eljárások a hálózati házirend-kiszolgáló, többek között a következőket hello ajánlás tooinstall hálózati házirend-kiszolgáló egy tartományvezérlőn leírását lásd: [ajánlott eljárások a hálózati házirend-kiszolgáló](https://technet.microsoft.com/library/cc771746).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>A helyszíni Active Directoryval szinkronizálva az Azure Active Directory 
toouse hello hálózati házirend-kiszolgáló kiterjesztése a helyszíni felhasználók kell az Azure ad-val szinkronizálva és a multi-factor Authentication engedélyezett. Ez a szakasz azt feltételezi, hogy a helyi felhasználók és az Azure AD Connect használatával AD vannak szinkronizálva. Információk az Azure AD connect című [integrálása a helyszíni címtárakat az Azure Active Directoryval](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Az Azure Active Directory GUID azonosítója
hálózati házirend-kiszolgáló tooinstall, tooknow hello GUID-hello Azure AD kell. A GUID-azonosítója az Azure AD hello hello kereséshez utasításokat alatt.

## <a name="configure-multi-factor-authentication"></a>Többtényezős hitelesítés beállítása 
Ez a szakasz leírja a távoli asztali átjáró hello Azure MFA integrálásához. Ha Ön rendszergazda konfigurálnia kell hello Azure MFA szolgáltatással a felhasználók saját maguk regisztrálhatják a multi-factor Authentication eszközök vagy alkalmazások.

Hello kövesse [Ismerkedés az Azure multi-factor Authentication hello felhőben](multi-factor-authentication-get-started-cloud.md) tooenable többtényezős hitelesítés az Azure Active Directory-felhasználók. 

### <a name="configure-accounts-for-two-step-verification"></a>A kétlépéses ellenőrzéshez fiókok beállítása
Egy fiók engedélyezése után az MFA szolgáltatásra, nem tud bejelentkezni, tooresources által szabályozott hello többtényezős hitelesítési szabályzat mindaddig, amíg sikeresen konfigurálta a megbízható eszköz toouse hello második hitelesítési tényezőt kell hitelesített segítségével kétlépéses hitelesítéssel.

Hello kövesse [mit Azure multi-factor Authentication jelent a számomra?](./end-user/multi-factor-authentication-end-user.md) toounderstand és megfelelően konfigurálja a felhasználói fiókjához a multi-factor Authentication az eszközök.

## <a name="install-and-configure-nps-extension"></a>Telepítse és konfigurálja a hálózati házirend-kiszolgáló bővítmény
Ez a szakasz leírja a távoli asztali szolgáltatások infrastruktúra toouse Azure MFA ügyfél-hitelesítés konfigurálása a távoli asztali átjáró hello.

### <a name="acquire-azure-active-directory-guid-id"></a>Szerezzen be az Azure Active Directory GUID azonosítója

Hello hálózati házirend-kiszolgáló bővítmény hello konfigurációjának részeként kell toosupply rendszergazdai hitelesítő adataival és hello Azure AD-azonosító az Azure AD-bérlő. hello lépések bemutatják, hogyan tooget hello bérlői azonosító.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) hello globális rendszergazdájaként hello Azure bérlői.
2. A bal oldali navigációs hello, válassza ki a hello **Azure Active Directory** ikonra.
3. Válassza ki **tulajdonságok**.
4. Hello tulajdonságok panelére léphet, hello Directory azonosító mellett kattintson hello **másolási** ikonra, toocopy hello azonosító tooclipboard alább látható módon.

 ![Tulajdonságok](./media/nps-extension-remote-desktop-gateway/image1.png)

### <a name="install-hello-nps-extension"></a>Hello NPS-kiterjesztés telepítése
Hello NPS-kiterjesztés telepítése a kiszolgálón, amelyen hello hálózati házirend- és Access Services (NPS) szerepkör telepítve van. Ez a Tervező hello RADIUS-kiszolgáló funkcionál. 

>[!Important]
> Győződjön meg arról, hogy nem telepíti a távoli asztali átjáró kiszolgálón hello hálózati házirend-kiszolgáló bővítmény.
> 

1. Töltse le a hello [hálózati házirend-kiszolgáló bővítmény](https://aka.ms/npsmfa). 
2. Másolás hello beállítása végrehajtható fájl (NpsExtnForAzureMfaInstaller.exe) toohello hálózati házirend-kiszolgáló.
3. Hello hálózati házirend-kiszolgáló, kattintson duplán a **NpsExtnForAzureMfaInstaller.exe**. Ha a rendszer kéri, kattintson a **futtatása**.
4. A hálózati házirend-kiszolgáló bővítmény hello Azure MFA párbeszédpanel, tekintse át a hello szoftverlicenc-feltételeket, ellenőrizze **elfogadom a licencfeltételeket toohello**, és kattintson a **telepítése**.
 
  ![Az Azure MFA beállítása](./media/nps-extension-remote-desktop-gateway/image2.png)

5. A hálózati házirend-kiszolgáló bővítmény hello Azure MFA párbeszédpanel kattintson a Bezárás gombra. 

  ![Az Azure MFA használatára a hálózati házirend-kiszolgáló bővítmény](./media/nps-extension-remote-desktop-gateway/image3.png)

### <a name="configure-certificates-for-use-with-hello-nps-extension-using-a-powershell-script"></a>PowerShell parancsfájl használatával hello hálózati házirend-kiszolgáló kiterjesztésű használt tanúsítványok konfigurálása
A következő lépésben tooconfigure tanúsítványok hello hálózati házirend-kiszolgáló bővítmény tooensure biztonságos kommunikációt, és biztosítani általi használatra. hello hálózati házirend-kiszolgáló összetevői az alábbiak a Windows PowerShell-parancsfájlt, amely beállítja a hálózati házirend-kiszolgáló egy önaláírt tanúsítványt. 

hello parancsfájl hello a következő műveleteket hajtja végre:

* Létrehoz egy önaláírt tanúsítványt
* Társítja az Azure ad-val egyszerű tanúsítvány-tooservice nyilvános kulcs
* Tárolók hello cert hello helyi számítógép tárolójában
* Hozzáférési toohello tanúsítvány titkos kulcs toohello hálózati felhasználó
* Hálózati házirend-kiszolgáló szolgáltatás újraindítása

Ha azt szeretné, toouse saját tanúsítványok, a tanúsítvány toohello szolgáltatás elv tooassociate hello nyilvános Azure ad-val kell, és így tovább.

toouse hello parancsfájl, adja meg a hello bővítmény a Azure AD rendszergazdai hitelesítő adataival, és az Azure AD bérlő azonosítója már korábban átmásolt hello. Hello hálózati házirend-kiszolgáló bővítmény telepítési hello parancsfájl futtatása minden hálózati házirend-kiszolgálón. Majd hello a következő:

1. Nyisson meg egy felügyeleti Windows PowerShell-parancssort.
2. Hello PowerShell parancssorába írja be a **cd "c:\Program Files\Microsoft\AzureMfa\Config"**, és nyomja le az ENTER **ENTER**.
3. Típus _.\AzureMfsNpsExtnConfigSetup.ps1_, és nyomja le az ENTER **ENTER**. hello parancsfájl toosee ellenőrzi, hogy a hello Azure Active Directory PowerShell-modul telepítve van. Ha nincs telepítve, hello parancsfájl telepíti hello modul.

  ![Az Azure AD PowerShell segítségével](./media/nps-extension-remote-desktop-gateway/image4.png)
  
4. Miután hello parancsfájl ellenőrzi a hello hello PowerShell-modult, hello Azure Active Directory PowerShell modul párbeszédpanel jelenik meg. Hello párbeszédpanelen adja meg a Azure AD rendszergazdai hitelesítő adatait, és a jelszót, és kattintson **bejelentkezés**.

  ![Powershell-fiók megnyitása](./media/nps-extension-remote-desktop-gateway/image5.png)

5. Amikor a rendszer kéri, illessze be a korábban kimásolt toohello vágólapra hello Bérlőazonosító, és nyomja le az ENTER **ENTER**.

  ![Adja meg a bérlő azonosítója](./media/nps-extension-remote-desktop-gateway/image6.png)

6. hello parancsfájl létrehoz egy önaláírt tanúsítványt, és más konfigurációs módosításokat hajt végre. hello kimeneti például a lent látható módon hello lemezképet kell lennie.

  ![Önaláírt tanúsítvány](./media/nps-extension-remote-desktop-gateway/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Hálózati házirend-kiszolgáló-összetevők konfigurálása a távoli asztali átjáró
Ebben a szakaszban konfigurálhatja a távoli asztali átjáró kapcsolatengedélyezési házirendek hello és más RADIUS-beállításai.

hello hitelesítési folyamat megköveteli, hogy a RADIUS-üzenetek között hello távoli asztali átjáró és hello hálózati házirend-kiszolgáló hello hálózati házirend-kiszolgálót futtató felcserélhetők. Ez azt jelenti, hogy konfigurálnia kell RADIUS-ügyfélbeállításokat a távoli asztali átjáró és a hello hálózati házirend-kiszolgáló, amelyen telepítve van a hálózati házirend-kiszolgáló bővítmény hello. 

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-toouse-central-store"></a>Távoli asztali átjáró kapcsolat engedélyezési házirendek toouse központi tároló konfigurálása
Távoli asztali kapcsolat engedélyezési házirendek (RD CAP-ok) adja meg a kapcsolódó tooa távoli asztali átjáró kiszolgálójának hello követelményeit. Távoli asztali CAPs helyileg tárolhatja (alapértelmezett), illetve kell tárolni az NPS szolgáltatást futtató RD CAP üzlet központi. Távoli asztali szolgáltatások az Azure MFA tooconfigure integrációja, toospecify hello használata egy központi tárolóban kell.

1. Hello RD átjárókiszolgálón, nyissa meg a **Kiszolgálókezelő**. 
2. Hello menüjének **eszközök**, pont túl**távoli asztali szolgáltatások**, és kattintson a **távoli asztali átjárókezelő**.

  ![Távoli asztali szolgáltatások](./media/nps-extension-remote-desktop-gateway/image8.png)

3. A távoli asztali átjáró Manager hello, kattintson a jobb gombbal  **\[kiszolgálónév\] (helyi)**, és kattintson a **tulajdonságok**.

  ![Kiszolgáló neve](./media/nps-extension-remote-desktop-gateway/image9.png)

4. A hello tulajdonságai párbeszédpanelen válassza ki a hello **RD CAP** áruház lapján.
5. Hello távoli asztali házirendjeihez lapon jelölje be **központi házirend-kiszolgálót futtató**. 
6. A hello **adjon meg egy nevet vagy IP-cím hello házirend-kiszolgálót futtató** mező, típus hello kiszolgáló IP-címét vagy kiszolgálónevét hello hello hálózati házirend-kiszolgáló bővítmény telepítési.

  ![Adja meg nevét vagy IP-cím](./media/nps-extension-remote-desktop-gateway/image10.png)
  
7. Kattintson az **Add** (Hozzáadás) parancsra.
8. A hello **közös titkos kulcs** párbeszédpanelen adja meg a közös titkos kulcsot, és kattintson **OK**. Győződjön meg arról, jegyezze fel a közös titkos kulcsot, és tárolja biztonságos helyen hello rekord.

 >[!NOTE]
 >Közös titkos kulcs használata tooestablish megbízhatósági hello RADIUS-kiszolgálók és az ügyfelek között. Hozzon létre egy hosszú és összetett jelszót.
 >

 ![Közös titkos kulcs](./media/nps-extension-remote-desktop-gateway/image11.png)

9. Kattintson a **OK** tooclose hello párbeszédpanel megnyitásához.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>A távoli asztali átjáró NPS RADIUS időtúllépési értékének konfigurálása
Nincs tooensure idő toovalidate felhasználók hitelesítő adatait, hajtsa végre a kétlépéses ellenőrzést, választ kap, és tooRADIUS üzenetek válaszolni, szükséges tooadjust hello RADIUS időtúllépési értéket.

1. Hello RD átjárókiszolgálón, a Kiszolgálókezelőben kattintson **eszközök**, és kattintson a **hálózati házirend-kiszolgáló**. 
2. A hello **hálózati házirend-kiszolgáló (helyi)** konzolt, bontsa ki a **RADIUS-ügyfelek és kiszolgálók**, és válassza ki **távoli RADIUS-kiszolgáló**.

 ![Távoli RADIUS-kiszolgáló](./media/nps-extension-remote-desktop-gateway/image12.png)

3. Hello részleteket tartalmazó ablaktáblában kattintson duplán a **TS ÁTJÁRÓ kiszolgáló csoport**.

 >[!NOTE]
 >A RADIUS-kiszolgálócsoport hello központi kiszolgáló az NPS-házirendek konfigurálása során létrejött. Ha egynél több hello csoport hello távoli asztali átjáró RADIUS-üzenetek toothis kiszolgáló vagy kiszolgálók csoport továbbítja.
 >

4. A hello **TS ÁTJÁRÓ kiszolgáló csoport tulajdonságai** párbeszédpanel megnyitásához, jelölje be hello IP-cím vagy hello toostore RD CAP-OK konfigurálta a hálózati házirend-kiszolgáló, és kattintson a neve **szerkesztése**. 

 ![TS átjáró kiszolgáló csoport](./media/nps-extension-remote-desktop-gateway/image13.png)

5. A hello **RADIUS-kiszolgáló szerkesztése** párbeszédpanel megnyitásához, jelölje be hello **terheléselosztás** fülre.
6. A hello **terheléselosztás** lap hello **eldobása előtt válasz nélkül másodpercben** mezőben módosítsa hello alapértelmezett érték 3 tooa értéket 30 – 60 másodperc között.
7. A hello **kiszolgáló nem érhető el, amelynél kérelmek között eltelt másodpercek száma** mezőbe hello alapértelmezett értéke 30 másodperc tooa értéket, amely nagyobb, mint hello előző lépésben megadott hello érték egyenlő tooor módosítása.

 ![RADIUS-kiszolgáló szerkesztése](./media/nps-extension-remote-desktop-gateway/image14.png)

8.  Kattintson az OK gombra, két tooclose hello párbeszédpanelek időpontokat.

### <a name="verify-connection-request-policies"></a>Kapcsolatkérelem-házirend ellenőrzése 
Alapértelmezés szerint a hello távoli asztali átjáró toouse kapcsolatengedélyezési házirendek, egy központi házirend áruház konfigurálásakor hello távoli asztali átjáró konfigurált tooforward CAP kérelmek toohello hálózati házirend-kiszolgáló. hello hálózati házirend-kiszolgáló hello Azure MFA kiterjesztésű telepítve, a folyamatok hello RADIUS hozzáférési kérelem. hello lépések bemutatják, hogyan tooverify hello alapértelmezett kapcsolat házirendre vonatkozó kérelmet. 

1. A távoli asztali átjáró hello hello hálózati házirend-kiszolgáló (helyi) konzolon bontsa ki **házirendek**, és válassza ki **kapcsolatkérelem-házirendek**.
2. Kattintson a jobb gombbal **házirendek csatlakozás**, és kattintson duplán a **TS ÁTJÁRÓ engedélyezési házirend**.
3. A hello **TS ÁTJÁRÓ engedélyezési házirend tulajdonságai** párbeszédpanelen kattintson hello **beállítások** lapon.
4. A **beállítások** kattintson a lap Kapcsolatkérelem továbbítása **hitelesítési**. RADIUS-ügyfél a hitelesítési kérelmek konfigurált tooforward.

 ![Hitelesítési beállítások](./media/nps-extension-remote-desktop-gateway/image15.png)
 
5. Kattintson a **Mégse**. 

## <a name="configure-nps-on-hello-server-where-hello-nps-extension-is-installed"></a>Hálózati házirend-kiszolgáló hello kiszolgálón, amelyen telepítve van a hálózati házirend-kiszolgáló bővítmény hello konfigurálása
hello hálózati házirend-kiszolgáló telepített igények toobe képes tooexchange RADIUS-üzenetek hello hálózati házirend-kiszolgáló a távoli asztali átjáró hello hello hálózati házirend-kiszolgáló bővítmény esetén. exchange-tooenable ezt az üzenetet, tooconfigure hello hálózati házirend-kiszolgáló összetevői hello kiszolgálón, amelyen telepítve van a hálózati házirend-kiszolgáló hálózatbővítési szolgáltatásként hello van szüksége. 

### <a name="register-server-in-active-directory"></a>Kiszolgáló regisztrálása az Active Directoryban
toofunction megfelelően az ebben a forgatókönyvben, meg kell regisztrálni az Active Directory toobe hello hálózati házirend-kiszolgáló.

1. Nyissa meg **Kiszolgálókezelő**.
2. A Kiszolgálókezelőben kattintson **eszközök**, és kattintson a **hálózati házirend-kiszolgáló**. 
3. Hello hálózati házirend-kiszolgáló konzol, kattintson a jobb gombbal **hálózati házirend-kiszolgáló (helyi)**, és kattintson a **kiszolgáló regisztrálása az Active Directoryban**. 
4. Kattintson a **OK** kétszer.

 ![Regisztrálja a kiszolgálót az ad-ben](./media/nps-extension-remote-desktop-gateway/image16.png)

5. Hagyja nyitva a következő eljárással hello hello konzol.

### <a name="create-and-configure-radius-client"></a>Hozza létre és konfigurálja a RADIUS-ügyfél 
Távoli asztali átjáró hello toobe egy RADIUS-ügyfél toohello hálózati házirend-kiszolgáló konfigurálva van szüksége. 

1. Hálózati házirend-kiszolgáló bővítmény hello futtató, a hello hello hálózati házirend-kiszolgálón **hálózati házirend-kiszolgáló (helyi)** konzoljában a jobb gombbal **RADIUS-ügyfelek** kattintson **új**.

 ![Új RADIUS-ügyfelek](./media/nps-extension-remote-desktop-gateway/image17.png)

2. A hello **új RADIUS-ügyfél** párbeszédpanelen adja meg egy rövid nevet, például a _átjáró_, és hello IP-cím vagy hello távoli asztali átjáró kiszolgáló DNS-neve. 
3. A hello **közös titkos kulcs** és hello **közös titkos kulcs jóváhagyása** mezőkben hello adja meg ugyanazt a titkos kulcsot előtt használt.

 ![Nevét és címét](./media/nps-extension-remote-desktop-gateway/image18.png)

4. Kattintson a **OK** tooclose hello új RADIUS-ügyfél párbeszédpanel.

### <a name="configure-network-policy"></a>Hálózati házirend konfigurálása
Visszaírási hello hálózati házirend-kiszolgáló az Azure MFA bővítmény hello hello kijelölt központi házirend áruház hello kapcsolat engedélyezési házirend (CAP). Ezért kell tooimplement egy CAP hello hálózati házirend-kiszolgáló server tooauthorize érvényes kapcsolatok kérelemhez.  

1. Hello hálózati házirend-kiszolgáló (helyi) konzolon bontsa ki **házirendek**, és kattintson a **hálózati házirendek**.
2. Kattintson a jobb gombbal **kapcsolatok tooother kiszolgálók**, és kattintson a **házirend ismétlődő**. 

 ![Ismétlődő házirend](./media/nps-extension-remote-desktop-gateway/image19.png)

3. Kattintson a jobb gombbal **másolása a kapcsolatok tooother kiszolgálók**, és kattintson a **tulajdonságok**.

 ![Hálózat tulajdonságai](./media/nps-extension-remote-desktop-gateway/image20.png)

4. A hello **másolása a kapcsolatok tooother kiszolgálók** párbeszédpanelen, a házirend neve adja meg egy megfelelő nevet, például a **RDG_CAP**. Ellenőrizze **házirend engedélyezve**, és válassza ki **hozzáférést**. A hálózati hozzáférési típus, bejelölheti **távoli asztali átjáró**, vagy hagyhatja azt **meghatározatlan**.

 ![A kapcsolatok másolása](./media/nps-extension-remote-desktop-gateway/image21.png)

5.  Kattintson a hello **megkötések** fülre, és ellenőrizze **engedélyezése az ügyfelek tooconnect hitelesítés nélkül**.

 ![Ügyfelek tooconnect](./media/nps-extension-remote-desktop-gateway/image22.png)

6. Választható lehetőségként kattintson a hello **feltételek** lapra, és adja hozzá a feltételeket, amelyeknek teljesülniük kell az hello kapcsolat toobe jogosult, például egy adott Windows-csoport tagjának kell lennie.

 ![Feltételek](./media/nps-extension-remote-desktop-gateway/image23.png)

7. Kattintson az **OK** gombra. Amikor a kért tooview kapcsolódó súgótémakörben hello, kattintson **nem**.
8. Győződjön meg arról, hogy az új házirend hello tetején hello listában, hogy hello házirend engedélyezve van, és hozzáférést biztosít.

 ![Hálózati házirendek](./media/nps-extension-remote-desktop-gateway/image24.png)

## <a name="verify-configuration"></a>Konfiguráció ellenőrzése
tooverify hello konfigurálása kell a távoli asztali átjáró hello toolog megfelelő RDP-ügyféllel. Lehet, hogy toouse, amely engedélyezi-e a kapcsolat engedélyezési házirendeket, és az Azure MFA számára engedélyezve van. 

Az alábbi képen hello megjelenítése, hello használható **távoli asztali webes elérés** lap.

![Távoli asztali webes elérés](./media/nps-extension-remote-desktop-gateway/image25.png)

Belépéskor sikeresen megtörtént a hitelesítő adatait az elsődleges hitelesítéshez, távoli asztali kapcsolat párbeszédpanel hello állapotát jeleníti meg a távoli kapcsolat kezdeményezése alább látható módon. 

Ha sikeres hitelesítéséhez az Azure MFA korábban konfigurált hello másodlagos hitelesítési módszerrel, csatlakoztatott toohello erőforrás áll. Ha másodlagos hitelesítés hello nem sikeres, hozzáférési tooresource sem kap. 

![Távoli kapcsolat kezdeményezése](./media/nps-extension-remote-desktop-gateway/image26.png)

Hello példában az alábbi hello hitelesítő alkalmazást Windows Phone-eszközön használt tooprovide hello másodlagos hitelesítés.

![Fiókok](./media/nps-extension-remote-desktop-gateway/image27.png)

Miután sikeresen hitelesítette hello másodlagos hitelesítési módszerrel, a távoli asztali átjáró hello a szokásos módon van bejelentkezve. Azonban mivel szükséges toouse egy másodlagos hitelesítési módszer használatával, ha megbízható eszközt, hello bejelentkezési folyamat nem nagyobb biztonságot nyújt, mint egyébként lenne.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Eseménynaplók sikeres bejelentkezési események megtekintése
tooview hello sikeres bejelentkezési események hello Windows Eseménynapló-naplókban, kiadhatja a következő Windows PowerShell parancs tooquery hello Windows Terminálszolgáltatások és a Windows biztonsági naplókat hello.

tooquery sikeres bejelentkezési események hello átjáró műveleti naplókat _(eseménynapló\alkalmazás- és a szolgáltatások Logs\Microsoft\Windows\TerminalServices-Gateway\Operational_, a következő parancsok hello használata:

* _Get-WinEvent - naplónév Microsoft-Windows-TerminalServices-átjáró/működési_ |} ahol {$_.ID - eq "300"} |} FL 
* A parancs a Windows-eseményeket, amelyek megjelenítik a hello felhasználói erőforrás engedélyezési házirend követelményeinek (RD RAP) teljesül, és nyert hozzáférést jeleníti meg.

![Az Eseménynapló megtekintése](./media/nps-extension-remote-desktop-gateway/image28.png)

* _Get-WinEvent - naplónév Microsoft-Windows-TerminalServices-átjáró/működési_ |} ahol {$_.ID - eq "200"} |} FL
* A parancs megjeleníti hello események, amelyek megjelenítik a felhasználói kapcsolat engedélyezési házirend követelményeinek teljesülése esetén.

![Kapcsolat engedélyezési](./media/nps-extension-remote-desktop-gateway/image29.png)

Ez a napló és szűrő az eseményazonosítót, 300 és 200 is megtekintheti. sikeres bejelentkezési események tooquery hello biztonsági eseménynapló, hello a következő parancsot használja:

* _Get-WinEvent - naplónév biztonsági_ |} ahol {$_.ID - eq "6272"} |} FL 
* Ez a parancs futhat bármelyiken hello központi hálózati házirend-kiszolgáló vagy a távoli asztali átjárókiszolgáló hello. 

![Sikeres bejelentkezési események](./media/nps-extension-remote-desktop-gateway/image30.png)

Alább látható módon hello biztonsági naplóban vagy hello hálózati házirend- és elérési szolgáltatások egyéni nézet, is megtekintheti:

![Hálózati házirend- és elérési szolgáltatások](./media/nps-extension-remote-desktop-gateway/image31.png)

Hello hálózati házirend-kiszolgáló bővítmény telepítési az Azure MFA hello kiszolgálón található alkalmazás eseménynaplók, adott toohello bővítmény _alkalmazások és szolgáltatások Logs\Microsoft\AzureMfa_. 

![Az Eseménynapló alkalmazást](./media/nps-extension-remote-desktop-gateway/image32.png)

## <a name="troubleshoot-guide"></a>Az útmutató hibaelhárítása

Hello konfigurációs nem a várt módon működik, ha hello első hely toostart tootroubleshoot, amely a felhasználó hello tooverify konfigurált toouse Azure MFA. Csatlakozás toohello hello felhasználó [Azure-portálon](https://portal.azure.com). Ha a felhasználó másodlagos ellenőrzési kéri, és sikeresen tudja elvégezni a hitelesítést, megszüntetheti az Azure MFA helytelen konfigurációban.

Ha az Azure MFA hello felhasználó(k) szolgáltatás működik, tekintse át hello vonatkozó eseménynaplók. Ezek közé tartozik a hello biztonsági esemény, az átjáró működik és az Azure MFA naplók hello előző szakaszban tárgyalt. 

Alább egy példa kimenet megjeleníti a sikertelen bejelentkezési esemény (Event ID 6273) a biztonsági napló van.

![Sikertelen bejelentkezési esemény](./media/nps-extension-remote-desktop-gateway/image33.png)

Alább az hello AzureMFA naplókból kapcsolódó esemény:

![Az Azure MFA-napló](./media/nps-extension-remote-desktop-gateway/image34.png)

Speciális tooperform beállítások, tájékoztatást hello hálózati házirend-kiszolgáló naplófájlok hello NPS szolgáltatást futtató hibaelhárítása. Ezekben a naplófájlokban létrejönnek _%SystemRoot%\System32\Logs_ vesszővel tagolt szövegfájlok mappában. 

Ezek leírását naplófájlok című [értelmezhetők hálózati házirend-kiszolgáló naplófájlok](https://technet.microsoft.com/library/cc771748.aspx). Ezekben a naplófájlokban hello bejegyzések nehéz toointerpret nélkül importálja őket egy táblázatot vagy egy adatbázisban is lehet. Több IAS elemzők megtalálhatja a értelmezése hello naplófájlok online tooassist. 

hello az alábbi képen láthatók hello kimeneti az egyik ilyen letölthető [shareware alkalmazás](http://www.deepsoftware.com/iasviewer). 

![Shareware alkalmazás](./media/nps-extension-remote-desktop-gateway/image35.png)

Végezetül, a további beállítások elhárításával kapcsolatos tudnivalókat, protokollelemző is használhat ilyen [Microsoft Message Analyzert](https://technet.microsoft.com/library/jj649776.aspx). 

hello a Microsoft Message Analyzert alábbi képen látható szűrt hello felhasználónevet tartalmazó RADIUS protokollal a hálózati forgalom **Contoso\AliceC**.

![Microsoft Message Analyzert](./media/nps-extension-remote-desktop-gateway/image36.png)

## <a name="next-steps"></a>Következő lépések
[Hogyan tooget Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md)

[Távoli asztali átjáró és RADIUS-t használó Azure Multi-Factor Authentication-kiszolgáló](multi-factor-authentication-get-started-server-rdg.md)

[A helyszíni címtárak integrálása az Azure Active Directoryval](../active-directory/connect/active-directory-aadconnect.md)
