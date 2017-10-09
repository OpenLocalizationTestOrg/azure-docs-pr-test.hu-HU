---
title: "egy Azure Site Recovery segítségével többrétegű Citrix XenDesktop és XenApp telepítési aaaReplicate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooprotect és helyreállítás Citrix XenDesktop és XenApp üzembe helyezése Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: ponatara
ms.openlocfilehash: c4ea9f95f91c585cdcf9d776b02c0967f4c16ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a>Egy Azure Site Recovery segítségével többrétegű Citrix XenApp és XenDesktop telepítési replikálása

## <a name="overview"></a>Áttekintés

Citrix XenDesktop egy olyan asztali virtualizálási megoldás, letölti a asztali környezetek és alkalmazások ondemand szolgáltatás tooany-felhasználóként, bárhonnan. A FlexCast kézbesítési technológia XenDesktop gyorsan és biztonságosan biztosíthat alkalmazások és asztali számítógépek toousers.
Ma, Citrix XenApp nem minden vész helyreállítási képességeket biztosítják.

Egy jó vész-helyreállítási megoldást kell modellezési helyreállítási tervek hello körül fent összetett alkalmazási architektúrákban engedélyezése és hello teszi testre szabott tooadd lépéseket toohandle alkalmazástársítások ezért biztosító különböző rétegek közötti is rendelkeznek egy egy kattintással meg arról, hogy megoldás létrehozása a hello eseményeket, ami tooa katasztrófa RTO csökkentéséhez.

Ez a dokumentum részletes útmutatást a vész-helyreállítási megoldást a helyszíni Hyper-V és a VMware vSphere platformokon Citrix XenApp telepítések készítéséhez. A dokumentum is ismerteti, hogyan tooperform (vész-helyreállítási részletezési) feladatátvételi tesztet, és nem tervezett feladatátvétel tooAzure helyreállítási tervek, hello támogatott konfigurációk és előfeltételek használatával.


## <a name="prerequisites"></a>Előfeltételek

Megkezdése előtt győződjön meg arról, hogy hello következő:

1. [A virtuális gép tooAzure replikálása](site-recovery-vmware-to-azure.md)
1. Hogyan túl[egy helyreállítási hálózathoz. terv](site-recovery-network-design.md)
1. [Ez a teszt feladatátvételi tooAzure](site-recovery-test-failover-to-azure.md)
1. [Ez a feladatátvételi tooAzure](site-recovery-failover.md)
1. Hogyan túl[tartományvezérlő replikálása](site-recovery-active-directory.md)
1. Hogyan túl[SQL-kiszolgáló replikálása](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Központi telepítés minták

Citrix XenApp és XenDesktop farm jellemzően a következő telepítési minta hello rendelkezik:

**Telepítési minta**

Központi Citrix XenApp és XenDesktop AD DNS-kiszolgálóval, az SQL-adatbázis-kiszolgáló, Citrix kézbesítési vezérlő, kirakat kiszolgálót, XenApp főkiszolgáló (VDA), Citrix XenApp licenckiszolgáló

![Telepítési minta 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Webhely-helyreállítási támogatás

Ez a cikk hello célra vSphere 6.0 által felügyelt VMware virtuális gépeken a Citrix központi telepítések / System Center VMM 2012 R2 volt használt toosetup vész-Helyreállítási.

### <a name="source-and-target"></a>Forrása és célja

**A forgatókönyv** | **másodlagos hely tooa** | **tooAzure**
--- | --- | ---
**Hyper-V** | Nincs a hatókörben | Igen
**VMware** | Nincs a hatókörben | Igen
**Fizikai kiszolgáló** | Nincs a hatókörben | Igen

### <a name="versions"></a>Verziók
Futó Hyper-V vagy VMware virtuális gépek vagy fizikai kiszolgálók, ügyfelek XenApp összetevőit is telepítheti. Az Azure Site Recovery mindkét fizikai és virtuális központi telepítések tooAzure védhet.
Az Azure-ban XenApp Észrevételek 7.7 vagy újabb támogatott, mert csak ezen verziói telepítések esetén feladatátvételre tooAzure vész-helyreállítási vagy áttelepítési.

### <a name="things-tookeep-in-mind"></a>Dolgot tookeep szem előtt

1. Védelem és helyreállítás a helyszíni kiszolgáló OS gépek toodeliver használatával XenApp közzétett alkalmazást, és XenApp közzétett asztalok központi telepítések esetén támogatott.

2. Védelmi és helyreállítási a helyszíni központi telepítések ügyfél virtuális asztalok, beleértve a Windows 10 asztali operációs rendszer gépek toodeliver asztali VDI használata nem támogatott. Ennek oka az, így nem támogatja az asztallal OS'es gépek hello helyreállítási.  Emellett néhány ügyfél virtuális asztali tökéletesíthető (pl.. Windows 7) még nem támogatottak a licencelés az Azure-ban. [Itt részletesen tájékozódhat](https://azure.microsoft.com/pricing/licensing-faq/) az ügyfél/kiszolgáló asztali környezeteinek Azure-ban történő licenceléséről.

3.  Az Azure Site Recovery nem replikálja, és védeni a meglévő helyszíni MCS vagy PVS klónok.
Ezek klónok használatával Azure RM kiépítése a kézbesítési vezérlőről kell toorecreate.

4. NetScaler nem látható el védelemmel, Azure Site Recovery segítségével NetScaler freebsd rendszerű alapul, és az Azure Site Recovery nem támogatja az operációs rendszer freebsd rendszerű védelméről. Ehhez toodeploy kell, és az Azure piactéren új NetScaler berendezések konfigurálása után feladatátvételi tooAzure.


## <a name="replicating-virtual-machines"></a>Virtuális gépek replikálása

a következő hello Citrix XenApp telepítési összetevői hello védett toobe tooenable replikációs és helyreállítási van szükség.

* AD DNS-kiszolgáló védelme
* Az SQL server-adatbázis védelme
* Citrix kézbesítési vezérlő védelme
* Kirakat server védelméről.
* Védelmi XenApp főkiszolgáló (VDA)
* Citrix XenApp licenckiszolgáló védelme


**AD DNS-kiszolgáló replikáció**

Tekintse meg a túl[védelme Active Directory és az Azure Site Recovery DNS](site-recovery-active-directory.md) kapcsolatos replikálódik, és a tartományvezérlő beállítása az Azure-ban.

**SQL Server adatbázisreplikáció**

Tekintse meg a túl[SQL Server védelme az SQL Server vészhelyreállítási és az Azure Site Recovery](site-recovery-sql.md) hello részletes műszaki útmutatást ajánlott az SQL kiszolgáló védelmére irányuló lehetőségek.

Hajtsa végre a [Ez az útmutató](site-recovery-vmware-to-azure.md) toostart replikálása hello más összetevő virtuális gépek tooAzure.

![XenApp összetevők védelme](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**Számítási és hálózati beállítások**

Után a védett gépek hello (állapotát jeleníti meg, "Protected" replikált elemek alapján), hello számítási, és meg kell toobe konfigurált hálózati beállításokat.
A számítás és hálózat > számítási tulajdonságok, megadhatja a hello Azure virtuális gép nevét és a cél méretét.
Ha szeretné, módosítsa a hello neve toocomply az Azure-követelményeknek. Is megtekintheti, és adja hozzá a hello célhálózat, alhálózati és toohello Azure virtuális Géphez rendelendő IP-címet.

Vegye figyelembe a következőket hello:

* Beállíthat hello cél IP-címet. Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni. Ha egy címet, amely nem használható feladatátadásra, hello feladatátvétel nem fog működni. cél IP-cím a feladatátvételi teszthez használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.

* Hello AD és a DNS-kiszolgáló esetén hello megőrzése a helyi cím segítségével azonos cím hello hello Azure virtuális hálózat hello DNS-kiszolgálóként adja meg.

hálózati adapterek száma hello hello mérete határozza meg hello cél virtuális gépre, az alábbiak szerint határozza meg:

*   Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.
*   Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet majd hello cél maximális fogja használni.
* Például ha a forrásgépen két hálózati adaptert, és hello célgép mérete négy támogatja, akkor hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.
*   Ha a virtuális gépnek több hálózati adapter hello lesz az összes csatlakoznak toohello ugyanazon a hálózaton.
*   Ha hello virtuális gépnek több hálózati adaptert, majd hello hello listában megjelenő első egy lesz hello alapértelmezett hálózati adapter hello Azure virtuális gép.


## <a name="creating-a-recovery-plan"></a>Helyreállítási terv létrehozása

Után a replikáció hello XenApp összetevő virtuális gépek engedélyezve van, a hello következő lépésre toocreate helyreállítási tervet.
A helyreállítási terv csoportok együtt a virtuális gépek a feladatátvétel és helyreállítás hasonló követelményei.  

**A helyreállítási terv lépéseket toocreate**

1. Helyreállítási terv hello hello XenApp összetevő virtuális gépek hozzáadása.
2. Kattintson a helyreállítási terv -> + helyreállítási terv. Adjon meg a helyreállítási terv hello intuitív nevet.
3. A VMware virtuális gépek: forrás kiválasztása VMware folyamatkiszolgálót, mint a Microsoft Azure cél, és üzembe helyezési modellel, az erőforrás-kezelő, majd kattintson a elemeket választhat ki.
4. A Hyper-V virtuális gépek: válassza ki a forrás VMM-kiszolgálóként, megcélozni a Microsoft Azure és a telepítési modell, erőforrás-kezelő és kattintson a elemeket választhat ki majd hello XenApp telepítési virtuális gépek.

### <a name="adding-virtual-machines-toofailover-groups"></a>Virtuális gépek toofailover csoportok hozzáadása

A helyreállítási terv testreszabott tooadd feladatátvételi csoportok az adott indítási sorrend, parancsfájlok, illetve manuális műveletek is lehet. a következő csoportok hello kell toobe hozzáadott toohello helyreállítási terv.

1. Feladatátvételi csoport1: AD DNS
2. Feladatátvételi csoport2: SQL Server virtuális gépen
2. Feladatátvételi 3: VDA fő kép VM
3. Feladatátvételi 4: Kézbesítési vezérlő és kirakat server virtuális gépen


### <a name="adding-scripts-toohello-recovery-plan"></a>A parancsfájlok toohello helyreállítási terv hozzáadása

Parancsfájlokat lehet futtatni előtt vagy után egy adott csoport a helyreállítási tervet. Manuális műveletek lehet is része és feladatátvétel során.

hello testreszabott helyreállítási tervben az alábbi hello néz ki:

1. Feladatátvételi csoport1: AD DNS
2. Feladatátvételi csoport2: SQL Server virtuális gépen
3. Feladatátvételi 3: VDA fő kép VM

   >[!NOTE]     
   >4, 6, 7 tartalmazó manuális vagy parancsfájl műveletek lépésekre vonatkozó tooonly egy helyszíni XenApp > MCS/PVS katalógusok környezetben.

4. 3. csoport kézi vagy parancsfájl művelet: leállítási fő VDA VM hello átadja a feladatokat tooAzure fő VDA virtuális gép futó állapotban lesz. toocreate új MCS katalógusok Azure ARM használatával, hello fő VDA VM szükség toobe a Leállítva (de lefoglalt) állapotát. Leállítás hello VM Azure portálról.

5. Feladatátvételi 4: Kézbesítési vezérlő és kirakat server virtuális gépen
6. 3 manuális vagy parancsfájl 1 művelet:

    ***Azure RM állomás kapcsolat hozzáadása***

    Hozzon létre Azure ARM-gazdagép kapcsolati kézbesítési vezérlő gép tooprovision új MCS katalógusok az Azure-ban. Hello lépésekkel, ez a [cikk](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).

7. 3 manuális vagy parancsfájl művelet 2:

    ***Hozza létre az MCS katalógusok az Azure-ban***

    az MCS vagy PVS klónok hello elsődleges hely meglévő hello nem lesz replikált tooAzure. Ezek segítségével hello klónok replikálva fő VDA és Azure ARM-kiépítés kézbesítési vezérlőből toorecreate van szüksége. Hello lépésekkel, ez a [cikk](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate MCS katalogizálása az Azure-ban.

![Helyreállítási terv XenApp összetevők](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >A parancsfájlokat [hely](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS az új IP-címek hello feladatátvételt hello > virtuális gépek vagy tooattach hello a terheléselosztó nem sikerült átadó virtuális gép, ha szükséges.


## <a name="doing-a-test-failover"></a>A teszt feladatátvétel

Hajtsa végre a [Ez az útmutató](site-recovery-test-failover-to-azure.md) toodo feladatátvételi tesztet.

![Helyreállítási terv](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>A feladatátvétel

Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) Ha feladatátvételt végez.

## <a name="next-steps"></a>Következő lépések

Is [további](https://aka.ms/citrix-xenapp-xendesktop-with-asr) kapcsolatos Citrix XenApp és XenDesktop a központi telepítések Ez a dokumentum replikálása. Tekintse meg hello útmutatást túl[replikálja más alkalmazások](site-recovery-workload.md) Site Recovery segítségével.
