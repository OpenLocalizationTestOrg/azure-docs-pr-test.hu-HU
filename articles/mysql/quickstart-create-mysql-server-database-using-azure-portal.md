---
title: "Első lépések: Azure-adatbázis létrehozása MySQL-kiszolgálóhoz - Azure Portal | Microsoft Docs"
description: "Ez a cikk lépéseket használatával végigvezeti az Azure portál tooquickly hello öt perc MySQL kiszolgáló minta Azure-adatbázis létrehozása."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.openlocfilehash: d5754fe7a6f0f0f4b3fa19d456c4e15e64ca396c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával
Azure MySQL-adatbázis egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezelése, és magas rendelkezésre állású MySQL-adatbázisok hello felhőben méretezni. A gyors üzembe helyezés bemutatja, hogyan toocreate egy Azure-adatbázis hello Azure-portál használatával KB MySQL-kiszolgáló esetében. 

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure
Nyissa meg a webböngészőt, és keresse meg a toohello [Microsoft Azure-portálon](https://portal.azure.com/). Adja meg a hitelesítő adatok toosign toohello portálon. az alapértelmezett nézet hello a szolgáltatás irányítópultján.

## <a name="create-azure-database-for-mysql-server"></a>Azure-adatbázis létrehozása MySQL-kiszolgálóhoz
A MySQL-kiszolgálóhoz készült Azure-adatbázis [számítási és tárolási erőforrások](./concepts-compute-unit-and-storage.md) egy meghatározott készletével együtt jön létre. hello server belül létrejön egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md).

Kövesse ezeket a lépéseket toocreate egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis:

1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található (+) gombra.

2. Válassza ki **adatbázisok** a hello **új** lapon, és válassza ki **MySQL az Azure-adatbázis** a hello **adatbázisok** lap. Is beírhat **MySQL** a hello új lap keresési mezőbe toofind hello szolgáltatást.
![Azure Portal - új - adatbázis - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Hello új kiszolgáló részletei űrlap kitöltése a következő információ, hello kép megelőző hello szerint:

    **Beállítás** | **Ajánlott érték** | **Mező leírása** 
    ---|---|---
    Kiszolgálónév | myserver4demo | Válasszon egy egyedi nevet, amely azonosítja a MySQL-kiszolgálóhoz készült Azure-adatbázist. hello tartománynév *mysql.database.azure.com* ad meg az alkalmazások tooconnect a hozzáfűzött toohello kiszolgálónév. hello kiszolgálónév csak kisbetűket, számokat és kötőjel (-) karakter hello tartalmazhat, és 3 és 63 karakter kell tartalmaznia.
    Előfizetés | Az Ön előfizetése | hello Azure-előfizetést, amelyet az toouse a kiszolgáló. Ha több előfizetéssel rendelkezik, válassza ki a hello amelyben hello erőforrás lesz számlázva, a megfelelő előfizetést.
    Erőforráscsoport | myResourceGroup | Meghatározhat egy új erőforráscsoport-nevet, vagy használhat egy meglévőt az előfizetéséből.
    Kiszolgáló-rendszergazdai bejelentkezés | myadmin | Ellenőrizze a saját bejelentkezési fiók toouse toohello kiszolgálóhoz kapcsolódáskor. hello rendszergazda bejelentkezési név nem lehet "azure_superuser", "admin", "rendszergazda", "Gyökér", "Vendég" vagy "public".
    Jelszó | *A választása szerint* | Hozzon létre egy új jelszót hello kiszolgáló rendszergazdai fiókot. A 8 too128 karaktereket kell tartalmaznia. A jelszó tartalmaznia kell legalább hármat a következő kategóriák hello – az angol ábécé betűket, angol ábécé kisbetűi, számok (0-9) és egyéb karakterek (!, $, #, %, stb.).
    Jelszó megerősítése | *A választása szerint*| Erősítse meg a hello rendszergazdai fiók jelszavát.
    Hely | *hello régió legközelebbi tooyour felhasználók*| Válassza ki a legközelebbi tooyour felhasználók vagy más Azure-alkalmazások hello helyet.
    Verzió | *Válassza ki a hello legújabb verziója*| Válassza ki hello legújabb verziót, hacsak nem rendelkezik konkrét követelmények.
    Tarifacsomag | **Alapszintű**, **50 számítási egység****50 GB** | Kattintson a **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új adatbázishoz. Válasszon **alapszintű rétegben** hello lapon hello tetején. Kattintson a bal szélső hello hello **számítási egység** csúszkát tooadjust hello érték toohello legalább összeg érhető el a gyors üzembe helyezés a. Kattintson a **Ok** toosave hello tarifacsomag kiválasztása. Tekintse meg a következő képernyőkép hello.
    PIN-kód toodashboard | Jelölőnégyzet | Ellenőrizze a hello **PIN-kód toodashboard** beállítás tooallow könnyű nyomon a kiszolgáló hello első irányítópult oldalán, az Azure-portálon.

    > [!IMPORTANT]
    > hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott is szükséges toolog toohello Server és az adatbázisok későbbi szakaszában a gyors üzembe helyezés. Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.
    > 

    ![Azure portál – MySQL, adja meg a szükséges hello űrlap bemeneti létrehozása](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Kattintson a **létrehozása** tooprovision hello kiszolgáló. Kiépítés néhány percet vesz igénybe, mentése too20 perc maximális.
   
5.  Hello eszköztáron kattintson **értesítések** (harang ikonra) toomonitor hello telepítési folyamat.

## <a name="configure-a-server-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabály konfigurálása

hello Azure adatbázis MySQL szolgáltatás tűzfal hello kiszolgáló-szintjén hoz létre. A tűzfal megakadályozza, hogy külső alkalmazások és eszközök csatlakozás toohello kiszolgáló és azon tárolt adatbázisokhoz hello kiszolgáló, kivéve, ha egy tűzfalszabály tooopen hello tűzfal adott IP-címekhez. 

1.  Keresse meg a kiszolgáló, hello központi telepítés befejezése után. Ha szükséges, használja a keresési funkciót. Kattintson például **összes erőforrás** hello bal oldali menüből és hello kiszolgálónevet írja be (például hello példa *myserver4demo*) az újonnan létrehozott kiszolgáló toosearch. Kattintson a kiszolgáló nevét, a hello keresési eredmény jelenik meg. Hello **áttekintése** lapon, a kiszolgáló megnyílik, és további konfigurációs lehetőségeket.

2. Hello kiszolgáló lapon válassza **kapcsolatbiztonsági**.

3.  A hello **tűzfal-szabályok** hello üres szövegmezőben a hello elemcsoportban kattintson **szabálynév** oszlop toobegin hello tűzfalszabály létrehozása. 

    A gyors üzembe helyezés, a most oszthatja az IP-címek hello server kitöltésével hello szövegmezőben az egyes oszlopok a következő értékek hello:

    Szabály neve | Kezdő IP-cím | Záró IP-cím 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. A felső eszköztárán hello hello **kapcsolatbiztonsági** lapján kattintson **mentése**. Várjon néhány percet, és figyelje meg hello értesítést jelenít meg, hogy kapcsolatbiztonsági frissítése sikeresen befejeződött a folytatás előtt.

    > [!NOTE]
    > Kapcsolatok tooAzure MySQL adatbázis 3306 porton keresztül kommunikálnak. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a port 3306 kimenő forgalmát. Ha igen, csak akkor tudja tooconnect tooyour server kivéve, ha az IT-részleg 3306 portot nyit meg.
    > 

## <a name="get-hello-connection-information"></a>Hello kapcsolatadatok beolvasása
tooconnect tooyour adatbázis-kiszolgáló, tooremember hello teljes kiszolgáló nevét és a rendszergazdai bejelentkezési hitelesítő adatok szükségesek. Előfordulhat, hogy a feljegyzett ezeket az értékeket hello gyors üzembe helyezés cikk korábbi részében. Abban az esetben, ha nem, könnyedén megtalálhatja a hello kiszolgáló nevét és a bejelentkezési adatok hello kiszolgálóról **áttekintése** lap vagy hello **tulajdonságok** hello Azure portálra a lap.

1. Nyissa meg kiszolgáló **Áttekintés** lapját. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**. 
    A kurzorral rámutat minden mező, és hello másolás ikon toohello jobb hello szöveg jelenik meg. Kattintson a hello másolás ikon szükséges toocopy hello értékként.

    Ebben a példában hello kiszolgáló neve, *myserver4demo.mysql.database.azure.com*, és a kiszolgáló-rendszergazdai bejelentkezés hello  *myadmin@myserver4demo* .

## <a name="connect-toomysql-using-mysql-command-line-tool"></a>Csatlakozás tooMySQL mysql parancssori eszközzel
Több alkalmazások használható tooconnect tooyour Azure adatbázis MySQL-kiszolgáló. Első alkalommal a hello [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) parancssori eszköz hogyan tooillustrate tooconnect toohello kiszolgáló.  Egy webes böngésző is használhat, és hello Azure Cloud rendszerhéj nélkül hello itt leírt módon kell tooinstall további szoftvereket. Ha helyileg van telepítve, a saját számítógépre hello mysql segédprogram, ott is lehet csatlakoztatni.

1. Indítsa el az Azure felhőalapú rendszerhéj hello keresztül hello terminál ikon (> _) hello a felső, jobb oldalán hello Azure portál weblap.

2. hello Azure Cloud rendszerhéj megnyitása a böngészőben, amely lehetővé teszi, tootype bash rendszerhéjat parancsok.

    ![Parancssor - mysql parancssor példa](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. Hello felhő rendszerhéj parancssorába csatlakoztassa a MySQL-kiszolgáló Azure-adatbázis tooyour beírásával hello mysql parancssori zöld hello.

    a következő formátumban hello használt tooconnect tooan Azure adatbázis hello mysql segédprogram MySQL-kiszolgáló:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Például a következő parancs hello tooour példa kiszolgáló csatlakozik:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    mysql-paraméter |Ajánlott érték|Leírás
    ---|---|---
    --host | *kiszolgáló neve* | Adja meg a hello kiszolgálónév hello Azure adatbázis MySQL a korábban létrehozott használt. Az itt látható példakiszolgáló a myserver4demo.mysql.database.azure.com. Hello teljesen minősített tartománynevét használja (\*. mysql.database.azure.com) hello példában látható módon. Kövesse hello hello előző szakasz tooget hello kapcsolati adatokat, ha nem emlékszik a kiszolgáló nevét. 
    --user | *kiszolgáló-rendszergazdai bejelentkezési név* |Írja be a hello server admin bejelentkezési felhasználónevének megadni, ha az Azure-adatbázis hello MySQL a korábban létrehozott. Ha nem emlékszik hello felhasználónév, kövesse a hello előző szakasz tooget hello kapcsolatadatok hello lépéseit.  hello formátuma  *username@servername* .
    --password | *várjon a hitelesítőadat-kérésig* | Kérni fogja túl adja meg a "Jelszó megadni" követően a hello parancsot. Amikor a rendszer kéri, írja be a hello azonos hello kiszolgáló létrehozásakor megadott jelszót.  Megjegyzés: hello írta be a jelszó karakterek nem látható a hello bash Rákérdezés amikor beírja őket. Nyomja le az enter adta meg az összes hello karakterek tooauthenticate és csatlakozás után.

   A csatlakozás után hello mysql segédprogram megjeleníti a `mysql>` Rákérdezés tootype parancsok meg. 

    Példa a mysql-kimenetre:
    ```bash
    Welcome toohello MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' tooclear hello current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Ha hello tűzfal nincs konfigurálva a tooallow hello IP-címe hello Azure Cloud rendszerhéj, hello alábbi hiba akkor fordul elő:
    >
    > Hiba (28000) 2003: Ügyfél IP-cím 123.456.789.0 tooaccess hello kiszolgáló nem engedélyezett.
    >
    > tooresolve hello hiba, győződjön meg arról, hogy hello kiszolgáló konfigurációja megfelel hello hello szükséges lépések *konfigurálása egy kiszolgálószintű tűzfalszabályt* hello című szakaszban.

4. Nézet kiszolgáló állapota tooensure hello kapcsolat működőképességét. Típus `status` : hello mysql > kérni, ha van csatlakoztatva.
    ```sql
    status
    ```

   > [!TIP]
   > További parancsokért lásd: [az MySQL 5.7 referenciaútmutatójának 4.5.1-es fejezetét](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

5.  Hozzon létre egy üres adatbázist hello mysql > Rákérdezés hello a következő parancs beírásával:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    Hello parancsot is igénybe vehet néhány percet toocomplete. 

    A MySQL-kiszolgálóhoz létrehozott Azure-adatbázisban egy vagy több adatbázist is létrehozhat. Részt vesz egy önálló adatbázis / server tooutilize toocreate hello erőforrások, vagy hozzon létre több adatbázisok tooshare hello erőforrásokat. Nem hozható létre adatbázis korlát toohello száma, de több adatbázis közös hello ugyanahhoz a kiszolgáló erőforrásait. 

6. Hello mysql hello adatbázisaiban listában > Rákérdezés hello a következő parancs beírásával:

    ```sql
    SHOW DATABASES;
    ```

7.  Típus `\q` , és nyomja le az ENTER tooquit hello mysql eszköz. Hello Azure Cloud rendszerhéj bezárhatja, miután befejezte az.

Most már rendelkezik Azure-adatbázis toohello MySQL a kapcsolat, és létrehozott egy üres felhasználói adatbázist. Toohello következő szakasz toorepeat egy hasonló a gyakorlatban tooconnect toohello továbbra is ugyanazon a kiszolgálón egy másik közös eszközzel, MySQL-munkaterületet.

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a>Csatlakoztassa a kiszolgálót hello MySQL munkaterület grafikus felhasználói Felülettel eszközzel toohello
tooconnect tooAzure MySQL kiszolgáló grafikus felhasználói Felülettel hello eszközzel MySQL-munkaterületet:

1.  Indítsa el a hello MySQL munkaterület alkalmazás az ügyfélszámítógépre. A MySQL Workbench-et [innen](https://dev.mysql.com/downloads/workbench/) töltheti le és telepítheti.

2.  A **új kapcsolat beállítása** párbeszédpanelen adja meg következő információ hello **paraméterek** lapon:

    ![új kapcsolat beállítása](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Beállítás** | **Ajánlott érték** | **Mező leírása** |
    |---|---|---|
    |   Kapcsolat neve | Bemutató kapcsolat | Adjon meg egy címkét a kapcsolathoz. |
    | Kapcsolati módszer | Standard (TCP/IP) | A Standard (TCP/IP) elégséges. |
    | Gazdanév | *kiszolgáló neve* | Adja meg a hello kiszolgálónév hello Azure adatbázis MySQL a korábban létrehozott használt. Az itt látható példakiszolgáló a myserver4demo.mysql.database.azure.com. Hello teljesen minősített tartománynevét használja (\*. mysql.database.azure.com) hello példában látható módon. Kövesse hello hello előző szakasz tooget hello kapcsolati adatokat, ha nem emlékszik a kiszolgáló nevét.  |
    | Port | 3306 | Mindig használjon port 3306 MySQL adatbázis tooAzure kapcsolódáskor. |
    | Felhasználónév |  *kiszolgáló-rendszergazdai bejelentkezési név* | Írja be a hello server admin bejelentkezési felhasználónevének megadni, ha az Azure-adatbázis hello MySQL a korábban létrehozott. A példában szereplő felhasználónév a következő: myadmin@myserver4demo. Ha nem emlékszik hello felhasználónév, kövesse a hello előző szakasz tooget hello kapcsolatadatok hello lépéseit. hello formátuma  *username@servername* .
    | Jelszó | az ön jelszava | Kattintson a tárolóban... gombra toosave hello jelszó tárolásához. |

    Kattintson a **kapcsolat tesztelése** tootest, ha az összes paraméter megfelelően vannak konfigurálva. Kattintson az OK toosave hello kapcsolat. 

    > [!NOTE]
    > SSL alapértelmezés szerint a kiszolgálón érvényben van, és további konfigurálást igényli a rendelés tooconnect sikeresen megtörtént. Lásd: [konfigurálása az SSL-kapcsolat az alkalmazás toosecurely a MySQL adatbázis tooAzure kapcsolati](./howto-configure-ssl.md).  A gyors üzembe helyezés toodisable SSL használni szeretne, ha látogasson el a hello Azure-portálon, és kattintson hello kapcsolat biztonsági lap toodisable hello kényszerítése SSL kapcsolat váltása gombra.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása
Hello gyors üzembe helyezés létrehozott hello erőforrások karbantartása vagy törlésével hello [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md), amely erőforrásokat is magában foglalja az összes hello hello erőforráscsoportban vagy egy kiszolgáló-erőforrás hello törlésével, ha azt szeretné tookeep hello egyéb erőforrások változatlanok maradnak.

> [!TIP]
> Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épül. Ha toocontinue, a következő toowork quickstarts, így nem karbantartáshoz hello a gyors üzembe helyezés a létrejött erőforrásokat. Ha nem tervezi toocontinue, használja a hello által a hello Azure-portálon található gyors üzembe helyezési lépéseket toodelete összes erőforrás létrehozása után.
>

toodelete hello teljes erőforráscsoport található, újonnan létrehozott hello server beleértve:
1.  Keresse meg az erőforráscsoport hello Azure-portálon. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** majd például a fenti példában az erőforráscsoport neve hello **myresourcegroup**.
2.  Az erőforráscsoport oldalán kattintson a **Törlés** parancsra. Az erőforráscsoport, például a fenti példában majd típusának hello neve **myresourcegroup**, a hello szöveg mezőben tooconfirm törlésre, és kattintson **törlése**.

Vagy ehelyett toodelete hello az újonnan létrehozott kiszolgálón:
1.  Ha nincs megnyitva, keresse meg a kiszolgáló hello Azure-portálon. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás**, majd keresse meg a létrehozott hello kiszolgáló.
2.  A hello **áttekintése** hello kattintson **törlése** hello felső ablaktábla gombjára.
![MySQL-hez készült Azure-adatbázis – kiszolgáló törlése](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Erősítse meg a kiszolgálónév hello toodelete szeretne, és hello tartozó adatbázisok érintett megjelenítése. Adja meg a kiszolgáló nevét hello szövegmezőben, például a fenti példában **myserver4demo**, és kattintson a **törlése**.

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Az első Azure-adatbázis MySQL-adatbázishoz megtervezése](./tutorial-design-database-using-portal.md)

