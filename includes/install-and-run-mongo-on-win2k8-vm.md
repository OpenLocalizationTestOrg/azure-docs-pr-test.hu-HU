Kövesse az alábbi lépéseket tooinstall, és futtassa a MongoDB Windows Server rendszerű virtuális gépen.

> [!IMPORTANT]
> Alapértelmezés szerint nem engedélyezettek a MongoDB biztonsági funkciók, például hitelesítés és az IP-cím kötés. Biztonsági funkciókat engedélyezni kell a MongoDB tooa éles környezetben üzembe helyezése előtt.  További információkért lásd: [biztonsági és hitelesítési](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. A távoli asztal toohello virtuális gép csatlakoztatása után nyissa meg az Internet Explorer hello **Start** menü hello virtuális gépen.
2. Jelölje be hello **eszközök** hello jobb felső sarkában található gombra.  A **Internetbeállítások**, jelölje be hello **biztonsági** lapra, majd válassza ki a hello **megbízható helyek** ikonra, végül kattintson a hello **helyek** jelölt gombra. Adja hozzá *https://\*. mongodb.org* toohello a megbízható helyek listájához.
3. Nyissa meg túl[letölti – MongoDB](https://www.mongodb.com/download-center#community).
4. Hello található **jelenlegi stabil kiadásban** a **közösségi Server**, jelölje be legújabb hello **64 bites** változatban a Windows hello. Töltse le, majd futtassa a hello MSI telepítő.
5. MongoDB telepítése általában a C:\Program Files\MongoDB. Keresse meg a környezeti változók hello asztalán, és hello MongoDB bináris fájlok elérési útja toohello ELÉRÉSI változó hozzáadása. Például előfordulhat hello bináris fájljai a C:\Program Files\MongoDB\Server\3.4\bin a számítógépen.
6. MongoDB adatainak és naplókönyvtárainak könyvtárak létrehozása a hello adatlemez (például meghajtó **F:**) hello előző lépésekben létrehozott. A **Start**, jelölje be **parancssor** tooopen egy parancssori ablakot.  Típus:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. toorun hello adatbázis futtatásához:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Összes naplózási üzenetek irányított toohello *F:\MongoLogs\mongolog.log* mongod.exe server elindul, és Adatbázisnapló-fájlok preallocates fájlt. Ez hello Adatbázisnapló-fájlok a MongoDB toopreallocate néhány percet igénybe vehet, és indítsa el a kapcsolatfigyelést. hello parancssor marad összpontosítanak ezt a feladatot, a MongoDB-példány futtatása közben.
8. toostart hello MongoDB felügyeleti rendszerhéjat, nyisson meg egy másik parancs ablakot a **Start** és hello típus a következő parancsokat:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    hello adatbázist hello insert hozta létre.
9. Másik lehetőségként telepítése mongod.exe szolgáltatásként:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    A szolgáltatást, MongoDB nevű "Mongo DB" leírását. Hello `--logpath` beállítás kell használt toospecify naplófájl, óta hello szolgáltatást futtató nem rendelkezik egy toodisplay parancs kimenetét.  Hello `--logappend` beállítás megadja, hogy hello szolgáltatás újraindítását okozza-e a kimeneti tooappend toohello naplófájlokat.  Hello `--dbpath` beállítás hello adatkönyvtára hello helyét adja meg. A szolgáltatással kapcsolatos további parancssori kapcsolókat lásd: [parancssori kapcsolók szolgáltatással kapcsolatos][MongoWindowsSvcOptions].

    toostart hello szolgáltatást, futtassa a parancsot:

        C:\> net start MongoDB
10. Most, hogy a MongoDB telepítve van és fut, a Windows tűzfal port tooopen kell tehát távolról csatlakozhat tooMongoDB.  A hello **Start** menü **felügyeleti eszközök** , majd **fokozott biztonságú Windows tűzfal**.
11. a) hello bal oldali ablaktáblában jelöljön ki **bejövő szabályok**.  A hello **műveletek** ablaktáblán a jobb oldali hello válassza **új szabály létrehozása...** .

    ![A Windows tűzfal][Image1]

    b) a hello **új bejövő szabály varázsló**, jelölje be **Port** majd **következő**.

    ![A Windows tűzfal][Image2]

    c) válassza **TCP** , majd **adott helyi portok**.  Adjon meg egy portot az "27017" (hello alapértelmezett portot figyel MongoDB), és kattintson a **következő**.

    ![A Windows tűzfal][Image3]

    d) válassza **hello csatlakozás engedélyezése** kattintson **következő**.

    ![A Windows tűzfal][Image4]

    e) kattintson **következő** újra.

    ![A Windows tűzfal][Image5]

    f) adjon meg egy nevet hello szabályhoz, például "MongoPort", és kattintson a **Befejezés**.

    ![A Windows tűzfal][Image6]

12. Ha nem konfigurál egy olyan végpont mongodb hello virtuális gép létrehozása után, most is elvégezhető. Távolról kell hello tűzfalszabály és hello végpont toobe képes tooconnect tooMongoDB is.

  Hello Azure-portálon, kattintson **virtuális gépek (klasszikus)**, kattintson az új virtuális gép hello neve, majd **végpontok**.

    ![Végpontok][Image7]

13. Kattintson az **Add** (Hozzáadás) parancsra.

14. "Mongo" protokoll nevű végpont hozzáadása **TCP**, és mindkét **nyilvános** és **titkos** portok készlet túl "27017". Ez a port megnyitása lehetővé teszi, hogy a MongoDB toobe érhető el távolról.

    ![Végpontok][Image9]

> [!NOTE]
> hello port 27017 hello alapértelmezett portot használják a MongoDB. Hello megadásával módosíthatja az alapértelmezett port `--port` paraméter hello mongod.exe kiszolgáló indításakor. Győződjön meg arról, hogy toogive hello ugyanazt a portszámot a hello tűzfal és az előző utasítások hello "Mongo" végpont hello.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
