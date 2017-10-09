---
title: "Azure Portal: Azure-adatbázis létrehozása PostgreSQL-hez | Microsoft Docs"
description: "Gyors útmutató toocreate start, és Azure adatbázis kezeléséhez Azure portál felhasználói felületének használatával PostgreSQL-kiszolgáló."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 08/10/2017
ms.openlocfilehash: 61753268147d6ea283d1aa5d6baf269d2756d25a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-in-hello-azure-portal"></a>Hozzon létre egy Azure-adatbázis PostgreSQL a hello Azure-portálon

Azure PostgreSQL-adatbázishoz egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezeléséhez, és magas rendelkezésre állású PostgreSQL-adatbázisok hello felhőben méretezése. A gyors üzembe helyezés bemutatja, hogyan toocreate egy Azure-adatbázis hello Azure-portál használatával KB PostgreSQL-kiszolgáló esetében.

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon
Nyissa meg a webböngészőt, és keresse meg a toohello [Microsoft Azure-portálon](https://portal.azure.com/). Adja meg a hitelesítő adatok toosign toohello portálon. az alapértelmezett nézet hello a szolgáltatás irányítópultján.

## <a name="create-an-azure-database-for-postgresql"></a>Azure-adatbázis létrehozása PostgreSQL-hez

Az Azure-adatbázis PostgreSQL-kiszolgálóhoz [számítási és tárolási erőforrások](./concepts-compute-unit-and-storage.md) egy meghatározott készletével együtt jön létre. hello server belül létrejön egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md).

Kövesse ezeket a lépéseket toocreate egy PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis:
1.  Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található (+) gombra.
2.  Válassza ki **adatbázisok** a hello **új** lapon, és válassza ki **PostgreSQL az Azure-adatbázis** a hello **adatbázisok** lap.
 ![Azure-adatbázishoz tartozó PostgreSQL - hello adatbázis létrehozása](./media/quickstart-create-database-portal/1-create-database.png)

3.  Hello új kiszolgáló részletei űrlap kitöltése a következő információ, hello kép megelőző hello szerint:

    Beállítás|Ajánlott érték|Leírás
    ---|---|---
    Kiszolgálónév |*mypgserver-20170401*|Válasszon egy egyedi nevet, amely azonosítja a PostgreSQL-kiszolgálóhoz készült Azure-adatbázist. hello tartománynév *postgres.database.azure.com* ad meg az alkalmazások tooconnect a hozzáfűzött toohello kiszolgálónév. hello kiszolgálónév csak kisbetűket, számokat és kötőjel (-) karakter hello tartalmazhat, és 3 és 63 karakter kell tartalmaznia.
    Előfizetés|*Az Ön előfizetése*|hello Azure-előfizetést, amelyet az toouse a kiszolgáló. Ha több előfizetéssel rendelkezik, válassza ki a hello amelyben hello erőforrás lesz számlázva, a megfelelő előfizetést.
    Erőforráscsoport|*myresourcegroup*| Meghatározhat egy új erőforráscsoport-nevet, vagy használhat egy meglévőt az előfizetéséből.
    Kiszolgáló-rendszergazdai bejelentkezés |*mylogin*| Ellenőrizze a saját bejelentkezési fiók toouse toohello kiszolgálóhoz kapcsolódáskor. hello rendszergazda bejelentkezési név nem lehet "azure_superuser", "azure_pg_admin", "admin", "rendszergazda", "Gyökér", "Vendég" vagy "public", és nem kezdődhet "pg_".
    Jelszó |*A választása szerint* | Hozzon létre egy új jelszót hello kiszolgáló rendszergazdai fiókot. A 8 too128 karaktereket kell tartalmaznia. A jelszó tartalmaznia kell legalább hármat a következő kategóriák hello – az angol ábécé betűket, angol ábécé kisbetűi, számok (0-9) és egyéb karakterek (!, $, #, %, stb.).
    Hely|*hello régió legközelebbi tooyour felhasználók*| Válassza ki a legközelebbi tooyour felhasználók hello helyet.
    PostgreSQL-verzió|*Válassza ki a hello legújabb verziója*| Válassza ki hello legújabb verziót, hacsak nem rendelkezik konkrét követelmények.
    Tarifacsomag | **Alapszintű**, **50 számítási egység****50 GB** | Kattintson a **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új adatbázishoz. Adja meg az alapszintű csomag hello lapon hello tetején. Kattintson a bal szélső hello hello számítási egység csúszkát tooadjust hello érték toohello legkisebb a gyors üzembe helyezés érhető el. Kattintson a **Ok** toosave hello tarifacsomag kiválasztása. Tekintse meg a következő képernyőkép hello.
    | PIN-kód toodashboard | Jelölőnégyzet | Ellenőrizze a hello **PIN-kód toodashboard** beállítás tooallow könnyű nyomon a kiszolgáló hello első irányítópult oldalán, az Azure-portálon.

  > [!IMPORTANT]
  > hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében. Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.

    ![PostgreSQL - tarifacsomag kiválasztása hello Azure-adatbázis](./media/quickstart-create-database-portal/2-service-tier.png)

4.  Kattintson a **létrehozása** tooprovision hello kiszolgáló. Kiépítés néhány percet vesz igénybe, mentése too20 perc maximális.

5.  Hello eszköztáron kattintson **értesítések** toomonitor hello telepítési folyamat.
 ![Azure-adatbázis PostgreSQL-hez - Értesítések megtekintése](./media/quickstart-create-database-portal/3-notifications.png)
   
  Alapértelmezés szerint a **postgres** adatbázis a kiszolgáló alatt jön létre. Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) egy alapértelmezett adatbázis való használatra szolgál, a felhasználók, segédprogramok és harmadik féltől származó alkalmazások. 

## <a name="configure-a-server-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabály konfigurálása

hello Azure adatbázis PostgreSQL szolgáltatás tűzfal hello kiszolgáló-szintjén hoz létre. A tűzfal megakadályozza, hogy külső alkalmazások és eszközök csatlakozás toohello kiszolgáló és azon tárolt adatbázisokhoz hello kiszolgáló, kivéve, ha egy tűzfalszabály tooopen hello tűzfal adott IP-címekhez. 

1.  Keresse meg a kiszolgáló, hello központi telepítés befejezése után. Ha szükséges, használja a keresési funkciót. Kattintson például **összes erőforrás** hello bal oldali menüből és hello kiszolgálónevet írja be (például hello példa *mypgserver-20170401*) az újonnan létrehozott kiszolgáló toosearch. Kattintson a kiszolgáló nevét, a hello keresési eredmény jelenik meg. Hello **áttekintése** lapon, a kiszolgáló megnyílik, és további konfigurációs lehetőségeket.
 
    ![PostgreSQL-hez készült Azure-adatbázis – Kiszolgálónév keresése](./media/quickstart-create-database-portal/4-locate.png)

2.  Hello lapon válassza az **kapcsolatbiztonsági**. 
    ![PostgreSQL-hez készült Azure-adatbázis – Tűzfalszabály létrehozása](./media/quickstart-create-database-portal/5-firewall-2.png)

3.  A hello **tűzfal-szabályok** hello üres szövegmezőben a hello elemcsoportban kattintson **szabálynév** oszlop toobegin hello tűzfalszabály létrehozása. 

    A gyors üzembe helyezési a most oszthatja az IP-címek hello server kitöltésével hello szövegmezőben az egyes oszlopok a következő értékek hello:

    Szabály neve | Kezdő IP-cím | Záró IP-cím 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. A hello felső eszköztárán hello kapcsolat biztonsági beállításait tartalmazó lapot, kattintson **mentése**. Várjon néhány percet, és figyelje meg hello értesítést jelenít meg, hogy kapcsolatbiztonsági frissítése sikeresen befejeződött a folytatás előtt.

    > [!NOTE]
    > Kapcsolatok tooyour Azure adatbázis PostgreSQL-kiszolgáló port 5432 protokollt használó kommunikációra. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a port 5432 kimenő forgalmát. Ha igen, csak akkor tudja tooconnect tooyour server kivéve, ha az IT-részleg 5432 portot nyit meg.
    >

## <a name="get-hello-connection-information"></a>Hello kapcsolatadatok beolvasása

A PostgreSQL-kiszolgálóhoz készült Azure-adatbázis létrehozásakor egy **postgres** nevű alapértelmezett adatbázis jön létre. tooconnect tooyour adatbázis-kiszolgáló, tooremember hello teljes kiszolgáló nevét és a rendszergazdai bejelentkezési hitelesítő adatok szükségesek. Előfordulhat, hogy a feljegyzett ezeket az értékeket hello gyors üzembe helyezési cikk korábbi részében. Abban az esetben, ha nem, könnyedén megtalálhatja a hello kiszolgáló nevét és a bejelentkezési adatait hello server – áttekintés oldalra a hello Azure-portálon.

1. Nyissa meg kiszolgáló **Áttekintés** lapját. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
    A kurzorral rámutat minden mező, és hello másolás ikon toohello jobb hello szöveg jelenik meg. Kattintson a hello másolás ikon szükséges toocopy hello értékként.

 ![Azure-adatbázis PostgreSQL-hez - Kiszolgáló-rendszergazdai bejelentkezés](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-toopostgresql-database-using-psql-in-cloud-shell"></a>Csatlakozás tooPostgreSQL adatbázis psql felhő rendszerhéj használatával

Számos alkalmazás használatával tooconnect tooyour Azure Database PostgreSQL-kiszolgáló. Most először használja hello psql parancssori segédprogram tooillustrate hogyan tooconnect toohello kiszolgáló.  Egy webes böngésző is használhat, és hello Azure Cloud rendszerhéj nélkül hello itt leírt módon kell tooinstall további szoftvereket. Ha helyileg van telepítve, a saját számítógépre hello psql segédprogram, ott is lehet csatlakoztatni.

1. Indítsa el a hello Azure Cloud rendszerhéj hello terminál ikonra a felső navigációs panelen hello keresztül.

   ![Azure-adatbázis PostgreSQL-hez - Azure Cloud Shell terminálikon](./media/quickstart-create-database-portal/7-cloud-console.png)

2. hello Azure Cloud rendszerhéj megnyitása a böngészőben, amely lehetővé teszi, tootype bash rendszerhéjat parancsok.

   ![Azure-adatbázis PostgreSQL-hez - Azure Shell Bash parancssor](./media/quickstart-create-database-portal/8-bash.png)

3. Parancssorba hello felhő rendszerhéj csatlakozás beírásával hello psql parancssori zöld hello tooa adatbázis az Azure-adatbázis PostgreSQL-kiszolgáló.

    hello következő formátuma használt tooconnect tooan Azure adatbázis hello PostgreSQL kiszolgáló [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Például a következő parancs hello tooan példa kiszolgáló csatlakozik:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql paraméter |Ajánlott érték|Leírás
    ---|---|---
    --host | *kiszolgáló neve* | Adja meg a hello kiszolgálónév hello Azure adatbázis PostgreSQL a korábban létrehozott használt. Az itt látható példakiszolgáló a mypgserver-20170401.postgres.database.azure.com. Hello teljesen minősített tartománynevét használja (\*. postgres.database.azure.com) hello példában látható módon. Kövesse hello hello előző szakasz tooget hello kapcsolati adatokat, ha nem emlékszik a kiszolgáló nevét. 
    --port | **5432** | Mindig használjon port 5432 PostgreSQL tooAzure adatbázishoz kapcsolódáskor. 
    --username | *kiszolgáló-rendszergazdai bejelentkezési név* |Írja be a hello server admin bejelentkezési felhasználónevének megadni, ha az Azure-adatbázis hello PostgreSQL a korábban létrehozott. Ha nem emlékszik hello felhasználónév, kövesse a hello előző szakasz tooget hello kapcsolatadatok hello lépéseit.  hello formátuma  *username@servername* .
    --dbname | **postgres** | Hello alapértelmezett rendszer által létrehozott adatbázis név használata *postgres* hello első kapcsolathoz. Később létrehozhatja a saját adatbázisát.

    Futó hello psql parancsot, miután a saját paraméterértékekkel felszólító tootype hello kiszolgáló rendszergazdai jelszavát. Ez a jelszó van hello azonos hello kiszolgáló létrehozásakor megadott. 

    psql paraméter |Ajánlott érték|Leírás
    ---|---|---
    jelszó | *az Ön rendszergazdai jelszava* | Vegye figyelembe, hello karakterek nem látható a hello bash Rákérdezés beírt jelszó. Nyomja le az enter adta meg az összes hello karakterek tooauthenticate és csatlakozás után.

    A csatlakozás után a hello psql segédprogram sql-parancsok írhatja postgres üzenetet jelenít meg. Hello kezdeti kapcsolat kimenet a figyelmeztetés megjelenhet mivel lehet, hogy az Azure felhőalapú rendszerhéj hello hello psql hello Azure-adatbázis a PostgreSQL-kiszolgáló verziója eltérő verzióval. 
    
    Példa psql kimenetre:
    ```bash
    psql (9.5.7, server 9.6.2)
    WARNING: psql major version 9.5, server major version 9.6.
        Some psql features might not work.
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
    Type "help" for help.
   
    postgres=> 
    ```

    > [!TIP]
    > Ha hello tűzfal nincs konfigurálva a tooallow hello IP-címe hello Azure Cloud rendszerhéj, hello alábbi hiba akkor fordul elő:
    > 
    > "psql: FATAL:  no pg_hba.conf entry for host "138.91.195.82", user "mylogin", database "postgres", SSL on FATAL:  SSL connection is required. Please specify SSL options and retry.
    > 
    > tooresolve hello hiba, győződjön meg arról, hogy hello kiszolgáló konfigurációja megfelel hello hello szükséges lépések *konfigurálása egy kiszolgálószintű tűzfalszabályt* hello című szakaszban.

4.  Hozzon létre egy üres adatbázist: hello Rákérdezés hello a következő parancs beírásával:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    Hello parancsot is igénybe vehet néhány percet toocomplete. 

5.  Hello parancssorba hajtható végre a következő parancs tooswitch kapcsolat található, újonnan létrehozott toohello adatbázis hello **mypgsqldb**.
    ```bash
    \c mypgsqldb
    ```

6.  Írja be a \q, és nyomja le az ENTER tooquit psql. Hello Azure Cloud rendszerhéj bezárhatja, miután befejezte az.

Most már rendelkezik Azure-adatbázis toohello PostgreSQL a kapcsolat, és létrehozott egy üres felhasználói adatbázist. Továbbra is toohello egy másik közös eszközzel, pgAdmin következő szakasz tooconnect.

## <a name="connect-toopostgresql-database-using-pgadmin"></a>Csatlakozás tooPostgreSQL adatbázist pgAdmin használatával

tooconnect tooAzure PostgreSQL kiszolgáló grafikus felhasználói Felülettel hello eszközzel _pgAdmin_
1.  Indítsa el a hello _pgAdmin_ alkalmazás az ügyfélszámítógépre. A _pgAdmin-t_ http://www.pgadmin.org/ oldalról telepítheti.
2.  Kattintson a hello **új kiszolgáló hozzáadása** hello ikon **Gyorshivatkozások** hello Center szakasza hello irányítópult-oldalon.
3.  A hello **- kiszolgáló létrehozása** párbeszédpanel **általános** lapra, adja meg egy egyedi nevet a hello kiszolgáló, például **Azure PostgreSQL Server**.
![pgAdmin tool - create - server](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.  A hello **- kiszolgáló létrehozása** párbeszédpanelen **kapcsolat** lapon megadott hello beállításokkal, kattintson **mentése**.
   ![pgAdmin - Create - Server](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    pgAdmin-paraméter |Ajánlott érték|Leírás
    ---|---|---
    Gazdagépnév/-cím | *kiszolgáló neve* | Adja meg a hello kiszolgálónév hello Azure adatbázis PostgreSQL a korábban létrehozott használt. Az itt látható példakiszolgáló a mypgserver-20170401.postgres.database.azure.com. Hello teljesen minősített tartománynevét használja (\*. postgres.database.azure.com) hello példában látható módon. Kövesse hello hello előző szakasz tooget hello kapcsolati adatokat, ha nem emlékszik a kiszolgáló nevét. 
    Port | **5432** | Mindig használjon port 5432 PostgreSQL tooAzure adatbázishoz kapcsolódáskor.  
    Karbantartási adatbázis | **postgres** | Hello alapértelmezett rendszer által létrehozott adatbázis név használata *postgres*.
    Felhasználónév | *kiszolgáló-rendszergazdai bejelentkezési név* | Írja be a hello server admin bejelentkezési felhasználónevének megadni, ha az Azure-adatbázis hello PostgreSQL a korábban létrehozott. Ha nem emlékszik hello felhasználónév, kövesse a hello előző szakasz tooget hello kapcsolatadatok hello lépéseit. hello formátuma  *username@servername* .
    Jelszó | *az Ön rendszergazdai jelszava* |  hello jelszó úgy döntött, hogy a gyors üzembe helyezés során korábban küldje el hello kiszolgáló létrehozásakor.
    Szerepkör | *hagyja üresen* | Nem kell ezen a ponton tooprovide szerepkör neve. Hello mezőt hagyja üresen.
    SSL-mód | Kötelező | Alapértelmezés szerint a rendszer minden Azure PostgreSQL-kiszolgálót az SSL-kényszerítéssel bekapcsolva hoz létre. tooturn ki SSL kényszerítése, a részletek megtekintéséhez [kényszerítése SSL](./concepts-ssl-connection-security.md).
    
5.  Kattintson a **Save** (Mentés) gombra.
6.  Hello böngésző bal oldali ablaktáblán bontsa ki a hello **kiszolgálók** csomópont. Válassza ki például a kiszolgáló **Azure PostgreSQL Server** tooconnect tooit kattintson.
7. Bontsa ki hello kiszolgáló csomópontját, és végül **adatbázisok** rajta. hello lista tartalmaznia kell a meglévő *postgres* adatbázis, és minden új felhasználói adatbázis, például a *mypgsqldb*, hello előző szakaszban létrehozott. Vegye figyelembe, hogy a PostgreSQL-hez készült Azure-adatbázis segítségével kiszolgálónként több adatbázist is létrehozhat.
8. Kattintson a jobb gombbal a **adatbázisok**, válassza ki a hello **létrehozása** menü **adatbázis**.
9.  Írja be az adatbázis nevét, az Ön által választott hello **adatbázis** mezőjét, többek között *mypgsqldb* hello példában látható módon. 
10. Jelölje be hello **tulajdonos** hello adatbázis hello legördülő listából. Válassza ki a kiszolgáló-rendszergazda bejelentkezési nevét (a példánkban ez a *mylogin*).
10. Kattintson a **mentése** toocreate új, üres adatbázist.
11. A hello **böngésző** ablaktáblában tekintse meg a kiszolgálónév alatti adatbázisok listája hello létrehozott hello adatbázis.
 ![pgAdmin - Create - Database](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Az erőforrások eltávolítása
Hello gyors üzembe helyezés létrehozott hello erőforrások karbantartása vagy törlésével hello [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md), amely erőforrásokat is magában foglalja az összes hello hello erőforráscsoportban vagy egy kiszolgáló-erőforrás hello törlésével, ha azt szeretné tookeep hello egyéb erőforrások változatlanok maradnak.

> [!TIP]
> Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épül. Ha toocontinue, a következő toowork quickstarts, így nem karbantartáshoz hello a gyors üzembe helyezés a létrejött erőforrásokat. Ha nem tervezi toocontinue, használja a következő lépéseket toodelete erőforrások hozta létre a hello Azure-portálon található gyors üzembe helyezési hello.

toodelete hello teljes erőforráscsoport található, újonnan létrehozott hello server beleértve:
1.  Keresse meg az erőforráscsoport hello Azure-portálon. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** majd például a fenti példában az erőforráscsoport neve hello **myresourcegroup**.
2.  Az erőforráscsoport oldalán kattintson a **Törlés** parancsra. Az erőforráscsoport, például a fenti példában majd típusának hello neve **myresourcegroup**, a hello szöveg mezőben tooconfirm törlésre, és kattintson **törlése**.

Vagy ehelyett toodelete hello az újonnan létrehozott kiszolgálón:
1.  Ha nincs megnyitva, keresse meg a kiszolgáló hello Azure-portálon. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás**, majd keresse meg a létrehozott hello kiszolgáló.
2.  A hello **áttekintése** hello kattintson **törlése** hello felső ablaktábla gombjára.
![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló törlése](./media/quickstart-create-database-portal/12-delete.png)
3.  Erősítse meg a kiszolgálónév hello toodelete szeretne, és hello tartozó adatbázisok érintett megjelenítése. Adja meg a kiszolgáló nevét hello szövegmezőben, például a fenti példában **mypgserver-20170401**, és kattintson a **törlése**.

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./howto-migrate-using-export-and-import.md)
