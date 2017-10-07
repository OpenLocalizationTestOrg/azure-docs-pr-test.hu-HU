---
title: "gyakorlatilag az SQL Server Azure virtuális gépeken futó aaaManage költségek |} Microsoft Docs"
description: "Ajánlott eljárásokat biztosít hello megfelelő SQL Server virtuális gép kiválasztása árképzési modellt."
services: virtual-machines-windows
documentationcenter: na
author: luisherring
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/18/2017
ms.author: jroth
ms.openlocfilehash: 6c6a4394e95b5a915ea3e7d5965730000d331036
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Útmutató a SQL Server Azure virtuális gépek díjszabása

Ez a témakör az SQL Server Azure virtuális gép árképzési útmutatást. Több lehetőség, amelyek hatással vannak a költség, és fontos toopick hello jobb kép, hogy az üzleti követelményeinek költségek egyensúlyba kerüljön.

## <a name="free-licensed-sql-server-editions"></a>Ingyenes licenccel rendelkező SQL Server kiadásai

Ha azt szeretné, hogy toodevelop, teszteléséhez vagy a koncepció igazolása build, majd szabad licenccel rendelkező hello használata **SQL Server Developer kiadásában**. Jelen kiadásában mindent tartalmaz, az SQL Server Enterprise edition, így használhatja toobuild bármilyen kívánt alkalmazás. Éles környezetben toorun csak nem engedélyezett. SQL Server Developer virtuális gép csak összegű ideiglenes terheléssel a hello hello VM, a nem SQL Server licencelési költségeit.

Ha azt szeretné, hogy egy egyszerűsített munkaterhelés éles környezetben toorun (< 4 mag, < 1 GB memória, < 10 GB/adatbázis), majd használja az ingyenes licenccel rendelkező hello **SQL Server Express edition**. SQL Express virtuális gép csak felszámított hello költségét hello VM, a nem SQL licencelési.

A következő fejlesztési és tesztelési célú, illetve egyszerűsített termelési számítási feladatokhoz is mentheti pénz válasszon, amely megfelel az ilyen terhelések kisebb Virtuálisgép-méretet. hello DS1v2 előfordulhat, hogy az ilyen terhelések érdemes választani.

toocreate SQL Server 2016 Azure virtuális gép valamelyik ezeket a lemezképeket, tekintse meg a következő hivatkozások hello:

- [SQL Server 2016 fejlesztői Azure virtuális Gépen](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016-ARM)
- [SQL Server 2016 Express Azure virtuális gép](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1ExpressWindowsServer2016-ARM)

## <a name="paid-sql-server-editions"></a>Fizetett SQL Server kiadásai

Ha egy nem lightweight éles munkaterhelés, használja a következő SQL Server kiadásai hello egyikét:

| SQL Server Edition | Számítási feladat |
|-----|-----|
| Web | Kis webhelyek |
| Standard | Kis toomedium munkaterhelések |
| Enterprise | Nagy méretű vagy kritikus fontosságú munkaterhelésekhez|

Az SQL Server licencelési ezeknek a kiadásoknak két beállítások toopay rendelkezik: *kell fizetnie, amennyit egy használati* vagy *saját licenc*.

### <a name="pay-per-usage"></a>Használati / kell fizetnie

**Kifizető hello SQL Server licenc / használati** azt jelenti, hogy hello perc költség hello Azure virtuális Gépen futó tartalmaz hello SQL Server licence hello költségét. Látható az hello SQL Server különböző kiadásai (Web, Standard, Enterprise) díjszabása hello hello [Azure virtuális gép árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). hello költsége van hello ugyanazt az SQL Server bármely verziója (2008 R2 too2016). Mivel az SQL Server licencelési általában hello percalapú licencelési költségeit VM magok száma hello függ.

SQL Server hello fizető / használati licencelési ajánlott:

- Ideiglenes vagy rendszeres munkaterhelések. Például, hogy a alkalmazások igényeihez toosupport esemény néhány hónappal minden év, vagy az üzleti elemző hétfőn.
- Ismeretlen élettartama vagy skálája a munkaterhelések. Például egy alkalmazást, amely nincs szükség néhány hónapon belül, vagy több vagy kevesebb számítási teljesítményt, attól függően, hogy igény szerint szükségesek.

SQL Server 2016 Azure virtuális gép egy fizetési / használati rendszerképei toocreate lásd: a következő hivatkozások hello:

- [SQL Server 2016 webes Azure virtuális Gépen](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)
- [SQL Server 2016 szabványos Azure virtuális gép](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)
- [SQL Server 2016 Enterprise Azure virtuális Gépen](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)

> [!IMPORTANT]
> Egy SQL Server virtuális gép létrehozásakor a hello Azure-portálon hello becsült havi költségét hello megjelenő **méret kiválasztása** panel nem tartalmazza az SQL Server licencelési költségeit. Ez az hello önmagában VM hello költségét.
>
> ![Válassza ki a Virtuálisgép-méret panelen](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Hello szabad licenccel rendelkező Express és Developer kiadásában SQL Server ez pedig hello teljes becsült költség. A Web-, Standard és Enterprise, talált hello további SQL licencelési költségeket a hello [Windows virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Hello árképzést ismertető oldalra jelölje ki a cél az SQL Server kiadása.

### <a name="bring-your-own-license-byol"></a>Hozza magával a licencét (BYOL).

**A saját SQL Server licence kapcsolásának License Mobility keresztül**, más néven tooas **BYOL**, azt jelenti, hogy egy meglévő SQL Server mennyiségi licenc használata Software Assurance egy Azure virtuális gép. BYOL használó SQL Server virtuális gép csak felszámított hello költség hello VM, a nem SQL Server licencelési, futó, figyelembe véve, hogy már vásárolt licencek és frissítési garancia mennyiségi licencelési programon keresztül.

Hozása a saját SQL keresztül License Mobility licencelési ajánlott:

- Folyamatos munkaterhelések. Például egy alkalmazást, amelyet a toosupport üzleti műveletek 24 x 7.
- Az ismert élettartamát és a skála munkaterhelések. Például egy alkalmazást, amely a teljes év hello és mely igény szerinti rendelkezik lett előrejelzett lesz szükség.

toouse BYOL SQL Server virtuális gépen, akkor licenccel kell rendelkeznie az SQL Server Standard vagy Enterprise és [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), amely az egyes keresztül szükséges lehetőség [mennyiségi licencelés](https://www.microsoft.com/en-us/download/details.aspx?id=10585) programok és egy nem kötelező vásárolja meg másokkal.  árképzési szint mennyiségi licencelési programok keresztül elérhető hello változik, a szerződés és hello és mennyiség, vagy kötelezettségvállalás tooSQL Server hello típusa alapján. De a szokásos megoldás, mint a saját licenc folyamatos termelési számítási feladatokhoz mihamarabb elérhetővé tenni a következő előnyöket hello:

| BYOL juttatás | Leírás |
|-----|-----|
| **Költségmegtakarítások** | Hozása a saját SQL Server licence költséghatékonyabb megoldás, mint fizető / használata, ha egy munkaterhelés folyamatosan fog futni, SQL Server Standard vagy Enterprise a *10 hónapnál*. |
| **Hosszú távú megtakarítások** | Átlagosan van *30 % évente olcsóbb* toobuy vagy megújítása hello SQL Server licence első három évben. Ezenkívül követően három év, nincs szükség a toorenew hello licenc többé, frissítési garancia csak díja. Ezen a ponton van *200 % olcsóbb*. |
| **Szabad passzív másodlagos replika** | Egy másik előnye, hogy a saját licenc, hello [szabad passzív egy másodlagos másodpéldány a licencelés](https://azure.microsoft.com/pricing/licensing-faq/) egy SQL-kiszolgálón a magas rendelkezésre állás érdekében. Ez kivágása a fele hello licencelési költségeit (pl. használatával az Always On rendelkezésre állási csoportok) magas rendelkezésre állású SQL Server telepítését. hello jogok toorun hello passzív másodlagos szolgáltatáson keresztül hello feladatátvételi kiszolgálók Software Assurance juttatásra. |

SQL Server 2016 Azure virtuális gép egy bring your-saját-licencet rendszerképei toocreate előtagként a "{BYOL}" hello virtuális gépek lásd:

- [SQL Server 2016 Enterprise Azure virtuális Gépen](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [SQL Server 2016 szabványos Azure virtuális gép](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!NOTE]
> Tudassa velünk, 10 napon belül hány fogja használni az Azure SQL Server-licencet. hello hivatkozások toohello előző képeknek utasításokat hogyan toodo ez.

## <a name="avoid-unecessary-costs"></a>Unecessary költségek elkerülése érdekében

Ha a futó alkalmazások és szolgáltatások nem folyamatosan használ, fontolja meg hello inaktív időszakokban hello virtuális gép leállítása. A fizetés használat alapján történik.

Például ha egyszerűen próbálja ki az SQL Server egy Azure virtuális gépen, ne tooincur díjak távozó véletlenül hétig futnak. Egy megoldás toouse hello [automatikus leállítási szolgáltatás](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL virtuális gép autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Automatikus leállítási része által nyújtott hasonló szolgáltatásokat nagyobb mennyiségű [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Az egyéb munkafolyamatok, fontolja meg automatikusan leállítani, és Azure virtuális gépek újraindításával és a parancsfájl-kezelési megoldás, mint [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Leállítás és felszabadítás, a virtuális gép hello csak úgy tooavoid díjakat. Egyszerűen leállítása vagy energiagazdálkodási beállítások tooshut le a virtuális gép hello használata továbbra is terhel használat.

## <a name="next-steps"></a>Következő lépések

Általános Azure díjszabása útmutatásért lásd: [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](../../../billing/billing-getting-started.md).

Hello legújabb virtuális gépek díjszabása, beleértve az SQL Server, lásd: hello [Azure virtuális gép árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Tekintse át a többi SQL Server virtuális gép témakörei [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).
