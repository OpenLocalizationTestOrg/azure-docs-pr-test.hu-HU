---
title: "a ClearDB MySql-adatbázisokat az Azure App Service aaaFAQ |} Microsoft Docs"
description: "Kapcsolatos kérdésekre ad toocommon ClearDB MySQL-adatbázisokat az Azure App Service segítségével."
documentationcenter: php
services: 
author: sunbuild
manager: yochayk
editor: 
tags: mysql
ms.assetid: c2ed5e78-6d7d-4d0c-b7ee-a52ae41ceab8
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: sumuth
ms.openlocfilehash: 3d9c9daca2b845ede8d3a1fdadefa7e668d62dee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>ClearDB MySQL-adatbázisok használata az Azure App Service szolgáltatásban – gyakori kérdések
Ez a GYIK kapcsolatos kérdésekre ad közös használatával, és az Azure Web Apps adatbázisok beszerzési a ClearDB MySQL.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Milyen lehetőségek állnak rendelkezésre az Azure-beli MySQL?
Több lehetőség közül választhat:

* [ClearDB megosztott MySQL-adatbázis](/marketplace/partners/cleardb/databases/)
* [ClearDB MySQL prémium fürtök](/marketplace/partners/cleardb-clusters/cluster/)
* [Egy Azure virtuális Gépen futó MySQL-fürt](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)
* [Egyetlen példány futhat MySQL az Azure virtuális gépen](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

ClearDB egy MySQL szolgáltatást futtató és felügyeli a hello MySQL infrastruktúra meg. Amikor futtatja a saját MySQL-fürt vagy az adatbázis egy Azure virtuális gépen, tooset hello MySQL kiszolgáló rendelkezik, és őrizniük azok javítást frissült.

## <a name="do-i-need-a-credit-card-for-hello-web-app--mysql-template-in-hello-azure-marketplace"></a>Kell hitelkártya hello webes alkalmazás + hello Azure piactér MySQL-sablon?
Ez az előfizetés használ hello típusától függ. Íme néhány gyakran használt előfizetés típusa:

* [Használatalapú fizetés](/offers/ms-azr-0003p/): hitelkártya, valamint a hitelkártya díjfizetéssel fizetős MySQL-adatbázis megvásárolt igényel.
* [Ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/): használja a Microsoft Azure-szolgáltatások, de nem engedélyezi a külső erőforrások beszerzési kreditek tartalmazza. toopurchase harmadik féltől származó szolgáltatással vagy egy fizetős MySQL-adatbázis toouse egy hitelkártyának kell engedélyezni az előfizetés. A Web Apps szabad ClearDB MySQL-adatbázis is létrehozhat.
* [MSDN-előfizetés](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/) és **MSDN Dev teszt használatalapú**: hasonló tooFree próba, az MSDN-előfizetés szükséges toohave a hitelkártya toopurchase egy fizetős ClearDB MySQL megoldást.
* [Nagyvállalati Szerződés (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/): Nagyvállalati ügyfelek számlázása a EA szemben az összes Azure piactéren (külső) vásárolt külön, összevont számlán negyedévenként. Hello pénzügyi kötelezettségvállalást a piactér beszerzésének kívül kell fizetni. Vegye figyelembe, hogy a jelenleg, Azure-tároló nincs elérhető toocustomers Azerbajdzsán, horvát, norvég és Puerto Rico regisztrált. 
* [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): a Web Apps csak szabad ClearDB adatbázist hozhat létre. Létrehozhat szabad ClearDB MySQL-adatbázisok hello száma korlátozva van. Ne feledje, hogy ingyenes adatbázist nem toobe használt termelési web Apps, mivel ez a szolgáltatás csak próbaverzió szól.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-hello-azure-marketplace"></a>Miért kell fizetnem $3.50 a webes alkalmazás + MySQL hello Azure Piactérről származó?
hello alapértelmezett adatbázis-beállítás program Titan $3.50. Az adatbázis létrehozása során nem megmutatjuk, hello költség, és előfordulhat, hogy véletlenül vásárol egy adatbázist, amelyeket nem állt szándékában. Egy olyan módon tooimprove hello felület toofind keressük, de addig ellenőrizni kell az összes a kijelölt árképzési szinteket a web app és az adatbázis való kattintás előtt **létrehozása** és hello erőforrások hello telepítés elindítása.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-toomy-database"></a>MySQL használom a saját Azure virtuális gépen. Csatlakozás az Azure web app toomy adatbázis is?
Igen. A webes alkalmazás tooyour adatbázis mindaddig, amíg az Azure virtuális gép adott távelérési tooyour webes alkalmazás is elérheti. További információkért lásd: [MySQL telepítése virtuális gépen](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Ahol országok, amelyek a ClearDB prémium MySQL-fürt támogatott?
[ClearDB prémium MySQL fürtök](/marketplace/partners/cleardb-clusters/cluster/) érhetők el az összes Azure-régiók világszerte hello India, Ausztrália, Dél-Brazília és Kína kivételével.

## <a name="can-i-create-a-new-cluster-prior-toocreating-a-database-with-cleardb-premium-cluster-solution"></a>Hozható létre egy új fürt előzetes toocreating adatbázis ClearDB prémium fürt megoldással?
Üres ClearDB fürtök létrehozására nem, nem támogatott. hello Azure-portál lehetővé teszi toocreate adatbázisok fürtben, amely előfordulhat, hogy hozzon létre egy új fürt hello ugyanannyi időt vesz igénybe.

## <a name="will-i-be-warned-if-i-try-toodelete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>A rendszer I figyelmezteti Ha toodelete ClearDB MySQL-adatbázis, amely használatban van az alkalmazások valamelyike meg?
Nem, Azure nem figyelmeztet, ha törli a piactér beszerzési, amely az alkalmazás függ.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Mely régiókat hozható létre a ClearDB adatbázist?
Az Azure piactér nincs elérhető toocustomers Azerbajdzsán, horvát, Norvégia vagy Puerto Rico regisztrálva. ClearDB nem érhető el ezeken a területeken.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Milyen tarifacsomag kell kiválasztani a egy éles web app és az adatbázis?
A Web Apps használja a Basic vagy magasabb szintű tarifacsomagban használható. A ClearDB ajánlott Szaturnusz vagy Jupiter terv. Tekintse át a hello szolgáltatások & mindkét egyes tarifacsomagok korlátaival [webalkalmazások](https://azure.microsoft.com/pricing/details/app-service/) és [ClearDB MySQL-adatbázisok](/marketplace/partners/cleardb/databases/) toochoose hello, amelyik megfelel az igényeinek.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-tooanother"></a>Hogyan lehet frissíteni a ClearDB adatbázist egy terv tooanother?
A hello [Azure-portálon](https://portal.azure.com), ClearDB megosztott üzemeltetési adatbázis költenie. Olvassa el ezt [cikk](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) további toolearn. Jelenleg nem támogatjuk frissítés ClearDB prémium fürtök a hello Azure-portálon.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Nem látom az Azure-portálon a ClearDB adatbázist?
Ha a Microsoft Azure Resource Manager használatával ClearDB-adatbázis létrehozása vagy [új Azure-portál](https://portal.azure.com), nem lesz látható a hello [régi Azure Portal](https://manage.windowsazure.com). toowork-kerülheti van toolink az adatbázis manuálisan toohello webalkalmazás. Hasonlóképpen ha hello ClearDB adatbázist létrehozni [régi portál](https://manage.windowsazure.com) nem lesz képes toosee kell az adatbázis a hello [új Azure-portál](https://portal.azure.com). Nincs munkahelyi körül hello utóbbi forgatókönyvhöz van.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Aki tegye I támogatás kérése, amikor az adatbázis nem működik?
Ügyfél [ClearDB támogatási](https://www.cleardb.com/developers/help/support) az esetleges adatbázis-kapcsolatos probléma. Tooprovide előkészíteni az Azure-előfizetés adatainak őket.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Létrehozhatók további felhasználók számára a ClearDB MySQL-adatbázis fürtmegoldás?
Nem. További felhasználók nem hozható létre, de további adatbázisok a ClearDB adatbázist fürtöt hozhat létre.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-tooplanetary-plans-today-on-cleardb-portal"></a>Basic/Pro adatsorozat adatbázisok lehet frissített helyben hasonló tooPlanetary tervek ma a ClearDB portálon?
Igen, adatbázisok lehet alapvető adatsorozat frissítése helyi (alapvető 500 keresztül alapvető 60). Pro adatsorozat lehet frissíteni a helyi (és Pro 1000 Pro 125) kivételével Pro 60. Pro 60 adatbázis frissítése jelenleg nem támogatott. 

## <a name="when-i-migrate-my-resources-from-one-subscription-tooanother-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Ha az erőforrások szeretnék áttelepíteni egy előfizetés tooanother, nem a ClearDB MySQL-adatbázis települnek át is?
Végrehajtásakor erőforrásmigrálás előfizetésekhez, néhány [korlátozások](app-service-web/app-service-move-resources.md) alkalmazni. A ClearDB MySQL-adatbázis egy külső szolgáltatás, és ezért nem települnek át az Azure-előfizetés az áttelepítés során. Ha a nem kezelt hello áttelepítése a MySQL-adatbázis előzetes toomigrating Azure erőforrásokat, a ClearDB MySQL adatbázis le kell tiltani. Manuálisan telepítse át először az adatbázisokat, és utána végezze el az Azure-előfizetés áttelepítése a webalkalmazás. 

## <a name="i-hit-hello-spending-limit-on-my-subscription-i-removed-hello-limit-and-my-app-service-is-online-however-hello-database-is-not-accessible-how-do-i-re-enable-hello-cleardb-database"></a>I elérte az előfizetésben költségkeret hello. Hello korlát eltávolított, és az App Service érhető el, azonban hello adatbázis nem érhető el. Hogyan újra engedélyezhető hello ClearDB adatbázist?
Ügyfél [ClearDB támogatási](https://www.cleardb.com/developers/help/support) toore-enable hello adatbázis. Biztosítani nekik a Azure-előfizetés információkat és az adatbázis nevét.

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-tooan-ea-subscription"></a>A ClearDB adatbázist átvihető hitelkártya előfizetés tooan EA előfizetésből?
Meglévő ClearDB adatbázist hello meglévő előfizetések társított hello hitelkártya használatára. toouse egy EA előfizetés toomigrate tooa új adatbázisként adatok van szüksége:

* Vásárolja meg a Nagyvállalati előfizetéssel rendelkező új ClearDB adatbázist.
* Telepítse át az adatokat tooyour új adatbázist.
* Az alkalmazás toouse hello új adatbázis frissítéséhez.
* Törölje a régi ClearDB adatbázist.

Hozzon létre egy új webalkalmazást MySQL (ClearDB), vagy hozzon létre egy MySQL-adatbázis (ClearDB), a hello előfizetésben határozza meg, hogyan fogja kell fizetnie hello szolgáltatást. Az EA előfizetői azt nem blokkolja hello beszerzési hello külső szolgáltatások például ClearDB hello Azure-portálon. EA előfizetések pénzügyi kötelezettségvállalást a kívül vannak számlázva, és negyedéves és utólag számlázása. hello EA ügyfél fel a fizetési módot, például a hitelkártya toopay bármely harmadik fél piactér szolgáltatások tooset kellene lennie.

## <a name="where-can-i-see-hello-charges-for-cleardb-resources-in-an-ea-subscription"></a>Ahol láthatja hello díjak ClearDB erőforrások egy EA az előfizetést?
A közvetlen EA ügyfelek Azure piactér díjak láthatók a hello vállalati portálon. Ne feledje, hogy piactér vásárlásokat és a fogyasztás kívül pénzügyi kötelezettségvállalást a számlázása számlázása negyedévente, és az elmaradt. EA rendelkeznek toopay közvetlen toohello külső szolgáltatók és az is, például a EA a hitelkártya fizetési metódus engedélyezésével fiók.

Közvetett EA ügyfelek megtalálhatja az Azure piactér előfizetések hello **előfizetések kezelése oldalt** hello vállalati portálon, de a díjszabási oldalát van-e rejtve. Ügyfelek azok LSP piactér költségekkel kapcsolatos segítségért.

Hozzáférés tooAzure piactér külső-szolgáltatások a EA Azure regisztrációs rendszergazdák által kezelhető. Azok tiltsa le, vagy engedélyezze a hozzáférést too3rd fél vásárlás a hello fiókok szakaszban hello vállalati portál Áruházbeli fiókok kezelése és előfizetések hello.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Ki tegye lehet kapcsolatba lépni kérdések kapcsolatban a ClearDB szolgáltatások saját EA az előfizetést?
Ügyfél [vállalati ügyfélszolgálathoz](http://aka.ms/AzureEntSupport) rendelkező tanúsítványinformációit toobilling a EA regisztráció alatt. hello EA Portal támogatási csoport lesz a kérdése megválaszolásában, vagy a probléma megoldása érdekében.

## <a name="more-information"></a>További információ
[Az Azure piactér – gyakori kérdések](/marketplace/faq/)

