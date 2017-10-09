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
# <a name="install-and-configure-postgresql-on-azure"></a><span data-ttu-id="726fd-103">A PostgreSQL telepítése és konfigurálása Azure-ban</span><span class="sxs-lookup"><span data-stu-id="726fd-103">Install and configure PostgreSQL on Azure</span></span>
<span data-ttu-id="726fd-104">PostgreSQL egy speciális nyílt forráskódú adatbázis hasonló tooOracle és DB2.</span><span class="sxs-lookup"><span data-stu-id="726fd-104">PostgreSQL is an advanced open-source database similar tooOracle and DB2.</span></span> <span data-ttu-id="726fd-105">Ez magában foglalja a vállalati szolgáltatások, mint a teljes ACID megfelelőségi megbízható tranzakciós feldolgozást és több verzió egyidejűség-vezérlési.</span><span class="sxs-lookup"><span data-stu-id="726fd-105">It includes enterprise-ready features such as full ACID compliance, reliable transactional processing, and multi-version concurrency control.</span></span> <span data-ttu-id="726fd-106">Például az ANSI SQL és az SQL/MED (beleértve a külső adatokat burkolók Oracle, MySQL, MongoDB és sok más) szabványokat is támogatja.</span><span class="sxs-lookup"><span data-stu-id="726fd-106">It also supports standards such as ANSI SQL and SQL/MED (including foreign data wrappers for Oracle, MySQL, MongoDB, and many others).</span></span> <span data-ttu-id="726fd-107">Széles körben bővíthető támogatja a több mint 12 eljárási nyelveket, GIN és GiST indexek, térbeli adatokat, és több NoSQL-hasonló szolgáltatásokat JSON vagy kulcs-érték-alapú alkalmazások is.</span><span class="sxs-lookup"><span data-stu-id="726fd-107">It is highly extensible with support for over 12 procedural languages, GIN and GiST indexes, spatial data support, and multiple NoSQL-like features for JSON or key-value-based applications.</span></span>

<span data-ttu-id="726fd-108">Ebből a cikkből megtudhatja, hogyan tooinstall és PostgreSQL konfigurálása a Linux operációs rendszert futtató Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="726fd-108">In this article, you will learn how tooinstall and configure PostgreSQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a><span data-ttu-id="726fd-109">PostgreSQL telepítése</span><span class="sxs-lookup"><span data-stu-id="726fd-109">Install PostgreSQL</span></span>
> [!NOTE]
> <span data-ttu-id="726fd-110">Már rendelkeznie kell egy Azure virtuális géphez, Linuxot futtató rendelés toocomplete Ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="726fd-110">You must already have an Azure virtual machine running Linux in order toocomplete this tutorial.</span></span> <span data-ttu-id="726fd-111">toocreate, és állítsa be a Linux virtuális gépek, a folytatás előtt tekintse meg a [Azure Linux virtuális gép oktatóanyag](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="726fd-111">toocreate and set up a Linux VM before proceeding, see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

<span data-ttu-id="726fd-112">Ebben az esetben a portot 1999 használni hello PostgreSQL-port.</span><span class="sxs-lookup"><span data-stu-id="726fd-112">In this case, use port 1999 as hello PostgreSQL port.</span></span>  

<span data-ttu-id="726fd-113">Csatlakozás toohello PuTTY segítségével létrehozott Linux virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="726fd-113">Connect toohello Linux VM you created via PuTTY.</span></span> <span data-ttu-id="726fd-114">Ha hello első alkalommal használja az Azure Linux virtuális gép, lásd: [hogyan tooUse Azure Linux SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hogyan toouse PuTTY toolearn tooconnect tooa Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="726fd-114">If this is hello first time you're using an Azure Linux VM, see [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toolearn how toouse PuTTY tooconnect tooa Linux VM.</span></span>

1. <span data-ttu-id="726fd-115">Futtassa a következő parancs tooswitch toohello gyökér (rendszergazda) hello:</span><span class="sxs-lookup"><span data-stu-id="726fd-115">Run hello following command tooswitch toohello root (admin):</span></span>
   
        # sudo su -
2. <span data-ttu-id="726fd-116">Néhány terjesztéseket táblává PostgreSQL telepítése előtt telepítenie kell.</span><span class="sxs-lookup"><span data-stu-id="726fd-116">Some distributions have dependencies that you must install before installing PostgreSQL.</span></span> <span data-ttu-id="726fd-117">A következő felsorolásban szereplő distro kereséséhez és hello megfelelő parancs:</span><span class="sxs-lookup"><span data-stu-id="726fd-117">Check for your distro in this list and run hello appropriate command:</span></span>
   
   * <span data-ttu-id="726fd-118">Red Hat alap Linux:</span><span class="sxs-lookup"><span data-stu-id="726fd-118">Red Hat base Linux:</span></span>
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="726fd-119">Debian alap Linux:</span><span class="sxs-lookup"><span data-stu-id="726fd-119">Debian base Linux:</span></span>
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="726fd-120">SUSE Linux:</span><span class="sxs-lookup"><span data-stu-id="726fd-120">SUSE Linux:</span></span>
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. <span data-ttu-id="726fd-121">Töltse le a PostgreSQL hello legfelső szintű könyvtárba, és ezután bontsa ki a hello csomag:</span><span class="sxs-lookup"><span data-stu-id="726fd-121">Download PostgreSQL into hello root directory, and then unzip hello package:</span></span>
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    <span data-ttu-id="726fd-122">a fenti hello csak példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="726fd-122">hello above is an example.</span></span> <span data-ttu-id="726fd-123">Hello található részletes töltse le a címet hello [/pub/forrás / Index](https://ftp.postgresql.org/pub/source/).</span><span class="sxs-lookup"><span data-stu-id="726fd-123">You can find hello more detailed download address in hello [Index of /pub/source/](https://ftp.postgresql.org/pub/source/).</span></span>
4. <span data-ttu-id="726fd-124">toostart hello build, futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="726fd-124">toostart hello build, run these commands:</span></span>
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. <span data-ttu-id="726fd-125">Ha azt szeretné, toobuild minden, ami építhetők, többek között a hello dokumentáció (HTML- és man lapok) és a kiegészítő modulok (contrib), futtassa a hello inkább a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="726fd-125">If  you want toobuild everything that can be built, including hello documentation (HTML and man pages) and additional modules (contrib), run hello following command instead:</span></span>
   
        # gmake install-world
   
    <span data-ttu-id="726fd-126">A következő megerősítő üzenetet hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="726fd-126">You should receive hello following confirmation message:</span></span>
   
        PostgreSQL, contrib, and documentation successfully made. Ready tooinstall.

## <a name="configure-postgresql"></a><span data-ttu-id="726fd-127">PostgreSQL konfigurálása</span><span class="sxs-lookup"><span data-stu-id="726fd-127">Configure PostgreSQL</span></span>
1. <span data-ttu-id="726fd-128">(Választható) Hozzon létre egy szimbolikus hivatkozást tooshorten hello PostgreSQL-hivatkozás toonot hello verziószámát tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="726fd-128">(Optional) Create a symbolic link tooshorten hello PostgreSQL reference toonot include hello version number:</span></span>
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. <span data-ttu-id="726fd-129">Hozzon létre egy könyvtárat hello adatbázis:</span><span class="sxs-lookup"><span data-stu-id="726fd-129">Create a directory for hello database:</span></span>
   
        # mkdir -p /opt/pgsql_data
3. <span data-ttu-id="726fd-130">Nem legfelső szintű felhasználó létrehozásához és módosításához, a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="726fd-130">Create a non-root user and modify that user’s profile.</span></span> <span data-ttu-id="726fd-131">Váltson a toothis új felhasználó (nevű *postgres* ebben a példában):</span><span class="sxs-lookup"><span data-stu-id="726fd-131">Then, switch toothis new user (called *postgres* in our example):</span></span>
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > <span data-ttu-id="726fd-132">Biztonsági okokból PostgreSQL használja a nem gyökér szintű felhasználó tooinitialize, elindításához, vagy állítsa le a hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="726fd-132">For security reasons, PostgreSQL uses a non-root user tooinitialize, start, or shut down hello database.</span></span>
   > 
   > 
4. <span data-ttu-id="726fd-133">Hello szerkesztése *bash_profile* fájl alábbi hello parancsok beírásával.</span><span class="sxs-lookup"><span data-stu-id="726fd-133">Edit hello *bash_profile* file by entering hello commands below.</span></span> <span data-ttu-id="726fd-134">Ezek a sorok kerülnek hello toohello végét *bash_profile* fájlt:</span><span class="sxs-lookup"><span data-stu-id="726fd-134">These lines will be added toohello end of hello *bash_profile* file:</span></span>
   
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
5. <span data-ttu-id="726fd-135">Hello végrehajtása *bash_profile* fájlt:</span><span class="sxs-lookup"><span data-stu-id="726fd-135">Execute hello *bash_profile* file:</span></span>
   
        $ source .bash_profile
6. <span data-ttu-id="726fd-136">A telepítés ellenőrzése hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="726fd-136">Validate your installation by using hello following command:</span></span>
   
        $ which psql
   
    <span data-ttu-id="726fd-137">Ha a telepítés sikeres, a következő válasz hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="726fd-137">If your installation is successful, you will see hello following response:</span></span>
   
        /opt/pgsql/bin/psql
7. <span data-ttu-id="726fd-138">Hello PostgreSQL verziója is ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="726fd-138">You can also check hello PostgreSQL version:</span></span>
   
        $ psql -V
8. <span data-ttu-id="726fd-139">Hello adatbázis inicializálása:</span><span class="sxs-lookup"><span data-stu-id="726fd-139">Initialize hello database:</span></span>
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    <span data-ttu-id="726fd-140">A következő kimeneti hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="726fd-140">You should receive hello following output:</span></span>

![Kép](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a><span data-ttu-id="726fd-142">PostgreSQL beállítása</span><span class="sxs-lookup"><span data-stu-id="726fd-142">Set up PostgreSQL</span></span>
<!--    [postgres@ test ~]$ exit -->

<span data-ttu-id="726fd-143">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="726fd-143">Run hello following commands:</span></span>

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

<span data-ttu-id="726fd-144">Hello /etc/init.d/postgresql fájl két változók módosításához.</span><span class="sxs-lookup"><span data-stu-id="726fd-144">Modify two variables in hello /etc/init.d/postgresql file.</span></span> <span data-ttu-id="726fd-145">hello előtag PostgreSQL toohello telepítési elérési útjának értéke: **/opt/pgsql**.</span><span class="sxs-lookup"><span data-stu-id="726fd-145">hello prefix is set toohello installation path of PostgreSQL: **/opt/pgsql**.</span></span> <span data-ttu-id="726fd-146">PGDATA toohello adatokat tároló elérési útja PostgreSQL van beállítva: **/opt/pgsql_data**.</span><span class="sxs-lookup"><span data-stu-id="726fd-146">PGDATA is set toohello data storage path of PostgreSQL: **/opt/pgsql_data**.</span></span>

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Kép](./media/postgresql-install/no2.png)

<span data-ttu-id="726fd-148">Hello fájl toomake módosítsa azt végrehajtható:</span><span class="sxs-lookup"><span data-stu-id="726fd-148">Change hello file toomake it executable:</span></span>

    # chmod +x /etc/init.d/postgresql

<span data-ttu-id="726fd-149">Indítsa el a PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="726fd-149">Start PostgreSQL:</span></span>

    # /etc/init.d/postgresql start

<span data-ttu-id="726fd-150">Annak ellenőrzése, hogy PostgreSQL hello végpontja meg:</span><span class="sxs-lookup"><span data-stu-id="726fd-150">Check if hello endpoint of PostgreSQL is on:</span></span>

    # netstat -tunlp|grep 1999

<span data-ttu-id="726fd-151">A következő kimeneti hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="726fd-151">You should see hello following output:</span></span>

![Kép](./media/postgresql-install/no3.png)

## <a name="connect-toohello-postgres-database"></a><span data-ttu-id="726fd-153">Csatlakozás toohello Postgres adatbázis</span><span class="sxs-lookup"><span data-stu-id="726fd-153">Connect toohello Postgres database</span></span>
<span data-ttu-id="726fd-154">Még egyszer kapcsolóhoz toohello postgres felhasználó:</span><span class="sxs-lookup"><span data-stu-id="726fd-154">Switch toohello postgres user once again:</span></span>

    # su - postgres

<span data-ttu-id="726fd-155">Postgres-adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="726fd-155">Create a Postgres database:</span></span>

    $ createdb events

<span data-ttu-id="726fd-156">Csatlakoztassa az újonnan létrehozott toohello események adatbázis:</span><span class="sxs-lookup"><span data-stu-id="726fd-156">Connect toohello events database that you just created:</span></span>

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a><span data-ttu-id="726fd-157">Hozzon létre és Postgres tábla törlése</span><span class="sxs-lookup"><span data-stu-id="726fd-157">Create and delete a Postgres table</span></span>
<span data-ttu-id="726fd-158">Most, hogy toohello adatbázis csatlakozott, a táblák azt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="726fd-158">Now that you have connected toohello database, you can create tables in it.</span></span>

<span data-ttu-id="726fd-159">Például hozzon létre egy új példa Postgres tábla hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="726fd-159">For example, create a new example Postgres table by using hello following command:</span></span>

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

<span data-ttu-id="726fd-160">Most már meg van adva hello négy oszlop táblázatot a következő oszlopneveket és korlátozások:</span><span class="sxs-lookup"><span data-stu-id="726fd-160">You have now set up a four-column table with hello following column names and restrictions:</span></span>

1. <span data-ttu-id="726fd-161">"name" oszlop már csak korlátozott által hello VARCHAR parancs toobe 20 hello karakterből állnak.</span><span class="sxs-lookup"><span data-stu-id="726fd-161">hello “name” column has been limited by hello VARCHAR command toobe under 20 characters long.</span></span>
2. <span data-ttu-id="726fd-162">hello "étele" oszlop, amely be fogja hozni az egyes hello étele elem jelzi.</span><span class="sxs-lookup"><span data-stu-id="726fd-162">hello “food” column indicates hello food item that each person will bring.</span></span> <span data-ttu-id="726fd-163">VARCHAR korlátozza a szöveg toobe alatt 30 karakternél.</span><span class="sxs-lookup"><span data-stu-id="726fd-163">VARCHAR limits this text toobe under 30 characters.</span></span>
3. <span data-ttu-id="726fd-164">hello "megerősítve" oszlop rekordok e hello személy amelyeknél reagált a meghívásra toohello piknik.</span><span class="sxs-lookup"><span data-stu-id="726fd-164">hello “confirmed” column records whether hello person has RSVP’d toohello potluck.</span></span> <span data-ttu-id="726fd-165">hello elfogadható értékek a következők: "Y", "N".</span><span class="sxs-lookup"><span data-stu-id="726fd-165">hello acceptable values are "Y" and "N".</span></span>
4. <span data-ttu-id="726fd-166">hello "date" oszlopban látható, ha azok-höz készült hello esemény.</span><span class="sxs-lookup"><span data-stu-id="726fd-166">hello “date” column shows when they signed up for hello event.</span></span> <span data-ttu-id="726fd-167">Postgres megköveteli, hogy a dátum, éééé-hh-nn. írható</span><span class="sxs-lookup"><span data-stu-id="726fd-167">Postgres requires that dates be written as yyyy-mm-dd.</span></span>

<span data-ttu-id="726fd-168">Ha a tábla létrehozása sikeresen befejeződött hello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="726fd-168">You should see hello following if your table has been successfully created:</span></span>

![Kép](./media/postgresql-install/no4.png)

<span data-ttu-id="726fd-170">Hello táblaszerkezet hello a következő parancs használatával is ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="726fd-170">You can also check hello table structure by using hello following command:</span></span>

![Kép](./media/postgresql-install/no5.png)

### <a name="add-data-tooa-table"></a><span data-ttu-id="726fd-172">Tooa adattábla hozzáadása</span><span class="sxs-lookup"><span data-stu-id="726fd-172">Add data tooa table</span></span>
<span data-ttu-id="726fd-173">Információk először beszúrni egy sort:</span><span class="sxs-lookup"><span data-stu-id="726fd-173">First, insert information into a row:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

<span data-ttu-id="726fd-174">A kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="726fd-174">You should see this output:</span></span>

![Kép](./media/postgresql-install/no6.png)

<span data-ttu-id="726fd-176">Néhány további személyek toohello tábla is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="726fd-176">You can add a couple more people toohello table as well.</span></span> <span data-ttu-id="726fd-177">Néhány lehetőség, vagy létrehozhat saját:</span><span class="sxs-lookup"><span data-stu-id="726fd-177">Here are some options, or you can create your own:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a><span data-ttu-id="726fd-178">Táblák megjelenítése</span><span class="sxs-lookup"><span data-stu-id="726fd-178">Show tables</span></span>
<span data-ttu-id="726fd-179">A következő parancs tooshow tábla hello használata:</span><span class="sxs-lookup"><span data-stu-id="726fd-179">Use hello following command tooshow a table:</span></span>

    select * from potluck;

<span data-ttu-id="726fd-180">hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="726fd-180">hello output is:</span></span>

![Kép](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a><span data-ttu-id="726fd-182">A tábla adatainak törlése</span><span class="sxs-lookup"><span data-stu-id="726fd-182">Delete data in a table</span></span>
<span data-ttu-id="726fd-183">A következő parancs toodelete adatok a táblázatban hello használata:</span><span class="sxs-lookup"><span data-stu-id="726fd-183">Use hello following command toodelete data in a table:</span></span>

    delete from potluck where name=’John’;

<span data-ttu-id="726fd-184">Ez törli az összes hello adatokat hello "John" sort a.</span><span class="sxs-lookup"><span data-stu-id="726fd-184">This deletes all hello information in hello "John" row.</span></span> <span data-ttu-id="726fd-185">hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="726fd-185">hello output is:</span></span>

![Kép](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a><span data-ttu-id="726fd-187">A tábla adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="726fd-187">Update data in a table</span></span>
<span data-ttu-id="726fd-188">A következő parancs tooupdate adatok a táblázatban hello használata.</span><span class="sxs-lookup"><span data-stu-id="726fd-188">Use hello following command tooupdate data in a table.</span></span> <span data-ttu-id="726fd-189">Ez egy Sandy megerősítette, hogy ő lesz ott, túl nem fogja módosítani a saját RSVP "N", "Y":</span><span class="sxs-lookup"><span data-stu-id="726fd-189">For this one, Sandy has confirmed that she is attending, so we will change her RSVP from "N" too"Y":</span></span>

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a><span data-ttu-id="726fd-190">További információ a PostgreSQL beolvasása</span><span class="sxs-lookup"><span data-stu-id="726fd-190">Get more information about PostgreSQL</span></span>
<span data-ttu-id="726fd-191">Most, hogy egy Azure Linux virtuális gép PostgreSQL hello telepítése befejeződött, élvezheti használja azt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="726fd-191">Now that you have completed hello installation of PostgreSQL in an Azure Linux VM, you can enjoy using it in Azure.</span></span> <span data-ttu-id="726fd-192">toolearn PostgreSQL, kapcsolatos további információkért látogasson el a hello [PostgreSQL webhely](http://www.postgresql.org/).</span><span class="sxs-lookup"><span data-stu-id="726fd-192">toolearn more about PostgreSQL, visit hello [PostgreSQL website](http://www.postgresql.org/).</span></span>

