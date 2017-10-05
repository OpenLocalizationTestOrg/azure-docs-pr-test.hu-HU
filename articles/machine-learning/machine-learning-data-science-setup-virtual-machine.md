---
title: "Állítson be egy virtuális gépet IPython Notebook kiszolgálóként |} Microsoft Docs"
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
ms.openlocfilehash: 66fd9e5573390ac6faeb82ad5b0f7ddb18d50a77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Azure virtuális gép beállítása IPython Notebook-kiszolgálóként a speciális elemzésekhez
Ez a témakör azt ismerteti, hogyan felkészítse és konfigurálja az Azure virtuális gép speciális elemzésekre a adatok tudományos környezet részeként használható. A Windows rendszerű virtuális gép támogatási eszközöket, például a IPython Notebook, Azure Tártallózó, AzCopy, valamint más hasznos a speciális elemzés projektek segédeszközök van konfigurálva. Az Azure Tártallózó és AzCopy, például módszereket biztosítanak az kényelmes feltölteni az adatokat az Azure blob storage a helyi számítógépről, vagy a blob storage-ból tölthetik le a helyi számítógépen.

## <a name="create-vm"></a>1. lépés: Általános célú Azure virtuális gép létrehozása
Ha már rendelkezik egy Azure virtuális gépen, és csupán be szeretne rajta IPython Notebook kiszolgáló beállítása, ezt a lépést kihagyhatja, és folytassa a [2. lépés: a végpont hozzáadása a meglévő virtuális gépből IPython notebookok](#add-endpoint).

A virtuális gép létrehozása Azure-folyamat elindítása előtt meg kell határoznia a gép feldolgozni az adatokat a projekthez szükséges mérete. Kisebb gépek kevesebb memóriával és kevesebb, mint nagyobb gépek CPU-magokat rendelkezhetnek, de ugyanakkor kevésbé költséges. Gépek típusainak és árakat listájáért lásd: a <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtual Machines díjszabása </a> lap

1. Jelentkezzen be <a href="https://manage.windowsazure.com" target="_blank">a klasszikus Azure portálon</a>, és kattintson a **új** bal alsó sarkában. Egy ablakban jelenik meg. Válassza ki **számítási** -> **virtuális gép** -> **FROM GYŰJTEMÉNY**.
   
    ![Munkaterület létrehozása][24]
2. A következő lemezképek közül választhat:
   
   * Windows Server 2012 R2 Datacenter
   * Windows Server Essentials felhasználói felülete (Windows Server 2012 R2)
     
     Kattintson a, nyissa meg a következő konfigurációs lap jobb alsó részen jobbra mutató nyílra.
     
     ![Munkaterület létrehozása][25]
3. Adjon meg egy nevet a virtuális géphez szeretne létrehozni, válassza ki a gép méretét (alapértelmezett: A3) szeretné az adatokat, a gép érintetlen folyamat méret és milyen teljesítményű alapján (memória mérete és a számítási magok száma), adja meg egy felhasználónevet és jelszót a gép a gépet. Kattintson a jobb nyissa meg a következő konfigurációs lapra mutató nyílra.
   
    ![Munkaterület létrehozása][26]
4. Válassza ki a **régió/AFFINITÁS csoport/virtuális hálózati** , amely tartalmazza a **TÁRFIÓK** , hogy azt tervezi, hogy a virtuális gép számára, és válassza ki, hogy a tárfiók. A lap alján található végpont hozzáadása a **VÉGPONTOK** mezőbe írja be a a végpont neve ("IPython" Itt). Dönthet úgy, mint bármilyen karakterlánc a **neve** a végpontot, és bármely egész számot 0 és 65536 között, amely **elérhető** , a **nyilvános PORT**. A **MAGÁNHÁLÓZATI PORT** kell **9999**. Meg kell **elkerülése érdekében** már tartozik nyilvános portokat használ az internetes szolgáltatások. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Az internetes szolgáltatások által használt portokat</a> van rendelve, és el kell kerülni portokat listáját tartalmazza.
   
    ![Munkaterület létrehozása][27]
5. Kattintson a pipa jelre a virtuális gép elindítása a létesítésének folyamatát kell használnia.
   
    ![Munkaterület létrehozása][28]

A virtuális gép kiépítési folyamat befejezéséhez 15-25 percig is eltarthat. A virtuális gép létrehozása után a gép kell állapota **futtató**.

![Munkaterület létrehozása][29]

## <a name="add-endpoint"></a>2. lépés: A végpont hozzáadása IPython notebookok egy meglévő virtuális géphez
Ha a virtuális gép útmutató 1. lépésben létrehozott, a végpont IPython jegyzetfüzet már hozzá lett adva, és figyelmen kívül hagyja ezt a lépést.

Ha a virtuális gép már létezik, és hozzá kell adnia egy végpontot, amely alatt a klasszikus Azure portálon való első bejelentkezéskor telepíteni fogja a 3. lépésben IPython jegyzetfüzet, válassza ki a virtuális gépet, és adja hozzá a végpont IPython Notebook kiszolgáló. Az alábbi ábra tartalmaz egy Képernyőkép a portálon, a végpont IPython jegyzetfüzet Windows virtuális gépként való hozzáadása után.

![Munkaterület létrehozása][17]

## <a name="run-commands"></a>3. lépés: IPython jegyzetfüzet és egyéb támogató eszközök telepítése
A virtuális gép létrehozása után a távoli asztal protokoll (RDP) használatával jelentkezzen be a Windows virtuális gépre. Útmutatásért lásd: [jelentkezzen be a virtuális gép futó Windows Server hogyan](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Nyissa meg a **parancssor** (**nem a Powershell-parancsablakot**), egy **rendszergazda** , és futtassa a következő parancsot.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

A telepítés befejeződése után a IPython Notebook kiszolgáló automatikusan a elindult-e a *C:\\felhasználók\\\<felhasználónév\>\\dokumentumok\\IPython notebookok* könyvtár.

Amikor a rendszer kéri, írja be a jelszót a IPython jegyzetfüzet és a számítógép rendszergazdai jelszót. Ez lehetővé teszi a IPython jegyzetfüzet szolgáltatásként futtathatja a számítógépen.

## <a name="access"></a>4. lépés: Hozzáférési IPython notebookok egy webböngészőből
A kiszolgálóhoz való hozzáféréshez IPython Notebook, nyisson meg egy webes böngésző és a bemeneti *https://&#60;virtual számítógép DNS-neve >: &#60; nyilvános port számát >* az URL-cím szövegmezőbe. Itt a *&#60; nyilvános portszám >* kell lennie a portszámot, amikor a IPython Notebook végpont hozzá lett adva.

A *&#60; virtuális gép DNS-neve >* található a klasszikus Azure portálon. Kattintson a bejelentkezés után a klasszikus portálra **virtuális gépek**, jelölje ki a gépet hozott létre, és válassza ki **IRÁNYÍTÓPULT**, a DNS-neve jelenik meg az alábbiak szerint:

![Munkaterület létrehozása][19]

Arról, hogy figyelmeztetést fog történni *van a webhely biztonsági tanúsítványával kapcsolatos problémára* (az Internet Explorer) vagy *a kapcsolat nincs titkos* (Chrome), ahogy az az alábbi ábrán. Kattintson a **továbblépés a webhelyre (nem ajánlott)** (az Internet Explorer) vagy **speciális** , majd  **folytatása a &#60;* DNS-név*> (nem biztonságos) ** (Chrome) a folytatáshoz. Ezután jelszót kér a felhasználótól a korábban megadott telepítőfájlja a IPython notebookok eléréséhez.

**Az Internet Explorer:**
![munkaterület létrehozása][20]

**Chrome:**
![munkaterület létrehozása][21]

Jelentkezzen be a könyvtár IPython Notebook után *DataScienceSamples* jelennek meg a böngészőben. Ez a könyvtár minta IPython notebookok Microsoft tudományos feladatok elvégzésével felhasználók által megosztott tartalmaz. A minta IPython notebookok a kivett [ **GitHub-tárházban** ](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) IPython Notebook server-beállítási folyamata során a virtuális gépekhez. A Microsoft kezeli, és ebben a tárházban gyakran frissül. Felhasználók előfordulhat, hogy keresse fel a GitHub-tárházban a közelmúltban frissített minta IPython notebookok eléréséhez.
![Munkaterület létrehozása][18]

## <a name="upload"></a>5. lépés: Töltse fel a meglévő IPython jegyzetfüzet a helyi gép a IPython Notebook kiszolgálóra
A felhasználók számára a helyi számítógépeken meglévő IPython jegyzetfüzet feltölteni a virtuális gépeken a IPython Notebook kiszolgálóra könnyű megoldást biztosítson a IPython notebookok. A IPython Notebook webböngészőben való bejelentkezés után kattintson a be a **directory** a IPython Notebook feltöltött. Töltse fel a helyi gépről IPython Notebook .ipynb fájlt, majd válasszon ki a **Fájlkezelőben**, és húzással azt a webböngésző IPython Notebook könyvtárához. Kattintson a **feltöltése** gombra, a .ipynb fájlt feltölteni a IPython Notebook kiszolgálóra. Más felhasználók is indíthatja a webböngésző használatával legyen.

![Munkaterület létrehozása][22]

![Munkaterület létrehozása][23]

## <a name="shutdown"></a>Állítsa le és vonja kiosztani a virtuális gép, ha nincsenek használatban
Azure virtuális gépeken árú **csak a valóban használt funkciókért kell fizetnie, amennyit**. Győződjön meg arról, hogy meg vannak nem kiszámlázott amikor nem használja a virtuális gépet, hogy rendelkezik kell lennie a **leállítva (Deallocated)** állapot, ha nincsenek használatban.

> [!NOTE]
> Ha leállítja a virtuális gépet a virtuális Gépen belül (a Windows energiagazdálkodási lehetőségek használatával), a virtuális gép le van állítva, de lefoglalt marad. Nem továbbra is fizetnie kell ezért érdekében minden esetben állítsa le a virtuális gépek a [a klasszikus Azure portálon](http://manage.windowsazure.com/). A virtuális Gépet a PowerShell segítségével állítsa le meghívásával **ShutdownRoleOperation** a "PostShutdownAction" egyenlő "StoppedDeallocated".
> 
> 

Állítsa le, és a virtuális gép felszabadítása:

1. Jelentkezzen be a [a klasszikus Azure portálon](http://manage.windowsazure.com/) a fiókjával.  
2. Válassza ki **virtuális gépek** bal oldali navigációs sávján.
3. A virtuális gépek listájának megtekintéséhez kattintson a virtuális gép nevére, majd nyissa meg a **IRÁNYÍTÓPULT** lap.
4. Kattintson a lap alján **LEÁLLÍTÁSI**.

![Virtuális gép leállítása][15]

A virtuális gép lesz felszabadítása. lehetséges, de nem törlődnek. A klasszikus Azure portálon bármikor előfordulhat, hogy indítsa újra a virtuális gép.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Az Azure virtuális gép készen áll a használatra: következő?
A virtuális gép már a adatok tudományos gyakorlatokban használatra kész. A virtuális gép is az Azure Machine Learning és az Team tudományos folyamat feltárása és feldolgozására, az adatok és más feladatok együtt IPython Notebook kiszolgálóként használatra kész.

A következő lépéseket az adatok tudományos folyamatban leképezett a [tanulási elérési](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , HDInsight, a folyamat az adatok áthelyezése, illetve mintát, hogy az adatokat az Azure Machine Learning tanulva előkészítésekor lépéseket is lehetnek.

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
