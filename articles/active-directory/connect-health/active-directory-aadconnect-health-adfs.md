---
title: az Azure AD Connect Health AD FS-sel aaaUsing |} Microsoft Docs
description: "Ez a hello Azure AD Connect Health-oldal hogyan toomonitor a helyszíni AD FS infrastruktúra."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0cd26e8762be65e09d22e1f113e5165c4f131715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Az AD FS monitorozása az Azure AD Connect Health használatával
dokumentációjának következő hello adott toomonitoring van az Azure AD Connect Health AD FS infrastruktúráját. Az Azure AD Connect (szinkronizálási szolgáltatás) az Azure AD Connect Health használatával történő megfigyelésével kapcsolatos információkat [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md) című témakörben tekintheti meg. Az Active Directory tartományi szolgáltatások az Azure AD Connect Health használatával történő megfigyelésével kapcsolatos információkat pedig a [Using Azure AD Connect Health with AD DS](active-directory-aadconnect-health-adds.md) (Az Azure AD Connect Health használata az AD DS szolgáltatással) című témakörben találja.

## <a name="alerts-for-ad-fs"></a>AD FS-riasztások
hello Azure AD Connect Health-riasztások szakaszban biztosítja, hogy hello aktív riasztások listáját. Minden egyes riasztás vonatkozó információkat, a megoldási lépések és a hivatkozások toorelated dokumentációja tartalmazza.

Kattintson duplán egy aktív vagy megoldott riasztás, tooopen egy új panel további információkat, lépéseket, amelyek tooresolve hello riasztást, és hivatkozások toorelevant dokumentációját. Az elmúlt hello megoldott riasztások esetében az előzményadatokat is megtekintheti.

![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Használatelemzés az AD FS szolgáltatáshoz
Az Azure AD Connect Health használatelemzés elemzi az összevonási kiszolgálók hitelesítési forgalmát hello. Hello használati analytics mezőbe tooopen hello elemzés panel, amely jelzi, hogy több metrikák és a Csoportosítások duplán kattintva.

> [!NOTE]
> Használatelemzés az AD FS toouse, gondoskodnia kell arról, hogy engedélyezve van-e az AD FS-naplózás. További információkért tekintse meg az [AD FS-naplózás engedélyezését](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health/report1.png)

tooselect további metrikákat, adjon meg egy időtartományt, vagy toochange hello csoportosítás, kattintson a jobb gombbal a használatelemzés diagramra hello, és válassza a diagram szerkesztése lehetőséget. Majd adja meg a hello időtartományt, jelöljön ki egy másik metrikát, és hello csoportosítási módosítani. Hello terjesztési hello hitelesítési forgalom különféle "metrikák" alapján tekintheti meg és hello a következő szakaszban ismertetett vonatkozó "csoportosítási szempont" paraméterek használatával mindegyik metrikát csoportosíthatja:

**Metrika: Kérelmek száma összesen** – Az AD FS-kiszolgálók által feldolgozott kérelmek teljes száma.

|Csoportosítási szempont | Mit jelent a hello csoportosítási és miért hasznos? |
| --- | --- |
| Összes | Hello száma az összes AD FS-kiszolgáló által feldolgozott kérelmek teljes számát jeleníti meg.|
| Alkalmazás | Csoportok hello hello célzott függő entitások alapján kérelmek teljes száma. Ez a csoportosítás akkor hasznos, melyik alkalmazás hány százalékát hello teljes forgalmat fogad toounderstand. |
|  Kiszolgáló |Csoportok hello hello kérelmet feldolgozó hello kiszolgálók alapján kérelmek teljes száma. Ez a csoportosítás akkor hasznos toounderstand hello terheléseloszlását hello teljes forgalmat.
| Munkahelyi csatlakoztatás |Csoportok hello kérelmek teljes száma alapján, hogy hamarosan azokat az eszközökön, azok a munkahelyhez csatlakoztatott (ismert). Ez a csoportosítás akkor hasznos toounderstand, ha a erőforrásaihoz ismeretlen toohello identitás-infrastruktúra eszközök férjenek hozzá. |
|  Hitelesítési módszer | Csoportok hello kérelmek teljes száma a hitelesítéshez használt hello hitelesítési módszer alapján. Ez a csoportosítás akkor hasznos toounderstand hello hitelesítési módszereket a hitelesítéshez leggyakrabban használt. Az alábbiakban hello lehetséges hitelesítési módszerek <ol> <li>Integrált Windows-hitelesítés (Windows)</li> <li>Űrlapalapú hitelesítés (Űrlapok)</li> <li>Egyszeri bejelentkezés (SSO)</li> <li>X509 tanúsítványalapú hitelesítés (Tanúsítvány)</li> <br>Hello összevonási kiszolgálók hello kérelmet SSO Cookie-val kap, ha a kérésre (egyszeri bejelentkezés) egyszeri Bejelentkezéses számítanak. Ebben az esetben ha hello cookie érvényes, a hello felhasználói nem kért tooprovide hitelesítő adatokat, és zökkenőmentes hozzáférést toohello alkalmazás lekérdezi. Ez a viselkedés esetében gyakori, ha több, hello összevonási kiszolgálók által védett függő entitással rendelkezik. |
| Hálózati hely | Csoportok hello hello hello felhasználó hálózati helye alapján kérelmek teljes száma. Ez lehet intranet vagy extranet. Ez a csoportosítás akkor hasznos tooknow hello forgalom hány százaléka érkezik hello intranet vagy extranet. |


**Metrika: Összes sikertelen kérelem** -sikertelen hello összevonási szolgáltatás által feldolgozott kérelmek hello teljes száma. (Ez a metrika csak az AD FS for Windows Server 2012 R2 szolgáltatás esetében elérhető)

|Csoportosítási szempont | Mit jelent a hello csoportosítási és miért hasznos? |
| --- | --- |
| Hibatípus | Hello alapján előre meghatározott hibatípusok hibák számát jeleníti meg. Ez a csoportosítás akkor hasznos toounderstand hello gyakori hibatípusokat. <ul><li>Helytelen felhasználónév vagy jelszó: hibák tooincorrect felhasználónév vagy jelszó miatt.</li> <li>"Extranet kizárás": hibák miatt toohello kérelmeket, amelyek az extranetről kizárt felhasználótól érkezett </li><li> "A jelszó lejárt": hibák miatt lejárt a jelszava toousers bejelentkezik.</li><li>"A fiók le van tiltva": hibák miatt toousers naplózási letiltott fiókkal.</li><li>"Device Authentication": hibák miatt toousers hibás tooauthenticate eszköz-hitelesítéssel.</li><li>"Felhasználói tanúsítvány hitelesítéséhez": Érvénytelen tanúsítvány miatt toousers hibás tooauthenticate miatt sikertelen.</li><li>"MFA": többtényezős hitelesítéssel toouser hibás tooauthenticate miatt sikertelen.</li><li>"Más hitelesítő adatok": "Issuance Authorization": tooauthorization hibák miatt sikertelen.</li><li>"Issuance Delegation": tooissuance hibák miatt sikertelen.</li><li>"Token elfogadási": hibák miatt tooADFS elutasítása hello token az egy külső identitásszolgáltatótól.</li><li>"Protokoll": tooprotocol hibák miatt sikertelen.</li><li>„Unknown” (Ismeretlen): Minden más. Bármely egyéb olyan hiba, amelyek nem felelnek meg hello definiált kategóriák.</li> |
| Kiszolgáló | Csoportok hello hibákat hello server alapján. Ez a csoportosítás akkor hasznos toounderstand hello hiba terjesztési kiszolgáló között. Az egyenetlen eloszlás esetleg jelezheti azt, hogy valamelyik kiszolgáló meghibásodott. |
| Hálózati hely | Csoportok hello hibák hello hálózati hely hello kérelmek (intranet vagy extranet) alapján. Ez a csoportosítás akkor hasznos toounderstand hello típusú sikertelen kérelemtípusokat. |
|  Alkalmazás | Csoportok hello hibák hello célzott alkalmazás (függő entitás) alapján. Ez a csoportosítás akkor hasznos toounderstand melyik célzott alkalmazás kapja a legtöbb hibát. |

**Metrika : Felhasználók száma** – A hitelesítést aktívan az AD FS segítségével végző egyedi felhasználók száma

|Csoportosítási szempont | Mit jelent a hello csoportosítási és miért hasznos? |
| --- | --- |
|Összes |Ez a metrika egy adott időszeletben hello összevonási szolgáltatással hello adja a felhasználók átlagos száma. hello felhasználók nincsenek csoportosítva. <br>hello átlag a választott időszelet hello függ. |
| Alkalmazás |Csoportok hello felhasználók átlagos darabszámát hello alapján célzott alkalmazás (függő entitás). Ez a csoportosítás akkor hasznos toounderstand melyik alkalmazás hány felhasználó használja. |

## <a name="performance-monitoring-for-ad-fs"></a>Teljesítményfigyelés az AD FS szolgáltatáshoz
Az Azure AD Connect Health-teljesítményfigyelés a metrikákkal kapcsolatos figyelési információkat biztosít. Hello figyelés bejelölve nyit meg egy új panel hello metrikák részletes tájékoztatást.

![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health/perf1.png)

Hello panel felső hello hello szűrő beállítás kiválasztásával szűrhet server toosee különálló kiszolgálók metrikáit. toochange metrika, kattintson a jobb gombbal a figyelés diagramra a figyelés panel hello hello, és válassza a diagram szerkesztése lehetőséget (vagy select hello diagram szerkesztése gomb). Hello új panelen a megnyitott további metrikák hello legördülő listán válassza ki, és adjon meg egy időtartományt hello teljesítményadatok megtekintéséhez.

## <a name="reports-for-ad-fs"></a>Jelentések az AD FS szolgáltatáshoz
Az Azure AD Connect Health jelentéseket biztosít az AD FS tevékenységére és teljesítményére vonatkozóan. Ezeknek a jelentéseknek a segítségével a rendszergazdák betekintést nyerhetnek az AD FS kiszolgálón zajló tevékenységekbe.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>A felhasználónevet/jelszót leggyakrabban sikertelenül megadó 50 felhasználó
Az AD FS-kiszolgáló sikertelen hitelesítési kérelmek leggyakoribb oka hello egyike érvénytelen hitelesítő adatok, ez azt jelenti, hogy a rossz felhasználónévvel vagy jelszóval rendelkező kérelmek esetében. Általában akkor fordul elő, toousers megfelelő toocomplex jelszavak, elfelejtett jelszó vagy elírás eredménye.

Azonban más okokból is meghiúsulhat kezeli az AD FS-kiszolgálók, például a kérelem váratlan számú: egy alkalmazás gyorsítótárak felhasználói hitelesítő adatokat és hello hitelesítő adatok lejárnak, vagy egy rosszindulatú felhasználó toosign kísérlet egy figyelembe több jól ismert jelszavakat. E két többek között az okból lehet, hogy a kérelmekben tooa megugrását vezethet.

Az Azure AD Connect Health AD FS készít egy jelentést tooinvalid felhasználónév vagy jelszó miatt sikertelen bejelentkezési kísérlet az 50 legfontosabb felhasználókról. Ez a jelentés megvalósítani hello farmok kiszolgálóinak hello AD FS által generált hello naplózási események feldolgozása

![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health-adfs/report1a.png)

A jelentésen belül könnyen elérhetők toohello információt a következő közül választhat:

* Elmúlt 30 napban hello rossz felhasználónévvel/jelszóval rendelkező meghiúsult kérelmek teljes száma
* A rossz felhasználónévvel/jelszóval sikertelenül bejelentkezni próbáló felhasználók átlagos száma naponta.

Ez a kijelző kattintva viszi toohello fő jelentéspanel, amelyen további részletek. Ezen a panelen egy diagramon a trendek toohelp meghatározásához rossz felhasználónévvel vagy jelszóval kérelmekkel kapcsolatos adatokat tartalmazza. Emellett tartalmazza az 50 legfontosabb felhasználók hello listáját a hello sikertelen bejelentkezési kísérletek számát.

hello graph hello a következő információkat biztosítja:

* hello tooa rossz felhasználónév/jelszó napi szinten miatt meghiúsult bejelentkezések teljes száma.
* hello meghiúsult egy napi szinten bejelentkezéseket megkísérlő egyedi felhasználók teljes száma.
* A legutóbbi kérelmet leadó ügyfél IP-címe

![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health-adfs/report3a.png)

a jelentésben a hello hello a következő információkat:

| Jelentéselem | Leírás |
| --- | --- |
| Felhasználóazonosító |Használt hello Felhasználóazonosítót mutatja. Ezt az értéket a rendszer milyen hello adta-e, felhasználó, ami bizonyos esetekben az hello használt rossz Felhasználóazonosítót. |
| Sikertelen próbálkozások |Megjeleníti a sikertelen kísérletek teljes száma hello adott felhasználóazonosítóhoz. hello tábla hello legtöbb csökkenő sorrendben a sikertelen kísérletek száma alapján van rendezve. |
| Utolsó sikertelen kísérlet |Hello kapcsolatos legutóbbi hiba bekövetkezése hello időbélyeg megjelenítése |
| Legutóbbi sikertelen kísérlet IP-címe |Hello legújabb helytelen kérelem hello ügyfél IP-címét jeleníti meg. |

> [!NOTE]
> Ez a jelentés automatikusan frissül az két óránként hello idő alatt gyűjtött új információkkal. Ennek eredményeképpen bejelentkezési kísérletek hello utolsó két órán belül nem szerepelhetnek hello jelentésben.
>
>

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Az Azure AD Connect Health-ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Az Azure AD Connect Health műveletei](active-directory-aadconnect-health-operations.md)
* [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md)
* [Az Azure AD Connect Health használata az AD DS szolgáltatással](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)