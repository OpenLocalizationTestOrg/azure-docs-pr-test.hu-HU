---
title: "az Azure Security Center javaslatok tooenhance biztonsági aaaUse |} Microsoft Docs"
description: " Ismerje meg, hogyan toouse biztonsági szabályzatok és javaslatok az Azure Security Center toohelp csökkenthető a biztonsági támadások. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: terrylan
ms.openlocfilehash: bd314350a5abfceea3e171f2e1b55afe4549c1b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-security-center-recommendations-tooenhance-security"></a>Használja az Azure Security Center javaslatok tooenhance biztonsági
Jelentős biztonsági esemény hello esélyét csökkentheti a biztonsági házirend konfigurálása, majd az Azure Security Center által nyújtott hello ajánlások megvalósítása. Ez a cikk bemutatja, hogyan toouse biztonsági szabályzatok és javaslatok a Security Center toohelp csökkenthető a biztonsági támadások.

> [!NOTE]
> Ez a cikk épít hello szerepkörök és a Security Center hello bemutatott fogalmakkal [tervezési és műveletek útmutató](security-center-planning-and-operations-guide.md). A folytatás előtt célszerű tooreview hello tervezési útmutató.
>
>

## <a name="managing-security-recommendations"></a>Biztonsági javaslatok kezelése
A biztonsági házirend vezérlőelemek hello megadott előfizetés vagy az erőforrás-csoporton belüli erőforrások ajánljuk, hogy hello csoportját határozza meg. A Security Center házirendek tooyour a vállalat biztonsági követelményeinek megfelelően határozza meg. több, lásd: toolearn [biztonsági házirendek beállítása a Security Center](security-center-policies.md).

Biztonsági házirendek erőforráscsoportok öröklődtek hello előfizetés szintjéről.

![Biztonsági házirend öröklése][1]

Ha egyéni házirendek adott erőforráscsoportban van szüksége, letilthatja öröklési hello erőforráscsoportban. toodisable, öröklési tooUnique be hello biztonsági szabályzat panel, és szabja testre a Security Center bemutató javaslatok hello vezérlők.

Például ha hello SQL adatbázis átlátszó Data Encryption (TDE) házirend nem igénylő munkaterhelések, kapcsolja ki a hello házirend hello előfizetés szintjén, és kapcsolja be csak azoknál a hello erőforráscsoport, ahol szükség-e Erőforráscsoportoknál.

> [!NOTE]
> Erőforráscsoport és az előfizetés-szintű szabályzat közötti ütközés esetén a hello erőforrás szintű szabályzat élvez elsőbbséget.
>
>

A Security Center elemzi az Azure-erőforrások biztonsági állapotának hello. Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, állítsa be a biztonsági házirend hello hello vezérlők meg ajánlásainkat hoz létre. hello javaslatok végigvezetik hello folyamatán hello szükséges biztonsági vezérlők.

A Security Center arra utalnak, Rendszerfrissítések, operációs rendszer konfigurációja aktuális ajánlások hálózati alhálózatok és virtuális gépek (VM), az SQL Database Auditing, SQL-adatbázis TDE, biztonsági csoportok, és a webalkalmazás tűzfalak. A Security Center javaslatait körét legfrissebb hello, lásd: [a Security Center biztonsági javaslatok kezelése](security-center-recommendations.md).

## <a name="scenario"></a>Forgatókönyv
Ez a forgatókönyv bemutatja, hogyan toouse Security Center toohelp csökkentik a jelentős biztonsági incidensek hello esélyét figyelését a Security Center javaslatait és műveletek. hello forgatókönyv használja hello fiktív cég, Contoso, és szerepel a szerepkörök hello Security Center [tervezési és műveletek útmutató](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). hello szerepkörök olyan, személyek és csoportok, amelyek használhatják a Security Center tooperform különböző biztonsági feladatok. hello szerepkörök a következők:

![A forgatókönyv szerepkörök][2]

Contoso része a helyszíni erőforrások tooAzure nemrég telepítettek át. Contoso tooimplement szeretne, és karbantartása, amelyekkel csökkenthető a biztonsági rés tooa biztonsági támadásnak a hello felhőben lévő erőforrások védelmét.

## <a name="recommended-solution"></a>Javasolt megoldás
A megoldás toouse Security Center tooprevent és biztonsági rések észlelése. Contoso tooSecurity központ keresztül az Azure-előfizetéssel rendelkezik. Hello [ingyenes szint](security-center-pricing.md) a Security Center automatikusan engedélyezve van minden Azure-előfizetés és adatgyűjtés engedélyezve van az előfizetésben szereplő összes virtuális.

David a Contoso informatikai biztonsági, konfigurálja a **biztonsági házirend** biztonsági központ használatával. A Security Center elemzi hello Contoso Azure-erőforrások biztonsági állapotát. Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, az alkalmazás létrehozza **javaslatok** hello vezérlők beállított hello biztonsági házirend alapján.

Jeff, a felhő számítási feladatok felelőse, végrehajtási és karbantartása a Contoso-biztonsági házirendek megfelelően védelmet felelős. Jeff figyelheti hello javaslatok hozta létre a Security Center tooapply védelmet. hello javaslatok Jeff végigvezeti hello folyamatán hello szükséges biztonsági vezérlők.

A Jeff tooimplement sorrendjének és karbantartása a védelmet, és kiszűri a biztonsági rések, he kell:

- A Security Center által biztosított biztonsági javaslatok figyelése
- Értékelje ki a biztonsági javaslatok, és döntse el, ha azt kell alkalmazni, vagy hagyja figyelmen kívül
- Biztonsági javaslatok alkalmazása

Most hajtsa végre a Jeff tartozó lépéseket toosee hogyan használja a Security Center javaslatok tooguide őt hello folyamatot, amely vezérlők tooeliminate biztonsági rések konfigurálása.

## <a name="how-tooimplement-this-solution"></a>Hogyan tooimplement ehhez a megoldáshoz
Túl bejelentkezik Jeff[Azure-portálon](https://azure.microsoft.com/features/azure-portal/) és megnyílik a Security Center konzolján hello. A napi tevékenységek megfigyelése részeként Phil ellenőrzi toosee ha vannak a biztonsági javaslatok hello lépések végrehajtásával:

1. Jeff kiválasztja hello **javaslatok** csempe tooopen hello **javaslatok** panelen.
   ![Jelölje be hello javaslatok csempe][3]
2. Jeff ellenőrzi, hogy hello: javaslatok listája. Azt látja, hogy a Security Center megadta-e a prioritási sorrendben szerepel, a legmagasabb prioritású toolowest prioritású javaslatok hello listája. Eldönti, tooaddress egy magas prioritású javaslat hello listán. Kiválasztja **Endpoint Protection telepítése** a hello **javaslatok** panelen.
3. Hello **Endpoint Protection telepítése** ekkor megnyílik a virtuális gépek listájának megjelenítése nélkül kártevőirtó engedélyezve van a panel. Jeff ellenőrzi, hogy hello található virtuális gépek listájára, minden virtuális gép kiválasztása, és ezután kiválasztja **3 virtuális gép telepítése**.
   ![Végpontvédelem telepítése][4]
4. Hello **válassza ki az Endpoint Protection** panel nyílik meg olyan Jeff két kártevőirtó megoldások. Jeff kiválasztja hello **Microsoft Antimalware** megoldás.
5. Hello kártevőirtó megoldás kapcsolatos további információk jelennek meg. Jeff kiválasztja **létrehozása**.
   ![A Microsoft antimalware][5]
6. Jeff hello szükséges konfigurációs beállítások ír hello **telepítése** panelt, és kijelöli **OK**.

[A Microsoft Antimalware](../security/azure-security-antimalware.md) mostantól aktív hello a kijelölt virtuális gépek.

Jeff továbbra is hello magas prioritású virtuális gép és a közepes prioritású virtuális gép javaslatok toomove döntések végrehajtásáról. Jeff hivatkozik hello [biztonsági javaslatok kezelése](security-center-recommendations.md) toounderstand hello követelményekkel, és mindegyiket egy funkciója alkalmazza, ha a következő cikket.

Jeff Tanulja meg, amely [Microsoft biztonsági válasz Center (MSRC)](../security/azure-security-response-center.md) hajt végre, válassza ki a biztonság ellenőrzése hello Azure-hálózatot és az infrastruktúra, és fenyegetést eszközintelligencia és visszaélés panaszokat kapott harmadik felek számára. Ha Jeff biztonsági kapcsolattartási részletes adatokat biztosít a Contoso Azure-előfizetéssel, Microsoft-ügyfelek Contoso Ha hello MSRC észleli, hogy a Contoso ügyféladatok elérése egy törvénybe ütköző vagy jogosulatlan félnek. Most kövesse Jeff hello alkalmazza **adja meg biztonsági kapcsolattartási adatait** javaslat (ajánlást kiegészített közepes a fenti ajánlásokat hello listáját).

1. Jeff kiválasztja **adja meg biztonsági kapcsolattartási adatait** a hello **javaslatok** panel, amely megnyitja hello **adja meg biztonsági kapcsolattartási adatait** panelen.
2. Jeff hello Azure-előfizetés tooprovide kapcsolattartási adatokat a választja ki. Egy második **adja meg biztonsági kapcsolattartási adatait** panel nyílik meg.
   ![Biztonsági adatai][6]
3. A hello második **adja meg biztonsági kapcsolattartási adatait** panelen Jeff kerül:

  - hello biztonsági kapcsolattartási e-mail címek vesszővel (nincs adja meg e-mail címek számának korlátozása toohello)
  - egy biztonsági telefonszám

4. Jeff is hello beállítás bekapcsolása **küldése e-mailt küld kapcsolatos riasztások** kapcsolatos magas súlyosságú riasztások tooreceive e-maileket.
5. Jeff kiválasztja **OK** tooapply hello biztonsági információk tooContoso előfizetés forduljon.

Végezetül Jeff ellenőrzi, hogy hello kis prioritású virtuális gép ajánlás **szervizelje az operációs rendszer biztonsági rések** és megállapítja, hogy ez a javaslat nem alkalmazható. RON toodismiss hello javaslat. Jeff választja ki, amely megjelenik a toohello jobbra, és választják ki hello három pont **leállítási**.
   ![Hagyja figyelmen kívül a javaslat][7]

## <a name="conclusion"></a>Összegzés
A Security Center javaslatait figyelése segíthet a biztonsági rés megszüntetéséhez előtt a támadás akkor fordul elő. Megakadályozhatja, hogy a biztonsági incidensek megvalósításával és fenntartásával a Security Center biztonsági szabályzatokkal védelmet.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png
