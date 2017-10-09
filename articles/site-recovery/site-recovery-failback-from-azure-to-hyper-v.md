---
title: "a Hyper-v virtuális gépek az Azure Site Recovery aaaFailback |} Microsoft Docs"
description: "Az Azure Site Recovery koordinálja hello replikációjának, feladatátvételének és helyreállításának virtuális gépek és fizikai kiszolgálók. További információk a feladat-visszavétel a Azure tooon helyszíni adatközpontban."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 50cda9105de6b6fb23e4c62942fdaffc55c3efa4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="failback-in-site-recovery-for-hyper-v-virtual-machines"></a>A Site Recovery szolgáltatásban, a Hyper-V virtuális gépek feladat-visszavétel

Ez a cikk ismerteti, hogyan toofailback virtuális gépek a Site Recovery által védett.

## <a name="prerequisites"></a>Előfeltételek
1. Győződjön meg arról, hogy hello elsődleges hely VMM-kiszolgáló vagy Hyper-V-kiszolgáló csatlakoztatva van.
2. Kell elvégezte **véglegesítése** hello virtuális gépen.

## <a name="why-is-there-no-button-called-failback"></a>Miért van nevű feladat-visszavétel gomb?
Hello-portál nincs nincs explicit kézmozdulat nevű feladat-visszavételre. Feladat-visszavétel egy lépést, ahol, térjen vissza toohello elsődleges hely. Definíció feladatátvevő akkor, ha Ön feladatátvételi hello virtuális gépek primary(on-premises) hely toorecovery (Azure), és a feladat-visszavétel feladatátvételi hello virtuális gépek helyreállítási biztonsági tooprimary.

Kezdeményezzen feladatátvételt, ha az hello panel figyelmeztet hello feladat hello irányát. Ha hello irány Azure tooOn helyszíni, a feladat-visszavétel is.

## <a name="why-is-there-only-a-planned-failover-gesture-toofailback"></a>Miért nem csak egy tervezett feladatátvételt kézmozdulat toofailback?
Azure magas rendelkezésre állású környezet és a virtuális gépek mindig elérhető lesz. Feladat-visszavétel döntheti el, egy kis állásidő tootake, hogy hello munkaterhelések indíthatja újra futtatni a helyszíni egy tervezett tevékenység. Ez a adatvesztés nélküli vár. Ezért csak egy tervezett feladatátvételt kézmozdulat elérhető, kapcsolja ki a hello virtuális gépek Azure-ban, hello legutóbbi módosítások letölti és gondoskodjon arról, hogy adatvesztés nélküli.

## <a name="initiate-failback"></a>Feladat-visszavétel kezdeményezése
Hello elsődleges toosecondary helyről feladatátvétel után a replikált virtuális gépek a Site Recovery által nem védett, és hello aktív helyként hello másodlagos hely jár el. Kövesse az alábbi eljárások toofail hátsó toohello eredeti elsődleges hely. Ez az eljárás ismerteti, hogyan toorun egy tervezett feladatátvételt a helyreállítási terv. Másik megoldásként futtathatja egyetlen virtuális gép feladatátvétele hello hello **virtuális gépek** fülre.

1. Válassza ki **helyreállítási tervek** > *recoveryplan_name*. Kattintson a **feladatátvételi** > **tervezett feladatátvétel**.
2. A hello ** tervezett feladatátvétel erősítse meg ** lapon, válassza ki a hello forrás és cél helyeket. Vegye figyelembe a hello feladatátvételi irányát. Ha hello feladatátvétel az elsődleges, várt és az összes virtuális gép hello másodlagos helye Ez az információ csak.
3. Az Azure-ból visszavétele esetén válassza a beállítások **adatszinkronizálás**:

   * **Szinkronizálja az adatokat (szinkronizálás csak a változáskülönbözeteit) feladatátvétel előtt**– ezt a beállítást, azok leállítása nélkül szinkronizálják minimálisra csökkenti a virtuális gépek állásidő. Ez a következő hello:
     * 1. fázis: Pillanatfelvételt a hello virtuális gépet az Azure-ban, és átmásolja toohello helyszíni Hyper-V gazdagépen. hello gépet továbbra is fut az Azure-ban.
     * 2. fázis: Hello virtuális gépet az Azure-ban leáll, így nem új módosítások hiba fordul elő. a változási különbözeteket hello végső készletének átvitt toohello helyszíni kiszolgáló és hello a helyszíni virtuális gép már elindult.

    - **Szinkronizálja az adatokat csak a feladatátvétel során (teljes letöltésére)**– használja ezt a beállítást, ha Ön már van Azure-on futó hosszú ideig. Ez a beállítás nem gyorsabb, mert elvárjuk, hogy a legtöbb hello lemez megváltozott, és nem szeretnénk ellenőrzőösszeg számítása toospend idő. Egy letöltés hello lemez hajtja végre. Ez akkor hasznos, ha hello helyszíni virtuális gép törölve lett.

    >[!NOTE]
    >Javasoljuk, hogy ezt a beállítást használja, ha már van Azure egy ideig (havonta vagy több) vagy hello helyszíni virtuális gép törölve lett. Ez a beállítás nem számításokat bármely ellenőrzőösszeg.
    >
    >




4. Ha engedélyezett az adattitkosítás a hello felhőszolgáltatásokat, a **titkosítási kulcs** válassza hello kibocsátott tanúsítványt, ha engedélyezte az adattitkosítást hello VMM-kiszolgálón a szolgáltató telepítése során.
5. Kezdeményezze a hello feladatátvételt. Kövesse a hello hello feladatátvételi folyamat előrehaladásának **feladatok** fülre.
6. Ha hello beállítás toosynchronize hello adatok hello feladatátvétel, a kezdeti adatok szinkronizálása befejeződött, és készen áll a tooshut le a virtuális gépek hello Azure, még egyszer hello előtt kijelölt kattintson **feladatok** tervezett feladatátvétel feladat neve **Teljes feladatátvétel**. Ezzel leállítja hello Azure machine átvitelek hello legújabb változik toohello a helyszíni virtuális gép és indítása hello helyszíni virtuális gép.
7. Most már bejelentkezhet alakzatot hello virtuális gép toovalidate érhető el megfelelően.
8. hello virtuális gép állapota függőben van. Kattintson a **véglegesítése** toocommit hello feladatátvételi.
9. Most sorrendben toocomplete hello feladat-visszavétel kattintson **visszirányú replikálása** toostart hello elsődleges helyen hello virtuális gép védelmét.

## <a name="failback-tooan-alternate-location"></a>Feladat-visszavétel tooan másik helyre
Ha a védelem közötti telepítése után a [Hyper-V hely és az Azure](site-recovery-hyper-v-site-to-azure.md) tooability toofailback Azure tooan helyszíni másodlagos helyről van. Ez akkor hasznos, ha tooset fel új helyszíni hardverre van szüksége. Ez mikéntjét.

1. Ha hoz létre új hardver telepíteni a Windows Server 2012 R2 és Hyper-V szerepkör hello kiszolgálón hello.
2. Hozzon létre egy virtuális hálózati kapcsoló hello azonos nevet kellett hello eredeti kiszolgálón.
3. Válassza ki **védett elemek** -> **védelmi csoport**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> toofail vissza szeretné, és válassza ki **tervezett Feladatátvételi**.
4. A **tervezett feladatátvétel megerősítése** válasszon **létrehozása a helyszíni virtuális gép Ha még nem létezik**.
5. A **állomásnév** válasszon hello kívánja tooplace hello virtuális gép új Hyper-V gazdakiszolgálón.
6. Az adatszinkronizálás nem javasoljuk hello beállítást választja **hello feladatátvétel előtt hello adatok szinkronizálása**. Ez minimalizálja a virtuális gépek állásidő, mivel azok leállítása nélkül szinkronizálják. Ez a következő hello:

   * 1. fázis: Pillanatfelvételt a hello virtuális gépet az Azure-ban, és átmásolja toohello helyszíni Hyper-V gazdagépen. hello gépet továbbra is fut az Azure-ban.
   * 2. fázis: Hello virtuális gépet az Azure-ban leáll, így nem új módosítások hiba fordul elő. hello végleges módosítások átvitt toohello helyszíni kiszolgálón, és hello a helyszíni virtuális gép már elindult.
7. Hello pipa toobegin hello feladatátvevő (feladat-visszavétel) gombra.
8. Miután hello kezdeti szinkronizálás befejezését, és készen áll a tooshut le hello virtuális gépet az Azure-ban, kattintson **feladatok** > <planned failover job> > **befejezheti a feladatátvételt**. Ezzel leállítja hello Azure machine, átvitelek hello legutóbbi módosítások toohello helyszíni virtuális gép, és elindítja azt.
9. Bejelentkezhet hello a helyszíni virtuális gép tooverify minden a várt módon működik. Kattintson a **véglegesítése** toofinish hello feladatátvételi.
10. Kattintson a **visszirányú replikálása** toostart hello a helyszíni virtuális gép védelmét.

    > [!NOTE]
    > Ha közben az adatok szinkronizálását lépés megszakítása hello feladat-visszavételi feladaton, hello a helyszíni virtuális gép egy sérült állapotba kerül. Ennek az az oka adatszinkronizálás másol Azure virtuális lemezek toohello helyszíni adatlemezek hello legfrissebb adatokat, és csak hello szinkronizálás befejeződése után, hello lemezadatokat nem konzisztens állapotú. Ha hello helyszíni virtuális gép indult adatszinkronizálás megszüntetése után, akkor esetleg nem indulnak el. Újra indítás, feladatátvétel toocomplete hello adatok szinkronizálását.
    >
    >



## <a name="next-steps"></a>Következő lépések

Ha az hello feladat-visszavételi feladaton, **véglegesítése** hello virtuális gép. Véglegesítési hello Azure virtuális gép és a lemezek törlése, és előkészíti a hello VM toobe védett újra.

Után **véglegesítése**, megkezdheti az hello *visszirányú replikálása*. Elindítja a helyszíni hátsó tooAzure hello virtuális gép védelme. Vegye figyelembe, hogy csak replikálása hello módosítása a művelet csak módosítja a virtuális gép ki van kapcsolva az Azure-ban, és ezért a különbözeti küld hello óta.
