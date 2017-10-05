---
title: "Az Azure Data Factory formátumú és tömörítést |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory által támogatott formátumok."
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
ms.openlocfilehash: f4746e0dd249e417b8077a9bc733d2886daafdf2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a><span data-ttu-id="fbbb8-104">Azure Data Factory által támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="fbbb8-104">File and compression formats supported by Azure Data Factory</span></span>
<span data-ttu-id="fbbb8-105">*Ez a témakör a következő összekötőkre vonatkozik: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [fájlrendszer](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), és [SFTP](data-factory-sftp-connector.md).*</span><span class="sxs-lookup"><span data-stu-id="fbbb8-105">*This topic applies to the following connectors: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [File System](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), and [SFTP](data-factory-sftp-connector.md).*</span></span>

<span data-ttu-id="fbbb8-106">Az Azure Data Factory formátuma a következő fájltípusokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-106">Azure Data Factory supports the following file format types:</span></span>

* [<span data-ttu-id="fbbb8-107">Szöveges formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-107">Text format</span></span>](#text-format)
* [<span data-ttu-id="fbbb8-108">JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="fbbb8-108">JSON format</span></span>](#json-format)
* [<span data-ttu-id="fbbb8-109">Az Avro formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-109">Avro format</span></span>](#avro-format)
* [<span data-ttu-id="fbbb8-110">ORC formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-110">ORC format</span></span>](#orc-format)
* [<span data-ttu-id="fbbb8-111">Parquet formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-111">Parquet format</span></span>](#parquet-format)

## <a name="text-format"></a><span data-ttu-id="fbbb8-112">Szöveges formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-112">Text format</span></span>
<span data-ttu-id="fbbb8-113">Ha szeretne egy szövegfájlt olvasni vagy írni egy szövegfájlba, állítsa be a `type` tulajdonságot a `format` a adatkészlet szakasza **szöveges**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-113">If you want to read from a text file or write to a text file, set the `type` property in the `format` section of the dataset to **TextFormat**.</span></span> <span data-ttu-id="fbbb8-114">Emellett megadhatja a következő **választható** tulajdonságokat a `format` szakaszban.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-114">You can also specify the following **optional** properties in the `format` section.</span></span> <span data-ttu-id="fbbb8-115">A konfigurálással kapcsolatban lásd [A TextFormat használatát bemutató példa](#textformat-example) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-115">See [TextFormat example](#textformat-example) section on how to configure.</span></span>

| <span data-ttu-id="fbbb8-116">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fbbb8-116">Property</span></span> | <span data-ttu-id="fbbb8-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="fbbb8-117">Description</span></span> | <span data-ttu-id="fbbb8-118">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="fbbb8-118">Allowed values</span></span> | <span data-ttu-id="fbbb8-119">Kötelező</span><span class="sxs-lookup"><span data-stu-id="fbbb8-119">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="fbbb8-120">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="fbbb8-120">columnDelimiter</span></span> |<span data-ttu-id="fbbb8-121">A fájlokban az oszlopok elválasztására használt karakter.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-121">The character used to separate columns in a file.</span></span> <span data-ttu-id="fbbb8-122">Érdemes lehet használni a ritka nem nyomtatható karakter lehet, hogy nem valószínűleg már szerepel az adatok.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-122">You can consider to use a rare unprintable char that may not likely exists in your data.</span></span> <span data-ttu-id="fbbb8-123">Adja meg például a "\u0001", ami indítsa el a fejléc (SOH) jelöli.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-123">For example, specify "\u0001", which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="fbbb8-124">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-124">Only one character is allowed.</span></span> <span data-ttu-id="fbbb8-125">Az **alapértelmezett** érték a **vessző (,)**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-125">The **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="fbbb8-126">A Unicode karakterek használatához tekintse meg [Unicode-karaktereket](https://en.wikipedia.org/wiki/List_of_Unicode_characters) a megfelelő kód lekérése.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-126">To use a Unicode character, refer to [Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) to get the corresponding code for it.</span></span> |<span data-ttu-id="fbbb8-127">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-127">No</span></span> |
| <span data-ttu-id="fbbb8-128">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="fbbb8-128">rowDelimiter</span></span> |<span data-ttu-id="fbbb8-129">A fájlokban a sorok elválasztására használt karakter.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-129">The character used to separate rows in a file.</span></span> |<span data-ttu-id="fbbb8-130">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-130">Only one character is allowed.</span></span> <span data-ttu-id="fbbb8-131">Az **alapértelmezett** érték olvasáskor a következő értékek bármelyike: **[„\r\n”, „\r”, „\n”]**, illetve **„\r\n”** írás esetén.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-131">The **default** value is any of the following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="fbbb8-132">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-132">No</span></span> |
| <span data-ttu-id="fbbb8-133">escapeChar</span><span class="sxs-lookup"><span data-stu-id="fbbb8-133">escapeChar</span></span> |<span data-ttu-id="fbbb8-134">Az oszlophatároló feloldására szolgáló speciális karakter a bemeneti fájl tartalmában.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-134">The special character used to escape a column delimiter in the content of input file.</span></span> <br/><br/><span data-ttu-id="fbbb8-135">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-135">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="fbbb8-136">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-136">Only one character is allowed.</span></span> <span data-ttu-id="fbbb8-137">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-137">No default value.</span></span> <br/><br/><span data-ttu-id="fbbb8-138">Ha például vessző (,) az oszlophatároló, de a vessző karaktert szeretné megjeleníteni a szövegben (például: „Helló, világ”), megadhatja a „$” karakter feloldójelként, és a „Helló$, világ” karakterláncot használhatja a forrásban.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-138">Example: if you have comma (',') as the column delimiter but you want to have the comma character in the text (example: "Hello, world"), you can define ‘$’ as the escape character and use string "Hello$, world" in the source.</span></span> |<span data-ttu-id="fbbb8-139">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-139">No</span></span> |
| <span data-ttu-id="fbbb8-140">quoteChar</span><span class="sxs-lookup"><span data-stu-id="fbbb8-140">quoteChar</span></span> |<span data-ttu-id="fbbb8-141">Egy karakterláncérték idézéséhez használt karakter.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-141">The character used to quote a string value.</span></span> <span data-ttu-id="fbbb8-142">Ekkor az idézőjel-karakterek közötti oszlop- és sorhatárolókat a rendszer a karakterláncérték részeként kezeli.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-142">The column and row delimiters inside the quote characters would be treated as part of the string value.</span></span> <span data-ttu-id="fbbb8-143">Ez a tulajdonság a bemeneti és a kimeneti adatkészleteken is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-143">This property is applicable to both input and output datasets.</span></span><br/><br/><span data-ttu-id="fbbb8-144">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-144">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="fbbb8-145">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-145">Only one character is allowed.</span></span> <span data-ttu-id="fbbb8-146">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-146">No default value.</span></span> <br/><br/><span data-ttu-id="fbbb8-147">Ha például vessző (,) az oszlophatároló, de a vessző karaktert szeretné megjeleníteni a szövegben (például: <Helló, világ>), megadhatja a " (angol dupla idézőjel) értéket idézőjel-karakterként, és a "Helló$, világ" karakterláncot használhatja a forrásban.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-147">For example, if you have comma (',') as the column delimiter but you want to have comma character in the text (example: <Hello, world>), you can define " (double quote) as the quote character and use the string "Hello, world" in the source.</span></span> |<span data-ttu-id="fbbb8-148">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-148">No</span></span> |
| <span data-ttu-id="fbbb8-149">nullValue</span><span class="sxs-lookup"><span data-stu-id="fbbb8-149">nullValue</span></span> |<span data-ttu-id="fbbb8-150">A null értéket jelölő egy vagy több karakter.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-150">One or more characters used to represent a null value.</span></span> |<span data-ttu-id="fbbb8-151">Egy vagy több karakter.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-151">One or more characters.</span></span> <span data-ttu-id="fbbb8-152">Az **alapértelmezett** értékek az **„\N” és „NULL”** olvasás, illetve **„\N”** írás esetén.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-152">The **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="fbbb8-153">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-153">No</span></span> |
| <span data-ttu-id="fbbb8-154">encodingName</span><span class="sxs-lookup"><span data-stu-id="fbbb8-154">encodingName</span></span> |<span data-ttu-id="fbbb8-155">A kódolási név megadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-155">Specify the encoding name.</span></span> |<span data-ttu-id="fbbb8-156">Egy érvényes kódolási név.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-156">A valid encoding name.</span></span> <span data-ttu-id="fbbb8-157">Lásd az [Encoding.EncodingName tulajdonságot](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-157">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="fbbb8-158">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-158">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="fbbb8-159">Az **alapértelmezett** érték az **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-159">The **default** value is **UTF-8**.</span></span> |<span data-ttu-id="fbbb8-160">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-160">No</span></span> |
| <span data-ttu-id="fbbb8-161">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="fbbb8-161">firstRowAsHeader</span></span> |<span data-ttu-id="fbbb8-162">Megadja, hogy az első sort fejlécnek kell-e tekinteni.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-162">Specifies whether to consider the first row as a header.</span></span> <span data-ttu-id="fbbb8-163">A bemeneti adatkészletek első sorát a Data Factory fejlécként olvassa be.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-163">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="fbbb8-164">A kimeneti adatkészletek első sorát a Data Factory fejlécként írja ki.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-164">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="fbbb8-165">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-165">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="fbbb8-166">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="fbbb8-166">True</span></span><br/><span data-ttu-id="fbbb8-167"><b>False (alapértelmezett)</b></span><span class="sxs-lookup"><span data-stu-id="fbbb8-167"><b>False (default)</b></span></span> |<span data-ttu-id="fbbb8-168">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-168">No</span></span> |
| <span data-ttu-id="fbbb8-169">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="fbbb8-169">skipLineCount</span></span> |<span data-ttu-id="fbbb8-170">Az adatok bemeneti fájlokból való olvasásakor kihagyandó sorok számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-170">Indicates the number of rows to skip when reading data from input files.</span></span> <span data-ttu-id="fbbb8-171">Ha a skipLineCount és a firstRowAsHeader tulajdonság is meg van adva, a rendszer először kihagyja a sorokat, majd beolvassa a fejléc-információkat a bemeneti fájlból.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-171">If both skipLineCount and firstRowAsHeader are specified, the lines are skipped first and then the header information is read from the input file.</span></span> <br/><br/><span data-ttu-id="fbbb8-172">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-172">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="fbbb8-173">Egész szám</span><span class="sxs-lookup"><span data-stu-id="fbbb8-173">Integer</span></span> |<span data-ttu-id="fbbb8-174">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-174">No</span></span> |
| <span data-ttu-id="fbbb8-175">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="fbbb8-175">treatEmptyAsNull</span></span> |<span data-ttu-id="fbbb8-176">Meghatározza, hogy az adatok bemeneti fájlból történő olvasásakor a null vagy üres értékeket null értékként kell-e kezelni.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-176">Specifies whether to treat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="fbbb8-177">**True (alapértelmezett)**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-177">**True (default)**</span></span><br/><span data-ttu-id="fbbb8-178">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="fbbb8-178">False</span></span> |<span data-ttu-id="fbbb8-179">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-179">No</span></span> |

### <a name="textformat-example"></a><span data-ttu-id="fbbb8-180">A TextFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="fbbb8-180">TextFormat example</span></span>
<span data-ttu-id="fbbb8-181">A következő JSON-definícióban egy adatkészlet egyes nem kötelező tulajdonságok vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-181">In the following JSON definition for a dataset, some of the optional properties are specified.</span></span>

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

<span data-ttu-id="fbbb8-182">`quoteChar` helyett `quoteChar` használatához cserélje le a sort `escapeChar` értékre a következő escapeChar kifejezéssel:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-182">To use an `escapeChar` instead of `quoteChar`, replace the line with `quoteChar` with the following escapeChar:</span></span>

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="fbbb8-183">A firstRowAsHeader és a skipLineCount használatára vonatkozó forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="fbbb8-183">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="fbbb8-184">Egy nem fájlalapú forrásból másol egy szöveges fájlba, és szeretne hozzáadni egy fejlécsort, amely tartalmazza a séma (például SQL-séma) metaadatait.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-184">You are copying from a non-file source to a text file and would like to add a header line containing the schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="fbbb8-185">Ebben a forgatókönyvben adja meg a `firstRowAsHeader` értékét igazként a kimeneti adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-185">Specify `firstRowAsHeader` as true in the output dataset for this scenario.</span></span>
* <span data-ttu-id="fbbb8-186">Egy fejlécsort tartalmazó szöveges fájlból másol egy nem fájlalapú fogadóba, és el szeretné hagyni azt a sort.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-186">You are copying from a text file containing a header line to a non-file sink and would like to drop that line.</span></span> <span data-ttu-id="fbbb8-187">Adja meg a `firstRowAsHeader` értékét igazként a bemeneti adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-187">Specify `firstRowAsHeader` as true in the input dataset.</span></span>
* <span data-ttu-id="fbbb8-188">Egy szöveges fájlból másol, és szeretne kihagyni néhány sort az elejéről, amelyek nem tartalmaznak adatokat vagy fejléc-információkat.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-188">You are copying from a text file and want to skip a few lines at the beginning that contain no data or header information.</span></span> <span data-ttu-id="fbbb8-189">Adja meg a `skipLineCount` értékét a kihagyni kívánt sorok számának jelzéséhez.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-189">Specify `skipLineCount` to indicate the number of lines to be skipped.</span></span> <span data-ttu-id="fbbb8-190">Ha a fájl hátralévő része fejlécsort tartalmaz, a `firstRowAsHeader` is megadható.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-190">If the rest of the file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="fbbb8-191">Ha a `skipLineCount` és a `firstRowAsHeader` is meg van adva, a rendszer először kihagyja a sorokat, majd beolvassa a fejléc-információkat a bemeneti fájlból</span><span class="sxs-lookup"><span data-stu-id="fbbb8-191">If both `skipLineCount` and `firstRowAsHeader` are specified, the lines are skipped first and then the header information is read from the input file</span></span>

## <a name="json-format"></a><span data-ttu-id="fbbb8-192">JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="fbbb8-192">JSON format</span></span>
<span data-ttu-id="fbbb8-193">A **importálási/exportálási egy JSON-fájl,-be/Azure Cosmos DB van**, a további részletekért lásd [importálási/exportálási JSON-dokumentumok](data-factory-azure-documentdb-connector.md#importexport-json-documents) szakasz [helyezi át az adatokat az Azure Cosmos DB](data-factory-azure-documentdb-connector.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-193">To **import/export a JSON file as-is into/from Azure Cosmos DB**, the see [Import/export JSON documents](data-factory-azure-documentdb-connector.md#importexport-json-documents) section in [Move data to/from Azure Cosmos DB](data-factory-azure-documentdb-connector.md) article.</span></span>

<span data-ttu-id="fbbb8-194">Ha szeretne elemezni a JSON-fájlokat, vagy az adatok írása JSON formátumban, állítsa be a `type` tulajdonságot a `format` szakaszban **JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-194">If you want to parse the JSON files or write the data in JSON format, set the `type` property in the `format` section to **JsonFormat**.</span></span> <span data-ttu-id="fbbb8-195">Emellett megadhatja a következő **választható** tulajdonságokat a `format` szakaszban.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-195">You can also specify the following **optional** properties in the `format` section.</span></span> <span data-ttu-id="fbbb8-196">A konfigurálással kapcsolatban lásd [A JsonFormat használatát bemutató példa](#jsonformat-example) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-196">See [JsonFormat example](#jsonformat-example) section on how to configure.</span></span>

| <span data-ttu-id="fbbb8-197">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fbbb8-197">Property</span></span> | <span data-ttu-id="fbbb8-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="fbbb8-198">Description</span></span> | <span data-ttu-id="fbbb8-199">Kötelező</span><span class="sxs-lookup"><span data-stu-id="fbbb8-199">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fbbb8-200">filePattern</span><span class="sxs-lookup"><span data-stu-id="fbbb8-200">filePattern</span></span> |<span data-ttu-id="fbbb8-201">Az egyes JSON-fájlokban tárolt adatok mintáját jelzi.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-201">Indicate the pattern of data stored in each JSON file.</span></span> <span data-ttu-id="fbbb8-202">Az engedélyezett értékek a következők: **setOfObjects** és **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-202">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="fbbb8-203">Az **alapértelmezett** érték a **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-203">The **default** value is **setOfObjects**.</span></span> <span data-ttu-id="fbbb8-204">A mintákkal kapcsolatban lásd a [JSON-fájlminták](#json-file-patterns) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-204">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="fbbb8-205">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-205">No</span></span> |
| <span data-ttu-id="fbbb8-206">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="fbbb8-206">jsonNodeReference</span></span> | <span data-ttu-id="fbbb8-207">Ha egy azonos mintával rendelkező tömbmezőben található objektumokat szeretne iterálni, vagy azokból adatokat kinyerni, adja meg a tömb JSON-útvonalát.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-207">If you want to iterate and extract data from the objects inside an array field with the same pattern, specify the JSON path of that array.</span></span> <span data-ttu-id="fbbb8-208">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-208">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="fbbb8-209">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-209">No</span></span> |
| <span data-ttu-id="fbbb8-210">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="fbbb8-210">jsonPathDefinition</span></span> | <span data-ttu-id="fbbb8-211">Megadja az egyes oszlopmegfeleltetések JSON-útvonalának kifejezését testre szabott oszlopnevekkel (kezdje kisbetűvel).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-211">Specify the JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="fbbb8-212">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat, és ki tud nyerni adatokat objektumokból vagy tömbökből.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-212">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="fbbb8-213">A gyökérobjektum alatti mezők esetében kezdjen a gyökér $ értékkel. A `jsonNodeReference` tulajdonság által kiválasztott tömbben lévő mezők esetében kezdjen a tömbelemmel.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-213">For fields under root object, start with root $; for fields inside the array chosen by `jsonNodeReference` property, start from the array element.</span></span> <span data-ttu-id="fbbb8-214">A konfigurálással kapcsolatban lásd [A JsonFormat használatát bemutató példa](#jsonformat-example) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-214">See [JsonFormat example](#jsonformat-example) section on how to configure.</span></span> | <span data-ttu-id="fbbb8-215">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-215">No</span></span> |
| <span data-ttu-id="fbbb8-216">encodingName</span><span class="sxs-lookup"><span data-stu-id="fbbb8-216">encodingName</span></span> |<span data-ttu-id="fbbb8-217">A kódolási név megadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-217">Specify the encoding name.</span></span> <span data-ttu-id="fbbb8-218">Az érvényes kódolási nevekkel kapcsolatban lásd az [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-218">For the list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="fbbb8-219">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-219">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="fbbb8-220">Az **alapértelmezett** érték az **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-220">The **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="fbbb8-221">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-221">No</span></span> |
| <span data-ttu-id="fbbb8-222">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="fbbb8-222">nestingSeparator</span></span> |<span data-ttu-id="fbbb8-223">A beágyazási szinteket elválasztó karakter.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-223">Character that is used to separate nesting levels.</span></span> <span data-ttu-id="fbbb8-224">Az alapértelmezett érték a „.” (pont).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-224">The default value is '.' (dot).</span></span> |<span data-ttu-id="fbbb8-225">Nem</span><span class="sxs-lookup"><span data-stu-id="fbbb8-225">No</span></span> |

### <a name="json-file-patterns"></a><span data-ttu-id="fbbb8-226">JSON-fájlminták</span><span class="sxs-lookup"><span data-stu-id="fbbb8-226">JSON file patterns</span></span>

<span data-ttu-id="fbbb8-227">Másolási tevékenység során tudja értelmezni a JSON-fájlok a következő minták:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-227">Copy activity can parse the following patterns of JSON files:</span></span>

- <span data-ttu-id="fbbb8-228">**I. típus: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-228">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="fbbb8-229">Minden fájl egyetlen objektumot, illetve több, sorokkal határolt/összefűzött objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-229">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="fbbb8-230">Ha ezt a lehetőséget választja egy kimeneti adatkészletben, a másolási tevékenység egyetlen JSON-fájlt állít elő, soronként egy objektummal (sorokkal határolt).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-230">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="fbbb8-231">**példa egy objektumot tartalmazó JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-231">**single object JSON example**</span></span>

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

    * <span data-ttu-id="fbbb8-232">**példa sorokkal határolt JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-232">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="fbbb8-233">**példa összefűzött JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-233">**concatenated JSON example**</span></span>

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

- <span data-ttu-id="fbbb8-234">**II. típus: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-234">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="fbbb8-235">Minden fájl objektumok egy tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-235">Each file contains an array of objects.</span></span>

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

### <a name="jsonformat-example"></a><span data-ttu-id="fbbb8-236">A JsonFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="fbbb8-236">JsonFormat example</span></span>

<span data-ttu-id="fbbb8-237">**1. eset: Adatok másolása JSON-fájlokból**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-237">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="fbbb8-238">Tekintse meg a következő két minta, amikor az adatok másolása JSON-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-238">See the following two samples when copying data from JSON files.</span></span> <span data-ttu-id="fbbb8-239">Vegye figyelembe az általános szempontok:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-239">The generic points to note:</span></span>

<span data-ttu-id="fbbb8-240">**1. példa: adatok kigyűjtése objektumból és tömbből**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-240">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="fbbb8-241">Ebben a példában egy JSON-gyökérobjektum képződik le egyetlen rekordba táblázatos nézetben.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-241">In this sample, you expect one root JSON object maps to single record in tabular result.</span></span> <span data-ttu-id="fbbb8-242">Ha a JSON-fájl a következőt tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-242">If you have a JSON file with the following content:</span></span>  

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
<span data-ttu-id="fbbb8-243">és az adatok objektumokból és tömbökből való kigyűjtésével szeretné átmásolni egy Azure SQL-táblába az alábbi formátumban:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-243">and you want to copy it into an Azure SQL table in the following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="fbbb8-244">id</span><span class="sxs-lookup"><span data-stu-id="fbbb8-244">id</span></span> | <span data-ttu-id="fbbb8-245">deviceType</span><span class="sxs-lookup"><span data-stu-id="fbbb8-245">deviceType</span></span> | <span data-ttu-id="fbbb8-246">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="fbbb8-246">targetResourceType</span></span> | <span data-ttu-id="fbbb8-247">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="fbbb8-247">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="fbbb8-248">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="fbbb8-248">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="fbbb8-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="fbbb8-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="fbbb8-250">PC</span><span class="sxs-lookup"><span data-stu-id="fbbb8-250">PC</span></span> | <span data-ttu-id="fbbb8-251">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="fbbb8-251">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="fbbb8-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="fbbb8-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="fbbb8-253">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="fbbb8-253">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="fbbb8-254">A **JsonFormat** típusú bemeneti adatkészlet a következőképpen van meghatározva (részleges meghatározás, csak a fontos részekkel).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-254">The input dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="fbbb8-255">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-255">More specifically:</span></span>

- <span data-ttu-id="fbbb8-256">A `structure` szakasz határozza meg a testre szabott oszlopneveket és a megfelelő adattípusokat, miközben átalakítja őket táblázatos adatokká.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-256">`structure` section defines the customized column names and the corresponding data type while converting to tabular data.</span></span> <span data-ttu-id="fbbb8-257">Ez a szakasz **nem kötelező**, kivéve, ha oszlopleképezést kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-257">This section is **optional** unless you need to do column mapping.</span></span> <span data-ttu-id="fbbb8-258">Lásd: [dataset Forrásoszlopok leképezése cél adatkészlet oszlopok](data-factory-map-columns.md) című szakaszban talál további információt.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-258">See [Map source dataset columns to destination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="fbbb8-259">A `jsonPathDefinition` határozza meg az egyes oszlopok JSON-útvonalát, amely jelzi, hogy honnan történjen az adatok kinyerése.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-259">`jsonPathDefinition` specifies the JSON path for each column indicating where to extract the data from.</span></span> <span data-ttu-id="fbbb8-260">Ha adatokat szeretne másolni egy tömbből, az **array[x].property** használatával kinyerheti az adott tulajdonság értékét az x. objektumból, vagy az **array[*].property** használatával rákereshet az értékre az ilyen tulajdonságot tartalmazó összes objektumban.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-260">To copy data from array, you can use **array[x].property** to extract value of the given property from the xth object, or you can use **array[*].property** to find the value from any object containing such property.</span></span>

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

<span data-ttu-id="fbbb8-261">**2. példa: a tömbből származó ugyanazon minta keresztalkalmazása több objektumra**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-261">**Sample 2: cross apply multiple objects with the same pattern from array**</span></span>

<span data-ttu-id="fbbb8-262">Ebben a példában egy JSON-gyökérobjektumot alakít át több rekorddá táblázatos nézetben.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-262">In this sample, you expect to transform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="fbbb8-263">Ha a JSON-fájl a következőt tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-263">If you have a JSON file with the following content:</span></span>  

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
<span data-ttu-id="fbbb8-264">és szeretné átmásolni egy Azure SQL-táblába az alábbi formátumban, a tömbben lévő adatok egybesimításával, valamint keresztillesztést létrehozni a közös gyökérinformációval:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-264">and you want to copy it into an Azure SQL table in the following format, by flattening the data inside the array and cross join with the common root info:</span></span>

| <span data-ttu-id="fbbb8-265">ordernumber</span><span class="sxs-lookup"><span data-stu-id="fbbb8-265">ordernumber</span></span> | <span data-ttu-id="fbbb8-266">orderdate</span><span class="sxs-lookup"><span data-stu-id="fbbb8-266">orderdate</span></span> | <span data-ttu-id="fbbb8-267">order_pd</span><span class="sxs-lookup"><span data-stu-id="fbbb8-267">order_pd</span></span> | <span data-ttu-id="fbbb8-268">order_price</span><span class="sxs-lookup"><span data-stu-id="fbbb8-268">order_price</span></span> | <span data-ttu-id="fbbb8-269">city</span><span class="sxs-lookup"><span data-stu-id="fbbb8-269">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="fbbb8-270">01</span><span class="sxs-lookup"><span data-stu-id="fbbb8-270">01</span></span> | <span data-ttu-id="fbbb8-271">20170122</span><span class="sxs-lookup"><span data-stu-id="fbbb8-271">20170122</span></span> | <span data-ttu-id="fbbb8-272">P1</span><span class="sxs-lookup"><span data-stu-id="fbbb8-272">P1</span></span> | <span data-ttu-id="fbbb8-273">23</span><span class="sxs-lookup"><span data-stu-id="fbbb8-273">23</span></span> | <span data-ttu-id="fbbb8-274">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="fbbb8-274">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="fbbb8-275">01</span><span class="sxs-lookup"><span data-stu-id="fbbb8-275">01</span></span> | <span data-ttu-id="fbbb8-276">20170122</span><span class="sxs-lookup"><span data-stu-id="fbbb8-276">20170122</span></span> | <span data-ttu-id="fbbb8-277">P2</span><span class="sxs-lookup"><span data-stu-id="fbbb8-277">P2</span></span> | <span data-ttu-id="fbbb8-278">13</span><span class="sxs-lookup"><span data-stu-id="fbbb8-278">13</span></span> | <span data-ttu-id="fbbb8-279">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="fbbb8-279">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="fbbb8-280">01</span><span class="sxs-lookup"><span data-stu-id="fbbb8-280">01</span></span> | <span data-ttu-id="fbbb8-281">20170122</span><span class="sxs-lookup"><span data-stu-id="fbbb8-281">20170122</span></span> | <span data-ttu-id="fbbb8-282">P3</span><span class="sxs-lookup"><span data-stu-id="fbbb8-282">P3</span></span> | <span data-ttu-id="fbbb8-283">231</span><span class="sxs-lookup"><span data-stu-id="fbbb8-283">231</span></span> | <span data-ttu-id="fbbb8-284">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="fbbb8-284">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="fbbb8-285">A **JsonFormat** típusú bemeneti adatkészlet a következőképpen van meghatározva (részleges meghatározás, csak a fontos részekkel).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-285">The input dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="fbbb8-286">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-286">More specifically:</span></span>

- <span data-ttu-id="fbbb8-287">A `structure` szakasz határozza meg a testre szabott oszlopneveket és a megfelelő adattípusokat, miközben átalakítja őket táblázatos adatokká.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-287">`structure` section defines the customized column names and the corresponding data type while converting to tabular data.</span></span> <span data-ttu-id="fbbb8-288">Ez a szakasz **nem kötelező**, kivéve, ha oszlopleképezést kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-288">This section is **optional** unless you need to do column mapping.</span></span> <span data-ttu-id="fbbb8-289">Lásd: [dataset Forrásoszlopok leképezése cél adatkészlet oszlopok](data-factory-map-columns.md) című szakaszban talál további információt.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-289">See [Map source dataset columns to destination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="fbbb8-290">A `jsonNodeReference` jelzi az orderlines **tömb** alatti, azonos mintával rendelkező objektumok iterálását, illetve az adatok azokból való kinyerését.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-290">`jsonNodeReference` indicates to iterate and extract data from the objects with the same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="fbbb8-291">A `jsonPathDefinition` határozza meg az egyes oszlopok JSON-útvonalát, amely jelzi, hogy honnan történjen az adatok kinyerése.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-291">`jsonPathDefinition` specifies the JSON path for each column indicating where to extract the data from.</span></span> <span data-ttu-id="fbbb8-292">Ebben a példában az „ordernumber”, az „orderdate” és a „city” a „$.” értékkel kezdődő JSON-útvonallal jelzett gyökérobjektum alatt találhatók, míg az „order_pd” és az „order_price” a „$.” értéket nem tartalmazó tömbelemből származtatott útvonallal vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-292">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from the array element without "$.".</span></span>

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

<span data-ttu-id="fbbb8-293">**Vegye figyelembe a következő szempontokat:**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-293">**Note the following points:**</span></span>

* <span data-ttu-id="fbbb8-294">Ha a `structure` és a `jsonPathDefinition` nincs meghatározva a Data Factory-adatkészletben, a másolási tevékenység az első objektumból észleli a sémát, és a teljes objektumot egybesimítja.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-294">If the `structure` and `jsonPathDefinition` are not defined in the Data Factory dataset, the Copy Activity detects the schema from the first object and flatten the whole object.</span></span>
* <span data-ttu-id="fbbb8-295">Ha a JSON-bemenet egy tömböt tartalmaz, a másolási tevékenység alapértelmezés szerint a tömb teljes értékét egy karakterlánccá alakítja át.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-295">If the JSON input has an array, by default the Copy Activity converts the entire array value into a string.</span></span> <span data-ttu-id="fbbb8-296">Választhatja, hogy a `jsonNodeReference` és/vagy a `jsonPathDefinition` használatával kívánja kinyerni belőle az adatokat, vagy ki is hagyhatja ezt a műveletet, ha nem adja meg a `jsonPathDefinition` értékét.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-296">You can choose to extract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="fbbb8-297">Ha ismétlődő nevek szerepelnek ugyanazon a szinten, a másolási tevékenység az utolsót választja ki.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-297">If there are duplicate names at the same level, the Copy Activity picks the last one.</span></span>
* <span data-ttu-id="fbbb8-298">A tulajdonságnevek megkülönböztetik a kis- és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-298">Property names are case-sensitive.</span></span> <span data-ttu-id="fbbb8-299">A rendszer két azonos nevű, de eltérő kis- és nagybetűket tartalmazó tulajdonságot két különálló tulajdonságként kezel.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-299">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="fbbb8-300">**2. eset: Adatok írása JSON-fájlba**</span><span class="sxs-lookup"><span data-stu-id="fbbb8-300">**Case 2: Writing data to JSON file**</span></span>

<span data-ttu-id="fbbb8-301">Ha az SQL-adatbázis a következő táblázatban:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-301">If you have the following table in SQL Database:</span></span>

| <span data-ttu-id="fbbb8-302">id</span><span class="sxs-lookup"><span data-stu-id="fbbb8-302">id</span></span> | <span data-ttu-id="fbbb8-303">order_date</span><span class="sxs-lookup"><span data-stu-id="fbbb8-303">order_date</span></span> | <span data-ttu-id="fbbb8-304">order_price</span><span class="sxs-lookup"><span data-stu-id="fbbb8-304">order_price</span></span> | <span data-ttu-id="fbbb8-305">order_by</span><span class="sxs-lookup"><span data-stu-id="fbbb8-305">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="fbbb8-306">1</span><span class="sxs-lookup"><span data-stu-id="fbbb8-306">1</span></span> | <span data-ttu-id="fbbb8-307">20170119</span><span class="sxs-lookup"><span data-stu-id="fbbb8-307">20170119</span></span> | <span data-ttu-id="fbbb8-308">2000</span><span class="sxs-lookup"><span data-stu-id="fbbb8-308">2000</span></span> | <span data-ttu-id="fbbb8-309">David</span><span class="sxs-lookup"><span data-stu-id="fbbb8-309">David</span></span> |
| <span data-ttu-id="fbbb8-310">2</span><span class="sxs-lookup"><span data-stu-id="fbbb8-310">2</span></span> | <span data-ttu-id="fbbb8-311">20170120</span><span class="sxs-lookup"><span data-stu-id="fbbb8-311">20170120</span></span> | <span data-ttu-id="fbbb8-312">3500</span><span class="sxs-lookup"><span data-stu-id="fbbb8-312">3500</span></span> | <span data-ttu-id="fbbb8-313">Patrick</span><span class="sxs-lookup"><span data-stu-id="fbbb8-313">Patrick</span></span> |
| <span data-ttu-id="fbbb8-314">3</span><span class="sxs-lookup"><span data-stu-id="fbbb8-314">3</span></span> | <span data-ttu-id="fbbb8-315">20170121</span><span class="sxs-lookup"><span data-stu-id="fbbb8-315">20170121</span></span> | <span data-ttu-id="fbbb8-316">4000</span><span class="sxs-lookup"><span data-stu-id="fbbb8-316">4000</span></span> | <span data-ttu-id="fbbb8-317">Jason</span><span class="sxs-lookup"><span data-stu-id="fbbb8-317">Jason</span></span> |

<span data-ttu-id="fbbb8-318">és az egyes rekordokhoz, várt írni egy JSON-objektum a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-318">and for each record, you expect to write to a JSON object in the following format:</span></span>
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

<span data-ttu-id="fbbb8-319">A **JsonFormat** típusú kimeneti adatkészlet a következőképpen van meghatározva (részleges meghatározás, csak a fontos részekkel).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-319">The output dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="fbbb8-320">Pontosabban `structure` szakaszban definiálja a testreszabott tulajdonságnevek célfájlt, `nestingSeparator` (alapértelmezett érték ".") a neve a nest réteg azonosítására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-320">More specifically, `structure` section defines the customized property names in destination file, `nestingSeparator` (default is ".") are used to identify the nest layer from the name.</span></span> <span data-ttu-id="fbbb8-321">Ez a szakasz **nem kötelező**, kivéve, ha módosítani szeretné a tulajdonság nevét a forrásoszlop nevéhez képest, vagy egyes tulajdonságokat egymásba szeretne ágyazni.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-321">This section is **optional** unless you want to change the property name comparing with source column name, or nest some of the properties.</span></span>

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

## <a name="avro-format"></a><span data-ttu-id="fbbb8-322">Az AVRO formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-322">AVRO format</span></span>
<span data-ttu-id="fbbb8-323">Ha elemezni szeretné a Avro-fájlokat, vagy Avro formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **AvroFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-323">If you want to parse the Avro files or write the data in Avro format, set the `format` `type` property to **AvroFormat**.</span></span> <span data-ttu-id="fbbb8-324">Nem kell meghatároznia semmilyen tulajdonságot a Format szakaszban a typeProperties szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-324">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="fbbb8-325">Példa:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-325">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="fbbb8-326">Az Avro formátum Hive-táblákban való használatával kapcsolatban lásd az [Apache Hive oktatóanyagát](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-326">To use Avro format in a Hive table, you can refer to [Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="fbbb8-327">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-327">Note the following points:</span></span>  

* <span data-ttu-id="fbbb8-328">[Összetett adattípusú](http://avro.apache.org/docs/current/spec.html#schema_complex) használata nem támogatott (rögzíti, enumerálások, tömbök, térképeket, unióját képezi, és rögzített).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-328">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions, and fixed).</span></span>

## <a name="orc-format"></a><span data-ttu-id="fbbb8-329">ORC formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-329">ORC format</span></span>
<span data-ttu-id="fbbb8-330">Ha elemezni szeretné a ORC-fájlokat, vagy ORC formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **OrcFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-330">If you want to parse the ORC files or write the data in ORC format, set the `format` `type` property to **OrcFormat**.</span></span> <span data-ttu-id="fbbb8-331">Nem kell meghatároznia semmilyen tulajdonságot a Format szakaszban a typeProperties szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-331">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="fbbb8-332">Példa:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-332">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="fbbb8-333">Ha nem **adott állapotban** másol ORC-fájlokat a helyszíni és a felhőbeli adattárolók között, telepítenie kell a JRE (Java-futtatókörnyezet) 8-as verzióját az átjáró számítógépre.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-333">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need to install the JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="fbbb8-334">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-334">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="fbbb8-335">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-335">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="fbbb8-336">Válassza ki a megfelelő verziót.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-336">Choose the appropriate one.</span></span>
>
>

<span data-ttu-id="fbbb8-337">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-337">Note the following points:</span></span>

* <span data-ttu-id="fbbb8-338">Az összetett adattípusok nem támogatottak (STRUCT, MAP, LIST, UNION)</span><span class="sxs-lookup"><span data-stu-id="fbbb8-338">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="fbbb8-339">Az ORC-fájlok három, [tömörítéshez kapcsolódó beállítással](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/) rendelkeznek: NONE, ZLIB, SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-339">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="fbbb8-340">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-340">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="fbbb8-341">Az adatok olvasásához a metaadatokban szereplő tömörítési kodeket használja.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-341">It uses the compression codec is in the metadata to read the data.</span></span> <span data-ttu-id="fbbb8-342">Az ORC-fájlokba való írás esetén azonban a Data Factory a ZLIB tömörítést választja, amely az alapértelmezett az ORC-fájlok esetében.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-342">However, when writing to an ORC file, Data Factory chooses ZLIB, which is the default for ORC.</span></span> <span data-ttu-id="fbbb8-343">Jelenleg nincs lehetőség ennek a viselkedésnek a felülírására.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-343">Currently, there is no option to override this behavior.</span></span>

## <a name="parquet-format"></a><span data-ttu-id="fbbb8-344">Parquet formátum</span><span class="sxs-lookup"><span data-stu-id="fbbb8-344">Parquet format</span></span>
<span data-ttu-id="fbbb8-345">Ha elemezni szeretné a Parquet-fájlokat, vagy Parquet formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **ParquetFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-345">If you want to parse the Parquet files or write the data in Parquet format, set the `format` `type` property to **ParquetFormat**.</span></span> <span data-ttu-id="fbbb8-346">Nem kell meghatároznia semmilyen tulajdonságot a Format szakaszban a typeProperties szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-346">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="fbbb8-347">Példa:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-347">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="fbbb8-348">Ha nem **adott állapotban** másol Parquet-fájlokat a helyszíni és a felhőbeli adattárolók között, telepítenie kell a JRE (Java-futtatókörnyezet) 8-as verzióját az átjáró számítógépre.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-348">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need to install the JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="fbbb8-349">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-349">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="fbbb8-350">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="fbbb8-350">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="fbbb8-351">Válassza ki a megfelelő verziót.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-351">Choose the appropriate one.</span></span>
>
>

<span data-ttu-id="fbbb8-352">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-352">Note the following points:</span></span>

* <span data-ttu-id="fbbb8-353">Az összetett adattípusok nem támogatottak (MAP, LIST)</span><span class="sxs-lookup"><span data-stu-id="fbbb8-353">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="fbbb8-354">A Parquet-fájlok a következő tömörítéshez kapcsolódó beállításokat használják: NONE, SNAPPY, GZIP és LZO.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-354">Parquet file has the following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="fbbb8-355">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-355">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="fbbb8-356">Az adatok olvasásához a metaadatokban szereplő tömörítési kodeket használja.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-356">It uses the compression codec in the metadata to read the data.</span></span> <span data-ttu-id="fbbb8-357">A Parquet-fájlokba való írás esetén azonban a Data Factory a SNAPPY tömörítést választja, amely az alapértelmezett a Parquet-fájlok esetében.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-357">However, when writing to a Parquet file, Data Factory chooses SNAPPY, which is the default for Parquet format.</span></span> <span data-ttu-id="fbbb8-358">Jelenleg nincs lehetőség ennek a viselkedésnek a felülírására.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-358">Currently, there is no option to override this behavior.</span></span>

## <a name="compression-support"></a><span data-ttu-id="fbbb8-359">Tömörítés támogatása</span><span class="sxs-lookup"><span data-stu-id="fbbb8-359">Compression support</span></span>
<span data-ttu-id="fbbb8-360">Nagy méretű adatkészletekhez feldolgozása okozhat, és a hálózati szűk keresztmetszeteket.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-360">Processing large data sets can cause I/O and network bottlenecks.</span></span> <span data-ttu-id="fbbb8-361">Ezért tárolók a tömörített adatok is nem csak felgyorsítsa az adatok átvitele a hálózaton és lemezterületet spóroljon a, de is kapcsolja az jelentős teljesítményjavulást eredményezhet a big Data típusú adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-361">Therefore, compressed data in stores can not only speed up data transfer across the network and save disk space, but also bring significant performance improvements in processing big data.</span></span> <span data-ttu-id="fbbb8-362">Tömörítés jelenleg a fájlalapú adatok tárolókhoz, például az Azure Blob és helyszíni fájlrendszer használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-362">Currently, compression is supported for file-based data stores such as Azure Blob or On-premises File System.</span></span>  

<span data-ttu-id="fbbb8-363">A DataSet adatkészlet tömörítése megadásához használja a **tömörítés** tulajdonság az adatkészlet JSON az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-363">To specify compression for a dataset, use the **compression** property in the dataset JSON as in the following example:</span></span>   

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

<span data-ttu-id="fbbb8-364">Tegyük fel, hogy a minta-adatkészleteken a másolási tevékenység kimenete szolgál, a másolási tevékenység kimeneti adatokat tömöríti a GZIP kodek optimális arány használatával, és jegyezze meg a tömörített adatok az Azure Blob Storage pagecounts.csv.gz fájlba.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-364">Suppose the sample dataset is used as the output of a copy activity, the copy activity compresses the output data with GZIP codec using optimal ratio and then write the compressed data into a file named pagecounts.csv.gz in the Azure Blob Storage.</span></span>

> [!NOTE]
> <span data-ttu-id="fbbb8-365">Tömörítési beállítások nem támogatottak az adatok a **AvroFormat**, **OrcFormat**, vagy **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-365">Compression settings are not supported for data in the **AvroFormat**, **OrcFormat**, or **ParquetFormat**.</span></span> <span data-ttu-id="fbbb8-366">Az alábbi formátumokban fájlok olvasásakor a Data Factory észlel, és a tömörítési kodek használ a metaadatokban.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-366">When reading files in these formats, Data Factory detects and uses the compression codec in the metadata.</span></span> <span data-ttu-id="fbbb8-367">Ezek a formátumok a fájlokat írásakor adat-előállító úgy dönt, az alapértelmezett tömörítési kodek azt a formátumot.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-367">When writing to files in these formats, Data Factory chooses the default compression codec for that format.</span></span> <span data-ttu-id="fbbb8-368">Például ZLIB OrcFormat és a ParquetFormat SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-368">For example, ZLIB for OrcFormat and SNAPPY for ParquetFormat.</span></span>   

<span data-ttu-id="fbbb8-369">A **tömörítés** szakasz két tulajdonságokkal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-369">The **compression** section has two properties:</span></span>  

* <span data-ttu-id="fbbb8-370">**Típus:** a tömörítési kodek, amely lehet **GZIP**, **Deflate**, **BZIP2**, vagy **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-370">**Type:** the compression codec, which can be **GZIP**, **Deflate**, **BZIP2**, or **ZipDeflate**.</span></span>  
* <span data-ttu-id="fbbb8-371">**Szint:** a tömörítési arány, amely lehet **Optimal** vagy **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-371">**Level:** the compression ratio, which can be **Optimal** or **Fastest**.</span></span>

  * <span data-ttu-id="fbbb8-372">**Leggyorsabb:** a tömörítési művelet még akkor is, ha a fájl nem optimális tömörített, minél gyorsabban be kell.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-372">**Fastest:** The compression operation should complete as quickly as possible, even if the resulting file is not optimally compressed.</span></span>
  * <span data-ttu-id="fbbb8-373">**Optimális**: A tömörítési műveletet kell lehet optimális tömörített, akkor is, ha a művelet befejezéséhez hosszabb időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-373">**Optimal**: The compression operation should be optimally compressed, even if the operation takes a longer time to complete.</span></span>

    <span data-ttu-id="fbbb8-374">További információkért lásd: [tömörítési szint](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-374">For more information, see [Compression Level](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) topic.</span></span>

<span data-ttu-id="fbbb8-375">Ha a `compression` egy bemeneti adatkészlet JSON tulajdonság, a folyamat elolvashatják a tömörített adatok a forrás; és egy kimeneti adatkészlet JSON tulajdonság megadása esetén a másolási tevékenység tömörített adatokat írhatna a cél.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-375">When you specify `compression` property in an input dataset JSON, the pipeline can read compressed data from the source; and when you specify the property in an output dataset JSON, the copy activity can write compressed data to the destination.</span></span> <span data-ttu-id="fbbb8-376">Itt van néhány szituáció:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-376">Here are a few sample scenarios:</span></span>

* <span data-ttu-id="fbbb8-377">Tömörített olvasási GZIP adatait az Azure blob kibontani azt, és eredmény adatok írása az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-377">Read GZIP compressed data from an Azure blob, decompress it, and write result data to an Azure SQL database.</span></span> <span data-ttu-id="fbbb8-378">A bemeneti Azure-Blob adatkészlet megadhatja a `compression` `type` GZIP JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-378">You define the input Azure Blob dataset with the `compression` `type` JSON property as GZIP.</span></span>
* <span data-ttu-id="fbbb8-379">Adatokat olvasni egy egyszerű szöveges fájlt a helyi fájlrendszer, a tömörítés a GZip formátumban, és a tömörített adatok írása az Azure-blobot.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-379">Read data from a plain-text file from on-premises File System, compress it using GZip format, and write the compressed data to an Azure blob.</span></span> <span data-ttu-id="fbbb8-380">Egy kimeneti Azure Blob adatkészlet megadhatja a `compression` `type` GZip JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-380">You define an output Azure Blob dataset with the `compression` `type` JSON property as GZip.</span></span>
* <span data-ttu-id="fbbb8-381">Az FTP-kiszolgáló, .zip fájl olvasása azt belül a fájlokat, és azokat a fájlokat az Azure Data Lake Store ártó kibontani.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-381">Read .zip file from FTP server, decompress it to get the files inside, and land those files into Azure Data Lake Store.</span></span> <span data-ttu-id="fbbb8-382">Egy bemeneti FTP adatkészlet megadhatja a `compression` `type` ZipDeflate JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-382">You define an input FTP dataset with the `compression` `type` JSON property as ZipDeflate.</span></span>
* <span data-ttu-id="fbbb8-383">A GZIP-tömörített adatokat olvasni az Azure blob, azt kibontani, BZIP2 használatával tömörítése és eredmény adatokat írni az Azure blob.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-383">Read a GZIP-compressed data from an Azure blob, decompress it, compress it using BZIP2, and write result data to an Azure blob.</span></span> <span data-ttu-id="fbbb8-384">Megadhatja, hogy a bemeneti Azure-Blob adatkészlet `compression` `type` GZIP és a kimeneti adatkészlet `compression` `type` BZIP2 ebben az esetben értékre.</span><span class="sxs-lookup"><span data-stu-id="fbbb8-384">You define the input Azure Blob dataset with `compression` `type` set to GZIP and the output dataset with `compression` `type` set to BZIP2 in this case.</span></span>   


## <a name="next-steps"></a><span data-ttu-id="fbbb8-385">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbbb8-385">Next steps</span></span>
<span data-ttu-id="fbbb8-386">Tekintse meg a fájlalapú adatok tárolóinak Azure Data Factory támogatja a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="fbbb8-386">See the following articles for file-based data stores supported by Azure Data Factory:</span></span>

- [<span data-ttu-id="fbbb8-387">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="fbbb8-387">Azure Blob Storage</span></span>](data-factory-azure-blob-connector.md)
- [<span data-ttu-id="fbbb8-388">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fbbb8-388">Azure Data Lake Store</span></span>](data-factory-azure-datalake-connector.md)
- [<span data-ttu-id="fbbb8-389">FTP</span><span class="sxs-lookup"><span data-stu-id="fbbb8-389">FTP</span></span>](data-factory-ftp-connector.md)
- [<span data-ttu-id="fbbb8-390">HDFS</span><span class="sxs-lookup"><span data-stu-id="fbbb8-390">HDFS</span></span>](data-factory-hdfs-connector.md)
- [<span data-ttu-id="fbbb8-391">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="fbbb8-391">File System</span></span>](data-factory-onprem-file-system-connector.md)
- [<span data-ttu-id="fbbb8-392">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="fbbb8-392">Amazon S3</span></span>](data-factory-amazon-simple-storage-service-connector.md)
