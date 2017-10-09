---
title: "Összekötő aaaPowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure Microsoft Windows PowerShell-összekötő."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 44ff6b1f53283000b72e15f861e0f86c21afe12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-powershell-connector-technical-reference"></a>Technikai útmutató a Windows PowerShell-összekötő
Ez a cikk ismerteti a Windows PowerShell-összekötő hello. hello cikk vonatkozik toohello a következő termékek:

* A Microsoft Identity Manager 2016 (MIM2016)
* A Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Kell használnia a 4.1.3671.0 gyorsjavítás vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2, hello összekötő rendelkezésre áll hello letölthető [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-hello-powershell-connector"></a>Hello PowerShell-összekötő áttekintése
hello PowerShell-összekötő lehetővé teszi toointegrate hello szinkronizálási szolgáltatás által biztosított Windows PowerShell-alapú API-k külső rendszerekkel. hello összekötő hidat között hello bővíthető kapcsolat Hívásalapú kezelőügynök hello képességeket biztosít, 2 (ECMA2) keretrendszer és a Windows PowerShell. Hello ECMA keretrendszer kapcsolatos további információkért lásd: hello [bővíthető kapcsolat 2.2 felügyeleti ügynök hivatkozás](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Előfeltételek
Összekötő hello használata előtt győződjön meg arról hello következő hello szinkronizálási kiszolgálón rendelkezik:

* Microsoft keretrendszer 4.5.2-es vagy újabb verzió
* A Windows PowerShell 2.0-s, 3.0-s vagy 4.0

hello végrehajtási házirend hello szinkronizálási szolgáltatás kiszolgálón konfigurált tooallow hello összekötő toorun Windows PowerShell-parancsfájlok kell lennie. Digitális aláírással hello parancsfájlok hello összekötő fut, kivéve hello végrehajtási házirend konfigurálása a parancs futtatásával:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
toocreate hello szinkronizálási szolgáltatás Windows PowerShell-összekötőhöz, meg kell adnia egy sorozatát hello synchronization szolgáltatás által kért hello lépések végrehajtása Windows PowerShell-parancsfájlok. Attól függően, hogy hello adatforráshoz kapcsolódott tooand hello funkció van szüksége meg kell valósítani hello parancsfájlok függően változik. Ez a szakasz ismerteti az egyes hello parancsfájlok, végrehajtható, és ha azok szükségesek.

hello Windows PowerShell-összekötő tervezett toostore minden hello parancsfájlok hello szinkronizálási szolgáltatás adatbázisa belül. Hello fájlrendszeren tárolt lehetséges toorun parancsfájlokat is, minden parancsprogram közvetlenül a toohello összekötő-konfiguráció egyszerűbb tooinsert hello törzsében.

tooCreate egy PowerShell-összekötő, a **szinkronizálási szolgáltatás** válasszon **kezelőügynök** és **létrehozása**. Jelölje be hello **PowerShell (Microsoft)** összekötő.

![Összekötő létrehozása](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Kapcsolatok
Adja meg a távoli rendszer tooa csatlakozni konfigurációs paramétereket. Ezek az értékek biztonságosan tárolja hello szinkronizálási szolgáltatás által, rendelkezésre álló tooyour Windows PowerShell-parancsfájlok hello összekötő futtatásakor.

![Kapcsolatok](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

A következő kapcsolat paraméterek hello konfigurálhatja:

**Kapcsolatok**

| Paraméter | Alapértelmezett érték | Cél |
| --- | --- | --- |
| Kiszolgáló |<Blank> |Kiszolgáló neve, amely összekötő hello csatlakoznia kell. |
| Tartomány |<Blank> |Hello credential toostore hello összekötő futtatásakor használatra tartományán. |
| Felhasználó |<Blank> |Hello credential toostore hello összekötő futtatásakor használja a felhasználóneve. |
| Jelszó |<Blank> |Hello credential toostore hello összekötő futtatásakor használja a jelszavát. |
| Összekötő-fiók megszemélyesítése |False (Hamis) |Amikor igaz értékű, hello szinkronizálási szolgáltatás fut hello Windows PowerShell-parancsfájlok hello hitelesítőadatok hello környezetében. Ha lehetséges, javasoljuk, hogy hello **$Credentials** tooeach átadott paraméter parancsfájl helyett a megszemélyesítési használatos. További információt a további engedélyeket, amelyek szükséges toouse ezt a beállítást, a következő témakörben: [megszemélyesítéshez további konfigurációs](#additional-configuration-for-impersonation). |
| Felhasználói profil betöltése során megszemélyesítésekor |False (Hamis) |Arra utasítja a Windows tooload hello felhasználói profil hello összekötő hitelesítő adatok a megszemélyesítés során. Ha hello megszemélyesített felhasználó rendelkezik barangoló profil, hello connector nem töltődik be hello központi profil. További információt a további engedélyeket, amelyek szükséges toouse ezt a paramétert, a következő témakörben: [megszemélyesítéshez további konfigurációs](#additional-configuration-for-impersonation). |
| Amikor megszemélyesítésekor bejelentkezési típusa |None |Bejelentkezési típusa a megszemélyesítés során. További információkért lásd: hello [dwLogonType] [ dw] dokumentációját. |
| Csak az aláírt parancsfájlok |False (Hamis) |Igaz értéke esetén a hello Windows PowerShell-összekötő ellenőrzi, hogy minden parancsprogram rendelkezik-e érvényes digitális aláírással. Ha értéke HAMIS, győződjön meg arról, hogy hello szinkronizálási szolgáltatás kiszolgáló Windows PowerShell végrehajtási házirendjét RemoteSigned vagy nem korlátozott. |

**Közös modul**  
hello összekötő lehetővé teszi egy megosztott Windows PowerShell-modul toostore hello konfigurációban. Ha hello összekötő parancsfájlt futtat, hello modul Windows PowerShell kicsomagolja toohello fájlrendszer, hogy minden parancsprogram importálhatók.

Jelszó-szinkronizálás, importálása és exportálása parancsfájlok hello közös modul kibontott toohello connector MAData mappában. A séma, az érvényesítési, a hierarchia és a partíció felderítési parancsfájlok a hello közös modul az kibontott toohello % TEMP % mappában. Mindkét esetben hello kibontott közös modul nevű parancsprogram toohello közös modul parancsfájl neve beállítása alapján történik.

egy modul tooload nevű FIMPowerShellConnectorModule.psm1 hello MAData mappából, a következő utasítás hello használata:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

egy modul tooload hívása FIMPowerShellConnectorModule.psm1 hello % TEMP % mappában, a következő utasítás hello használata:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**A paraméter érvényesítése**  
hello érvényesítési parancsfájlja egy nem kötelező Windows PowerShell-parancsfájlt, amely lehet, hogy érvényesek-e a hello rendszergazda által megadott összekötő-konfigurációs paraméterek használt tooensure. Érvényesítéséhez server, a kapcsolat hitelesítő adatait, és a kapcsolódási paraméterek közös hello érvényesítési parancsfájlja is érvényesek. hello érvényesítési parancsfájl neve után hello következő lapokat és a párbeszédpanelek módosítja:

* Kapcsolatok
* Globális paraméterek
* Partíció konfigurációját

hello érvényesítési parancsfájlja megkapja a következő paraméterek hello-összekötőről származó hello:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |hello konfiguráció lapon vagy hello ellenőrzési kérelem kiváltó párbeszédpanel. |
| ConfigParameters |[A KeyedCollection gyűjteményben] [ keyk] [karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |

hello érvényesítési parancsfájlja ParameterValidationResult objektum toohello csővezetéket kell visszaadnia.

**Séma felderítése**  
hello séma felderítési parancsfájl megadása kötelező. Ez a parancsfájl adja vissza hello objektumtípusok, attribútumokat és attribútum megkötések adott hello szinkronizálási szolgáltatást használja, amikor attribútum folyamata szabályok konfigurálása. hello séma felderítési parancsfájl összekötő létrehozása során fut, és feltölti a hello összekötő séma. Hello hello Synchronization Service Managert a séma frissítése a művelet is használják.

hello séma felderítési parancsfájl megkapja a következő paraméterek hello-összekötőről származó hello:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben] [ keyk] [karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |

hello parancsfájl kell visszaadnia egyetlen [séma] [ schema] objektum toohello folyamat. hello sémaobjektum áll [SchemaType] [ schemaT] objektumtípusok képviselő objektumok (például: felhasználókat és csoportokat). hello SchemaType objektum gyűjteményét tartalmazza [SchemaAttribute] [ schemaA] hello attribútumok képviselő objektumok (például: az Utónév, a Vezetéknév és a levelezési címe) hello típusú.

**További paraméterek**  
Ezenkívül toohello szabványos konfigurációs beállítások, adhat meg további egyéni beállítások, amelyek adott toohello hello összekötő példányát. Ezek a paraméterek adhatók meg, hello összekötő, partíciót, vagy futtatási lépés szintjeit, és elérhető az hello vonatkozó Windows PowerShell-parancsfájlt. Egyéni konfigurációs beállítások is tárolható hello szinkronizálási szolgáltatás adatbázis egyszerű szöveges formátumban, vagy előfordulhat, hogy titkosítani. hello szinkronizálási szolgáltatás automatikusan titkosítja, és szükség esetén biztonságos konfigurációs beállítások visszafejti.

toospecify egyéni konfigurációs beállításokat, minden paraméter vesszővel (,) külön hello neve.

egyéni konfigurációs beállítások tooaccess egy parancsfájlt, kell utótag aláhúzásjellel hello neve ( \_ ) és hello hatókör hello paraméter (globális, partíciók vagy RunStep). Ha például tooaccess hello globális FileName paraméter, használja a következő kódrészletet:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Funkciók
Felügyeleti ügynök Designer hello hello képességek lapján hello viselkedést és hello összekötő funkció határoz meg. Ezen a lapon hello választások nem lehet módosítani, hello összekötő létrehozásakor. Ez a táblázat hello kapacitásbeállításai.

![Funkciók](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

| Képesség | Leírás |
| --- | --- |
| [Megkülönböztető név stílus][dnstyle] |Azt jelzi, ha hello összekötő támogatja az megkülönböztető nevét, és ha igen, milyen stílus. |
| [Exportálás típusa][exportT] |Meghatározza, hogy hello típusú objektumok jelennek meg toohello exportálási parancsfájl. <li>AttributeReplace – például hello teljes készletét a többértékű attribútumhoz tartozó értékek, ha hello attribútum.</li><li>AttributeUpdate – hello attribútum tartozik a hello eltérések tooa többértékű attribútum.</li><li>MultivaluedReferenceAttributeUpdate - értékek nem hivatkozásos többértékű attribútumok és többértékű hivatkozás attribútumok csak eltérések teljes készletét tartalmazza.</li><li>ObjectReplace – például egy objektum attribútumainak, ha bármely attribútum módosítások</li> |
| [Adatok normalizálási][DataNorm] |Arra utasítja a hello szinkronizálási szolgáltatás toonormalize horgonyzási attribútumok előtt tooscripts megadták. |
| [Objektum megerősítése][oconf] |Hello szinkronizálási szolgáltatás függőben lévő importálás viselkedés hello konfigurálása <li>Normál – alapértelmezett viselkedését, hogy minden exportált módosítások toobe importálási keresztül megerősítve vár</li><li>NoDeleteConfirmation – a törölt objektum nincs nincs függőben lévő importálás jön létre.</li><li>NoAddAndDeleteConfirmation – objektum létrehozásakor vagy törölni, nincs nincs függőben lévő importálás jön létre.</li> |
| Megkülönböztető név használata alapjainak |Ha hello megkülönböztető név stílus be van állítva tooLDAP, hello horgonyattribútum hello összekötő terület is hello megkülönböztető neve. |
| Több összekötő párhuzamos műveletek |Ha be van jelölve, a Windows PowerShell több összekötő is futtatható egyidejűleg. |
| Partíciók |Ha be van jelölve, a hello connector több partíciót és partíció felderítési támogatja. |
| hierarchia |Ha be van jelölve, a hello összekötő támogatja az LDAP stílus hierarchikus struktúra. |
| Importálás engedélyezése |Ha be van jelölve, a hello összekötő importálja adatok importálása parancsfájlok segítségével. |
| Különbözeti importálás engedélyezése |Ha be van jelölve, a hello összekötő kérhetnek eltéréseit – a hello importálása parancsfájlok. |
| Exportálás engedélyezése |Ha be van jelölve, a hello összekötő exportálja az adatokat exportálás parancsfájlok segítségével. |
| Teljes Exportálás engedélyezése |Ha be van jelölve, a hello exportálása hello teljes kapcsolódási térbe exportálása parancsfájlok támogatása. toouse ezt a beállítást, Exportálás engedélyezése is ellenőrizni kell. |
| Nincs hivatkozási érték első exportálási fázis |Ha be van jelölve, hivatkozás attribútumok exportálása egy második exportálási fázisban. |
| Objektum átnevezése engedélyezése |Ha be van jelölve, a megkülönböztető nevét módosíthatja. |
| Törlés-hozzáadása, cseréje |Ha be van jelölve, törlése hozzáadási műveletek exportált egyetlen helyettesíti. |
| Jelszó műveletek engedélyezése |Ha be van jelölve, a jelszó-szinkronizálás parancsfájlok használata támogatott. |
| Az első fázisban exportálási jelszó engedélyezése |Ha be van jelölve, a jelszavak kiépítése során be exportált hello objektum létrehozásakor. |

### <a name="global-parameters"></a>Globális paraméterek
Globális paraméterek lapján hello hello felügyeleti ügynök Designer lehetővé teszi tooconfigure hello Windows PowerShell-parancsfájlok hello-összekötő által futtatott. Globális érték hello kapcsolat lapján definiált egyéni konfigurációs beállítások is konfigurálhatja.

**Partíció felderítése**  
A partíció egy különálló névtér egy megosztott sémáján belül. Például az Active Directory minden tartományi partíció egy erdőn belül. Egy partíciója hello logikai csoportosítását importálási és exportálási műveleteket. Importálás és exportálás partícióval rendelkezik, mivel ebben a környezetben történik olyan környezetben, és minden műveletet. Partíciók toorepresent hierarchiába kellene az LDAP-kiszolgálón. hello megkülönböztető neve partíció importálási tooverify minden visszaadott objektum partíció hello hatókörén belül van használatban. hello partíció megkülönböztető neve hello metaverse toohello összekötő terület toodetermine hello partícióról exportálás alatt egy objektum társítva kell kiépítése során is használható.

hello partíció felderítési parancsfájl hello-összekötőről származó paraméterek következő hello kap:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |

hello parancsfájl kell visszaadnia egy akár egyetlen [partíció] [ part] objektum vagy partíció objektumok toohello csővezeték [T] listája.

**Hierarchiafelderítés**  
hello hierarchia felderítési parancsfájl csak akkor használja, ha hello megkülönböztető név stílus funkció LDAP. hello parancsfájl használt tooallow meg toobrowse, és válassza tárolók csoportja, amely akkor tekinthető, vagy kívül esik a exportálására és importálásra műveletek. hello parancsfájl csak biztosítania kell, amely közvetlen hello legfelső szintű megadott toohello csomópontparancsfájl csomópontok listáját.

hello hierarchia felderítési parancsfájl hello-összekötőről származó paraméterek következő hello kap:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| ParentNode |[HierarchyNode][hn] |hello gyökércsomópont hello hierarchia mely hello a parancsfájl közvetlen gyermekei kell visszaadnia. |

hello parancsfájl vagy kell visszaadnia egy vagy egyetlen HierarchyNode gyermekobjektum gyermek HierarchyNode objektumok toohello csővezeték [T] listája.

#### <a name="import"></a>Importálás
Importálási műveleteket támogató összekötők három parancsfájlok kell megvalósítania.

**A kezdő importálása**  
hello megkezdéséhez az importálási parancsfájl futtatása egy importálási futtatása lépés hello elején. Ezzel a lépéssel kapcsolat toohello forrás rendszert hoz létre, és előkészítő lépések megtenni, mielőtt a rendszer adatok importálását hello csatlakoztatva.

hello megkezdéséhez az importálási parancsfájl kap hello paraméterek hello összekötő a következő:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Hello parancsfájl tájékoztatja hello importálása futtatása (különbözeti vagy teljes), partíció, a hierarchia, a vízjel és a méretet a várt típusú. |
| Típusok |[Séma][schema] |Hello kapcsolódási térbe importált séma. |

hello parancsfájl kell visszaadnia egyetlen [OpenImportConnectionResults] [ oicres] toohello sorban, például objektum:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Adatok importálása**  
hello importálási adatok parancsfájl neve hello összekötővel amíg hello parancsfájl azt jelenti, hogy nincs további adatok tooimport. Windows PowerShell-összekötő hello 9999 objektumok oldalméretet rendelkezik. A parancsfájl több mint 9999 az importálandó objektumok adja vissza, ha támogatniuk kell a lapozást. hello összekötő tesz elérhetővé használható tooa tároló vízjelet, hogy minden alkalommal hello importálása adatok parancsfájl egyéni tulajdonság neve, a parancsfájl folytatja, ahol abbamaradtak objektumok importálása.

hello importálási adatok parancsfájl hello-összekötőről származó paraméterek következő hello kap:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Tartás hello vízjel (CustomData), lapozható importálása során, és importálja a különbözeti. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Hello parancsfájl tájékoztatja hello importálása futtatása (különbözeti vagy teljes), partíció, a hierarchia, a vízjel és a méretet a várt típusú. |
| Típusok |[Séma][schema] |Hello kapcsolódási térbe importált séma. |

hello importálási adatok parancsfájlt kell írnia egy listát [[CSEntryChange][csec]] objektum toohello folyamat. Ez a gyűjtemény CSEntryChange attribútumok, amelyek megfelelnek az egyes importált objektumhoz tevődik össze. A teljes importálás kísérletek során ez a gyűjtemény minden objektum összes attribútumának CSEntryChange objektum teljes készletét kell rendelkeznie. A különbözeti importálás során hello CSEntryChange objektum vagy hello attribútum szintű eltérések minden objektum tooimport, vagy egy teljes ábrázolása hello objektumok, amelyek megváltoztak a (a név felülírandó mód) kell tartalmaznia.

**Záró importálása**  
Futtassa hello importálási hello megkötése hello End importálása parancsfájl van futni. Ez a parancsfájl végrehajtásának bármely Adattisztítási feladatok szükséges (például Bezárás kapcsolatok toosystems és válaszoljon toofailures).

hello end importálási parancsfájl hello-összekötőről származó paraméterek következő hello kap:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Hello parancsfájl tájékoztatja hello importálása futtatása (különbözeti vagy teljes), partíció, a hierarchia, a vízjel és a méretet a várt típusú. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Hello parancsfájl értesíti hello OK hello importálás befejeződött. |

hello parancsfájl kell visszaadnia egyetlen [CloseImportConnectionResults] [ cicres] toohello sorban, például objektum:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exportálás
Azonos toohello hello összekötő architektúrájának importálás, exportálás támogató összekötők implementálnia kell három parancsfájlok.

**Exportálás megkezdése**  
hello megkezdéséhez az exportálási parancsfájl futtatása egy Exportálás futtatási lépés hello elején. Ezzel a lépéssel kapcsolat toohello forrás rendszert hoz létre, és bármely előkészítő lépések elvégzésével előtt a rendszer adatok toohello exportálása csatlakoztatva.

hello megkezdéséhez az exportálási parancsfájl kap hello paraméterek hello összekötő a következő:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Exportálás futtatási (különbözeti vagy teljes), partíció, a hierarchiában, és a méretet a várt típusú hello hello parancsfájl értesíti. |
| Típusok |[Séma][schema] |Az exportált hello kapcsolódási térbe séma. |

hello parancsfájl kell vissza a kimeneti toohello folyamat.

**Adatok exportálása**  
Szinkronizálási szolgáltatás hello meghívja a hello adatok exportálása parancsfájl, ahányszor szükséges tooprocess összes függőben lévő exportálások van. Ha hello kapcsolódási térbe mint hello összekötő méretet a további függőben lévő exportálások, előfordulhat, hogy az adatok parancsfájl hívható többször hello exportálás és az esetlegesen többször hello ugyanahhoz az objektumhoz.

hello exportálási adatok parancsfájl hello-összekötőről származó paraméterek következő hello kap:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| CSEntries |IList[CSEntryChange][csec] |Hello összekötő terület objektumok teljes listájához a függőben lévő exportálások toobe feldolgozott ebben a fázisban. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Exportálás futtatási (különbözeti vagy teljes), partíció, a hierarchiában, és a méretet a várt típusú hello hello parancsfájl értesíti. |
| Típusok |[Séma][schema] |Az exportált hello kapcsolódási térbe séma. |

hello exportálási adatok parancsfájlt kell visszaadnia egy [PutExportEntriesResults] [ peeres] objektum toohello folyamat. Ez az objektum nem információk szükségesek a tooinclude eredmény minden exportált összekötő kivéve, ha hiba vagy a módosítás toohello horgonyattribútum történik. Például tooreturn PutExportEntriesResults objektum toohello folyamat:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Záró exportálása**  
Hello exportálási hello megkötése futtassa, hello parancsfájl toorun End exportálása. Ez a parancsfájl végrehajtásának bármely Adattisztítási feladatok szükséges (például Bezárás kapcsolatok toosystems és válaszoljon toofailures).

hello end exportálási parancsfájl hello-összekötőről származó paraméterek következő hello kap:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Exportálás futtatási (különbözeti vagy teljes), partíció, a hierarchiában, és a méretet a várt típusú hello hello parancsfájl értesíti. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Hello parancsfájl értesíti hello OK hello exportálása befejeződött. |

hello parancsfájl kell vissza a kimeneti toohello folyamat.

#### <a name="password-synchronization"></a>A jelszó-szinkronizálás
A Windows PowerShell-összekötők kapcsolatos módosításokat/használható célként.

hello jelszó parancsfájl hello-összekötőről származó paraméterek következő hello kap:

| Név | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] |Hello összekötő konfigurációs paraméterei táblájában. |
| Hitelesítő adat |[PSCredential][pscred] |Hello kapcsolat lapon hello rendszergazda által megadott hitelesítő adatokat tartalmazza. |
| Partíció |[Partíció][part] |Hello CSEntry címtárpartíció van. |
| CSEntry |[CSEntry][cse] |Összekötő terület bejegyzés hello objektum, amely a jelszó módosítása vagy alaphelyzetbe állítása kapott. |
| Művelettípus |Karakterlánc |Azt jelzi, hogy hello műveletet a alaphelyzetbe állítása (**SetPassword**), vagy pedig módosultak (**ChangePassword**). |
| PasswordOptions |[PasswordOptions][pwdopt] |Hello meghatározó jelzők szánt, jelszó-átállítási viselkedését. Ez a paraméter csak akkor érhető el, ha művelettípus **SetPassword**. |
| Régi_jelszó |Karakterlánc |Hello objektum régi jelszavát a jelszó-változtatásának feltöltve. Ez a paraméter csak akkor érhető el, ha művelettípus **ChangePassword**. |
| ÚjJelszó |Karakterlánc |Töltődik hello objektum új jelszót, amely hello parancsfájl-et kell beállítania. |

jelszó parancsfájl hello nem várt tooreturn bármely eredmények toohello Windows PowerShell kimenetátirányítási mechanizmusával van. Ha hiba lép fel hello jelszó parancsfájl, hello parancsfájl kell előidéznie hello kivételek tooinform hello szinkronizálási szolgáltatás hello problémával kapcsolatban a következő egyikét:

* [PasswordPolicyViolationException] [ pwdex1] – vált ki, ha hello jelszó nem felel meg a hello jelszóházirend csatlakoztatott hello rendszerben.
* [PasswordIllFormedException] [ pwdex2] – vált ki, ha nincs csatlakoztatva hello rendszer elfogadható hello jelszó.
* [PasswordExtension] [ pwdex3] – hello jelszó parancsfájlban más hibák lépett fel.

## <a name="sample-connectors"></a>A minta-összekötők
Hello elérhető minta összekötők teljes áttekintéséért lásd: [Windows PowerShell Connector összekötő mintavételt][samp].

## <a name="other-notes"></a>Egyéb megjegyzések
### <a name="additional-configuration-for-impersonation"></a>További konfigurációs a megszemélyesítéshez
Adja meg, de a megszemélyesített hello következő hello szinkronizálási szolgáltatás kiszolgáló engedélyeinek hello felhasználók:

Olvasási hozzáférés toohello a következő beállításkulcsokat:

* HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
* HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Environment

toodetermine hello biztonsági azonosító (SID) hello Synchronization Service szolgáltatás fiókjának, futtassa a következő PowerShell-parancsok hello:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Olvasási hozzáférés toohello a következő fájl rendszer mappák:

* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\MaData\\{következőnek}

Hello Windows PowerShell-összekötő nevét hello helyettesítéséhez hello {következőnek} helyőrző.

## <a name="troubleshooting"></a>Hibaelhárítás
* Hogyan tooenable naplózási tootroubleshoot hello összekötő információkért lásd: hello [hogyan ETW-nyomkövetés összekötők tooEnable](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
