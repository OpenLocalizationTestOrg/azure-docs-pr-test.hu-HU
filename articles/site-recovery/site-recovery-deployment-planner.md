---
title: "aaaAzure Site Recovery telepítési planner VMware-Azure |} Microsoft Docs"
description: "Ez a hello Azure Site Recovery telepítési planner felhasználói útmutató."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/28/2017
ms.author: nisoneji
ms.openlocfilehash: a8c13cd47850575769e0186528807bc525bdeec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery Deployment Planner
Ez a cikk a VMware-Azure üzemi környezetek hello Azure Site Recovery telepítési Planner felhasználói útmutatója.

## <a name="overview"></a>Áttekintés

Mielőtt elkezdené a VMware virtuális gépek (VM) védelme a Site Recovery segítségével, foglaljon le elegendő sávszélesség, a napi adatok-módosítási gyakorisága, toomeet a kívánt helyreállítási időkorlát (RPO) alapján. Kell, hogy toodeploy hello megfelelő konfigurációs kiszolgálók számát és folyamat kiszolgálók helyszíni.

Szükség toocreate hello megfelelő típusú és a cél Azure storage-fiókok számát. Hozzon létre standard vagy prémium szintű tárfiókokat a fokozatosan növekvő használat során megnövekedett éles kiszolgálók miatt. Hello tárolási típus, egy virtuális gép, a munkaterhelés jellemzők (például: olvasási/írási i/o műveletek másodpercenkénti [sec] vagy adatforgalommal) alapján választja, és korlátozza a Site Recovery.

hello Site Recovery telepítési planner nyilvános előnézete funkció a parancssori eszköz, amely jelenleg csak hello VMware-Azure forgatókönyv érhető el. A VMware virtuális gépek profilhoz, sikeres replikáció a eszközt (a nem éles érinti, amely minden) toounderstand hello sávszélesség és az Azure tárolási követelmények segítségével távolról, és feladatátvételi teszt. Hello eszköz a Site Recovery összetevők helyszíni telepítése nélkül is futtathatja. Azonban pontos tooget elért átviteli sebesség eredmények, azt javasoljuk, hogy hello planner hello minimális követelménynek megfelelő hello Site Recovery konfigurációs kiszolgáló, hogy idővel kell toodeploy hello első lépéseket rendelkezésre álló Windows Serveren éles környezetben.

hello eszközt biztosít a következő adatok hello:

**Kompatibilitási felmérés**

* A virtuális gép jogosultságfelmérése a lemezszám, a lemezméretek, az IOPS, a forgalom és a rendszerindítási típus (EFI/BIOS) alapján
* a különbözeti replikáció szükséges becsült hello hálózati sávszélesség

**A hálózatisávszélesség-igény és RPO-elemzés**

* a különbözeti replikáció szükséges becsült hello hálózati sávszélesség
* amely a Site Recovery lekérheti a helyszíni tooAzure hello átviteli sebesség
* virtuális gépek toobatch hello alapján hello száma becsült sávszélesség toocomplete kezdeti replikáció egy adott időn belül

**Azure infrastruktúra-követelmények**

* hello tárolási típusa (standard vagy prémium szintű storage-fiók) követelmények az egyes virtuális gépek
* standard és prémium szintű storage-fiókok toobe beállítva replikálásra hello száma
* Tárfiókok elnevezési javaslatai az Azure Storage útmutatója alapján
* hello tárfiók elhelyezés virtuális gépen
* feladatátvételi teszt vagy hello előfizetésben feladatátvétel előtt állítsa be az Azure magok toobe hello száma
* az egyes Azure virtuális gép által ajánlott méret hello a helyszíni virtuális gép

**Helyszíni infrastruktúra-követelmények**
* hello szükséges konfigurációs kiszolgálók számát és a folyamat kiszolgálók toobe telepített helyszíni

>[!IMPORTANT]
>
>Mivel a használati valószínűleg tooincrease adott idő alatt, az összes hello számítás megy végbe, feltéve, hogy egy 30 százalékos növekedési tényező munkaterhelés jellemzők, és minden hello metrikák profilkészítési 95. percentilis értéke előző eszköz (olvasás/írás IOPS, kavarog, ezért oda-). Mindkét elem (a növekedési tényező és a százalékérték is) konfigurálható. bővebben növekedési tényező toolearn hello "növekedési tényezős szempontok" című szakaszában talál. További információ a PERCENTILIS értékének toolearn hello "PERCENTILIS hello kiszámításához használt" című szakaszában talál.
>

## <a name="requirements"></a>Követelmények
hello eszköznek két fő szakaszra: profilkészítési és jelentés létrehozásakor. Egy harmadik beállítás toocalculate átviteli csak is van. a következő táblázat hello hello server mely hello a profilkészítési és átviteli mérési kezdeményezett hello követelményei jelenjenek meg:

| Kiszolgálókövetelmények | Leírás|
|---|---|
|Profilkészítés és az átviteli sebesség mérése| <ul><li>Operációs rendszer: Microsoft Windows Server 2012 R2<br>(ideális esetben a megfelelő legalább hello [hello konfigurációs kiszolgáló javaslatok méretezés](https://aka.ms/asr-v2a-on-prem-components))</li><li>Gépkonfiguráció: 8 vCPU, 16 GB RAM, 300 GB HDD</li><li>[Microsoft .NET-keretrendszer 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[A Visual Studio 2012 szoftverhez készült Microsoft Visual C++ terjeszthető változata](https://aka.ms/vcplusplus-redistributable)</li><li>Internet-hozzáférés tooAzure erről a kiszolgálóról</li><li>Azure Storage-fiók</li><li>Rendszergazdai hozzáférés hello kiszolgálón</li><li>Minimális szabad lemezterület 100 GB (feltéve, hogy 1000 virtuális gépen átlagosan gépenként három lemezről 30 napig készít profilokat)</li><li>VMware vCenter statisztika szint beállításainak too2 vagy a magas szintű állítható be</li><li>Engedélyezze 443 portot: automatikus központi telepítési Planner használja a port tooconnect toovCenter kiszolgálón vagy ESXi-állomáson</ul></ul>|
| Jelentéskészítés | 2013-as vagy újabb Microsoft Excellel rendelkező Windows PC vagy Windows Server |
| Felhasználói engedélyek | Csak olvasási jogosultságot hello során profilkészítési tooaccess hello VMware vCenter server/VMware vSphere ESXi-gazdagép által használt felhasználói fiók |

> [!NOTE]
>
>hello eszköz profilt is csak VMDK és az RDM lemezzel rendelkező virtuális gépek számára. Nem képes az iSCSI- vagy NFS-lemezzel rendelkező virtuális gépek profilkészítésére. A Site Recovery támogatja az iSCSI és NFS lemezek VMware-kiszolgálók, de mivel hello telepítési planner nincs hello Vendég, és azt profilokra csak a vCenter teljesítményszámlálók felhasználásával, hello eszköz nem rendelkezik a lemez legyen kijelölve betekintést.
>

## <a name="download-and-extract-hello-public-preview"></a>Letöltéséhez és kibontásához hello nyilvános előzetes verzió
1. Töltse le a legújabb verziójának hello hello [Site Recovery telepítési planner nyilvános előzetes](https://aka.ms/asr-deployment-planner).  
hello eszköz egy .zip mappa van csomagolva. hello hello eszköz jelenlegi verziója csak a VMware-Azure hello forgatókönyvben támogatja.

2. Hello .zip mappa toohello Windows server, amelyből el kívánja toorun hello eszköz másolja.  
A Windows Server 2012 R2 hello eszköz futtathatja, ha hello kiszolgálón hálózati hozzáférési tooconnect toohello vCenter kiszolgáló vagy vSphere ESXi-állomáson, amely tárolja a hello virtuális gépek toobe csatolást. Azonban azt javasoljuk, hogy a kiszolgáló, amelynek hardverkonfiguráció megfelel-e hello hello eszközt futtatja [konfigurációs kiszolgáló méretezése iránymutatás](https://aka.ms/asr-v2a-on-prem-components). Ha már telepítette a Site Recovery összetevők helyszíni, futtassa a hello eszközt hello konfigurációs kiszolgálóról.

 Azt javasoljuk, hogy rendelkezik-e hello hello konfigurációs kiszolgálón (amely tartalmaz egy folyamat a épített kiszolgálót) hello hello eszközt futtató hardverkonfigurációja megegyezik. Ilyen konfiguráció biztosítja, hogy hello elért átviteli sebesség adott hello eszköz jelentések egyezések hello tényleges átviteli, hogy a replikáció során a Site Recovery érhető el. hello átviteli számítási rendelkezésre álló hálózati sávszélességet hello kiszolgálón és a hardveres konfigurációjáról (CPU, tárolási és így tovább) hello kiszolgáló függ. Hello eszközt bármely más kiszolgálóról futtatja, ha a kiszolgáló tooMicrosoft Azure hello átviteli kiszámítása. Emellett hello hardverkonfiguráció hello kiszolgáló, a konfigurációs kiszolgáló hello eltérőek lehetnek, mert hello elért átviteli sebesség, amely az eszköz jelentéseket hello pontatlanok lehetnek.

3. Bontsa ki a hello .zip mappa.  
hello mappát tartalmaz a több fájlokkal és almappákkal együtt. hello végrehajtható fájl ASRDeploymentPlanner.exe hello szülő mappában.

    Példa:  
    Másolja a hello .zip fájl tooE: \ meghajtó, és bontsa ki.
   E:\ASR Deployment Planner-Preview_v1.2.zip

    E:\ASR Deployment Planner-Preview_v1.2\ ASR Deployment Planner-Preview_v1.2\ ASRDeploymentPlanner.exe

## <a name="capabilities"></a>Funkciók
A következő három módot hello valamelyikében hello parancssori eszköz (ASRDeploymentPlanner.exe) is futtathatja:

1. Profilkészítés  
2. Jelentéskészítés
3. Átviteli sebesség lekérdezése

Először is futtathatja hello profilkészítési mód toogather VM adatforgalommal és iops-érték. A következő jelentésből hello eszköz toogenerate hello toofind hello hálózati sávszélességet és tárolóhelyet követelményeknek.

## <a name="profiling"></a>Profilkészítés
Profilkészítési módban hello deployment planner eszköz toohello vCenter-kiszolgáló vagy vSphere ESXi állomás toocollect teljesítményadatainak hello Virtuálisgép kapcsolódik.

* Profilkészítési nem befolyásolja hello teljesítmény hello az üzemi virtuális gépeket, mert nincs közvetlen kapcsolat toothem. Összes teljesítményadatának összegyűjtése az hello vCenter kiszolgáló vagy vSphere ESXi-állomáson.
* tooensure, hogy nincs jelentős hatást kiszolgálón hello profilkészítési, hello eszköz lekérdezések hello vCenter kiszolgáló vagy vSphere ESXi-állomáson, 15 percenként egyszer miatt. A lekérdezési időköz nem veszélyeztetheti a profilkészítési pontosságának, mert hello eszköz minden percben teljesítményszámláló adatait tárolja.

### <a name="create-a-list-of-vms-tooprofile"></a>Virtuális gépek tooprofile listájának létrehozása
Először hello virtuális gépek toobe csatolást listáját. Virtuális gépek összes hello nevét a vCenter kiszolgáló vagy vSphere ESXi-állomáson parancsaival hello VMware vSphere PowerCLI eljárást követő hello beolvasása. Azt is megteheti egy hello rövid fájlnevek listázhatja, vagy az IP-címek hello tooprofile manuálisan kívánt virtuális gépeket.

1. Jelentkezzen be toohello VM adott VMware vSphere PowerCLI telepítve van-e.
2. Nyissa meg a hello VMware vSphere PowerCLI konzolt.
3. Győződjön meg arról, hogy hello végrehajtási házirend engedélyezve van-e hello parancsfájlt. Ha le van tiltva, indítsa el a hello VMware vSphere PowerCLI konzol felügyeleti üzemmódban, és engedélyez hello a következő parancs futtatásával:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4. Előfordulhat, hogy ha Connect-VIServer értéke nem értelmezhető parancsmag hello nevét a következő parancs optionly kell toorun hello.
 
            Add-PSSnapin VMware.VimAutomation.Core 

5. tooget összes hello neve a vCenter kiszolgáló vagy vSphere ESXi-alapú virtuális gépek üzemeltetéséhez, és hello listát tárolására egy .txt fájlban, az itt felsorolt futtatási hello két parancsot.
Cserélje le a &lsaquo;server name&rsaquo; (kiszolgáló neve), a &lsaquo;user name&rsaquo; (felhasználónév), a &lsaquo;password&rsaquo; (jelszó), az &lsaquo;outputfile.txt&rsaquo; (kimenetifájl.txt) paramétereket saját értékeire.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>

6. Nyissa meg a hello kimeneti fájlt a Jegyzettömbben, és minden virtuális gép, amelyet az tooprofile tooanother fájl (például ProfileVMList.txt), egy virtuális gép nevét egy sorban hello nevei másolja. Ez a fájl a bemeneti toohello vettük *- VMListFile* hello parancssori eszköz paraméter.

    ![Hello telepítési planner a virtuális gép listája](./media/site-recovery-deployment-planner/profile-vm-list.png)

### <a name="start-profiling"></a>Profilkészítés indítása
Miután a virtuális gépek toobe csatolást hello listája, a hello eszköz a profilkészítési módban is futtathatja. Kötelező és választható paramétereit hello eszköz toorun hello listája itt profilkészítési módban van.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Paraméter neve | Leírás |
|---|---|
| -Művelet | StartProfiling |
| -Kiszolgáló | hello teljesen minősített tartománynév vagy IP-címét hello vCenter kiszolgáló vagy vSphere ESXi-állomáson, amelynek a virtuális gépek toobe csatolást.|
| -Felhasználó | hello felhasználói név tooconnect toohello vCenter kiszolgáló vagy vSphere ESXi-állomáson. hello felhasználónak toohave csak olvasási hozzáféréssel, minimum van szüksége.|
| -VMListFile | hello fájlt, amely virtuális gépek toobe csatolást hello listáját tartalmazza. hello fájl elérési út lehet abszolút vagy relatív. hello fájl soronként egy virtuális gép neve vagy IP-címet kell tartalmaznia. Hello fájlban megadott virtuálisgép-név ugyanaz, mint a Virtuálisgép-nevet hello hello vCenter kiszolgáló vagy vSphere ESXi-állomáson lévő kell hello.<br>Például VMList.txt hello fájl tartalmazza a következő virtuális gépek hello:<ul><li>virtual_machine_A</li><li>10.150.29.110</li><li>virtual_machine_B</li><ul> |
| -NoOfDaysToProfile | hello hány napig a mely profilkészítési toobe futtatásához. Azt javasoljuk, hogy több mint 15 nappal tooensure, hogy a munkaterhelés mintát környezetében hello hello keresztül a megadott időszak követi, és egy pontos ajánlás tooprovide használt profilkészítés. |
| -Directory | (Választható) hello univerzális elnevezési konvenció (UNC) vagy a helyi könyvtár elérési útja toostore profilkészítési profilkészítési során létrehozott adatokat. Ha a könyvtár neve nincs megadva, "ProfiledData" nevű hello aktuális elérési úton található hello könyvtár hello alapértelmezett címtár lesz. |
| -Password | (Választható) hello jelszó toouse tooconnect toohello vCenter kiszolgáló vagy vSphere ESXi-állomáson. Ha nem ad meg egy most, bekéri az hello parancs végrehajtásakor.|
| -StorageAccountName | (Választható) hello tárfiók nevet használt toofind hello átviteli elérhető adatainak replikálását a helyszíni tooAzure. hello eszköz feltöltések teszt toothis tárolási fiók toocalculate adatátvitelt.|
| -StorageAccountKey | (Választható) hello tárfiók kulcs által használt tooaccess hello tárfiók. Nyissa meg az Azure portál toohello > Storage-fiókok ><*Tárfiók neve*>> Beállítások > elérési kulcsok > Key1 (vagy a klasszikus tárfiók elsődleges elérési kulcsát). |
| -Környezet | (nem kötelező) Ez az Ön Azure Storage-fiókjának célkörnyezete. Ez a következő három érték egyike lehet: AzureCloud, AzureUSGovernment, AzureChinaCloud. Az alapértelmezett érték az AzureCloud. Ha a cél Azure-régiót Azure Amerikai Egyesült államokbeli kormányzati vagy Azure Kína felhők, használja a hello paramétert. |


Azt javasoljuk, hogy legalább 15 too30 napig profilját a virtuális gépek. Hello időszak profilkészítési, során ASRDeploymentPlanner.exe folyamatosan működik. hello eszköz profilkészítési idő bemenetből fogad adatokat napban. Ha szeretne tooprofile néhány órában vagy percben hello eszköz, a gyors vizsgálat hello nyilvános előzetes verziójában, szüksége lesz tooconvert hello idő hello egyenértékű mérték nap be. Például: 30 percig tooprofile hello bemeneti 30/(60*24) kell legyen = 0.021 nap. hello minimális idő profilkészítési engedélyezett értéke 30 perc.

Profilkészítési, során opcionálisan átadhatók a storage-fiók nevét és a Site Recovery érhető el a replikálást a konfigurációs kiszolgáló hello vagy folyamat server tooAzure hello időpontjában, a kulcs toofind hello átviteli sebesség. Ha hello storage-fiók nevét és a kulcs nem átadott során profilkészítési, hello eszköz elérhető átviteli sebesség nem számít.

Hello eszköz különböző csoportjai számára virtuális gépek több példányát is futtathatja. Győződjön meg arról, hogy hello virtuális gép neve nem ismétlődnek hello beállítása profilkészítési valamelyikében. Például, ha Ön rendelkezik csatolást tíz virtuális gépek (VM1 VM10 keresztül), és néhány nap múlva szeretné tooprofile egy másik öt virtuális gépek (VM11 VM15 keresztül), hello eszköz futtatásához egy másik parancssori konzolban a virtuális gépek hello második együttesét a (VM11 VM15 keresztül). Győződjön meg arról, hogy hello második virtuális gépek csoportja nem rendelkezik a virtuális gépek nevénél hello első profilkészítési példányból hello második futtatásához használja a különböző kimeneti könyvtár. Két példányban hello eszköz használata esetén a profilkészítési hello azonos virtuális gépek és -felhasználási hello azonos kimeneti könyvtár, generált hello jelentés pontatlan lehet.

Virtuálisgép-konfigurációk művelet profilkészítési hello hello elején egyszer rögzített, és VMDetailList.xml nevű fájlban tárolja. Ez az információ akkor használatos, ha a hello jelentést hoz létre. Minden oldalától hello kezdete toohello profilkészítési (például egy nagyobb számot mag, a lemezek vagy a hálózati adapterek) Virtuálisgép-konfiguráció módosítását nem rögzíti. Ha profilozott Virtuálisgép-konfiguráció megváltozott hello során profilkészítési hello nyilvános előzetes verziójában, itt a következő hello megoldás tooget legújabb VM részletek hello jelentés létrehozásakor:

* Készítsen biztonsági másolatot VMdetailList.xml, és törölje a hello fájlt a jelenlegi helyéről.
* -Felhasználó és - jelszavát argumentumokat adhat a jelentéskészítésre hello időpontjában.

profilkészítési parancs hello directory profilkészítési hello több fájlt hoz létre. Ne törölje hello fájlokat, mivel ezzel így hatással van a jelentés létrehozásához.

#### <a name="example-1-profile-vms-for-30-days-and-find-hello-throughput-from-on-premises-tooazure"></a>1. példa: Profil virtuális gépeket 30 nap, és a helyszíni tooAzure keresés hello átviteli sebesség
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  30  -User vCenterUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>2. példa: Profilkészítés virtuális gépről 15 napon keresztül

```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User vCenterUser1
```

#### <a name="example-3-profile-vms-for-1-hour-for-a-quick-test-of-hello-tool"></a>3. példa: Profil virtuális gépeknek a virtuális eszköz hello gyors vizsgálat 1 óra
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  0.04  -User vCenterUser1
```

>[!NOTE]
>
>* Ha hello kiszolgáló hello eszköz futó újraindították vagy összeomlott, vagy ha bezárja hello eszköz a Ctrl + C, csatolást hello adatok megmaradnak. Azonban esély van a hiányzó hello profilozott adatok az elmúlt 15 perc. Ilyen esetben hello kiszolgáló újraindítása után futtassa újra a hello eszköz profilkészítési módban.
>* Ha hello storage-fiók nevét és a kulcs átadott hello eszköz intézkedések hello átviteli sebesség a profilkészítési hello utolsó lépésnél. Ha hello eszköz le van zárva, profilkészítési befejezése előtt, hello átviteli sebesség nem számítható ki. toofind hello átviteli létrehozása előtt hello jelentés, hello GetThroughput művelet hello parancssori konzolról futtatható. Ellenkező esetben generált hello jelentés nem tartalmazza a hello átviteli sebességet.


## <a name="generate-a-report"></a>Jelentés létrehozása
hello hoz létre egy makró engedélyezve van a Microsoft Excel (fájl XLSM) kimenetként hello jelentés, amely összefoglalja az összes hello telepítési javaslatok. hello jelentés DeploymentPlannerReport_ nevű <*egyedi azonosítószáma*> .xlsm és a elhelyezett hello megadott könyvtárban.

Profilkészítési befejezése után a hello eszköz jelentéskészítésre módban is futtathatja. a következő táblázat hello kötelező és választható eszköz paraméterek toorun jelentéskészítésre módban listáját tartalmazza.

`ASRDeploymentPlanner.exe -Operation GenerateReport /?`

|Paraméter neve | Leírás |
|-|-|
| -Művelet | Jelentés készítése |
| -Kiszolgáló |  hello vCenter vagy vSphere kiszolgáló teljesen minősített tartománynév vagy IP-cím (használata hello azonos nevét vagy IP-címet, amelyet a profilkészítési hello időpontjában használt) ahol hello csatolást jelentése toobe létrehozott virtuális gépek találhatók. Vegye figyelembe, hogy ha egy vCenter-kiszolgálót a profilkészítési hello időpontjában, server nem használható a vSphere jelentés létrehozásához, és fordítva.|
| -VMListFile | hello hello listája jelentés hello profilozott virtuális gépeket tartalmazó fájl előállított toobe. hello fájl elérési út lehet abszolút vagy relatív. hello fájlt tartalmaznia kell egy virtuális gép nevét vagy IP-cím soronként. hello virtuális gépek nevénél hello fájlban megadott ugyanaz, mint a hello virtuális gépek nevénél hello vCenter kiszolgáló vagy vSphere ESXi-állomáson, és egyezés profilkészítési során használt kell hello.|
| -Directory | (Választható) hello UNC vagy helyi könyvtár elérési útja, ahol hello csatolást adatok (profilkészítési folyamán létrehozott fájlokat) tárolja. Ezek az adatok megadása kötelező hello jelentés előállítása érdekében. Ha a név nincs megadva, a rendszer a „ProfiledData” könyvtárat használja. |
| -GoalToCompleteIR | (Választható) hello óraszámon belül mely hello a kezdeti replikációja, hello csatolást virtuális gépek kell toobe befejeződött. hello jön létre a jelentésben a virtuális gépek, amelynek kezdeti replikáció is elvégezhető a megadott hello hello száma idő. hello alapértelmezés szerint 72 óra. |
| -Felhasználó | (Választható) hello felhasználói név toouse tooconnect toohello vCenter vagy vSphere kiszolgáló. hello értéke használt toofetch hello legfrissebb konfigurációs adatok a hello virtuális gépeket, például a lemezek hello száma, a magok száma és a hálózati adapterek, hello jelentésben toouse száma. Ha hello neve nincs megadva, hello konfigurációs kickoff profilkészítési hello hello elején gyűjtött adatok. |
| -Password | (Választható) hello jelszó toouse tooconnect toohello vCenter kiszolgáló vagy vSphere ESXi-állomáson. Ha nincs megadva hello jelszó, bekéri az később hello parancs végrehajtásakor. |
| -DesiredRPO | (Választható) hello kívánt helyreállításipont-célkitűzést, percben. hello alapértelmezett érték 15 perc.|
| -Bandwidth | Sávszélesség (Mbps). hello paraméter toouse toocalculate hello hello az elérhető helyreállítási Időkorlát megadott sávszélesség. |
| -StartDate | (Választható) hello kezdő dátum és időpont a hh-nn-YYYY:HH:MM (24 órás formátumban). A *StartDate* és az *EndDate* paraméter megadása kötelező. A StartDate megadása esetén hello jelentés csatolást hello StartDate és az EndDate közötti gyűjtött adatok létrejön. |
| -EndDate | (Választható) hello záró dátuma és időpontja a hh-nn-YYYY:HH:MM (24 órás formátumban). Az *EndDate* és a *StartDate* paraméter megadása kötelező. EndDate megadása esetén hello jelentés csatolást hello StartDate és az EndDate közötti begyűjtött adatokból keletkezik. |
| -GrowthFactor | (Választható) hello növekedési tényező, százalékban kifejezve. hello alapértelmezett értéke 30 százalék. |
| -UseManagedDisks | (Nem kötelező) UseManagedDisks – Igen/Nem. Az alapértelmezett érték az Igen. virtuális gépek, amelyek felvehetők egy tárfiók hello számát figyelembe véve, hogy helyett a nem felügyelt lemezt felügyelt lemezes virtuális gépeinek feladatátvételi és tesztelési célú feladatátvételi történik kiszámítása. |

#### <a name="example-1-generate-a-report-with-default-values-when-hello-profiled-data-is-on-hello-local-drive"></a>1. példa: Az alapértelmezett értékekkel jelentés készítése hello helyi meghajtó a csatolást hello adatok esetén
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-hello-profiled-data-is-on-a-remote-server"></a>2. példa: Jelentés készítése, amikor egy távoli kiszolgálón csatolást hello adatok
Hello távoli címtár olvasási/írási hozzáférést kell rendelkeznie.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-and-goal-toocomplete-ir-within-specified-time"></a>3. példa: A megadott időn belül az egy adott sávszélesség és a cél toocomplete IR jelentés készítése
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -Bandwidth 100 -GoalToCompleteIR 24
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-hello-default-30-percent"></a>4. példa: Egy 5 százalékos növekedési tényező hello alapértelmezett 30 százalékos helyett a jelentés készítése
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>5. példa: Jelentés létrehozása a profilkészítés során használt adatok egy részéből
Például 30 napnyi adat profilozott rendelkezik, és szeretné, hogy egy jelentés toogenerate csak 20 napra.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minute-rpo"></a>6. példa: Jelentés készítése 5 perces helyreállítási időkorláttal
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

## <a name="percentile-value-used-for-hello-calculation"></a>A PERCENTILIS hello kiszámításához használt
**Milyen alapértelmezett PERCENTILIS hello Teljesítményelemzési mutatón során profilkészítési biztosítja hello eszközt használja, egy jelentést készít az összegyűjtött?**

hello eszköz alapértelmezett toohello 95. értékek olvasási/írási IOPS, az IOPS, és azok hello a virtuális gépek profilkészítési alatt gyűjtött adatforgalommal írni. Ez a metrika biztosítja, hogy hello 100 PERCENTILIS csúcs ideiglenes események miatt jelenhet meg a virtuális gépek nem használt toodetermine a cél tárfiók és a forrás-sávszélesség követelmények van. Az ideiglenes esemény lehet például egy naponta egyszer futtatott biztonsági mentési feladat, rendszeres időközönként végzett adatbázis-indexelés, elemzésijelentés-készítési tevékenység vagy bármely hasonló, rövid ideig tartó, időpontalapú esemény.

95. értékek használatával biztosítja az IGAZ képe valós munkaterhelés jellemzőit, és lehetővé teszi az hello lehető legjobb teljesítményt hello munkaterhelések Azure futtatásakor. Nem tervezzük, hogy meg kellene toochange ezt a számot. Ha hello érték (toohello 90. százalékos érték, például) módosítja, frissítheti hello konfigurációs fájl *ASRDeploymentPlanner.exe.config* a hello alapértelmezett mappát, és mentse azt a hello meglévő csatolást toogenerate új jelentés adatok.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>A növekedési tényezővel kapcsolatos szempontok
**Miért kell figyelembe vennem a növekedési tényezővel kapcsolatos szempontokat a környezetek megtervezésekor?**

A munkaterhelés jellemzőit, feltéve, hogy idővel használata során előforduló esetleges növekedését növekedését kritikus tooaccount. Védelem van beállítva, ha módosítja a munkaterhelés jellemzőit, miután letiltásával és újbóli engedélyezése az üdvözlő védelmi nélkül nem lehet átállítani a tooa eltérő tárfiók védelemre.

Tegyük fel például, hogy a virtuális gép jelenleg megfelel egy standard szintű tárreplikációs fiókhoz. Hello keresztül következő három hónapban, több olyan változás is valószínűleg toooccur:

* hello VM hello alkalmazás felhasználója hello számát növeli.
* hello eredményül kapott megnövekedett forgalmának kezeléséhez hello VM hello VM toogo toopremium tárolási van szükség, hogy a Site Recovery replikációs tudja tartani.
* Következésképpen fog toodisable rendelkezik, és engedélyezze újra a védelmi tooa prémium szintű storage-fiók.

Határozottan javasoljuk, hogy központi telepítésének megtervezése során, és amíg hello alapértelmezett értéke 30 százalékos növekedési tervezése. Az alkalmazás használati mintát és növekedési leképezések szakértői hello, és ennek megfelelően módosíthatja ezt a számot a jelentés készítése során. Ezenkívül jelentéseket is létrehozhat több hello a különböző növekedési tényező az azonos csatolást adatokat, és határozza meg, milyen cél tárolás és a forrás sávszélesség ajánlások működhet a legjobban az Ön.

hello jön létre a Microsoft Excel jelentés hello a következő információkat tartalmazza:

* [Input](site-recovery-deployment-planner.md#input) (Bemenet)
* [Javaslatok](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Recommendations-Bandwidth Input](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input) (Javaslatok – Bemeneti sávszélesség)
* [VM&lt;-&gt;Storage Placement](site-recovery-deployment-planner.md#vm-storage-placement) (Virtuálisgép&lt;-&gt;tároló elhelyezése)
* [Compatible VMs](site-recovery-deployment-planner.md#compatible-vms) (Kompatibilis virtuális gépek)
* [Incompatible VMs](site-recovery-deployment-planner.md#incompatible-vms) (Nem kompatibilis virtuális gépek)

![Deployment Planner](./media/site-recovery-deployment-planner/dp-report.png)

## <a name="get-throughput"></a>Átviteli sebesség lekérdezése

tooestimate hello átviteli-mel a Site Recovery is a helyszíni tooAzure hello eszköz GetThroughput módban fusson a replikáció során. hello eszköz kiszámítja hello átviteli eszköz hello hello kiszolgálóról futó. Ideális esetben ez a kiszolgáló hello konfigurációs kiszolgáló méretezési útmutató alapul. Ha már telepítette a Site Recovery infrastruktúra összetevői a helyszíni, futtassa a hello eszközt hello konfigurációs kiszolgálón.

Nyisson meg egy parancssori konzolt, és válassza a toohello Site Recovery üzembe helyezése tervezési eszköz mappát. Futtassa az ASRDeploymentPlanner.exe fájlt az alábbi paraméterekkel.

`ASRDeploymentPlanner.exe -Operation GetThroughput /?`

|Paraméter neve | Leírás |
|-|-|
| -Művelet | Átviteli sebesség lekérdezése |
| -Directory | (Választható) hello UNC vagy helyi könyvtár elérési útja, ahol hello csatolást adatok (profilkészítési folyamán létrehozott fájlokat) tárolja. Ezek az adatok megadása kötelező hello jelentés előállítása érdekében. Ha a könyvtár neve nincs megadva, a rendszer a „ProfiledData” könyvtárat használja. |
| -StorageAccountName | hello storage-fiók neve, amely a helyszíni tooAzure adatainak replikálását toofind hello sávszélességre használatban van. hello eszköz feltöltések vizsgálati adatok toothis tárolási fiók toofind hello sávszélességre. |
| -StorageAccountKey | hello tárfiók kulcs által használt tooaccess hello tárfiók. Nyissa meg az Azure portál toohello > Storage-fiókok ><*Tárfiók neve*>> Beállítások > elérési kulcsok > Key1 (vagy a klasszikus tárfiók elsődleges hívóbetűjét). |
| -VMListFile | a felhasznált számítási hello sávszélesség csatolást virtuális gépek toobe hello listáját tartalmazó hello fájl. hello fájl elérési út lehet abszolút vagy relatív. hello fájl soronként egy virtuális gép neve vagy IP-címet kell tartalmaznia. hello virtuális gépek nevénél hello fájlban megadott ugyanaz, mint a hello virtuális gépek nevénél hello vCenter kiszolgáló vagy vSphere ESXi-állomáson lévő kell hello.<br>Például VMList.txt hello fájl tartalmazza a következő virtuális gépek hello:<ul><li>VM_A</li><li>10.150.29.110</li><li>VM_B</li></ul>|
| -Környezet | (nem kötelező) Ez az Ön Azure Storage-fiókjának célkörnyezete. Ez a következő három érték egyike lehet: AzureCloud, AzureUSGovernment, AzureChinaCloud. Az alapértelmezett érték az AzureCloud. Ha a cél Azure-régiót Azure Amerikai Egyesült államokbeli kormányzati vagy Azure Kína felhők, használja a hello paramétert. |

hello eszköz hoz létre több 64 MB-os asrvhdfile <> # .vhd-fájlokat (ahol a "#" azt hello fájlok száma) hello megadott könyvtárban. hello eszköz hello fájlok toohello tárolási fiók toofind hello teljesítmény feltöltését. Hello átviteli mérik, miután hello eszköz bővítményi hello törli az hello tárfiók és hello helyi kiszolgálóról. Ha hello eszköz bármilyen okból megszakad, miközben átviteli kiszámítja, hello storage vagy a helyi kiszolgáló hello hello fájlok nem törli. Toodelete kell őket manuálisan.

hello átviteli mérik megadott pontján időben, és hello maximális átviteli sebesség replikálás során a Site Recovery érhető el, feltéve, hogy más tényező marad hello azonos. Például, ha bármely alkalmazás indítása, a replikáció során változik ugyanazon a hálózaton, hello tényleges átviteli hello nagyobb sávszélességet. Ha a konfigurációs kiszolgáló hello GetThroughput parancsot futtatja, hello eszköz nem érzékeli a védett virtuális gépek és a folyamatban lévő replikáció. hello hello mért átviteli eredménye eltérő, ha hello GetThroughput művelet fut. Ha hello védett virtuális gépek magas adatot kavarog egyidejűleg. Azt javasoljuk, hogy hello eszköz különböző időpontokban közben toounderstand profilkészítési milyen átviteli szintű különböző időpontokban érhető el. Hello jelentésben hello eszköz hello utolsó mért átviteli jeleníti meg.

### <a name="example"></a>Példa
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Directory  E:\vCenter1_ProfiledData -VMListFile E:\vCenter1_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

>[!NOTE]
>
> Futtassa a hello eszközt olyan kiszolgálóra, amely rendelkezik hello CPU foglalhatják hello konfigurációs kiszolgáló, és ugyanazt a tárhelyet.
>
> A replikáció hello ajánlott sávszélesség toomeet hello RPO 100 százalék hello idő beállítása. Miután hello jobb sávszélességet, ha nem lát hello eszköz által jelentett hello elért átviteli sebesség növekedése, hello a következő:
>
>  1. Minden hálózati szolgáltatásminőségi (QoS), amely korlátozza az Site Recovery átviteli van-e, ellenőrizze a toodetermine.
>
>  2. Toodetermine ellenőrizze, hogy fizikailag támogatott Microsoft Azure régióban toominimize hálózati késés a legközelebbi hello van a Site Recovery-tárolóban.
>
>  3. A helyi tároló jellemzők toodetermine ellenőrizze, hogy javítható hello hardver (például HDD tooSSD).
>
>  4. Hello folyamatkiszolgáló hello Site Recovery beállításainak módosítását túl[hello növelje meg a hálózati sávszélesség a replikáláshoz használt](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

## <a name="recommendations-with-desired-rpo-as-input"></a>Javaslatok a kívánt helyreállítási időkorlát (RPO) bemenetként való megadásával

### <a name="profiled-data"></a>Profilkészítés során létrehozott adatok

![hello telepítési planner hello csatolást adatok nézet](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Profilozott periódus**: hello időszak során melyik hello profilkészítési futtatták. Alapértelmezés szerint hello eszköz tartalmazza az összes profilozott adatokat hello számítás, kivéve, ha hello jelentés egy adott időszakra StartDate és az EndDate beállításokat használva jelentés létrehozása során létrehozott.

**Kiszolgálónév**: hello nevét vagy IP-címét hello VMware vCenter vagy ESXi-állomáson, amelynek virtuális gépek jelentést hoz létre.

**A helyreállítási Időkorlát szükséges**: hello helyreállításipont-célkitűzést a központi telepítés. Alapértelmezés szerint a hello szükséges hálózati sávszélesség kiszámítása a helyreállítási Időkorlát értékei 15, 30 és 60 perc. Hello kijelölés alapján, érintett hello frissülnek hello lapra. Ha hello használt *DesiredRPOinMin* paraméter hello jelentés, megjelenő érték hello szükséges RPO eredmény létrehozása közben.

### <a name="profiling-overview"></a>Profilkészítés áttekintése

![Profilkészítési hello telepítési planner eredményezi](./media/site-recovery-deployment-planner/profiling-overview.png)

**Virtuális gépek csatolást teljes**: hello virtuális gépek, amelyek profilozott adatok teljes száma. Hello VMListFile bármely virtuális gépek, amelyek nem voltak csatolást nevekkel rendelkezzen, ha a virtuális gépek ezek nem számítanak bele hello jelentéskészítésre és hello teljes profilozott virtuális gépek száma nem tartoznak.

**Kompatibilis a virtuális gépek**: hello beállított virtuális gépeket, a Site Recovery védett tooAzure lehet. Kompatibilis a virtuális gépek melyik hello a szükséges hálózati sávszélesség, a storage-fiókok száma, az Azure magok száma és a konfigurációs kiszolgálók számát és további folyamat kiszolgálók számított hello száma is. hello részletek minden kompatibilis virtuális gép hello "Kompatibilis virtuális gépek" szakaszban találhatók.

**Nem kompatibilis a virtuális gépek**: hello, amelyek nem kompatibilisek a Site Recovery szolgáltatással védelem profilozott virtuális gépeinek számával. hello okai kompatibilitási hello "Nem kompatibilis virtuális gépek" szakaszban találhatók. Ha hello VMListFile nevek bármely virtuális gépek, amelyek nem voltak csatolást, virtuális gépek ki vannak zárva hello nem kompatibilis virtuális gépek száma. A virtuális gépek listázott "Adatok nem található" hello "nem kompatibilis virtuális gépek" szakasz hello végén.

**Desired RPO**(Kívánt RPO): A kívánt helyreállítási időkorlát, percben megadva. hello jelentés létrejön három RPO-érték: 30 és 60 perc (alapértelmezett), 15. hello sávszélesség javaslat hello jelentésben hello lap jobb hello felső hello szükséges RPO legördülő listából alapján módosul. Ha létrehozta a hello jelentés hello segítségével *- DesiredRPO* paraméter egy egyéni értékkel, az egyéni érték jelenik meg: hello alapértelmezett hello szükséges RPO legördülő listában.

### <a name="required-network-bandwidth-mbps"></a>Szükséges hálózati sávszélesség (Mbps)

![Hello telepítési planner szükséges hálózati sávszélesség](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**toomeet RPO 100 százalék hello idő:** hello ajánlott sávszélesség MB/s toobe lefoglalt toomeet a kívánt helyreállítási Időkorlát hello idő 100 százalék. A sávszélesség mennyiségét kell lennie a kompatibilis virtuális gépek tooavoid stabil állapot különbözeti replikálását dedikált bármely RPO megsértésének.

**hello ideje 90 %-át a helyreállítási Időkorlát toomeet**: szélessávú árképzési miatt, vagy más okból, ha nem állítható be hello szükséges sávszélesség toomeet a kívánt helyreállítási Időkorlát 100 százalék hello idő, választhat toogo egy alacsonyabb sávszélességű beállítástól, amely megfelel a a keresett helyreállítási Időkorlát hello ideje 90 %-át. toounderstand hello következményei beállítása az alacsony sávszélességű, hello jelentés biztosít egy elemzési hello számát és a helyreállítási Időkorlát megsértésének tooexpect időtartamát.

**Elért átviteli sebesség:** hello átviteli hello kiszolgálóról, amelyen hello GetThroughput parancs toohello Microsoft Azure terület, ahol hello storage-fiókban található futtatása. Az átviteli sebesség száma azt jelzi, becsült hello szintje, amely kompatibilis a virtuális gépek a Site Recovery segítségével, feltéve, hogy a konfigurációs kiszolgáló, vagy a folyamat kiszolgáló tárolási és hálózati jellemzők maradnak hello ugyanaz legyen, mint hello védelemnél érhető el hello kiszolgáló, amelyből hello eszköz futtatása.

A replikáció célszerű hello ajánlott sávszélesség toomeet hello RPO hello idő 100 százalék. Miután hello sávszélességgel, ha nem látja az elért hello teljesítmény növekedése hello eszköz által jelentett, hajtsa végre a következő hello:

1. Minden hálózati szolgáltatásminőségi (QoS), amely korlátozza az Site Recovery átviteli van-e, ellenőrizze a toosee.

2. Toosee ellenőrizze, hogy fizikailag támogatott Microsoft Azure régióban toominimize hálózati késés a legközelebbi hello van a Site Recovery-tárolóban.

3. A helyi tároló jellemzők toodetermine ellenőrizze, hogy javítható hello hardver (például HDD tooSSD).

4. Hello folyamatkiszolgáló hello Site Recovery beállításainak módosítását túl[hello összeg hálózati sávszélesség a replikáláshoz használt növelése](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Ha hello eszköz konfigurációs kiszolgálón vagy a folyamatkiszolgálót, már rendelkezik védett virtuális gépek futnak, futtassa hello eszközt néhány alkalommal. hello attól függően, hogy ezen a ponton az időben feldolgozott forgalom mennyisége hello megváltozása átviteli sebesség érhető el.

A Site Recovery minden vállalati üzemelő példánya esetében az [ExpressRoute](https://aka.ms/expressroute) használata javasolt.

### <a name="required-storage-accounts"></a>Szükséges tárfiókok
a következő diagram azt mutatja be hello tárolási teljes száma (standard és premium) fiókok, amelyek minden szükséges tooprotect hello hello kompatibilis virtuális gépeket. milyen tárolási fiók toouse az egyes virtuális gépek toolearn hello "Virtuálisgép-tároló elhelyezési" című szakaszában talál.

![Hello telepítési planner szükséges storage-fiók](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

### <a name="required-number-of-azure-cores"></a>Szükséges Azure-magok száma
Ez a eredménye hello magok toobe előtt az összes feladatátvételi vagy a teszt feladatátvételi hello kompatibilis virtuális gépek száma. Ha túl kevés magok hello előfizetésben elérhető, a Site Recovery hello helyreállításkor toocreate virtuális gépek sikertelen feladatátvételi vagy feladatátvételi tesztelése.

![A központi telepítés planner hello Azure magszámra vonatkozó követelménynek](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

### <a name="required-on-premises-infrastructure"></a>Szükséges helyszíni infrastruktúra
Ez a szám hello teljes száma érték a konfigurációs kiszolgálók és a további folyamat kiszolgálók toobe konfigurálva, hogy elegendő az tooprotect összes hello kompatibilis virtuális gépeket. Attól függően, hogy támogatott hello [hello konfigurációs kiszolgáló javaslatok méretezés](https://aka.ms/asr-v2a-on-prem-components), hello eszköz javasolhat további kiszolgálókat. hello javaslat alapján hello hello napi forgalom vagy hello (feltéve, hogy a virtuális gépenként három lemezt átlag) védett virtuális gépek maximális száma nagyobb, amelyik találati hello konfigurációs kiszolgálóján vagy hello további folyamat első. Naponta és védett lemezek teljes száma a teljes adatforgalom hello részleteit a hello "Input" szakaszban találhat.

![Hello telepítési planner kötelező a helyszíni infrastruktúra](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

### <a name="what-if-analysis"></a>Lehetőségelemzés
Az elemzés ismerteti, hogy hány megsértésének fordulhat elő, ha időszak profilkészítési hello során a szükséges hello RPO toobe alacsonyabb sávszélességet teljesül hello idő csak 90 %-át. A helyreállítási időkorlát túllépése bármelyik nap egy vagy több alkalommal is előfordulhat. hello grafikon azt ábrázolja, hello csúcs RPO hello nap.
Az elemzés alapján, megadhatja, hogy ha hello RPO megsértésének összes nap és napi találati RPO maximális száma az elfogadható hello megadott alacsony sávszélességű. Ha elfogadható, a replikáció foglalhatja hello alacsony sávszélességű, ellenkező esetben lefoglalni hello nagyobb sávszélességet javasolt toomeet hello szükséges RPO hello idő 100 százalék.

![Elemzési a hello telepítési planner](./media/site-recovery-deployment-planner/what-if-analysis.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>A virtuálisgép-köteg ajánlott mérete a kezdeti replikációhoz
Ebben a szakaszban ajánlott hello száma párhuzamos toocomplete hello kezdeti replikálás a hello 72 órán belül védhető virtuális gépeken javasolt sávszélesség toomeet szükséges RPO 100 százalék hello idő beállítása. Ezen érték konfigurálható. toochange a jelentés-eseménygenerálás, használjon hello *GoalToCompleteIR* paraméter.

hello itt grafikonon sávszélesség értéktartománya és egy számított VM köteg mérete száma toocomplete kezdeti replikálás 72 óra hello átlagos alapján észlelte virtuális gép mérete összes hello kompatibilis virtuális gépeket.

Hello nyilvános előzetes verziójában hello jelentés nem adja meg, mely virtuális gépek szerepelnie kell egy kötegben. Hello "kompatibilis virtuális gépek" szakasz toofind látható minden egyes Virtuálisgép-méretet hello lemezméretet használja, és jelölje ki azokat a köteg, vagy választhat a munkaterhelés ismert jellemzők alapján hello virtuális gépeket. hello befejezési idő hello kezdeti replikációs változásokat arányosan, hello tényleges Virtuálisgép-lemez mérete alapján, használt lemezterület és a rendelkezésre álló hálózati teljesítményt.

![A virtuálisgép-köteg ajánlott mérete](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

### <a name="growth-factor-and-percentile-values-used"></a>A használt növekedési tényező és százalékértékek
Ez a szakasz hello hello alján látható hello PERCENTILIS használt hello teljesítményszámlálók csatolást hello virtuális gépek (alapértelmezett érték 95. százalékos érték) lap, és hello növekedési tényező (alapértelmezett érték 30 százalékos) használt összes hello számításokban.

![A használt növekedési tényező és százalékértékek](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Javaslatok az elérhető sávszélesség bemenetként való megadásával

![Javaslatok az elérhető sávszélesség bemenetként való megadásával](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Előfordulhat olyan helyzet, hogy legfeljebb x Mbps sávszélességet tud beállítani a Site Recovery replikációjára. hello az eszköz lehetővé teszi tooinput rendelkezésre álló sávszélesség (használatával hello - jelentés létrehozása során sávszélesség paraméter) és az elérhető helyreállítási Időkorlát hello perc múlva. Az elérhető RPO-érték a dönthet, hogy e tooset fel további sávszélesség szükséges, vagy OK áll egy vész-helyreállítási megoldást a Helyreállítási időkorláttal áll.

![Elérhető RPO 500 Mbps sávszélességhez](./media/site-recovery-deployment-planner/achievable-rpos.png)

## <a name="input"></a>Input (Bemenet)
hello bemeneti munkalap biztosít hello áttekintést csatolást VMware-környezetben.

![Hello áttekintése csatolást VMware-környezetben](./media/site-recovery-deployment-planner/Input.png)

**Kezdő dátum** és **záró dátum**: hello hello profilkészítési adatok jelentéskészítésre figyelembe venni a kezdő és záró dátumát. Alapértelmezés szerint hello kezdő dátum értéke hello profilkészítési, időpont és hello záró dátum hello dátum, amikor adatgyűjtés leáll. Hello "StartDate" és "EndDate" értékek Ez lehet, ha hello jelentés ezekkel a paraméterekkel jön létre.

**Profilkészítési napok száma összesen**: a profilkészítési hello közötti napok számát hello start, és befejezési dátuma az melyik hello jelentés jön létre.

**Kompatibilis a virtuális gépek száma**: hello száma kompatibilis virtuális gépek melyik hello szükséges hálózati sávszélesség, tárolási száma fiókok, a Microsoft Azure mag, konfigurációs kiszolgálók és a további folyamat kiszolgálók számítja ki.

**Összes kompatibilis virtuális gépén lemezek számától**: hello egyik használt hello száma a bemeneti toodecide hello konfigurációs kiszolgálók számát és további folyamat kiszolgálók toobe hello központi telepítésben használja.

**Lemezek kompatibilis virtuális gépenként átlagos száma**: az összes kompatibilis virtuális gépek között számított hello lemezek átlagos száma.

**Lemez mérete (GB) átlagos**: hello átlagos mérete számított összes kompatibilis virtuális gépek között.

**Helyreállítási Időkorlát (perc) szükséges**: a jelentés generációs tooestimate szükséges hello időpontjában hello "DesiredRPO" paraméternek átadott vagy hello alapértelmezett helyreállítási cél vagy hello értékét sávszélesség.

**Szükséges sávszélesség (Mbps)**: hello érték, amely a jelentés generációs tooestimate hello időpontjában hello "Sávszélesség" paraméternek átadott elérhető helyreállítási Időkorlát.

**Megfigyelt tipikus adatforgalommal naponta (GB)**: hello átlagos adatforgalommal megfigyelt összes profilkészítési napon keresztül. Ez a szám lesz egyik hello bemenetek toodecide hello konfigurációs kiszolgálók számát és további folyamat kiszolgálók toobe hello központi telepítésben használja.


## <a name="vm-storage-placement"></a>Virtuálisgép-tároló elhelyezése

![Virtuálisgép-tároló elhelyezése](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Lemez tárolási típusa**: vagy egy standard vagy prémium tárfiókot, amely használt tooreplicate összes hello hello szerepel megfelelő virtuális gépek **virtuális gépek tooPlace** oszlop.

**Javasolt előtag**: hello javasolt három karakteres előtag hello tárfiók elnevezési használható. A saját előtag használható, de hello eszköz javaslat a következő hello [storage-fiókok elnevezési partícióazonosító](https://aka.ms/storage-performance-checklist).

**Javasolt a fióknév**: hello storage-fiók neve után is hello javasolt előtag. Cserélje le a belül hello csúcsos zárójel (< és >) a egyéni bevitellel hello nevét.

**A Tárfiók jelentkezzen**: minden hello replikálási naplókhoz standard szintű tárfiók vannak tárolva. Virtuális gép esetében, amelyek replikálják tooa prémium szintű storage-fiók beállítása egy további standard szintű tárfiók naplók tárolásához. Egyetlen standard szintű naplótárolási fiókot több prémium szintű replikációs tárfiók is használhat. Virtuális gépek, amelyek replikált toostandard tárolóprotokollok fiókok hello ugyanazt a tárfiókot, a naplók.

**Napló fióknév javasolt**: A tárfiók napló neve után is hello javasolt előtag. Cserélje le a belül hello csúcsos zárójel (< és >) a egyéni bevitellel hello nevét.

**Elhelyezési összegzés**: hello összegzését teljes virtuális gép terhelését hello tárfiók hello időpontban replikációs és feladatátvételi vagy a feladatátvételi teszt. Ez magában foglalja a virtuális gépek csatlakoztatott toohello tárfiókot, teljes olvasási/írási iops-érték közötti minden virtuális gép, ezt a tárfiókot, teljes jelennek írási (replikáció) iops-érték, a telepítő teljes mérete összes lemezt, és a lemezek számától hello teljes száma.

**Virtuális gépek tooPlace**: az összes hello virtuális gépek, amelyek az optimális teljesítmény és használja a tárfiók megadott hello kell elhelyezni.

## <a name="compatible-vms"></a>Kompatibilis virtuális gépek
![A kompatibilis virtuális gépek Excel-táblázata](./media/site-recovery-deployment-planner/compatible-vms.png)

**Virtuális gép neve**: hello a virtuális gép nevét vagy IP-cím hello VMListFile a jelentés létrehozásakor használt. Az oszlop megjeleníti a hello lemezeket (VMDKs) toohello csatolt virtuális gépek is. toodistinguish vCenter virtuális gépek ismétlődő nevek vagy IP-címek, hello nevek a következők hello ESXi állomásnevet. hello felsorolt ESXi-állomáson egy hello ahol hello VM elhelyezett hello eszköz észlelt a program hello profilkészítési időszak során.

**VM Compatibility** (Virtuálisgép-kompatibilitás): Az érték **Yes** (Igen) és **Yes**\* (Igen) lehet. **Igen** \* szolgál olyan esetekben, mely hello a virtuális gép egy beválik, ha nem [prémium szintű Azure Storage](https://aka.ms/premium-storage-workload). Itt hello csatolást magas-forgalom IOPS lemez beleilleszkedik hello P20 vagy P30 kategória, de hello hello lemez mérete okozza azt toobe le tooa P10 vagy P20 leképezve. hello tárfiók mellett dönt, hogy mely prémium szintű tároló lemez írja be toomap egy lemezt, mérete alapján. Példa:
* 128 GB alatt P10.
* 128 GB too512 GB egy P20.
* 512 GB too1024 GB egy P30.
* 1025 GB too2048 GB egy P40.
* 2049 GB too4095 GB egy P50.

Ha egy lemezt hello munkaterhelés jellemzői használatba hello P20 vagy P30 kategória, de hello mérete leképezhető tooa alacsonyabb prémium szintű storage lemeztípus le, hello eszköz jelöli meg, hogy virtuális Gépet állítsanak **Igen**\*. hello eszköz azt javasolja, hogy Ön vagy hello forrás lemez mérete toofit váltson át a prémium szintű storage lemeztípus ajánlott hello, vagy módosítsa hello tároló lemez típusa feladatátvételt követően.

**Storage Type** (Tároló típusa): Standard vagy Premium.

**Javasolt előtag**: hello háromkarakteres storage-fiók előtagja.

**A Tárfiók**: hello neve, amely hello javasolt tárfiók előtagot használja.

**R/W IOPS (a növekedési tényező)**: hello maximális munkaterhelést olvasási/írási IOPS hello lemezen (alapértelmezett érték 95. százalékos érték), beleértve hello jövőbeli növekedési tényező (alapértelmezett érték 30 százalékos). Vegye figyelembe, hogy a teljes olvasási/írási iops-érték a virtuális gépek hello nem mindig hello VM egyes lemezek olvasási/írási IOPS hello összege, mert hello maximális olvasási/írási iops-érték a virtuális gép hello hello csúcs az egyes lemezek hello összegére olvasási/írási IOPS során minden hello perce profilkészítési időszak.

**Adatok Kavarog egyidejűleg (a növekedési tényező) MB/s mértékegységben**: hello csúcs változási sebessége a lemezen hello (alapértelmezett érték 95. százalékos érték), beleértve a hello jövőbeli növekedési tényező (alapértelmezett érték 30 százalékos). Vegye figyelembe, hogy hello teljes adatforgalommal a virtuális gép hello nincs mindig hello összege hello VM egyes lemezek adatforgalommal, mert hello csúcs adatforgalommal a virtuális gép hello hello csúcs az egyes lemezek forgalom hello összegére során a profilkészítési időszak hello percenként.

**Az Azure Virtuálisgép-méretet**: hello ideális leképezése Azure Cloud Services virtuális gép méretét a helyszíni virtuális gép. hello leképezési hello helyszíni Virtuálisgép-memória, lemez/magok/hálózati adaptert, és olvasási/írási IOPS számát alapul. hello ajánljuk, mindig hello legalacsonyabb Azure virtuális gép méretét, amely megfelel az összes hello a helyszíni virtuális gép jellemzővel.

**Lemezek száma**: hello virtuálisgép-lemezeket (VMDKs) a virtuális gép hello teljes száma.

**Lemez (GB) méretű**: hello hello virtuális gép összes lemeze a telepítő teljes méretét. hello eszköz hello virtuális gép lemezmérete hello hello egyes lemezek is szerepel.

**Magok**: hello száma CPU processzormag, a virtuális gép hello.

**Memória (MB)**: hello RAM a virtuális gép hello.

**Hálózati adapter**: hello hello VM a hálózati adapterek száma.

**Rendszerindító típus**: hello virtuális gép rendszerindító típusú legyen. Ez BIOS vagy EFI lehet. Az Azure Site Recovery jelenleg csak a BIOS rendszerindítási típust támogatja. Minden hello virtuális gépek EFI rendszerindítási típusa nem kompatibilis a virtuális gépek munkalap láthatók.

**Operációs rendszer típusa**: hello hello virtuális gép operációsrendszer-típus. Ennek értéke Windows, Linux vagy egyéb lehet.

## <a name="incompatible-vms"></a>Nem kompatibilis virtuális gépek

![A nem kompatibilis virtuális gépek Excel-táblázata](./media/site-recovery-deployment-planner/incompatible-vms.png)

**Virtuális gép neve**: hello a virtuális gép nevét vagy IP-cím hello VMListFile a jelentés létrehozásakor használt. Az oszlop is megjeleníti, amelyek csatlakoztatott toohello virtuális gépek hello VMDKs. toodistinguish vCenter virtuális gépek ismétlődő nevek vagy IP-címek, hello nevek a következők hello ESXi állomásnevet. hello felsorolt ESXi-állomáson egy hello ahol hello VM elhelyezett hello eszköz észlelt a program hello profilkészítési időszak során.

**Virtuális gép kompatibilitási**: azt jelzi, miért kap a virtuális gép hello nem kompatibilis használatra a Site Recovery. hello okokat nem kompatibilis a virtuális gép hello lemezek leírt és, alapú a közzétett [tárhelyet](https://aka.ms/azure-storage-scalbility-performance), hello következő lehet:

* A lemez mérete nagyobb 4095 GB-nál. Az Azure Storage jelenleg nem támogatja a 4095 GB-nál nagyobb adatlemez-méretet.
* Az operációsrendszer-lemez mérete nagyobb 2048 GB-nál. Az Azure Storage jelenleg nem támogatja a 2048 GB-nál nagyobb méretű operációsrendszer-lemezeket.
* A rendszerindítás típusa EFI. Az Azure Site Recovery jelenleg csak a BIOS rendszerindítási típusú virtuális gépeket támogatja.

* Teljes Virtuálisgép-méretet (replikáció + TFO) meghaladja a támogatott hello tárfiók méretkorlát (35 TB). A kompatibilitási általában akkor fordul elő, ha a virtuális gép hello egyetlen lemezén egy jellemző, amelynek hossza meghaladja a hello maximálisan támogatott Azure vagy a Site Recovery érték szabványos tárolására. Ilyen példány hello VM hello a prémium szintű storage zónába leküldéses értesítések. Azonban a hello maximális, a prémium szintű tárfiók támogatott mérete 35 TB, és egy védett virtuális gép nem látható el védelemmel, több tárfiókok között. Vegye figyelembe azt is, hogy ha egy védett virtuális gép feladatátvételi teszt végrehajtása, fusson hello ugyanazt a tárfiókot, ahol elmélyedne a replikáció. Ebben a példában 2 darab hello lemez replikációs tooprogress hello méretének beállítása, és feladatátvételi toosucceed tesztelése párhuzamosan.
* A forrás IOPS-érték meghaladja a tároló lemezenkénti 5000-es IOPS-korlátját.
* A forrás IOPS-érték meghaladja a tároló virtuális gépenkénti 80 000-es IOPS-korlátját.
* Átlagos adatmennyiség meghaladja a megengedett Site Recovery adatok forgalom értéket a 10 MB/s átlagos hello lemez i/o-mérethez.
* Teljes adatforgalommal közötti összes lemezt a virtuális gép hello meghaladja hello maximális támogatott Site Recovery adatok forgalom virtuális gépenként 54 Mbit/s.
* Átlagos hatékony írási IOPS mérete meghaladja a lemez 840 hello támogatott hely helyreállítási IOPS korlátját.
* Számított pillanatkép-tároláshoz meghaladja a támogatott hello pillanatkép tárolási legfeljebb 10 TB.

**R/W IOPS (a növekedési tényező)**: hello maximális munkaterhelést IOPS hello lemez (alapértelmezett érték 95. százalékos érték), beleértve a hello jövőbeli növekedési tényező (alapértelmezett érték 30 százalékos). Vegye figyelembe, hogy a teljes olvasási/írási iops-érték a virtuális gép hello hello nem mindig hello VM egyes lemezek olvasási/írási IOPS hello összege, mert hello maximális olvasási/írási iops-érték a virtuális gép hello hello csúcs az egyes lemezek hello összegére olvasási/írási IOPS során minden hello perce profilkészítési időszak.

**A MB/s (a növekedési tényező) Kavarog adatok**: hello csúcs változási sebessége hello lemezen (alapértelmezett 95. százalékos érték), beleértve a hello jövőbeli növekedési tényező (alapértelmezett 30 százalék). Vegye figyelembe, hogy hello teljes adatforgalommal a virtuális gép hello nincs mindig hello összege hello VM egyes lemezek adatforgalommal, mert hello csúcs adatforgalommal a virtuális gép hello hello csúcs az egyes lemezek forgalom hello összegére során a profilkészítési időszak hello percenként.

**Lemezek száma**: hello a virtuális gép hello VMDKs teljes száma.

**Lemez (GB) méretű**: hello hello virtuális gép összes lemeze a telepítő teljes méretét. hello eszköz hello virtuális gép lemezmérete hello hello egyes lemezek is szerepel.

**Magok**: hello száma CPU processzormag, a virtuális gép hello.

**Memória (MB)**: hello hello VM a RAM mennyisége.

**Hálózati adapter**: hello hello VM a hálózati adapterek száma.

**Rendszerindító típus**: hello virtuális gép rendszerindító típusú legyen. Ez BIOS vagy EFI lehet. Az Azure Site Recovery jelenleg csak a BIOS rendszerindítási típust támogatja. Minden hello virtuális gépek EFI rendszerindítási típusa nem kompatibilis a virtuális gépek munkalap láthatók.

**Operációs rendszer típusa**: hello hello virtuális gép operációsrendszer-típus. Ennek értéke Windows, Linux vagy egyéb lehet.


## <a name="site-recovery-limits"></a>A Site Recovery korlátai

**Replikáció tárolási célja** | **Forráslemez átlagos I/O-mérete** |**Forráslemez átlagos adatváltozása** | **Forráslemez teljes napi adatváltozása**
---|---|---|---
Standard szintű Storage | 8 KB | 2 MBps | Lemezenként 168 GB
Prémium szintű P10 lemez | 8 KB | 2 MBps | Lemezenként 168 GB
Prémium szintű P10 lemez | 16 KB | 4 MBps | Lemezenként 336 GB
Prémium szintű P10 lemez | 32 KB vagy több | 8 MBps | Lemezenként 672 GB
Prémium szintű P20 vagy P30 lemez | 8 KB  | 5 MBps | Lemezenként 421 GB
Prémium szintű P20 vagy P30 lemez | 16 KB vagy több |10 MBps | Lemezenként 842 GB

Ezek átlagos értékek, amelyek 30 százalékos I/O-átfedést feltételeznek. A Site Recovery képes magasabb átviteli sebesség kezelésére az átfedési arány, a nagyobb írási méretek és a számítási feladatok tényleges I/O-viselkedése alapján. hello előző számok feltételezi a tipikus várakozó körülbelül öt percet. Ez azt jelenti, hogy a feltöltést követő öt percben megtörténik az adat feldolgozása, és létrejön egy helyreállítási pont.

Ezek a korlátok a saját tesztjeinken alapulnak, de nem fedhetik le az alkalmazások minden lehetséges I/O-kombinációját. A tényleges eredmények a saját alkalmazásának I/O-műveletei alapján változhatnak. A legjobb eredmények elérése érdekében után is üzembe helyezésének tervezése, mindig ajánlott kell végrehajtania a széles körű alkalmazás tesztelése a teszt feladatátvételi tooget hello igaz teljesítmény kép használata.

## <a name="updating-hello-deployment-planner"></a>Hello telepítési planner frissítése
tooupdate hello telepítési planner hello a következő:

1. Töltse le a legújabb verziójának hello hello [Azure Site Recovery telepítési planner](https://aka.ms/asr-deployment-planner).

2. Másolás hello .zip mappa tooa kiszolgáló, amelyet az toorun azt meg.

3. Bontsa ki a hello .zip mappa.

4. Hello alábbi módszerekkel:
 * Ha hello legújabb verziója nem tartalmazza a profilkészítési javítás és profilkészítési már folyamatban van a hello planner az aktuális verziójára, továbbra is hello adatainak összegyűjtése.
 * Ha hello legújabb verzióját a profilkészítési javítást tartalmaz, azt javasoljuk, hogy állítsa le a jelenlegi verziót a profilkészítési, és indítsa újra a hello profilkészítési hello új verziójával.

  >[!NOTE]
  >
  >Hello új verziójára, pass hello azonos kimeneti könyvtár elérési útja, így hello eszköz hozzáfűzi a profiladatok a profilkészítési indításakor hello a meglévő fájlokat. A teljes adatkészletet profilozott lesz toogenerate hello jelentés használt. Ha át egy másik kimeneti könyvtár, új fájlok jönnek létre, és a régi adatok csatolást nem használható toogenerate hello jelentés.
  >
  >Minden egyes új központi telepítési planner az összesítő frissítés hello .zip fájl. Nincs szükség a toocopy hello legújabb fájlok toohello előző mappára. Létrehozhat és használhat egy új mappát is.


## <a name="version-history"></a>Verzióelőzmények

### <a name="131"></a>1.3.1
Frissítve: 2017. július 19.

A következő új szolgáltatást tartalmazza:

* Nagy méretű (1 TB-nál nagyobb) lemezek támogatása a jelentéskészítésben. Most már a lemez mérete nagyobb, mint 1 TB-os (legfeljebb 4095 GB) rendelkező virtuális gépek telepítési planner tooplan replikációs is használhatja.
További információ a [Nagyméretű lemezek támogatásáról az Azure Site Recoveryben](https://azure.microsoft.com/en-us/blog/azure-site-recovery-large-disks/)


### <a name="13"></a>1.3
Frissítve: 2017. május 9.

A következő új szolgáltatást tartalmazza:

* Most már elérhető a felügyelt lemezek támogatása a jelentéskészítés során. hello virtuális gépek száma helyezhető tooa egyetlen tárolási fiók, ha a lemez felügyelt alapján számított van kiválasztva a feladatátvételi és tesztelési célú feladatátvevő.        


### <a name="12"></a>1.2
Frissítve: 2017. április 7.

A következő javításokat tartalmazza:

* Hozzáadott rendszerindító minden virtuális gép toodetermine ellenőrzése (a BIOS vagy az EFI) írja be, ha kompatibilis vagy nem kompatibilis hello védelemre hello virtuális gép.
* Az operációs rendszer hozzáadott hello kompatibilis virtuális gépek mindegyik virtuális gép adatai és a nem kompatibilis a virtuális gépek munkalapok írja be.
* hello GetThroughput művelet mostantól támogatott a hello Amerikai Egyesült államokbeli kormányzati és Kína Microsoft Azure-régióban.
* A rendszer most már néhány új előfeltételt is ellenőriz a vCenter- és az ESXi-kiszolgálók esetében.
* Helytelen jelentés készítésének első amikor területi beállítása toonon angol.


### <a name="11"></a>1.1
Frissítve: 2017. március 9.

A következő problémák rögzített hello:

* hello eszköz nem profilját a virtuális gépek, ha hello vCenter két vagy több virtuális gépek ugyanazt a nevet vagy IP-cím hello különböző ESXi-gazdagépek közötti.
* Másolás és a keresési le van tiltva hello kompatibilis virtuális gépek és a nem kompatibilis a virtuális gépek munkalapok tartalmazzák.

### <a name="10"></a>1.0
Frissítve: 2017. február 23.

Az Azure Site Recovery telepítési Planner 1.0 nyilvános előzetes hello alábbi ismert problémák (a jövőbeli frissítéseket az érintett toobe) rendelkezik:

* hello eszköz csak VMware-Azure forgatókönyvek, a Hyper-V-az-Azure-telepítésekre nem működik. Hyper-V-az-Azure-bA forgatókönyvek esetén használja a hello [Hyper-V kapacitás planner eszköz](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* hello GetThroughput művelet nem támogatott a hello Amerikai Egyesült államokbeli kormányzati és Kína Microsoft Azure-régióban.
* hello eszköz nem profilját a virtuális gépek, ha hello vCenter-kiszolgáló két vagy több virtuális gépek ugyanazt a nevet vagy IP-cím hello különböző ESXi-gazdagépek közötti. Ebben a verzióban hello eszköz kihagyja az ismétlődő virtuális gép nevét vagy IP-címek hello VMListFile profilkészítési. hello megoldás tooprofile hello virtuális gépeket, hello vCenter-kiszolgáló segítségével, az ESXi-állomáson. Minden ESXi-gazdagép esetében futtatnia kell egy példányt.
