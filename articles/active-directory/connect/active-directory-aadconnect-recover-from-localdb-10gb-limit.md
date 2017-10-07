---
title: "Az Azure AD Connect: Hogyan toorecover a 10 GB-os LocalDB korlátozása probléma |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toorecover Azure AD Connect szinkronizálási szolgáltatás LocalDB 10 GB-os beolvasásakor probléma korlátozza."
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 7b8ce6e19b68837639017bb0315eda4b924d525a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-how-toorecover-from-localdb-10-gb-limit"></a>Az Azure AD Connect: Hogyan toorecover LocalDB 10 GB-os korlát
Az Azure AD Connect egy SQL Server adatbázis toostore identitásadatok igényel. Az Azure AD Connect telepítése az SQL Server 2012 Express LocalDB hello alapértelmezett használja, vagy használja a saját teljes SQL. Az SQL Server Express 10 GB-os méretkorláttal rendelkezik. Ha a LocalDB-t használja és eléri a korlátot, az Azure AD Connect szinkronizálási szolgáltatása többé nem indul majd el, vagy nem végzi el megfelelően a szinkronizálást. A cikkben hello helyreállítási lépéseket.

## <a name="symptoms"></a>Probléma
Két közös jelenségek van:

* Az Azure AD Connect szinkronizálási szolgáltatás **fut** , de nem sikerül a toosynchronize *"leállt-adatbázis-lemez-teljes"* hiba.

* Az Azure AD Connect szinkronizálási szolgáltatás **nem toostart van**. Amikor toostart hello szolgáltatást, az nem esemény 6323 és hibaüzenet *"hello kiszolgáló hibába ütközött, mert nincs elég szabad lemezterület az SQL Server."*

## <a name="short-term-recovery-steps"></a>Rövid távú helyreállítási lépések
Ez a témakör hello Azure AD Connect szinkronizálási szolgáltatás tooresume művelet szükséges lépéseket tooreclaim DB helyet. hello lépések az alábbiak:
1. [Hello szinkronizálási szolgáltatás állapotának meghatározása](#determine-the-synchronization-service-status)
2. [Hello adatbázis zsugorítása](#shrink-the-database)
3. [Futtassa az előzményadatok törlése](#delete-run-history-data)
4. [Rövidítse le a futtatási előzményei adatok megőrzési időtartama](#shorten-retention-period-for-run-history-data)

### <a name="determine-hello-synchronization-service-status"></a>Hello szinkronizálási szolgáltatás állapotának meghatározása
Először határozza meg, hogy a hello szinkronizálási szolgáltatás még fut, vagy nem:

1. Jelentkezzen be tooyour Azure AD Connect-kiszolgáló rendszergazdájához.

2. Nyissa meg túl**szolgáltatásvezérlő**.

3. Hello állapotának ellenőrzése **Microsoft Azure AD Sync**.


4. Ha fut, nem állítsa le vagy hello szolgáltatást. Kihagyás [zsugorítás hello adatbázis](#shrink-the-database) . lépés:, és nyissa meg túl[Delete futtatása előzményadatok](#delete-run-history-data) lépés.

5. Ha nem fut, próbálja toostart hello szolgáltatást. Ha hello szolgáltatás sikeresen elindul, hagyja ki [zsugorítás hello adatbázis](#shrink-the-database) . lépés:, és nyissa meg túl[Delete futtatása előzményadatok](#delete-run-history-data) lépés. Ellenkező esetben folytassa [zsugorítás hello adatbázis](#shrink-the-database) lépés.

### <a name="shrink-hello-database"></a>Hello adatbázis zsugorítása
Hello zsugorítási művelet toofree fel elegendő DB terület toostart hello szinkronizálási szolgáltatást használja. Az felszabadítja DB hello adatbázis szóközök eltávolításával. Ez a lépés esetén a legjobb, mert nem garantált, hogy mindig helyreállíthatja terület. További információ a zsugorítási művelet toolearn, olvassa el ebben a cikkben [adatbázis zsugorítása](https://msdn.microsoft.com/library/ms189035.aspx).

> [!IMPORTANT]
> Hagyja ki ezt a lépést, ha hello szinkronizálási szolgáltatás toorun kaphat. Tooshrink hello SQL DB tooincreased töredezettsége toopoor teljesítményére eredményezhet, nem ajánlott.

az Azure AD Connect létrehozott hello adatbázis neve hello **ADSync**. a zsugorítási művelet tooperform, be kell jelentkeznie vagy hello sysadmin vagy hello adatbázis DBO. Az Azure AD Connect telepítése során a következő fiókok hello jogokat kapnak a sysadmin (rendszergazda):
* A helyi rendszergazdák
* hello felhasználói fiók, de a használt toorun az Azure AD Connect telepítés.
* hello Azure AD Connect szinkronizálási szolgáltatás keretében működő hello használt szinkronizálási szolgáltatásfiók.
* hello helyi csoport telepítése során létrehozott ADSyncAdmins.

1. Készítsen biztonsági mentést hello adatbázis másolása **ADSync.mdf** és **ADSync_log.ldf** alatt található fájlok `%ProgramFiles%\program files\Microsoft Azure AD Sync\Data` tooa biztonságos helyen.

2. Indítson el egy új PowerShell-munkamenetet.

3. Keresse meg a toofolder `%ProgramFiles%\Program Files\Microsoft SQL Server\110\Tools\Binn`.

4. Start **sqlcmd** segédprogram hello parancs futtatásával `./SQLCMD.EXE -S “(localdb)\.\ADSync” -U <Username> -P <Password>`, hello egy rendszergazdai hitelesítő adatai használatával, vagy hello adatbázis DBO.

5. tooshrink hello adatbázis, a hello sqlcmd parancssorból (1 >), adja meg `DBCC Shrinkdatabase(ADSync,1);`, utána pedig `GO` hello következő kódsorban.

6. Ha hello művelet sikeres, próbálja meg újra toostart hello szinkronizálási szolgáltatást. Ha hello szinkronizálási szolgáltatás elindítható, nyissa meg túl[Delete futtatása előzményadatok](#delete-run-history-data) lépés. Ha nem, forduljon a támogatási szolgálathoz.

### <a name="delete-run-history-data"></a>Futtassa az előzményadatok törlése
Alapértelmezés szerint az Azure AD Connect megtartja a futtatási előzményei adatok tooseven nap alatt érkezett be. Ebben a lépésben azt törlése hello előzmények tooreclaim DB adatterülethez futtassa, hogy az Azure AD Connect szinkronizálási szolgáltatás is indítsa el újra a szinkronizálással.

1.  Start **Synchronization Service Managert** fog tooSTART → által szinkronizálási szolgáltatást.

2.  Nyissa meg toohello **műveletek** fülre.

3.  A **műveletek**, jelölje be **egyértelmű futtatása**...

4.  Választhat **törölje az összes futtatása** vagy **előtt fut törlése... <date>**  lehetőséget. Ajánlott elindításához futtassa a két napnál régebbi előzmények adatok törlésével. Ha az adatbázis méretét a probléma továbbra is toorun, majd válassza a hello **törölje az összes futtatása** lehetőséget.

### <a name="shorten-retention-period-for-run-history-data"></a>Rövidítse le a futtatási előzményei adatok megőrzési időtartama
Ez a lépés akkor több szinkronizálási ciklust követően hello 10 GB-os korlát probléma rendszert futtató tooreduce hello valószínűségét.

1. Nyisson meg egy új PowerShell-munkamenetet.

2. Futtatás `Get-ADSyncScheduler` és jegyezze fel a hello PurgeRunHistoryInterval tulajdonságot, amelynek hello aktuális megőrzési időszak.

3. Futtatás `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` tooset hello megőrzési időszak tootwo nap. Hello megőrzési időszak szükség szerint módosítsa.

## <a name="long-term-solution--migrate-toofull-sql"></a>Hosszú távú megoldás – áttelepítése toofull SQL
Hello problémát általában előzetes, hogy 10 GB-os adatbázisméret már nincs elegendő-e az Azure AD Connect toosynchronize a helyszíni Active Directory tooAzure AD. Javasoljuk, hogy vált toousing hello teljes SQL server verziója. Az Azure AD Connect meglévő központi LocalDB hello közvetlenül hello adatbázis SQL teljes verziójának hello nem cserélje. Ehelyett egy új Azure AD Connect-kiszolgáló SQL hello teljes verzióját kell telepítenie. Javasoljuk, hogy végezzen mozgó áttelepítés hello új Azure AD Connect-kiszolgáló (az SQL-adatbázis) Ha van telepítve egy átmeneti kiszolgálón, a következő toohello meglévő az Azure AD Connect kiszolgáló (LocalDB). 
* További információk az hogyan tooconfigure távoli SQL az Azure AD Connect, tekintse meg a tooarticle [az Azure AD Connect egyéni telepítési](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom).
* Az Azure AD Connect frissítése mozgó áttelepítési utasításokért tekintse meg a tooarticle [az Azure AD Connect: frissítés a legújabb egy korábbi verziója toohello](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration).

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
