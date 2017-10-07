---
title: "SQL-összekötő lépés által aaaGeneric. lépés |} Microsoft Docs"
description: "Ez a cikk van érdekében hello általános SQL-összekötő részletes használatával egy egyszerű HR rendszeren keresztül."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b1b5f89ab588de6f92f173a7bc00f97180067669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-step-by-step"></a><span data-ttu-id="133ac-103">Általános SQL-összekötő – részletes útmutató</span><span class="sxs-lookup"><span data-stu-id="133ac-103">Generic SQL Connector step-by-step</span></span>
<span data-ttu-id="133ac-104">Ez a témakör részletesen ismerteti az.</span><span class="sxs-lookup"><span data-stu-id="133ac-104">This topic is a step-by-step guide.</span></span> <span data-ttu-id="133ac-105">Egy egyszerű példa HR adatbázist hoz létre, és használja azt az egyes felhasználók és a csoporttagságuk importálásához.</span><span class="sxs-lookup"><span data-stu-id="133ac-105">It creates a simple sample HR database and use it for importing some users and their group membership.</span></span>

## <a name="prepare-hello-sample-database"></a><span data-ttu-id="133ac-106">Hello mintaadatbázis előkészítése</span><span class="sxs-lookup"><span data-stu-id="133ac-106">Prepare hello sample database</span></span>
<span data-ttu-id="133ac-107">SQL Server rendszert futtató kiszolgálón, futtassa a hello SQL-parancsfájlt található [függelék](#appendix-a). Ezt a parancsfájlt a mintaadatbázis hello nevű GSQLDEMO hoz létre.</span><span class="sxs-lookup"><span data-stu-id="133ac-107">On a server running SQL Server, run hello SQL script found in [Appendix A](#appendix-a). This script creates a sample database with hello name GSQLDEMO.</span></span> <span data-ttu-id="133ac-108">hello hálózatiobjektum-modellje hello létrehozott adatbázis tűnik, hogy a kép:</span><span class="sxs-lookup"><span data-stu-id="133ac-108">hello object model for hello created database looks like this picture:</span></span>  
<span data-ttu-id="133ac-109">![Hálózatiobjektum-modellje](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-109">![Object Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span></span>

<span data-ttu-id="133ac-110">A felhasználó azt szeretné, hogy toouse tooconnect toohello adatbázis is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="133ac-110">Also create a user you want toouse tooconnect toohello database.</span></span> <span data-ttu-id="133ac-111">Ebben a bemutatóban hello felhasználói FABRIKAM\SQLUser nevezik, és hello tartományban található.</span><span class="sxs-lookup"><span data-stu-id="133ac-111">In this walkthrough, hello user is called FABRIKAM\SQLUser and located in hello domain.</span></span>

## <a name="create-hello-odbc-connection-file"></a><span data-ttu-id="133ac-112">Hello ODBC kapcsolati fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="133ac-112">Create hello ODBC connection file</span></span>
<span data-ttu-id="133ac-113">Általános SQL-összekötő hello ODBC tooconnect toohello távoli kiszolgálót használ.</span><span class="sxs-lookup"><span data-stu-id="133ac-113">hello Generic SQL Connector is using ODBC tooconnect toohello remote server.</span></span> <span data-ttu-id="133ac-114">Először kell toocreate hello ODBC kapcsolati információkat tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="133ac-114">First we need toocreate a file with hello ODBC connection information.</span></span>

1. <span data-ttu-id="133ac-115">Indítsa el a kiszolgálón a hello ODBC segédprogramja:</span><span class="sxs-lookup"><span data-stu-id="133ac-115">Start hello ODBC management utility on your server:</span></span>  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. <span data-ttu-id="133ac-117">Jelölje be hello lapon **fájl DSN**.</span><span class="sxs-lookup"><span data-stu-id="133ac-117">Select hello tab **File DSN**.</span></span> <span data-ttu-id="133ac-118">Kattintson a **hozzáadása...** .</span><span class="sxs-lookup"><span data-stu-id="133ac-118">Click **Add...**.</span></span>  
   <span data-ttu-id="133ac-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span></span>
3. <span data-ttu-id="133ac-120">hello out-of-box illesztőprogram works azokat, amelyek válassza ki azt, és kattintson **következő >**.</span><span class="sxs-lookup"><span data-stu-id="133ac-120">hello out-of-box driver works fine, so select it and click **Next>**.</span></span>  
   <span data-ttu-id="133ac-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span></span>
4. <span data-ttu-id="133ac-122">Nevezze el hello fájl, például a **GenericSQL**.</span><span class="sxs-lookup"><span data-stu-id="133ac-122">Give hello file a name, such as **GenericSQL**.</span></span>  
   <span data-ttu-id="133ac-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span></span>
5. <span data-ttu-id="133ac-124">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="133ac-124">Click **Finish**.</span></span>  
   <span data-ttu-id="133ac-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span></span>
6. <span data-ttu-id="133ac-126">Idő tooconfigure hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="133ac-126">Time tooconfigure hello connection.</span></span> <span data-ttu-id="133ac-127">Hello adatforrás adjon meg helyes leírást, és adja meg az SQL Server rendszert futtató hello server hello nevét.</span><span class="sxs-lookup"><span data-stu-id="133ac-127">Give hello data source a good description and provide hello name of hello server running SQL Server.</span></span>  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. <span data-ttu-id="133ac-129">Adja meg, hogyan tooauthenticate SQL.</span><span class="sxs-lookup"><span data-stu-id="133ac-129">Select how tooauthenticate with SQL.</span></span> <span data-ttu-id="133ac-130">Ebben az esetben azt a Windows-hitelesítés használatára.</span><span class="sxs-lookup"><span data-stu-id="133ac-130">In this case, we use Windows Authentication.</span></span>  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. <span data-ttu-id="133ac-132">Adjon meg hello hello mintaadatbázis, **GSQLDEMO**.</span><span class="sxs-lookup"><span data-stu-id="133ac-132">Provide hello name of hello sample database, **GSQLDEMO**.</span></span>  
   <span data-ttu-id="133ac-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span></span>
9. <span data-ttu-id="133ac-134">Minden alapértelmezett ezen a képernyőn megtartása.</span><span class="sxs-lookup"><span data-stu-id="133ac-134">Keep everything default on this screen.</span></span> <span data-ttu-id="133ac-135">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="133ac-135">Click **Finish**.</span></span>  
   <span data-ttu-id="133ac-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span></span>
10. <span data-ttu-id="133ac-137">minden a várt módon működik, tooverify kattintson **tesztelése adatforrás**.</span><span class="sxs-lookup"><span data-stu-id="133ac-137">tooverify everything is working as expected, click **Test Data Source**.</span></span>  
    <span data-ttu-id="133ac-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span></span>
11. <span data-ttu-id="133ac-139">Ellenőrizze, hogy hello teszt sikeres.</span><span class="sxs-lookup"><span data-stu-id="133ac-139">Make sure hello test is successful.</span></span>  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. <span data-ttu-id="133ac-141">hello ODBC konfigurációs fájl most kell fájl DSN látható.</span><span class="sxs-lookup"><span data-stu-id="133ac-141">hello ODBC configuration file should now be visible in File DSN.</span></span>  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

<span data-ttu-id="133ac-143">Most már tudunk hello fájl igazolnia kell, és hello összekötő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="133ac-143">We now have hello file we need and can start creating hello Connector.</span></span>

## <a name="create-hello-generic-sql-connector"></a><span data-ttu-id="133ac-144">Hello általános SQL-összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="133ac-144">Create hello Generic SQL Connector</span></span>
1. <span data-ttu-id="133ac-145">Hello Synchronization Service Manager felhasználói felületén, válassza ki **összekötők** és **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="133ac-145">In hello Synchronization Service Manager UI, select **Connectors** and **Create**.</span></span> <span data-ttu-id="133ac-146">Válassza ki **általános SQL (Microsoft)** , és adjon neki könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="133ac-146">Select **Generic SQL (Microsoft)** and give it a descriptive name.</span></span>  
   <span data-ttu-id="133ac-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span></span>
2. <span data-ttu-id="133ac-148">Hello előző szakaszban létrehozott hello DSN fájl található, és töltse fel az toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="133ac-148">Find hello DSN file you created in hello previous section and upload it toohello server.</span></span> <span data-ttu-id="133ac-149">Adja meg a hello hitelesítő adatok tooconnect toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="133ac-149">Provide hello credentials tooconnect toohello database.</span></span>  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. <span data-ttu-id="133ac-151">Ebben a forgatókönyvben azt van így könnyen nekünk, és tegyük fel például, hogy nincsenek-e két objektumtípusok **felhasználói** és **csoport**.</span><span class="sxs-lookup"><span data-stu-id="133ac-151">In this walkthrough, we are making it easy for us and say that there are two object types, **User** and **Group**.</span></span>
   <span data-ttu-id="133ac-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span></span>
4. <span data-ttu-id="133ac-153">toofind hello attribútumok, azt szeretnénk, ha azok összekötő toodetect hello hello tábla részei megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="133ac-153">toofind hello attributes, we want hello Connector toodetect those attributes by looking at hello table itself.</span></span> <span data-ttu-id="133ac-154">Mivel a **felhasználók** egy fenntartott szó az SQL-ben, a szögletes szögletes zárójelbe [] tooprovide kell.</span><span class="sxs-lookup"><span data-stu-id="133ac-154">Since **Users** is a reserved word in SQL, we need tooprovide it in square brackets [ ].</span></span>  
   <span data-ttu-id="133ac-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span></span>
5. <span data-ttu-id="133ac-156">Idő toodefine hello horgonyattribútum és hello megkülönböztető név attribútum.</span><span class="sxs-lookup"><span data-stu-id="133ac-156">Time toodefine hello anchor attribute and hello DN attribute.</span></span> <span data-ttu-id="133ac-157">A **felhasználók**, két hello attribútumok felhasználónév és a EmployeeID hello kombinációja használjuk.</span><span class="sxs-lookup"><span data-stu-id="133ac-157">For **Users**, we use hello combination of hello two attributes username and EmployeeID.</span></span> <span data-ttu-id="133ac-158">A **csoport**, GroupName használjuk (nem fog reális valósághű, de ez a forgatókönyv működik).</span><span class="sxs-lookup"><span data-stu-id="133ac-158">For **group**, we use GroupName (not realistic in real-life, but for this walkthrough it works).</span></span>
   <span data-ttu-id="133ac-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span></span>
6. <span data-ttu-id="133ac-160">Nem minden attribútumtípust észlelhető az SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="133ac-160">Not all attribute types can be detected in a SQL database.</span></span> <span data-ttu-id="133ac-161">különösen nem hello hivatkozási attribútum típusa.</span><span class="sxs-lookup"><span data-stu-id="133ac-161">hello reference attribute type in particular cannot.</span></span> <span data-ttu-id="133ac-162">Hello csoport objektumtípus el kell toochange hello OwnerID és MemberID tooreference.</span><span class="sxs-lookup"><span data-stu-id="133ac-162">For hello group object type, we need toochange hello OwnerID and MemberID tooreference.</span></span>  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. <span data-ttu-id="133ac-164">hello attribútumok azt összes hivatkozás attribútumok hello előző lépésben ezen értékek a következők hivatkoznak hello objektumtípus beállítást választani.</span><span class="sxs-lookup"><span data-stu-id="133ac-164">hello attributes we selected as reference attributes in hello previous step require hello object type these values are a reference to.</span></span> <span data-ttu-id="133ac-165">Ebben az esetben hello felhasználói objektum típusa.</span><span class="sxs-lookup"><span data-stu-id="133ac-165">In our case, hello User object type.</span></span>  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. <span data-ttu-id="133ac-167">Hello globális paraméterek lapon jelölje be **vízjel** hello különbözeti stratégia szerint.</span><span class="sxs-lookup"><span data-stu-id="133ac-167">On hello Global Parameters page, select **Watermark** as hello delta strategy.</span></span> <span data-ttu-id="133ac-168">Hello dátum és idő formátumban is beírhat **éééé-hh-nn óó: pp:**.</span><span class="sxs-lookup"><span data-stu-id="133ac-168">Also type in hello date/time format **yyyy-MM-dd HH:mm:ss**.</span></span>
   <span data-ttu-id="133ac-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span></span>
9. <span data-ttu-id="133ac-170">A hello **konfigurálása partíciók és hierarchiák** lapon, válassza ki mindkét típusú objektumokat.</span><span class="sxs-lookup"><span data-stu-id="133ac-170">On hello **Configure Partitions and Hierarchies** page, select both object types.</span></span>
   <span data-ttu-id="133ac-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span></span>
10. <span data-ttu-id="133ac-172">A hello **típusú objektumokat válasszon** és **attribútumok kiválasztása**, jelölje be a típusú objektumokat és az összes attribútum.</span><span class="sxs-lookup"><span data-stu-id="133ac-172">On hello **Select Object Types** and **Select Attributes**, select both object types and all attributes.</span></span> <span data-ttu-id="133ac-173">A hello **horgonyok konfigurálása** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="133ac-173">On hello **Configure Anchors** page, click **Finish**.</span></span>

## <a name="create-run-profiles"></a><span data-ttu-id="133ac-174">Futtatási profilok létrehozása</span><span class="sxs-lookup"><span data-stu-id="133ac-174">Create Run Profiles</span></span>
1. <span data-ttu-id="133ac-175">Hello Synchronization Service Manager felhasználói felületén, válassza ki **összekötők**, és **Configure Run Profiles**.</span><span class="sxs-lookup"><span data-stu-id="133ac-175">In hello Synchronization Service Manager UI, select **Connectors**, and **Configure Run Profiles**.</span></span> <span data-ttu-id="133ac-176">Kattintson a **új profil**.</span><span class="sxs-lookup"><span data-stu-id="133ac-176">Click **New Profile**.</span></span> <span data-ttu-id="133ac-177">Először **teljes importálás**.</span><span class="sxs-lookup"><span data-stu-id="133ac-177">We start with **Full Import**.</span></span>  
   <span data-ttu-id="133ac-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span></span>
2. <span data-ttu-id="133ac-179">Hello típusának kiválasztása **teljes importálás (csak szakaszban)**.</span><span class="sxs-lookup"><span data-stu-id="133ac-179">Select hello type **Full Import (Stage Only)**.</span></span>  
   <span data-ttu-id="133ac-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span></span>
3. <span data-ttu-id="133ac-181">Válassza ki a hello partíció **objektum = felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="133ac-181">Select hello partition **OBJECT=User**.</span></span>  
   <span data-ttu-id="133ac-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span></span>
4. <span data-ttu-id="133ac-183">Válassza ki **tábla** és típus **[felhasználók]**.</span><span class="sxs-lookup"><span data-stu-id="133ac-183">Select **Table** and type **[USERS]**.</span></span> <span data-ttu-id="133ac-184">Görgessen lefelé toohello többértékű objektum típushoz című rész, és adja meg a kép a következő hello hasonlóan hello adatok.</span><span class="sxs-lookup"><span data-stu-id="133ac-184">Scroll down toohello multi-valued object type section and enter hello data as in hello following picture.</span></span> <span data-ttu-id="133ac-185">Válassza ki **Befejezés** toosave hello lépés.</span><span class="sxs-lookup"><span data-stu-id="133ac-185">Select **Finish** toosave hello step.</span></span>  
   <span data-ttu-id="133ac-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span></span>  
   <span data-ttu-id="133ac-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span></span>  
5. <span data-ttu-id="133ac-188">Válassza ki **új lépés**.</span><span class="sxs-lookup"><span data-stu-id="133ac-188">Select **New Step**.</span></span> <span data-ttu-id="133ac-189">Most, jelölje be **objektum = csoport**.</span><span class="sxs-lookup"><span data-stu-id="133ac-189">This time, select **OBJECT=Group**.</span></span> <span data-ttu-id="133ac-190">Hello utolsó lapon mint a következő képen hello hello konfigurációt használja.</span><span class="sxs-lookup"><span data-stu-id="133ac-190">On hello last page, use hello configuration as in hello following picture.</span></span> <span data-ttu-id="133ac-191">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="133ac-191">Click **Finish**.</span></span>  
   <span data-ttu-id="133ac-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span></span>  
   <span data-ttu-id="133ac-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span><span class="sxs-lookup"><span data-stu-id="133ac-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span></span>  
6. <span data-ttu-id="133ac-194">Nem kötelező: Ha kívánja, konfigurálhatja a további futtatási profilokat.</span><span class="sxs-lookup"><span data-stu-id="133ac-194">Optional: If you want to, you can configure additional run profiles.</span></span> <span data-ttu-id="133ac-195">Ez a forgatókönyv csak a teljes importálás hello használjuk.</span><span class="sxs-lookup"><span data-stu-id="133ac-195">For this walkthrough, only hello Full Import is used.</span></span>
7. <span data-ttu-id="133ac-196">Kattintson a **OK** módosítása toofinish futtatási profil szerepel.</span><span class="sxs-lookup"><span data-stu-id="133ac-196">Click **OK** toofinish changing run profiles.</span></span>

## <a name="add-some-test-data-and-test-hello-import"></a><span data-ttu-id="133ac-197">Adja hozzá az egyes tesztelési adatokat, valamint hello importálása</span><span class="sxs-lookup"><span data-stu-id="133ac-197">Add some test data and test hello import</span></span>
<span data-ttu-id="133ac-198">Töltse ki a mintaadatbázis néhány tesztadatot.</span><span class="sxs-lookup"><span data-stu-id="133ac-198">Fill out some test data in your sample database.</span></span> <span data-ttu-id="133ac-199">Amikor elkészült, válassza ki a **futtatása** és **teljes importálás**.</span><span class="sxs-lookup"><span data-stu-id="133ac-199">When you are ready, select **Run** and **Full import**.</span></span>

<span data-ttu-id="133ac-200">Ez a felhasználó két telefonszámot és egy csoport néhány tagjához.</span><span class="sxs-lookup"><span data-stu-id="133ac-200">Here is a user with two phone numbers and a group with some members.</span></span>  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a><span data-ttu-id="133ac-203">A függelék</span><span class="sxs-lookup"><span data-stu-id="133ac-203">Appendix A</span></span>
<span data-ttu-id="133ac-204">**SQL parancsfájl toocreate hello mintaadatbázis**</span><span class="sxs-lookup"><span data-stu-id="133ac-204">**SQL script toocreate hello sample database**</span></span>

```SQL
---Creating hello Database---------
Create Database GSQLDEMO
Go
-------Using hello Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
