---
title: "aaaView a hozzáférési és használati jelentések |} Microsoft Docs"
description: "Ismerteti, hogyan tooview hozzáférési és használati jelentések hello adatintegritási és biztonsági a szervezete címtárának toogain betekintést."
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: a074bc4e-cf3f-4ad1-8cc6-4199d2e09ce4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 1c18fd2a327ae8b67f62ce2754f643bdb03514a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-your-access-and-usage-reports"></a>View your access and usage reports (A hozzáférési és használati jelentések megtekintése)
*Ebben a dokumentációban hello része [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).*

Használhatja az Azure Active Directory hozzáférési és használati jelentések toogain láthatósága hello adatintegritási és biztonsági a szervezete címtárát. Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat, hogy azok megfelelően megtervezheti toomitigate kockázatok vizsgálandó.

Hello Azure felügyeleti portálon, a jelentések szerint vannak kategóriába sorolva hello a következő módon:

* Az anomáliadetektálási jelentések – tartalmazó bejelentkezési eseményeket, hogy észleltünk toobe rendellenes. Célunk toomake tud-e a tevékenység, és lehetővé teszik a toobe képes toomake arról, hogy az esemény gyanúsnak meghatározása.
* Integrált alkalmazás jelentések – hogyan használja a szervezet a felhőalapú alkalmazások betekintést nyújt. Az Azure Active Directory integrálható a felhőalapú alkalmazások ezer.
* Hibajelentések – fiókok tooexternal alkalmazások létesítésekor előforduló hibákat adja meg.
* Eszköz/sign tevékenységek adatai egy adott felhasználó felhasználóspecifikus jelentések – jeleníti meg.
* Tevékenységi naplóit – tartalmazó feljegyzés hello belül minden naplózott események az utolsó 24 óra, 7 napja, vagy utolsó 30 nap során, valamint csoport tevékenység módosításainak és jelszó alaphelyzetbe állítása és nyilvántartási tevékenység.

> [!NOTE]
> * Néhány speciális anomáliadetektálási és erőforrás használati jelentések csak érhetők el, ha engedélyezi a [Azure Active Directory Premium](active-directory-get-started-premium.md). Speciális jelentések segítségével toopotential fenyegetések hozzáférés biztonság, növelése válaszol, és kérjen hozzáférési tooanalytics a eszköz-hozzáférési és az alkalmazás használata.
> * Az Azure Active Directory prémium és alapszintű kiadásai érhetők el a kínai ügyfelek számára az Azure Active Directory hello világszerte példányát használja. Az Azure Active Directory prémium és alapszintű kiadásai jelenleg nem támogatottak Kínában a 21Vianet által működtetett hello Microsoft Azure szolgáltatásban. További információkért lépjen velünk kapcsolatba: hello [Azure Active Directory fórumán](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="reports"></a>Jelentések
| Jelentés | Leírás |
| --- | --- |
| **Rendellenes tevékenységet jelentések** | |
| [Az ismeretlen forrásból származó indított bejelentkezések](active-directory-reporting-sign-ins-from-unknown-sources.md) |Azt jelezheti a egy kísérlet toosign nélkül nyomon követve. |
| [Több hiba után indított bejelentkezések](active-directory-reporting-sign-ins-after-multiple-failures.md) |Egy sikeres durva erőszakos támadást is jelezhet. |
| [Több földrajzi területről indított bejelentkezések](active-directory-reporting-sign-ins-from-multiple-geographies.md) |Azt jelezheti, hogy több felhasználó jelentkezik hello azonos fiók. |
| [IP-címekről a gyanús tevékenységeket bejelentkezések](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md) |Azt jelezheti sikeres bejelentkezés tartós behatolás kísérlet után. |
| [Valószínűleg fertőzött eszközökről indított bejelentkezések](active-directory-reporting-sign-ins-from-possibly-infected-devices.md) |Azt jelezheti egy kísérlet toosign a potenciálisan fertőzött eszközökről. |
| [Szabálytalan bejelentkezési tevékenység](active-directory-reporting-irregular-sign-in-activity.md) |Azt jelezheti minták bejelentkezési események rendellenes toousers'. |
| [Szokatlan bejelentkezési tevékenység rendelkező felhasználók](active-directory-reporting-users-with-anomalous-sign-in-activity.md) |Azt jelzi, felhasználók, akiknek a fiókja veszélyben. |
| Felhasználók, akiknek kiszivárogtak a hitelesítő adatai |Felhasználók, akiknek kiszivárogtak a hitelesítő adatai |
| **Tevékenység-naplók** | |
| Ellenőrzési jelentés |A címtárban naplózott események |
| Jelszó-visszaállítási tevékenység |Jelszó-átállításra, a szervezetben előforduló részletes áttekintést nyújt. |
| Jelszó-visszaállítási regisztrációs tevékenység |Itt részletes nézet a jelszó alaphelyzetbe állítása a szervezetben előforduló regisztráció. |
| Önkiszolgáló csoportok tevékenységéről |Egy tevékenység napló tooall biztosít a címtárban önkiszolgáló tevékenység csoport |
| **Integrált alkalmazások** | |
| Az alkalmazás használatának |Integrálva van a címtár összes SaaS-alkalmazások használatának összegzését biztosít. |
| Tevékenység-kiépítési |Lehetővé teszi a kísérletek előzményeit tooprovision fiókok tooexternal. |
| Jelszó-helyettesítő állapota |Részletes áttekintést nyújt az SaaS-alkalmazásokhoz kapcsolódó kulcsváltás állapotának automatikus jelszó. |
| Alkalmazás-kiépítési hibák |Azt jelzi, hogy egy hatás toousers tooexternal alkalmazásokat. |
| **Rights management** | |
| RMS-használat |A Rights Management használatának összegzését tartalmazza |
| Legtöbb aktív RMS-felhasználók |Felsorolja az RMS-védelemmel ellátott fájlok hozzáférő legfelső 1000 aktív felhasználók |
| RMS-eszköz használata |Az RMS-védelemmel ellátott fájlok elérése során használt eszközök listája |
| RMS-kompatibilis alkalmazások használatát |Biztosítja a használati RMS-kompatibilis alkalmazások |

## <a name="report-editions"></a>A jelentés kiadás
| Jelentés | Ingyenes | Basic | Prémium |
| --- | --- | --- | --- |
| **Rendellenes tevékenységet jelentések** | | | |
| Az ismeretlen forrásból származó indított bejelentkezések |✓ |✓ |✓ |
| Több hiba után indított bejelentkezések |✓ |✓ |✓ |
| Több földrajzi területről indított bejelentkezések |✓ |✓ |✓ |
| IP-címekről a gyanús tevékenységeket bejelentkezések | | |✓ |
| Valószínűleg fertőzött eszközökről indított bejelentkezések | | |✓ |
| Szabálytalan bejelentkezési tevékenység | | |✓ |
| Szokatlan bejelentkezési tevékenység rendelkező felhasználók | | |✓ |
| Felhasználók, akiknek kiszivárogtak a hitelesítő adatai | | |✓ |
| **Tevékenység-naplók** | | | |
| Ellenőrzési jelentés |✓ |✓ |✓ |
| Jelszó-visszaállítási tevékenység | | |✓ |
| Jelszó-visszaállítási regisztrációs tevékenység | | |✓ |
| Önkiszolgáló csoportok tevékenységéről | | |✓ |
| **Integrált alkalmazások** | | | |
| Az alkalmazás használatának | | |✓ |
| Tevékenység-kiépítési |✓ |✓ |✓ |
| Jelszó-helyettesítő állapota | | |✓ |
| Alkalmazás-kiépítési hibák |✓ |✓ |✓ |
| **Rights management** | | | |
| RMS-használat | | |Csak RMS |
| Legtöbb aktív RMS-felhasználók | | |Csak RMS |
| RMS-eszköz használata | | |Csak RMS |
| RMS-kompatibilis alkalmazások használatát | | |Csak RMS |

## <a name="anomalous-activity-reports"></a>Rendellenes tevékenységet jelentések
<p>Tevékenységjelentések jelző gyanús tevékenység tooOffice365, Azure felügyeleti portál, Azure AD hozzáférési Panel, Sharepoint online-hoz, a Dynamics CRM Online és más Microsoft online services bejelentkezési bejelentkezés szokatlan hello.</p>

<p>Az összes ezekre a jelentésekre, kivéve a jelentés "Több hibát követő bejelentkezések" hello is ez a jelző gyanús <i>összevont</i> modulok toohello fent említett szolgáltatások, függetlenül attól, hello összevonás-szolgáltatóként aláírásához. </p>

<p>a következő jelentések hello érhetők el: </p><ul>

<li>[Az ismeretlen forrásból származó indított bejelentkezések](active-directory-reporting-sign-ins-from-unknown-sources.md).</li>

<li>[Több hiba után indított bejelentkezések](active-directory-reporting-sign-ins-after-multiple-failures.md).</li>

<li>[Több földrajzi területről indított bejelentkezések](active-directory-reporting-sign-ins-from-multiple-geographies.md).</li>

<li>[IP-címekről a gyanús tevékenységeket bejelentkezések](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md).</li>

<li>[Szabálytalan bejelentkezési tevékenység](active-directory-reporting-irregular-sign-in-activity.md).</li>

<li>[Valószínűleg fertőzött eszközökről indított bejelentkezések](active-directory-reporting-sign-ins-from-possibly-infected-devices.md).</li>

<li>[Felhasználók rendellenes bejelentkezési tevékenységet](active-directory-reporting-users-with-anomalous-sign-in-activity.md).</li>

<li>Felhasználók, akiknek kiszivárogtak a hitelesítő adatai</li></ul>

## <a name="activity-logs"></a>Tevékenységnaplók
### <a name="audit-report"></a>Ellenőrzési jelentés
| Leírás | A jelentés helyét |
|:--- |:--- |
| Feljegyzés az elmúlt 24 óra, 7 napja vagy utolsó 30 nap hello belül minden naplózott eseményeket jeleníti meg. <br /> További információkért lásd: [Azure Active Directory Auditnaplójának jelentési eseményei](active-directory-reporting-audit-events.md) |Directory > Jelentések lap |

![Ellenőrzési jelentés](./media/active-directory-view-access-usage-reports/auditReport.PNG)

### <a name="password-reset-activity"></a>Jelszó-visszaállítási tevékenység
| Leírás | A jelentés helyét |
|:--- |:--- |
| Látható, minden jelszó alaphelyzetbe állítása, amely a szervezet történt kísérlet. |Directory > Jelentések lap |

![Jelszó-visszaállítási tevékenység](./media/active-directory-view-access-usage-reports/passwordResetActivity.PNG)

### <a name="password-reset-registration-activity"></a>Jelszó-visszaállítási regisztrációs tevékenység
| Leírás | A jelentés helyét |
|:--- |:--- |
| Megjeleníti a minden jelszó-átállítási regisztrációk a szervezet történt |Directory > Jelentések lap |

![Jelszó-visszaállítási regisztrációs tevékenység](./media/active-directory-view-access-usage-reports/passwordResetRegistrationActivity.PNG)

### <a name="self-service-groups-activity"></a>Önkiszolgáló csoportok tevékenységéről
| Leírás | A jelentés helyét |
|:--- |:--- |
| A címtár összes tevékenység hello önkiszolgáló kezelt csoportok jeleníti meg. |Directory > felhasználók > <i>felhasználói</i> > eszközök lapon |

![Önkiszolgáló csoportok tevékenységéről](./media/active-directory-view-access-usage-reports/selfServiceGroupsActivity.PNG)

## <a name="integrated-applications-reports"></a>Integrált alkalmazások jelentések
### <a name="application-usage-summary"></a>Alkalmazáshasználat: összegzés
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés esetén célszerű használni toosee használata az összes hello SaaS-alkalmazásokhoz a címtárban. Ez a jelentés hello hány felhasználó kattintott a hozzáférési Panel hello hello alkalmazás alapul. |Directory > Jelentések lap |

Ez a jelentés tartalmaz bejelentkezések túl*összes* alkalmazás, amely a címtár éri el, beleértve az előre integrált Microsoft-alkalmazáshoz.

Előre integrált Microsoft-alkalmazások közé tartoznak az Office 365, a Sharepoint, az hello Azure felügyeleti portálon és mások számára.

![Alkalmazás használatának összegzése](./media/active-directory-view-access-usage-reports/applicationUsage.PNG)

### <a name="application-usage-detailed"></a>Alkalmazáshasználat: részletes
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés használja, ha azt szeretné, hogy toosee mennyi egy adott SaaS-alkalmazás használatban van. Ez a jelentés hello hány felhasználó kattintott a hozzáférési Panel hello hello alkalmazás alapul. |Directory > Jelentések lap |

### <a name="application-dashboard"></a>Alkalmazás irányítópultja
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés azt jelzi, hogy összesített bejelentkezési modulok toohello alkalmazás által a felhasználók a szervezetben, egy adott időszakban. hello diagram hello irányítópult-oldalon segítségével azonosíthatja a trendeket, hogy az alkalmazás összes használatra. |Directory > alkalmazások > irányítópulton |

## <a name="error-reports"></a>Hibajelentések
### <a name="account-provisioning-errors"></a>Alkalmazás-kiépítési hibák
| Leírás | A jelentés helyét |
|:--- |:--- |
| Használja a toomonitor az SaaS-alkalmazások tooAzure Active Directory fiókok hello szinkronizálás során előforduló hibákról. |Directory > Jelentések lap |

![Alkalmazás-kiépítési hibák](./media/active-directory-view-access-usage-reports/accountProvisioningErrors.PNG)

## <a name="user-specific-reports"></a>Felhasználó-specifikus jelentések
### <a name="devices"></a>Eszközök
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés használható, ha toosee hello IP-cím és, hogy egy adott felhasználó tooaccess Azure Active Directory használt eszközök földrajzi elhelyezkedését. |Directory > felhasználók > <i>felhasználói</i> > eszközök lapon |

### <a name="activity"></a>Tevékenység
| Leírás | A jelentés helyét |
|:--- |:--- |
| Hello bejelentkezési tevékenység egy felhasználó számára látható. hello jelentés tartalmaz információkat, például hello alkalmazás be van jelentkezve, eszköz, IP-cím és helyét. Nem gyűjtünk hello előzmények a felhasználók számára, hogy jelentkezzen be egy Microsoft-fiókkal. |Directory > felhasználók > <i>felhasználói</i> > Tevékenység lap |

#### <a name="sign-in-events-included-in-hello-user-activity-report"></a>Jelentkezzen be az esemény (Event) a hello felhasználói tevékenységgel kapcsolatos jelentés
Bejelentkezési események csak bizonyos típusú felhasználó tevékenységgel kapcsolatos jelentés hello fog megjelenni.

| Esemény típusa | Tartalmaz? |
| --- | --- |
| Modulok toohello jelentkezzen [hozzáférési Panel](http://myapps.microsoft.com/) |Igen |
| Modulok toohello jelentkezzen [Azure felügyeleti portálon](https://manage.windowsazure.com/) |Igen |
| Modulok toohello jelentkezzen [Microsoft Azure-portálon](https://portal.azure.com/) |Igen |
| Modulok toohello jelentkezzen [Office 365 portál](http://portal.office.com/) |Igen |
| Modulok tooa natív alkalmazások aláírása, például az Outlookhoz (lásd a kivételt alább) |Igen |
| Modulok tooa összevont/kiépített alkalmazások hello hozzáférési panelre, például a Salesforce keresztül |Igen |
| Modulok tooa jelszó-alapú alkalmazások hello hozzáférési panelre, például a Twitter keresztül |Igen |
| Modulok tooa egyéni üzleti alkalmazások toohello directory hozzáadott |Nincs (hamarosan elérhető) |
| Modulok tooan Azure AD alkalmazásproxy alkalmazások toohello directory hozzáadott |Nincs (hamarosan elérhető) |

> Megjegyzés: Ez a jelentés a zaj tooreduce hello mennyisége bejelentkezések által hello [Microsoft Online Services bejelentkezési segéd](http://community.office365.com/en-us/w/sso/534.aspx) nem láthatók.
> 
> 

## <a name="things-tooconsider-if-you-suspect-security-breach"></a>Ha azt gyanítja, hogy a biztonsági szabálysértés a dolgot tooconsider
Ha azt gyanítja, hogy egy felhasználói fiókot tulajdonítják el, vagy bármilyen gyanús felhasználói tevékenység, amely a címtáradatok tooa biztonság megsértése hello felhőben vezethet, érdemes lehet tooconsider hello a következő műveletek egyikét:

* Lépjen kapcsolatba a hello felhasználói tooverify hello tevékenység
* Hello felhasználói jelszó alaphelyzetbe állítása
* [Többtényezős hitelesítés engedélyezése](../multi-factor-authentication/multi-factor-authentication-get-started.md) a fokozott biztonság

## <a name="view-or-download-a-report"></a>Megtekintése vagy jelentés letöltése
1. Hello a klasszikus Azure portálon, kattintson **Active Directory**, kattintson a szervezete címtárának hello nevét, majd **jelentések**.
2. Hello jelentések lapján kattintson a kívánt tooview hello jelentést és/vagy töltse le.
   
   > [!NOTE]
   > Ha ez hello első alkalommal használja az Azure Active Directory szolgáltatás reporting hello, megjelenik egy üzenet tooOpt a. Ha elfogadja, kattintson a hello pipa ikon toocontinue.
   > 
   > 
3. Kattintson a legördülő menü következő tooInterval hello, majd válassza ki valamelyik hello idő tartományt kell használni, amikor ez a jelentés létrehozása a következő:
   
   * Utolsó 24 órában
   * Az elmúlt 7 napban
   * Az elmúlt 30 napban
4. Kattintson a hello pipa ikon toorun hello jelentés.
   * Másolatot too1000 események jelenik meg a klasszikus Azure portálon hello.
5. Szükség esetén kattintson **letöltése** toodownload hello jelentés tooa tömörített vesszővel tagolt (CSV) formátumú fájl offline megjelenítése vagy archiválásra szolgál.
   * Másolatot too75 000 események fognak szerepelni hello letöltött fájl.
   * Több adat, tekintse meg a hello [Azure AD jelentéskészítési API](active-directory-reporting-api-getting-started.md).

## <a name="ignore-an-event"></a>Esemény mellőzése
Anomáliabiztonsági jelentéseket tekinti meg, ha azt tapasztalhatja, hogy figyelmen kívül hagyhatja láthatók a kapcsolódó jelentések különféle eseményeket. egy esemény, tooignore egyszerűen jelölje ki a hello esemény hello jelentésben, és kattintson a **figyelmen kívül hagyása**. Hello **figyelmen kívül hagyása** gomb véglegesen eltávolítja a kijelölt esemény hello hello jelentés, és csak a licenccel rendelkező globális rendszergazdák használható.

## <a name="automatic-email-notifications"></a>Automatikus e-mail értesítések
A további információ az Azure AD adatszolgáltatási értesítések, tekintse meg [Azure Active Directory jelentéskészítési értesítései](active-directory-reporting-notifications.md).

## <a name="whats-next"></a>A következő lépések
* [Bevezetés a Prémium szintű Azure Active Directory használatába](active-directory-get-started-premium.md)
* [Vállalati arculat megjelenítése a tooyour bejelentkezés és a hozzáférési Panel oldalon](active-directory-add-company-branding.md)

