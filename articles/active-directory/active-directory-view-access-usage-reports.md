---
title: "A hozzáférési és használati jelentések megtekintése |} Microsoft Docs"
description: "Ismerteti, hogyan betekintést az integritásra és a szervezete címtárát a biztonsági hozzáférési és használati jelentések megtekintése."
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
ms.openlocfilehash: 038ac79ebf61c6429fbf7ca21eefe9414bcfc03a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="view-your-access-and-usage-reports"></a>View your access and usage reports (A hozzáférési és használati jelentések megtekintése)
*Ez a dokumentáció az [Azure Active Directory Reporting-útmutató](active-directory-reporting-guide.md) része.*

Azure Active Directory hozzáférési és használati jelentések segítségével hogy lássák az integritásra és a munkahely címtárában biztonságát. Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat a vizsgálandó, hogy azok megfelelően megtervezheti kockázatok csökkentésének lehetőségeit.

Az Azure felügyeleti portálon, a jelentések szerint vannak kategóriába sorolva a következőképpen:

* Az anomáliadetektálási jelentések – jelenleg található a rendellenes eseményeket tartalmazó bejelentkezés. Célunk, ellenőrizze, hogy tisztában legyen ilyen tevékenység, és lehetővé teszik a tudni győződjön meg arról, hogy az esemény gyanúsnak meghatározása.
* Integrált alkalmazás jelentések – hogyan használja a szervezet a felhőalapú alkalmazások betekintést nyújt. Az Azure Active Directory integrálható a felhőalapú alkalmazások ezer.
* Hibajelentések – adja meg a külső alkalmazásokba fiókok létesítésekor előforduló hibákat.
* Eszköz/sign tevékenységek adatai egy adott felhasználó felhasználóspecifikus jelentések – jeleníti meg.
* Tevékenységi naplóit – tartalmazó belül utolsó 24 órában, legutóbbi 7 nap vagy 30 napnál, valamint csoport tevékenység módosításainak és jelszó alaphelyzetbe állítása és nyilvántartási tevékenység összes naplózott eseményeket rögzíti.

> [!NOTE]
> * Néhány speciális anomáliadetektálási és erőforrás használati jelentések csak érhetők el, ha engedélyezi a [Azure Active Directory Premium](active-directory-get-started-premium.md). Speciális jelentések segítségével hozzáférés a biztonság növelése, reagáljon a lehetséges veszélyforrásokra és eszköz-hozzáférési és az alkalmazás használata analytics eléréséhez.
> * Az Azure Active Directory Prémium és Alapszintű kiadásai az Azure Active Directory világszerte elérhető példányával érhetők el a kínai ügyfelek számára. Az Azure Active Directory Prémium és Alapszintű kiadásai jelenleg nem támogatottak Kínában a 21Vianet által működtetett Microsoft Azure szolgáltatásban. További információkért lépjen velünk kapcsolatba az [Azure Active Directory fórumán](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="reports"></a>Jelentések
| Jelentés | Leírás |
| --- | --- |
| **Rendellenes tevékenységet jelentések** | |
| [Az ismeretlen forrásból származó indított bejelentkezések](active-directory-reporting-sign-ins-from-unknown-sources.md) |Utalhat nyomon követve nélkül bejelentkezni. |
| [Több hiba után indított bejelentkezések](active-directory-reporting-sign-ins-after-multiple-failures.md) |Egy sikeres durva erőszakos támadást is jelezhet. |
| [Több földrajzi területről indított bejelentkezések](active-directory-reporting-sign-ins-from-multiple-geographies.md) |Azt jelezheti, hogy több felhasználó jelentkezik ugyanazt a fiókot. |
| [IP-címekről a gyanús tevékenységeket bejelentkezések](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md) |Azt jelezheti sikeres bejelentkezés tartós behatolás kísérlet után. |
| [Valószínűleg fertőzött eszközökről indított bejelentkezések](active-directory-reporting-sign-ins-from-possibly-infected-devices.md) |Jelentkezzen be a potenciálisan fertőzött eszközökről kísérlet arra utalhat. |
| [Szabálytalan bejelentkezési tevékenység](active-directory-reporting-irregular-sign-in-activity.md) |Azt jelezheti rendellenes minták a felhasználói bejelentkezési eseményeket. |
| [Szokatlan bejelentkezési tevékenység rendelkező felhasználók](active-directory-reporting-users-with-anomalous-sign-in-activity.md) |Azt jelzi, felhasználók, akiknek a fiókja veszélyben. |
| Felhasználók, akiknek kiszivárogtak a hitelesítő adatai |Felhasználók, akiknek kiszivárogtak a hitelesítő adatai |
| **Tevékenység-naplók** | |
| Ellenőrzési jelentés |A címtárban naplózott események |
| Jelszó-visszaállítási tevékenység |Jelszó-átállításra, a szervezetben előforduló részletes áttekintést nyújt. |
| Jelszó-visszaállítási regisztrációs tevékenység |Itt részletes nézet a jelszó alaphelyzetbe állítása a szervezetben előforduló regisztráció. |
| Önkiszolgáló csoportok tevékenységéről |Az összes naplóz csoport önkiszolgáló tevékenység a könyvtárban |
| **Integrált alkalmazások** | |
| Az alkalmazás használatának |Integrálva van a címtár összes SaaS-alkalmazások használatának összegzését biztosít. |
| Tevékenység-kiépítési |A külső alkalmazások fiókok kiépítéséhez kísérletek előzményeit biztosítja. |
| Jelszó-helyettesítő állapota |Részletes áttekintést nyújt az SaaS-alkalmazásokhoz kapcsolódó kulcsváltás állapotának automatikus jelszó. |
| Alkalmazás-kiépítési hibák |Azt jelzi, hogy az hatással lehet a külső alkalmazásokkal való hozzáférést. |
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
<p>Tevékenységjelentések a rendellenes bejelentkezési tevékenységet Office365, Azure felügyeleti portál, Azure AD hozzáférési Panel, Sharepoint online-hoz, a Dynamics CRM Online-hoz és más Microsoft online services gyanús bejelentkezési jelző.</p>

<p>Az összes ezekre a jelentésekre, kivéve a "Több hibát követő bejelentkezések" jelentés is ez a jelző gyanús <i>összevont</i> bejelentkezések a fentebb említett szolgáltatások, függetlenül az összevonás-szolgáltatóként. </p>

<p>Az alábbi jelentések érhetők el: </p><ul>

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
| Feljegyzés az összes naplózott események az elmúlt 24 óra, 7 napja vagy utolsó 30 nap megjelenik. <br /> További információkért lásd: [Azure Active Directory Auditnaplójának jelentési eseményei](active-directory-reporting-audit-events.md) |Directory > Jelentések lap |

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
| A címtár összes tevékenység a felügyelt önkiszolgáló csoportok jeleníti meg. |Directory > felhasználók > <i>felhasználói</i> > eszközök lapon |

![Önkiszolgáló csoportok tevékenységéről](./media/active-directory-view-access-usage-reports/selfServiceGroupsActivity.PNG)

## <a name="integrated-applications-reports"></a>Integrált alkalmazások jelentések
### <a name="application-usage-summary"></a>Alkalmazáshasználat: összegzés
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés használható, ha azt szeretné, hogy a címtárban SaaS-alkalmazások használatát. Ez a jelentés a felhasználók az alkalmazás a hozzáférési panelen kattintott hányszor alapul. |Directory > Jelentések lap |

Ez a jelentés tartalmazza a bejelentkezési modulok *összes* alkalmazás, amely a címtár éri el, beleértve az előre integrált Microsoft-alkalmazáshoz.

Előre integrált Microsoft-alkalmazások közé tartoznak az Office 365, Sharepoint, az Azure felügyeleti portálon és mások számára.

![Alkalmazás használatának összegzése](./media/active-directory-view-access-usage-reports/applicationUsage.PNG)

### <a name="application-usage-detailed"></a>Alkalmazáshasználat: részletes
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés akkor használja, ha meg szeretné tekinteni, mennyi egy adott SaaS-alkalmazás használatban van. Ez a jelentés a felhasználók az alkalmazás a hozzáférési panelen kattintott hányszor alapul. |Directory > Jelentések lap |

### <a name="application-dashboard"></a>Alkalmazás irányítópultja
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés összegző bejelentkezési jelzi az alkalmazásnak a felhasználók a szervezetben, egy adott időszakban modulok. A diagram az irányítópult-oldalon segítségével azonosíthatja a trendeket, hogy az alkalmazás összes használatra. |Directory > alkalmazások > irányítópulton |

## <a name="error-reports"></a>Hibajelentések
### <a name="account-provisioning-errors"></a>Alkalmazás-kiépítési hibák
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ezzel a figyelheti az SaaS-alkalmazásokhoz az Azure Active Directoryba a fiókok a szinkronizálás során előforduló hibákat. |Directory > Jelentések lap |

![Alkalmazás-kiépítési hibák](./media/active-directory-view-access-usage-reports/accountProvisioningErrors.PNG)

## <a name="user-specific-reports"></a>Felhasználó-specifikus jelentések
### <a name="devices"></a>Eszközök
| Leírás | A jelentés helyét |
|:--- |:--- |
| Ez a jelentés akkor használja, ha meg szeretné tekinteni az IP-cím és egy adott felhasználó Azure Active Directory elérésére használt eszközök földrajzi elhelyezkedését. |Directory > felhasználók > <i>felhasználói</i> > eszközök lapon |

### <a name="activity"></a>Tevékenység
| Leírás | A jelentés helyét |
|:--- |:--- |
| A bejelentkezési tevékenység egy felhasználó számára látható. A jelentés tartalmaz információkat, például az alkalmazás be van jelentkezve, a használt eszköz, a IP-cím és a hely. Nem gyűjtünk az előzmények a felhasználók számára, hogy jelentkezzen be egy Microsoft-fiókkal. |Directory > felhasználók > <i>felhasználói</i> > Tevékenység lap |

#### <a name="sign-in-events-included-in-the-user-activity-report"></a>Jelentkezzen be a felhasználói tevékenység jelentésben szereplő események
A felhasználói tevékenység jelentés bejelentkezési események csak bizonyos típusú fog megjelenni.

| Esemény típusa | Tartalmaz? |
| --- | --- |
| Az indított bejelentkezések a [hozzáférési Panel](http://myapps.microsoft.com/) |Igen |
| Az indított bejelentkezések a [Azure felügyeleti portálon](https://manage.windowsazure.com/) |Igen |
| Az indított bejelentkezések a [Microsoft Azure-portálon](https://portal.azure.com/) |Igen |
| Az indított bejelentkezések a [Office 365 portál](http://portal.office.com/) |Igen |
| Natív alkalmazás, például az Outlookhoz indított bejelentkezések (lásd a kivételt alább) |Igen |
| A hozzáférési panelre, például a Salesforce keresztül összevont/kiépített alkalmazásokhoz indított bejelentkezések |Igen |
| A hozzáférési panelre, például a Twitter keresztül jelszó alapú alkalmazásokhoz indított bejelentkezések |Igen |
| Egy egyéni üzleti alkalmazást, amely a címtár hozzá lett adva a indított bejelentkezések |Nincs (hamarosan elérhető) |
| Az Azure AD alkalmazásproxy alkalmazásához, amely hozzá van adva a könyvtár indított bejelentkezések |Nincs (hamarosan elérhető) |

> Megjegyzés: Ebben a jelentésben zaj mennyiségének csökkentése érdekében bejelentkezések által a [Microsoft Online Services bejelentkezési segéd](http://community.office365.com/en-us/w/sso/534.aspx) nem láthatók.
> 
> 

## <a name="things-to-consider-if-you-suspect-security-breach"></a>Megfontolandó szempontok, ha azt gyanítja, biztonsági szabálysértés
Ha azt gyanítja, hogy egy felhasználói fiókot tulajdonítják el, vagy bármilyen gyanús felhasználói tevékenység, amely azt eredményezheti, hogy a biztonsági sérülése directory adatait a felhőben, érdemes lehet figyelembe venni az alábbi műveletek közül:

* Lépjen kapcsolatba a felhasználó a tevékenység ellenőrzése
* A felhasználói jelszó alaphelyzetbe állítását
* [Többtényezős hitelesítés engedélyezése](../multi-factor-authentication/multi-factor-authentication-get-started.md) a fokozott biztonság

## <a name="view-or-download-a-report"></a>Megtekintése vagy jelentés letöltése
1. A klasszikus Azure portálon kattintson **Active Directory**, kattintson a szervezete címtárának nevét, majd **jelentések**.
2. A jelentések lapon kattintson a jelentés megtekintéséhez és/vagy a letölteni kívánt.
   
   > [!NOTE]
   > Ha ez az első alkalommal használja az Azure Active Directory jelentéskészítési funkciót, egy üzenetet, amely részt vesz a láthatja. Ha elfogadja, kattintson a pipa ikonra a folytatáshoz.
   > 
   > 
3. Kattintson a legördülő menü mellett időköz, és válasszon egyet kell használni, amikor ez a jelentés létrehozása a következő alkalommal tartományok:
   
   * Utolsó 24 órában
   * Az elmúlt 7 napban
   * Az elmúlt 30 napban
4. Kattintson a pipa ikonra a jelentés futtatásához.
   * Legfeljebb 1000 esemény jelenik meg a klasszikus Azure portálon.
5. Szükség esetén kattintson **letöltése** a jelentés letöltéséhez offline megtekintése vagy archiválásra szolgál egy tömörített fájl vesszővel tagolt (CSV) formátumban.
   * Legfeljebb 75,000 események fognak szerepelni a letöltött fájlt.
   * Több adat, tekintse meg a [Azure AD jelentéskészítési API](active-directory-reporting-api-getting-started.md).

## <a name="ignore-an-event"></a>Esemény mellőzése
Anomáliabiztonsági jelentéseket tekinti meg, ha azt tapasztalhatja, hogy figyelmen kívül hagyhatja láthatók a kapcsolódó jelentések különféle eseményeket. Figyelmen kívül hagyása egy eseményt, egyszerűen jelölje ki a jelentésben az esemény, és kattintson a **figyelmen kívül hagyása**. A **figyelmen kívül hagyása** gomb véglegesen törli a kijelölt esemény a jelentésből, és csak a licenccel rendelkező globális rendszergazdák használható.

## <a name="automatic-email-notifications"></a>Automatikus e-mail értesítések
A további információ az Azure AD adatszolgáltatási értesítések, tekintse meg [Azure Active Directory jelentéskészítési értesítései](active-directory-reporting-notifications.md).

## <a name="whats-next"></a>A következő lépések
* [Bevezetés a Prémium szintű Azure Active Directory használatába](active-directory-get-started-premium.md)
* [Vállalati arculat megjelenítése a bejelentkezési és a hozzáférési panel oldalon](active-directory-add-company-branding.md)

