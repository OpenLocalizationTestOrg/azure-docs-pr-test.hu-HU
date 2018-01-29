---
title: "Felderítése és a helyszíni VMware virtuális gépek Azure-bA Azure áttelepítése áttelepítési értékeléséhez |} Microsoft Docs"
description: "Ismerteti, hogyan felderítése és a helyszíni VMware virtuális gépek áttelepítése az Azure-bA az Azure áttelepítése szolgáltatás értékeléséhez."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: a5019d3f729f2efbd01fca021b0089c7f99b0014
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Fedezze fel és felmérheti a helyszíni VMware virtuális gépek áttelepítése az Azure-bA

A [Azure áttelepítése](migrate-overview.md) szolgáltatások értékelésére a helyszíni munkaterhelések Azure való áttelepítésre.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Hozzon létre egy Azure áttelepítése projektet.
> * Egy helyszíni adatgyűjtő virtuális gép (VM) beállítását a helyszíni VMware virtuális gépek a ellenőrzéséhez.
> * Virtuális gépek csoportnak, és hozzon létre egy értékelést.


Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) a virtuális gép létrehozásának megkezdése előtt.


## <a name="prerequisites"></a>Előfeltételek

- **VMware**: A virtuális gépek áttelepítését tervezi, a vCenter Server futó verzióját 5.5, 6.0 vagy 6.5 kell kezelnie. Emellett kell egy ESXi gazdagépen futó verziójával 5.0-s vagy újabb, a gyűjtő virtuális gép központi telepítéséhez. 
 
> [!NOTE]
> Hyper-V támogatása az ütemtervet, és engedélyezve lesz elérhető. 

- **vCenter Server fiók**: a vCenter Server eléréséhez csak olvasható-fiók szükséges. Az Azure áttelepítése ezt a fiókot használja a helyszíni virtuális gépek felderítése.
- **Engedélyek**: a vCenter-kiszolgáló, a virtuális gép létrehozása a fájl importálásával engedélyekre van szükség. PETESEJTEK formátumban. 
- **Statisztika beállítások**: A statisztika a vcenter Server kell beállítás 3. szint telepítés megkezdése előtt. 3 szintje alacsonyabb, ha assessment működni fog, de nem gyűjti a teljesítményadatokat tárolási és hálózati. Javaslatok ebben az esetben kerül sor mérete teljesítményadatokat lemezek és a hálózati adapterek Processzor és memória- és konfigurációs adatok alapján. 

## <a name="log-in-to-the-azure-portal"></a>Bejelentkezés az Azure Portalra
Jelentkezzen be az [Azure portálra](https://portal.azure.com).

## <a name="create-a-project"></a>Projekt létrehozása

1. Az Azure portálon kattintson **hozzon létre egy erőforrást**.
2. Keresse meg **Azure áttelepítése**, és válassza ki a szolgáltatást **Azure áttelepítése (előzetes verzió)** a keresési eredmények között. Ezt követően kattintson a **Create** (Létrehozás) gombra.
3. Adja meg a projekt nevét, és az Azure-előfizetés a projekthez.
4. Hozzon létre egy új erőforráscsoportot.
5. Adja meg a helyet, ahová a projekt létrehozásához, majd kattintson a **létrehozása**. A régióban nyugati középső Régiójában az előzetes verzió csak az Azure áttelepítése projektek hozhat létre. Azonban továbbra is megtervezheti a cél Azure-beli hely az áttelepítéshez. A projekt helyére csak fogja tárolni a metaadatokat, a helyszíni virtuális gépek összegyűjtött. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Töltse le az adatgyűjtő-készülék

Azure áttelepítése létrehoz egy a helyszíni virtuális Gépre, a gyűjtő készülék néven ismert. A virtuális gép deríti fel a helyszíni VMware virtuális gépeket, és elküldi a metaadatokat róluk az Azure áttelepítése szolgáltatáshoz. A gyűjtő készülék beállításához töltse le egy. PETESEJTEK fájlt, és importálja a helyszíni vCenter-kiszolgáló a virtuális gép létrehozásához.

1. Az Azure áttelepítése projektben kattintson **bevezetés** > **felderítési & felmérési** > **gépek felderítése**.
2. A **gépek észlelése**, kattintson a **letöltése**, hogy töltse le a. PETESEJTEK fájlt.
3. A **projekt hitelesítő adatok másolása**, másolja a projekt Azonosítót, és a kulcs. Szükség van ezekre a gyűjtő konfigurálásakor.

    ![.Ova fájl letöltése](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Ellenőrizze az adatgyűjtő-készülék

Ellenőrizze, hogy a. PETESEJTEK fájl biztonságos, csak telepítheti azt.

1. A számítógépen, amelyre a fájlt letöltötte nyissa meg egy rendszergazdai parancsablakot.
2. A következő parancsot a kivonat létrehozásához a petesejtjének:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Példa használati:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. A generált kivonatoló meg kell felelnie, hogy ezeket a beállításokat.
    
    1.0.8.49 petesejtek verziójához
    **Algoritmus** | **Kivonat értéke**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7 
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    A petesejtek verziója 1.0.8.40:

    **Algoritmus** | **Kivonat értéke**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>A gyűjtő virtuális gép létrehozása

A letöltött fájlt importálja a vCenter-kiszolgálóhoz.

1. Kattintson a vSphere Client konzolon **fájl** > **OVF-sablon telepítése**.

    ![OVF telepítése](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. Az OVF-sablon központi telepítése varázslóban > **forrás**, adja meg a .ova fájl helyét.
3. A **neve** és **hely**, adjon meg egy rövid nevet a gyűjtő virtuális gép számára, és a készlet objektum található, amely a virtuális gép üzemelni fog.
5. A **állomás/fürt**, adja meg a gazdagép vagy fürt a gyűjtő virtuális gép elindul.
7. A tároló adja meg a célhelyet, a gyűjtő virtuális gép számára.
8. A **lemezformátum**, adja meg a lemez típusát és méretét.
9. A **Hálózatleképezés**, adja meg a hálózatot, amelyhez a gyűjtő Virtuálisgép kapcsolódik. A hálózati kell internetkapcsolattal, a metaadatok küldése az Azure-bA. 
10. Tekintse át és hagyja jóvá a beállításokat, majd kattintson a **Befejezés**.

## <a name="run-the-collector-to-discover-vms"></a>Futtassa a gyűjtő virtuális gépek felderítése

1. A vSphere Client-konzolon kattintson a jobb gombbal a virtuális gép > **nyissa meg a konzolt**.
2. Adja meg a nyelvet, időzóna és a készülék jelszó beállításait.
3. Az asztalon kattintson a **futtassa a gyűjtő** helyi.
4. Nyissa meg az Azure áttelepítése gyűjtő **előfeltétel**.
    - Fogadja el a licencfeltételeket, és a külső adatokat olvasni.
    - A gyűjtő ellenőrzi, hogy a virtuális gép rendelkezik-e internet-hozzáféréssel.
    - Ha a virtuális gép fér hozzá az internethez olyan proxyn keresztül, kattintson a **proxybeállítások**, és adja meg a proxykiszolgáló címét és a figyelő portja. Adjon meg hitelesítő adatokat, ha a proxy hitelesítést igényel.

    > [!NOTE]
    > A proxykiszolgáló címét meg kell adni az űrlap http://ProxyIPAddress vagy http://ProxyFQDN. Csak a HTTP-proxy használata támogatott.

    - A gyűjtő ellenőrzi, hogy fut-e a collectorservice. A szolgáltatás a gyűjtő virtuális gép alapértelmezés szerint telepítve van.
    - Töltse le és telepítse a VMware PowerCLI.

5. A **adja meg a vCenter-kiszolgáló adatait**, tegye a következőket:
    - Adja meg a név (FQDN) vagy a vCenter-kiszolgáló IP-címét.
    - A **felhasználónév** és **jelszó**, adja meg a csak olvasható fiók hitelesítő adatait, amelyet a gyűjtő virtuális gépek felderítése a vCenter Server fog használni.
    - A **gyűjtemény hatókör**, válassza ki a virtuális gép felderítési hatókörét. A gyűjtő csak a megadott hatókörön belüli virtuális gépek képes felderíteni. Egy adott mappában, a datacenter vagy a fürt állítható be hatókör. Hogy 1000-nél több virtuális gép nem tartalmaz. 

6. A **megadása áttelepítési projekt**, adja meg az Azure áttelepítése projekt Azonosítót, és a portálról másolt kulcsot. Ha nem másolja át a fájlokat, az Azure-portál megnyitása a gyűjtő virtuális gép. A projekt **áttekintése** kattintson **gépek felderítése**, és másolja az értékeket.  
7. A **gyűjtemény folyamatjelző**felderítési figyelje, és győződjön meg arról, hogy a virtuális gépek gyűjtött metaadatai a hatókörben. A gyűjtő megadja egy hozzávetőleges felderítés időtartamát.

> [!NOTE]
> A gyűjtő csak az "Angol (Egyesült Államok)" az operációs rendszer nyelve és a gyűjtő felület nyelvének támogatja. További nyelvek támogatása hamarosan elérhető.


### <a name="verify-vms-in-the-portal"></a>Ellenőrizze a virtuális gépek a portálon

Felderítési idő függ VMs hány derít fel. Általában 100 virtuális gépen is fut a gyűjtő végeztével időt vesz igénybe egy órát a felderítés befejezéséhez körül. 

1. Az áttelepítés Planner projektben kattintson **kezelése** > **gépek**.
2. Ellenőrizze, hogy a portálon jelennek meg a felderíteni kívánt virtuális gépeket.


## <a name="create-and-view-an-assessment"></a>Hozzon létre és értékelés megtekintése

Virtuális gépek később, akkor csoportosításához, és hozzon létre értékelését. 

1. A projekt **áttekintése** kattintson **+ assessment létrehozása**.
2. Kattintson a **összes** assessment tulajdonságainak áttekintése.
3. A csoport létrehozásához, és adjon meg egy felügyeleticsoport-nevet.
4. Válassza ki a gépeket, a csoporthoz hozzáadni kívánt.
5. Kattintson a **létrehozása Assessment**, a csoport és az értékelés létrehozásához.
6. Az értékelés létrehozása után megtekintheti a **áttekintése** > **irányítópult**.
7. Kattintson a **assessment exportálása**, letöltheti az Excel-fájlként.

### <a name="sample-assessment"></a>A minta értékelése

Íme egy példa értékelési jelentést. Az Azure és a becsült havi költségeket kompatibilis csatlakoznak-e virtuális gépek kapcsolatos információkat tartalmaz. 

![Értékelés jelentés](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-kompatibilitás

Ez a nézet megjeleníti az egyes gépek készültségi állapotát.

- Virtuális gépekhez, amely készen áll Azure telepítse át azt javasolja, hogy a virtuális gép méretét, az Azure-ban.
- Virtuális gépekhez, amelyek nem állnak készen Azure áttelepítése bemutatja ennek okát, valamint javítási lépéseket.
- Az Azure áttelepítése az áttelepítés használható eszközök javasol. Ha a számítógép megfelelő-e a növekedési és shift áttelepítési, ajánlott az Azure Site Recovery szolgáltatásban. Ha egy adatbázis-számítógép, ajánlott az Azure-adatbázis áttelepítési szolgáltatás.

  ![Assessment readiness](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Havi költségbecslés

Ez a nézet megjeleníti az összes számítási és tárolási költsége Azure-beli virtuális gépek és az egyes gépek részletei. Becsült költség kiszámítása a teljesítmény-alapú méretével kapcsolatos megfontolások a gépek és a lemezek és az értékelés tulajdonságok használatával. 

> [!NOTE]
> A helyszíni virtuális gépek futtatásához használt Azure infrastruktúrában található, mint a szolgáltatási (IaaS) virtuális gép áttelepítése Azure által biztosított költség becslése szolgál. Az Azure áttelepítése nem tekinti bármely Platformszolgáltatást (PaaS) vagy a szolgáltatott szoftverként (SaaS) költségek szoftverként. 

Becsült havi költségeket a számítási és tárolási összesítése a csoportban lévő virtuális gép esetében. 

![Értékelés VM költsége](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Egy adott gép tekintse meg a részleteket is lebontva.

![Értékelés VM költsége](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>További lépések

- [Ismerje meg,](how-to-scale-assessment.md) felderítése és értékeléséhez nagy VMware környezetben.
- Útmutató segítségével nagy-abban, hogy assessment csoportok létrehozásához [gép függőségi leképezése](how-to-create-group-machine-dependencies.md)
- [További](concepts-assessment-calculation.md) kapcsolatos értékelések kiszámítási módját.
