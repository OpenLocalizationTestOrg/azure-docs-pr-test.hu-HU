---
title: "Az Azure AD Connect: Verziókiadások |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure AD Connect és az Azure AD Sync összes kiadásaiban"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b55e0f2d426e34ceef9869d5a6d1b0956d8bd076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-version-release-history"></a>Az Azure AD Connect: Verziókiadások
hello Azure Active Directory (Azure AD) csapat rendszeresen frissíti az Azure AD Connect új szolgáltatásait és funkcióit. Nem minden kiegészítéseket alkalmazható tooall célközönség.

Ez a cikk, nyomon követheti, hello verziók kiadott tervezett toohelp és toounderstand-e hogy szükséges tooupdate toohello legújabb verziója.

Ez az kapcsolódó témaköröket:


Témakör |  Részletek
--------- | --------- |
Az Azure AD Connect lépéseket tooupgrade | Különböző módszerekkel túl[frissítés a legújabb egy korábbi verziója toohello](active-directory-aadconnect-upgrade-previous-version.md) az Azure AD Connect kiadás.
Szükséges engedélyek | Szükséges jogosultságok tooapply egy frissítést, a következő témakörben: [fiókok és engedélyek](./active-directory-aadconnect-accounts-permissions.md#upgrade).
Letöltés| [Azure AD Connect letöltése](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="115610"></a>1.1.561.0
Állapot: Július 23 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Rögzített probléma

* Hello out-of-box szinkronizálási szabály okozó problémát rögzített "lejárt tooAD - felhasználó ImmutableId" toobe eltávolítása:

  * hello probléma akkor fordul elő, amikor az Azure AD Connect frissítve van, vagy ha a feladat beállítás hello *szinkronizálás konfigurációjának frissítése* hello Azure AD Connect varázsló használt tooupdate az Azure AD Connect-szinkronizálás konfigurálása.
  
  * A szinkronizálási szabály hello engedélyező alkalmazható toocustomers [msDS-ConsistencyGuid Forráshorgony szolgáltatás](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). Ez a szolgáltatás verzióját 1.1.524.0 és követően jelent meg. Hello szinkronizálási szabály eltávolítása után az Azure AD Connect nem töltheti fel a helyszíni AD ms-DS-ConsistencyGuid attribútum hello ObjectGuid attribútum értéke. Nem akadályozza meg azt, hogy a felhasználók új létre az Azure AD-be.
  
  * hello javítás biztosítja, hogy hello szinkronizálási szabály már nem törlődik, frissítés, vagy a konfiguráció módosítása során mindaddig, amíg hello szolgáltatás engedélyezve van. Meglévő ügyfelek, akik a probléma által érintett a hello javítás is biztosítja, hogy hello szinkronizálási szabály jelenik meg az Azure AD Connect toothis verziójának frissítése után vissza.

* Rögzített out-of-box szinkronizálási szabályok toohave sorrend értéke 100-nál kisebb okozó hibát:

  * Általában elsőbbséget értékek 0 – 99 egyéni szinkronizálási szabályok számára vannak fenntartva. A frissítés során hello sorrend out-of-box szinkronizálási szabályok értékei frissített tooaccommodate szinkronizálási szabály módosításokat. Toothis probléma miatt out-of-box szinkronizálási szabályok rendelt sorrend értéke 100-nál kisebb.
  
  * hello javítás megakadályozza hello probléma frissítés során. Azonban ez nem állítja vissza hello sorrend értékek a meglévő ügyfelek, akik hello probléma által érintett. Hello jövőbeli toohelp a hello visszaállítás egy különálló javítást lesznek közzétéve.

* Megtörtént egy probléma javítása adott hello [tartományok és szervezeti egységek szűrése képernyő](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) a hello Azure AD Connect varázsló azt *összes tartományok és szervezeti egységek szinkronizálása* lehetőség választása, annak ellenére, hogy a szervezeti egység-alapú szűrés engedélyezve van.

*   Megtörtént egy probléma javítása, hogy okozta hello [címtárpartíciók konfigurálása képernyő](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) a Synchronization Service Managert tooreturn hibát adott vissza, ha hello hello *frissítése* gombra kattint. hello hibaüzenet *"hiba történt a tartományok frissítése során:"System.Collections.ArrayList"tootype típusú objektum nem toocast" Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* hello hiba történt, amikor új Active Directory-tartománynak tooan meglévő AD-erdőhöz hozzá lett adva, és próbálja tooupdate Azure AD Connect használatával hello a frissítés gombra.

#### <a name="new-features-and-improvements"></a>Új szolgáltatásait és fejlesztéseit

* [Automatikus frissítési szolgáltatás](active-directory-aadconnect-feature-automatic-upgrade.md) bővített toosupport ügyfeleivel hello konfigurációi a következő volt:
  * Hello eszköz visszaírási funkció engedélyezését.
  * Hello csoport visszaírási funkció engedélyezését.
  * hello telepítési nincs egy expressz beállításokat, vagy a DirSync frissítését.
  * A hello metaverse több mint 100 000 objektummal rendelkezik.
  * Több erdő mint toomore csatlakozik. A gyorstelepítés csak tooone erdő csatlakozik.
  * hello Címtárösszekötőben fiók már nem hello alapértelmezett MSOL_ fiók.
  * hello kiszolgáló beállítása toobe átmeneti módban.
  * Hello felhasználói visszaírási funkció engedélyezését.
  
  >[!NOTE]
  >hello hatókör bővítése hello automatikus frissítési szolgáltatás az Azure AD Connect build 1.1.105.0 és után ügyfeleket érinti. Ha nem szeretné, hogy az Azure AD Connect kiszolgáló toobe automatikusan frissül, meg kell futtassa a következő parancsmagot az Azure AD Connect-kiszolgálón: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Engedélyezése vagy tiltása automatikus frissítése kapcsolatos további információkért tekintse meg a tooarticle [az Azure AD Connect: automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Állapot: Nem lesz feloldva. A build változásai verzió 1.1.561.0 szerepelnek.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Rögzített probléma

* Megtörtént egy probléma javítása, ami miatt a hello out-of-box szinkronizálási szabály "Out tooAD - felhasználó ImmutableId" toobe távolítja el szűrési konfigurációja OU-alapú frissül. A szinkronizálási szabály szükség a hello [msDS-ConsistencyGuid Forráshorgony szolgáltatás](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).

* Megtörtént egy probléma javítása adott hello [tartományok és szervezeti egységek szűrése képernyő](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) a hello Azure AD Connect varázsló azt *összes tartományok és szervezeti egységek szinkronizálása* lehetőség választása, annak ellenére, hogy a szervezeti egység-alapú szűrés engedélyezve van.

*   Megtörtént egy probléma javítása, hogy okozta hello [címtárpartíciók konfigurálása képernyő](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) a Synchronization Service Managert tooreturn hibát adott vissza, ha hello hello *frissítése* gombra kattint. hello hibaüzenet *"hiba történt a tartományok frissítése során:"System.Collections.ArrayList"tootype típusú objektum nem toocast" Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* hello hiba történt, amikor új Active Directory-tartománynak tooan meglévő AD-erdőhöz hozzá lett adva, és próbálja tooupdate Azure AD Connect használatával hello a frissítés gombra.

#### <a name="new-features-and-improvements"></a>Új szolgáltatásait és fejlesztéseit

* [Automatikus frissítési szolgáltatás](active-directory-aadconnect-feature-automatic-upgrade.md) bővített toosupport ügyfeleivel hello konfigurációi a következő volt:
  * Hello eszköz visszaírási funkció engedélyezését.
  * Hello csoport visszaírási funkció engedélyezését.
  * hello telepítési nincs egy expressz beállításokat, vagy a DirSync frissítését.
  * A hello metaverse több mint 100 000 objektummal rendelkezik.
  * Több erdő mint toomore csatlakozik. A gyorstelepítés csak tooone erdő csatlakozik.
  * hello Címtárösszekötőben fiók már nem hello alapértelmezett MSOL_ fiók.
  * hello kiszolgáló beállítása toobe átmeneti módban.
  * Hello felhasználói visszaírási funkció engedélyezését.
  
  >[!NOTE]
  >hello hatókör bővítése hello automatikus frissítési szolgáltatás az Azure AD Connect build 1.1.105.0 és után ügyfeleket érinti. Ha nem szeretné, hogy az Azure AD Connect kiszolgáló toobe automatikusan frissül, meg kell futtassa a következő parancsmagot az Azure AD Connect-kiszolgálón: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Engedélyezése vagy tiltása automatikus frissítése kapcsolatos további információkért tekintse meg a tooarticle [az Azure AD Connect: automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Állapot: Július 2017

>[!NOTE]
>A build nincs elérhető toocustomers hello Azure AD Connect automatikus frissítése funkción keresztül.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Rögzített probléma
* Megtörtént egy probléma javítása hello Initialize-ADSyncDomainJoinedComputerSync parancsmaggal, ami miatt a hello ellenőrzött tartomány konfigurált hello meglévő szolgáltatás kapcsolódási pont objektum toobe módosítható, még akkor is, ha még mindig érvényes tartományt. Ez a probléma akkor fordul elő, ha az Azure AD-bérlő több ellenőrzött tartomány hello szolgáltatáskapcsolódási pont konfigurálásához használható.

#### <a name="new-features-and-improvements"></a>Új szolgáltatásait és fejlesztéseit
* Jelszóvisszaírás már elérhető a Microsoft Azure Government felhő és a Microsoft Cloud Németország Preview. Az Azure AD Connect hello különböző szolgáltatáspéldány-támogatással kapcsolatos további információkért tekintse meg a tooarticle [az Azure AD Connect: szempontjai példányok](active-directory-aadconnect-instances.md).

* hello Initialize-ADSyncDomainJoinedComputerSync parancsmaggal most már AzureADDomain nevű új nem kötelező paraméter. Ez a paraméter lehetővé teszi, hogy adható meg, amelyeket hello szolgáltatáskapcsolódási pont konfigurálásához használt tartományi toobe érvényesíteni.

### <a name="pass-through-authentication"></a>Átmenő hitelesítés

#### <a name="new-features-and-improvements"></a>Új szolgáltatásait és fejlesztéseit
* áteresztő hitelesítés módosult szükséges hello ügynök hello neve *Microsoft Azure AD alkalmazásproxy-összekötő* túl*Microsoft Azure AD Connect hitelesítési ügynök*.

* Alapértelmezés szerint áteresztő hitelesítés engedélyezése nem engedélyezi a Jelszókivonat-szinkronizálást.


## <a name="115530"></a>1.1.553.0
Állapot: Június 2017

> [!IMPORTANT]
> A build bevezetett séma- és szinkronizálási szabály módosítások vannak. Az Azure AD Connect szinkronizálási szolgáltatás teljes importálást és teljes szinkronizálást lépéseket vált frissítés után. A hello érintő változások részleteiről az alábbiakban található. tootemporarily teljes importálást és teljes szinkronizálást lépéseket késlelteti a frissítés után, tekintse meg a tooarticle [hogyan toodefer teljes szinkronizálás a frissítés után](active-directory-aadconnect-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Az Azure AD Connect szinkronizálása

#### <a name="known-issue"></a>Ismert hiba
* Használó ügyfelek érintő probléma van a [OU-alapú szűrés](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) és az Azure AD Connect-szinkronizálás. Toohello kiválasztásakor [tartományok és szervezeti egységek szűrése lap](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) hello Azure AD Connect varázsló a következő viselkedés hello várható:
  * Ha OU-alapú szűrés engedélyezve van, hello **szinkronizálása a kiválasztott tartományok és szervezeti egységek** beállítást.
  * Ellenkező esetben hello **összes tartományok és szervezeti egységek szinkronizálása** beállítást.

hello arra vonatkozik, hogy probléma a hello **összes tartományok és szervezeti egységek beállítás szinkronizálása** mindig ki hello varázsló futtatásakor.  Ez akkor fordul elő, akkor is, ha a szervezeti egység alapú szűrés korábban beállított. Az AAD Connect konfigurációs módosítások mentése előtt győződjön meg arról, hogy hello **szinkronizálása a kiválasztott tartományok és szervezeti egységek beállítást** , és győződjön meg arról, hogy minden hozzá toosynchronize kell újra vannak engedélyezve. Ellenkező esetben OU-alapú szűrés letiltásra kerül.

#### <a name="fixed-issues"></a>Javított problémák

* Megtörtént egy probléma javítása a jelszóvisszaírás, amely lehetővé teszi az Azure AD-rendszergazda tooreset hello jelszavát egy helyszíni AD emelt szintű felhasználói fiókot. hello probléma akkor fordul elő, amikor az Azure AD Connect hello jelszó alaphelyzetbe állítása engedélyt kap hello rendszerjogosultságú fiók keresztül. hello kérdést ezen verziója nem teszi lehetővé az Azure AD-rendszergazda tooreset hello jelszavát, amely egy tetszőleges helyszíni által az Azure AD Connect AD emelt szintű felhasználói fiók kivéve, ha hello rendszergazda az adott fiók tulajdonosának hello. További információkért tekintse meg túl[biztonsági tanácsadó 4033453](https://technet.microsoft.com/library/security/4033453).

* Rögzített kapcsolatos problémát toohello [msDS-ConsistencyGuid adatforrás kapcsolatainak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funkció, ahol az Azure AD Connect does nem visszaírási tooon helyszíni AD msDS-ConsistencyGuid attribútum. hello probléma fordul elő, ha több helyszíni AD-erdőhöz hozzá tooAzure AD Connect és hello *felhasználók identitásai több könyvtárak beállítás léteznek* van kiválasztva. E konfiguráció használatakor hello eredő szinkronizálási szabályok nem sikerült adatokkal feltölteni hello sourceAnchorBinary hello Metaverzum-attribútumban. hello sourceAnchorBinary attribútum az msDS-ConsistencyGuid attribútum hello forrásattribútum szolgál. Ennek eredményeképpen visszaírási toohello ms-DSConsistencyGuid attribútum nem történik meg. toofix hello problémát, a következő szinkronizálási szabályok le lett, amely hello sourceAnchorBinary attribútum hello Metaverse mindig fel van töltve a frissített tooensure:
  * Az ad - InetOrgPerson AccountEnabled.xml
  * Az ad - InetOrgPerson Common.xml
  * Az ad - felhasználó AccountEnabled.xml
  * Az ad - felhasználó Common.xml
  * Az ad - felhasználó csatlakoztatnia SOAInAAD.xml

* Korábban, akkor is igaz, ha hello [msDS-ConsistencyGuid adatforrás kapcsolatainak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funkció nincs engedélyezve, hello "lejárt tooAD – felhasználói ImmutableId" szinkronizálási szabály továbbra is kerül tooAzure AD Connect. hello hatás ártalmatlanok, és nem okoz msDS-ConsistencyGuid attribútum toooccur visszaírása. tooavoid zavart vált logika, amely a szinkronizálási szabály hello tooensure csak hozzá, ha hello szolgáltatás engedélyezve van.

* Rögzített, ami miatt a jelszó kivonatát szinkronizálási toofail 611 hiba eseménnyel kapcsolatos problémát. Ez a probléma akkor fordul elő, amikor valamelyik vagy több tartomány tartományvezérlői el lettek távolítva a helyszíni AD. Az egyes jelszó-szinkronizálási ciklusok végén hello hello szinkronizálási cookie-k ki a helyszíni AD tartalmaz meghívása azonosítók eltávolítása hello tartományvezérlők USN (frissítési sorszámot) érték a 0. a jelszószinkronizálás-kezelő hello nem toopersist szinkronizálási cookie-t tartalmazó USN értéke 0, és hibaesemény 611 hibát jelez. Hello következő szinkronizálás során ciklus, hello a jelszószinkronizálás-kezelő felhasználások hello utolsó megőrzött szinkronizálási cookie-t, amely nem tartalmaz USN értéke 0. Ennek hatására hello ugyanazt a jelszómódosítást toobe újraszinkronizálása. A javítás hello a jelszószinkronizálás-kezelő továbbra is fennáll hello szinkronizálási cookie-k megfelelően.

* Korábban még akkor is, ha az automatikus frissítés le van tiltva hello Set-ADSyncAutoUpgrade parancsmag használatával, hello automatikus frissítési folyamat folytatja frissítés toocheck rendszeres időközönként, és letöltött hello telepítő toohonor megfelelő támaszkodik. A javítás hello automatikus frissítési folyamat már nem ellenőrzi a frissítésre rendszeres időközönként. hello javítás automatikusan alkalmazza, ha az adott Azure AD Connect verzió frissítési telepítő egyszer végrehajtása.

#### <a name="new-features-and-improvements"></a>Új szolgáltatásait és fejlesztéseit

* Korábban, hello [msDS-ConsistencyGuid adatforrás kapcsolatainak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funkció csak a rendelkezésre álló toonew központi telepítések volt. Most már elérhető tooexisting központi telepítések. Pontosabban:
  * tooaccess hello szolgáltatást, hello Azure AD Connect varázsló elindításához, és válassza ki a hello *frissítés Forráshorgony* lehetőséget.
  * A beállítás csak a sourceAnchor attribútum objectGuid használ látható tooexisting telepítések esetén.
  * Hello beállítás konfigurálásakor hello varázsló ellenőrzi a helyszíni Active Directoryban hello msDS-ConsistencyGuid attribútum hello állapotát. Ha hello attribútum nincs konfigurálva a user objektum hello könyvtárban, hello varázsló hello msDS-ConsistencyGuid hello sourceAnchor attribútum használ. Ha egy vagy több felhasználói objektumok hello könyvtárban hello attribútum van beállítva, hello varázsló arra a következtetésre jut hello attribútum más alkalmazások használják, és nem felel meg a sourceAnchor attribútum, és hello Forráshorgony módosítás tooproceed nem teszi lehetővé. Ha biztos benne, hogy hello attribútum nincs meglévő alkalmazások által használt, hogyan toosuppress hello hiba információt kell toocontact támogatása.

* Adott túl**userCertificate** eszközobjektumot az Azure AD Connect most attribútum keresi a szükséges tanúsítványok értékek [csatlakozás tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10 élmény](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) és a szűrők meg hello többi tooAzure AD szinkronizálása előtt. Ez a viselkedés, a "Kimenő tooAAD - szerint az eszköz SOAInAD csatlakoztatás" hello out-of-box szinkronizálási szabály frissítve lett tooenable.

* Az Azure AD Connect mostantól támogatja a visszaírási az Exchange Online **cloudPublicDelegates** attribútum tooon helyszíni AD **publicDelegates** attribútum. Ez lehetővé teszi a hello forgatókönyv, ahol az Exchange Online-postaláda engedélyezhetők a helyszíni Exchange postaládával SendOnBehalfTo jogok toousers. toosupport ezt a szolgáltatást, a "kimenő tooAD – a felhasználó Exchange hibrid PublicDelegates visszaírási" új out-of-box szinkronizálási szabály hozzá lett adva. A szinkronizálási szabály csak kerül tooAzure AD csatlakozás, ha Exchange hibrid szolgáltatás engedélyezve van.

*   Az Azure AD Connect mostantól támogatja a hello szinkronizálása **altRecipient** Azure ad-attribútum. Ez a változás a következő out-of-box szinkronizálási szabályok lettek toosupport tooinclude szükséges hello Attribútumfolyam frissítése:
  * Az AD-felhasználó Exchange-ből
  * Kimenő tooAAD – felhasználói ExchangeOnline
  
* Hello **cloudSOAExchMailbox** hello Metaverse attribútuma azt jelzi, hogy egy adott felhasználó Exchange Online-postaláda vagy sem. A definiálás lett frissített tooinclude további Exchange Online RecipientDisplayTypes, ilyen berendezéseket és konferenciaterem postaládák. tooenable Ez a változás hello definition hello cloudSOAExchMailbox attribútum, amely a out-of-box szinkronizálási szabály "A az aad-ben – felhasználói Exchange hibrid", a frissült:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

... toohello következő:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* A hozzáadott hello következő X509Certificate2-kompatibilis függvények létrehozása szinkronizálási szabály kifejezések toohandle tanúsítvány értékek hello userCertificate attribútum készlete:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|CertThumbprint|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Válassza ezt:|
    |CertKeyAlgorithmParams|CertHashString|Ha|
    |||a|

* Változás történt a következő volt bevezetett tooallow ügyfelek toocreate egyéni szinkronizálási szabályok tooflow sAMAccountName domainNetBios és a csoport objektumainak domainFQDN, valamint a felhasználói objektumok distinguishedName:

  * TooMV séma a következő attribútumok bővült:
    * Csoport: AccountName
    * Csoport: domainNetBios
    * Csoport: domainFQDN
    * Személyesen: distinguishedName

  * TooAzure Címtárösszekötőben séma a következő attribútumok bővült:
    * Csoport: OnPremisesSamAccountName
    * Csoport: NetBiosName
    * Csoport: tartománynév
    * Felhasználó: OnPremisesDistinguishedName

* hello ADSyncDomainJoinedComputerSync parancsmag parancsfájl most már AzureEnvironment nevű új nem kötelező paraméter. hello paraméter használt toospecify megfelelő Azure Active Directory-bérlő melyik régióban hello van tárolva. Érvényes értékek a következők:
  * AzureCloud (alapértelmezett)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Frissített szinkronizálási szabály szerkesztő toouse (helyett kiépíteni) hivatkozás típusú hello alapértelmezett értékként szinkronizálási szabály létrehozása közben csatlakozik.

### <a name="ad-fs-management"></a>AD FS-Szolgáltatáskezelés

#### <a name="issues-fixed"></a>Megoldott problémák

* És következő URL-címek használata az Azure AD tooimprove hibatűrő módon hitelesítési kimaradás által bevezetett új WS-Federation végpontok lesz hozzáadott tooon helyszíni az AD FS fél szintű bizalmi kapcsolatainak konfigurációja a műveletre:
  * https://ests.login.microsoftonline.com/login.srf
  * https://stamp2.login.microsoftonline.com/login.srf
  * https://CCS.login.microsoftonline.com/login.srf
  * https://CCS-sdf.login.microsoftonline.com/login.srf
  
* Rögzített IssuerID az AD FS toogenerate megfelelő jogcím értékét okozó problémát. hello probléma akkor fordul elő, ha több ellenőrzött tartomány hello Azure AD-bérlő és hello tartományutótagját hello userPrincipalName tulajdonsághoz használt attribútumot toogenerate hello IssuerID nincs legalább 3-szintek mély (például johndoe@us.contoso.com). hello probléma megoldásához hello jogcímszabályok által használt hello regex frissítése.

#### <a name="new-features-and-improvements"></a>Új szolgáltatásait és fejlesztéseit
* Korábban, hello az AD FS Tanúsítványkezelő funkciója biztosítja az Azure AD Connect csak használható az Azure AD Connecten keresztül felügyelt az AD FS-farmok. Most hello szolgáltatás használható az AD FS-farmok, amelyeket nem kezel az Azure AD Connect használatával.

## <a name="115240"></a>1.1.524.0
Kiadás dátuma: Előfordulhat, hogy 2017

> [!IMPORTANT]
> A build bevezetett séma- és szinkronizálási szabály módosítások vannak. Az Azure AD Connect szinkronizálási szolgáltatás teljes importálást és teljes szinkronizálást lépéseket vált frissítés után. A hello érintő változások részleteiről az alábbiakban található.
>
>

**Javított problémák:**

Az Azure AD Connect szinkronizálása

* Megtörtént egy probléma javítása, amely aktiválja az automatikus frissítés toooccur a hello Azure AD Connect-kiszolgáló, akkor is, ha az ügyfél letiltotta a Set-ADSyncAutoUpgrade parancsmaggal hello hello szolgáltatást. Ez a javítás hello automatikus frissítési folyamat hello kiszolgálón is ellenőrzi a frissítéshez rendszeres időközönként, de hello letöltött telepítőt eleget tegyen hello automatikus frissítési konfiguráció.
* A DirSync helybeni frissítés során az Azure AD Connect létrehoz egy Azure AD szolgáltatás az Azure ad-vel szinkronizálandó hello Azure AD-összekötő által használt fiók toobe. Miután hello-fiók létrehozása az Azure AD Connect hello fiókkal az Azure AD hitelesíti. Egyes esetekben hitelesítés nem sikerül átmeneti hibái miatt pedig emiatt DirSync helybeni frissítés toofail hibával *"hiba történt a feladat végrehajtása konfigurálása az AAD Sync: AADSTS50034: toosign az alkalmazásba, hello fiók fel kell venni toohello xxx.onmicrosoft.com directory."* tooimprove hello rugalmasságát, DirSync frissítését, az Azure AD Connect hello hitelesítési lépés most újrapróbálkozik.
* Hiba történt a build 443-as DirSync helybeni frissítés toosucceed okozó problémát, de még nem jöttek létre futtatási profilokat a címtár-szinkronizálás szükséges. Ez az Azure AD Connect build logika javító tartalmazza. Amikor ügyfél toothis build frissíti, az Azure AD Connect észleli a hiányzó futtatási profilok és létrehozása.
* A jelszó-szinkronizálás folyamat toofail toostart Event ID 6900 és hibát okozó hibát rögzített *"Elemet ugyanazzal a kulccsal már hozzá lett adva hello"*. Ez a probléma akkor fordul elő, ha OU-szűrés konfigurációs tooinclude AD konfigurációs partíción frissíti. a probléma, jelszó-szinkronizálás most feldolgozni toofix szinkronizálja a jelszó-módosítások csak tartománypartíciók AD. Például a konfigurációs partíció nem tartománypartícióról kimarad.
* Expressz telepítés során az Azure AD Connect létrehoz egy helyszíni Active Directory tartományi szolgáltatások fiókot használják a helyszíni hello AD összekötő toocommunicate toobe AD. Korábban a hello fiók hello PASSWD_NOTREQD jelzővel hello felhasználói-fiók-vezérlő attribútum beállítása jön létre, és a véletlenszerű jelszót hello fiók be van állítva. Most az Azure AD Connect explicit módon eltávolítja hello PASSWD_NOTREQD jelzőt hello jelszó hello fiók beállítása után.
* Hiba történt a DirSync frissítési toofail okozó hibát rögzített *"holtpont alakult ki az sql server melyik közben tooacquire egy Alkalmazászárolás"* amikor hello mailNickname attribútum hello megtalálható a helyszíni AD-séma, de nem a kötött toohello AD-felhasználó objektumosztály.
* Rögzített eszköz visszaírási szolgáltatás tooautomatically okozó hibát le kell tiltani, ha a rendszergazda frissítése folyamatban van az Azure AD Connect szinkronizálási konfigurációját az Azure AD Connect varázsló segítségével. Ennek oka hello varázsló elvégzése előfeltétel ellenőrzése hello meglévő visszaírási eszközkonfiguráció a helyszíni AD-és hello az ellenőrzés sikertelen. hello javítás tooskip hello ellenőrizze akkor, ha a eszközvisszaíró korábban már engedélyezve van.
* tooconfigure OU szűrés, hello Azure AD Connect varázsló, vagy hello Synchronization Service Managert. Korábban hello Azure AD Connect varázsló tooconfigure szervezeti egységek szűrése használatakor létrehozott új szervezeti egységek ezt követően érhetők el a címtár-szinkronizálás. Ha nem szeretné, hogy új szervezeti egységek toobe tartalmazza, konfigurálnia kell a szervezeti egység szűrés használatával hello Synchronization Service Managert. Most, érhet el hello kívánt viselkedést eredményező beállítást, az Azure AD Connect varázsló segítségével.
* Rögzített admin, telepítése ahelyett, hogy a hello dbo séma hello hello sémája alatt hozott létre az Azure AD Connect toobe által igényelt tárolt eljárások okozó hibát.
* Rögzített hello TrackingId attribútum nincs megadva az AAD Connect kiszolgáló eseménynaplók hello Azure AD toobe által visszaadott okozó hibát. hello probléma akkor fordul elő, ha az Azure AD Connect Azure ad-átirányítás üzenetet kap, és az Azure AD Connect nem tooconnect toohello végpont megadott. hibaelhárítás során az ügyféloldali szolgáltatásnaplók támogatási szakértők toocorrelate hello TrackingId használja.
* Ha az Azure AD Connect Azure ad-LargeObject hibaüzenetet kap, az Azure AD Connect állít elő, a EventID 6941 és üzenet esemény *"hello telepített objektum mérete túl nagy. Trim hello számát ezen az objektumon."* A hello azonos idő, az Azure AD Connect is hoz létre egy eseményazonosító 6900 és az üzenet félrevezető esemény *"Microsoft.Online.Coexistence.ProvisionRetryException: a hello Windows nem toocommunicate az Azure Active Directory szolgáltatás."* toominimize zavart, az Azure AD Connect nem állít elő, ez utóbbi esetben hello LargeObject hiba fogadásakor.
* Rögzített tooupdate hello általános LDAP-összekötő konfigurációja közben nem válaszol a Synchronization Service Managert toobecome hello okozó hibát.

**Új funkciók fejlesztései:**

Az Azure AD Connect szinkronizálása
* Szinkronizálási szabály módosításokat – hello követő szinkronizálási szabály változások történtek:
  * Frissített alapértelmezett szinkronizálási szabály toonot exportálási attribútumainak beállítása **userCertificate** és **userSMIMECertificate** Ha hello attribútumok értéke meghaladja a 15.
  * AD-attribútumok **employeeID** és **msExchBypassModerationLink** mostantól beletartoznak hello szinkronizálási szabály alapértelmezés szerint.
  * AD attribútum **fénykép** szinkronizálási szabály alapértelmezés szerint el lett távolítva.
  * Hozzáadott **preferredDataLocation** toohello Metaverse séma- és AAD-összekötő séma. Ügyfelek, vagy az Azure AD-attribútumok alapján valósíthatja meg tooupdate egyéni szabályok toodo így szinkronizálása. További információk a hello attribútum toofind tekintse meg a tooarticle szakasz [az Azure AD Connect szinkronizálása: hogyan toomake egy módosítás toohello alapértelmezett konfiguráció - PreferredDataLocation szinkronizálásának engedélyezése](active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-preferreddatalocation).
  * Hozzáadott **userType** toohello Metaverse séma- és AAD-összekötő séma. Ügyfelek, vagy az Azure AD-attribútumok alapján valósíthatja meg tooupdate egyéni szabályok toodo így szinkronizálása.

* Az Azure AD Connect most automatikusan lehetővé teszi, hogy hello ConsistencyGuid attribútum hello Forráshorgony attribútumaként a helyszíni Active Directory-objektumok. További, az Azure AD Connect hello objectGuid attribútum értékű hello ConsistencyGuid attribútumot tölti fel, ha üres. Ez a funkció csak a megfelelő toonew telepítési. Ezzel a funkcióval kapcsolatban további toofind tekintse meg a tooarticle szakasz [az Azure AD Connect: tervezési alapelvek - msDS-ConsistencyGuid használata sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).
* Új Invoke-ADSyncDiagnostics lett parancsmag hibaelhárítási hozzáadott toohelp diagnosztizálása Jelszókivonat-szinkronizálást kapcsolatos hiba lépett fel. Hello parancsmag használatával kapcsolatos információkért tekintse meg a tooarticle [és az Azure AD Connect-szinkronizálás jelszó-szinkronizálás hibaelhárítása](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-synchronization).
* Az Azure AD Connect mostantól támogatja a szinkronizálási Mail-Enabled nyilvános mappa a következő helyről objektumokat a helyszíni AD tooAzure AD. Engedélyezheti, hogy az Azure AD Connect varázsló a választható szolgáltatások segítségével hello szolgáltatást. Ezzel a funkcióval kapcsolatban további toofind tekintse meg a tooarticle [Office 365 Directory-alapú biztonsági blokkolja a támogatása a helyszíni Mail nyilvános mappák](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Az Azure AD Connect szükséges egy Tartományi fiók toosynchronize a helyszíni AD. Korábban Ha telepítette az Azure AD Connect hello Express móddal, biztosíthatja, hogy hello egy vállalati rendszergazdai fiók hitelesítő adatait, és az Azure AD Connect hozna létre az Active Directory tartományi szolgáltatások hello fiók szükséges. Azonban az egyéni telepítés és erdők tooan meglévő központi telepítés hozzáadása, akkor volt szükséges tooprovide hello AD DS-fiókjához helyette. Most akkor is hello beállítás tooprovide egy vállalati rendszergazdai fiók hitelesítő adatait hello egyéni telepítés során, és lehetővé teszik a szükséges hello AD DS-fiók létrehozása az Azure AD Connect.
* Az Azure AD Connect mostantól támogatja az SQL AOA. Az Azure AD Connect telepítése előtt engedélyeznie kell az SQL AOA. A telepítés során az Azure AD Connect észleli a megadott hello SQL-példány engedélyezve van az SQL AOA-e nem. Ha SQL AOA engedélyezve van, az Azure AD Connect további adatok Ha SQL AOA-e a konfigurált toouse szinkron replikáció vagy aszinkron replikáció. Hello rendelkezésre állási csoport figyelőjének beállításakor javasoljuk, hogy állítsa a hello RegisterAllProvidersIP tulajdonság too0. Ennek az az oka az Azure AD Connect jelenleg használ SQL Native Client tooconnect tooSQL és az SQL natív ügyfél nem támogatja a MultiSubNetFailover tulajdonság hello használatával.
* Ha az Azure AD Connect-kiszolgáló LocalDB használata hello adatbázis, és elérte 10 GB-os méret, hello szinkronizálási szolgáltatás nem indul el. Korábban szükség van tooperform hello LocalDB tooreclaim ShrinkDatabase művelet elég hely a DB hello szinkronizálási szolgáltatás toostart. Miután amely, akkor használja hello Synchronization Service Managert toodelete futtatható előzmények tooreclaim DB szabad hely. Most Start-ADSyncPurgeRunHistory parancsmag toopurge előzményadatok LocalDB tooreclaim DB területről fut is használhatja. További, ez a parancsmag támogatja-e egy offline üzemmód (megadásával hello - offline paraméter) lehet, amely során használandó hello szinkronizálási szolgáltatás nem működik. Megjegyzés: hello kapcsolat nélküli módban csak csak használható, ha hello szinkronizálási szolgáltatás nem fut, valamint használt hello adatbázis localdb programba.
* tooreduce hello tárhely szükséges, az Azure AD Connect szinkronizálási hiba részletes adatait most tömöríti előtt LocalDB/SQL-adatbázisban tárolja őket. Frissítésekor az Azure AD Connect toothis verziójának egy korábbi verziójából származó, az Azure AD Connect egy egyszeri tömörítési végez meglévő szinkronizálási hiba részletes adatait.
* Korábban, miután frissített OU-szűrés konfigurációs, akkor manuálisan futtatnia kell a teljes importálás tooensure meglévő objektum megfelelően szereplő/kizárja a címtár-szinkronizálás. Most, az Azure AD Connect automatikusan elindítja a teljes importálás során hello tovább szinkronizálási ciklust. További, a teljes importálás alkalmazott toohello AD összekötők hello frissítés hatással lehet csak lehet. Megjegyzés: ennek a fejlesztésnek köszönhetően hello Azure AD Connect varázsló használatával végzett szűrés alkalmazható tooOU. Szűrés hello Synchronization Service Manager használatával létrehozott frissítés alkalmazható tooOU nincs.
* Korábban biztonságicsoport-alapú szűrés támogatja a felhasználók, csoportok, és forduljon objektumok csak. Most csoport-alapú szűrés is támogatja számítógép-objektumokat.
* Törölheti a kapcsolódási térbe adatok korábban, az Azure AD Connect sync scheduler letiltásáig. Most hello Synchronization Service Managert blokkok hello adattörlést kapcsolódási térbe azt észleli, hogy hello Feladatütemező engedélyezve van-e. További a rendszer figyelmeztetést adja vissza tooinform ügyfelek kapcsolatos esetleges adatvesztés hello összekötő címtér adatainak törlésekor.
* Korábban le kell tiltania az Azure AD Connect varázsló toorun írjanak PowerShell elő megfelelően. Ez a probléma részlegesen megoldódott. PowerShell írjanak elő engedélyezheti az Azure AD Connect varázsló toomanage szinkronizálási konfigurációjának használatakor. PowerShell írjanak elő le kell tiltani, ha az Azure AD Connect varázsló toomanage az AD FS konfigurációt használ.



## <a name="114860"></a>1.1.486.0
Kiadás dátuma:-2017. április

**Javított problémák:**
* Rögzített hello probléma, ahol az Azure AD Connect nem telepíthető sikeresen a Windows Server honosított verzióját.

## <a name="114840"></a>1.1.484.0
Kiadás dátuma:-2017. április

**Ismert problémák:**

* Az Azure AD Connect ezen verziója lesz telepítése nem sikerült a következő feltételek hello összes teljesülése esetén:
   1. Vagy a DirSync helybeni frissítés vagy friss telepítése az Azure AD Connect végzik.
   2. Ha a beépített Rendszergazdák csoportjának hello kiszolgálón hello neve nem "Rendszergazdák" a Windows Server honosított verzióját használja.
   3. Hello alapértelmezett SQL Server 2012 Express LocalDB helyett a saját teljes SQL biztosít az Azure AD Connect telepített használ.

**Javított problémák:**

Az Azure AD Connect szinkronizálása
* Megtörtént egy probléma javítása ahol hello szinkronizálásütemezési kihagyja-e hello teljes szinkronizálási lépést, ha egy vagy több összekötők adott szinkronizálási lépés futtatási profil hiányzik. Például manuálisan hozzáadott egy összekötő hello Synchronization Service Managert használja a különbözeti importálás az futtatási profiljának létrehozása nélkül. Ez a javítás biztosítja, hogy hello szinkronizálásütemezési továbbra is fennáll, a többi összekötőt különbözeti importálás toorun.
* Megtörtént egy probléma javítása ahol hello szinkronizálási szolgáltatás azonnal leállítja a feldolgozást a futtatási profil van észlel a problémát a Futtatás hello lépések egyike. Ez a javítás biztosítja, hogy hello szinkronizálási szolgáltatás átugrása lépés futtatásához, és továbbra is tooprocess hello rest. Például hogy a különbözeti importálás, futtassa a profil az AD-összekötő több futtatási lépés (egy mindegyik helyszíni AD-tartomány). Szinkronizálási szolgáltatás hello fognak futni különbözeti importálás hello más AD-tartományok akkor is, ha ezek egyikét hálózati kapcsolati problémáit.
* Rögzített hello Azure AD-összekötő frissítés toobe automatikus frissítése eltekint okozó hibát.
* Megtörtént egy probléma javítása, hogy okok az Azure AD Connect tooincorrectly megállapításához, hogy hello kiszolgáló egy olyan tartományvezérlőre, amely kapcsolja okok DirSync toofail frissíteni a telepítés során.
* Rögzített helyi frissítési toonot létrehozása minden DirSync okozó hibát futtassa az Azure AD-összekötő hello-profil.
* Megtörtént egy probléma javítása ahol hello Synchronization Service Manager felhasználói felülete nem válaszol tooconfigure általános LDAP-összekötő tett kísérlet során.

AD FS-Szolgáltatáskezelés
* Ha hello Azure AD Connect varázsló sikertelen lesz, ha hello AD FS elsődleges csomópont lett áthelyezett tooanother server rögzített kapcsolatos problémát.

Asztali egyszeri bejelentkezés
* Rögzített problémát hello Azure AD Connect varázsló, ahol hello bejelentkezési képernyő nem engedélyezi az asztal SSO funkció engedélyezése, ha úgy döntött, hogy a jelszó-szinkronizálás, a bejelentkezési lehetőséget új telepítése során.

**Új funkciók fejlesztései:**

Az Azure AD Connect szinkronizálása
* Azure AD Connect szinkronizálási szolgáltatás már támogatja a szolgáltatásfiókként virtuális szolgáltatásfiók, a felügyelt szolgáltatásfiókok és csoportosan felügyelt szolgáltatásfiók a hello használatát. Ez vonatkozik az Azure AD Connect csak toonew telepítése. Ha az Azure AD Connect telepítése:
    * Alapértelmezés szerint az Azure AD Connect varázsló létrehoz egy virtuális szolgáltatásfiókot, és használja a szolgáltatás fiók.
    * Ha tartományvezérlőre telepíti, az Azure AD Connect visszavált tooprevious viselkedését, ahol azt tartományi felhasználói fiókot hoz létre, és helyette használja annak szolgáltatásfiókként.
    * Hello alapértelmezett viselkedést felülírhatja hello következő egyikét:
      * Egy csoport felügyelt szolgáltatásfiók
      * Felügyelt szolgáltatásfiók
      * Egy tartományi felhasználói fiók
      * Helyi felhasználói fiók
* Korábban Ha tooa új példányt az Azure AD Connect tartalmazó frissít összekötők frissítése, vagy szinkronizálási szabály változások, az Azure AD Connect egy teljes szinkronizálási ciklust indít. Most az Azure AD Connect szelektív módon elindítja a teljes importálás csak az összekötők, frissítés, és teljes szinkronizálást lépést csak az összekötők szinkronizálási szabály módosult.
* Korábban a hello exportálás törlési küszöbét csak keresztül hello szinkronizálásütemezési kiváltó tooexports vonatkozik. Most hello szolgáltatás ki van bővítve tooinclude exportálja manuálisan indított hello ügyfél hello Synchronization Service Manager használatával.
* Az Azure AD-bérlő nincs a szolgáltatás konfigurációja, amely azt jelzi, hogy a jelszó-szinkronizálás szolgáltatás engedélyezve van a bérlő vagy nem. Korábban is egyszerűen hello szolgáltatás konfigurációs toobe az Azure AD Connect megfelelően van konfigurálva, hogy az aktív és egy átmeneti kiszolgálón. Most, az Azure AD Connect megkísérli tookeep hello szolgáltatás konfigurációját az aktív konzisztens csak az Azure AD Connect-kiszolgáló.
* Az Azure AD Connect varázsló most észleli, és a rendszer figyelmeztetést ad vissza, ha a helyszíni AD nem rendelkezik AD Lomtár engedélyezve van.
* Korábban exportálás tooAzure AD érvénytelenné válik, és sikertelen lesz, ha hello kombinált hello objektumok hello kötegben mérete meghaladja a meghatározott küszöbérték. Most hello szinkronizálási szolgáltatás újra megkísérli a tooresend hello objektumok különálló, kisebb kötegekben Ha hello hiba lép fel.
* Szinkronizálási szolgáltatás kulcskezelés alkalmazás hello el lett távolítva a Windows Start menüből. Titkosítási kulcs felügyeleti továbbra is toobe parancssori felületen miiskmu.exe használata támogatott. A titkosítási kulcs kezelésével kapcsolatos információkért tekintse meg a tooarticle [Abandoning hello Azure AD Connect Sync titkosítási kulcs](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Korábban Ha hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavának módosításához hello szinkronizálási szolgáltatás nem lesz képes start megfelelően elhagyott hello titkosítási kulcs, és újra lesz inicializálva a hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavát. Most ez már nincs szükség.

Asztali egyszeri bejelentkezés

* Az Azure AD Connect varázsló már nem szükséges port 9090 toobe megnyitott hello hálózaton áteresztő hitelesítés és asztali SSO konfigurálásakor. Csak a 443-as porton szükség. 

## <a name="114430"></a>1.1.443.0
Kiadás dátuma: Március 2017

**Javított problémák:**

Az Azure AD Connect szinkronizálása
* Ha hello megjelenített neve a hello Azure AD-összekötő nem tartalmaz hello kezdeti onmicrosoft.com tartomány hozzárendelt toohello az Azure AD bérlő az Azure AD Connect varázsló toofail okozó problémát rögzített.
* Kijavítva, miközben kapcsolatot tooSQL adatbázis hello szinkronizáláshoz használt szolgáltatásfióknak hello jelszavát például az aposztróf, kettőspont és lemezterület speciális karaktereket tartalmaz. Ha az Azure AD Connect varázsló toofail okozó problémát.
* Rögzített egy probléma, amely az Azure AD Connect-kiszolgáló az átmeneti környezetű üzemmód a "hello dimage rendelkezik, amely eltér attól hello kép horgonya" toooccur hello hibát okoz, miután ideiglenesen kizárt egy helyszíni AD szinkronizálása objektumra, és tartalmazza majd meg újra szinkronizálása.
* Rögzített egy probléma, amely az Azure AD Connect-kiszolgáló az átmeneti környezetű üzemmód a "hello objektum megkülönböztető név szerint egy látszólagos" toooccur hello hibát okoz, miután ideiglenesen kizárt egy helyszíni AD szinkronizálása objektumra, és majd érhető el, újra a szinkronizálással.

AD FS-Szolgáltatáskezelés
* Kijavítva, ahol az Azure AD Connect varázsló nélkül AD FS konfigurációjának frissítése, hello jobb jogcímeket a függő entitás megbízhatóságának másodlagos bejelentkezési azonosító konfigurálása után hello problémát.
* Megtörtént egy probléma javítása az Azure AD Connect varázsló esetén nem toocorrectly leíró AD FS-kiszolgáló szolgáltatás fiókkal vannak konfigurálva a userPrincipalName formátumban sAMAccountName formátum helyett.

Átmenő hitelesítés
* Ha át keresztül hitelesítés van beállítva, de az összekötő regisztrálása meghiúsul az Azure AD Connect varázsló toofail okozó problémát rögzített.
* Rögzített toobypass ellenőrzéseket varázsló Ha asztali SSO szolgáltatás engedélyezve van a kijelölt bejelentkezési módszert az Azure AD Connect okozó problémát.

Jelszó alaphelyzetbe állítása
* Megtörtént egy probléma javítása miatt előfordulhat, hogy hello Azure AAD-csatlakozás server toonot kísérlet toore-csatlakozás, ha egy tűzfal vagy a proxy hello kapcsolat következtében leállt.

**Új funkciók fejlesztései:**

Az Azure AD Connect szinkronizálása
* Get-ADSyncScheduler parancsmag most SyncCycleInProgress nevű új logikai tulajdonság adja vissza. Hello értéket adott vissza. Ha igaz, az azt jelenti, hogy van-e folyamatban lévő ütemezett szinkronizálási ciklust.
* Azure AD Connect telepítés és a telepítési naplók tárolásához célmappa naplófájlokból %localappdata%\AADConnect too%programdata%\AADConnect tooimprove kisegítő toohello át lett helyezve.

AD FS-Szolgáltatáskezelés
* Támogatása az AD FS Farm SSL-tanúsítvány frissítése.
* Támogatása az AD FS 2016 felügyeletéhez.
* AD FS telepítése során mostantól megadhatja a meglévő csoportosan felügyelt szolgáltatásfiók (csoportosan felügyelt szolgáltatásfiók).
* Most már beállíthat SHA-256 hello aláírás-kivonatoló algoritmus az Azure AD közvetítőpartneri megbízhatósághoz.

Jelszó alaphelyzetbe állítása
* Bevezetett fejlesztéseket tooallow hello termék toofunction szigorúbb tűzfalszabályok használó környezetekben.
* Továbbfejlesztett kapcsolat megbízhatóság tooAzure Service Bus.

## <a name="113800"></a>1.1.380.0
Kiadás dátuma: 2016. December

**Rögzített problémát:**

* A build rögzített hello probléma hol hello issuerid jogcímszabály az Active Directory összevonási szolgáltatások (AD FS) hiányzik.

>[!NOTE]
>A build nincs elérhető toocustomers hello Azure AD Connect automatikus frissítése funkción keresztül.

## <a name="113710"></a>1.1.371.0
Kiadás dátuma: 2016. December

**Ismert hiba:**

* a build hello issuerid jogcímszabály az AD FS hiányzik. hello issuerid jogcímszabály szükség, ha összevonja a több tartományt az Azure Active Directoryval (Azure AD). Ha használja az Azure AD Connect toomanage a helyszíni AD FS üzembe helyezése, toothis build frissítése hello meglévő issuerid jogcímszabály eltávolítja az AD FS konfigurációt. Hello a probléma megkerüléséhez hello issuerid jogcímszabály hozzáadásával hello telepítés/frissítés után. Hello issuerid hozzáadására vonatkozó részleteket jogcímszabály, részletek toothis a cikk a [többtartományos támogatás az Azure AD összevonási szolgáltatásához](active-directory-aadconnect-multiple-domains.md).

**Rögzített problémát:**

* Ha a Port 9090 hello kimenő kapcsolat nincs megnyitva, hello Azure AD Connect telepítése vagy frissítése sikertelen.

>[!NOTE]
>A build nincs elérhető toocustomers hello Azure AD Connect automatikus frissítése funkción keresztül.

## <a name="113700"></a>1.1.370.0
Kiadás dátuma: 2016. December

**Ismert problémák:**

* a build hello issuerid jogcímszabály az AD FS hiányzik. hello issuerid jogcímszabály szükség, ha összevonja a több tartomány és az Azure AD. Ha használja az Azure AD Connect toomanage a helyszíni AD FS üzembe helyezése, toothis build frissítése hello meglévő issuerid jogcímszabály eltávolítja az AD FS konfigurációt. Hello a probléma megkerüléséhez hello issuerid jogcímszabály hozzáadásával a telepítés/frissítés után. Részletek issuerid hozzáadására vonatkozó jogcímszabály, részletek toothis a cikk a [többtartományos támogatás az Azure AD összevonási szolgáltatásához](active-directory-aadconnect-multiple-domains.md).
* Port 9090 nyitott kimenő toocomplete telepítési kell lennie.

**Új szolgáltatások:**

* Áteresztő hitelesítés (előzetes verzió).

>[!NOTE]
>A build nincs elérhető toocustomers hello Azure AD Connect automatikus frissítése funkción keresztül.

## <a name="113430"></a>1.1.343.0
Kiadás dátuma: 2016. November

**Ismert hiba:**

* a build hello issuerid jogcímszabály az AD FS hiányzik. hello issuerid jogcímszabály szükség, ha összevonja a több tartomány és az Azure AD. Ha használja az Azure AD Connect toomanage a helyszíni AD FS üzembe helyezése, toothis build frissítése hello meglévő issuerid jogcímszabály eltávolítja az AD FS konfigurációt. Hello a probléma megkerüléséhez hello issuerid jogcímszabály hozzáadásával a telepítés/frissítés után. Részletek issuerid hozzáadására vonatkozó jogcímszabály, részletek toothis a cikk a [többtartományos támogatás az Azure AD összevonási szolgáltatásához](active-directory-aadconnect-multiple-domains.md).

**Javított problémák:**

* Egyes esetekben az Azure AD Connect telepítése sikertelen lesz, mivel nem toocreate egy helyi szolgáltatás fiók, amelynek jelszó megfelel a jelszóházirend hello szervezet által meghatározott bonyolultsági hello szintjének.
* Megtörtént egy probléma javítása ahol illesztési szabályok vannak nem reevaluated válásakor hello kapcsolódási térbe objektumára egyidejűleg out érhető egy szabály csatlakoztatást, és a hatókör válik egy másik. Ez akkor fordulhat elő, ha két vagy több illesztési szabály, amelynek illesztési feltételek közül mind kölcsönösen kizárják egymást.
* Megtörtént egy probléma javítása vonatkozó bejövő szinkronizálási szabályok (az Azure AD), amelyek nem tartalmaznak illesztési szabályok, ahol nem dolgoznak, ha alacsonyabb sorrend értékeknél illesztési szabályokat tartalmazó telepítve.

**Fejlesztései:**

* A Windows Server 2016 Standard vagy magasabb az Azure AD Connect telepítésének támogatása.
* Támogatja a távoli adatbázis hello SQL Server 2016 az Azure AD Connect használatával.

## <a name="112810"></a>1.1.281.0
Kiadás dátuma: 2016. augusztus

**Javított problémák:**

* Módosítások toosync időköz nem kerül sor, amíg hello következő szinkronizálási ciklusban befejeződése után.
* Az Azure AD Connect varázsló nem fogadja el az Azure AD-fiókot, akiknek a felhasználóneve kezdődik aláhúzásjellel (\_).
* Az Azure AD Connect varázsló sikertelen tooauthenticate hello Azure AD-fiókot, ha hello fiókhoz tartozó jelszó túl sok speciális karaktereket tartalmaz. Hibaüzenet "nem toovalidate hitelesítő adatok. Váratlan hiba történt." adja vissza.
* Átmeneti kiszolgáló eltávolítása az Azure AD-bérlő jelszó-szinkronizálás letiltása, és leállítja a jelszó-szinkronizálás toofail active Server.
* Ritka esetekben a jelszó-szinkronizálás sikertelen lesz, nem tárolt hello felhasználói Jelszókivonat esetén.
* Ha az Azure AD Connect-kiszolgáló az átmeneti környezetű üzemmód engedélyezve van, a jelszóvisszaírás nincs letiltva ideiglenesen.
* Ha a kiszolgáló átmeneti módban nem Azure AD Connect varázsló megjelenítése hello tényleges jelszó-szinkronizálás és a jelszó visszaírási konfigurálása. Azt mindig azt is le van tiltva.
* Konfigurációs módosítások toopassword szinkronizálás és jelszóvisszaíró nem maradnak meg az Azure AD Connect varázsló átmeneti módban kiszolgáló esetén.

**Fejlesztései:**

* Hello Start-ADSyncSyncCycle parancsmag tooindicate frissítve, hogy-e képes toosuccessfully start egy új szinkronizálási ciklusban, vagy nem.
* A hozzáadott hello Stop-ADSyncSyncCycle parancsmag tooterminate szinkronizálási ciklusban és műveletet, amelyek jelenleg folyamatban van.
* Frissített hello Stop-ADSyncScheduler parancsmag tooterminate szinkronizálási ciklusban és műveletet, amelyek jelenleg folyamatban van.
* Konfigurálásakor [címtárbővítmények](active-directory-aadconnectsync-feature-directory-extensions.md) az Azure AD Connect varázsló hello Azure AD-attribútum "Teletex karakterlánc" típusú most is választható.

## <a name="111890"></a>1.1.189.0
Kiadás dátuma: 2016. június

**Javított problémák és fejlesztései:**

* Az Azure AD Connect mostantól a FIPS előírásainak megfelelő kiszolgálóra telepíthető.
  * A jelszó-szinkronizálás, lásd: [jelszó-szinkronizálással és FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips).
* Ha NetBIOS-nevet nem sikerült feloldani toohello FQDN hello Active Directory-összekötőt a rögzített kapcsolatos problémát.

## <a name="111800"></a>1.1.180.0
Kiadás dátuma: 2016. május

**Új szolgáltatások:**

* Figyelmezteti, és ellenőrizze a tartomány, ha az Azure AD Connect futtatása előtt egyáltalán nem nyújt segítséget.
* Támogatása az [Microsoft Cloud Németország](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
* Hello legújabb támogatása [a Microsoft Azure Government felhő](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastruktúra és az új URL-követelményeknek.

**Javított problémák és fejlesztései:**

* A hozzáadott szűrési toohello szinkronizálási szabály szerkesztő toomake azt könnyen toofind szinkronizálási szabályokat.
* Javítja a teljesítményt a kapcsolódási térbe törlésekor.
* Rögzített problémát, ha ugyanazt az objektumot le lett törölve és hozzáadott hello hello azonos futtatása (hívott törlése vagy hozzáadása).
* Letiltott szinkronizálási szabály már nem újból engedélyezi a található objektumok és attribútumok használatát a frissítési vagy könyvtár séma frissítése.

## <a name="111300"></a>1.1.130.0
Kiadás dátuma: 2016. április

**Új szolgáltatások:**

* Támogatása a többértékű attribútumok túl[címtárbővítmények](active-directory-aadconnectsync-feature-directory-extensions.md).
* A további konfigurációs eltéréseket támogatása [automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) toobe figyelembe veendő jogosult a frissítésre.
* Egyes parancsmagjai hozzáadott [egyéni Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Kiadás dátuma: 2016. március

**Javított problémák:**

* Arról, hogy a gyorstelepítés nem használható a Windows Server 2008 (R2 előtti), mert a jelszó-szinkronizálás nem támogatott az operációs rendszer.
* Frissítés a Dirsync szolgáltatásról az egyéni szűrő beállításokkal nem működött a várt módon.
* Tooa újabb verziójára történő frissítés, de nem módosítások toohello konfiguráció, a teljes importálás vagy szinkronizálás nem ütemezhető.

## <a name="111100"></a>1.1.110.0
Kiadás dátuma: 2016. február

**Javított problémák:**

* Frissítés a korábbi kiadásokban nem működik, ha hello telepítési nincs hello alapértelmezett C:\Program Files mappában található.
* Ha telepítette, és törölje a jelet **hello szinkronizálási folyamat indítása** hello telepítővarázsló hello végén futó hello telepítővarázsló másodszor teszik lehetővé a hello Feladatütemező.
* hello ütemező nem várt módon működnek a kiszolgálókon, ahol hello hu-hu dátum/idő formátuma nem használatos. Azt is letiltja `Get-ADSyncScheduler` tooreturn helyes időben.
* Ha telepítette az Azure AD Connect egy korábbi verziója AD FS-sel, hello bejelentkezési lehetőséget és a frissítés, újra hello telepítési varázsló nem futtatható.

## <a name="111050"></a>1.1.105.0
Kiadás dátuma: 2016. február

**Új szolgáltatások:**

* [Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) szolgáltatás expressz beállításokat ügyfelek esetén.
* Hello globális rendszergazda Azure többtényezős hitelesítés és a Privileged Identity Management használatával hello telepítővarázsló támogatása.
  * Tooallow van szüksége a proxy tooalso forgalom toohttps://secure.aadcdn.microsoftonline-p.com engedélyezése, ha a többtényezős hitelesítés használata.
  * A multi-factor Authentication tooproperly munkahelyi szüksége tooadd https://secure.aadcdn.microsoftonline-p.com tooyour megbízható helyek listájához.
* Engedélyezi a hello felhasználói bejelentkezési módszer módosítása a kezdeti telepítés után.
* Engedélyezése [tartomány és szervezeti egységek szűrése](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) hello telepítési varázsló. Ezenkívül lehetővé teszi tooforests csatlakozás, ha tartományok nem állnak rendelkezésre.
* [A Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md) toohello szinkronizálási motor részét.

**A szolgáltatások preview tooGA le:**

* [Eszközvisszaíró](active-directory-aadconnect-feature-device-writeback.md).
* [Címtárbővítmények](active-directory-aadconnectsync-feature-directory-extensions.md).

**Új előzetes verziójú funkciók:**

* hello új alapértelmezett szinkronizálási ciklusban időköz értéke 30 perc. Három óra az toobe használt összes korábbi kiadásokban. Hozzáadja a támogatási toochange hello [Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md) viselkedését.

**Javított problémák:**

* hello ellenőrizze a DNS-tartományok lapon nem mindig ismerte fel a hello tartományok.
* Active Directory összevonási szolgáltatások konfigurálásához tartományi rendszergazdai hitelesítő adatokat kér.
* hello a helyszíni AD-fiókok nem ismeri fel hello telepítővarázslóját, ha egy másik DNS-fa hello gyökértartomány mint a tartományban található.

## <a name="1091310"></a>1.0.9131.0
Kiadás dátuma: 2015. decemberi

**Javított problémák:**

* A jelszó-szinkronizálás nem működik, Active Directory tartományi szolgáltatások (AD DS), de működik a jelszavak módosításakor, ha úgy állítja be a jelszót.
* Ha proxykiszolgálót, hitelesítési tooAzure AD sikertelen lehet a telepítés során, illetve ha a frissítés megszakadt hello konfigurálása lapon.
* A teljes SQL Server-példányhoz az Azure AD Connect korábbi kiadásáról frissítése sikertelen lesz, ha nem egy SQL Server rendszer rendszergazdai (SA).
* Hello "nem tooaccess hello ADSync SQL-adatbázis" hibát frissítése távoli SQL Server az Azure AD Connect korábbi kiadásáról jeleníti meg.

## <a name="1091250"></a>1.0.9125.0
Kiadás dátuma: 2015. November

**Új szolgáltatások:**

* Az AD FS tooAzure AD-megbízhatóság olyan módon konfigurálhatja újra.
* Hello Active Directory-sémát frissítheti és szinkronizálási szabályok generálni.
* Letilthatja a szinkronizálási szabály.
* "AuthoritativeNull" adhatja meg a szinkronizálási szabályoknak új literálként.

**Új előzetes verziójú funkciók:**

* [Az Azure AD Connect Health szinkronizálási szolgáltatás](../connect-health/active-directory-aadconnect-health-sync.md).
* Támogatja az [Azure AD tartományi szolgáltatások](../active-directory-passwords-update-your-own-password.md) jelszó-szinkronizálás.

**Új támogatott forgatókönyv szerint:**

* Több helyszíni Exchange-szervezet támogatja. További információkért lásd: [hibrid telepítések esetén több Active Directory-erdő](https://technet.microsoft.com/library/jj873754.aspx).

**Javított problémák:**

* Jelszó-szinkronizálás problémák:
  * Egy objektum áthelyezni az out érhető tooin-hatókör nem lesz a jelszó szinkronizálva. Ez magában foglalja a szervezeti egység- és attribútumszűrés.
  * Egy új szervezeti egység tooinclude szinkronban kiválasztása nem követeli meg a teljes jelszó-szinkronizálást.
  * Ha egy letiltott felhasználó engedélyezve hello jelszó nem szinkronizálja.
  * hello jelszó újrapróbálkozási várólista végtelen és hello előző legfeljebb 5000 objektumok toobe kivont el lett távolítva.
* Nem sikerült tooconnect tooActive Directory, az erdő működési szintje Windows Server 2016.
* Nem sikerült toochange hello csoport csoport hello kezdeti telepítés után szűréséhez használt.
* Már nem hoz létre egy új felhasználói profil hello Azure AD Connect kiszolgáló ennek során a jelszó módosítása a jelszóvisszaírás engedélyezése minden felhasználó.
* Nem sikerült toouse hosszú egész értékek szinkronban szabályok hatókörök.
* hello be "eszközök visszaírásához" tiltva maradjon, ha a tartományvezérlő nem érhető el.

## <a name="1086670"></a>1.0.8667.0
Kiadás dátuma: 2015. augusztus

**Új szolgáltatások:**

* az Azure AD Connect telepítővarázsló már hello tooall Windows Server-nyelvekre honosított.
* Támogatja a fiók zárolásának feloldásához, az Azure ad használata esetén.

**Javított problémák:**

* Az Azure AD Connect telepítővarázsló összeomlik, ha egy másik felhasználó továbbra is fennáll, telepítési hello személy, aki az indításakor hello telepítés helyett.
* Ha az Azure AD Connect korábbi eltávolítási szabályszerűen toouninstall az Azure AD Connect szinkronizálása sikertelen, nincs lehetőség tooreinstall.
* Gyors telepítés használatával, ha hello felhasználó nem hello erdő gyökértartományának hello, vagy ha az Active Directory egy nem angol nyelvű verzióját használja az Azure AD Connect nem tud telepíteni.
* Ha hello hello Active Directory felhasználói fiók Tartományneve nem oldható fel, a félrevezető "Sikertelen toocommit hello schema" hibaüzenet jelenik meg.
* Ha az Active Directory-összekötő hello használt hello fiók kívül hello varázsló módosul, a utólagosan hello varázsló hibát jelez.
* Az Azure AD Connect néha tooinstall tartományvezérlőn sikertelen lesz.
* Nem engedélyezi, és tiltsa le a "Az átmeneti környezetű üzemmód", ha kiterjesztési attribútumot hozzá lett adva.
* A jelszóvisszaírás bizonyos konfigurációk hello Active Directory-összekötő rossz jelszó miatt nem sikerül.
* A DirSync nem frissíthető, ha a megkülönböztető név (DN) használatos attribútumszűrés.
* Túlzott CPU-használat jelszóátállítás használata esetén.

**Az eltávolított előzetes verziójú funkciók:**

* hello előzetes verziójú funkciók [felhasználó-visszaírás](active-directory-aadconnect-feature-preview.md#user-writeback) ideiglenesen eltávolították a preview ügyfelek visszajelzései alapján. Lesz hozzáadva később vezettünk megadott visszajelzés hello után.

## <a name="1086410"></a>1.0.8641.0
Kiadás dátuma: 2015. június

**Az Azure AD Connect eredeti kiadását.**

Módosított neve az Azure AD Sync tooAzure AD Connect.

**Új szolgáltatások:**

* [Gyorsbeállítások](active-directory-aadconnect-get-started-express.md) telepítése
* Is [Active Directory összevonási szolgáltatások konfigurálása](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
* Is [frissítés a Dirsync szolgáltatásról](active-directory-aadconnect-dirsync-upgrade-get-started.md)
* [Véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
* Bevezetett [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode)

**Új előzetes verziójú funkciók:**

* [Felhasználók visszaírásához.](active-directory-aadconnect-feature-preview.md#user-writeback)
* [Csoportok visszaírásához](active-directory-aadconnect-feature-preview.md#group-writeback)
* [Eszközök visszaírásához.](active-directory-aadconnect-feature-device-writeback.md)
* [Címtárbővítmények](active-directory-aadconnect-feature-preview.md)

## <a name="104940501"></a>1.0.494.0501
Kiadás dátuma: 2015. május

**Új követelményt:**

* Az Azure AD Sync megköveteli hello .NET-keretrendszer 4.5.1-es verziója toobe telepítve.

**Javított problémák:**

* Az Azure AD jelszóvisszaírást Azure Service Bus kapcsolati hiba miatt sikertelen.

## <a name="104910413"></a>1.0.491.0413
Kiadás dátuma: 2015. áprilisi

**Javított problémák és fejlesztései:**

* Active Directory-összekötő hello nem törli megfelelően feldolgozni Ha hello Lomtár funkció engedélyezve van, és több tartomány hello erdőben vannak.
* importálási műveletek teljesítményének hello javult a hello Azure Active Directory-összekötőt.
* Ha a csoport túllépte hello tagság (alapértelmezés szerint hello érték too50, 000 objektumok), hello csoport törölve lett az Azure Active Directoryban. Hello új viselkedéssel hello csoport nem törlődik, hiba történt, és új csoporttagsági változások nem lesznek exportálva.
* Nem lehet kiépíteni egy új objektumot, ha már van azonos DN hello az előkészített törlési hello kapcsolódási térbe szerepelnek.
* Bizonyos objektumok szinkronizálás során a különbözeti szinkronizálás vannak megjelölve az akkor is, ha nincs változás előkészített hello objektumon.
* Jelszó szinkronizációt is eltávolítja az előnyben részesített hello Tartományvezérlők listájának.
* CSExportAnalyzer bizonyos objektumok állapotokkal problémák vannak.

**Új szolgáltatások:**

* Illesztés most kapcsolódhatnak túl "a" hello MV objektumtípust.

## <a name="104850222"></a>1.0.485.0222
Kiadás dátuma: 2015. február

**Fejlesztései:**

* Továbbfejlesztett importálási teljesítményét.

**Javított problémák:**

* A jelszó-szinkronizálás eleget tegyen attribútumszűrés által használt hello cloudFiltered attribútum. Szűrt objektum már nem a jelszó-szinkronizálás hatókörében van.
* Ritka esetekben, ahol hello topológia kellett sok tartományvezérlőt a jelszó-szinkronizálás nem működik.
* "Leállt-kiszolgáló" hello Azure AD-összekötő az eszközkezelés után importálásakor engedélyezve van az Azure AD/Intune-ban.
* Csatlakozás több tartományt ugyanazon erdő idegen rendszerbiztonsági (FSP) nem egyértelmű illesztési hibát okoz.

## <a name="104751202"></a>1.0.475.1202
Kiadás dátuma: 2014. decemberi

**Új szolgáltatások:**

* Jelszó-szinkronizálás attribútum alapú szűrés mostantól támogatott. További információkért lásd: [jelszó-szinkronizálás szűrése](active-directory-aadconnectsync-configure-filtering.md).
* hello msDS-externaldirectoryobjectid azonosítót attribútum vissza tooActive Directory írása. Ez a szolgáltatás biztosítja az Office 365-alkalmazások támogatását. Az Exchange hibrid telepítés Online és helyszíni postaládákhoz OAuth2 tooaccess használ.

**Rögzített frissítési problémák:**

* Hello kiszolgálón hello bejelentkezési segéd újabb verziója érhető el.
* Egy egyéni telepítési elérési út használt tooinstall Azure AD Sync.
* Érvénytelen egyéni illesztési feltétel blokkok hello frissítés.

**Egyéb javításai:**

* Rögzített hello sablonok Office Pro Plus.
* Rögzített telepítési problémák okozta a felhasználói neveket, amelyek kezdődhet kötőjellel.
* Rögzített irányítás elvesztése szempontjából hello sourceAnchor beállítás még egyszer hello telepítése varázsló futtatásakor.
* A jelszó-szinkronizálás fix ETW nyomkövetést.

## <a name="104701023"></a>1.0.470.1023
Kiadás dátuma: 2014. október

**Új szolgáltatások:**

* Több jelszó-szinkronizálás a helyszíni Active Directory tooAzure AD.
* Honosított telepítési felületnyelvek tooall Windows Server.

**Frissítés a AADSync 1.0-s verziója**

Ha már rendelkezik telepített Azure AD Sync, nincs tootake még abban az esetben, ha bármelyik hello out-of-box szinkronizálási szabályok módosította egy további lépést. Miután frissítette toohello 1.0.470.1023 kiadás, hello szinkronizálási szabályok módosította ismétlődik. Az hello minden módosított szinkronizálási szabály, a következő:

1.  Keresse meg a hello szinkronizálási szabály módosította, és jegyezze fel az hello módosításokat.
* Hello szinkronizálási szabály törlése.
* Keresse meg a hello új szinkronizálási szabály, amely az Azure AD Sync által létrehozott, és ezután alkalmazza újra hello módosításokat.

**Active Directory-fiókot hello engedélyeit**

hello Active Directory-fiókot az Active Directory további engedélyek toobe képes tooread hello jelszókivonatait kell engedélyezni. hello engedélyek toogrant elnevezése "Replikálása változások" és "Replikációs Directory vált összes." Mindkét engedélyekre szükség toobe képes tooread hello kivonatai.

## <a name="104190911"></a>1.0.419.0911
Kiadás dátuma: 2014. szeptember

**Az Azure AD Sync eredeti kiadását.**

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
