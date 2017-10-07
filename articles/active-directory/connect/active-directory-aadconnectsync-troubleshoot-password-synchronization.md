---
title: "és az Azure AD Connect-szinkronizálás jelszó-szinkronizálás aaaTroubleshoot |} Microsoft Docs"
description: "Ez a cikk nyújt információt tootroubleshoot jelszó-szinkronizálási hibák."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 390eafec792cb39251627c14cb754f8bb30035b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a>A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás hibaelhárítása
Ez a témakör a hogyan tootroubleshoot állít ki a jelszó-szinkronizálás lépéseit ismerteti. Jelszavak nem szinkronizál a várt módon, ha azok a felhasználók egy része vagy az összes felhasználó számára. Az Azure Active Directory (Azure AD) telepítési csatlakozás verziója 1.1.524.0, vagy később, már létezik egy diagnosztikai parancsmag használható tootroubleshoot jelszó-szinkronizálás problémákat:

* Ha a probléma lehet ahol nincs jelszavak szinkronizálódnak, tekintse meg a toohello [nem jelszavai szinkronizálva vannak: hello diagnosztikai parancsmaggal hibaelhárításához](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) szakasz.

* Ha az egyes objektumok problémát, tekintse meg a toohello [több objektum van nem jelszó-szinkronizálás: hello diagnosztikai parancsmaggal hibaelhárításához](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) szakasz.

Az Azure AD Connect telepítési korábbi verzióival:

* Ha a probléma lehet ahol nincs jelszavak szinkronizálódnak, tekintse meg a toohello [nem jelszavai szinkronizálva vannak: hibaelhárítási manuális](#no-passwords-are-synchronized-manual-troubleshooting-steps) szakasz.

* Ha az egyes objektumok problémát, tekintse meg a toohello [több objektum van nem jelszó-szinkronizálás: hibaelhárítási manuális](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) szakasz.

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-hello-diagnostic-cmdlet"></a>Nincs jelszavai szinkronizálva vannak: hello diagnosztikai parancsmaggal hibaelhárításához
Használhatja a hello `Invoke-ADSyncDiagnostics` parancsmag toofigure, miért nem jelszavak szinkronizálódnak.

> [!NOTE]
> Hello `Invoke-ADSyncDiagnostics` parancsmag verziójához rendelkezésre álló csak az Azure AD Connect 1.1.524.0 vagy újabb verzió.

### <a name="run-hello-diagnostics-cmdlet"></a>Hello diagnosztika parancsmag futtatása
ahol nincs jelszavai szinkronizálva vannak az tootroubleshoot problémák:

1. Nyisson meg egy új Windows PowerShell-munkamenetet az Azure AD Connect kiszolgálón hello **Futtatás rendszergazdaként** lehetőséget.

2. Futtatás `Set-ExecutionPolicy RemoteSigned` vagy `Set-ExecutionPolicy Unrestricted`.

3. Futtassa az `Import-Module ADSyncDiagnostics` parancsot.

4. Futtassa az `Invoke-ADSyncDiagnostics -PasswordSync` parancsot.

### <a name="understand-hello-results-of-hello-cmdlet"></a>Hello eredmények hello parancsmag ismertetése
hello diagnosztikai parancsmag a következő ellenőrzések hello hajtja végre:

* Ellenőrzi, hogy hello jelszó-szinkronizálási szolgáltatás engedélyezve van az Azure AD-bérlő.

* Ellenőrzi, hogy hello Azure AD Connect kiszolgáló nem átmeneti módban van.

* Az egyes meglévő helyszíni Active Directory connector (amely felel meg a meglévő Active Directory-erdő tooan):

   * Ellenőrzi, hogy hello jelszó-szinkronizálás szolgáltatás engedélyezve van.
   
   * Jelszó szinkronizálási szívverés események hello keres Windows-alkalmazás eseményt naplózza.

   * Minden egyes hello a helyszíni Active Directory-összekötőt az Active Directory-tartomány:

      * Ellenőrzi, hogy hello tartománya hello Azure AD Connect-kiszolgáló elérhető.

      * Ellenőrzi, hogy hello a helyszíni Active Directory-összekötő által használt hello Active Directory tartományi szolgáltatások (AD DS) fiókok hello megfelelő felhasználónév, jelszó és a jelszó-szinkronizáláshoz szükséges engedélyekkel rendelkezik-e.

a következő diagram hello hello parancsmag egy egyetlen tartományból, a helyszíni Active Directory-topológia hello eredményét mutatja be:

![Diagnosztikai kimenetet a jelszó-szinkronizáláshoz](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

Ez a szakasz többi hello ismerteti adott hello parancsmag által visszaadott eredmények és a kapcsolódó problémákat.

#### <a name="password-synchronization-feature-isnt-enabled"></a>Jelszó-szinkronizálási szolgáltatás nincs engedélyezve
Ha még nem engedélyezte a jelszó-szinkronizálás hello Azure AD Connect varázsló segítségével, a következő hiba hello ad vissza:

![A jelszó-szinkronizálás nincs engedélyezve](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Az Azure AD Connect-kiszolgáló átmeneti módban van
Ha hello Azure AD Connect-kiszolgáló átmeneti módban, a jelszó-szinkronizálás ideiglenesen le van tiltva, és hello következő hibaüzenet:

![Az Azure AD Connect-kiszolgáló átmeneti módban van](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a>Jelszó szinkronizálási szívverés események
Minden egyes a helyszíni Active Directory-összekötőt a saját jelszó-szinkronizálás csatorna rendelkezik. Ha nincs minden szinkronizált jelszó módosítások toobe hello jelszó-szinkronizálás csatorna jön létre, a szívverés (eseményazonosító 654) jön létre 30 percenként az alkalmazások eseménynaplójában keresse meg a Windows hello. Az egyes a helyszíni Active Directory-összekötő hello parancsmag rákeres a hello kapcsolódó szívverés eseményekre vonatkozóan elmúlt három óra. Ha nincs szívverés esemény található, hello következő hibaüzenet:

![Nincs jelszó-szinkronizálás szív járulhatnak esemény](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS fiók nem rendelkezik megfelelő engedélyekkel
Ha az Active Directory tartományi szolgáltatások hello hello által használt fiók a helyszíni Active Directory connector toosynchronize jelszókivonatait nem rendelkezik megfelelő engedélyekkel hello hello következő hibaüzenet:

![Helytelen hitelesítő adatok](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Active Directory tartományi szolgáltatások fiók helytelen felhasználónév vagy jelszó
Ha hello Active Directory tartományi Szolgáltatásokban a fiókot használják hello a helyszíni Active Directory connector toosynchronize jelszókivonatait egy helytelen felhasználónév vagy jelszó, a hello következő hibaüzenet:

![Helytelen hitelesítő adatok](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-hello-diagnostic-cmdlet"></a>Több objektum van nem jelszó-szinkronizálás: hello diagnosztikai parancsmaggal hibaelhárításához
Használhatja a hello `Invoke-ADSyncDiagnostics` parancsmag toodetermine miért egy objektum nem szinkronizál jelszavakat.

> [!NOTE]
> Hello `Invoke-ADSyncDiagnostics` parancsmag verziójához rendelkezésre álló csak az Azure AD Connect 1.1.524.0 vagy újabb verzió.

### <a name="run-hello-diagnostics-cmdlet"></a>Hello diagnosztika parancsmag futtatása
ahol nincs jelszavai szinkronizálva vannak az tootroubleshoot problémák:

1. Nyisson meg egy új Windows PowerShell-munkamenetet az Azure AD Connect kiszolgálón hello **Futtatás rendszergazdaként** lehetőséget.

2. Futtatás `Set-ExecutionPolicy RemoteSigned` vagy `Set-ExecutionPolicy Unrestricted`.

3. Futtassa az `Import-Module ADSyncDiagnostics` parancsot.

4. Futtassa a következő parancsmag hello:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   Példa:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-hello-results-of-hello-cmdlet"></a>Hello eredmények hello parancsmag ismertetése
hello diagnosztikai parancsmag a következő ellenőrzések hello hajtja végre:

* Megvizsgálja a hello Active Directory kapcsolódási térbe, Metaverse és az Azure Active Directory-objektum hello hello állapotának AD kapcsolódási térbe.

* Ellenőrzi, hogy vannak-e a jelszó-szinkronizálás szinkronizálási szabályait engedélyezve van, és toohello Active Directory-objektumot alkalmazza.

* Próbálja meg hello utolsó kísérlet toosynchronize hello jelszó hello objektum tooretrieve és megjelenítési hello eredményeit.

a következő diagram hello hello parancsmag hello eredményét mutatja be, egyetlen objektumhoz jelszó-szinkronizálás hibaelhárítása során:

![A jelszó-szinkronizáláshoz - egyetlen objektumhoz diagnosztikai kimenetet](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

Ez a szakasz többi hello ismerteti adott hello parancsmag által visszaadott eredmények és a kapcsolódó problémákat.

#### <a name="hello-active-directory-object-isnt-exported-tooazure-ad"></a>hello Active Directory-objektum nem AD exportált tooAzure
A jelszó-szinkronizálás a helyszíni Active Directory-fiók sikertelen lesz, mivel nincs hello Azure AD-bérlő a megfelelő objektum. a visszaadott hiba a következő hello:

![Az Azure AD-objektum hiányzik.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Felhasználó rendelkezik-e egy ideiglenes jelszót
Jelenleg az Azure AD Connect nem támogatja ideiglenes jelszó-szinkronizálás Azure AD-val. A jelszó akkor tekinthető toobe ideiglenes Ha hello **jelszó módosítása a következő bejelentkezéskor** hello a helyszíni Active Directory-felhasználó a beállítás. a visszaadott hiba a következő hello:

![Ideiglenes jelszót ne exportálja.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-toosynchronize-password-arent-available"></a>Utolsó kísérlet toosynchronize jelszó eredményei nem érhetők el
Alapértelmezés szerint az Azure AD Connect szinkronizálási bejelentkezési kísérletek hét napja hello eredményeit tárolja. Ha nincsenek kiválasztott hello Active Directory-objektum nem járt eredménnyel, figyelmeztetés a következő hello adja vissza:

![Egyetlen objektum – a Nincs szinkronizálás az előző diagnosztikai kimenete](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Nincs jelszavai szinkronizálva vannak: manuális hibaelhárítási lépések
Kövesse a lépéseket toodetermine, miért nem jelszavai szinkronizálva vannak az:

1. A Connect-kiszolgálója hello [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode)? Átmeneti módban nem szinkronizálja a jelszavakat.

2. Hello hello parancsprogrammal [hello jelszó-szinkronizálási beállítások állapotának lekérése](#get-the-status-of-password-sync-settings) szakasz. Ez lehetővé teszi az hello jelszó-szinkronizálás konfigurációs áttekintését.  

    ![PowerShell parancsfájl kimenete, jelszó-szinkronizálási beállítások](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. Ha hello szolgáltatás nincs engedélyezve az Azure ad-ben, vagy ha hello szinkronizálási csatorna állapota nincs engedélyezve, futtassa a hello Connect telepítővarázslóját. Válassza ki **testre szabhatja a szinkronizálási beállítások**, és törölje a jelszó-szinkronizálást. Ez a változás ideiglenesen letiltja hello szolgáltatást. Majd futtassa újból hello varázslót, és engedélyezze újra a jelszó-szinkronizálást. Hello parancsprogrammal újra, hogy a konfigurációs hello tooverify helyességéről.

4. Keressen hello eseménynaplóban a hibákat. Keresse meg a következő események, amelyek azt jelentené, hogy egy probléma hello:
    * Forrás: "A címtár-szinkronizálás" azonosító: 0, 611, 652, ha ezek az események 655 kapcsolat hibát. Eseménynapló-üzenet hello probléma esetében erdő információkat tartalmaz. További információkért lásd: [csatlakozási probléma](#connectivity problem).

5. Ha nem szívverés, vagy ha nincs más, futtassa [indul el, a teljes szinkronizálás az összes jelszavak](#trigger-a-full-sync-of-all-passwords). Hello parancsfájl csak egyszer futtatnia.

6. Lásd: hello [egy olyan objektum, amely nem szinkronizál jelszavak hibaelhárítása](#one-object-is-not-synchronizing-passwords) szakasz.

### <a name="connectivity-problems"></a>Kapcsolódási problémák

Az Azure AD-kapcsolat van?

Hello fiók rendelkezik szükséges engedélyek tooread hello jelszókivonatait minden tartományban? Ha a csatlakozás a gyorsbeállítások használatával telepítette, hello engedélyek már kell megfelelő. 

Ha egyéni telepítési, hello engedélyek beállítása manuálisan hello következő tevékenységek végrehajtásával:
    
1. hello Active Directory-összekötő kezdő által használt toofind hello fiók **Synchronization Service Managert**. 
 
2. Nyissa meg túl**összekötők**, majd keresse meg a hello a helyszíni Active Directory-erdő hibaelhárítást. 
 
3. Válassza ki a hello összekötő, és kattintson a **tulajdonságok**. 
 
4. Nyissa meg túl**tooActive Directory-erdő csatlakozás**.  
    
    ![Active Directory-összekötő által használt fiók](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    Megjegyzés: hello felhasználónév és a hello tartomány hello fiók.
    
5. Start **Active Directory – felhasználók és számítógépek**, és ellenőrizze, hogy korábban található hello fiók jogosult hello kövesse állítsa be megfelelően az erdőben lévő összes tartományban hello gyökérmappájában:
    * Változások replikálása
    * Replikálása Directory összes módosítása

6. Hello Azure AD Connect elérhető-e tartományvezérlők vannak? Ha hello Connect-kiszolgáló nem tud csatlakozni a tooall tartományvezérlők, konfigurálja **csak az elsődleges tartományvezérlőt használja**.  
    
    ![Az Active Directory-összekötő által használt tartományvezérlő](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. Lépjen vissza túl**Synchronization Service Managert** és **címtárpartíciók konfigurálása**. 
 
8. Válassza ki a tartományt a **válassza ki az a címtárpartíciókat**, jelölje be hello **csak használja az elsődleges tartományvezérlő** jelölőnégyzetet, majd kattintson a **konfigurálása**. 

9. Hello listában adja meg a jelszó-szinkronizálás használata ajánlott a Connect hello tartományvezérlők. hello ugyanazt a listát történő importálásának és exportálásának is szolgál. Hajtsa végre ezeket a lépéseket minden tartományban.

10. Ha hello parancsfájl bemutatja, hogy van-e nem szívverés, futtassa az hello parancsfájl [indul el, a teljes szinkronizálás az összes jelszavak](#trigger-a-full-sync-of-all-passwords).

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Több objektum van nem jelszó-szinkronizálás: manuális hibaelhárítási lépések
Jelszó-szinkronizálás problémák könnyen elháríthatja az hello egy objektumot állapotának megtekintésével.

1. A **Active Directory – felhasználók és számítógépek**, hello felhasználói keressen, és ellenőrizze, hogy hello **kell változtatni a jelszót a következő bejelentkezéskor** jelölőnégyzet nincs bejelölve.  

    ![Active Directory hatékony jelszavak](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    Ha hello jelölőnégyzet be van jelölve, kérje meg a hello felhasználói toosign és hello jelszó módosítása. Ideiglenes jelszavak nincsenek szinkronizálva az Azure ad-val.

2. Ha hello jelszó helyes-e az Active Directoryban, hajtsa végre a szinkronizálási motor hello hello felhasználói. A helyszíni Active Directory tooAzure AD a következő hello felhasználó láthatja, hogy van-e a leíró hiba hello objektumon.

    a. Indítsa el a hello [Synchronization Service Managert](active-directory-aadconnectsync-service-manager-ui.md).

    b. Kattintson a **összekötők**.

    c. Jelölje be hello **Active Directory-összekötő** ahol hello felhasználó tartozik.

    d. Válassza ki **Összekötőtér keresési**.

    e. A hello **hatókör** mezőben válassza **megkülönböztető név vagy a horgony**, majd adja meg a teljes DN hello hibaelhárítási hello felhasználó.

    ![A kapcsolódási térbe megkülönböztető névvel rendelkező felhasználó keresése](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    f. Keresse meg a hello felhasználói keres, és kattintson a **tulajdonságok** toosee összes hello attribútumok. Ha hello felhasználó nincs hello keresési eredményt, ellenőrizze a [szűrési szabályok](active-directory-aadconnectsync-configure-filtering.md) és győződjön meg arról, hogy [alkalmaz és a módosítások ellenőrzéséhez](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) hello felhasználói tooappear a csatlakozás a.

    g. toosee hello jelszó szinkronizálási részletek hello hello objektumának múlt héten kattintson **napló**.  

    ![Napló részletes adatai](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    Ha hello objektum napló üres, az Azure AD Connect lett nem tooread hello Jelszókivonat az Active Directoryból. Folytassa a [kapcsolódási hibák](#connectivity-errors). Ha látja, mint bármely más érték **sikeres**, tekintse meg a táblázat toohello [jelszó-szinkronizálás napló](#password-sync-log).

    h. Jelölje be hello **Leszármaztatás** lapot, és győződjön meg arról, hogy legalább egy szinkronizálási szabály a hello **PasswordSync** oszlop **igaz**. Hello alapértelmezés szerint hello neve hello szinkronizálási szabály: **a az AD - felhasználó AccountEnabled**.  

    ![Leszármaztatás felhasználókkal kapcsolatos információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    i. Kattintson a **Metaverzum-objektum tulajdonságai** toodisplay felhasználói attribútumok listáját.  

    ![Metaverzum-információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    Győződjön meg arról, hogy nincs **cloudFiltered** attribútumnak jelen. Győződjön meg arról, hogy hello tartomány tulajdonságai (domainFQDN és domainNetBios) hello várt értékek.

    j. Kattintson a hello **összekötők** fülre. Győződjön meg arról, hogy megjelenik-e összekötők tooboth a helyszíni Active Directory és az Azure AD.

    ![Metaverzum-információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    k. Az Azure AD jelképező válassza hello sorban kattintson **tulajdonságok**, majd kattintson a hello **Leszármaztatás** külön-külön hello összekötő terület objektum meg kell adni a hello egy kimenő forgalomra vonatkozó szabály **PasswordSync** oszlopkészlet túl**igaz**. Hello alapértelmezés szerint hello neve hello szinkronizálási szabály: **tooAAD - felhasználók csatlakozásra kimenő**.  

    ![Összekötő Összekötőtér-objektum tulajdonságai párbeszédpanel](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Jelszó-szinkronizálás napló
hello állapot oszlop hello a következő értékeket veheti fel:

| status | Leírás |
| --- | --- |
| Sikeres |Jelszó szinkronizálása sikerült. |
| FilteredByTarget |Jelszó beállítása túl**kell változtatni a jelszót a következő bejelentkezéskor**. Jelszó nem lett szinkronizálva. |
| NoTargetConnection |Nincs objektum hello metaverse vagy a kapcsolódási térbe hello Azure AD. |
| SourceConnectorNotPresent |Nem található a hello a helyszíni Active Directory kapcsolódási térbe objektum. |
| TargetNotExportedToDirectory |hello Azure AD kapcsolódási térbe hello objektum még nem lett exportálva. |
| MigratedCheckDetailsForMoreInfo |Naplóbejegyzés build 1.0.9125.0 előtt készült, és megjelenik-e a korábbi állapotába. |
| Hiba |Szolgáltatás ismeretlen hibával tért vissza. |
| Ismeretlen |Hiba történt a jelszó-kivonatok köteg tooprocess tett kísérlet során.  |
| MissingAttribute |Adott attribútumok (például a Kerberos-kivonat) Azure AD tartományi szolgáltatások által igényelt nem érhetők el. |
| RetryRequestedByTarget |Adott attribútumok (például a Kerberos-kivonat) Azure AD tartományi szolgáltatások által igényelt korábban nem érhető el. Egy kísérlet tooresynchronize hello felhasználói Jelszókivonat történik. |

## <a name="scripts-toohelp-troubleshooting"></a>Parancsfájlok toohelp hibaelhárítása

### <a name="get-hello-status-of-password-sync-settings"></a>A jelszó-szinkronizálási beállítások hello állapotának beolvasása
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update hello script toouse hello appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>A teljes szinkronizálás az összes jelszavak eseményindító
> [!NOTE]
> Ez a parancsfájl csak egyszer futtatnia. Toorun azt egynél többször van szükség, ha valami mással hello probléma. tootroubleshoot hello probléma, forduljon a Microsoft támogatási szolgálatához.

A következő parancsfájl hello segítségével a teljes szinkronizálás az összes jelszavak indíthat el:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Következő lépések
* [A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás megvalósítása](active-directory-aadconnectsync-implement-password-synchronization.md)
* [Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
