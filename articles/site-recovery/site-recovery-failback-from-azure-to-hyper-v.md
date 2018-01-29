---
title: "Az Azure Site Recovery a Hyper-v virtuális gépek feladat-visszavétel |} Microsoft Docs"
description: "Az Azure Site Recovery koordinálja a replikálása, feladatátvétele és helyreállítási virtuális gépek és fizikai kiszolgálók. Ismerje meg a feladat-visszavétel az Azure-ból a helyszíni adatközpontban."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: rajanaki
ms.openlocfilehash: fafaf3f55f07741d438a06e58713d57d465b1137
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/28/2017
---
# <a name="failback-in-site-recovery-for-hyper-v-virtual-machines"></a>A Site Recovery szolgáltatásban, a Hyper-V virtuális gépek feladat-visszavétel

Ez a cikk ismerteti, hogyan feladat-visszavételt a virtuális gépek Site Recovery által védett.

## <a name="prerequisites"></a>Előfeltételek
1. Győződjön meg arról, hogy az elsődleges hely VMM-kiszolgáló vagy Hyper-V kiszolgáló csatlakozik-e.
2. Kell elvégezte **véglegesítése** a virtuális gépen.

## <a name="why-is-there-no-button-called-failback"></a>Miért van nevű feladat-visszavétel gomb?
A portál nincs nincs explicit kézmozdulat nevű feladat-visszavételre. Feladat-visszavétel egy lépést, ahol, térjen vissza az elsődleges hely. Definíció a feladatátvétel során, feladatátvételi helyreállítási (Azure) helyről a virtuális gépek primary(on-premises), és akkor feladatátvételi elsődleges biztonsági a helyreállítási virtuális gépek feladat-visszavétel esetén.

Kezdeményezzen feladatátvételt, ha a panel nyújt tájékoztatást a feladat irányát. Irány esetén az Azure-ból a helyszíni, akkor a feladat-visszavételre.

## <a name="why-is-there-only-a-planned-failover-gesture-to-failback"></a>Miért nem csak a tervezett feladatátvétel hitelesítési mód feladat-visszavételi?
Azure magas rendelkezésre állású környezet és a virtuális gépek mindig rendelkezésre állnak. Feladat-visszavétel egy tervezett tevékenység, ha úgy dönt, hogy eltarthat egy kis állásidő, hogy a munkaterhelések indíthatja újra futtatni a helyszíni. Ez a adatvesztés nélküli vár. Ezért csak egy tervezett feladatátvételt kézmozdulat elérhető, kapcsolja ki a virtuális gépeket az Azure-ban, a legutóbbi változtatásokat letölti és gondoskodjon arról, hogy adatvesztés nélküli.

## <a name="do-i-need-a-process-server-in-azure-to-failback-to-hyper-v"></a>A folyamatkiszolgálót a feladat-visszavétel a Hyper-v-hez az Azure-ban van szükségem?
Nem, a folyamat kiszolgálóra szükség, csak akkor, ha VMware virtuális gépek védelmére. További összetevők is telepíthető, ha védelme/feladat-visszavétel a Hyper-v virtuális gépek.

## <a name="initiate-failback"></a>Feladat-visszavétel kezdeményezése
Feladatátvétel az elsődleges, másodlagos helyre a replikált virtuális gépek a Site Recovery által nem védett, és a másodlagos hely most nevében jár el az aktív hely. Ezen eljárások visszaadják feladataikat az eredeti elsődleges hely. Ez az eljárás ismerteti a helyreállítási terv tervezett feladatátvétel futtatása. Másik megoldásként futtathatja egyetlen virtuális gép a feladatátvételt a **virtuális gépek** fülre.

1. Válassza ki **helyreállítási tervek** > *recoveryplan_name*. Kattintson a **feladatátvételi** > **tervezett feladatátvétel**.
2. Az a ** tervezett feladatátvétel erősítse meg ** lapon, válassza ki a forrás- és helyek. Jegyezze fel a feladatátvételi irányát. Ha a feladatátvételt az elsődleges, a várt és az összes virtuális gép Ez az információ csak a másodlagos helyen.
3. Az Azure-ból visszavétele esetén válassza a beállítások **adatszinkronizálás**:

   * **Szinkronizálja az adatokat (szinkronizálás csak a változáskülönbözeteit) feladatátvétel előtt**– ezt a beállítást, azok leállítása nélkül szinkronizálják minimálisra csökkenti a virtuális gépek állásidő. Ez hajtja végre az alábbi lépéseket:
     * 1. fázis: Pillanatfelvételt a virtuális gép az Azure-ban, és másolja azt a helyszíni Hyper-V gazdagépen. A számítógép továbbra is fut az Azure-ban.
     * 2. fázis: Leállítja a virtuális gép az Azure-ban, hogy nincs új változások történnek van. A módosítások átkerülnek a helyszíni kiszolgáló és a helyszíni virtuális gép különbözeti végső készletének már elindult.

    - **Szinkronizálja az adatokat csak a feladatátvétel során (teljes letöltésére)**– használja ezt a beállítást, ha Ön már van Azure-on futó hosszú ideig. Ez a beállítás nem gyorsabb, mert elvárjuk, hogy a lemez a legtöbb megváltozott, és nem szeretnénk ellenőrzőösszeg számítása az időt. A lemez letölteni egy hajtja végre. Ez akkor hasznos, ha a helyszíni virtuális gép törölve lett.

    >[!NOTE]
    >Javasoljuk, hogy ezt a beállítást használja, ha már van Azure egy ideig (havonta vagy több) vagy a helyszíni virtuális gép törölve lett. Ez a beállítás nem számításokat bármely ellenőrzőösszeg.


4. Ha engedélyezett az adattitkosítás a felhőben lévő **titkosítási kulcs** válassza ki a tanúsítványt, ha engedélyezte az adattitkosítást a VMM-kiszolgálón a szolgáltató telepítése során ki.
5. Kezdeményezze a feladatátvételt. A feladatátvételi folyamat előrehaladásának követheti a **feladatok** fülre.
6. Ha a feladatátvétel előtt az adatok szinkronizálása lehetőséget választotta, miután a kezdeti szinkronizálása befejeződött, és készen áll a állítsa le a virtuális gépek Azure-ban kattintson a **feladatok** tervezett feladatátvétel feladatnév **Teljes feladatátvétel**. Ez a leállítja a gépet az Azure, átviszi a legutóbbi változtatásokat a helyszíni virtuális gépre, és elindítja a virtuális gép a helyszíni.
7. Most a virtuális gép érvényesíti bejelentkezik érhető el várt módon is.
8. A virtuális gép állapota függőben van. Kattintson a **véglegesítési** a feladatátvétel véglegesítésének.
9. Most kattintson a feladat-visszavétel befejezéséhez **visszirányú replikálása** az elsődleges helyen lévő virtuális gép védelmének megkezdéséhez.

## <a name="failback-to-an-alternate-location"></a>Feladat-visszavétel másik helyre
Ha a védelem közötti telepítése után egy [Hyper-V hely és az Azure](site-recovery-hyper-v-site-to-azure.md) képességét feladat-visszavétel az Azure-ból egy másodlagos helyszíni helyre kell. Ez akkor hasznos, ha új helyszíni hardverelemek kell. Ez mikéntjét.

1. Ha hoz létre új hardver telepítéséhez Windows Server 2012 R2 és a Hyper-V szerepkört a kiszolgálón.
2. Hozzon létre egy virtuális hálózati kapcsoló volt az eredeti kiszolgálón ugyanazzal a névvel.
3. Válassza ki **védett elemek** -> **védelmi csoport**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> feladat-visszavételt, és válassza ki a kívánt **tervezett Feladatátvételi**.
4. A **tervezett feladatátvétel megerősítése** válasszon **létrehozása a helyszíni virtuális gép Ha még nem létezik**.
5. A gazdagép neve ** válassza ki az új Hyper-V gazdakiszolgálón, amelyen el szeretné helyezni a virtuális gép.
6. Az adatok szinkronizálását, azt javasoljuk, azt a lehetőséget választja **szinkronizálja az adatokat a feladatátvétel előtt**. Ez minimalizálja a virtuális gépek állásidő, mivel azok leállítása nélkül szinkronizálják. A következőket teszi:

   * 1. fázis: Pillanatfelvételt a virtuális gép az Azure-ban, és másolja azt a helyszíni Hyper-V gazdagépen. A számítógép továbbra is fut az Azure-ban.
   * 2. fázis: Leállítja a virtuális gép az Azure-ban, hogy nincs új változások történnek van. A végső készlete módosítások átkerülnek a helyszíni kiszolgáló, és a helyszíni virtuális gép már elindult.
7. Kattintson a pipa ikonra, a feladatátvétel (feladat-visszavétel) megkezdéséhez.
8. Miután a kezdeti szinkronizálás befejezését, és készen áll a állítsa le a virtuális gépet az Azure-ban kattintson a **feladatok** > <planned failover job> > **befejezheti a feladatátvételt**. Ez a leállítja a gépet az Azure, átviszi a legutóbbi változtatásokat a helyszíni virtuális gép, és elindítja azt.
9. Ellenőrizze, hogy minden a várt módon működik a helyszíni virtuális géphez jelentkezhetnek be. Kattintson a **véglegesítése** a feladatátvétel befejezéséhez.
10. Kattintson a **visszirányú replikálása** a helyszíni virtuális gép védelmének megkezdéséhez.

    > [!NOTE]
    > A feladat-visszavételi feladaton közben az adatok szinkronizálását lépés megszakítása, a helyszíni virtuális gép egy sérült állapotba kerül. Ennek az az oka adatszinkronizálás másolja át a legfrissebb adatok Azure virtuális gépek lemezei a helyszíni adatok lemezekre, és csak a szinkronizálás befejeződése után, a lemez adatai nem lehet konzisztens állapotú. Ha az On-helyszíni virtuális gép indult adatszinkronizálás megszüntetése után, akkor előfordulhat, hogy nem indulnak el. Az adatszinkronizálás befejezéséhez feladatátvevő újraindítása.

## <a name="time-taken-to-failback"></a>Feladat-visszavételi igénybe vett idő
Az adatszinkronizálás befejezése és a virtuális gép szükséges idő különböző tényezőktől függ. Igénybe vett idő betekintést, biztosítva azt lehet bemutatják, mi történik, adatok szinkronizálása közben.

Az adatszinkronizálás pillanatképet készít a virtuális gépek lemezeit a, és megkezdi az ellenőrzést blokkonként, és kiszámítja az ellenőrzőösszeg. Ez a számított ellenőrzőösszeg való összehasonlításra az azonos blokkhoz a helyszíni ellenőrzőösszeg helyszíni zajlik. Abban az esetben, ha a ellenőrzőösszegeket egyeznek, akkor a adatblokk nem kerül át. Nem felel meg, ha az adatblokk átkerül a helyszínen. Az átviteli időt attól függ, hogy a rendelkezésre álló sávszélességet. Az ellenőrzőösszeg sebességétől néhány GB-ban / perc. 

Hogy felgyorsítsa az adatok letöltése, beállíthatja a MARS agent a letöltés parallelize több szál segítségével. Tekintse meg a [dokumentum itt](https://support.microsoft.com/en-us/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) a letöltési szálat a az ügynök módosításával.


## <a name="next-steps"></a>Következő lépések

Ha a feladat-visszavételi feladaton **véglegesítése** a virtuális gép. Véglegesítési törli a virtuális gépet az Azure és a lemezek, és előkészíti a virtuális Gépet újra kell védeni.

Miután **véglegesítése**, is kezdeményezhető a *visszirányú replikálása*. Ekkor elindul a virtuális gép védelmét a helyszíni vissza az Azure-bA. Ez a művelet csak replikálja a módosításokat, mert a virtuális gép ki van kapcsolva az Azure-ban, és ezért a csak a különbözeti változásokat továbbítja.
