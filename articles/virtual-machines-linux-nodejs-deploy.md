---
title: "aaaDeploy egy Node.js-alkalmazás tooLinux virtuális gépek Azure-ban"
description: "Megtudhatja, hogyan toodeploy a Node.js alkalmazás tooLinux virtuális gépek Azure-ban."
services: 
documentationcenter: nodejs
author: stepro
manager: dmitryr
editor: 
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: /azure
ms.assetid: 857a812d-c73e-4af7-a985-2d0baf8b6f71
ms.service: multiple
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2016
ms.author: stephpr
ms.openlocfilehash: e876c8170a06f102f7f32ec7cb930b876647a707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-nodejs-application-toolinux-virtual-machines-in-azure"></a>A Node.js-alkalmazás tooLinux az Azure virtuális gépek telepítése
Ez az oktatóanyag bemutatja, hogyan tootake egy Node.js-alkalmazást, és telepítse azt az Azure-ban futó tooLinux virtuális gépek. hello az oktatóanyag utasításai követhetők bármely operációs rendszeren, amely alkalmas a Node.js futtatására.

Megtudhatja, hogyan:

* Elágazás és a klón egy egyszerű teendőlistát Megvalósító alkalmazás; tartalmazó GitHub-tárházat
* Hozza létre és konfigurálja a két Linux virtuális gépek Azure toorun hello alkalmazásban;
* Hello alkalmazás többször frissítések toohello webes előtér virtuális gép küldésével.

> [!NOTE]
> toocomplete ebben az oktatóanyagban GitHub-fiók és a Microsoft Azure-fiókra van szükség, és képes toouse Git fejlesztési gépről hello.
> 
> Ha a GitHub-fiók nem rendelkezik, iratkozzon fel a [Itt](https://github.com/join).
> 
> Ha nem rendelkezik egy [Microsoft Azure](https://azure.microsoft.com/) fiókja, regisztrálhat egy ingyenes próbaverzióra [Itt](https://azure.microsoft.com/pricing/free-trial/). Ez is végigvezeti hello regisztrációs folyamat az egy [Microsoft Account](http://account.microsoft.com) Ha még nem rendelkezik egy. Másik lehetőségként, ha a Visual Studio előfizetői, is [aktiválhatja MSDN előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
> 
> Ha még nem git a fejlesztői számítógépén, használata Macintosh- vagy Windows-számítógépen, telepítse a git [Itt](http://www.git-scm.com). Ha Linux használ, telepítse a git használatával, mint hello mechanizmus legmegfelelőbb értéket, `sudo apt-get install git`.
> 
> 

## <a name="forking-and-cloning-hello-todo-application"></a>Elágazó és a Klónozás hello Teendőlista alkalmazás
Az oktatóprogram során használt Teendőlista alkalmazás hello egy egyszerű webes előtér keresztül, amely a teendőlista nyomon követi a MongoDB-példány valósítja meg. Nyissa meg a bejelentkezést követően a tooGitHub [Itt](https://github.com/stepro/node-todo) toofind alkalmazás hello és oszthatja ketté, a jobb felső hello hello-kapcsolat használatával. Ez hozzon létre egy tárház nevű fiókjában *accountname*/node-todo.

Most a fejlesztői gépen klónozza a tárházat:

    git clone https://github.com/accountname/node-todo.git

Fogjuk használni a helyi klónozott tárház hello egy kicsit később változtatások toohello forráskódot.

## <a name="creating-and-configuring-hello-linux-virtual-machines"></a>Létrehozásához és konfigurálásához hello Linux virtuális gépek
Azure Linux virtuális gépek használatával nyers számítás kiváló visszajelzési rendelkezik. Ez a része hello oktatóanyag bemutatja, hogyan könnyen léptetési két Linux virtuális gépek és hello Teendőlista alkalmazás toothem, futó hello webes előtér egy és hello más hello a MongoDB-példány telepítése.

### <a name="creating-virtual-machines"></a>Virtuális gépek létrehozása
hello legegyszerűbb módja toocreate egy új virtuális gépet az Azure-ban toouse hello Azure portálon. Kattintson a [Itt](https://portal.azure.com) a toosign, és indítsa el a webböngésző hello Azure portálon. Azure portál be van töltve, hello befejezést hello a következő lépéseket:

* Kattintson a hello "+ új" hivatkozásra;
* Válassza ki a hello "Számítási" kategória, és válassza a "Ubuntu Server 14.04 LTS";
* Hello "erőforrás-kezelő" telepítési modell kiválasztása, és kattintson a "Létrehozás";
* Töltse ki az alábbi utasításokat követve hello alapjai:
  * Adjon meg egy újabb; könnyen azonosítható nevet
  * A jelen oktatóanyag esetében válassza a jelszó-hitelesítés;
  * Hozzon létre egy új erőforráscsoportot azonosítható névvel.
* Hello virtuális gép méretét, a "A1 szabványos" akkor ésszerű választani, ehhez az oktatóanyaghoz.
* A további beállításokhoz győződjön meg arról hello lemez típusa "Standard", és fogadja el az összes többi alapértelmezett hello.
* Indítsa hello létrehozása hello összefoglaló oldalon.

Hajtsa végre a fenti hello feldolgozása kétszer toocreate két Linux virtuális gépek, egy a hello webes előtér és egy hello MongoDB-példány. Hello virtuális gépek létrehozását körülbelül 5 – 10 percig tart.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>A DNS-bejegyzés hozzárendelése a virtuális gépek
Az Azure-ban létrehozott virtuális gépek csak egy nyilvános IP-címet, például 1.2.3.4 keresztül érhető el alapértelmezés szerint is. Most Meggyőződünk hello gépek könnyebben azonosítható DNS-bejegyzések hozzárendelésével.

Miután hello portal azt jelzi, hogy létrejöttek-e a hello virtuális gépek, kattintson a hello bal oldali navigációs sávja hello "Virtual machines" hivatkozásra, és keresse meg a gépek. Az egyes gépek:

* Keresse meg a hello alapvető erőforrások lapon, majd kattintson a hello nyilvános IP-cím;
* A hello nyilvános IP-címének konfigurációja rendelje hozzá egy DNS-névcímke, és mentse.

hello portal biztosítja az adott hello néven érhető el. Hello konfigurációjának mentése után a virtuális gépek lesz hasonló állomásnevek túl`machinename.region.cloudapp.azure.com`.

### <a name="connecting-toohello-virtual-machines"></a>Csatlakozás a virtuális gépek toohello
Ha a virtuális gépek kiépített, előre konfigurált tooallow távoli kapcsolatok voltak SSH-n keresztül. Ez a hello mechanizmus tooconfigure hello virtuális gépek használjuk. Ha Windows használ a fejlesztési, ha még nem rendelkezik egy szüksége lesz tooget egy SSH-ügyfél. Egy közös választott, amely letölthető a PuTTY [Itt](http://www.chiark.greenend.org.uk/~sgtatham/putty/). A Macintosh- és Linux operációs rendszer származnak SSH előre telepített verziója.

### <a name="configuring-hello-web-frontend-virtual-machine"></a>Hello webes előtér virtuális gép konfigurálása
SSH toohello webes előtér gép létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz. Egy parancs parancssori futtatásával követ üdvözlő üzenet megjelenik.

Első lépésként most Meggyőződünk arról, hogy a git szoftver, csomópont is telepítve vannak:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

Hello alkalmazás webes előtér néhány natív Node.js modulok támaszkodik, mivel tooinstall hello alapvető összeállítási eszközök gyűjteménye is kell:

    sudo apt-get install -y build-essential

Végezetül most telepíteni a Node.js-alkalmazás neve *végtelen*, toorun Node.js kiszolgálóalkalmazások segítségével:

    sudo npm install -g forever

Ezek azok a virtuális gép toobe képes toorun hello alkalmazás webes előtér szükséges összes hello függőségek, így folytassuk, hogy futnak. toodo, először létrehozunk egy operációs rendszer klónja hello GitHub-tárházban korábban meg ágazik el, így könnyen közzéteheti a frissítéseket toohello virtuális gép (oktatóanyag a következőket ismerteti a frissítési forgatókönyv később), és majd klónozni hello operációs rendszer Klónozás tooprovide hello egy verziója az tárház ténylegesen hajt végre.

Hello kezdőkönyvtára (~) kiindulva, futtassa a következő parancsok hello (cseréje *accountname* a GitHub felhasználói fiók nevével):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Most adja meg hello csomópont-todo könyvtárat, és futtassa az alábbi parancsokat:

    npm install
    forever start server.js

hello alkalmazás webes előtér most már fut, azonban, hogy egy további lépést egy webböngészőből hello alkalmazás eléréséhez. hello létrehozott virtuális gép által védett egy Azure-erőforrás neve egy *hálózati biztonsági csoport*, amely hozták létre, amikor a kiépített hello virtuális gép számára. Ezt az erőforrást jelenleg csak külső kérelmek tooport irányított 22 toobe toohello virtuális gépet, mely lehetővé teszi az SSH-kommunikációhoz hello gép, de semmi mást teszi lehetővé. Ezért a rendelés tooview hello Teendőlista alkalmazás, amely konfigurált toorun a 8080-as porton, ezt a portot is igények toobe megnyitotta a.

Térjen vissza a toohello Azure portálon, és végezze el az alábbi lépésekkel hello:

* Kattintson a "Erőforráscsoportok" hello bal oldali navigációs sávja;
* A virtuális gépet; tartalmazó hello erőforráscsoport kiválasztása
* Hello az erőforrások listájában válassza ki a hello hálózati biztonsági csoportot (hello egy egy pajzs ikon);
* Hello tulajdonságait válassza a "bejövő biztonsági szabályok";
* A hello eszköztárában kattintson a "Hozzáadás";
* Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-teendőlista";
* Hello protokoll túl beállítása "TCP";
* Állítsa be a hello Célporttartomány túl "8080-as";
* Kattintson az OK gombra, majd várja meg a létrehozott hello biztonsági szabály toobe.

A biztonsági szabály létrehozása után hello Teendőlista alkalmazás nyilvánosan látható hello internet, és tallózással is kikeresheti tooit, például, mint az egy URL-cím segítségével:

    http://machinename.region.cloudapp.azure.com:8080

Megfigyelheti, hogy annak ellenére, hogy még nincs konfigurálva hello MongoDB virtuális gép, hello Teendőlista alkalmazás teljesen működőképes toobe megjelenik. Ez azért, mert hello forrás tárház szoftveresen kötött toouse előre telepített MongoDB-példány. Miután hello MongoDB virtuális gép rendelkezik konfigurált, rendszer lépjen vissza és módosítása hello forrás kód tooutilize a személyes MongoDB-példány helyett.

### <a name="configuring-hello-mongodb-virtual-machine"></a>Hello MongoDB virtuális gép konfigurálása
SSH toohello második gép létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz. A MongoDB telepítése után megtekintheti a hello üdvözlő üzenet és a parancssort, (ezek az utasítások vették [Itt](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Alapértelmezés szerint MongoDB van beállítva, ezért csak elérhető helyileg. Ebben az oktatóanyagban azt konfigurálja MongoDB, a rendszer az elérhető legyen a hello alkalmazás virtuális gépről. A sudo a környezetben, nyissa meg hello /etc/mongod.conf fájlt, és keresse meg a hello `# network interfaces` szakasz. Változás hello `net.bindIp` konfigurációs érték túl`0.0.0.0`.

> [!NOTE]
> Ez a konfiguráció akkor csak ez az oktatóanyag hello alkalmazásában. Az **nem** egy ajánlott biztonsági eljárás, és nem használható üzemi környezetben.
> 
> 

Most már biztosítja hello szolgáltatás el van indítva MongoDB:

    sudo service mongod restart

MongoDB porton 27017 alapértelmezés szerint működik. Így hello a megszokott módon, hogy változtatnunk tooopen hello webes előtér virtuális gépen, a 8080-as port igazolnia kell, hogy tooopen port 27017 hello MongoDB virtuális gépen.

Térjen vissza a toohello Azure portálon, és végezze el az alábbi lépésekkel hello:

* Kattintson a "Erőforráscsoportok" hello bal oldali navigációs sávja;
* Hello MongoDB virtuális gépet; tartalmazó hello erőforráscsoport kiválasztása
* Hello az erőforrások listájában válassza ki az hello hálózati biztonsági csoport (hello egy egy pajzs ikon) rendelkező hello azonos neve, mint toohello MongoDB virtuális gép;
* Hello tulajdonságait válassza a "bejövő biztonsági szabályok";
* A hello eszköztárában kattintson a "Hozzáadás";
* Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-mongo";
* Hello protokoll túl beállítása "TCP";
* Állítsa be a hello Célporttartomány túl "27017";
* Kattintson az OK gombra, majd várja meg a létrehozott hello biztonsági szabály toobe.

## <a name="iterating-on-hello-todo-application"></a>A Teendőlista alkalmazás hello léptetés
Az eddigi jelenleg két Linux virtuális gépek kiépítése: futtató hello alkalmazás webes előtér- és egy, a MongoDB-példány fut. De a probléma – hello webes előtér nem ténylegesen használó kiépítve hello MongoDB-példány még. Most javíthatja ki, hogy hello webes előtér kód toouse frissítése kódolt példány helyett egy környezeti változó.

### <a name="changing-hello-todo-application"></a>Hello Teendőlista alkalmazás módosítása
A fejlesztői számítógépén, ha először klónozott hello csomópont-todo tárház, nyissa meg a hello `node-todo/config/database.js` a kedvenc szerkesztő fájlt, és módosítsa hello URL-cím hello kódolt értékből például `mongodb://...` túl`process.env.MONGODB`.

A változtatások véglegesítése a határidő, és leküldéses toohello GitHub-főkiszolgáló:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Sajnos ez nem közzététele hello módosítása toohello webes előtér virtuális gépet. Most Meggyőződünk módosítások toothat néhány további virtuális gépek tooenable egy egyszerű, de hatékony mechanizmus a frissítések közzétételéhez, így gyorsan figyelheti hello hatását hello hello éles környezetben.

### <a name="configuring-hello-web-frontend-virtual-machine"></a>Hello webes előtér virtuális gép konfigurálása
Előhívja a korábban létrehozott egy operációs rendszer Klónozás hello csomópont-todo tárház hello webes előtér virtuális gépen. Az elemről kiderül, hogy ez a művelet létrehozott egy új Git távoli toowhich módosítások továbbíthatja. Azonban egyszerűen küldését toothis távoli nem elég adnak hello gyors iterációs modell, amely a fejlesztők olyan eszközökre vonatkozóan feldolgozása során a rendszer a kódot.

Mi azt kellene például toobe képes toodo van győződjön meg arról, hogy egy leküldéses toohello távoli tárházba hello a virtuális gép esetén hello futó Teendőlista alkalmazás automatikusan frissül. Szerencsére Ez az egyszerű tooachieve git.

Git hurkok, melynek neve adott alkalommal tooreact tooactions hello tárház végzett, a számát mutatja. Ezek vannak megadva a rendszerhéj-parancsfájlok használatával hello tárházban `hooks` mappát. hello hook leginkább megfelelő hello automatikus frissítési forgatókönyv hello `post-update` esemény.

SSH munkamenet toohello webes előtér virtuális gépen, módosítsa a toohello `~/node-todo.git/hooks` könyvtár, és adja hozzá a következő nevű tartalom tooa fájl hello `post-update` (cseréje `machinename` és `region` a MongoDB-virtuális gép adatait):

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

Győződjön meg arról, ez a fájl végrehajtható hello a következő parancs futtatásával:

    chmod 755 post-update

Ez a parancsfájl ellenőrzi, hogy hello aktuális kiszolgálóalkalmazás le van állítva, hello klónozott tárház hello kód frissített toohello legújabb, frissített függőségek teljesülnek, és hello kiszolgáló újraindul. Emellett biztosítja hello környezet előkészítése az első alkalmazás frissítés tooget hello MongoDB-példány fogad egy környezeti változó lett konfigurálva.

### <a name="configuring-your-development-machine"></a>A fejlesztői számítógépén konfigurálása
Most folytassuk a fejlesztési számítógépén toohello webes előtér virtuális gép csatlakoztatva. Ez a lehető legegyszerűbb hello operációs rendszer tárház hozzáadása távoli tárházként hello virtuális gépen. Futtassa a következő parancs toodo hello ez (cseréje *felhasználói* a webes előtér virtuális gép bejelentkezési névvel és *machinename* és *régió* szükség szerint):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

Ez az összes szükséges tooenable kérdez le, vagy a közzétételi érvényben, toohello webes előtér virtuális gép vált.

### <a name="publishing-updates"></a>Közzétételi frissítések
Most közzététele hello egy módosítás tett eddig, hogy hello alkalmazás fogja használni a saját MongoDB-példány:

    git push azure master

Kimeneti hasonló toothis kell megjelennie:

    Counting objects: 4, done.
    Delta compression using up too4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    toousername@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Ez a parancs után frissítse hello alkalmazást egy webböngészőben. Képes toosee itt bemutatott hello teendőlista üres, és már nem a megosztott kapcsolt toohello MongoDB-példány telepítve kell lennie.

az oktatóanyagban toocomplete hello most Meggyőződünk egy másik, több látható módosítása. A fejlesztői számítógépén nyissa meg a kedvenc szerkesztőjével hello node-todo/public/index.html fájl. Keresse meg a hello jumbotron fejlécet, és módosítsa a "Vagyok a teendőlista aholic" túl "vagyok a teendőlista-aholic Azure!" hello címet.

Most tegyük véglegesítése:

    git commit -am "Azurify hello title"

Ez alkalommal, most közzététele előtt azt tooback toohello GitHub-tárház hello módosítása tooAzure:

    git push azure master

Ez a parancs után frissítési hello weblapot, és akkor jelenik meg hello módosításokat. Mivel azok szépen, hello módosítás vissza toohello származási távoli leküldéses: 

    git push origin master

## <a name="next-steps"></a>Következő lépések
Ez a cikk bemutatta, hogyan tootake egy Node.js-alkalmazást, és telepítse azt az Azure-ban futó tooLinux virtuális gépek. További információ a Linux virtuális gépek Azure-ban toolearn lásd [bemutatása tooLinux Azure](/documentation/articles/virtual-machines-linux-introduction/).

További információ toodevelop Node.js-alkalmazások Azure, lásd: hello [Node.js fejlesztői központ](/develop/nodejs/).

