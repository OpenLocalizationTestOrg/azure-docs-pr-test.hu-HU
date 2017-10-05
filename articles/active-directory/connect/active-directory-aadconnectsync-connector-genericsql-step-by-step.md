---
title: "Általános SQL összekötő lépés-lépésre |} Microsoft Docs"
description: "Ez a cikk van érdekében egy egyszerű HR rendszer részletes az általános SQL-összekötővel."
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
ms.openlocfilehash: 3fdc1b405b95180d031aa4ad45b406f7fc149d8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="generic-sql-connector-step-by-step"></a><span data-ttu-id="07a6a-103">Általános SQL-összekötő – részletes útmutató</span><span class="sxs-lookup"><span data-stu-id="07a6a-103">Generic SQL Connector step-by-step</span></span>
<span data-ttu-id="07a6a-104">Ez a témakör részletesen ismerteti az.</span><span class="sxs-lookup"><span data-stu-id="07a6a-104">This topic is a step-by-step guide.</span></span> <span data-ttu-id="07a6a-105">Egy egyszerű példa HR adatbázist hoz létre, és használja azt az egyes felhasználók és a csoporttagságuk importálásához.</span><span class="sxs-lookup"><span data-stu-id="07a6a-105">It creates a simple sample HR database and use it for importing some users and their group membership.</span></span>

## <a name="prepare-the-sample-database"></a><span data-ttu-id="07a6a-106">A minta-adatbázis előkészítése</span><span class="sxs-lookup"><span data-stu-id="07a6a-106">Prepare the sample database</span></span>
<span data-ttu-id="07a6a-107">A kiszolgálón futó SQL Server, az SQL-parancsfájl futtatása található [függelék](#appendix-a). Ezt a parancsfájlt a mintaadatbázis GSQLDEMO nevű hoz létre.</span><span class="sxs-lookup"><span data-stu-id="07a6a-107">On a server running SQL Server, run the SQL script found in [Appendix A](#appendix-a). This script creates a sample database with the name GSQLDEMO.</span></span> <span data-ttu-id="07a6a-108">Az adatbázis létrehozása a hálózatiobjektum-modellt a kép néz ki:</span><span class="sxs-lookup"><span data-stu-id="07a6a-108">The object model for the created database looks like this picture:</span></span>  
<span data-ttu-id="07a6a-109">![Hálózatiobjektum-modellje](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-109">![Object Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span></span>

<span data-ttu-id="07a6a-110">A felhasználó az adatbázishoz való kapcsolódáshoz használni kívánt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="07a6a-110">Also create a user you want to use to connect to the database.</span></span> <span data-ttu-id="07a6a-111">Ebben a bemutatóban a felhasználó FABRIKAM\SQLUser nevezik, és a tartományban található.</span><span class="sxs-lookup"><span data-stu-id="07a6a-111">In this walkthrough, the user is called FABRIKAM\SQLUser and located in the domain.</span></span>

## <a name="create-the-odbc-connection-file"></a><span data-ttu-id="07a6a-112">Az ODBC-kapcsolat fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="07a6a-112">Create the ODBC connection file</span></span>
<span data-ttu-id="07a6a-113">Az általános SQL-összekötő ODBC használja a távoli kiszolgálóhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="07a6a-113">The Generic SQL Connector is using ODBC to connect to the remote server.</span></span> <span data-ttu-id="07a6a-114">Először kell hozzon létre egy fájlt az ODBC-kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="07a6a-114">First we need to create a file with the ODBC connection information.</span></span>

1. <span data-ttu-id="07a6a-115">Indítsa el az ODBC segédprogramja a kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="07a6a-115">Start the ODBC management utility on your server:</span></span>  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. <span data-ttu-id="07a6a-117">Válassza ki a lapon **fájl DSN**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-117">Select the tab **File DSN**.</span></span> <span data-ttu-id="07a6a-118">Kattintson a **hozzáadása...** .</span><span class="sxs-lookup"><span data-stu-id="07a6a-118">Click **Add...**.</span></span>  
   <span data-ttu-id="07a6a-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span></span>
3. <span data-ttu-id="07a6a-120">A out-of-box illesztőprogram works azokat, így parancsára **következő >**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-120">The out-of-box driver works fine, so select it and click **Next>**.</span></span>  
   <span data-ttu-id="07a6a-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span></span>
4. <span data-ttu-id="07a6a-122">Nevezze el a fájlt, például a **GenericSQL**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-122">Give the file a name, such as **GenericSQL**.</span></span>  
   <span data-ttu-id="07a6a-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span></span>
5. <span data-ttu-id="07a6a-124">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="07a6a-124">Click **Finish**.</span></span>  
   <span data-ttu-id="07a6a-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span></span>
6. <span data-ttu-id="07a6a-126">A kapcsolat beállítása idő.</span><span class="sxs-lookup"><span data-stu-id="07a6a-126">Time to configure the connection.</span></span> <span data-ttu-id="07a6a-127">Az adatforrás adjon meg helyes leírást, és adja meg az SQL Server rendszert futtató kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="07a6a-127">Give the data source a good description and provide the name of the server running SQL Server.</span></span>  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. <span data-ttu-id="07a6a-129">Válassza ki, hogyan hitelesítheti az SQL.</span><span class="sxs-lookup"><span data-stu-id="07a6a-129">Select how to authenticate with SQL.</span></span> <span data-ttu-id="07a6a-130">Ebben az esetben azt a Windows-hitelesítés használatára.</span><span class="sxs-lookup"><span data-stu-id="07a6a-130">In this case, we use Windows Authentication.</span></span>  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. <span data-ttu-id="07a6a-132">Adja meg a mintaadatbázis nevét **GSQLDEMO**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-132">Provide the name of the sample database, **GSQLDEMO**.</span></span>  
   <span data-ttu-id="07a6a-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span></span>
9. <span data-ttu-id="07a6a-134">Minden alapértelmezett ezen a képernyőn megtartása.</span><span class="sxs-lookup"><span data-stu-id="07a6a-134">Keep everything default on this screen.</span></span> <span data-ttu-id="07a6a-135">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="07a6a-135">Click **Finish**.</span></span>  
   <span data-ttu-id="07a6a-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span></span>
10. <span data-ttu-id="07a6a-137">Ellenőrizze, hogy minden a várt módon működik, kattintson a **tesztelése adatforrás**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-137">To verify everything is working as expected, click **Test Data Source**.</span></span>  
    <span data-ttu-id="07a6a-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span></span>
11. <span data-ttu-id="07a6a-139">Győződjön meg arról, hogy a teszt sikeres.</span><span class="sxs-lookup"><span data-stu-id="07a6a-139">Make sure the test is successful.</span></span>  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. <span data-ttu-id="07a6a-141">Az ODBC-konfigurációs fájl most kell fájl DSN látható.</span><span class="sxs-lookup"><span data-stu-id="07a6a-141">The ODBC configuration file should now be visible in File DSN.</span></span>  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

<span data-ttu-id="07a6a-143">Most már tudunk a fájlt, igazolnia kell, és az összekötő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="07a6a-143">We now have the file we need and can start creating the Connector.</span></span>

## <a name="create-the-generic-sql-connector"></a><span data-ttu-id="07a6a-144">Az általános SQL-összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="07a6a-144">Create the Generic SQL Connector</span></span>
1. <span data-ttu-id="07a6a-145">A Synchronization Service Manager felhasználói felületén válassza ki a **összekötők** és **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-145">In the Synchronization Service Manager UI, select **Connectors** and **Create**.</span></span> <span data-ttu-id="07a6a-146">Válassza ki **általános SQL (Microsoft)** , és adjon neki könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="07a6a-146">Select **Generic SQL (Microsoft)** and give it a descriptive name.</span></span>  
   <span data-ttu-id="07a6a-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span></span>
2. <span data-ttu-id="07a6a-148">Az előző szakaszban létrehozott DSN fájl található, és töltse fel a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="07a6a-148">Find the DSN file you created in the previous section and upload it to the server.</span></span> <span data-ttu-id="07a6a-149">Adja meg az adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="07a6a-149">Provide the credentials to connect to the database.</span></span>  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. <span data-ttu-id="07a6a-151">Ebben a forgatókönyvben azt van így könnyen nekünk, és tegyük fel például, hogy nincsenek-e két objektumtípusok **felhasználói** és **csoport**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-151">In this walkthrough, we are making it easy for us and say that there are two object types, **User** and **Group**.</span></span>
   <span data-ttu-id="07a6a-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span></span>
4. <span data-ttu-id="07a6a-153">Az attribútumok kereséséhez azt szeretnénk, az összekötő azok észleléséhez a tábla részei megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="07a6a-153">To find the attributes, we want the Connector to detect those attributes by looking at the table itself.</span></span> <span data-ttu-id="07a6a-154">Mivel a **felhasználók** egy fenntartott szó SQL, igazolnia kell a szögletes zárójelbe [] biztosításához.</span><span class="sxs-lookup"><span data-stu-id="07a6a-154">Since **Users** is a reserved word in SQL, we need to provide it in square brackets [ ].</span></span>  
   <span data-ttu-id="07a6a-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span></span>
5. <span data-ttu-id="07a6a-156">Adja meg a horgonyattribútum és a megkülönböztető név attribútum idő.</span><span class="sxs-lookup"><span data-stu-id="07a6a-156">Time to define the anchor attribute and the DN attribute.</span></span> <span data-ttu-id="07a6a-157">A **felhasználók**, a két attribútum felhasználónév és a EmployeeID használjuk.</span><span class="sxs-lookup"><span data-stu-id="07a6a-157">For **Users**, we use the combination of the two attributes username and EmployeeID.</span></span> <span data-ttu-id="07a6a-158">A **csoport**, GroupName használjuk (nem fog reális valósághű, de ez a forgatókönyv működik).</span><span class="sxs-lookup"><span data-stu-id="07a6a-158">For **group**, we use GroupName (not realistic in real-life, but for this walkthrough it works).</span></span>
   <span data-ttu-id="07a6a-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span></span>
6. <span data-ttu-id="07a6a-160">Nem minden attribútumtípust észlelhető az SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="07a6a-160">Not all attribute types can be detected in a SQL database.</span></span> <span data-ttu-id="07a6a-161">A hivatkozási attribútum típusa különösen nem.</span><span class="sxs-lookup"><span data-stu-id="07a6a-161">The reference attribute type in particular cannot.</span></span> <span data-ttu-id="07a6a-162">A csoport objektumtípus módosítsa a OwnerID és a MemberID kell hivatkoznia kell.</span><span class="sxs-lookup"><span data-stu-id="07a6a-162">For the group object type, we need to change the OwnerID and MemberID to reference.</span></span>  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. <span data-ttu-id="07a6a-164">A jelenleg kijelölt összes hivatkozás attribútumok az előző lépésben beállítást az objektumtípus ezeket az értékeket attribútumokat mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="07a6a-164">The attributes we selected as reference attributes in the previous step require the object type these values are a reference to.</span></span> <span data-ttu-id="07a6a-165">Ebben az esetben a felhasználó objektum típusa.</span><span class="sxs-lookup"><span data-stu-id="07a6a-165">In our case, the User object type.</span></span>  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. <span data-ttu-id="07a6a-167">A globális paraméterek lapon válassza az **vízjel** , a különbözeti stratégia.</span><span class="sxs-lookup"><span data-stu-id="07a6a-167">On the Global Parameters page, select **Watermark** as the delta strategy.</span></span> <span data-ttu-id="07a6a-168">A dátum és idő formátumban is beírhat **éééé-hh-nn óó: pp:**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-168">Also type in the date/time format **yyyy-MM-dd HH:mm:ss**.</span></span>
   <span data-ttu-id="07a6a-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span></span>
9. <span data-ttu-id="07a6a-170">Az a **konfigurálása partíciók és hierarchiák** lapon, válassza ki mindkét típusú objektumokat.</span><span class="sxs-lookup"><span data-stu-id="07a6a-170">On the **Configure Partitions and Hierarchies** page, select both object types.</span></span>
   <span data-ttu-id="07a6a-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span></span>
10. <span data-ttu-id="07a6a-172">Az a **típusú objektumokat válasszon** és **attribútumok kiválasztása**, jelölje be a típusú objektumokat és az összes attribútum.</span><span class="sxs-lookup"><span data-stu-id="07a6a-172">On the **Select Object Types** and **Select Attributes**, select both object types and all attributes.</span></span> <span data-ttu-id="07a6a-173">Az a **horgonyok konfigurálása** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-173">On the **Configure Anchors** page, click **Finish**.</span></span>

## <a name="create-run-profiles"></a><span data-ttu-id="07a6a-174">Futtatási profilok létrehozása</span><span class="sxs-lookup"><span data-stu-id="07a6a-174">Create Run Profiles</span></span>
1. <span data-ttu-id="07a6a-175">A Synchronization Service Manager felhasználói felületén válassza ki a **összekötők**, és **Configure Run Profiles**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-175">In the Synchronization Service Manager UI, select **Connectors**, and **Configure Run Profiles**.</span></span> <span data-ttu-id="07a6a-176">Kattintson a **új profil**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-176">Click **New Profile**.</span></span> <span data-ttu-id="07a6a-177">Először **teljes importálás**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-177">We start with **Full Import**.</span></span>  
   <span data-ttu-id="07a6a-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span></span>
2. <span data-ttu-id="07a6a-179">Válassza ki a **teljes importálás (csak szakaszban)**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-179">Select the type **Full Import (Stage Only)**.</span></span>  
   <span data-ttu-id="07a6a-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span></span>
3. <span data-ttu-id="07a6a-181">Válassza ki a partíciót **objektum = felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-181">Select the partition **OBJECT=User**.</span></span>  
   <span data-ttu-id="07a6a-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span></span>
4. <span data-ttu-id="07a6a-183">Válassza ki **tábla** és típus **[felhasználók]**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-183">Select **Table** and type **[USERS]**.</span></span> <span data-ttu-id="07a6a-184">Görgessen le a többértékű objektum típushoz című rész, és írja be az adatokat, ahogy az alábbi képen.</span><span class="sxs-lookup"><span data-stu-id="07a6a-184">Scroll down to the multi-valued object type section and enter the data as in the following picture.</span></span> <span data-ttu-id="07a6a-185">Válassza ki **Befejezés** menteni a lépést.</span><span class="sxs-lookup"><span data-stu-id="07a6a-185">Select **Finish** to save the step.</span></span>  
   <span data-ttu-id="07a6a-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span></span>  
   <span data-ttu-id="07a6a-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span></span>  
5. <span data-ttu-id="07a6a-188">Válassza ki **új lépés**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-188">Select **New Step**.</span></span> <span data-ttu-id="07a6a-189">Most, jelölje be **objektum = csoport**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-189">This time, select **OBJECT=Group**.</span></span> <span data-ttu-id="07a6a-190">Az utolsó oldalon ahogy az alábbi képen konfigurációt használja.</span><span class="sxs-lookup"><span data-stu-id="07a6a-190">On the last page, use the configuration as in the following picture.</span></span> <span data-ttu-id="07a6a-191">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="07a6a-191">Click **Finish**.</span></span>  
   <span data-ttu-id="07a6a-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span></span>  
   <span data-ttu-id="07a6a-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span><span class="sxs-lookup"><span data-stu-id="07a6a-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span></span>  
6. <span data-ttu-id="07a6a-194">Nem kötelező: Ha kívánja, konfigurálhatja a további futtatási profilokat.</span><span class="sxs-lookup"><span data-stu-id="07a6a-194">Optional: If you want to, you can configure additional run profiles.</span></span> <span data-ttu-id="07a6a-195">Ez a forgatókönyv csak a teljes importálás használjuk.</span><span class="sxs-lookup"><span data-stu-id="07a6a-195">For this walkthrough, only the Full Import is used.</span></span>
7. <span data-ttu-id="07a6a-196">Kattintson a **OK** futtatási profilok módosításának befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="07a6a-196">Click **OK** to finish changing run profiles.</span></span>

## <a name="add-some-test-data-and-test-the-import"></a><span data-ttu-id="07a6a-197">Adja hozzá az egyes adatok tesztelésére, és tesztelje az importálás</span><span class="sxs-lookup"><span data-stu-id="07a6a-197">Add some test data and test the import</span></span>
<span data-ttu-id="07a6a-198">Töltse ki a mintaadatbázis néhány tesztadatot.</span><span class="sxs-lookup"><span data-stu-id="07a6a-198">Fill out some test data in your sample database.</span></span> <span data-ttu-id="07a6a-199">Amikor elkészült, válassza ki a **futtatása** és **teljes importálás**.</span><span class="sxs-lookup"><span data-stu-id="07a6a-199">When you are ready, select **Run** and **Full import**.</span></span>

<span data-ttu-id="07a6a-200">Ez a felhasználó két telefonszámot és egy csoport néhány tagjához.</span><span class="sxs-lookup"><span data-stu-id="07a6a-200">Here is a user with two phone numbers and a group with some members.</span></span>  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a><span data-ttu-id="07a6a-203">A függelék</span><span class="sxs-lookup"><span data-stu-id="07a6a-203">Appendix A</span></span>
<span data-ttu-id="07a6a-204">**SQL-parancsfájl a mintaadatbázis létrehozásához**</span><span class="sxs-lookup"><span data-stu-id="07a6a-204">**SQL script to create the sample database**</span></span>

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
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
