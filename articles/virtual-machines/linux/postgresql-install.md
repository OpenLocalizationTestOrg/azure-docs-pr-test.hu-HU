---
title: "a Linux virtuális gép PostgreSQL mentése aaaSet |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és PostgreSQL konfigurálása a Linux virtuális gépek Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: 40209647924dffce11500705eb2d9f41c14df6ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-postgresql-on-azure"></a>A PostgreSQL telepítése és konfigurálása Azure-ban
PostgreSQL egy speciális nyílt forráskódú adatbázis hasonló tooOracle és DB2. Ez magában foglalja a vállalati szolgáltatások, mint a teljes ACID megfelelőségi megbízható tranzakciós feldolgozást és több verzió egyidejűség-vezérlési. Például az ANSI SQL és az SQL/MED (beleértve a külső adatokat burkolók Oracle, MySQL, MongoDB és sok más) szabványokat is támogatja. Széles körben bővíthető támogatja a több mint 12 eljárási nyelveket, GIN és GiST indexek, térbeli adatokat, és több NoSQL-hasonló szolgáltatásokat JSON vagy kulcs-érték-alapú alkalmazások is.

Ebből a cikkből megtudhatja, hogyan tooinstall és PostgreSQL konfigurálása a Linux operációs rendszert futtató Azure virtuális géphez.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>PostgreSQL telepítése
> [!NOTE]
> Már rendelkeznie kell egy Azure virtuális géphez, Linuxot futtató rendelés toocomplete Ez az oktatóanyag. toocreate, és állítsa be a Linux virtuális gépek, a folytatás előtt tekintse meg a [Azure Linux virtuális gép oktatóanyag](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

Ebben az esetben a portot 1999 használni hello PostgreSQL-port.  

Csatlakozás toohello PuTTY segítségével létrehozott Linux virtuális Géphez. Ha hello első alkalommal használja az Azure Linux virtuális gép, lásd: [hogyan tooUse Azure Linux SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hogyan toouse PuTTY toolearn tooconnect tooa Linux virtuális gép.

1. Futtassa a következő parancs tooswitch toohello gyökér (rendszergazda) hello:
   
        # sudo su -
2. Néhány terjesztéseket táblává PostgreSQL telepítése előtt telepítenie kell. A következő felsorolásban szereplő distro kereséséhez és hello megfelelő parancs:
   
   * Red Hat alap Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian alap Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. Töltse le a PostgreSQL hello legfelső szintű könyvtárba, és ezután bontsa ki a hello csomag:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    a fenti hello csak példaként szolgál. Hello található részletes töltse le a címet hello [/pub/forrás / Index](https://ftp.postgresql.org/pub/source/).
4. toostart hello build, futtassa az alábbi parancsokat:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Ha azt szeretné, toobuild minden, ami építhetők, többek között a hello dokumentáció (HTML- és man lapok) és a kiegészítő modulok (contrib), futtassa a hello inkább a következő parancsot:
   
        # gmake install-world
   
    A következő megerősítő üzenetet hello kell megjelennie:
   
        PostgreSQL, contrib, and documentation successfully made. Ready tooinstall.

## <a name="configure-postgresql"></a>PostgreSQL konfigurálása
1. (Választható) Hozzon létre egy szimbolikus hivatkozást tooshorten hello PostgreSQL-hivatkozás toonot hello verziószámát tartalmazza:
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. Hozzon létre egy könyvtárat hello adatbázis:
   
        # mkdir -p /opt/pgsql_data
3. Nem legfelső szintű felhasználó létrehozásához és módosításához, a felhasználó. Váltson a toothis új felhasználó (nevű *postgres* ebben a példában):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Biztonsági okokból PostgreSQL használja a nem gyökér szintű felhasználó tooinitialize, elindításához, vagy állítsa le a hello adatbázis.
   > 
   > 
4. Hello szerkesztése *bash_profile* fájl alábbi hello parancsok beírásával. Ezek a sorok kerülnek hello toohello végét *bash_profile* fájlt:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Hello végrehajtása *bash_profile* fájlt:
   
        $ source .bash_profile
6. A telepítés ellenőrzése hello a következő parancs használatával:
   
        $ which psql
   
    Ha a telepítés sikeres, a következő válasz hello jelenik meg:
   
        /opt/pgsql/bin/psql
7. Hello PostgreSQL verziója is ellenőrizheti:
   
        $ psql -V
8. Hello adatbázis inicializálása:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    A következő kimeneti hello kell megjelennie:

![Kép](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL beállítása
<!--    [postgres@ test ~]$ exit -->

Futtassa a következő parancsok hello:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Hello /etc/init.d/postgresql fájl két változók módosításához. hello előtag PostgreSQL toohello telepítési elérési útjának értéke: **/opt/pgsql**. PGDATA toohello adatokat tároló elérési útja PostgreSQL van beállítva: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Kép](./media/postgresql-install/no2.png)

Hello fájl toomake módosítsa azt végrehajtható:

    # chmod +x /etc/init.d/postgresql

Indítsa el a PostgreSQL:

    # /etc/init.d/postgresql start

Annak ellenőrzése, hogy PostgreSQL hello végpontja meg:

    # netstat -tunlp|grep 1999

A következő kimeneti hello kell megjelennie:

![Kép](./media/postgresql-install/no3.png)

## <a name="connect-toohello-postgres-database"></a>Csatlakozás toohello Postgres adatbázis
Még egyszer kapcsolóhoz toohello postgres felhasználó:

    # su - postgres

Postgres-adatbázis létrehozása:

    $ createdb events

Csatlakoztassa az újonnan létrehozott toohello események adatbázis:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Hozzon létre és Postgres tábla törlése
Most, hogy toohello adatbázis csatlakozott, a táblák azt is létrehozhat.

Például hozzon létre egy új példa Postgres tábla hello a következő parancs használatával:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Most már meg van adva hello négy oszlop táblázatot a következő oszlopneveket és korlátozások:

1. "name" oszlop már csak korlátozott által hello VARCHAR parancs toobe 20 hello karakterből állnak.
2. hello "étele" oszlop, amely be fogja hozni az egyes hello étele elem jelzi. VARCHAR korlátozza a szöveg toobe alatt 30 karakternél.
3. hello "megerősítve" oszlop rekordok e hello személy amelyeknél reagált a meghívásra toohello piknik. hello elfogadható értékek a következők: "Y", "N".
4. hello "date" oszlopban látható, ha azok-höz készült hello esemény. Postgres megköveteli, hogy a dátum, éééé-hh-nn. írható

Ha a tábla létrehozása sikeresen befejeződött hello következő kell megjelennie:

![Kép](./media/postgresql-install/no4.png)

Hello táblaszerkezet hello a következő parancs használatával is ellenőrizheti:

![Kép](./media/postgresql-install/no5.png)

### <a name="add-data-tooa-table"></a>Tooa adattábla hozzáadása
Információk először beszúrni egy sort:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

A kimenetnek kell megjelennie:

![Kép](./media/postgresql-install/no6.png)

Néhány további személyek toohello tábla is hozzáadhat. Néhány lehetőség, vagy létrehozhat saját:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Táblák megjelenítése
A következő parancs tooshow tábla hello használata:

    select * from potluck;

hello kimenete:

![Kép](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>A tábla adatainak törlése
A következő parancs toodelete adatok a táblázatban hello használata:

    delete from potluck where name=’John’;

Ez törli az összes hello adatokat hello "John" sort a. hello kimenete:

![Kép](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>A tábla adatainak frissítése
A következő parancs tooupdate adatok a táblázatban hello használata. Ez egy Sandy megerősítette, hogy ő lesz ott, túl nem fogja módosítani a saját RSVP "N", "Y":

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>További információ a PostgreSQL beolvasása
Most, hogy egy Azure Linux virtuális gép PostgreSQL hello telepítése befejeződött, élvezheti használja azt az Azure-ban. toolearn PostgreSQL, kapcsolatos további információkért látogasson el a hello [PostgreSQL webhely](http://www.postgresql.org/).

