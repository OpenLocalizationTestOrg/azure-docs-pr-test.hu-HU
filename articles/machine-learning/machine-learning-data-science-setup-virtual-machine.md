---
title: "a virtuális gép IPython Notebook kiszolgálóként aaaSet |} Microsoft Docs"
description: "Állítsa be az Azure virtuális gép tudományos környezetben használható IPython kiszolgálóval speciális elemzésekre."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 818617c1-048e-49c2-b941-f9a983f93998
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
ms.openlocfilehash: 58386140ec7742ade1f7e183ec842a2b09b9dfca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Azure virtuális gép beállítása IPython Notebook-kiszolgálóként a speciális elemzésekhez
Ez a témakör bemutatja, hogyan tooprovision és speciális elemzésekre a adatok tudományos környezet részeként használható Azure virtuális gép konfigurálása. támogató eszközöket, például a IPython Notebook, Azure Tártallózó, AzCopy, valamint más hasznos a speciális elemzés projektek segédeszközök hello Windows rendszerű virtuális gép van beállítva. Az Azure Tártallózó és AzCopy, például módszereket biztosítanak az kényelmes tooupload adatok tooAzure blob storage-ának a helyi számítógépen vagy toodownload azt tooyour helyi számítógép blobtárolóból.

## <a name="create-vm"></a>1. lépés: Általános célú Azure virtuális gép létrehozása
Ha már rendelkezik egy Azure virtuális gépen, és most szeretné, hogy egy IPython Notebook kiszolgáló rajta tooset, ezt a lépést kihagyhatja, és folytassa túl[2. lépés: egy IPython notebookok tooan meglévő virtuális gép végpontjának felvétele](#add-endpoint).

A virtuális gép létrehozása Azure hello folyamat elindítása előtt kell toodetermine hello hello gépre, amelyik a projekthez szükséges tooprocess hello adatok méretét. Kisebb gépek kevesebb memóriával és kevesebb, mint nagyobb gépek CPU-magokat rendelkezhetnek, de ugyanakkor kevésbé költséges. Gépek típusainak és árakat, lásd: hello <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtual Machines díjszabása </a> lap

1. Jelentkezzen be túl<a href="https://manage.windowsazure.com" target="_blank">a klasszikus Azure portálon</a>, és kattintson a **új** hello bal alsó sarokban lévő. Egy ablakban jelenik meg. Válassza ki **számítási** -> **virtuális gép** -> **FROM GYŰJTEMÉNY**.
   
    ![Munkaterület létrehozása][24]
2. A következő lemezképek hello közül választhat:
   
   * Windows Server 2012 R2 Datacenter
   * Windows Server Essentials felhasználói felülete (Windows Server 2012 R2)
     
     Kattintson a jobbra mutató hello alacsonyabb jobb toogo hello tovább konfigurációs lapon hello nyíl.
     
     ![Munkaterület létrehozása][25]
3. Adjon meg egy nevet hello virtuális gép kívánt toocreate, hello gép válassza hello mérete (alapértelmezett: A3) hello adatok hello gép hello mérete alapján van, amelyet tooprocess és teljesítményének kívánt hello gép toobe (memória mérete és hello számítási magok száma) , adjon meg egy felhasználónevet és jelszót hello gép. Kattintson a jobb oldali toogo toohello következő konfigurálólapját hello nyíl.
   
    ![Munkaterület létrehozása][26]
4. Jelölje be hello **régió/AFFINITÁS csoport/virtuális hálózati** hello tartalmazó **TÁRFIÓK** , hogy a virtuális gép toouse tervezi, és válassza ki a tárfiók. Végpont hozzáadása a hello hello alján **VÉGPONTOK** hello név hello végpont ("IPython" Itt) mezőjében. Dönthet úgy, mint hello bármilyen karakterlánc **neve** hello végpontot, és bármely egész számot 0 és 65536 között, amely **elérhető** , hello **nyilvános PORT**. Hello **MAGÁNHÁLÓZATI PORT** toobe rendelkezik **9999**. Meg kell **elkerülése érdekében** már tartozik nyilvános portokat használ az internetes szolgáltatások. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Az internetes szolgáltatások által használt portokat</a> van rendelve, és el kell kerülni portokat listáját tartalmazza.
   
    ![Munkaterület létrehozása][27]
5. Kattintson a hello pipa toostart hello virtuális gépek létesítésének folyamatát kell használnia.
   
    ![Munkaterület létrehozása][28]

Szükség lehet a 15-25 percig toocomplete hello virtuális gépek létesítésének folyamatát kell használnia. Hello virtuális gép létrehozása után a gép hello állapotának megjelenítése kell **futtató**.

![Munkaterület létrehozása][29]

## <a name="add-endpoint"></a>2. lépés: IPython notebookok tooan meglévő virtuális gép végpont hozzáadása
Ha hello 1. lépés utasításai szerint létrehozott hello virtuális gépet, majd IPython jegyzetfüzet hello végpont már hozzá lett adva, és figyelmen kívül hagyja ezt a lépést.

Ha hello virtuális gép már létezik, és IPython Notebook, amely telepíteni fogja az alábbi a 3. lépés tooadd egy olyan végpont szükséges, első bejelentkezési tooAzure klasszikus portál, hello virtuális gépet kijelöli, és IPython Notebook kiszolgáló hello végpont hozzáadása. hello alábbi ábrán is látható tartalmaz egy képernyőfelvétel a hello portál hello végpont IPython jegyzetfüzet tooa Windows rendszerű virtuális gép hozzáadása után.

![Munkaterület létrehozása][17]

## <a name="run-commands"></a>3. lépés: IPython jegyzetfüzet és egyéb támogató eszközök telepítése
Hello virtuális gép létrehozása után használja a távoli asztal protokoll (RDP) toolog toohello Windows virtuális gépen. Útmutatásért lásd: [hogyan tooLog a virtuális gép futó Windows Server tooa](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Nyissa meg hello **parancssor** (**nem hello Powershell-parancsablak**), egy **rendszergazda** és futtatási hello a következő parancsot.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Ha hello telepítésének befejezése után IPython Notebook kiszolgáló a indul el automatikusan hello hello *C:\\felhasználók\\\<felhasználónév\>\\dokumentumok\\IPython Notebookok* könyvtár.

Amikor a rendszer kéri, adja meg a jelszót hello IPython jegyzetfüzet és hello hello gép rendszergazdai jelszavát. Ez lehetővé teszi, hogy hello IPython Notebook toorun hello gépen szolgáltatásként.

## <a name="access"></a>4. lépés: Hozzáférési IPython notebookok egy webböngészőből
tooaccess hello IPython Notebook kiszolgálót, nyissa meg a webes böngésző és a bemeneti *https://&#60;virtual számítógép DNS-neve >: &#60; nyilvános portszám >* hello URL-cím szövegmezőbe. Itt hello *&#60; nyilvános portszám >* amikor hello IPython Notebook végpont hozzá lett adva megadott hello portszámot kell lennie.

Hello *&#60; virtuális gép DNS-neve >* hello a klasszikus Azure portálon található. Klasszikus portál toohello a bejelentkezés után kattintson **virtuális gépek**, jelölje be hello gép hozott létre, és válassza ki **IRÁNYÍTÓPULT**, hello DNS-neve jelenik meg az alábbiak szerint:

![Munkaterület létrehozása][19]

Arról, hogy figyelmeztetést fog történni *van a webhely biztonsági tanúsítványával kapcsolatos problémára* (az Internet Explorer) vagy *a kapcsolat nincs titkos* (Chrome), ahogy az alábbi hello adatok. Kattintson a **Folytatás toothis webhelyre (nem javasolt)** (az Internet Explorer) vagy **speciális** , majd  **túl folytatható &#60;* DNS-név*> (nem biztonságos) ** (Chrome) toocontinue. Ezután jelszót kér a felhasználótól hello IPython notebookok hello megadott korábbi tooaccess meg.

**Az Internet Explorer:**
![munkaterület létrehozása][20]

**Chrome:**
![munkaterület létrehozása][21]

Toohello IPython Notebook, egy könyvtárat a bejelentkezés után *DataScienceSamples* fognak megjelenni az hello böngésző. Ez a könyvtár minta IPython notebookok Microsoft toohelp felhasználók viselkedési tudományos feladatok által megosztott tartalmaz. A minta IPython notebookok a kivett [ **GitHub-tárházban** ](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) toohello virtuális gépek hello IPython Notebook kiszolgáló telepítési folyamat során. A Microsoft kezeli, és ebben a tárházban gyakran frissül. Előfordulhat, hogy felkereshető hello GitHub tárház tooget hello nemrég frissített minta IPython notebookok.
![Munkaterület létrehozása][18]

## <a name="upload"></a>5. lépés: Töltse fel az a helyi számítógép toohello IPython Notebook kiszolgáló egy meglévő IPython Notebook
IPython notebookok könnyű megoldást biztosítson a felhasználók tooupload egy meglévő IPython jegyzetfüzetet a helyi gép toohello IPython Notebook server hello virtuális gépeken. Miután toohello IPython Notebook webböngészőben jelentkezik be, kattintson a hello **directory** adott IPython Notebook feltöltés hello. Ezt követően válassza a helyi gép hello hello egy IPython Notebook .ipynb fájl tooupload **Fájlkezelőben**, és húzással az toohello IPython Notebook directory hello böngészőben. Kattintson a hello **feltöltése** gomb, tooupload hello .ipynb fájl toohello IPython Notebook kiszolgáló. Más felhasználók is indíthatja a webböngésző használatával legyen.

![Munkaterület létrehozása][22]

![Munkaterület létrehozása][23]

## <a name="shutdown"></a>Állítsa le és vonja kiosztani a virtuális gép, ha nincsenek használatban
Azure virtuális gépeken árú **csak a valóban használt funkciókért kell fizetnie, amennyit**. nem éppen tooensure számlázva, amikor nem használja a virtuális gépet, rendelkezik toobe hello **leállítva (Deallocated)** állapot, ha nincsenek használatban.

> [!NOTE]
> Ha kikapcsolja a virtuális gép hello a virtuális gép (Windows energiagazdálkodási lehetőségek), hello belül hello virtuális gép le van állítva, de továbbra is lefoglalt. nem folytatja a számlázott, toobe tooensure minden esetben állítsa le a virtuális gépek hello [a klasszikus Azure portálon](http://manage.windowsazure.com/). Hello VM Powershell használatával állítsa le meghívásával **ShutdownRoleOperation** a "PostShutdownAction" egyenlő túl "StoppedDeallocated".
> 
> 

tooshut le, és hello virtuális gép felszabadítása:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com/) a fiókjával.  
2. Válassza ki **virtuális gépek** hello bal oldali navigációs sávon a.
3. A virtuális gépek hello listájában kattintson a virtuális gépet, majd lépjen toohello hello név **IRÁNYÍTÓPULT** lap.
4. Hello a hello lap alján, kattintson **LEÁLLÍTÁSI**.

![Virtuális gép leállítása][15]

hello virtuális gép lesz felszabadítása. lehetséges, de nem törlődnek. A klasszikus Azure portálon hello bármikor előfordulhat, hogy indítsa újra a virtuális gép.

## <a name="your-azure-vm-is-ready-toouse-whats-next"></a>Az Azure virtuális gép készen áll a toouse: következő?
A virtuális gép már készen áll a toouse az adatok tudományos gyakorlatokban. hello virtuális gép is az Azure Machine Learning és hello Team adatok tudományos folyamat egy IPython Notebook kiszolgálóként hello feltárása és az adatok feldolgozására és más feladatok együtt használatra kész.

Team adatok tudományos folyamat leképezett hello hello lépések hello [tanulási elérési](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , HDInsight, a folyamat az adatok áthelyezése, illetve az Azure Machine learning hello adatokból előkészítésekor minta ott lépéseket is lehetnek. Learning.

[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
