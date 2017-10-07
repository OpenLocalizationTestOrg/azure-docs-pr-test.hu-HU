---
title: egy Jupyter/IPython Notebook aaaCreate |} Microsoft Docs
description: "Ismerje meg, hogyan toodeploy hello Jupyter/IPython Notebook Linux virtuális gépeken létre hello resource manager üzembe helyezési modellben az Azure-ban."
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
ms.openlocfilehash: d7f2e45a8ba95163ebfb0f10babc91a2b3fd9390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a>Egy Azure virtuális gép létrehozása, Jupyter telepítése és futtatása a Jupyter Notebook az Azure-on
Hello [Jupyter projekt](http://jupyter.org), korábbi nevén hello [IPython projekt](http://ipython.org), olyan eszközöket biztosít tudományos számítástechnikai kód végrehajtását hello létrehozását a felhasználó hatékony interaktív rendszerhéjának használatával egy élő számítási dokumentumot. A notebook fájlok tetszőleges szöveg, matematikai képletek, bemeneti kódot, eredményeket, grafikus, videók és bármely más típusú, hogy a modern webböngésző megjelenítésére alkalmas adathordozó is tartalmazhat. E teljesen új tooPython van, és szeretné, hogy toolearn visszatöltött, interaktív környezetben vagy néhány súlyos párhuzamos és technikai computing, hello Jupyter Notebook kiváló választás tegye azt.

![Képernyőkép](./media/jupyter-notebook/ipy-notebook-spectral.png) használatával SciPy és Matplotlib csomagok tooanalyze hello szerkezete egy Hangrögzítés.

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter két módon: Azure notebookok vagy egyéni központi telepítés
Túl is használhatja a szolgáltatást biztosít az Azure[Jupyter használatának gyors megkezdéséhez ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Hello Azure Notebook szolgáltatás használatával egyszerűen ettől kezdve a hozzáférési tooJupyter interneten elérhető felület tartalmazó összes hello power a Python és a sok szalagtárak méretezhető számítási erőforrások.  Hello telepítési hello szolgáltatás kezeli, mivel a felhasználók elérhetik ezeket az erőforrásokat hello felügyelete és konfigurálása hello felhasználó nélkül.

Ha hello notebook szolgáltatás nem működik a forgatókönyvnek ezentúl tooread ebben a cikkben, amely fog bemutatja, hogyan toodeploy hello Jupyter Notebook a Microsoft Azure Linux virtuális gépek (VM) használatával.

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Hozza létre és konfigurálja a virtuális gépek az Azure-on
első lépés hello toocreate van Azure-on futó virtuális gép (VM).
Ez a virtuális gép egy teljes operációs rendszer hello felhőben, és hello Jupyter Notebook futtatásához használandó. Azure képes a Linux és a Windows virtuális gépek futtatását, és bemutatjuk, hogy a virtuális gépek mindkét fajtája Jupyter hello beállítása.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Hozzon létre egy Linux virtuális Gépet, és a Jupyter port megnyitása
Hajtsa végre az adott hello utasítások [Itt] [ portal-vm-linux] toocreate hello a virtuális gép *Ubuntu* terjesztési. Ez az oktatóanyag Ubuntu Server 14.04 LTS használja. Feltételezzük, hogy hello felhasználónév *azureuser*.

Miután telepíti a virtuális gép hello igazolnia kell a tooopen hello hálózati biztonsági csoport biztonsági szabály mentése.  Hello Azure-portálon, a go túl**hálózati biztonsági csoportok** és hello biztonsági csoport megfelelő tooyour VM nyitott hello lapján. Egy bejövő biztonsági szabály tooadd van szüksége a következő beállítások hello: **TCP** hello protokoll  **\***  a hello forrásport (nyilvános) és **9999** a hello (személyes) célport.

![Képernyőfelvétel](./media/jupyter-notebook/azure-add-endpoint.png)

Az a hálózati biztonsági csoporthoz, kattintson a **hálózati illesztők** és megjegyzés hello **nyilvános IP-cím** , szükséges tooconnect tooyour VM hello következő lépésben lesz.

## <a name="install-required-software-on-hello-vm"></a>Szükséges szoftverek telepítését hello méretű VM
toorun hello Jupyter Notebook a virtuális gépen, azt először telepítenie kell Jupyter és annak függőségeit. Csatlakozás tooyour linux virtuális gép ssh, és úgy döntött, hogy mikor létre hello felhasználónév/jelszó pár hello virtuális gép. Ez az oktatóanyag a rendszer a PuTTY használata és csatlakozás Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Ubuntu Jupyter telepítése
Anaconda, olyan népszerű adatok tudományos python elosztási, a megadott hello hivatkozások valamelyikével telepítése [alakíthatnak ki olyan Analytics](https://www.continuum.io/downloads).  Frissítésétől a jelen dokumentum írása hello, hello az alábbi hivatkozások olyan hello legtöbb toodate verziók fel.

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

    # Update Jupyter toohello latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Képernyőfelvétel](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter konfigurálása és az SSL használata
Telepítése után kell tootake egy kicsit toosetup hello konfigurációs fájlokat a Jupyter. A Jupyter konfigurálása során fellépő problémák tapasztal lehet hasznos toolook: hello [Jupyter Notebook-kiszolgálót futtató dokumentációja](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Tovább a Microsoft `cd` toohello profil directory toocreate az SSL-tanúsítványt, és szerkesztheti a hello-profil konfigurációs fájlt.

Linux hello a következő parancsot használja.

    cd ~/.jupyter

A következő parancs toocreate hello SSL-tanúsítvány (Linux és Windows rendszerekhez) hello használata.

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Vegye figyelembe, hogy azóta létrehozzuk önaláírt SSL-tanúsítvány toohello notebook csatlakozáskor a böngésző biztonsági figyelmeztetést ad meg.  Hosszú távú éles környezetben való használathoz érdemes toouse a szervezetéhez tartozó megfelelően aláírt tanúsítvány.  Tanúsítványkezelés nem ebben a bemutatóban a hello terjed, mivel most azt fogja odatapadjon tooa önaláírt tanúsítványt.

Ezenkívül toousing egy tanúsítványt, meg kell adni egy jelszót tooprotect a notebook jogosulatlan használatát.  Biztonsági okokból Jupyter titkosított jelszavakat használ a konfigurációs fájlban, ezért tooencrypt kell a jelszót először.  IPython biztosít egy segédprogram toodo a parancssorba futtassa a következő parancs hello.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Kérni fogja a jelszó és a megerősítés, és azt nyomtassa ki a hello jelszót. Megjegyzés: Ez a következő lépés hello.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided hello rest for security)

A következő fogjuk módosítani hello-profil konfigurációs fájl, amely a `jupyter_notebook_config.py` fájl hello használata során.  Vegye figyelembe, hogy a fájl nem létezik – most létrehozták hello esetben.  Ez a fájl rendelkezik mezők számát, és alapértelmezés szerint az összes jelenleg megjegyzésként szerepelnek.  Ez a fájl megnyitható tetszőlegesen a szövegszerkesztőben, és biztosítania kell legalább rendelkezik hello következő tartalom. **Lehet, hogy tooreplace hello c.NotebookApp.password hello config az előző lépésben hello hello sha1**.

    c = get_config()

    # You must give hello path toohello certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-hello-jupyter-notebook"></a>Jupyter Notebook hello futtatása
Ekkor készen áll a toostart hello Jupyter Notebook folyamatban. toodo, keresse meg a kívánt toostore notebookok, és indítsa el a következő parancs hello hello Jupyter Notebook server toohello directory.

    /anaconda3/bin/jupyter-notebook

Most meg kell tudni tooaccess a Jupyter Notebook hello címen `https://[PUBLIC-IP-ADDRESS]:9999`.

A notebook első megnyitásakor hello bejelentkezési oldal kéri a jelszót. És jelentkezik be, miután látni fog hello "Jupyter Notebook irányítópult", amely az összes jegyzetfüzet művelet vezérlőkarakterek központnak.  Ezen a lapon létrehozhat új, jegyzetfüzeteket és nyissa meg a már meglévőket.

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-hello-jupyter-notebook"></a>Hello Jupyter Notebook használatával
Ha hello **új** gomb, látni fogja a következő lap megnyitása hello.

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-untitled-notebook.png)

hello terület jelölésű egy `In []:` parancssorból hello beviteli terület, és itt adhatja meg bármelyik érvényes Python-kód, és akkor lesz hajtható végre, ha kattint `Shift-Enter` vagy kattintson a hello "Play" ikonra (hello jobbra mutató háromszög hello eszköztár).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>A hatékony paradigma: élő multimédiás számítási dokumentumok
hello notebook magának kell érzi, hogy nagyon természetes tooanyone, akik használják a Python és a szövegszerkesztőt, mert bizonyos értelemben vegyesen mindkét: végrehajthat kódblokkokat, Python, de is megtarthatja megjegyzések és egyéb szöveg túl egy cella "Kódból" hello stílus módosításával "Ma rkdown"hello legördülő menü segítségével az eszköztáron.

Jupyter sokkal több mint egy szövegszerkesztőt, lehetővé teszi a számítási és a gazdag media (szöveg, képek, videó és gyakorlatilag bármi modern webböngésző megjelenítheti) keverése. Kombinálhatja, szöveg, kód, videókat és egyéb!

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-editing-experience.png)

És a Python tartozó sok kiváló-könyvtárakban műszaki és tudományos számítógép-használatról, a következő képernyőfelvételen látható hello hello hatványa egyszerű számítás hajtható végre hello ugyanaz, mint egy összetett hálózati elemző egy környezetben megkönnyítése érdekében.

A modern webes hello hello hatványa keverése élő számítási ez paradigma számos lehetőséget kínál, és kiválóan alkalmas hello felhő; hello Notebook használhatók:

* A felderítő számítási firkatömb toorecord, működnek. a probléma.
* tooshare eredmények munkatársaival, "élő" számítási formában vagy vezetékhelyettesítési formátumokban (HTML, PDF).
* toodistribute és a jelen élő oktatóanyag, számítási, például az, a diákok azonnal kísérletezhet, hello valós kódot, és módosítsa azt, és hajtsa végre újból az interaktív módon.
* "végrehajtható által írt cikkeket", hogy kutatási hello eredményeit, hogy azonnal reprodukálható, tooprovide érvényesítve, valamint a kiterjesztett mások számára.
* Az együttműködési számítástechnikai platformként: több felhasználó is bejelentkezhetnek toothe azonos notebook server tooshare élő számítási munkamenet.

Ha toohello IPython forráskód [tárház][repository], töltse le, és a saját Azure Jupyter virtuális gépen kísérletezhet notebook példák egy teljes könyvtár talál.  Egyszerűen csak töltse le a hello `.ipynb` hello fájlok hely és feltöltésükhöz alakzatot hello irányítópult a jegyzetfüzet Azure virtuális gép (vagy letöltheti a fájlokat közvetlenül a virtuális gép hello).

## <a name="conclusion"></a>Összegzés
Jupyter Notebook hello hatékony felületet biztosít a hello hatványra emelésének Azure Python ökoszisztémájának hello párbeszédes formában történő eléréséhez.  Használati esetek beleértve egyszerű feltárása és Python, adatelemzés és a képi megjelenítés, szimuláció és párhuzamos számítástechnikai számos magában foglalja. hello eredményül kapott Notebook dokumentumok hajtja végre, és más Jupyter felhasználókkal való megosztás hello számítások teljes rekordja tartalmaznak.  hello Jupyter Notebook egy helyi alkalmazás használható, de ez akkor ajánlott, ha az Azure felhőben történő alkalmazáshoz

hello core Jupyter számára is elérhető belül a Visual Studio használatával a [a Python Tools for Visual Studio] [ Python Tools for Visual Studio] (PTVS). PTVS egy ingyenes és nyílt forráskódú beépülő modul a Microsoft Visual Studio be olyan speciális Python fejlesztői környezetben, amely tartalmaz egy speciális szerkesztőt az IntelliSense hibakeresési információ-profilkészítési és párhuzamos számítástechnikai integráció.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [Python fejlesztői központ](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
