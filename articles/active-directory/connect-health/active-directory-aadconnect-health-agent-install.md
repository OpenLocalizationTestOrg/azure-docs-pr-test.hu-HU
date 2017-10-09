---
title: "aaaAzure AD Connect Health-ügynök telepítése |} Microsoft Docs"
description: "Ez a hello Azure AD Connect Health lap, amely leírja a hello ügynök telepítése az AD FS és a szinkronizálás."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 9019628ec1d4c477496e08910cfd668ed1933a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-health-agent-installation"></a>Az Azure AD Connect Health-ügynök telepítése
Ez a dokumentum útmutatást nyújt a telepítése és konfigurálása hello Azure AD Connect Health-ügynököket. Letöltheti a hello ügynökök [Itt](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

## <a name="requirements"></a>Követelmények
a következő táblázat hello az Azure AD Connect Health használatának követelményeit sorolja fel.

| Követelmény | Leírás |
| --- | --- |
| Azure AD Premium |Az Azure AD Connect Health egy Azure AD Premium szolgáltatás, amelyhez Azure AD Premium szükséges. </br></br>További információkért lásd: [Ismerkedés az Azure AD Premium szolgáltatással](../active-directory-get-started-premium.md) </br>toostart egy 30 napos ingyenes próbaidőszakra, lásd: [a próbaverzió elindítása.](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Kell egy globális rendszergazda az Azure AD tooget az Azure AD Connect Health használatába |Alapértelmezés szerint csak hello globális rendszergazdák telepítése és konfigurálása hello health ügynökök tooget elindult, hozzáférés hello portálon, és műveletek hajtsanak végre az Azure AD Connect Health. További információkért lásd: [Az Azure AD-címtár felügyelete](../active-directory-administer.md). <br><br> Szerepköralapú hozzáférés-vezérlés használatával hozzáférést biztosíthat tooAzure AD Connect Health tooother felhasználókat a szervezetben. További információkért lásd: [Role Based Access Control for Azure AD Connect Health](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) (Szerepköralapú hozzáférés-vezérlés az Azure AD Connect Health-hez) </br></br>**Fontos:** hello hello ügynökök telepítése munkahelyi vagy iskolai fiókot kell használt fiók. Nem lehet Microsoft-fiók. További információkért lásd: [Regisztráció az Azure-ba szervezetként](../sign-up-organization.md) |
| Az Azure AD Connect Health-ügynököt az összes célkiszolgálóra telepíteni kell | Az Azure AD Connect Health telepíteni kell a hello Health-ügynököket toobe és célkiszolgálókon tooreceive hello adatok konfigurált és hello figyelés és az elemzések képességek biztosítása </br></br>Például a tooget adatait az AD FS infrastruktúra hello ügynök telepítenie kell a hello AD FS és a webalkalmazás-Proxy kiszolgálókon. Ehhez hasonlóan tooget adatokat a helyszíni Active Directory tartományi szolgáltatások infrastruktúra hello ügynököt telepíteni kell a hello tartományvezérlőkön. </br></br> |
| Kimenő kapcsolódás toohello Azure szolgáltatásvégpontokra | Telepítés és a futásidő során hello ügynöknek szüksége van a kapcsolat tooAzure AD Connect Health szolgáltatásvégpontjaival. Ha a kimenő kapcsolat le van tiltva a tűzfalakat használ, győződjön meg arról, hogy a következő végpontok hello kerülnek toohello engedélyezettek listájához: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net – port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|IP-címeken alapuló kimenő kapcsolatok | Az IP-cím alapú szűrést, ha a tűzfal, tekintse meg a toohello [Azure IP-címtartományok](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| A kimenő forgalom SSL-vizsgálata le van tiltva, illetve a rendszer szűri | hello ügynök regisztrációs lépés vagy az adatok feltöltési művelet sikertelen lehet, ha SSL-ellenőrzést vagy a kimenő forgalom hello hálózati rétegben megszüntetése. |
| Tűzfal portok hello hello-ügynököt futtató kiszolgálón. |hello ügynöknek szüksége van a tűzfal portok toobe ahhoz, hogy a hello Azure AD Health szolgáltatásvégpontjaival hello ügynök toocommunicate nyissa meg a következő hello.</br></br><li>443-as TCP-port</li><li>5671-es TCP-port</li> |
| Következő webhelyek, ha engedélyezve van az Internet Explorer fokozott biztonsági hello engedélyezése |Ha az Internet Explorer fokozott biztonsági engedélyezve van, majd hello következő webhelyeken talál engedélyezni kell az hello kiszolgálóra, amelyik folyamatos toohave hello ügynök van telepítve.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>a szervezet Azure Active Directory megbízható hello összevonási kiszolgáló. Például: https://sts.contoso.com</li> |
|A FIPS letiltása|Az Azure AD Connect Health-ügynökök nem támogatják a FIPS-t.|

## <a name="installing-hello-azure-ad-connect-health-agent-for-ad-fs"></a>Hello Azure AD Connect Health-ügynök az AD FS telepítése
toostart hello az ügynök telepítése, kattintson duplán a letöltött hello .exe fájlt. A hello első képernyőn kattintson a telepítés gombra.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health-requirements/install1.png)

Ha hello telepítés befejeződött, kattintson a konfigurálása.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health-requirements/install2.png)

Ekkor elindul a PowerShell ablakban tooinitiate hello ügynök regisztrációs folyamat. Amikor a rendszer kéri, jelentkezzen be az Azure AD-fiókot, amely rendelkezik hozzáférési tooperform ügynök regisztrációját. Alapértelmezés szerint a globális rendszergazdai fiókkal hello hozzáféréssel rendelkezik.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health-requirements/install3.png)

A bejelentkezést követően a PowerShell továbblép. Miután befejeződött, bezárhatja a Powershellt, és hello konfigurálása nem fejeződött be.

Ezen a ponton hello ügynök szolgáltatások kell elindult automatikusan így hello ügynök feltöltési hello szükséges adatok toohello felhőszolgáltatás biztonságos módon.

Ha nem teljesítette az összes hello Előfeltételek az hello korábbi szakaszokban ismertetett módon, figyelmeztetések hello PowerShell ablakban jelennek meg. Lehet, hogy toocomplete hello [követelmények](active-directory-aadconnect-health-agent-install.md#requirements) hello ügynök telepítése előtt. a következő képernyőkép hello ezeket a hibákat példája.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health-requirements/install4.png)

tooverify hello ügynök telepítve van, keresse meg a következő szolgáltatások hello kiszolgálón hello. Ha végrehajtotta hello konfigurációs, akkor már futnia kell. Ellenkező esetben le vannak állítva amíg hello konfigurálása nem fejeződött be.

* Azure AD Connect Health AD FS Diagnostics szolgáltatás
* Azure AD Connect Health AD FS Insights szolgáltatás
* Azure AD Connect Health AD FS Monitoring szolgáltatás

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health-requirements/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Ügynökök telepítése Windows Server 2008 R2 kiszolgálókon
Windows Server 2008 R2 kiszolgálók esetén végezze el a következő lépéseket:

1. Ellenőrizze, hogy hello kiszolgálón fut-e Service Pack 1 vagy újabb rendszerre.
2. Az ügynök telepítéséhez kapcsolja ki az Internet Explorer - Fokozott biztonsági beállításokat:
3. Telepítse a Windows PowerShell 4.0 mindegyik hello kiszolgálón hello AD Health-ügynök telepítése előtt. a Windows PowerShell 4.0 tooinstall:
   * Telepítés [Microsoft .NET-keretrendszer 4.5](https://www.microsoft.com/download/details.aspx?id=40779) a következő hivatkozás toodownload hello offline telepítőt hello segítségével.
   * Telepítse a PowerShell ISE-t (a Windows-szolgáltatásokból)
   * Telepítse a hello [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
   * Telepítse az Internet Explorer 10-es verzió vagy újabb hello kiszolgálón. Szükséges (az állapotfigyelő szolgáltatás hello tooauthenticate, Azure-rendszergazdai hitelesítő adataival.)
4. A Windows PowerShell 4.0 telepítése Windows Server 2008 R2 rendszerben További információkért lásd: hello wikicikket [Itt](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>AD FS-naplózás engedélyezése
> [!NOTE]
> Ez a szakasz csak tooAD FS kiszolgálók vonatkozik. Nincs toofollow ezeket a lépéseket a webalkalmazás-Proxy kiszolgálók hello.
>

Ahhoz, hogy a használatelemzés szolgáltatás toogather és elemezheti hello adatok, az Azure AD Connect Health-ügynök hello kell hello hello AD FS-naplók információkat. Ezek a naplók alapértelmezés szerint nincsenek bekapcsolva. A következő eljárások tooenable AD FS-naplózás használata hello és toolocate hello AD FS naplózása az AD FS-kiszolgálók, a naplók.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2008-r2"></a>az AD FS a Windows Server 2008 R2 naplózásának tooenable
1. Kattintson a **Start**, pont túl**programok**, pont túl**felügyeleti eszközök**, és kattintson a **helyi biztonsági házirend**.
2. Keresse meg a toohello **biztonsági beállítások\Helyi házirendek\Felhasználói jogosultságok kezelése** mappát, majd kattintson duplán a biztonsági naplózás létrehozása elemre.
3. A hello **helyi biztonsági beállítások** lapján ellenőrizze, hogy a hello AD FS 2.0 szolgáltatásfiók szerepel-e. Ha nincs, kattintson a **felhasználó vagy csoport hozzáadása** és toohello listát vesznek fel, és kattintson **OK**.
4. tooenable naplózás esetén nyisson meg egy parancssort emelt szintű jogosultságokkal, és futtassa a következő parancs hello:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zárja be a helyi biztonsági házirend, és ezután hello felügyeleti beépülő modul megnyitásához. tooopen hello kezelés beépülő modulja, kattintson a **Start**, pont túl**programok**, pont túl**felügyeleti eszközök**, majd kattintson az AD FS 2.0 Management.
6. Hello műveletek ablaktáblán kattintson az összevonási szolgáltatás tulajdonságainak szerkesztése.
7. A hello **összevonási szolgáltatás tulajdonságainak** párbeszédpanelen kattintson az hello **események** lapon.
8. Jelölje be hello **sikernaplók** és **hibanaplók** jelölőnégyzeteket.
9. Kattintson az **OK** gombra.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>a Windows Server 2012 R2 AD FS naplózásának tooenable
1. Nyissa meg **helyi biztonsági házirend** megnyitásával **Kiszolgálókezelő** hello kezdőképernyőn, vagy a Kiszolgálókezelő hello hello asztali tálcán, majd kattintson a **eszközök/helyi biztonsági házirend**.
2. Keresse meg a toohello **biztonsági beállítások\Helyi házirendek\Felhasználói jogosultságok kiosztása** mappát, majd kattintson rá duplán **biztonsági naplózás létrehozása**.
3. A hello **helyi biztonsági beállítások** lapján ellenőrizze, hogy a hello AD FS szolgáltatásfiók szerepel-e. Ha nincs, kattintson a **felhasználó vagy csoport hozzáadása** és toohello listát vesznek fel, és kattintson **OK**.
4. naplózás tooenable, nyisson meg egy parancssort emelt szintű jogosultságokkal, és futtassa a következő parancs hello: ```auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable```.
5. Bezárás **helyi biztonsági házirend**, majd nyissa meg a hello **AD FS felügyeleti** beépülő modul (a Kiszolgálókezelőben kattintson az eszközök, és válassza az AD FS felügyeleti).
6. Hello műveletek ablaktáblán kattintson **összevonási szolgáltatás tulajdonságainak szerkesztése**.
7. A hello összevonási szolgáltatás tulajdonságai párbeszédpanelen kattintson a hello **események** fülre.
8. Jelölje be hello **sikernaplók és a hibanaplók** jelölőnégyzeteket, majd **OK**.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2016"></a>az AD FS a Windows Server 2016 naplózásának tooenable
1. Nyissa meg **helyi biztonsági házirend** megnyitásával **Kiszolgálókezelő** hello kezdőképernyőn, vagy a Kiszolgálókezelő hello hello asztali tálcán, majd kattintson a **eszközök/helyi biztonsági házirend**.
2. Keresse meg a toohello **biztonsági beállítások\Helyi házirendek\Felhasználói jogosultságok kiosztása** mappát, majd kattintson rá duplán **biztonsági naplózás létrehozása**.
3. A hello **helyi biztonsági beállítások** lapján ellenőrizze, hogy a hello AD FS szolgáltatásfiók szerepel-e. Ha nincs, kattintson a **felhasználó vagy csoport hozzáadása** hello AD FS szolgáltatás fiók toohello listájához hozzáadni, és kattintson a **OK**.
4. tooenable naplózás esetén nyisson meg egy parancssort emelt szintű jogosultságokkal, és futtassa a következő parancs hello:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Bezárás **helyi biztonsági házirend**, majd nyissa meg a hello **AD FS felügyeleti** beépülő modul (a Kiszolgálókezelőben kattintson az eszközök, és válassza az AD FS felügyeleti).
6. Hello műveletek ablaktáblán kattintson **összevonási szolgáltatás tulajdonságainak szerkesztése**.
7. A hello összevonási szolgáltatás tulajdonságai párbeszédpanelen kattintson a hello **események** fülre.
8. Jelölje be hello **sikernaplók és a hibanaplók** jelölőnégyzeteket, majd **OK**. Ez alapértelmezés szerint ennek engedélyezett.
9. Nyisson meg egy PowerShell-ablakot, és futtassa a következő parancs hello: ```Set-AdfsProperties -AuditLevel Verbose```.

Vegye figyelembe, hogy alapértelmezés szerint az „alapszintű” naplózási szint van engedélyezve. További információk a hello [AD FS naplózása a Windows Server 2016 fejlesztés](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/auditing-enhancements-to-ad-fs-in-windows-server-2016)


#### <a name="toolocate-hello-ad-fs-audit-logs"></a>toolocate hello AD FS-naplók
1. Nyissa meg az **Eseménynaplót**.
2. Nyissa meg tooWindows naplókat, és válassza ki **biztonsági**.
3. Kattintson a jobb oldali hello **aktuális naplók szűrése**.
4. Az Eseményforrás alatt válassza az **AD FS-naplózás** elemet.

![AD FS-naplók](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [!WARNING]
> Csoportházirenddel letiltható az AD FS-naplózás. Ha az AD FS-naplózás le van tiltva, a bejelentkezési tevékenységek használatelemzései nem érhetők el. Győződjön meg róla, hogy nem rendelkezik olyan csoportházirenddel, amely letiltja az AD FS-naplózást.>
>


## <a name="installing-hello-azure-ad-connect-health-agent-for-sync"></a>Hello Azure AD Connect Health ügynök telepítése szinkronizáláshoz
hello Azure AD Connect Health szinkronizálási ügynöke automatikusan települ az Azure AD Connect legújabb buildjével hello. toouse az Azure AD Connect szinkronizálási szolgáltatás, toodownload hello legújabb verzióját az Azure AD Connect kell, és telepítse azt. Letöltheti a legújabb verzió hello [Itt](http://www.microsoft.com/download/details.aspx?id=47594).

tooverify hello ügynök telepítve van, keresse meg a következő szolgáltatások hello kiszolgálón hello. Ha végrehajtotta hello konfigurációs, akkor már futnia kell. Ellenkező esetben le vannak állítva amíg hello konfigurálása nem fejeződött be.

* Azure AD Connect Health Sync Insights szolgáltatás
* Azure AD Connect Health Sync Monitoring szolgáltatás

![Az Azure AD Connect Health for Sync ellenőrzése](./media/active-directory-aadconnect-health-sync/services.png)

> [!NOTE]
> Ne feledje, hogy az Azure AD Connect Health használatához az Azure AD Premium megléte szükséges. Ha még nem rendelkezik Azure AD Premium, nem toocomplete hello konfiguráció hello Azure-portálon is. További információkért lásd: hello [követelmények lapon](active-directory-aadconnect-health-agent-install.md#requirements).
>
>

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Az Azure AD Connect Health for Sync manuális regisztrációja
Ha hello Azure AD Connect Health for Sync-ügynök regisztrációja meghiúsul az Azure AD Connect sikeres telepítését követően, a következő PowerShell-paranccsal toomanually hello ügynök regisztrálása hello is használhatja.

> [!IMPORTANT]
> Csak a PowerShell-paranccsal van szükség, ha hello ügynök regisztrálása meghiúsul az Azure AD Connect telepítése után.
>
>

hello parancs az alábbi PowerShell szükséges, csak ha hello állapotügynök regisztrálása meghiúsul sikeres telepítése és konfigurálása az Azure AD Connect után is. hello Azure AD Connect Health szolgáltatások után hello-ügynök sikeresen regisztrált indul.

Hello Azure AD Connect Health-ügynök a szinkronizálás hello a következő PowerShell-parancs használatával manuálisan regisztrálhatja:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

hello parancs a következő paramétereket fogadja:

* AttributeFiltering: $true (alapértelmezett) – Ha az Azure AD Connect nem szinkronizálja hello alapértelmezett attribútumkészletet és testre szabott toouse egy szűrt attribútum beállítása. $false a többi esetben.
* StagingMode: $false (alapértelmezett) – Ha hello Azure AD Connect-kiszolgáló nem átmeneti módban, $true, ha hello kiszolgáló konfigurált toobe átmeneti módban.

Amikor a rendszer használjon hello ugyanazon globális rendszergazdai fiókkal (például admin@domain.onmicrosoft.com), amely az Azure AD Connect konfigurálásához használt.

## <a name="installing-hello-azure-ad-connect-health-agent-for-ad-ds"></a>Hello Azure AD Connect Health-ügynök az Active Directory tartományi szolgáltatások telepítése
toostart hello az ügynök telepítése, kattintson duplán a letöltött hello .exe fájlt. A hello első képernyőn kattintson a telepítés gombra.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Ha hello telepítés befejeződött, kattintson a konfigurálása.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Ekkor megnyílik egy parancssor, utána egy PowerShell parancsmag, amely végrehajtja a Register-AzureADConnectHealthADDSAgent parancsot. Ha tooAzure, a kért toosign habozzon, jelentkezzen be.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

A bejelentkezést követően a PowerShell továbblép. Miután befejeződött, bezárhatja a Powershellt, és hello konfigurálása nem fejeződött be.

Ezen a ponton hello szolgáltatások automatikusan engedélyezi a hello ügynök toomonitor kell indítani, és adatokat gyűjt. Ha nem teljesítette az összes hello Előfeltételek az hello korábbi szakaszokban ismertetett módon, figyelmeztetések hello PowerShell ablakban jelennek meg. Lehet, hogy toocomplete hello [követelmények](active-directory-aadconnect-health-agent-install.md#requirements) hello ügynök telepítése előtt. a következő képernyőkép hello ezeket a hibákat példája.

![Az Azure AD Connect Health for AD DS ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

tooverify hello ügynök telepítve van, keresse meg a következő hello tartományvezérlő hello.

* Azure AD Connect Health AD DS Insights szolgáltatás
* Azure AD Connect Health AD DS Monitoring szolgáltatás

Ha végrehajtotta hello konfigurációs, ezek a szolgáltatások már kell futtatnia. Ellenkező esetben le vannak állítva amíg hello konfigurálása nem fejeződött be.

![Az Azure AD Connect Health ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)


### <a name="agent-registration-using-powershell"></a>Ügynök regisztrációja a PowerShell használatával
Miután telepítette a hello megfelelő ügynök setup.exe, hello ügynök regisztrációs lépésében a következő PowerShell-parancsok attól függően, hogy hello szerepkör hello segítségével végezheti el. Nyisson meg egy PowerShell-ablakot, és hajtsa végre a megfelelő parancs hello:

```
    Register-AzureADConnectHealthADFSAgent
    Register-AzureADConnectHealthADDSAgent
    Register-AzureADConnectHealthSyncAgent

```

Ezek a parancsok "Hitelesítő" mint egy paraméterrel toocomplete hello regisztrációs fogadja el a nem interaktív módon, vagy egy kiszolgáló-Core számítógépen.
* hitelesítő adatok hello paraméterként átadott PowerShell változóként rögzíthetők.
* Minden Azure AD Identity, amelynek hozzáférési tooregister hello ügynökök, és nincs engedélyezve van az MFA biztosíthat.
* Alapértelmezés szerint a globális rendszergazdák hozzáférhetnek tooperform ügynök regisztrációját. Engedélyezheti továbbá más kiemelt jogosultságú identitások tooperform kevésbé ezt a lépést. További tudnivalók a [szerepköralapú hozzáférés-vezérlésről](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control).

```
    $cred = Get-Credential
    Register-AzureADConnectHealthADFSAgent -Credential $cred

```

## <a name="configure-azure-ad-connect-health-agents-toouse-http-proxy"></a>Az Azure AD Connect Health-ügynököket toouse HTTP-Proxy konfigurálása
Az Azure AD Connect Health-ügynököket toowork egy HTTP-Proxy konfigurálható.

> [!NOTE]
> * "Netsh WinHttp set ProxyServerAddress" használata nem támogatott módon hello ügynök System.NET névtérbeli toomake webes kérelmek használ, nem Microsoft Windows HTTP szolgáltatásokkal.
> * hello konfigurált Http Proxy címe használt toopass keresztül titkosított Https üzenetek.
> * A hitelesített proxyk (HTTPBasic használatával) nem támogatottak.
>
>

### <a name="change-health-agent-proxy-configuration"></a>Állapotügynök proxykonfigurációjának módosítása
A következő beállítások tooconfigure az Azure AD Connect Health Agent toouse egy HTTP-Proxy hello rendelkezik.

> [!NOTE]
> Minden Azure AD Connect Health Agent szolgáltatás újra kell indítani, ahhoz, hogy hello proxy beállításainak toobe frissítése. Futtassa a következő parancs hello:<br>
> Restart-Service AdHealth*
>
>

#### <a name="import-existing-proxy-settings"></a>Meglévő proxybeállítások importálása
##### <a name="import-from-internet-explorer"></a>Importálás Internet Explorerből
Internet Explorer HTTP-proxybeállításait importálhatók, hello által használt toobe az Azure AD Connect Health-ügynököket. Minden egyes hello állapotügynököt futtató hello kiszolgálók hajtható végre a következő PowerShell-paranccsal hello:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importálás WinHTTP-ből
A WinHTTP-proxybeállítások importálása, hello által használt toobe az Azure AD Connect Health-ügynököket. Minden egyes hello állapotügynököt futtató hello kiszolgálók hajtható végre a következő PowerShell-paranccsal hello:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Proxycímek manuális megadása
Egyes hello kiszolgálóin fut hello rendszerállapot-ügynöke, a következő hello a következő PowerShell-parancs futtatásával manuálisan proxykiszolgálót is megadhatja:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Példa: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver: 443*

* Az „address” lehet egy, a DNS által feloldható kiszolgálónév vagy egy IPv4-cím
* A „port” elhagyható. Ilyen esetben a 443 lesz az alapértelmezett port.

#### <a name="clear-existing-proxy-configuration"></a>Meglévő proxykonfiguráció törlése
Hello meglévő proxykonfiguráció törlése hello a következő parancs futtatásával:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Meglévő proxybeállítások olvasása
Hello jelenleg konfigurált proxybeállításokat elolvashatják hello a következő parancs futtatásával:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-tooazure-ad-connect-health-service"></a>Tesztelje a kapcsolatot tooAzure AD Connect Health szolgáltatással
Akkor lehet, hogy hibák lépnek fel, amelyek hello Azure AD Connect Health-ügynök toolose kapcsolat hello Azure AD Connect Health szolgáltatással. Ezek hálózati, engedélyekkel kapcsolatos és egyéb különféle hibák lehetnek.

Ha hello ügynök nem toosend toohello az Azure AD Connect Health szolgáltatás több mint két óra, a következő hello portálon riasztás hello jelezte: "Állapotfigyelő szolgáltatás nincs toodate be." Is meggyőződhet arról Ha hello hatással az Azure AD Connect Health agent képes tooupload toohello az Azure AD Connect Health szolgáltatás hello a következő PowerShell-parancs futtatásával:

    Test-AzureADConnectHealthConnectivity -Role ADFS

hello szerepkör paraméter hello a következő értékeket veheti:

* ADFS
* Sync
* ADDS

Hello - ShowResults jelzőjével hello parancs tooview használható részletes naplókat. A következő példa hello használata:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

> [!NOTE]
> toouse hello kapcsolódási eszköz kell első teljes hello ügynök regisztrációját. Ha nem tudja toocomplete hello ügynök regisztrációját, győződjön meg arról, hogy teljesül-e az összes hello [követelmények](active-directory-aadconnect-health-agent-install.md#requirements) az Azure AD Connect Health. A kapcsolódási teszt végrehajtása alapértelmezés szerint megtörténik az ügynök regisztrációja során.
>
>

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Az Azure AD Connect Health műveletei](active-directory-aadconnect-health-operations.md)
* [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md)
* [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md)
* [Az Azure AD Connect Health használata az AD DS szolgáltatással](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)
