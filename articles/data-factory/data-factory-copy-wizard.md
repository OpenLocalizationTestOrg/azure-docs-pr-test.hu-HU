---
title: "Másolja az adatokat könnyen másolása varázsló – Azure |} Microsoft Docs"
description: "Ismerje meg az adatokat másolni a támogatott adatforrások mosdók a Data Factory másolása varázsló használatával kapcsolatban."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 282cb4484f8209e6bb36f2a02d7a897f1ba0aa8e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a><span data-ttu-id="1c5c6-103">Átmásolnia vagy áthelyeznie az adatokat könnyen az Azure Data Factory másolása varázsló</span><span class="sxs-lookup"><span data-stu-id="1c5c6-103">Copy or move data easily with Azure Data Factory Copy Wizard</span></span>
<span data-ttu-id="1c5c6-104">Az Azure Data Factory másolása varázsló választásával dolgozhat fel adatokat, amely általában az első lépés egy végpont integrációs forgatókönyvet folyamatának megkönnyítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-104">The Azure Data Factory Copy Wizard is to ease the process of ingesting data, which is usually a first step in an end-to-end data integration scenario.</span></span> <span data-ttu-id="1c5c6-105">Az Azure Data Factory másolása varázsló áthaladás, ha nem kell megérteni az összes társított szolgáltatások, adathalmazok és adatcsatornák a JSON-definíciót.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-105">When going through the Azure Data Factory Copy Wizard, you do not need to understand any JSON definitions for linked services, datasets, and pipelines.</span></span> <span data-ttu-id="1c5c6-106">Miután végzett a varázsló utasításait, az a varázsló automatikusan létrehozza a kijelölt adatforrás adatainak másolása a kiválasztott cél csővezeték.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-106">However, after you complete all the steps in the wizard, the wizard automatically creates a pipeline to copy data from the selected data source to the selected destination.</span></span> <span data-ttu-id="1c5c6-107">Emellett a varázsló segít ellenőrzése a szerzői, időpontjában alatt okozhatnak adatait, amely menti a idő jelentős részét, különösen ha meg vannak adatok bevitele először az adatforrásból.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-107">In addition, the Copy Wizard helps you to validate the data being ingested at the time of authoring, which saves much of your time, especially when you are ingesting data for the first time from the data source.</span></span> <span data-ttu-id="1c5c6-108">A varázsló elindításához kattintson a **adatok másolása** csempét a data factory kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-108">To start the Copy Wizard, click the **Copy data** tile on the home page of your data factory.</span></span>

![Másolás varázsló](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a><span data-ttu-id="1c5c6-110">Adatok másolása egy egyszerűen elsajátítható varázslót</span><span class="sxs-lookup"><span data-stu-id="1c5c6-110">An intuitive wizard for copying data</span></span>
<span data-ttu-id="1c5c6-111">Ez a varázsló lehetővé teszi, hogy könnyedén helyezhetik át adatokat különböző forrásokból célhelyekre perc múlva.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-111">This wizard allows you to easily move data from a wide variety of sources to destinations in minutes.</span></span> <span data-ttu-id="1c5c6-112">Elvégzése után a varázsló, a másolási tevékenység során a folyamat automatikusan létrejön függő Data Factory entitások (társított szolgáltatások és adatkészletek) együtt.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-112">After going through the wizard, a pipeline with a copy activity is automatically created for you along with dependent Data Factory entities (linked services and datasets).</span></span> <span data-ttu-id="1c5c6-113">Nincsenek további lépéseket kell végrehajtani a folyamatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-113">No additional steps are required to create the pipeline.</span></span>   

![Adatforrás kiválasztása](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> <span data-ttu-id="1c5c6-115">Lásd: [másolása varázsló az oktatóanyag](data-factory-copy-data-wizard-tutorial.md) cikk lépéseit másolása minta folyamatokat létrehozni adatait az Azure blob-Azure SQL Database táblához.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-115">See [Copy Wizard tutorial](data-factory-copy-data-wizard-tutorial.md) article for step-by-step instructions to create a sample pipeline to copy data from an Azure blob to an Azure SQL Database table.</span></span> 
> 
> 

<span data-ttu-id="1c5c6-116">A varázsló a big Data típusú adatok szem előtt a kezdetektől úgy van kialakítva.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-116">The wizard is designed with big data in mind from the start.</span></span> <span data-ttu-id="1c5c6-117">Egyszerű és hatékony hozhatnak létre, amelyek több száz mappákat, fájlokat vagy az adatok másolása varázslóval táblák áthelyezése adat-előállító adatcsatornák.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-117">It is simple and efficient to author Data Factory pipelines that move hundreds of folders, files, or tables using the Copy Data wizard.</span></span> <span data-ttu-id="1c5c6-118">A varázsló támogatja a következő három szolgáltatás: automatikus adatelőnézet, séma rögzítési és leképezés és az adatok szűrése.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-118">The wizard supports the following three features: Automatic data preview, schema capture and mapping, and filtering data.</span></span> 

## <a name="automatic-data-preview"></a><span data-ttu-id="1c5c6-119">Automatikus megtekintés</span><span class="sxs-lookup"><span data-stu-id="1c5c6-119">Automatic data preview</span></span>
<span data-ttu-id="1c5c6-120">A varázsló lehetővé teszi, hogy tekintse át a választott adatforrással kapcsolatosan, hogy ellenőrizze, hogy az adatok a másolni kívánt adatokat az adatok egy részét.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-120">The copy wizard allows you to review part of the data from the selected data source for you to validate whether the data it is the right data you want to copy.</span></span> <span data-ttu-id="1c5c6-121">Ezenkívül ha az adatok szövegfájlba, másolása varázsló kijelölt szöveg sor és oszlop elválasztókat és séma automatikusan további.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-121">In addition, if the source data is in a text file, the copy wizard parses the text file to learn row and column delimiters, and schema automatically.</span></span> 

![Fájl formázási beállítások](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a><span data-ttu-id="1c5c6-123">Séma rögzítési és -leképezés</span><span class="sxs-lookup"><span data-stu-id="1c5c6-123">Schema capture and mapping</span></span>
<span data-ttu-id="1c5c6-124">A séma, bemeneti adatokat előfordulhat, hogy a kimeneti adatokat egyes esetekben sémája nem egyezik meg.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-124">The schema of input data may not match the schema of output data in some cases.</span></span> <span data-ttu-id="1c5c6-125">Ebben a forgatókönyvben kell hozzárendelni a cél séma oszlopok a forrás séma oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-125">In this scenario, you need to map columns from the source schema to columns from the destination schema.</span></span> 

<span data-ttu-id="1c5c6-126">Másolása varázsló automatikusan leképezi a forrás sémában oszlopok oszlopok a cél sémában.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-126">The copy wizard automatically maps columns in the source schema to columns in the destination schema.</span></span> <span data-ttu-id="1c5c6-127">A leképezések felülírása a legördülő listák segítségével (de) adja meg, hogy egy oszlopot kell figyelmen kívül hagyja az adatok másolásakor.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-127">You can override the mappings by using the drop-down lists (or) specify whether a column needs to be skipped while copying the data.</span></span>   

![Séma-hozzárendelése](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a><span data-ttu-id="1c5c6-129">Adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="1c5c6-129">Filtering data</span></span>
<span data-ttu-id="1c5c6-130">A varázsló lehetővé teszi szűrése forrásadatok csak, amelyet a cél/fogadó adattárba másolni kívánt adatok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-130">The wizard allows you to filter source data to select only the data that needs to be copied to the destination/sink data store.</span></span> <span data-ttu-id="1c5c6-131">Szűrés csökkenti a fogadó adattárba másolandó adatok mennyiségét, és ezért javítja a teljesítményt, a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-131">Filtering reduces the volume of the data to be copied to the sink data store and therefore enhances the throughput of the copy operation.</span></span> <span data-ttu-id="1c5c6-132">Biztosít egy relációs adatbázisban lévő adatok szűrése rugalmasan használatával SQL lekérdezési nyelv (vagy) fájlok az Azure blob mappában használatával [adat-előállító funkciók és változók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="1c5c6-132">It provides a flexible way to filter data in a relational database by using SQL query language (or) files in an Azure blob folder by using [Data Factory functions and variables](data-factory-functions-variables.md).</span></span>   

### <a name="filtering-of-data-in-a-database"></a><span data-ttu-id="1c5c6-133">Egy adatbázis adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="1c5c6-133">Filtering of data in a database</span></span>
<span data-ttu-id="1c5c6-134">A példában az SQL-lekérdezést használ a `Text.Format` függvény és `WindowStart` változó.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-134">In the example, the SQL query uses the `Text.Format` function and `WindowStart` variable.</span></span> 

![Kifejezések ellenőrzése](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a><span data-ttu-id="1c5c6-136">Az adatok Azure blob mappában szűrése</span><span class="sxs-lookup"><span data-stu-id="1c5c6-136">Filtering of data in an Azure blob folder</span></span>
<span data-ttu-id="1c5c6-137">Adatok másolása egy mappába, amely alapján futásidőben határozza meg a mappa elérési változók is használhatja [rendszerváltozók](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="1c5c6-137">You can use variables in the folder path to copy data from a folder that is determined at runtime based on [system variables](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="1c5c6-138">A támogatott értékek: **{year}**, **{month}**, **{day}**, **{óra}**, **{perc}**, és **{egyéni}**.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-138">The supported variables are: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}**, and **{custom}**.</span></span> <span data-ttu-id="1c5c6-139">Példa: inputfolder / {year} / {month} / {day}.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-139">Example: inputfolder/{year}/{month}/{day}.</span></span>

<span data-ttu-id="1c5c6-140">Tegyük fel, hogy rendelkezik-e bemeneti mappák a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="1c5c6-140">Suppose that you have input folders in the following format:</span></span>

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

<span data-ttu-id="1c5c6-141">Kattintson a **Tallózás** gombra kattint, a **fájl vagy mappa**, keresse meg az egyik mappát (például 2016 -> 03 -> 01 -> 02), és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-141">Click the **Browse** button for **File or folder**, browse to one of these folders (for example, 2016->03->01->02), and click **Choose**.</span></span> <span data-ttu-id="1c5c6-142">Megtekintheti az `2016/03/01/02` a szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-142">You should see `2016/03/01/02` in the text box.</span></span> <span data-ttu-id="1c5c6-143">Cserélje le **2016** rendelkező **{year}**, **03** rendelkező **{month}**, **01** rendelkező **{day}**, és **02** rendelkező **{óra}**, és nyomja meg a Tab.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-143">Now, replace **2016** with **{year}**, **03** with **{month}**, **01** with **{day}**, and **02** with **{hour}**, and press Tab.</span></span> <span data-ttu-id="1c5c6-144">Legördülő lista használatával válassza ki a formátumot az alábbi négy változók kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="1c5c6-144">You should see drop-down lists to select the format for these four variables:</span></span>

![Rendszer változók használata](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

<span data-ttu-id="1c5c6-146">Ahogy az az alábbi képernyőfelvételen, használhatja a **egyéni** változó, és bármilyen [támogatott formázási karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c5c6-146">As shown in the following screenshot, you can also use a **custom** variable and any [supported format strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span></span> <span data-ttu-id="1c5c6-147">Válasszon egy mappát a struktúra, használja a **Tallózás** először gombra.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-147">To select a folder with that structure, use the **Browse** button first.</span></span> <span data-ttu-id="1c5c6-148">Ezután cserélje le a értékét **{egyéni}**, és nyomja le az ENTER lapon, láthatja a szövegmezőben, ahová beírhatja a Formátum-karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-148">Then replace a value with **{custom}**, and press Tab to see the text box where you can type the format string.</span></span>     

![Egyéni változóval](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a><span data-ttu-id="1c5c6-150">A különböző adatok és objektumtípusok támogatása</span><span class="sxs-lookup"><span data-stu-id="1c5c6-150">Support for diverse data and object types</span></span>
<span data-ttu-id="1c5c6-151">A varázsló segítségével, akár több százszor is mappákat, fájlokat vagy táblák hatékonyan mozgatják.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-151">By using the Copy Wizard, you can efficiently move hundreds of folders, files, or tables.</span></span>

![Válassza ki azokat a táblákat, amelyből adatok másolása](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a><span data-ttu-id="1c5c6-153">Ütemezési beállítások</span><span class="sxs-lookup"><span data-stu-id="1c5c6-153">Scheduling options</span></span>
<span data-ttu-id="1c5c6-154">Futtathatja a másolási művelet egyszer vagy ütemezés (óránként, naponta, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="1c5c6-154">You can run the copy operation once or on a schedule (hourly, daily, and so on).</span></span> <span data-ttu-id="1c5c6-155">Mindkét lehetőség használható az összekötők a hardverekről a helyszínen, a felhő és a helyi asztali példány között.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-155">Both of these options can be used for the breadth of the connectors across on-premises, cloud, and local desktop copy.</span></span>

<span data-ttu-id="1c5c6-156">Egy egyszeri másolási művelet lehetővé teszi, hogy egy célra forrásból adatmozgás csak egyszer.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-156">A one-time copy operation enables data movement from a source to a destination only once.</span></span> <span data-ttu-id="1c5c6-157">Érvényes adatok bármilyen méretű és bármely támogatott formátumra.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-157">It applies to data of any size and any supported format.</span></span> <span data-ttu-id="1c5c6-158">Az ütemezett másolatát lehetővé teszi, hogy az előírt ismétlődése adatokat másolhat.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-158">The scheduled copy allows you to copy data on a prescribed recurrence.</span></span> <span data-ttu-id="1c5c6-159">Gazdag beállításaihoz (például az újra gombra, időtúllépés és riasztások) segítségével konfigurálhatja az ütemezett másolatát.</span><span class="sxs-lookup"><span data-stu-id="1c5c6-159">You can use rich settings (like retry, timeout, and alerts) to configure the scheduled copy.</span></span>

![Ütemezési tulajdonságok](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a><span data-ttu-id="1c5c6-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c5c6-161">Next steps</span></span>
<span data-ttu-id="1c5c6-162">A Data Factory másolása varázsló segítségével hozzon létre egy folyamatot másolási tevékenység az első útmutatást lásd: [oktatóanyag: hozzon létre egy folyamatot, a másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="1c5c6-162">For a quick walkthrough of using the Data Factory Copy Wizard to create a pipeline with Copy Activity, see [Tutorial: Create a pipeline using the Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

