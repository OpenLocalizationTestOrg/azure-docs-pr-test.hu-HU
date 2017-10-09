---
title: "az Azure Data Factory aaaFile és tömörítési formátum |} Microsoft Docs"
description: "Tudnivalók az Azure Data Factory által támogatott fájlformátumok hello."
keywords: "Blobadatok, az azure blob másolása"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 9d40517b059fc533776bcc088db8c531ee5b003d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a><span data-ttu-id="5566d-104">Azure Data Factory által támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="5566d-104">File and compression formats supported by Azure Data Factory</span></span>
<span data-ttu-id="5566d-105">*Ez a témakör a következő összekötők toohello vonatkozik: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [fájlrendszer](data-factory-onprem-file-system-connector.md), [ FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), és [SFTP](data-factory-sftp-connector.md).*</span><span class="sxs-lookup"><span data-stu-id="5566d-105">*This topic applies toohello following connectors: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [File System](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), and [SFTP](data-factory-sftp-connector.md).*</span></span>

<span data-ttu-id="5566d-106">Az Azure Data Factory hello a következő formátumban fájltípusokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="5566d-106">Azure Data Factory supports hello following file format types:</span></span>

* [<span data-ttu-id="5566d-107">Szöveges formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-107">Text format</span></span>](#text-format)
* [<span data-ttu-id="5566d-108">JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="5566d-108">JSON format</span></span>](#json-format)
* [<span data-ttu-id="5566d-109">Az Avro formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-109">Avro format</span></span>](#avro-format)
* [<span data-ttu-id="5566d-110">ORC formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-110">ORC format</span></span>](#orc-format)
* [<span data-ttu-id="5566d-111">Parquet formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-111">Parquet format</span></span>](#parquet-format)

## <a name="text-format"></a><span data-ttu-id="5566d-112">Szöveges formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-112">Text format</span></span>
<span data-ttu-id="5566d-113">Ha szeretné, hogy ki egy szövegfájlból tooread vagy tooa szöveges fájl írása, állítsa be a hello `type` hello tulajdonság `format` hello dataset része túl**szöveges**.</span><span class="sxs-lookup"><span data-stu-id="5566d-113">If you want tooread from a text file or write tooa text file, set hello `type` property in hello `format` section of hello dataset too**TextFormat**.</span></span> <span data-ttu-id="5566d-114">Azt is megadhatja, hello következő **választható** hello tulajdonságok `format` szakasz.</span><span class="sxs-lookup"><span data-stu-id="5566d-114">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="5566d-115">Lásd: [szöveges példa](#textformat-example) hogyan szakasz tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="5566d-115">See [TextFormat example](#textformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="5566d-116">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5566d-116">Property</span></span> | <span data-ttu-id="5566d-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="5566d-117">Description</span></span> | <span data-ttu-id="5566d-118">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="5566d-118">Allowed values</span></span> | <span data-ttu-id="5566d-119">Kötelező</span><span class="sxs-lookup"><span data-stu-id="5566d-119">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5566d-120">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="5566d-120">columnDelimiter</span></span> |<span data-ttu-id="5566d-121">hello karakter egy fájlban használt tooseparate oszlopok.</span><span class="sxs-lookup"><span data-stu-id="5566d-121">hello character used tooseparate columns in a file.</span></span> <span data-ttu-id="5566d-122">Érdemes lehet toouse ritka nem nyomtatható karakter lehet, amely valószínűleg nem lehetséges, hogy létezik-e az adatok.</span><span class="sxs-lookup"><span data-stu-id="5566d-122">You can consider toouse a rare unprintable char that may not likely exists in your data.</span></span> <span data-ttu-id="5566d-123">Adja meg például a "\u0001", ami indítsa el a fejléc (SOH) jelöli.</span><span class="sxs-lookup"><span data-stu-id="5566d-123">For example, specify "\u0001", which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="5566d-124">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5566d-124">Only one character is allowed.</span></span> <span data-ttu-id="5566d-125">Hello **alapértelmezett** értéke **vesszővel (",")**.</span><span class="sxs-lookup"><span data-stu-id="5566d-125">hello **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="5566d-126">toouse Unicode-karakter, tekintse meg a túl[Unicode-karaktereket](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget megfelelő kód hello azt.</span><span class="sxs-lookup"><span data-stu-id="5566d-126">toouse a Unicode character, refer too[Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello corresponding code for it.</span></span> |<span data-ttu-id="5566d-127">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-127">No</span></span> |
| <span data-ttu-id="5566d-128">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="5566d-128">rowDelimiter</span></span> |<span data-ttu-id="5566d-129">hello karakter egy fájlban használt tooseparate sorokat.</span><span class="sxs-lookup"><span data-stu-id="5566d-129">hello character used tooseparate rows in a file.</span></span> |<span data-ttu-id="5566d-130">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5566d-130">Only one character is allowed.</span></span> <span data-ttu-id="5566d-131">Hello **alapértelmezett** értéke bármelyik hello követő értékek olvasáskor: **["\r\n", "\r", "\n"]** és **"\r\n"** írás.</span><span class="sxs-lookup"><span data-stu-id="5566d-131">hello **default** value is any of hello following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="5566d-132">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-132">No</span></span> |
| <span data-ttu-id="5566d-133">escapeChar</span><span class="sxs-lookup"><span data-stu-id="5566d-133">escapeChar</span></span> |<span data-ttu-id="5566d-134">hello speciális karaktert használt oszlop elválasztó tooescape hello bemeneti fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="5566d-134">hello special character used tooescape a column delimiter in hello content of input file.</span></span> <br/><br/><span data-ttu-id="5566d-135">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="5566d-135">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="5566d-136">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5566d-136">Only one character is allowed.</span></span> <span data-ttu-id="5566d-137">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="5566d-137">No default value.</span></span> <br/><br/><span data-ttu-id="5566d-138">Példa: Ha vesszővel (', '), hello oszlop elválasztó, de szeretné toohave hello vesszővel karaktert hello szövegként (Példa: "Hello, world"), adja meg a "$" hello escape-karakter, és a karakterlánc "Hello$, world" hello forrásból.</span><span class="sxs-lookup"><span data-stu-id="5566d-138">Example: if you have comma (',') as hello column delimiter but you want toohave hello comma character in hello text (example: "Hello, world"), you can define ‘$’ as hello escape character and use string "Hello$, world" in hello source.</span></span> |<span data-ttu-id="5566d-139">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-139">No</span></span> |
| <span data-ttu-id="5566d-140">quoteChar</span><span class="sxs-lookup"><span data-stu-id="5566d-140">quoteChar</span></span> |<span data-ttu-id="5566d-141">hello kitöltéshez használandó tooquote karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="5566d-141">hello character used tooquote a string value.</span></span> <span data-ttu-id="5566d-142">hello oszlop- és sorfejlécek határolójelek belül hello idézőjel karakterek hello karakterláncértéket részeként kezelik.</span><span class="sxs-lookup"><span data-stu-id="5566d-142">hello column and row delimiters inside hello quote characters would be treated as part of hello string value.</span></span> <span data-ttu-id="5566d-143">Ez a tulajdonság akkor alkalmazható tooboth bemeneti és kimeneti adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="5566d-143">This property is applicable tooboth input and output datasets.</span></span><br/><br/><span data-ttu-id="5566d-144">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="5566d-144">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="5566d-145">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5566d-145">Only one character is allowed.</span></span> <span data-ttu-id="5566d-146">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="5566d-146">No default value.</span></span> <br/><br/><span data-ttu-id="5566d-147">Például, ha vesszővel (', '), hello oszlop elválasztó, de szeretné toohave vesszővel karaktert hello szöveges (Példa: < Hello, world >), definiálhat "(idézőjelek nélkül), hello idézőjel, és használja a hello karakterlánc"Hello, world"hello forrás.</span><span class="sxs-lookup"><span data-stu-id="5566d-147">For example, if you have comma (',') as hello column delimiter but you want toohave comma character in hello text (example: <Hello, world>), you can define " (double quote) as hello quote character and use hello string "Hello, world" in hello source.</span></span> |<span data-ttu-id="5566d-148">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-148">No</span></span> |
| <span data-ttu-id="5566d-149">nullValue</span><span class="sxs-lookup"><span data-stu-id="5566d-149">nullValue</span></span> |<span data-ttu-id="5566d-150">Egy vagy több karaktert használt toorepresent null értékű.</span><span class="sxs-lookup"><span data-stu-id="5566d-150">One or more characters used toorepresent a null value.</span></span> |<span data-ttu-id="5566d-151">Egy vagy több karakter.</span><span class="sxs-lookup"><span data-stu-id="5566d-151">One or more characters.</span></span> <span data-ttu-id="5566d-152">Hello **alapértelmezett** értékei **"\N" és "NULL"** olvasáskor és **"\N"** írás.</span><span class="sxs-lookup"><span data-stu-id="5566d-152">hello **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="5566d-153">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-153">No</span></span> |
| <span data-ttu-id="5566d-154">encodingName</span><span class="sxs-lookup"><span data-stu-id="5566d-154">encodingName</span></span> |<span data-ttu-id="5566d-155">Adja meg a hello kódolási név.</span><span class="sxs-lookup"><span data-stu-id="5566d-155">Specify hello encoding name.</span></span> |<span data-ttu-id="5566d-156">Egy érvényes kódolási név.</span><span class="sxs-lookup"><span data-stu-id="5566d-156">A valid encoding name.</span></span> <span data-ttu-id="5566d-157">Lásd az [Encoding.EncodingName tulajdonságot](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566d-157">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="5566d-158">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="5566d-158">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="5566d-159">Hello **alapértelmezett** értéke **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="5566d-159">hello **default** value is **UTF-8**.</span></span> |<span data-ttu-id="5566d-160">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-160">No</span></span> |
| <span data-ttu-id="5566d-161">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="5566d-161">firstRowAsHeader</span></span> |<span data-ttu-id="5566d-162">Meghatározza, hogy tooconsider hello első sor adatai alkossák fejléc.</span><span class="sxs-lookup"><span data-stu-id="5566d-162">Specifies whether tooconsider hello first row as a header.</span></span> <span data-ttu-id="5566d-163">A bemeneti adatkészletek első sorát a Data Factory fejlécként olvassa be.</span><span class="sxs-lookup"><span data-stu-id="5566d-163">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="5566d-164">A kimeneti adatkészletek első sorát a Data Factory fejlécként írja ki.</span><span class="sxs-lookup"><span data-stu-id="5566d-164">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="5566d-165">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="5566d-165">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="5566d-166">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="5566d-166">True</span></span><br/><span data-ttu-id="5566d-167"><b>False (alapértelmezett)</b></span><span class="sxs-lookup"><span data-stu-id="5566d-167"><b>False (default)</b></span></span> |<span data-ttu-id="5566d-168">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-168">No</span></span> |
| <span data-ttu-id="5566d-169">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="5566d-169">skipLineCount</span></span> |<span data-ttu-id="5566d-170">Meghatározza a sorok tooskip hello számát, bemeneti fájlok adatainak olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="5566d-170">Indicates hello number of rows tooskip when reading data from input files.</span></span> <span data-ttu-id="5566d-171">Ha skipLineCount és firstRowAsHeader is meg van adva, hello sorok először kimarad, és majd hello fejléc-információ a hello bemeneti fájl olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="5566d-171">If both skipLineCount and firstRowAsHeader are specified, hello lines are skipped first and then hello header information is read from hello input file.</span></span> <br/><br/><span data-ttu-id="5566d-172">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="5566d-172">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="5566d-173">Egész szám</span><span class="sxs-lookup"><span data-stu-id="5566d-173">Integer</span></span> |<span data-ttu-id="5566d-174">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-174">No</span></span> |
| <span data-ttu-id="5566d-175">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="5566d-175">treatEmptyAsNull</span></span> |<span data-ttu-id="5566d-176">Megadja, hogy a tootreat null vagy üres karakterlánc, a rendszer null értéket, amikor a bemeneti fájl adatainak olvasása.</span><span class="sxs-lookup"><span data-stu-id="5566d-176">Specifies whether tootreat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="5566d-177">**True (alapértelmezett)**</span><span class="sxs-lookup"><span data-stu-id="5566d-177">**True (default)**</span></span><br/><span data-ttu-id="5566d-178">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="5566d-178">False</span></span> |<span data-ttu-id="5566d-179">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-179">No</span></span> |

### <a name="textformat-example"></a><span data-ttu-id="5566d-180">A TextFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="5566d-180">TextFormat example</span></span>
<span data-ttu-id="5566d-181">A következő adatkészlet JSON-definícióból hello hello választható tulajdonságok vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="5566d-181">In hello following JSON definition for a dataset, some of hello optional properties are specified.</span></span>

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

<span data-ttu-id="5566d-182">toouse egy `escapeChar` helyett `quoteChar`, cserélje le a hello sor `quoteChar` a következő escapeChar hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-182">toouse an `escapeChar` instead of `quoteChar`, replace hello line with `quoteChar` with hello following escapeChar:</span></span>

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="5566d-183">A firstRowAsHeader és a skipLineCount használatára vonatkozó forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="5566d-183">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="5566d-184">Nem fájl forrás tooa szöveges fájlból másolja át, és szeretné tooadd hello séma metaadatokat tartalmazó fejléc sor (pl.: SQL-séma).</span><span class="sxs-lookup"><span data-stu-id="5566d-184">You are copying from a non-file source tooa text file and would like tooadd a header line containing hello schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="5566d-185">Adja meg `firstRowAsHeader` hello kimeneti adatkészlet ebben a forgatókönyvben a true.</span><span class="sxs-lookup"><span data-stu-id="5566d-185">Specify `firstRowAsHeader` as true in hello output dataset for this scenario.</span></span>
* <span data-ttu-id="5566d-186">A fejléc sor tooa nem fájl fogadó tartalmazó szöveges fájlból másolja át, és szeretné, hogy a sor toodrop.</span><span class="sxs-lookup"><span data-stu-id="5566d-186">You are copying from a text file containing a header line tooa non-file sink and would like toodrop that line.</span></span> <span data-ttu-id="5566d-187">Adja meg `firstRowAsHeader` hello bemeneti adatkészletet, igaz.</span><span class="sxs-lookup"><span data-stu-id="5566d-187">Specify `firstRowAsHeader` as true in hello input dataset.</span></span>
* <span data-ttu-id="5566d-188">Másolja át egy szövegfájlból, és szeretné, hogy tooskip néhány sor hello elején, adatok vagy a fejléc adatokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="5566d-188">You are copying from a text file and want tooskip a few lines at hello beginning that contain no data or header information.</span></span> <span data-ttu-id="5566d-189">Adja meg `skipLineCount` tooindicate hello száma sorok toobe kihagyva.</span><span class="sxs-lookup"><span data-stu-id="5566d-189">Specify `skipLineCount` tooindicate hello number of lines toobe skipped.</span></span> <span data-ttu-id="5566d-190">Ha hello rest hello fájl tartalmazza a fejléc, azt is megadhatja, `firstRowAsHeader`.</span><span class="sxs-lookup"><span data-stu-id="5566d-190">If hello rest of hello file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="5566d-191">Ha mindkét `skipLineCount` és `firstRowAsHeader` megadott, hello sorok először kimarad, és ezután hello fejléc-információ a hello bemeneti fájl olvasásának</span><span class="sxs-lookup"><span data-stu-id="5566d-191">If both `skipLineCount` and `firstRowAsHeader` are specified, hello lines are skipped first and then hello header information is read from hello input file</span></span>

## <a name="json-format"></a><span data-ttu-id="5566d-192">JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="5566d-192">JSON format</span></span>
<span data-ttu-id="5566d-193">túl**importálási/exportálási egy JSON-fájl,-be/Azure Cosmos DB van**, hello lásd: [importálási/exportálási JSON-dokumentumok](data-factory-azure-documentdb-connector.md#importexport-json-documents) szakasz [helyezi át az adatokat az Azure Cosmos DB](data-factory-azure-documentdb-connector.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="5566d-193">too**import/export a JSON file as-is into/from Azure Cosmos DB**, hello see [Import/export JSON documents](data-factory-azure-documentdb-connector.md#importexport-json-documents) section in [Move data to/from Azure Cosmos DB](data-factory-azure-documentdb-connector.md) article.</span></span>

<span data-ttu-id="5566d-194">Ha szeretné, hogy tooparse hello JSON-fájlokat vagy hello adatírás JSON formátumban, állítsa be a hello `type` hello tulajdonság `format` túl szakasz**JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="5566d-194">If you want tooparse hello JSON files or write hello data in JSON format, set hello `type` property in hello `format` section too**JsonFormat**.</span></span> <span data-ttu-id="5566d-195">Azt is megadhatja, hello következő **választható** hello tulajdonságok `format` szakasz.</span><span class="sxs-lookup"><span data-stu-id="5566d-195">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="5566d-196">Lásd: [JsonFormat példa](#jsonformat-example) hogyan szakasz tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="5566d-196">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="5566d-197">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5566d-197">Property</span></span> | <span data-ttu-id="5566d-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="5566d-198">Description</span></span> | <span data-ttu-id="5566d-199">Kötelező</span><span class="sxs-lookup"><span data-stu-id="5566d-199">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5566d-200">filePattern</span><span class="sxs-lookup"><span data-stu-id="5566d-200">filePattern</span></span> |<span data-ttu-id="5566d-201">Minden JSON-fájlban tárolt adatok hello mintát jelzi.</span><span class="sxs-lookup"><span data-stu-id="5566d-201">Indicate hello pattern of data stored in each JSON file.</span></span> <span data-ttu-id="5566d-202">Az engedélyezett értékek a következők: **setOfObjects** és **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="5566d-202">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="5566d-203">Hello **alapértelmezett** értéke **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="5566d-203">hello **default** value is **setOfObjects**.</span></span> <span data-ttu-id="5566d-204">A mintákkal kapcsolatban lásd a [JSON-fájlminták](#json-file-patterns) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="5566d-204">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="5566d-205">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-205">No</span></span> |
| <span data-ttu-id="5566d-206">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="5566d-206">jsonNodeReference</span></span> | <span data-ttu-id="5566d-207">Ha tooiterate és az adatok kinyerése egy tömb belül hello objektumok mezőben a hello azonos mintát, adja meg, hogy tömb hello JSON elérési útját.</span><span class="sxs-lookup"><span data-stu-id="5566d-207">If you want tooiterate and extract data from hello objects inside an array field with hello same pattern, specify hello JSON path of that array.</span></span> <span data-ttu-id="5566d-208">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="5566d-208">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="5566d-209">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-209">No</span></span> |
| <span data-ttu-id="5566d-210">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="5566d-210">jsonPathDefinition</span></span> | <span data-ttu-id="5566d-211">Adja meg a hello JSON elérési út minden oszlop leképezése egyéni oszlopnévvel (kisbetűvel kezdődik).</span><span class="sxs-lookup"><span data-stu-id="5566d-211">Specify hello JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="5566d-212">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat, és ki tud nyerni adatokat objektumokból vagy tömbökből.</span><span class="sxs-lookup"><span data-stu-id="5566d-212">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="5566d-213">A mezők a gyökérszintű objektum esetében indítsa el a legfelső szintű $; belül hello tömb által kiválasztott mező `jsonNodeReference` tulajdonság, hello tömbelem elindítását.</span><span class="sxs-lookup"><span data-stu-id="5566d-213">For fields under root object, start with root $; for fields inside hello array chosen by `jsonNodeReference` property, start from hello array element.</span></span> <span data-ttu-id="5566d-214">Lásd: [JsonFormat példa](#jsonformat-example) hogyan szakasz tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="5566d-214">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span> | <span data-ttu-id="5566d-215">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-215">No</span></span> |
| <span data-ttu-id="5566d-216">encodingName</span><span class="sxs-lookup"><span data-stu-id="5566d-216">encodingName</span></span> |<span data-ttu-id="5566d-217">Adja meg a hello kódolási név.</span><span class="sxs-lookup"><span data-stu-id="5566d-217">Specify hello encoding name.</span></span> <span data-ttu-id="5566d-218">Érvényes kódolási nevek hello listájáért lásd: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5566d-218">For hello list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="5566d-219">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="5566d-219">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="5566d-220">Hello **alapértelmezett** értéke: **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="5566d-220">hello **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="5566d-221">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-221">No</span></span> |
| <span data-ttu-id="5566d-222">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="5566d-222">nestingSeparator</span></span> |<span data-ttu-id="5566d-223">Az karakter, amely használt tooseparate beágyazási szinttel.</span><span class="sxs-lookup"><span data-stu-id="5566d-223">Character that is used tooseparate nesting levels.</span></span> <span data-ttu-id="5566d-224">hello alapértelmezett érték "." (pont).</span><span class="sxs-lookup"><span data-stu-id="5566d-224">hello default value is '.' (dot).</span></span> |<span data-ttu-id="5566d-225">Nem</span><span class="sxs-lookup"><span data-stu-id="5566d-225">No</span></span> |

### <a name="json-file-patterns"></a><span data-ttu-id="5566d-226">JSON-fájlminták</span><span class="sxs-lookup"><span data-stu-id="5566d-226">JSON file patterns</span></span>

<span data-ttu-id="5566d-227">Másolási tevékenység során tudja értelmezni a következő mintákat keressen az JSON-fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-227">Copy activity can parse hello following patterns of JSON files:</span></span>

- <span data-ttu-id="5566d-228">**I. típus: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="5566d-228">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="5566d-229">Minden fájl egyetlen objektumot, illetve több, sorokkal határolt/összefűzött objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5566d-229">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="5566d-230">Ha ezt a lehetőséget választja egy kimeneti adatkészletben, a másolási tevékenység egyetlen JSON-fájlt állít elő, soronként egy objektummal (sorokkal határolt).</span><span class="sxs-lookup"><span data-stu-id="5566d-230">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="5566d-231">**példa egy objektumot tartalmazó JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="5566d-231">**single object JSON example**</span></span>

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * <span data-ttu-id="5566d-232">**példa sorokkal határolt JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="5566d-232">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="5566d-233">**példa összefűzött JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="5566d-233">**concatenated JSON example**</span></span>

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- <span data-ttu-id="5566d-234">**II. típus: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="5566d-234">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="5566d-235">Minden fájl objektumok egy tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5566d-235">Each file contains an array of objects.</span></span>

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a><span data-ttu-id="5566d-236">A JsonFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="5566d-236">JsonFormat example</span></span>

<span data-ttu-id="5566d-237">**1. eset: Adatok másolása JSON-fájlokból**</span><span class="sxs-lookup"><span data-stu-id="5566d-237">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="5566d-238">Tekintse meg a következő két minta, amikor az adatok másolása JSON-fájlok hello.</span><span class="sxs-lookup"><span data-stu-id="5566d-238">See hello following two samples when copying data from JSON files.</span></span> <span data-ttu-id="5566d-239">általános hello toonote mutat:</span><span class="sxs-lookup"><span data-stu-id="5566d-239">hello generic points toonote:</span></span>

<span data-ttu-id="5566d-240">**1. példa: adatok kigyűjtése objektumból és tömbből**</span><span class="sxs-lookup"><span data-stu-id="5566d-240">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="5566d-241">Ez a minta elvárt egy legfelső szintű JSON-objektum leképezi a táblázatos eredmény toosingle rekord.</span><span class="sxs-lookup"><span data-stu-id="5566d-241">In this sample, you expect one root JSON object maps toosingle record in tabular result.</span></span> <span data-ttu-id="5566d-242">Ha egy JSON-fájl a következő tartalmat hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-242">If you have a JSON file with hello following content:</span></span>  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagmentProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
<span data-ttu-id="5566d-243">és azt szeretné, hogy azt egy Azure SQL táblázatba hello alábbi formázása, kinyeri az adatokat a objektumok és a tömb toocopy:</span><span class="sxs-lookup"><span data-stu-id="5566d-243">and you want toocopy it into an Azure SQL table in hello following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="5566d-244">id</span><span class="sxs-lookup"><span data-stu-id="5566d-244">id</span></span> | <span data-ttu-id="5566d-245">deviceType</span><span class="sxs-lookup"><span data-stu-id="5566d-245">deviceType</span></span> | <span data-ttu-id="5566d-246">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="5566d-246">targetResourceType</span></span> | <span data-ttu-id="5566d-247">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="5566d-247">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="5566d-248">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="5566d-248">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="5566d-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="5566d-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="5566d-250">PC</span><span class="sxs-lookup"><span data-stu-id="5566d-250">PC</span></span> | <span data-ttu-id="5566d-251">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="5566d-251">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="5566d-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="5566d-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="5566d-253">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="5566d-253">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="5566d-254">bemeneti adatkészlet hello **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban).</span><span class="sxs-lookup"><span data-stu-id="5566d-254">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="5566d-255">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="5566d-255">More specifically:</span></span>

- <span data-ttu-id="5566d-256">`structure`szakasz határozza meg a testreszabott hello oszlopneveket és a megfelelő adattípus hello tootabular adatok konvertálásakor.</span><span class="sxs-lookup"><span data-stu-id="5566d-256">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="5566d-257">Ez a szakasz **választható** kivéve toodo oszlopleképezés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5566d-257">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="5566d-258">Lásd: [forrás adatkészlet oszlopok toodestination dataset oszlop leképezése](data-factory-map-columns.md) című szakaszban talál további információt.</span><span class="sxs-lookup"><span data-stu-id="5566d-258">See [Map source dataset columns toodestination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="5566d-259">`jsonPathDefinition`az egyes oszlopok, ahol tooextract hello adatait jelző hello JSON elérési út megadása</span><span class="sxs-lookup"><span data-stu-id="5566d-259">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="5566d-260">toocopy adatokat egy olyan tömbből, használhat **tömb [x] .property** hello hello x-edik objektumot, vagy a tulajdonság megadott értéke tooextract használható  **tömb [*] .property** toofind hello értéket az összes ilyen tulajdonságot tartalmazó objektumtól.</span><span class="sxs-lookup"><span data-stu-id="5566d-260">toocopy data from array, you can use **array[x].property** tooextract value of hello given property from hello xth object, or you can use **array[*].property** toofind hello value from any object containing such property.</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagmentProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagmentProcessRunId": "$.context.custom.dimensions[1].ResourceManagmentProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

<span data-ttu-id="5566d-261">**2. példa: közötti több objektumnak azonos mintát egy olyan tömbből, hello alkalmazása**</span><span class="sxs-lookup"><span data-stu-id="5566d-261">**Sample 2: cross apply multiple objects with hello same pattern from array**</span></span>

<span data-ttu-id="5566d-262">Ez a minta elvárt tootransform egy legfelső szintű JSON-objektum táblázatos eredményben több bejegyzésekbe.</span><span class="sxs-lookup"><span data-stu-id="5566d-262">In this sample, you expect tootransform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="5566d-263">Ha egy JSON-fájl a következő tartalmat hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-263">If you have a JSON file with hello following content:</span></span>  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
<span data-ttu-id="5566d-264">és szeretné, hogy azt egy Azure SQL táblázatba hello alábbi formázása, hello hello tömbben található adatok közül egybesimítását toocopy Keresztillesztés információval hello közös legfelső szintű:</span><span class="sxs-lookup"><span data-stu-id="5566d-264">and you want toocopy it into an Azure SQL table in hello following format, by flattening hello data inside hello array and cross join with hello common root info:</span></span>

| <span data-ttu-id="5566d-265">ordernumber</span><span class="sxs-lookup"><span data-stu-id="5566d-265">ordernumber</span></span> | <span data-ttu-id="5566d-266">orderdate</span><span class="sxs-lookup"><span data-stu-id="5566d-266">orderdate</span></span> | <span data-ttu-id="5566d-267">order_pd</span><span class="sxs-lookup"><span data-stu-id="5566d-267">order_pd</span></span> | <span data-ttu-id="5566d-268">order_price</span><span class="sxs-lookup"><span data-stu-id="5566d-268">order_price</span></span> | <span data-ttu-id="5566d-269">city</span><span class="sxs-lookup"><span data-stu-id="5566d-269">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="5566d-270">01</span><span class="sxs-lookup"><span data-stu-id="5566d-270">01</span></span> | <span data-ttu-id="5566d-271">20170122</span><span class="sxs-lookup"><span data-stu-id="5566d-271">20170122</span></span> | <span data-ttu-id="5566d-272">P1</span><span class="sxs-lookup"><span data-stu-id="5566d-272">P1</span></span> | <span data-ttu-id="5566d-273">23</span><span class="sxs-lookup"><span data-stu-id="5566d-273">23</span></span> | <span data-ttu-id="5566d-274">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="5566d-274">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="5566d-275">01</span><span class="sxs-lookup"><span data-stu-id="5566d-275">01</span></span> | <span data-ttu-id="5566d-276">20170122</span><span class="sxs-lookup"><span data-stu-id="5566d-276">20170122</span></span> | <span data-ttu-id="5566d-277">P2</span><span class="sxs-lookup"><span data-stu-id="5566d-277">P2</span></span> | <span data-ttu-id="5566d-278">13</span><span class="sxs-lookup"><span data-stu-id="5566d-278">13</span></span> | <span data-ttu-id="5566d-279">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="5566d-279">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="5566d-280">01</span><span class="sxs-lookup"><span data-stu-id="5566d-280">01</span></span> | <span data-ttu-id="5566d-281">20170122</span><span class="sxs-lookup"><span data-stu-id="5566d-281">20170122</span></span> | <span data-ttu-id="5566d-282">P3</span><span class="sxs-lookup"><span data-stu-id="5566d-282">P3</span></span> | <span data-ttu-id="5566d-283">231</span><span class="sxs-lookup"><span data-stu-id="5566d-283">231</span></span> | <span data-ttu-id="5566d-284">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="5566d-284">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="5566d-285">bemeneti adatkészlet hello **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban).</span><span class="sxs-lookup"><span data-stu-id="5566d-285">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="5566d-286">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="5566d-286">More specifically:</span></span>

- <span data-ttu-id="5566d-287">`structure`szakasz határozza meg a testreszabott hello oszlopneveket és a megfelelő adattípus hello tootabular adatok konvertálásakor.</span><span class="sxs-lookup"><span data-stu-id="5566d-287">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="5566d-288">Ez a szakasz **választható** kivéve toodo oszlopleképezés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5566d-288">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="5566d-289">Lásd: [forrás adatkészlet oszlopok toodestination dataset oszlop leképezése](data-factory-map-columns.md) című szakaszban talál további információt.</span><span class="sxs-lookup"><span data-stu-id="5566d-289">See [Map source dataset columns toodestination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="5566d-290">`jsonNodeReference`azt jelzi, ugyanaz a mintát hello hello objektumok tooiterate és hogyan nyerhet ki adatokat **tömb** orderlines.</span><span class="sxs-lookup"><span data-stu-id="5566d-290">`jsonNodeReference` indicates tooiterate and extract data from hello objects with hello same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="5566d-291">`jsonPathDefinition`az egyes oszlopok, ahol tooextract hello adatait jelző hello JSON elérési út megadása</span><span class="sxs-lookup"><span data-stu-id="5566d-291">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="5566d-292">Ebben a példában a "ordernumber", "orderdate oszlopra" és "város" vannak a legfelső szintű objektum kezdve "$.", amíg a "order_pd" és "order_price" meg van határozva, származó hello tömbelem nélkül "$." elérési úttal rendelkező JSON-útvonalhoz.</span><span class="sxs-lookup"><span data-stu-id="5566d-292">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from hello array element without "$.".</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

<span data-ttu-id="5566d-293">**Vegye figyelembe a következő pontok hello:**</span><span class="sxs-lookup"><span data-stu-id="5566d-293">**Note hello following points:**</span></span>

* <span data-ttu-id="5566d-294">Ha hello `structure` és `jsonPathDefinition` nincsenek megadva itt: hello adat-előállító adatkészlet hello másolási tevékenység észleli hello hello első objektum sémáját, és egybesimítására hello teljes objektum.</span><span class="sxs-lookup"><span data-stu-id="5566d-294">If hello `structure` and `jsonPathDefinition` are not defined in hello Data Factory dataset, hello Copy Activity detects hello schema from hello first object and flatten hello whole object.</span></span>
* <span data-ttu-id="5566d-295">Ha hello JSON-bevitelben tömb, alapértelmezés szerint hello másolási tevékenység alakítja hello teljes tömb érték karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="5566d-295">If hello JSON input has an array, by default hello Copy Activity converts hello entire array value into a string.</span></span> <span data-ttu-id="5566d-296">Választhat tooextract adatok használatával `jsonNodeReference` és/vagy `jsonPathDefinition`, vagy hagyja ki a nem ad meg a legyen `jsonPathDefinition`.</span><span class="sxs-lookup"><span data-stu-id="5566d-296">You can choose tooextract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="5566d-297">Ha duplikált nevek: hello ugyanazon a szinten, szerzi hello legutóbb hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="5566d-297">If there are duplicate names at hello same level, hello Copy Activity picks hello last one.</span></span>
* <span data-ttu-id="5566d-298">A tulajdonságnevek megkülönböztetik a kis- és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="5566d-298">Property names are case-sensitive.</span></span> <span data-ttu-id="5566d-299">A rendszer két azonos nevű, de eltérő kis- és nagybetűket tartalmazó tulajdonságot két különálló tulajdonságként kezel.</span><span class="sxs-lookup"><span data-stu-id="5566d-299">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="5566d-300">**2. eset: Adatok tooJSON fájl írása**</span><span class="sxs-lookup"><span data-stu-id="5566d-300">**Case 2: Writing data tooJSON file**</span></span>

<span data-ttu-id="5566d-301">Ha rendelkezik a következő táblázat az SQL-adatbázis hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-301">If you have hello following table in SQL Database:</span></span>

| <span data-ttu-id="5566d-302">id</span><span class="sxs-lookup"><span data-stu-id="5566d-302">id</span></span> | <span data-ttu-id="5566d-303">order_date</span><span class="sxs-lookup"><span data-stu-id="5566d-303">order_date</span></span> | <span data-ttu-id="5566d-304">order_price</span><span class="sxs-lookup"><span data-stu-id="5566d-304">order_price</span></span> | <span data-ttu-id="5566d-305">order_by</span><span class="sxs-lookup"><span data-stu-id="5566d-305">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5566d-306">1</span><span class="sxs-lookup"><span data-stu-id="5566d-306">1</span></span> | <span data-ttu-id="5566d-307">20170119</span><span class="sxs-lookup"><span data-stu-id="5566d-307">20170119</span></span> | <span data-ttu-id="5566d-308">2000</span><span class="sxs-lookup"><span data-stu-id="5566d-308">2000</span></span> | <span data-ttu-id="5566d-309">David</span><span class="sxs-lookup"><span data-stu-id="5566d-309">David</span></span> |
| <span data-ttu-id="5566d-310">2</span><span class="sxs-lookup"><span data-stu-id="5566d-310">2</span></span> | <span data-ttu-id="5566d-311">20170120</span><span class="sxs-lookup"><span data-stu-id="5566d-311">20170120</span></span> | <span data-ttu-id="5566d-312">3500</span><span class="sxs-lookup"><span data-stu-id="5566d-312">3500</span></span> | <span data-ttu-id="5566d-313">Patrick</span><span class="sxs-lookup"><span data-stu-id="5566d-313">Patrick</span></span> |
| <span data-ttu-id="5566d-314">3</span><span class="sxs-lookup"><span data-stu-id="5566d-314">3</span></span> | <span data-ttu-id="5566d-315">20170121</span><span class="sxs-lookup"><span data-stu-id="5566d-315">20170121</span></span> | <span data-ttu-id="5566d-316">4000</span><span class="sxs-lookup"><span data-stu-id="5566d-316">4000</span></span> | <span data-ttu-id="5566d-317">Jason</span><span class="sxs-lookup"><span data-stu-id="5566d-317">Jason</span></span> |

<span data-ttu-id="5566d-318">és az egyes rekordokhoz, toowrite tooa JSON-objektumot várt hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="5566d-318">and for each record, you expect toowrite tooa JSON object in hello following format:</span></span>
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

<span data-ttu-id="5566d-319">hello kimeneti adatkészlet **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban).</span><span class="sxs-lookup"><span data-stu-id="5566d-319">hello output dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="5566d-320">Pontosabban `structure` szakaszban definiálja a testre szabott hello tulajdonságnevek célfájlt, `nestingSeparator` (alapértelmezett érték ".") használt tooidentify hello nest réteg hello neve alapján vannak.</span><span class="sxs-lookup"><span data-stu-id="5566d-320">More specifically, `structure` section defines hello customized property names in destination file, `nestingSeparator` (default is ".") are used tooidentify hello nest layer from hello name.</span></span> <span data-ttu-id="5566d-321">Ez a szakasz **választható** kivéve, ha szeretné, hogy toochange hello tulajdonságnév összehasonlítva az forrásoszlop neve, vagy beágyazni hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="5566d-321">This section is **optional** unless you want toochange hello property name comparing with source column name, or nest some of hello properties.</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="avro-format"></a><span data-ttu-id="5566d-322">Az AVRO formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-322">AVRO format</span></span>
<span data-ttu-id="5566d-323">Ha szeretné, hogy tooparse hello az Avro-fájlok vagy az Avro formátum hello adatírás, állítsa be a hello `format` `type` tulajdonság túl**AvroFormat**.</span><span class="sxs-lookup"><span data-stu-id="5566d-323">If you want tooparse hello Avro files or write hello data in Avro format, set hello `format` `type` property too**AvroFormat**.</span></span> <span data-ttu-id="5566d-324">Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell.</span><span class="sxs-lookup"><span data-stu-id="5566d-324">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="5566d-325">Példa:</span><span class="sxs-lookup"><span data-stu-id="5566d-325">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="5566d-326">az Avro-formátumban toouse Hive táblákat, olvassa el a túl[Apache Hive oktatóanyag](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="5566d-326">toouse Avro format in a Hive table, you can refer too[Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="5566d-327">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-327">Note hello following points:</span></span>  

* <span data-ttu-id="5566d-328">[Összetett adattípusú](http://avro.apache.org/docs/current/spec.html#schema_complex) használata nem támogatott (rögzíti, enumerálások, tömbök, térképeket, unióját képezi, és rögzített).</span><span class="sxs-lookup"><span data-stu-id="5566d-328">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions, and fixed).</span></span>

## <a name="orc-format"></a><span data-ttu-id="5566d-329">ORC formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-329">ORC format</span></span>
<span data-ttu-id="5566d-330">Ha szeretné, hogy tooparse hello ORC fájlokat vagy hello adatírás ORC formátumban, állítsa be a hello `format` `type` tulajdonság túl**OrcFormat**.</span><span class="sxs-lookup"><span data-stu-id="5566d-330">If you want tooparse hello ORC files or write hello data in ORC format, set hello `format` `type` property too**OrcFormat**.</span></span> <span data-ttu-id="5566d-331">Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell.</span><span class="sxs-lookup"><span data-stu-id="5566d-331">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="5566d-332">Példa:</span><span class="sxs-lookup"><span data-stu-id="5566d-332">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="5566d-333">Nem másolása ORC fájlokat **,-van** között a helyszíni és felhőalapú adattároló, az átjáró gépen tooinstall hello JRE 8 (Java Runtime Environment) van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5566d-333">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="5566d-334">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="5566d-334">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="5566d-335">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="5566d-335">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="5566d-336">Válassza ki a hello megfelelő egy.</span><span class="sxs-lookup"><span data-stu-id="5566d-336">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="5566d-337">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-337">Note hello following points:</span></span>

* <span data-ttu-id="5566d-338">Az összetett adattípusok nem támogatottak (STRUCT, MAP, LIST, UNION)</span><span class="sxs-lookup"><span data-stu-id="5566d-338">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="5566d-339">Az ORC-fájlok három, [tömörítéshez kapcsolódó beállítással](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/) rendelkeznek: NONE, ZLIB, SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="5566d-339">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="5566d-340">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="5566d-340">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="5566d-341">Használja hello tömörítési kodek hello metaadatok tooread hello adatok van.</span><span class="sxs-lookup"><span data-stu-id="5566d-341">It uses hello compression codec is in hello metadata tooread hello data.</span></span> <span data-ttu-id="5566d-342">Azonban tooan ORC fájl írásakor, adat-előállító úgy dönt, ZLIB, ez utóbbi érték az ORC hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="5566d-342">However, when writing tooan ORC file, Data Factory chooses ZLIB, which is hello default for ORC.</span></span> <span data-ttu-id="5566d-343">Jelenleg nincs lehetőség toooverride ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="5566d-343">Currently, there is no option toooverride this behavior.</span></span>

## <a name="parquet-format"></a><span data-ttu-id="5566d-344">Parquet formátum</span><span class="sxs-lookup"><span data-stu-id="5566d-344">Parquet format</span></span>
<span data-ttu-id="5566d-345">Ha szeretné, hogy tooparse hello Parquet fájlok vagy hello adatírás Parquet formátumban, állítsa be a hello `format` `type` tulajdonság túl**ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5566d-345">If you want tooparse hello Parquet files or write hello data in Parquet format, set hello `format` `type` property too**ParquetFormat**.</span></span> <span data-ttu-id="5566d-346">Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell.</span><span class="sxs-lookup"><span data-stu-id="5566d-346">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="5566d-347">Példa:</span><span class="sxs-lookup"><span data-stu-id="5566d-347">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="5566d-348">Nem Parquet fájlok másolása **,-van** között a helyszíni és felhőalapú adattároló, tooinstall hello JRE 8 (Java Runtime Environment) a átjáró számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5566d-348">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="5566d-349">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="5566d-349">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="5566d-350">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="5566d-350">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="5566d-351">Válassza ki a hello megfelelő egy.</span><span class="sxs-lookup"><span data-stu-id="5566d-351">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="5566d-352">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-352">Note hello following points:</span></span>

* <span data-ttu-id="5566d-353">Az összetett adattípusok nem támogatottak (MAP, LIST)</span><span class="sxs-lookup"><span data-stu-id="5566d-353">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="5566d-354">Parquet fájl rendelkezik-e hello alábbi tömörítési kapcsolatos beállítások: NONE, SNAPPY GZIP és LZO.</span><span class="sxs-lookup"><span data-stu-id="5566d-354">Parquet file has hello following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="5566d-355">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="5566d-355">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="5566d-356">Hello tömörítési kodek hello metaadatok tooread hello adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="5566d-356">It uses hello compression codec in hello metadata tooread hello data.</span></span> <span data-ttu-id="5566d-357">Azonban tooa Parquet fájl írásakor, adat-előállító úgy dönt, SNAPPY, hello alapértelmezett Parquet formátum.</span><span class="sxs-lookup"><span data-stu-id="5566d-357">However, when writing tooa Parquet file, Data Factory chooses SNAPPY, which is hello default for Parquet format.</span></span> <span data-ttu-id="5566d-358">Jelenleg nincs lehetőség toooverride ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="5566d-358">Currently, there is no option toooverride this behavior.</span></span>

## <a name="compression-support"></a><span data-ttu-id="5566d-359">Tömörítés támogatása</span><span class="sxs-lookup"><span data-stu-id="5566d-359">Compression support</span></span>
<span data-ttu-id="5566d-360">Nagy méretű adatkészletekhez feldolgozása okozhat, és a hálózati szűk keresztmetszeteket.</span><span class="sxs-lookup"><span data-stu-id="5566d-360">Processing large data sets can cause I/O and network bottlenecks.</span></span> <span data-ttu-id="5566d-361">Ezért tárolók a tömörített adatok is nem csak felgyorsítsa az adatok átvitele hello hálózaton és lemezterületet spóroljon a, de is kapcsolja az jelentős teljesítményjavulást eredményezhet a big Data típusú adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="5566d-361">Therefore, compressed data in stores can not only speed up data transfer across hello network and save disk space, but also bring significant performance improvements in processing big data.</span></span> <span data-ttu-id="5566d-362">Tömörítés jelenleg a fájlalapú adatok tárolókhoz, például az Azure Blob és helyszíni fájlrendszer használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="5566d-362">Currently, compression is supported for file-based data stores such as Azure Blob or On-premises File System.</span></span>  

<span data-ttu-id="5566d-363">a DataSet adatkészlet használata hello toospecify tömörítése **tömörítés** hello adatkészlet JSON beállítani, mint például a következő hello tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="5566d-363">toospecify compression for a dataset, use hello **compression** property in hello dataset JSON as in hello following example:</span></span>   

```json
{  
    "name": "AzureBlobDataSet",  
    "properties": {  
        "availability": {  
            "frequency": "Day",  
              "interval": 1  
        },  
        "type": "AzureBlob",  
        "linkedServiceName": "StorageLinkedService",  
        "typeProperties": {  
            "fileName": "pagecounts.csv.gz",  
            "folderPath": "compression/file/",  
            "compression": {  
                "type": "GZip",  
                "level": "Optimal"  
            }  
        }  
    }  
}  
```

<span data-ttu-id="5566d-364">Tegyük fel, hogy hello minta-adatkészleteken a másolási tevékenység hello kimenetként használata esetén a hello másolási tevékenység tömöríti hello kimeneti adatai a GZIP kodek optimális arány használatával, és majd tömörített hello adatírás pagecounts.csv.gz hello Azure Blob Storage a fájlba.</span><span class="sxs-lookup"><span data-stu-id="5566d-364">Suppose hello sample dataset is used as hello output of a copy activity, hello copy activity compresses hello output data with GZIP codec using optimal ratio and then write hello compressed data into a file named pagecounts.csv.gz in hello Azure Blob Storage.</span></span>

> [!NOTE]
> <span data-ttu-id="5566d-365">Tömörítési beállítások nem támogatottak a hello adatainak **AvroFormat**, **OrcFormat**, vagy **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5566d-365">Compression settings are not supported for data in hello **AvroFormat**, **OrcFormat**, or **ParquetFormat**.</span></span> <span data-ttu-id="5566d-366">Az alábbi formátumokban fájlok olvasásakor a Data Factory észlel, és hello tömörítési kodek használ hello metaadatai között.</span><span class="sxs-lookup"><span data-stu-id="5566d-366">When reading files in these formats, Data Factory detects and uses hello compression codec in hello metadata.</span></span> <span data-ttu-id="5566d-367">Az alábbi formátumokban toofiles írásakor adat-előállító úgy dönt, hello alapértelmezett tömörítési kodek azt a formátumot.</span><span class="sxs-lookup"><span data-stu-id="5566d-367">When writing toofiles in these formats, Data Factory chooses hello default compression codec for that format.</span></span> <span data-ttu-id="5566d-368">Például ZLIB OrcFormat és a ParquetFormat SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="5566d-368">For example, ZLIB for OrcFormat and SNAPPY for ParquetFormat.</span></span>   

<span data-ttu-id="5566d-369">Hello **tömörítés** szakasz két tulajdonságokkal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="5566d-369">hello **compression** section has two properties:</span></span>  

* <span data-ttu-id="5566d-370">**Típus:** hello tömörítési kodek, amely lehet **GZIP**, **Deflate**, **BZIP2**, vagy **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5566d-370">**Type:** hello compression codec, which can be **GZIP**, **Deflate**, **BZIP2**, or **ZipDeflate**.</span></span>  
* <span data-ttu-id="5566d-371">**Szint:** hello tömörítési arány, amely lehet **Optimal** vagy **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="5566d-371">**Level:** hello compression ratio, which can be **Optimal** or **Fastest**.</span></span>

  * <span data-ttu-id="5566d-372">**Leggyorsabb:** hello tömörítési művelet elvégzését, minél gyorsabban akkor is, ha az eredményül kapott fájl hello nem optimális tömörített.</span><span class="sxs-lookup"><span data-stu-id="5566d-372">**Fastest:** hello compression operation should complete as quickly as possible, even if hello resulting file is not optimally compressed.</span></span>
  * <span data-ttu-id="5566d-373">**Optimális**: hello tömörítési műveletet kell lehet optimális tömörített, akkor is, ha hello művelet egy hosszabb idő toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5566d-373">**Optimal**: hello compression operation should be optimally compressed, even if hello operation takes a longer time toocomplete.</span></span>

    <span data-ttu-id="5566d-374">További információkért lásd: [tömörítési szint](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="5566d-374">For more information, see [Compression Level](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) topic.</span></span>

<span data-ttu-id="5566d-375">Ha a `compression` tulajdonság egy bemeneti adatkészlet JSON, hello csővezeték tömörített adatokat olvasni a hello forrás; és egy kimeneti adatkészlet JSON hello tulajdonság megadása esetén a hello másolási tevékenység írhat-e a tömörített adatok toohello cél.</span><span class="sxs-lookup"><span data-stu-id="5566d-375">When you specify `compression` property in an input dataset JSON, hello pipeline can read compressed data from hello source; and when you specify hello property in an output dataset JSON, hello copy activity can write compressed data toohello destination.</span></span> <span data-ttu-id="5566d-376">Itt van néhány szituáció:</span><span class="sxs-lookup"><span data-stu-id="5566d-376">Here are a few sample scenarios:</span></span>

* <span data-ttu-id="5566d-377">Tömörített GZIP adatokat olvasni az Azure blob, azt kibontani és írási eredménye az tooan Azure SQL-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="5566d-377">Read GZIP compressed data from an Azure blob, decompress it, and write result data tooan Azure SQL database.</span></span> <span data-ttu-id="5566d-378">Megadhatja a bemeneti Azure-Blob adatkészlet hello hello `compression` `type` GZIP JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5566d-378">You define hello input Azure Blob dataset with hello `compression` `type` JSON property as GZIP.</span></span>
* <span data-ttu-id="5566d-379">Adatokat olvasni egy egyszerű szöveges fájlt a helyi fájlrendszer, a tömörítés a GZip formátumban és írási hello tömörített adatok tooan Azure-blobot.</span><span class="sxs-lookup"><span data-stu-id="5566d-379">Read data from a plain-text file from on-premises File System, compress it using GZip format, and write hello compressed data tooan Azure blob.</span></span> <span data-ttu-id="5566d-380">Megadhatja egy kimeneti Azure Blob adatkészlet hello `compression` `type` GZip JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5566d-380">You define an output Azure Blob dataset with hello `compression` `type` JSON property as GZip.</span></span>
* <span data-ttu-id="5566d-381">FTP-kiszolgáló, a .zip fájl olvasása kicsomagolása, tooget hello belül, és azokat a fájlokat az Azure Data Lake Store ártó.</span><span class="sxs-lookup"><span data-stu-id="5566d-381">Read .zip file from FTP server, decompress it tooget hello files inside, and land those files into Azure Data Lake Store.</span></span> <span data-ttu-id="5566d-382">Megadhatja egy bemeneti adatkészlet FTP hello `compression` `type` ZipDeflate JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5566d-382">You define an input FTP dataset with hello `compression` `type` JSON property as ZipDeflate.</span></span>
* <span data-ttu-id="5566d-383">A GZIP-tömörített adatokat olvasni az Azure blob, azt kibontani, BZIP2 használatával tömörítése és írási eredmény adatok tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="5566d-383">Read a GZIP-compressed data from an Azure blob, decompress it, compress it using BZIP2, and write result data tooan Azure blob.</span></span> <span data-ttu-id="5566d-384">Megadhatja a hello bemeneti Azure-Blob adatkészlet `compression` `type` tooGZIP beállítása és a kimeneti adatkészlet hello `compression` `type` tooBZIP2 ebben az esetben állítsa be.</span><span class="sxs-lookup"><span data-stu-id="5566d-384">You define hello input Azure Blob dataset with `compression` `type` set tooGZIP and hello output dataset with `compression` `type` set tooBZIP2 in this case.</span></span>   


## <a name="next-steps"></a><span data-ttu-id="5566d-385">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5566d-385">Next steps</span></span>
<span data-ttu-id="5566d-386">Tekintse meg a következő cikkek Azure Data Factory támogatja a fájlalapú adatok tárolóinak hello:</span><span class="sxs-lookup"><span data-stu-id="5566d-386">See hello following articles for file-based data stores supported by Azure Data Factory:</span></span>

- [<span data-ttu-id="5566d-387">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="5566d-387">Azure Blob Storage</span></span>](data-factory-azure-blob-connector.md)
- [<span data-ttu-id="5566d-388">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="5566d-388">Azure Data Lake Store</span></span>](data-factory-azure-datalake-connector.md)
- [<span data-ttu-id="5566d-389">FTP</span><span class="sxs-lookup"><span data-stu-id="5566d-389">FTP</span></span>](data-factory-ftp-connector.md)
- [<span data-ttu-id="5566d-390">HDFS</span><span class="sxs-lookup"><span data-stu-id="5566d-390">HDFS</span></span>](data-factory-hdfs-connector.md)
- [<span data-ttu-id="5566d-391">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="5566d-391">File System</span></span>](data-factory-onprem-file-system-connector.md)
- [<span data-ttu-id="5566d-392">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="5566d-392">Amazon S3</span></span>](data-factory-amazon-simple-storage-service-connector.md)
