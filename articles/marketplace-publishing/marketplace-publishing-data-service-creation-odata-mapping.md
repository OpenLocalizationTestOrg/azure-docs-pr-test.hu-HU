---
title: "egy szolgáltatás a piactér hello aaaGuide toocreating |} Microsoft Docs"
description: "Hogyan toocreate, hitelesíthet és az adatok szolgáltatás telepítése részletes utasításokat vásárolja meg a hello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: deb2e52dd03f5beb2ad6a927bd2d03e47d20b691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mapping-an-existing-web-service-tooodata-through-csdl"></a><span data-ttu-id="b20b2-103">Egy meglévő webes szolgáltatás tooOData keresztül CSDL leképezése</span><span class="sxs-lookup"><span data-stu-id="b20b2-103">Mapping an existing web service tooOData through CSDL</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b20b2-104">**Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.**</span><span class="sxs-lookup"><span data-stu-id="b20b2-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="b20b2-105">Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="b20b2-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="b20b2-106">Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="b20b2-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="b20b2-107">Ez a cikk áttekintést nyújt hogyan toouse egy CSDL toomap egy meglévő service tooan kompatibilis OData-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b20b2-107">This article gives an overview on how toouse a CSDL toomap an existing service tooan OData compatible service.</span></span> <span data-ttu-id="b20b2-108">Ismerteti, hogyan toocreate hello leképezési dokumentum (CSDL), amely átalakítja a hívások és hello hello ügyfél bemeneti kérése hello kimeneti (adatok) vissza toohello ügyfél kompatibilis OData-adatcsatorna keresztül.</span><span class="sxs-lookup"><span data-stu-id="b20b2-108">It explains how toocreate hello mapping document (CSDL) that transforms hello input request from hello client via a service call and hello output (data) back toohello client via an OData compatible feed.</span></span> <span data-ttu-id="b20b2-109">Microsoft Azure piactéren szolgáltatások toohello végfelhasználók közzététele hello OData protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="b20b2-109">Microsoft’s Azure Marketplace exposes services toohello end-users by using hello OData protocol.</span></span> <span data-ttu-id="b20b2-110">Tartalomszolgáltatók (adatok tulajdonosai) által feltárt szolgáltatások érhetők el a többféle formában, mint például a többi, SOAP, stb.</span><span class="sxs-lookup"><span data-stu-id="b20b2-110">Services that are exposed by content providers (Data Owners) are exposed in a variety of forms, such as REST, SOAP, etc.</span></span>

## <a name="what-is-a-csdl-and-its-structure"></a><span data-ttu-id="b20b2-111">Mi az a CSDL és struktúrája?</span><span class="sxs-lookup"><span data-stu-id="b20b2-111">What is a CSDL and its structure?</span></span>
<span data-ttu-id="b20b2-112">Egy CSDL (Fogalmi séma Definition Language) a meghatározása, hogy toodescribe webszolgáltatás vagy az adatbázis szolgáltatás közös XML szóhasználatára hasonlítanak toohello Azure piactér specifikációval.</span><span class="sxs-lookup"><span data-stu-id="b20b2-112">A CSDL (Conceptual Schema Definition Language) is a specification defining how toodescribe web service or database service in common XML verbiage toohello Azure Marketplace.</span></span>

<span data-ttu-id="b20b2-113">Hello – sematikus ábra **kérelem folyamata:**</span><span class="sxs-lookup"><span data-stu-id="b20b2-113">Simple overview of hello **request flow:**</span></span>

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

<span data-ttu-id="b20b2-114">Hello **adatfolyama** hello van ellenkező irányban:</span><span class="sxs-lookup"><span data-stu-id="b20b2-114">hello **data flow** is in hello opposite direction:</span></span>

  `Client <- Azure Marketplace <- Content Provider’s WebService`

<span data-ttu-id="b20b2-115">**1. ábra** ábrázolja, hogyan ügyfél volna a szerezhet tartalomszolgáltató (a szolgáltatás) átnézi hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="b20b2-115">**Figure 1** diagrams how a client would obtain data from a content provider (your service) by going through hello Azure Marketplace.</span></span>  <span data-ttu-id="b20b2-116">hello CSDL hello leképezési/átalakítása összetevő toohandle hello kérelem használja, és adatokat továbbítani hello tartalomszolgáltató Services-szolgáltatás(ok) és kérő ügyfél hello között.</span><span class="sxs-lookup"><span data-stu-id="b20b2-116">hello CSDL is used by hello Mapping/Transformation component toohandle hello request and data pass between hello content provider’s service(s) and hello requesting client.</span></span>

<span data-ttu-id="b20b2-117">*1. ábra: Részletes folyamata a kérelmező ügyfélnek toocontent szolgáltató Azure piactéren keresztül*</span><span class="sxs-lookup"><span data-stu-id="b20b2-117">*Figure 1: Detailed flow from requesting client toocontent provider via Azure Marketplace*</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

<span data-ttu-id="b20b2-119">Háttér Atom, Atom Pub és hello OData protokoll, mely hello Azure piactér kiterjesztések összeállítása, olvassa el: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span><span class="sxs-lookup"><span data-stu-id="b20b2-119">For background on Atom, Atom Pub, and hello OData protocol upon which hello Azure Marketplace extensions build, please review: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span></span>

<span data-ttu-id="b20b2-120">Hivatkozás promptjai kivonata: *"hello hello Open Data protokoll (OData alábbiakban említett tooas) célja tooprovide REST-alapú protokoll, CRUD-stílusú műveleteket (létrehozás, Olvasás, frissítés és törlés) adatszolgáltatások elérhető erőforrásokon. A "data"szolgáltatás"" végpont ahol van egy vagy több "gyűjtemények" minden nulla vagy több "bejegyzés", álló származó adatokat adta-e nevű-érték párokat. OData által közzétett Microsoft OASIS (szervezet hello fizetési a strukturált adatokat szabványok) szabványok szerint, hogy bárki toocan szeretne felépítéséhez, kiszolgálók, ügyfelek vagy eszközök jogdíjak és korlátozások nélkül."*</span><span class="sxs-lookup"><span data-stu-id="b20b2-120">Excerpt from above link: *“hello purpose of hello Open Data protocol (hereafter referred tooas OData) is tooprovide a REST-based protocol for CRUD-style operations (Create, Read, Update and Delete) against resources exposed as data services. A “data service” is an endpoint where there is data exposed from one or more “collections” each with zero or more “entries”, which consist of typed named-value pairs. OData is published by Microsoft under OASIS (Organization for hello Advancement of Structured Information Standards) Standards so that anyone that wants toocan build servers, clients or tools without royalties or restrictions.”*</span></span>

### <a name="three-critical-pieces-that-have-toobe-defined-by-hello-csdl-are"></a><span data-ttu-id="b20b2-121">Három kritikus hello CSDL által meghatározott toobe rendelkező van:</span><span class="sxs-lookup"><span data-stu-id="b20b2-121">Three Critical Pieces that have toobe defined by hello CSDL are:</span></span>
* <span data-ttu-id="b20b2-122">Hello **végpont** a szolgáltató hello hello webes cím (URI-val) hello szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b20b2-122">hello **endpoint** of hello Service Provider hello Web Address (URI) of hello Service</span></span>
* <span data-ttu-id="b20b2-123">Hello **Eseményadat-paraméterek** , bemeneti toohello szolgáltató átadta toohello tartalomszolgáltató szolgáltatás le toohello adattípus küldött hello paraméterek hello meghatározását.</span><span class="sxs-lookup"><span data-stu-id="b20b2-123">hello **data parameters** being passed as input toohello Service Provider hello definitions of hello parameters being sent toohello Content Provider’s service down toohello data type.</span></span>
* <span data-ttu-id="b20b2-124">**Séma** hello adatok toohello kérő szolgáltatás hello séma hello tartalomszolgáltató szolgáltatás kézbesíti hello adatok ad vissza, beleértve a tároló, a gyűjtemények, illetve táblákat, a változók/oszlopok és az adatok típus.</span><span class="sxs-lookup"><span data-stu-id="b20b2-124">**Schema** of hello data being returned toohello Requesting Service hello schema of hello data being delivered by hello Content Provider’s service, including Container, collections/tables, variables/columns, and data types.</span></span>

<span data-ttu-id="b20b2-125">a következő diagram hello hello folyamata, ahol hello ügyfél belép hello OData (hívás toohello tartalomszolgáltató webszolgáltatás) utasítás toogetting hello/eredmények vissza az áttekintését mutatja.</span><span class="sxs-lookup"><span data-stu-id="b20b2-125">hello following diagram shows an overview of hello flow from where hello client enters hello OData statement (call toohello content provider’s web service) toogetting hello results/data back.</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a><span data-ttu-id="b20b2-127">lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b20b2-127">Steps:</span></span>
1. <span data-ttu-id="b20b2-128">Ügyfél küld szolgáltatás híváson keresztül kész, de XML toohello Azure piactér definiált bemeneti paraméterek</span><span class="sxs-lookup"><span data-stu-id="b20b2-128">Client sends request via Service call complete with Input Parameters defined in XML toohello Azure Marketplace</span></span>
2. <span data-ttu-id="b20b2-129">CSDL használt toovalidate hello szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="b20b2-129">CSDL is used toovalidate hello Service call.</span></span>
   * <span data-ttu-id="b20b2-130">hello szolgáltatás hívása formázott majd toohello tartalom szolgáltatók szolgáltatás által küldött hello Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="b20b2-130">hello Formatted Service Call is then sent toohello Content Providers Service by hello Azure Marketplace</span></span>
3. <span data-ttu-id="b20b2-131">hello Webservice végrehajtja és előforma hello művelettel hello Http-műveletet (pl. GET) hello adat tooAzure piactér, ahol a hello kért adatok (ha van ilyen) tesz elérhetővé, az XML-formátuma toohello ügyfél leképezési definiált hello CSDL hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="b20b2-131">hello Webservice executes and preforms hello action of hello Http Verb (i.e. GET) hello data is returned tooAzure Marketplace where hello requested data (if any) is exposes in XML Format toohello Client using hello Mapping defined in hello CSDL.</span></span>
4. <span data-ttu-id="b20b2-132">hello ügyfél küldött hello adatokat (ha van ilyen) XML- vagy JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="b20b2-132">hello Client is sent hello data (if any) in XML or JSON format</span></span>

## <a name="definitions"></a><span data-ttu-id="b20b2-133">Meghatározások</span><span class="sxs-lookup"><span data-stu-id="b20b2-133">Definitions</span></span>
### <a name="odata-atom-pub"></a><span data-ttu-id="b20b2-134">OData ATOM pub</span><span class="sxs-lookup"><span data-stu-id="b20b2-134">OData ATOM pub</span></span>
<span data-ttu-id="b20b2-135">Egy bővítmény toohello ATOM pub, ahol minden bejegyzésben eredményeként a több sorban is állítsa be.</span><span class="sxs-lookup"><span data-stu-id="b20b2-135">An extension toohello ATOM pub where each entry represents one row of a result set.</span></span> <span data-ttu-id="b20b2-136">hello tartalom hello bejegyzés része továbbfejlesztett toocontain hello értékek hello sor – kulcs-érték párként.</span><span class="sxs-lookup"><span data-stu-id="b20b2-136">hello content part of hello entry is enhanced toocontain hello values of hello row – as key value pairs.</span></span> <span data-ttu-id="b20b2-137">További információt itt talál: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span><span class="sxs-lookup"><span data-stu-id="b20b2-137">More information is found here: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span></span>

### <a name="csdl---conceptual-schema-definition-language"></a><span data-ttu-id="b20b2-138">CSDL - fogalmi séma adatdefiníciós nyelv</span><span class="sxs-lookup"><span data-stu-id="b20b2-138">CSDL - Conceptual Schema Definition Language</span></span>
<span data-ttu-id="b20b2-139">Megadhatja a funkciók (SPROCs) és egy adatbázis keresztül közzétett entitásokat.</span><span class="sxs-lookup"><span data-stu-id="b20b2-139">Allows defining functions (SPROCs) and entities that are exposed through a database.</span></span> <span data-ttu-id="b20b2-140">További információt itt talál: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span><span class="sxs-lookup"><span data-stu-id="b20b2-140">More information found here: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span></span>  

> [!TIP]
> <span data-ttu-id="b20b2-141">Kattintson a hello **más verzióiban** legördülő lista és a megfelelő verzió kiválasztása, ha nem lát hello cikk.</span><span class="sxs-lookup"><span data-stu-id="b20b2-141">Click hello **other versions** dropdown and select a version if you don’t see hello article.</span></span>
> 
> 

### <a name="edm---entry-data-model"></a><span data-ttu-id="b20b2-142">EDM - bejegyzés adatmodell</span><span class="sxs-lookup"><span data-stu-id="b20b2-142">EDM - Entry Data Model</span></span>
* <span data-ttu-id="b20b2-143">Áttekintés: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]</span><span class="sxs-lookup"><span data-stu-id="b20b2-143">Overview: [http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx][OverviewLink]</span></span>

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* <span data-ttu-id="b20b2-144">Kép: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]</span><span class="sxs-lookup"><span data-stu-id="b20b2-144">Preview: [http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx][PreviewLink]</span></span>

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* <span data-ttu-id="b20b2-145">Adattípusok: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]</span><span class="sxs-lookup"><span data-stu-id="b20b2-145">Data types: [http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx][DataTypesLink]</span></span>

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

<span data-ttu-id="b20b2-146">hello következő mutatja hello részletes ahol hello ügyfél belép hello OData (hívás toohello tartalomszolgáltató webszolgáltatás) utasítás toogetting hello/eredmények vissza a bal oldali tooRight folyamata:</span><span class="sxs-lookup"><span data-stu-id="b20b2-146">hello following shows hello detailed Left tooRight flow from where hello client enters hello OData statement (call toohello content provider’s web service) toogetting hello results/data back:</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a><span data-ttu-id="b20b2-148">CSDL alapjai</span><span class="sxs-lookup"><span data-stu-id="b20b2-148">CSDL Basics</span></span>
<span data-ttu-id="b20b2-149">Egy CSDL (Fogalmi séma Definition Language) a meghatározása, hogy toodescribe webszolgáltatás vagy az adatbázis szolgáltatás közös XML szóhasználatára hasonlítanak toohello Azure piactér specifikációval.</span><span class="sxs-lookup"><span data-stu-id="b20b2-149">A CSDL (Conceptual Schema Definition Language) is a specification defining how toodescribe web service or database service in common XML verbiage toohello Azure Marketplace.</span></span> <span data-ttu-id="b20b2-150">CSDL ismerteti kritikus hello darab, amely **hello adatforrás toohello Azure Piactérről származó adatok áthaladását hello lehetővé teszi.**</span><span class="sxs-lookup"><span data-stu-id="b20b2-150">CSDL describes hello critical pieces that **makes hello passing of data from hello Data Source toohello Azure Marketplace possible.**</span></span> <span data-ttu-id="b20b2-151">hello fő eleme a dokumentum ismerteti:</span><span class="sxs-lookup"><span data-stu-id="b20b2-151">hello main pieces are described here:</span></span>

* <span data-ttu-id="b20b2-152">Az összes nyilvánosan elérhető funkciók (FunctionImport csomópont) leíró adatokat csatoló</span><span class="sxs-lookup"><span data-stu-id="b20b2-152">Interface information describing all publicly available functions (FunctionImport Node)</span></span>
* <span data-ttu-id="b20b2-153">Adattípus-információ az összes üzenet requests(input) és üzenet responses(outputs) (EntityContainer/EntitySet készlet/EntityType csomópontok)</span><span class="sxs-lookup"><span data-stu-id="b20b2-153">Data type information for all message requests(input) and message responses(outputs) (EntityContainer/EntitySet/EntityType Nodes)</span></span>
* <span data-ttu-id="b20b2-154">Kötési információ hello átviteli protokoll toobe használt (fejléc csomópont)</span><span class="sxs-lookup"><span data-stu-id="b20b2-154">Binding information about hello transport protocol toobe used (Header Node)</span></span>
* <span data-ttu-id="b20b2-155">Címadatok hello keresése a megadott service (a BaseURI attribútum)</span><span class="sxs-lookup"><span data-stu-id="b20b2-155">Address information for locating hello specified service (BaseURI attribute)</span></span>

<span data-ttu-id="b20b2-156">Hello CSDL a ez már a vég, hello szolgáltatás kérelmező és hello-szolgáltató platform - és nyelvfüggetlen szerződés jelöli.</span><span class="sxs-lookup"><span data-stu-id="b20b2-156">In a nutshell, hello CSDL represents a platform- and language-independent contract between hello service requestor and hello service provider.</span></span> <span data-ttu-id="b20b2-157">Hello CSDL, egy ügyfél segítségével keresse meg a webszolgáltatás service, illetve az adatbázis és a nyilvánosan elérhető funkciók meghívni.</span><span class="sxs-lookup"><span data-stu-id="b20b2-157">Using hello CSDL, a client can locate a web service/database service and invoke any of its publicly available functions.</span></span>

### <a name="relating-a-csdl-tooa-database-or-a-collection"></a><span data-ttu-id="b20b2-158">Egy CSDL tooa adatbázis vagy egy gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="b20b2-158">Relating a CSDL tooa Database or a Collection</span></span>
<span data-ttu-id="b20b2-159">**hello CSDL-specifikációja**</span><span class="sxs-lookup"><span data-stu-id="b20b2-159">**hello CSDL Specification**</span></span>

<span data-ttu-id="b20b2-160">CSDL egy webszolgáltatás-bővítmény leíró XML-nyelvtan.</span><span class="sxs-lookup"><span data-stu-id="b20b2-160">CSDL is XML grammar for describing a web service.</span></span> <span data-ttu-id="b20b2-161">hello specification maga 4 fő elemet oszlik: EntitySet, a következő functionimport elemben; Névtér, és az entitytype típus.</span><span class="sxs-lookup"><span data-stu-id="b20b2-161">hello specification itself is divided into 4 major elements:  EntitySet, FunctionImport; NameSpace, and EntityType.</span></span>

<span data-ttu-id="b20b2-162">toomake a absztrakciós könnyebb toounderstand lehetővé teszi, hogy egy CSDL tooa táblából.</span><span class="sxs-lookup"><span data-stu-id="b20b2-162">toomake this abstraction easier toounderstand lets relate a CSDL tooa table.</span></span>

<span data-ttu-id="b20b2-163">Ne feledje;</span><span class="sxs-lookup"><span data-stu-id="b20b2-163">Remember;</span></span>

  <span data-ttu-id="b20b2-164">CSDL jelöli hello platform - és nyelvfüggetlen szerződés **szolgáltatás kérelmező** és hello **szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="b20b2-164">CSDL represents a platform- and language-independent contract between hello **service requestor** and hello **service provider**.</span></span> <span data-ttu-id="b20b2-165">CSDL, használja a **ügyfél** keresheti meg a **webes szolgáltatás, illetve az adatbázis-szolgáltatás** és a nyilvánosan elérhető meghívni **funkciók.**</span><span class="sxs-lookup"><span data-stu-id="b20b2-165">Using CSDL, a **client** can locate a **web service/database service** and invoke any of its publicly available **functions.**</span></span>

<span data-ttu-id="b20b2-166">Az adatszolgáltatás-hello négy része egy CSDL-re egy adatbázis, táblázatok, oszlopok és eljárás tekintetében.</span><span class="sxs-lookup"><span data-stu-id="b20b2-166">For a Data Service hello four parts of a CSDL can be thought of in terms of a Database, Table, Column, and Store Procedure.</span></span>

<span data-ttu-id="b20b2-167">Vonatkozó ezek egy szolgáltatás számára az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b20b2-167">Relating these as follows for a Data Service:</span></span>

* <span data-ttu-id="b20b2-168">EntityContainer ~ adatbázis =</span><span class="sxs-lookup"><span data-stu-id="b20b2-168">EntityContainer  ~=  Database</span></span>
* <span data-ttu-id="b20b2-169">EntitySet ~ = táblázat</span><span class="sxs-lookup"><span data-stu-id="b20b2-169">EntitySet  ~=  Table</span></span>
* <span data-ttu-id="b20b2-170">EntityType ~ oszlopok =</span><span class="sxs-lookup"><span data-stu-id="b20b2-170">EntityType  ~= Columns</span></span>
* <span data-ttu-id="b20b2-171">FunctionImport ~ = tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="b20b2-171">FunctionImport  ~=  Stored Procedure</span></span>

<span data-ttu-id="b20b2-172">**HTTP-műveletek engedélyezett**</span><span class="sxs-lookup"><span data-stu-id="b20b2-172">**HTTP Verbs allowed**</span></span>

* <span data-ttu-id="b20b2-173">GET – a hello adatbázisból (gyűjteményének beolvasása) értéket ad vissza értéket</span><span class="sxs-lookup"><span data-stu-id="b20b2-173">GET – returns values from hello db (returns a Collection)</span></span>
* <span data-ttu-id="b20b2-174">POST – használt toopass adatok tooand választható visszatérési értékek a hello adatbázisból (hozzon létre egy új bejegyzést hello gyűjteményben, visszatérési azonosítója/URI-címe)</span><span class="sxs-lookup"><span data-stu-id="b20b2-174">POST – used toopass data tooand optional return values from hello db (Create a new entry in hello collection, return id/URI)</span></span>
* <span data-ttu-id="b20b2-175">TÖRLÉS – hello DB (a gyűjtemény törlése) adatainak törlése</span><span class="sxs-lookup"><span data-stu-id="b20b2-175">DELETE – Deletes data from hello DB (Deletes a collection)</span></span>
* <span data-ttu-id="b20b2-176">PUT – adatok frissítéséhez be egy DB (cserélje ki egy gyűjteményt, vagy hozzon létre egyet)</span><span class="sxs-lookup"><span data-stu-id="b20b2-176">PUT – Update data into a DB (replace a collection or create one)</span></span>

## <a name="metadatamapping-document"></a><span data-ttu-id="b20b2-177">Metaadat-hozzárendelés dokumentum</span><span class="sxs-lookup"><span data-stu-id="b20b2-177">Metadata/Mapping Document</span></span>
<span data-ttu-id="b20b2-178">hello metaadat-hozzárendelés dokumentum a tartalomszolgáltató meglévő webszolgáltatásokat, hogy akkor is elérhető egy OData webszolgáltatást hello Azure piactér rendszer által használt toomap.</span><span class="sxs-lookup"><span data-stu-id="b20b2-178">hello metadata/mapping document is used toomap a content provider’s existing web services so that it can be exposed as an OData web service by hello Azure Marketplace system.</span></span> <span data-ttu-id="b20b2-179">CSDL alapul, és megvalósítja néhány bővítmények tooCSDL tooaccommodate hello igényeinek REST alapú webszolgáltatások közzétéve az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="b20b2-179">It is based on CSDL and implements a few extensions tooCSDL tooaccommodate hello needs of REST based web services exposed through Azure Marketplace.</span></span> <span data-ttu-id="b20b2-180">hello bővítmények találhatók hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) névtér.</span><span class="sxs-lookup"><span data-stu-id="b20b2-180">hello extensions are found in hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) namespace.</span></span>

<span data-ttu-id="b20b2-181">A következő egy példa hello CSDL: (Másolás és beillesztés hello az alábbi példában CSDL az XML-szerkesztő, és módosítsa a toomatch a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b20b2-181">An example of hello CSDL follows:  (Copy and paste hello below example CSDL into an XML editor and change toomatch your Service.</span></span>  <span data-ttu-id="b20b2-182">Majd illessze be CSDL-leképezési DataService lapon a való létrehozásakor a szolgáltatás hello [Azure piactér közzétételi Portáljára](https://publish.windowsazure.com)).</span><span class="sxs-lookup"><span data-stu-id="b20b2-182">Then paste into CSDL Mapping under DataService tab when creating your service in hello  [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).</span></span>

<span data-ttu-id="b20b2-183">**Feltételek:** kapcsolatos hello CSDL kifejezések toohello [közzétételi Portáljára](https://publish.windowsazure.com) felhasználói felület (PPUI) feltételeit.</span><span class="sxs-lookup"><span data-stu-id="b20b2-183">**Terms:** Relating hello CSDL terms toohello [Publishing Portal](https://publish.windowsazure.com) UI (PPUI) terms.</span></span>

* <span data-ttu-id="b20b2-184">Ajánlat hello PPUI "Title" tooMyWebOffer konfigurációelemmel kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="b20b2-184">Offer “Title” in hello PPUI relates tooMyWebOffer</span></span>
* <span data-ttu-id="b20b2-185">Hello PPUI az értéket túl vonatkozik**Publisher megjelenített név** a hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="b20b2-185">MyCompany in hello PPUI relates too**Publisher Display Name** in hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI</span></span>
* <span data-ttu-id="b20b2-186">Az API-vonatkozik tooa Web vagy adatszolgáltatás (egy csomag hello PPUI)</span><span class="sxs-lookup"><span data-stu-id="b20b2-186">Your API relates tooa Web or Data Service (a Plan in hello PPUI)</span></span>

<span data-ttu-id="b20b2-187">**Hierarchia:** egy vállalat (tartalomszolgáltató) tulajdonában lévő esetében, amelyek Plan(s), nevezetesen service(s), mely sor fel egy API-t.</span><span class="sxs-lookup"><span data-stu-id="b20b2-187">**Hierarchy:** A Company (Content Provider) owns Offer(s) which have Plan(s), namely service(s), which line up with an API.</span></span>

### <a name="webservice-csdl-example"></a><span data-ttu-id="b20b2-188">Webszolgáltatás CSDL-példa</span><span class="sxs-lookup"><span data-stu-id="b20b2-188">WebService CSDL Example</span></span>
<span data-ttu-id="b20b2-189">Tooa szolgáltatás, amely a webes alkalmazás a végpont (például egy C#-alkalmazás) adategyezményt alkalmazza csatlakozik</span><span class="sxs-lookup"><span data-stu-id="b20b2-189">Connects tooa service that is exposing an web application endpoint (like a C# application)</span></span>

        <?xml version="1.0" encoding="utf-8"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all hello web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition near hello bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is hello entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines hello service call, replace & with hello encode value (&amp;).
        In hello input name value pairs {param} represents passed in value.
        Or hello value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows hello CSDL toospecifically specify hello verb of hello service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes hello parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how hello web service call is exposed toomarketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, hello {...} point toohello parameters defined below.
        Marketplace will read hello parameters from this URITemplate and fill hello values into hello corresponding parameters of hello BaseUri or RequestBody (below) during calls tooyour service.  
        It is okay for @d:BaseUri tooinclude information only for Marketplace consumption, it will not be exposed tooend users. i.e. hello hardcoded AccountKey in hello above BaseURI does not show up in hello client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of hello RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders tooinsert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how toopass userid and password via hello header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed tooend users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with hello value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited tooappearing as hello value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used toospecify values tooinsert into hello ATOM feed returned toohello end user.  You can specify hello element toocontain a fixed message by providing a value for hello element (this is hello default value).  @d:Map is an XPath expression that points into hello response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay tooalso add a d:Map attribute toooverride this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of hello web service call:
        @Name should match exactly (case sensitive) hello {…} placeholders in hello @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether hello parameter is required.
        @d:Regex - optional, attribute toodescribe hello string, limiting unwanted input at hello entry of hello system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in hello Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating hello service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in hello order listed.
        @d:Match is an Xpath query on hello service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is hello error code tooreturn if an response matches hello error.
        @d:errorMessage is hello user friendly message tooreturn when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect toohello service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- hello EntityContainer defines hello output data schema -->
        </EntityContainer>
        <!-- hello EntityType @d:Map defines hello repeating node (an XPath query) in hello response (output data schema). -->
        <!-- If these nodes are outside a namespace, add hello prefix in hello xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in hello ATOM feed, so comply with hello XML element naming restrictions (no spaces or other illegal characters).
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query toopoint at hello location tooextract hello content from your services response.
        hello "." is relative toohello repeating node in hello EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> <span data-ttu-id="b20b2-190">További CSDL-webszolgáltatás példákat cikkében hello cikk [példák leképezése egy meglévő webes szolgáltatás tooOData CSDLs keresztül](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span><span class="sxs-lookup"><span data-stu-id="b20b2-190">View more CSDL Web Service examples in hello article [Examples of mapping an existing web service tooOData through CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span></span>
> 
> 

### <a name="dataservice-csdl-example"></a><span data-ttu-id="b20b2-191">DataService CSDL-példa</span><span class="sxs-lookup"><span data-stu-id="b20b2-191">DataService CSDL Example</span></span>
<span data-ttu-id="b20b2-192">Csatlakozás tooa szolgáltatás, amely a adategyezményt alkalmazza adatbázis tábla vagy nézet végpont az alábbi példában látható két API-khoz, az adatbázis-alapú API CSDL (használhat nézetek táblák helyett).</span><span class="sxs-lookup"><span data-stu-id="b20b2-192">Connects tooa service that is exposing a database table or view as an endpoint Below example shows two APIs for Data base based API CSDL (can use views rather than tables).</span></span>

        <?xml version="1.0"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all hello data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of hello EntitySet as a Service
        @Name is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); hello table (or view) and columns toobe returned by hello data service.)
            Map is hello schema.tabel or schema.view
            dals.TableName is hello table Name
            Name is hello name identifier for hello EntityType and hello Name of hello service exposed toohello client via hello UI.
            dals:IsExposed determines if hello table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is hello schema name of hello table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines hello column properties and hello output of hello service.
            dals:ColumnName is hello name of hello column in hello table /view.
            Type is hello emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is hello maximum length of hello output value
            Name is hello name of hello Property and is exposed toohello client facing UI
            dals:IsReturned is hello Boolean that determines if hello Service exposes this value toohello client.
            IsQueryable is hello Boolean that determines if hello column can be used in a database query
            (For data Services: tooimprove Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is hello numerical position x in hello table or hello View, where x is from 1 toohello number of columns in hello table.
            dals:DatabaseDataType is hello data type of hello column in hello database, i.e. SQL data type dals:IsPrimaryKey indicates if hello column is hello Primary key in hello table/view.  (hello columns marked ISPrimaryKey are used in hello Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a><span data-ttu-id="b20b2-193">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b20b2-193">See Also</span></span>
* <span data-ttu-id="b20b2-194">Ha érdekli tanulási és ismertetése hello adott csomópontok és a paraméterek, olvassa el ebben a cikkben [szolgáltatás OData leképezési Adatcsomópontokat](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és a nagybetűk használatát a környezetben.</span><span class="sxs-lookup"><span data-stu-id="b20b2-194">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="b20b2-195">Ha érdekli példák megtekintésével, olvassa el ebben a cikkben [adatok szolgáltatás OData leképezési példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee példakód és kód szintaxis és a környezetben.</span><span class="sxs-lookup"><span data-stu-id="b20b2-195">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee sample code and understand code syntax and context.</span></span>
* <span data-ttu-id="b20b2-196">Ebben a cikkben egy adatszolgáltatás toohello Azure piactér-közzététel elérési előírt tooreturn toohello [adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="b20b2-196">tooreturn toohello prescribed path for publishing a Data Service toohello Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

