---
title: "Hozzon létre egy Jupyter/IPython Notebook |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítheti a Jupyter/IPython Notebook létrehozása az Azure resource manager üzembe helyezési modellben a Linux virtuális gépen."
services: virtual-machines-linux
documentationcenter: python
author: crwilcox
manager: wpickett
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 519f36dd-865e-4c1d-abe7-b87037796aa7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 11/10/2015
ms.author: crwilcox
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b5940190822cd5c5b78ea0e8f5c8695608d351d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a>Egy Azure virtuális gép létrehozása, Jupyter telepítése és futtatása a Jupyter Notebook az Azure-on
A [Jupyter projekt](http://jupyter.org), korábbi nevén a [IPython projekt](http://ipython.org), olyan eszközöket biztosít tudományos számítástechnikai kód végrehajtása miatt a létrehozása olyan élő számítási dokumentum felhasználó hatékony interaktív rendszerhéjának használatával. A notebook fájlok tetszőleges szöveg, matematikai képletek, bemeneti kódot, eredményeket, grafikus, videók és bármely más típusú, hogy a modern webböngésző megjelenítésére alkalmas adathordozó is tartalmazhat. Feltétlenül ismerkedik Python, és ismerje meg, hogy szórakoztató, interaktív környezetben vagy annak egy bizonyos súlyos párhuzamos és technikai számítástechnikai szeretné, hogy a Jupyter Notebook-e kiváló választás.

![Képernyőkép](./media/jupyter-notebook/ipy-notebook-spectral.png) használatával SciPy és Matplotlib csomagot kell elemezni egy hangrögzítés szerkezete.

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter két módon: Azure notebookok vagy egyéni központi telepítés
Azure kínál egy szolgáltatást, amely segítségével [Jupyter használatának gyors megkezdéséhez ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Az Azure Notebook szolgáltatás használatával egyszerűen hozzáférhetünk Jupyter tartozó interneten elérhető felület méretezhető számítási erőforrások eléréséről a Python a teljesítmény és a sok szalagtárak.  Mivel telepítését végzi el a szolgáltatást, felhasználók férhetnek hozzá ezekhez az erőforrásokhoz, felügyelete és konfigurálása a felhasználó által szükségessége nélkül.

Ha a notebook szolgáltatás nem működik a forgatókönyvnek ezentúl is ebben a cikkben, amely fog bemutatja, hogyan központi telepítése a Microsoft Azure-ban a Jupyter Notebook Linux virtuális gépek (VM) használatával.

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Hozza létre és konfigurálja a virtuális gépek az Azure-on
Az első lépés az Azure-on futó virtuális gép (VM) létrehozásához.
Ez a virtuális gép a felhőben található teljes operációs rendszer, és a Jupyter Notebook futtatásához használandó. Azure Linux és a Windows virtuális gépek futtatására képes, vagyis, és bemutatjuk, hogy a virtuális gépek mindkét típusú Jupyter telepítését.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Hozzon létre egy Linux virtuális Gépet, és a Jupyter port megnyitása
Kövesse az utasításokat, megadott [Itt] [ portal-vm-linux] a virtuális gép létrehozásához a *Ubuntu* terjesztési. Ez az oktatóanyag Ubuntu Server 14.04 LTS használja. Feltételezzük, hogy a felhasználónév *azureuser*.

Miután a virtuális gépet telepít kell nyissa meg a szabály a hálózati biztonsági csoport.  Ugrás az Azure portálról **hálózati biztonsági csoportok** , és nyissa meg a lap a biztonsági csoport a virtuális gép megfelelő. Hozzá kell adnia egy bejövő biztonsági szabály a következő beállításokkal: **TCP** a protokoll  **\***  a forrás (nyilvános) port és **9999** a célport (személyes).

![Képernyőfelvétel](./media/jupyter-notebook/azure-add-endpoint.png)

Az a hálózati biztonsági csoporthoz, kattintson a **hálózati illesztők** meg és jegyezze fel a **nyilvános IP-cím** csatlakozni a virtuális Gépet a következő lépésben lesz szükség szerint.

## <a name="install-required-software-on-the-vm"></a>Szükséges szoftverek telepítése a virtuális gépen
A Jupyter Notebook futtatásához a virtuális gépen, hogy először telepítenie kell Jupyter és annak függőségeit. Kapcsolódás a linux virtuális gép ssh és a felhasználónév/jelszó párosítsa azt a virtuális gép megadásakor. Ez az oktatóanyag a rendszer a PuTTY használata és csatlakozás Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Ubuntu Jupyter telepítése
Anaconda, olyan népszerű adatok tudományos python elosztási, a megadott hivatkozások valamelyikével telepítése [alakíthatnak ki olyan Analytics](https://www.continuum.io/downloads).  A jelen dokumentum írásáig a hivatkozásokat az alábbiakban a legtöbb legfeljebb dátum verziók.

#### <a name="anaconda-installs-for-linux"></a>Linux anaconda telepíti
<table>
  <th>Python 3.4</th>
  <th>Python 2.7-es</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bites</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bites</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Anaconda3 telepítése 2.3.0 Ubuntu 64 bites
Például azt, hogyan telepíthető Anaconda Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Képernyőfelvétel](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter konfigurálása és az SSL használata
Telepítése után kell beállítani a konfigurációs fájlokat a Jupyter néhány percet. A Jupyter konfigurálása során fellépő problémák tapasztal lássunk hasznos lehet a [Jupyter Notebook-kiszolgálót futtató dokumentációja](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Tovább a Microsoft `cd` a profil könyvtár az SSL-tanúsítvány létrehozásához és szerkesztéséhez a profilok konfigurációs fájlt.

Linux rendszeren használja az alábbi parancsot.

    cd ~/.jupyter

A következő paranccsal (Linux és Windows rendszerekhez) az SSL-tanúsítvány létrehozásához.

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Vegye figyelembe, hogy a beállítást, mivel egy önaláírt SSL-tanúsítványt, amikor csatlakozik a notebook a böngésző meg biztonsági figyelmeztetést ad.  A hosszú távú éles környezetben való használathoz érdemes a szervezetéhez tartozó megfelelően aláírt tanúsítványát használja.  Mivel ez a bemutató túlmutató tanúsítványkezelés, azt fogja anyagot egy önaláírt tanúsítványt most.

Mellett olyan tanúsítványt használ, meg kell adnia egy jelszót a notebook védelmében az illetéktelen használattól is.  Biztonsági okokból a Jupyter titkosított jelszavak a konfigurációs fájlban használ, ezért először titkosítja a jelszót kell be a számítógépre.  IPython biztosít egy segédprogram ehhez; a parancssorba a következő parancsot.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Kérni fogja a jelszó és a megerősítés, és azt nyomtassa ki a jelszót. Megjegyzés: Ez a következő lépéssel.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

A következő fogjuk módosítani a profil konfigurációs fájl, amely a `jupyter_notebook_config.py` fájl a könyvtárban.  Vegye figyelembe, hogy a fájl nem létezik – most létrehozták a esetben.  Ez a fájl rendelkezik mezők számát, és alapértelmezés szerint az összes jelenleg megjegyzésként szerepelnek.  Ez a fájl megnyitható tetszőlegesen a szövegszerkesztőben, és bizonyosodjon meg arról, hogy van legalább a következő tartalmat. **Ügyeljen arra, hogy a Config c.NotebookApp.password cserélje le az előző lépésben sha1**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Futtassa a Jupyter Notebook
Ezen a ponton a Jupyter Notebook készen áll azt. Ehhez keresse meg a kívánt jegyzetfüzeteket tárolja, és indítsa el a Jupyter Notebook kiszolgáló a következő paranccsal könyvtárát.

    /anaconda3/bin/jupyter-notebook

Meg kell tudni elérni a Jupyter Notebook a címen `https://[PUBLIC-IP-ADDRESS]:9999`.

Amikor először hozzáfér a notebook, a bejelentkezési oldal kéri a jelszót. És miután jelentkezik be, megjelenik a "Jupyter Notebook irányítópult", amely az összes jegyzetfüzet művelet vezérlőkarakterek központnak.  Ezen a lapon létrehozhat új, jegyzetfüzeteket és nyissa meg a már meglévőket.

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>A Jupyter Notebook használatával
Ha a **új** gomb, látni fogja a következő lap megnyitása.

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-untitled-notebook.png)

A terület jelölésű egy `In []:` kérdezzen rá a beviteli terület, és itt adhatja meg egyetlen érvényes Python kódját, és azt fogja hajtható végre, ha kattint `Shift-Enter` vagy kattintson a "Play" (a jobbra mutató háromszög az eszköztárban) ikonra.

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>A hatékony paradigma: élő multimédiás számítási dokumentumok
A notebook magának kell érzi, hogy nagyon természetes számára, akik használják a Python és a szövegszerkesztőt, mert bizonyos értelemben vegyesen mindkét: végrehajthat kódblokkokat, Python, de is megtarthatja megjegyzések és egyéb szöveg egy cella "Markdown" a "Kód" stílus módosításával az eszköztáron a legördülő menü segítségével.

Jupyter sokkal több mint egy szövegszerkesztőt, lehetővé teszi a számítási és a gazdag media (szöveg, képek, videó és gyakorlatilag bármi modern webböngésző megjelenítheti) keverése. Kombinálhatja, szöveg, kód, videókat és egyéb!

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-editing-experience.png)

És a Python tartozó sok kiváló szalagtárak műszaki és tudományos power számítástechnikai, az alábbi képernyőképen egyszerű számítás hajtható végre egy összetett hálózati elemzést követően az összes egy környezetben, azonos könnyű.

Ez a modern webes hatványa keverési az élő számítási paradigma számos lehetőséget kínál, és kiválóan alkalmas a felhőalapú; a Notebook használhatók:

* A számítási firkatömb felderítő rögzítéséhez, működnek. a probléma.
* Osztani az eredményeket, munkatársakat, vagy a "live" számítási formában, vagy vezetékhelyettesítési formátumokban (HTML, PDF).
* Terjesztéséhez, és élő oktatási anyagok, például a számítási, így a diákok azonnal kísérletezhet, és a tényleges kód van, módosítsa azt, és hajtsa végre újból az interaktív módon.
* Arra, hogy a "végrehajtható által írt cikkeket", hogy az eredmények kutatási oly módon, hogy melyek azonnal másolható, érvényesítése és bővítése mások számára.
* Az együttműködési számítástechnikai platformként: több felhasználó bejelentkezhet egy élő számítási munkamenet megosztásához ugyanazon notebook a kiszolgálón.

Ha a IPython forráskód [tárház][repository], töltse le, és a saját Azure Jupyter virtuális gépen kísérletezhet notebook példák egy teljes könyvtár található.  Egyszerűen csak töltse le a `.ipynb` -fájlok a helyről, és feltöltheti ezeket a notebook Azure virtuális gép irányítópultján alakzatot (vagy letöltheti a fájlokat közvetlenül a virtuális gép).

## <a name="conclusion"></a>Összegzés
A Jupyter Notebook hatékony felületet biztosít a hatványra emelésének Azure a Python ökoszisztémájának párbeszédes formában történő eléréséhez.  Használati esetek beleértve egyszerű feltárása és Python, adatelemzés és a képi megjelenítés, szimuláció és párhuzamos számítástechnikai számos magában foglalja. Az eredményül kapott Notebook dokumentumok tartalmazzák a hajtja végre, és más Jupyter felhasználókkal való megosztás számításokat teljes rekordja.  A Jupyter Notebook egy helyi alkalmazás használható, de ez akkor ajánlott, ha az Azure felhőben történő alkalmazáshoz

A core Jupyter számára is elérhető belül a Visual Studio használatával a [a Python Tools for Visual Studio] [ Python Tools for Visual Studio] (PTVS). PTVS egy ingyenes és nyílt forráskódú beépülő modul a Microsoft Visual Studio be olyan speciális Python fejlesztői környezetben, amely tartalmaz egy speciális szerkesztőt az IntelliSense hibakeresési információ-profilkészítési és párhuzamos számítástechnikai integráció.

## <a name="next-steps"></a>Következő lépések
További információ: [Python fejlesztői központban](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
