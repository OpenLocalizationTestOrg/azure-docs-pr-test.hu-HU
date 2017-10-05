---
title: "Egy Linux virtuális gépek Azure-ban Node.js-alkalmazás központi telepítése"
description: "Linux virtuális gépek Azure-ban egy Node.js-alkalmazás telepítésének megismerése."
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
ms.openlocfilehash: d3d9fecfafb9ba422420230716b9c985481d1e24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a>Egy Linux virtuális gépek Azure-ban Node.js-alkalmazás központi telepítése
Ez az oktatóanyag bemutatja, hogyan igénybe vehet egy Node.js-alkalmazást, és telepítse azt az Azure-ban futó Linux virtuális gépek számára. Az oktatóanyag utasításai követhetők bármely olyan operációs rendszeren, amely alkalmas a Node.js futtatására.

Megtudhatja, hogyan:

* Elágazás és a klón egy egyszerű teendőlistát Megvalósító alkalmazás; tartalmazó GitHub-tárházat
* Hozzon létre, és két Linux virtuális gép konfigurálása az Azure-futtatni az alkalmazást;
* Az alkalmazás többször a webes előtér virtuális gép frissítések küldésével.

> [!NOTE]
> Az oktatóanyag teljesítéséhez szüksége van GitHub-fiók és a Microsoft Azure-fiók és a Git fejlesztési gépről használhatja.
> 
> Ha a GitHub-fiók nem rendelkezik, iratkozzon fel a [Itt](https://github.com/join).
> 
> Ha nem rendelkezik egy [Microsoft Azure](https://azure.microsoft.com/) fiókja, regisztrálhat egy ingyenes próbaverzióra [Itt](https://azure.microsoft.com/pricing/free-trial/). Ez is végigvezeti a regisztrációs folyamat az egy [Microsoft Account](http://account.microsoft.com) Ha még nem rendelkezik egy. Másik lehetőségként, ha a Visual Studio előfizetői, is [aktiválhatja MSDN előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
> 
> Ha még nem git a fejlesztői számítógépén, használata Macintosh- vagy Windows-számítógépen, telepítse a git [Itt](http://www.git-scm.com). Ha Linux használ, telepítse a git, legmegfelelőbb mechanizmus használatával, mint `sudo apt-get install git`.
> 
> 

## <a name="forking-and-cloning-the-todo-application"></a>Elágazó és a Klónozás a Teendőlista alkalmazás
Az oktatóprogram során használt Teendőlista alkalmazás megvalósítja a egy egyszerű webes előtér, amely a teendőlista nyomon követi a MongoDB-példány felett. Nyissa meg a Githubon történő bejelentkezés után [Itt](https://github.com/stepro/node-todo) forkolhatja, a kapcsolat használatával jobb felső és az alkalmazás megkeresése. Ez hozzon létre egy tárház nevű fiókjában *accountname*/node-todo.

Most a fejlesztői gépen klónozza a tárházat:

    git clone https://github.com/accountname/node-todo.git

Fogjuk használni a helyi klónozott tárház egy kicsit később a Forráskód módosítása során.

## <a name="creating-and-configuring-the-linux-virtual-machines"></a>Létrehozása és konfigurálása a Linux virtuális gépek
Azure Linux virtuális gépek használatával nyers számítás kiváló visszajelzési rendelkezik. Ez az oktatóanyag azt mutatja be hogyan könnyen léptetési két Linux virtuális gépek és a Teendőlista alkalmazás, központi telepítése egyet, majd a MongoDB-példány fut a webes előtér részét.

### <a name="creating-virtual-machines"></a>Virtuális gépek létrehozása
A legegyszerűbben úgy, hogy egy új virtuális gép létrehozása az Azure-ban az Azure portál használatával. Kattintson a [Itt](https://portal.azure.com) jelentkezzen be, majd indítsa el az Azure-portálon a böngészőben. Miután az Azure portál be van töltve, kövesse az alábbi lépéseket:

* A "+ új" hivatkozásra;
* Válassza ki a "Számítási" kategória, és válassza a "Ubuntu Server 14.04 LTS";
* Válassza ki a "erőforrás-kezelő" üzembe helyezési modellel, és kattintson a "Létrehozás";
* Töltse ki az alábbi utasításokat követve alapvető:
  * Adjon meg egy újabb; könnyen azonosítható nevet
  * A jelen oktatóanyag esetében válassza a jelszó-hitelesítés;
  * Hozzon létre egy új erőforráscsoportot azonosítható névvel.
* A virtuálisgép-méret "A1 szabványos" esetén ésszerű választás ehhez az oktatóanyaghoz.
* A további beállításokhoz ellenőrizze a lemez típusát "Standard", és fogadja el a többi alapértelmezett értéket.
* Az összefoglalás lapon létrehozása indítsa.

Hajtsa végre a fenti kétszer létrehozásának folyamata két Linux virtuális gépek, egyet a webes előtér és egyet a MongoDB-példány. A virtuális gépek létrehozását körülbelül 5 – 10 percig tart.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>A DNS-bejegyzés hozzárendelése a virtuális gépek
Az Azure-ban létrehozott virtuális gépek csak egy nyilvános IP-címet, például 1.2.3.4 keresztül érhető el alapértelmezés szerint is. Ellenőrizze a gép könnyebben azonosítható DNS-bejegyzések hozzárendelésével.

Miután a portál azt jelzi, hogy létrejöttek-e a virtuális gépek, a bal oldali navigációs sávja a "Virtual machines" hivatkozásra, és keresse meg a gépek. Az egyes gépek:

* Keresse meg az alapvető erőforrások lapon, majd kattintson a nyilvános IP-címét;
* A nyilvános IP-címének konfigurációja a DNS-névcímke meg és mentéséhez.

A portál biztosítja, hogy rendelkezésre áll-e a megadott név. A konfiguráció mentése, után a virtuális gépek állomás neve hasonlít kell `machinename.region.cloudapp.azure.com`.

### <a name="connecting-to-the-virtual-machines"></a>A virtuális gépekhez való csatlakozás
Ha a virtuális gépek kiépített, amilyenek korábban voltak előre konfigurált SSH-n keresztül a távoli kapcsolatok lehetővé tételéhez. Ez az a mechanizmus segítségével a virtuális gépet állíthat be. Használatakor a Windows a fejlesztési, szüksége lesz egy SSH-ügyfél segítségével, ha még nem rendelkezik egy. Egy közös választott, amely letölthető a PuTTY [Itt](http://www.chiark.greenend.org.uk/~sgtatham/putty/). A Macintosh- és Linux operációs rendszer származnak SSH előre telepített verziója.

### <a name="configuring-the-web-frontend-virtual-machine"></a>A webes előtér virtuális gép konfigurálása
A webes előtér géphez létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz SSH. Egy parancs parancssori futtatásával követ üdvözlő üzenet megjelenik.

Első lépésként most Meggyőződünk arról, hogy a git szoftver, csomópont is telepítve vannak:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

Az alkalmazás webes előtér néhány natív Node.js modulok támaszkodik, mivel azt is telepítenie kell a build tools alapvető készlete:

    sudo apt-get install -y build-essential

Végezetül most telepíteni a Node.js-alkalmazás neve *végtelen*, ami segít Node.js server-alkalmazások futtatására:

    sudo npm install -g forever

Ezek a minden a függőségeket, hogy az alkalmazás webes előtér futtatni, ezért folytassuk, hogy fut a virtuális gépen szükséges. Ehhez először létrehozunk egy operációs rendszer klónja korábban meg ágazik el, így könnyen frissítések közzétételéhez a virtuális gép (oktatóanyag a következőket ismerteti a frissítési forgatókönyv később), és adja meg a tárház ténylegesen végrehajtható verzióját az operációs rendszer klónt majd klónozza a GitHub-tárházban.

A (~) kezdőkönyvtára, kezdve a következő parancsokat (cseréje *accountname* a GitHub felhasználói fiók nevével):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Most adja meg a csomópont-todo könyvtárat, és futtassa az alábbi parancsokat:

    npm install
    forever start server.js

Az alkalmazás webes előtér innentől kezdve fut, azonban nincs egy további lépést egy webböngészőből próbál hozzáférni az alkalmazáshoz. A létrehozott virtuális gépet egy Azure-erőforrás neve védi egy *hálózati biztonsági csoport*, amely jött létre az Ön, amikor a virtuális gép létesített. Jelenleg ez az erőforrás csak lehetővé teszi a külső kérelmek 22-es portot irányíthatja át a virtuális géphez, amely lehetővé teszi, hogy a gép, de semmi mást SSH-kommunikációhoz. Így megtekintheti a Teendőlista alkalmazás, a 8080-as porton futtatásra van beállítva, amely ezt a portot is kell megnyitását.

Térjen vissza az Azure portálra, és kövesse az alábbi lépéseket:

* Kattintson a "Erőforráscsoportok" a bal oldali navigációs sávja;
* Válassza ki az erőforrás-csoportot, amely tartalmazza a virtuális gép;
* Az eredményül kapott erőforrások, jelölje ki a hálózati biztonsági csoport (egy pajzs ikon egy);
* Válassza a Tulajdonságok "bejövő biztonsági szabályok";
* Az eszköztáron kattintson a "Hozzáadás";
* Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-teendőlista";
* A "TCP"; a protokoll beállítása
* Állítsa be a Célporttartomány "8080";
* Kattintson az OK gombra, és várja meg a létrehozandó szabály.

A biztonsági szabály létrehozása után a Teendőlista alkalmazás nyilvánosan látható az interneten, és tallózással is kikeresheti azt, például, mint az egy URL-cím segítségével:

    http://machinename.region.cloudapp.azure.com:8080

Megfigyelheti, hogy annak ellenére, hogy még nincs konfigurálva a MongoDB virtuális gépet, a Teendőlista alkalmazás úgy tűnik, hogy teljesen működőképes. Ez azért, mert a tárházban szoftveresen kötött előre telepített MongoDB-példány használata. Miután a MongoDB-virtuális gép rendelkezik konfigurált, rendszer lépjen vissza és a titkos MongoDB-példány helyett magukat, hogy a Forráskód módosítása.

### <a name="configuring-the-mongodb-virtual-machine"></a>A MongoDB-virtuális gép konfigurálása
A második géphez létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz SSH. A MongoDB telepítése után megtekintheti az üdvözlő üzenet és a parancssort, (ezek az utasítások vették [Itt](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Alapértelmezés szerint MongoDB van beállítva, ezért csak elérhető helyileg. Ebben az oktatóanyagban azt konfigurálja MongoDB, a rendszer az elérhető legyen a az alkalmazás virtuális gépről. A sudo a környezetben, nyissa meg a /etc/mongod.conf fájlt, és keresse meg a `# network interfaces` szakasz. Módosítsa a `net.bindIp` konfigurációs érték `0.0.0.0`.

> [!NOTE]
> Ez a konfiguráció akkor csak ez az oktatóanyag céljából. Az **nem** egy ajánlott biztonsági eljárás, és nem használható üzemi környezetben.
> 
> 

Most ellenőrizze, hogy a szolgáltatás el van indítva MongoDB:

    sudo service mongod restart

MongoDB porton 27017 alapértelmezés szerint működik. Igen azonos módon, hogy változtatnunk kell, nyissa meg a webes előtér virtuális gépen a 8080-as porton, igazolnia kell nyissa meg a portot 27017 a MongoDB virtuális gépen.

Térjen vissza az Azure portálra, és kövesse az alábbi lépéseket:

* Kattintson a "Erőforráscsoportok" a bal oldali navigációs sávja;
* Válassza ki az erőforráscsoportot, amely tartalmazza a MongoDB-virtuális gép;
* Az eredményül kapott erőforrások, jelölje ki a hálózati biztonsági csoport (egy a pajzs ikon) ugyanazzal a névvel, amelyek a MongoDB-virtuális gép; adott
* Válassza a Tulajdonságok "bejövő biztonsági szabályok";
* Az eszköztáron kattintson a "Hozzáadás";
* Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-mongo";
* A "TCP"; a protokoll beállítása
* Állítsa be a Célporttartomány "27017";
* Kattintson az OK gombra, és várja meg a létrehozandó szabály.

## <a name="iterating-on-the-todo-application"></a>A Teendőlista alkalmazás a léptetés
Az eddigi jelenleg két Linux virtuális gépek kiépítése: egyet, hogy fut az alkalmazás webes előtér- és egy, a MongoDB-példány fut. De a probléma merül fel, mert a webes előtér nem használja a kiosztott MongoDB-példány még. Most javíthatja ki, hogy a webes előtér-kód kódolt példány helyett egy környezeti változó használatával frissítése.

### <a name="changing-the-todo-application"></a>A Teendőlista alkalmazás módosítása
A fejlesztői számítógépén, ha először klónozása a csomópont-todo tárház, nyissa meg a `node-todo/config/database.js` a kedvenc szerkesztő fájlt, és módosítsa az URL-cím a kódolt érték például `mongodb://...` való `process.env.MONGODB`.

A változtatások véglegesítése a határidő, és küldje le a GitHub-főkiszolgáló:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Sajnos ez nem közzéteheti a módosítást a webes előtér virtuális gépet. Most Meggyőződünk néhány további módosításokat egy egyszerű, de hatékony mechanizmus a frissítések közzétételéhez, így gyorsan figyelheti a változásokat az éles környezetben engedélyezése virtuális gépekhez.

### <a name="configuring-the-web-frontend-virtual-machine"></a>A webes előtér virtuális gép konfigurálása
Előhívja a korábban létrehozott egy operációs rendszer klón a csomópont-todo tárház a webes előtér virtuális gépen. Az elemről kiderül, hogy ez a művelet létrehozott egy új távoli Git, amelyhez a módosítások továbbíthatja. Azonban egyszerűen küldését a ebben a távoli nem elég biztosítják a gyors iterációs modellt, amely a fejlesztők olyan eszközökre vonatkozóan feldolgozása során a rendszer a kódot.

Mi szeretnénk végezhető el, győződjön meg arról, hogy a virtuális gépen a távoli tárházba leküldéses akkor fordul elő, amikor a futó Teendőlista alkalmazás automatikusan frissül. Szerencsére Ez az egyszerű git eléréséhez.

Git egy hurkok, melynek neve adott időpontokban reagálni a tárház végzett műveletek számát mutatja. Ezek megjelennek a rendszerhéj-parancsfájlok használata a tárházban `hooks` mappát. A leginkább megfelelő az automatikus frissítési forgatókönyv hook van a `post-update` esemény.

Az SSH-munkamenetet a webes előtér virtuális géphez, így a `~/node-todo.git/hooks` könyvtár, és adja hozzá a tartalmat a következő nevű fájlba `post-update` (cseréje `machinename` és `region` a MongoDB-virtuális gép adatait):

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

Győződjön meg arról, ez a fájl végrehajtható, a következő parancs futtatásával:

    chmod 755 post-update

Ez a parancsfájl biztosítja, hogy az aktuális kiszolgáló alkalmazás le van állítva, a klónozott tárház a kód frissítése, hogy a legújabb, frissített függőségek teljesülnek, és a kiszolgáló újraindítása. Emellett biztosítja, hogy a környezet előkészítése a fogadásához az első alkalmazás frissítése a MongoDB-példány lekérése egy környezeti változó lett konfigurálva.

### <a name="configuring-your-development-machine"></a>A fejlesztői számítógépén konfigurálása
Most folytassuk a fejlesztési számítógépén csatlakoztatnia kell a webes előtér virtuális gépet. Ez a más dolga, mint az operációs rendszer tárház hozzáadása a virtuális gépen, mint egy távoli. Ehhez a következő parancsot (cseréje *felhasználói* a webes előtér virtuális gép bejelentkezési névvel és *machinename* és *régió* szükség szerint):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

Mást nem kérdez le engedélyezéséhez szükséges, vagy érvényben közzététel, megváltozik a webes előtér virtuális géphez.

### <a name="publishing-updates"></a>Közzétételi frissítések
Most közzététele a egy módosítás tett eddig, hogy az alkalmazás saját MongoDB-példány használja:

    git push azure master

Ez hasonló kimenetnek kell megjelennie:

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
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
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Ez a parancs után frissítse az alkalmazást egy webböngészőben. Tudja meg, hogy az itt bemutatott teendőlista üres és a megosztott telepített MongoDB-példány már nem kötött kell lennie.

Az oktatóanyag elvégzéséhez most Meggyőződünk egy másik, több látható módosítása. A fejlesztői számítógépén nyissa meg node-todo/public/index.html a kedvenc szerkesztőjével. Keresse meg a jumbotron fejlécet, és módosítsa a címet a "Vagyok a teendőlista aholic" a "Vagyok a teendőlista-aholic Azure!".

Most tegyük véglegesítése:

    git commit -am "Azurify the title"

Ez alkalommal, most közzéteheti a módosítást az Azure-bA előtt, hogy a GitHub-tárház biztonsági:

    git push azure master

Ez a parancs után frissítse a weblapot, és látni fogja a módosításokat. Mivel azok szépen, küldje le a módosítás vissza és a forrás távoli: 

    git push origin master

## <a name="next-steps"></a>Következő lépések
Ez a cikk bemutatta, hogyan számára a Node.js-alkalmazás és a központilag telepítenie kell az Azure-ban futó Linux virtuális gépek. Linux virtuális gépek Azure-ban kapcsolatos további információkért lásd: [Bevezetés az Azure-on Linux](/documentation/articles/virtual-machines-linux-introduction/).

A Node.js-alkalmazásoknak az Azure-on történő fejlesztéséről további információkat a következő témakörben talál: [Node.js fejlesztői központ](/develop/nodejs/).

