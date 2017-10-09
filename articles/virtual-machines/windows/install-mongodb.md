---
title: a Windows Azure-ban a MongoDB aaaInstall |} Microsoft Docs
description: "Ismerje meg, hogyan tooinstall MongoDB egy Windows Server 2012 R2 rendszert futtató Azure virtuális gépen létre hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: becd2c607d098e2bc806139e03f2c42f1f01f6f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Telepítse és konfigurálja a Windows Azure-ban mongodb-Protokolltámogatással
[MongoDB](http://www.mongodb.org) egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis. Ez a cikk végigvezeti telepítése és konfigurálása a MongoDB a Windows Server 2012 R2 virtuális gépen (VM) az Azure-ban. Emellett [a MongoDB telepítése egy Linux virtuális gépre az Azure-ban](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Előfeltételek
Mielőtt telepítése és konfigurálása a MongoDB, meg kell toocreate egy virtuális Gépet és, ideális esetben az adatok lemezre tooit hozzáadása. Tekintse meg a következő cikkek toocreate egy virtuális gép hello, és hozzá adatlemezt:

* Hozzon létre egy Windows Server virtuális gép az [hello Azure-portálon](quick-create-portal.md) vagy [Azure PowerShell](quick-create-powershell.md).
* Adatok tooa Windows Server virtuális gép használatával csatolása [hello Azure-portálon](attach-managed-disk-portal.md) vagy [Azure PowerShell](attach-disk-ps.md).

toobegin telepítése és konfigurálása a MongoDB, [jelentkezzen be Windows Server virtuális gép tooyour](connect-logon.md) távoli asztal használatával.

## <a name="install-mongodb"></a>A MongoDB telepítése
> [!IMPORTANT]
> Alapértelmezés szerint nem engedélyezettek a MongoDB biztonsági funkciók, például hitelesítés és az IP-cím kötés. Biztonsági funkciókat engedélyezni kell a MongoDB tooa éles környezetben üzembe helyezése előtt. További információkért lásd: [MongoDB biztonsági és hitelesítési](http://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. Távoli asztal használó virtuális gépek tooyour csatlakoztatása után nyissa meg az Internet Explorer hello **Start** hello VM menüjéből.
2. Válassza ki **az ajánlott biztonsági, adatvédelmi és kompatibilitási beállítások** Ha Internet Explorer először, majd kattintson az **OK**.
3. Internet Explorer fokozott biztonsági beállításai alapértelmezés szerint engedélyezve van. Adja hozzá az engedélyezett webhelyek hello MongoDB webhely toohello listájához:
   
   * Jelölje be hello **eszközök** hello jobb felső sarokban látható ikonra.
   * A **Internetbeállítások**, jelölje be hello **biztonsági** lapra, majd válassza ki a hello **megbízható helyek** ikonra.
   * Kattintson a hello **helyek** gombra. Adja hozzá *https://\*. mongodb.org* toohello listáját a megbízható helyek, valamint majd Bezárás hello párbeszédpanel megnyitásához.
     
     ![Internet Explorer biztonsági beállításainak konfigurálása](./media/install-mongodb/configure-internet-explorer-security.png)
4. Keresse meg a toohello [tölti le a MongoDB -](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads) lap.
5. Ha szükséges, jelölje be a hello **közösségi Server** edition és jelölje ki hello legújabb aktuális stabil verzióját a Windows Server 2008 R2 64 bites és újabb verzióihoz. toodownload hello installer, kattintson a **letöltési (msi)**.
   
    ![Töltse le a MongoDB-telepítő](./media/install-mongodb/download-mongodb.png)
   
    Hello telepítő futtatásához hello letöltés befejezése után.
6. Olvassa el és fogadja el a licencszerződést hello. Amikor a rendszer kéri, válassza ki a **Complete** telepítése.
7. Hello végső képernyőn kattintson a **telepítése**.

## <a name="configure-hello-vm-and-mongodb"></a>Virtuális gép hello és MongoDB konfigurálása
1. hello MongoDB telepítési nem frissíti a hello elérésiút-változókat. Hello MongoDB nélkül `bin` helyre a path változóban, kell toospecify hello teljes elérési útja a MongoDB végrehajtható fájl használata során. tooadd hello hely tooyour elérésiút-változónak:
   
   * Kattintson a jobb gombbal hello **Start** menüben, és válassza a **rendszer**.
   * Kattintson a **Speciális rendszerbeállítások**, és kattintson a **környezeti változók**.
   * A **rendszerváltozók**, jelölje be **elérési**, és kattintson a **szerkesztése**.
     
     ![Az ELÉRÉSIÚT-változókat konfigurálása](./media/install-mongodb/configure-path-variables.png)
     
     Adja hozzá a hello elérési tooyour MongoDB `bin` mappa. MongoDB telepítése általában a *C:\Program Files\MongoDB*. Ellenőrizze a virtuális gép hello telepítési útvonalán. hello következő példakóddal felveheti a hello alapértelmezett MongoDB telepítési helye toohello `PATH` változó:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > Lehet, hogy tooadd hello vezető pontosvessző (`;`), hogy a hely tooyour hozzáadni kívánt tooindicate `PATH` változó.

2. Az adatok a lemezen létrehozni a MongoDB adatainak és naplókönyvtárainak könyvtárak. A hello **Start** menüjében válassza **parancssor**. a következő példák hello hello könyvtárak létrehozása F: meghajtón
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. A MongoDB-példány kezdődnie hello a következő parancsot, hello elérési tooyour adatok beállítása, és ennek megfelelően jelentkezzen könyvtárak:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Ez hello Adatbázisnapló-fájlok a MongoDB tooallocate néhány percet igénybe vehet, és indítsa el a kapcsolatfigyelést. Összes naplózási üzenetek irányított toohello *F:\MongoLogs\mongolog.log* tárolási `mongod.exe` server elindul, és lefoglalja a Adatbázisnapló-fájlok.
   
   > [!NOTE]
   > hello parancssor marad összpontosítanak ezt a feladatot, a MongoDB-példány futtatása közben. Hello parancssori ablak nyitva toocontinue MongoDB futtató hagyja. Vagy a MongoDB telepítése a következő lépés hello szolgáltatást.

4. Robusztusabb MongoDB élmény érdekében telepítse a hello `mongod.exe` szolgáltatásként. Szolgáltatás létrehozása azt jelenti, hogy nem kell tooleave toouse MongoDB minden alkalommal fut egy parancssort. Hello szolgáltatás létrehozása az alábbiak szerint elérési tooyour adatainak és naplókönyvtárainak könyvtárak hello beállítása ennek megfelelően:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    hello előző parancs létrehoz egy szolgáltatást MongoDB, nevű "Mongo DB" leírását. a következő paraméterek hello is meg vannak adva:
   
   * Hello `--dbpath` beállítás hello adatkönyvtára hello helyét adja meg.
   * Hello `--logpath` beállítást meg kell használt toospecify egy naplófájl, mert hello futó szolgáltatás nem rendelkezik egy toodisplay parancs kimenetét.
   * Hello `--logappend` beállítás megadja, hogy hello szolgáltatás újraindítását okozza-e a kimeneti tooappend toohello naplófájlokat.
   
   toostart hello MongoDB szolgáltatást, futtathatja a következő parancs hello:
   
    ```
    net start MongoDB
    ```
   
    Hello MongoDB szolgáltatás létrehozásával kapcsolatos további információkért lásd: [egy Windows-szolgáltatás konfigurálása a mongodb-protokolltámogatással](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-hello-mongodb-instance"></a>Teszt hello MongoDB-példány
Egy példányban fut, vagy a szolgáltatásként telepített mongodb programot, és most elindíthatja létrehozása és használata az adatbázisokat. toostart hello MongoDB felügyeleti rendszerhéjat, nyisson meg egy másik parancssort a hello **Start** menü, és írja be a következő parancs hello:

```
mongo  
```

Hello hello adatbázisokkal listázhatja `db` parancsot. Adatok beszúrása az alábbiak szerint:

```
db.foo.insert( { a : 1 } )
```

Adatok keresése az alábbiak szerint:

```
db.foo.find()
```

hello hasonló toohello a következő példa a kimenetre:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Kilépés hello `mongo` konzolon a következőképpen:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Konfigurálja a tűzfal és a hálózati biztonsági csoportszabályok
Most, hogy a MongoDB telepítve és fut, nyissa meg a portot a Windows tűzfal, távolról csatlakozhat a tooMongoDB. toocreate új bejövő szabály tooallow TCP-port 27017, nyisson meg egy rendszergazda PowerShell-parancssort, és írja be a következő parancs hello:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

Hello szabály hello segítségével is létrehozhat **fokozott biztonságú Windows tűzfal** grafikus kezelőeszköz. Hozzon létre egy új bejövő szabály tooallow TCP-port 27017.

Szükség esetén hozzon létre egy hálózati biztonsági csoport szabály tooallow hozzáférés tooMongoDB a hello meglévő Azure virtuális hálózati alhálózat kívül. Létrehozhat hello hálózati biztonsági csoportszabályok hello segítségével [Azure-portálon](nsg-quickstart-portal.md) vagy [Azure PowerShell](nsg-quickstart-powershell.md). Hello Windows tűzfal-szabályokat, az TCP port 27017 toohello virtuális hálózati adapter a MongoDB VM engedélyezése.

> [!NOTE]
> TCP-port 27017 hello alapértelmezett portot használják a MongoDB. Hello segítségével módosíthatja ezt a portot `--port` paraméter indításakor `mongod.exe` manuálisan, vagy egy szolgáltatást. Ha megváltoztatja hello portot, győződjön meg arról, hogy tooupdate hello Windows tűzfal és a hálózati biztonsági csoport szabályok az előző lépésekben hello.


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban, megtudta, hogyan tooinstall és MongoDB konfigurálása a Windows virtuális gépén. Most már elérheti az MongoDB a Windows virtuális gépre, a következő hello témakörei speciális hello [MongoDB dokumentációt](https://docs.mongodb.com/manual/).

