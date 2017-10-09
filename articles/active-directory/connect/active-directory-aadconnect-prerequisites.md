---
title: "Az Azure AD Connect: Előfeltételek és hardver |} Microsoft Docs"
description: "Ez a témakör ismerteti a hello szükséges előfeltételek és az Azure AD Connect hello hardverkövetelményei"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2ec8b5da9440c92e8f9d6702d5e5e20de952b588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-for-azure-ad-connect"></a>Előfeltételek az Azure AD Connect
Ez a témakör ismerteti a hello szükséges előfeltételek és az Azure AD Connect hello hardverkövetelményeinek.

## <a name="before-you-install-azure-ad-connect"></a>Az Azure AD Connect telepítése előtt
Az Azure AD Connect telepítése előtt van néhány dolog, amelyekre szüksége van.

### <a name="azure-ad"></a>Azure AD
* Azure-előfizetés vagy egy [Azure próba-előfizetés](https://azure.microsoft.com/pricing/free-trial/). Ez az előfizetés csak akkor szükséges hello Azure-portál eléréséhez, és nem az Azure AD Connect használatával. Ha PowerShell vagy az Office 365 használ, akkor nincs szüksége egy Azure-előfizetés toouse az Azure AD Connect. Ha az Office 365-licencre van, majd használhatja hello Office 365 portálra. Egy fizetős Office 365-licenccel rendelkező is megkapható hello az Azure-portálon hello Office 365 portálról.
  * Is használhatja a hello [Azure-portálon](https://portal.azure.com). Ezen a portálon nem követeli meg az Azure AD-licencre.
* [Hello tartományok hozzáadásának és hitelesítésének](../active-directory-domains-add-azure-portal.md) toouse tervezi az Azure ad-ben. Például ha toouse contoso.com tervezze meg a felhasználók számára, akkor győződjön meg arról, hogy a tartomány ellenőrzése után, és nem csak hello contoso.onmicrosoft.com alapértelmezett tartomány használata.
* Az Azure AD-bérlő lehetővé teszi az alapértelmezett 50k objektumok. Ellenőrizze a tartomány, hello határértéke megnövekedett too300k objektumok. Ha még több objektumot az Azure ad-ben, akkor szüksége tooopen egy támogatási eset toohave hello maximális nőtt még tovább. Ha több mint 500 KB-os objektumokat, majd licenccel kell rendelkeznie, mint például az Office 365, Azure AD alapvető, Azure AD Premium számára, vagy a nagyvállalati mobilitási és biztonsági.

### <a name="prepare-your-on-premises-data"></a>A helyszíni adatok előkészítése
* Használjon [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) tooidentify hibák például az ismétlődések és a címtárban tooAzure AD és az Office 365 szinkronizálása előtt formázási problémák.
* Felülvizsgálati [engedélyezheti az Azure ad-ben nem kötelező a szinkronizálási funkciók](active-directory-aadconnectsyncservice-features.md) kiértékelheti, engedélyeznie kell a szolgáltatásokat.

### <a name="on-premises-active-directory"></a>Helyszíni Active Directory
* hello AD séma verziója és az erdő működési szintje Windows Server 2003 vagy újabb kell lennie. hello tartományvezérlők is valamely kiadását futtatja, mindaddig, amíg hello séma- és erdő szintű-e.
* Ha azt tervezi, hogy toouse hello szolgáltatás **jelszóvisszaírás**, akkor hello tartományvezérlők kell lennie a Windows Server 2008 (legújabb szervizcsomaggal) vagy újabb. Ha a tartományvezérlők 2008 (R2 előtti), akkor is telepítenie kell [gyorsjavítás KB2386717](http://support.microsoft.com/kb/2386717).
* az Azure AD által használt hello tartományvezérlőnek írhatónak kell lennie. Az **nem támogatott** egy írásvédett tartományvezérlő (csak olvasható tartományvezérlő) és az Azure AD Connect nem követi minden toouse átirányítások írni.
* Az **nem támogatott** toouse helyszíni erdők/tartományok által (egyetlen címke tartományok) használatával.
* Az **nem támogatott** toouse helyszíni erdők/tartományok "pontozott" (nevében pont szerepel ".") NetBios-nevét.
* Túl ajánlott[hello Active Directory Lomtár engedélyezése](active-directory-aadconnectsync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Az Azure AD Connect-kiszolgáló
* Az Azure AD Connect nem telepíthető Small Business Server vagy Windows Server Essentials. hello kiszolgáló Windows Server standard vagy jobb kell használnia.
* hello Azure AD Connect-kiszolgáló telepítve teljes grafikus felhasználói Felülettel kell rendelkeznie. Az **nem támogatott** tooinstall server core-on.
* Az Azure AD Connect a Windows Server 2008 vagy újabb rendszerre telepíthető. Ez a kiszolgáló lehet egy tartományvezérlő vagy tagkiszolgáló gyorsbeállítások használata esetén. Ha egyéni beállításokat használja, hello kiszolgáló is lehet önálló, és nem rendelkezik toobe illesztett tooa tartomány.
* Ha az Azure AD Connect telepítése a Windows Server 2008 vagy Windows Server 2008 R2, végezze el, hogy tooapply hello legújabb gyorsjavításokat a Windows Update webhelyről. hello telepítés nincs már nem javított kiszolgálóról tud toostart.
* Ha azt tervezi, hogy toouse hello szolgáltatás **jelszó-szinkronizálás**, akkor hello Azure AD Connect-kiszolgáló kell lennie a Windows Server 2008 R2 SP1 vagy újabb.
* Ha azt tervezi, hogy toouse egy **csoport felügyelt szolgáltatásfiók**, akkor hello Azure AD Connect-kiszolgáló kell lennie a Windows Server 2012 vagy újabb.
* rendelkeznie kell hello Azure AD Connect-kiszolgáló [.NET-keretrendszer 4.5.1](#component-prerequisites) vagy újabb és [Microsoft PowerShell 3.0](#component-prerequisites) vagy újabb verziója.
* hello Azure AD Connect-kiszolgáló nem lehet írjanak elő csoportházirend PowerShell engedélyezve van.
* Active Directory összevonási szolgáltatások telepítése történik, ha a hello kiszolgálók, ahol az AD FS és a webalkalmazás-Proxy telepítése történik kell-e a Windows Server 2012 R2 vagy újabb. [Rendszerfelügyeleti webszolgáltatások](#windows-remote-management) ezek a kiszolgálók távoli telepítéséhez engedélyezve kell lennie.
* Ha az Active Directory összevonási szolgáltatások telepítése történik, akkor [SSL-tanúsítványok](#ssl-certificate-requirements).
* Ha az Active Directory összevonási szolgáltatások telepítése, akkor meg kell tooconfigure [névfeloldás](#name-resolution-for-federation-servers).
* Ha a globális rendszergazdák többtényezős hitelesítés engedélyezve van, majd hello URL-cím **https://secure.aadcdn.microsoftonline-p.com** hello megbízható webhelyek listájának kell lennie. A hely toohello megbízható helyek listájához, MFA-kérdést és az azt kérő nem hozzáadva előtt felszólító tooadd áll. Az Internet Explorer tooadd használhatja azt a megbízható helyek tooyour.

### <a name="sql-server-used-by-azure-ad-connect"></a>Az Azure AD Connect által használt SQL Server
* Az Azure AD Connect egy SQL Server adatbázis toostore identitásadatok igényel. Alapértelmezés szerint telepítve van egy SQL Server 2012 Express LocalDB (az SQL Server Express egy világos verzió). SQL Server Express méretkorlátja 10 GB-os, amely lehetővé teszi toomanage körülbelül 100 000 objektumok. Ha toomanage címtárobjektumok nagyobb mennyiségű van szüksége, akkor toopoint hello telepítési varázsló tooa különböző az SQL Server telepítése.
* Egy külön SQL Server használatakor, majd ezek a követelmények vonatkoznak:
  * Az Azure AD Connect a Microsoft SQL Server az SQL Server 2008 (a legújabb szervizcsomaggal) tooSQL Server 2016 SP1 minden verziója támogatja. A Microsoft Azure SQL-adatbázis **nem támogatott** -adatbázisként.
  * Nem betűérzékeny SQL-rendezést kell használnia. Ezek rendezések azonosítják a \_CI_ neve. Az **nem támogatott** toouse a kis-és nagybetűket rendezést által azonosított \_CS_ neve.
  * SQL-példányonként egy szinkronizálási motor csak akkor is. Az **nem támogatott** tooshare egy SQL-példány FIM vagy MIM Sync, a DirSync vagy az Azure AD Sync.

### <a name="accounts"></a>Fiókok
* Az Azure AD globális rendszergazda fiókkal rendelkező toointegrate kívánja hello Azure AD-bérlő számára. Ennek a fióknak kell lennie egy **iskolai vagy a szervezeti fiók** , és nem lehet egy **Microsoft-fiók**.
* Ha a gyorsbeállítások használata, vagy a frissítésre a Dirsyncről, majd rendelkeznie kell egy vállalati rendszergazdai fiók a helyi Active Directory.
* [Az Active Directory fiókok](active-directory-aadconnect-accounts-permissions.md) használatakor hello egyéni beállítások telepítési útvonalat.

### <a name="connectivity"></a>Kapcsolatok
* hello Azure AD Connect-kiszolgáló intranetes és internetes DNS-feloldás szüksége van. hello DNS-kiszolgálónak képesnek kell lenniük tooresolve tooyour a helyszíni Active Directory és a hello Azure AD-végpont nevét.
* Ha az intraneten lévő tűzfalak vannak, és meg kell tooopen portok hello Azure AD Connect-kiszolgálók és a tartományvezérlők között, majd tekintse meg [az Azure AD Connect portok](active-directory-aadconnect-ports.md) további információt.
* Ha a proxy és a tűzfalon belül URL-címeket is elérhetők, majd hello URL-címek részletes ismertetését lásd: [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) kell megnyitni.
  * Használata a Microsoft Cloud Németországban vagy a Microsoft Azure Government felhő hello hello, majd tekintse meg [az Azure AD Connect szinkronizálási szolgáltatás példányának szempontok](active-directory-aadconnect-instances.md) URL-címek esetén.
* Az Azure AD Connect használatával a TLS 1.0 toocommunicate az Azure AD alapértelmezés szerint ki van. Hello utasításait követve módosíthatja a tooTLS 1.2 [TLS 1.2 engedélyezése az Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
* Ha egy kimenő proxy toohello Internet csatlakozáshoz használ, hello hello beállítása a következő **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** fájlt fel kell venni hello telepítővarázslója és az Azure AD Connect szinkronizálási toobe képes tooconnect toohello internetes és az Azure AD. Ez a szöveg hello aljához hello fájlt meg kell adni. Ebben a kódban &lt;PROXYADRESS&gt; jelöli hello tényleges proxy IP-címét vagy állomásnevét neve.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Ha a proxykiszolgálóhoz hitelesítés, majd hello szükséges [szolgáltatásfiók](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) kell elhelyezni hello tartományhoz, és használjon hello testre szabott beállítások telepítési elérési út toospecify egy [egyéni szolgáltatásfiók](active-directory-aadconnect-get-started-custom.md#install-required-components). Egy másik módosítás toomachine.config is kell. Ez a változás a machine.config rendelkező hello telepítési varázsló és a szinkronizálási motor válaszol tooauthentication kérelmek hello proxykiszolgálót. Az összes telepítési varázslólapok, kivéve a hello **konfigurálása** lap hello bejelentkezett felhasználó hitelesítő adatait használja. A hello **konfigurálása** lap hello végére hello telepítési varázsló hello környezetben bekapcsolva toohello [szolgáltatásfiók](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) Ön által létrehozott. hello machine.config szakasz kell kinéznie.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Az Azure AD Connect címtár-szinkronizálás részeként egy webes kérelem tooAzure AD küld, ha az Azure AD too5 perc toorespond is eltarthat. Esetében gyakori, a kiszolgálók toohave kapcsolat üresjárati időtúllépés proxybeállításait. Ellenőrizze, hogy hello beállítás tooat legalább 6 perc vagy több.

További információkért tekintse meg az MSDN kapcsolatos hello [proxy elem alapértelmezett](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
További információk, problémák adódnak a kapcsolódással, ha: [kapcsolódási problémák megoldásáról](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Egyéb
* Választható lehetőség: A vizsgálati felhasználói fiók tooverify szinkronizálását.

## <a name="component-prerequisites"></a>Összetevő-Előfeltételek
### <a name="powershell-and-net-framework"></a>PowerShell és a .net keretrendszer
Az Azure AD Connect Microsoft PowerShell és a .NET-keretrendszer 4.5.1 függ. Ebben a verzióban vagy a kiszolgálóra telepített egy újabb verziója szükséges. Az hello attól függően, hogy a Windows Server verzió a következő:

* Windows Server 2012 R2 rendszerben
  * Alapértelmezés szerint telepítve van a Microsoft PowerShell. Nincs szükség semmilyen műveletre.
  * .NET-keretrendszer 4.5.1 és a későbbi kibocsátásokban megtörténik a Windows Update szolgáltatáson keresztül érhető el. Ellenőrizze, hogy hello Vezérlőpult hello legújabb frissítések tooWindows kiszolgáló telepítette.
* Windows Server 2008R2 és a Windows Server 2012-ben
  * Microsoft PowerShell legújabb verziójának hello érhető el a **Windows Management Framework 4.0**, elérhető [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET-keretrendszer 4.5.1 és a későbbi kibocsátásokban megtörténik a használhatók [Microsoft Download Center](http://www.microsoft.com/downloads).
* Windows Server 2008
  * PowerShell hello legújabb támogatott verziója érhető el a **Windows Management Framework 3.0**, elérhető [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET-keretrendszer 4.5.1 és a későbbi kibocsátásokban megtörténik a használhatók [Microsoft Download Center](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Az Azure AD Connect engedélyezni a TLS 1.2-es
Az Azure AD Connect használja a TLS 1.0 alapértelmezés szerint hello a Szinkronizáló vezérlő kiszolgálója és az Azure AD között hello kommunikáció titkosítására. Hello-kiszolgálón konfigurálja a .net alkalmazások toouse TLS 1.2 alapértelmezés szerint ez módosíthatja. További információ a TLS 1.2 megtalálhatók [Microsoft biztonsági tanácsadó 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. A TLS 1.2 Windows Server 2008 nem engedélyezhető. Windows Server 2008R2 van szüksége, vagy később. Győződjön meg arról, hogy hello .net 4.5.1 gyorsjavítás az operációs rendszer telepítése, lásd: [Microsoft biztonsági tanácsadó 2960358](https://technet.microsoft.com/security/advisory/2960358). Lehetséges, hogy ez a gyorsjavítás vagy a kiszolgálóra már telepített egy újabb verzióját.
2. Windows Server 2008R2 használja, majd győződjön meg arról, a TLS 1.2 engedélyezve van. A Windows Server 2012 rendszerű kiszolgáló és újabb verzióiban a TLS 1.2 már engedélyezni kell.
   ```
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   ```
3. Minden operációs rendszerek esetén állítsa be ezt a beállításkulcsot, és indítsa újra hello kiszolgálót.
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
   "SchUseStrongCrypto"=dword:00000001
   ```
4. Ha szeretné a TLS 1.2 tooenable hello a Szinkronizáló vezérlő kiszolgálója és egy távoli SQL Server között, akkor győződjön meg arról, hogy rendelkezik hello szükséges verziók telepítve [TLS 1.2-es Microsoft SQL Server támogatása](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Előfeltételek összevonási telepítése és konfigurálása
### <a name="windows-remote-management"></a>Rendszerfelügyeleti webszolgáltatások
Az Azure AD Connect toodeploy Active Directory összevonási szolgáltatások vagy hello webalkalmazás-Proxy használata esetén ezek a követelmények ellenőrzése:

* Ha a célkiszolgáló hello tartományhoz csatlakozik, majd győződjön meg arról, hogy engedélyezve van-e a Windows távoli felügyelete
  * Egy emelt szintű PSH parancs ablakban paranccsal`Enable-PSRemoting –force`
* Ha a hello célként megadott kiszolgáló egy a tartományhoz nem csatlakoztatott WAP gépen, akkor néhány további követelmények
  * Hello célszámítógépen (WAP gép):
    * Hello winrm biztosítása (Rendszerfelügyeleti webszolgáltatások / WS-Management) szolgáltatás fut-e keresztül hello szolgáltatások beépülő modul
    * Egy emelt szintű PSH parancs ablakban paranccsal`Enable-PSRemoting –force`
  * Hello számítógép melyik hello varázsló fut (ha hello célgépen a tartományhoz nem csatlakozó vagy nem megbízható tartomány):
    * Egy emelt szintű PSH parancs ablakban hello parancs használata`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * A Kiszolgálókezelőben:
      * DMZ WAP állomás toomachine készlet hozzáadása (a Kiszolgálókezelő -> kezelés -> kiszolgálók hozzáadása... DNS lapon)
      * A Kiszolgálókezelő minden kiszolgálók lap: kattintson jobb gombbal a WAP-kiszolgáló, és válassza ki a kezelés másként..., hello WAP gép (nem tartományi) helyi hitelesítő adatok megadása
      * toovalidate távoli PSH kapcsolatot, a Kiszolgálókezelő minden kiszolgáló lap hello: kattintson jobb gombbal a WAP-kiszolgáló, és válassza ki a Windows PowerShell. Meg kell nyitnia a távoli PSH munkamenet tooensure távoli PowerShell-munkamenetekben hozhatók létre.

### <a name="ssl-certificate-requirements"></a>SSL-tanúsítványokkal szemben támasztott követelmények
* Erősen ajánlott toouse hello azonos SSL-tanúsítványt az AD FS farm összes csomópontja és az összes webalkalmazás-proxy kiszolgálók között.
* hello tanúsítványnak kell lennie egy X509 tanúsítványt.
* Egy önaláírt tanúsítványt az összevonási kiszolgálóján egy tesztkörnyezetben használhatja. Azonban az éles környezetben, azt javasoljuk, hogy a nyilvános hitelesítésszolgáltatótól hello tanúsítvány beszerzése.
  * Ha nincs nyilvánosan megbízható tanúsítványt használ, győződjön meg arról, hogy hello tanúsítvány egyes webalkalmazás-Proxy kiszolgálókra telepített megbízható mindkét hello helyi kiszolgálón, és az összes összevonási kiszolgálóján
* hello identitás hello tanúsítvány hello összevonási szolgáltatás nevét (például sts.contoso.com) meg kell egyeznie.
  * hello identitás vagy a tulajdonos alternatív nevével (SAN) kiterjesztését típus dNSName vagy, ha nincsenek SAN bejegyzések, hello tulajdonosnév szerepel a köznapi nevet.  
  * Több SAN-bejegyzés használható hello tanúsítvány, feltéve hello összevonási szolgáltatás neve megegyezik az egyik legyen.
  * Ha azt tervezi, munkahelyi csatlakoztatás toouse, egy további SAN hello értékkel kell **enterpriseregistration.** a szervezet például hello egyszerű felhasználónév (UPN) utótag követ **: enterpriseregistration.contoso.com**.
* A CryptoAPI next generation (CNG) kulcsokat és a kulcstároló-szolgáltatók alapján tanúsítványok használata nem támogatott. Ez azt jelenti, hogy a kriptográfiai Szolgáltató (kriptográfiai szolgáltató) és a nem kulcstároló-Szolgáltatóra (kulcstároló-szolgáltató) alapján tanúsítványt kell használnia.
* Helyettesítő tanúsítványokat támogatottak.

### <a name="name-resolution-for-federation-servers"></a>Összevonási kiszolgálók névfeloldása
* Állítsa be a DNS-rekordjainak hello AD FS összevonási szolgáltatás nevét (például sts.contoso.com) (a belső DNS-kiszolgáló) hello intranetes és extranetes hello (nyilvános DNS a tartományregisztráló keresztül). A hello intranetes DNS-rekord, győződjön meg arról, hogy A rekordok, és nem a CNAME-rekordot. Ez elengedhetetlen ahhoz a tartományhoz csatlakozó számítógépeken a megfelelő windows-hitelesítés toowork.
* Egynél több AD FS-kiszolgálón vagy webalkalmazás-Proxy kiszolgáló telepítése, majd győződjön meg arról, hogy a terheléselosztó már konfigurálta, hogy a terheléselosztó hello DNS-rekordjainak hello AD FS összevonási szolgáltatás nevét (például sts.contoso.com) pont toohello.
* Windows integrált hitelesítést toowork böngésző alkalmazások Internet Explorerrel intranet győződjön meg arról, hogy az AD FS összevonási szolgáltatás nevét (például sts.contoso.com) hello fel van véve toohello intranet zóna Internet Explorer. Ez is vezérelhető a csoportházirend és a telepített tooall keresztül a tartományhoz csatlakoztatott számítógépeket.

## <a name="azure-ad-connect-supporting-components"></a>Az Azure AD Connect támogató összetevők
hello összetevők, amelyek az Azure AD Connect telepíti az hello, amelyen telepítve van-e az Azure AD Connect listája látható. A lista létrehozási alapvető Expressz telepítés. Ha úgy dönt, toouse egy másik SQL Server hello telepítse a szinkronizálási szolgáltatások lapon, majd az SQL Express LocalDB nincs telepítve helyileg.

* Azure AD Connect Health
* Microsoft Online Services bejelentkezési segéd (de nem függőségi viszonyban telepítve) informatikai szakemberek számára
* Microsoft SQL Server 2012 parancssori segédeszközök
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 natív ügyfél
* A Microsoft Visual C++ 2013 Újraterjesztési csomag

## <a name="hardware-requirements-for-azure-ad-connect"></a>Az Azure AD Connect hardverkövetelményei
hello az alábbi táblázat hello Azure AD Connect sync számítógépén hello minimális követelményeinek.

| Az Active Directory-objektumok száma | CPU | Memory (Memória) | Merevlemez mérete |
| --- | --- | --- | --- |
| Kevesebb mint 10 000 |1,6 GHz-es |4 GB |70 GB |
| 10,000–50,000 |1,6 GHz-es |4 GB |70 GB |
| 50,000–100,000 |1,6 GHz-es |16 GB |100 GB |
| 100 000 vagy több objektumot az SQL Server teljes verzióját hello pedig szükséges | | | |
| 100,000–300,000 |1,6 GHz-es |32 GB |300 GB |
| 300,000–600,000 |1,6 GHz-es |32 GB |450 GB |
| Legfeljebb 600 000 |1,6 GHz-es |32 GB |500 GB |

AD FS-t futtató számítógépek minimális követelményei hello vagy a webalkalmazás-kiszolgálón hello következő:

* CPU: Két alapvető 1,6 GHz-es vagy újabb
* MEMÓRIA: 2 GB-os vagy újabb
* Azure virtuális gép: A2 konfigurációs vagy újabb

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
