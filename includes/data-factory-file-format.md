## <a name="specifying-formats"></a><span data-ttu-id="07397-101">Formátumok meghatározása</span><span class="sxs-lookup"><span data-stu-id="07397-101">Specifying formats</span></span>
<span data-ttu-id="07397-102">Az Azure Data Factory támogatja a következő formátumban típusok hello:</span><span class="sxs-lookup"><span data-stu-id="07397-102">Azure Data Factory supports hello following format types:</span></span>

* [<span data-ttu-id="07397-103">Szöveges formátum</span><span class="sxs-lookup"><span data-stu-id="07397-103">Text Format</span></span>](#specifying-textformat)
* [<span data-ttu-id="07397-104">JSON formátum</span><span class="sxs-lookup"><span data-stu-id="07397-104">JSON Format</span></span>](#specifying-jsonformat)
* [<span data-ttu-id="07397-105">Avro formátum</span><span class="sxs-lookup"><span data-stu-id="07397-105">Avro Format</span></span>](#specifying-avroformat)
* [<span data-ttu-id="07397-106">ORC formátum</span><span class="sxs-lookup"><span data-stu-id="07397-106">ORC Format</span></span>](#specifying-orcformat)
* [<span data-ttu-id="07397-107">Parquet formátum</span><span class="sxs-lookup"><span data-stu-id="07397-107">Parquet Format</span></span>](#specifying-parquetformat)

### <a name="specifying-textformat"></a><span data-ttu-id="07397-108">TextFormat megadása</span><span class="sxs-lookup"><span data-stu-id="07397-108">Specifying TextFormat</span></span>
<span data-ttu-id="07397-109">Ha szeretné, hogy tooparse hello szövegfájlok vagy hello adatírás szöveges formátumú, állítsa be a hello `format` `type` tulajdonság túl**szöveges**.</span><span class="sxs-lookup"><span data-stu-id="07397-109">If you want tooparse hello text files or write hello data in text format, set hello `format` `type` property too**TextFormat**.</span></span> <span data-ttu-id="07397-110">Azt is megadhatja, hello következő **választható** hello tulajdonságok `format` szakasz.</span><span class="sxs-lookup"><span data-stu-id="07397-110">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="07397-111">Lásd: [szöveges példa](#textformat-example) hogyan szakasz tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="07397-111">See [TextFormat example](#textformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="07397-112">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="07397-112">Property</span></span> | <span data-ttu-id="07397-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="07397-113">Description</span></span> | <span data-ttu-id="07397-114">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="07397-114">Allowed values</span></span> | <span data-ttu-id="07397-115">Kötelező</span><span class="sxs-lookup"><span data-stu-id="07397-115">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="07397-116">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="07397-116">columnDelimiter</span></span> |<span data-ttu-id="07397-117">hello karakter egy fájlban használt tooseparate oszlopok.</span><span class="sxs-lookup"><span data-stu-id="07397-117">hello character used tooseparate columns in a file.</span></span> <span data-ttu-id="07397-118">Érdemes lehet toouse ritka nem nyomtatható karakter, amely valószínűleg nem létezik az adatokon: például adja meg a "\u0001" által képviselt indítsa el a fejléc (SOH).</span><span class="sxs-lookup"><span data-stu-id="07397-118">You can consider toouse a rare unprintable char which not likely exists in your data: e.g. specify "\u0001" which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="07397-119">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="07397-119">Only one character is allowed.</span></span> <span data-ttu-id="07397-120">Hello **alapértelmezett** értéke **vesszővel (",")**.</span><span class="sxs-lookup"><span data-stu-id="07397-120">hello **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="07397-121">toouse Unicode-karakter, tekintse meg a túl[Unicode-karaktereket](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget megfelelő kód hello azt.</span><span class="sxs-lookup"><span data-stu-id="07397-121">toouse an Unicode character, refer too[Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello corresponding code for it.</span></span> |<span data-ttu-id="07397-122">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-122">No</span></span> |
| <span data-ttu-id="07397-123">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="07397-123">rowDelimiter</span></span> |<span data-ttu-id="07397-124">hello karakter egy fájlban használt tooseparate sorokat.</span><span class="sxs-lookup"><span data-stu-id="07397-124">hello character used tooseparate rows in a file.</span></span> |<span data-ttu-id="07397-125">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="07397-125">Only one character is allowed.</span></span> <span data-ttu-id="07397-126">Hello **alapértelmezett** értéke bármelyik hello követő értékek olvasáskor: **["\r\n", "\r", "\n"]** és **"\r\n"** írás.</span><span class="sxs-lookup"><span data-stu-id="07397-126">hello **default** value is any of hello following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="07397-127">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-127">No</span></span> |
| <span data-ttu-id="07397-128">escapeChar</span><span class="sxs-lookup"><span data-stu-id="07397-128">escapeChar</span></span> |<span data-ttu-id="07397-129">hello speciális karaktert használt oszlop elválasztó tooescape hello bemeneti fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="07397-129">hello special character used tooescape a column delimiter in hello content of input file.</span></span> <br/><br/><span data-ttu-id="07397-130">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="07397-130">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="07397-131">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="07397-131">Only one character is allowed.</span></span> <span data-ttu-id="07397-132">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="07397-132">No default value.</span></span> <br/><br/><span data-ttu-id="07397-133">Példa: Ha vesszővel (', '), hello oszlop elválasztó, de szeretné toohave hello vesszővel karaktert hello szövegként (Példa: "Hello, world"), adja meg a "$" hello escape-karakter, és a karakterlánc "Hello$, world" hello forrásból.</span><span class="sxs-lookup"><span data-stu-id="07397-133">Example: if you have comma (',') as hello column delimiter but you want toohave hello comma character in hello text (example: "Hello, world"), you can define ‘$’ as hello escape character and use string "Hello$, world" in hello source.</span></span> |<span data-ttu-id="07397-134">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-134">No</span></span> |
| <span data-ttu-id="07397-135">quoteChar</span><span class="sxs-lookup"><span data-stu-id="07397-135">quoteChar</span></span> |<span data-ttu-id="07397-136">hello kitöltéshez használandó tooquote karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="07397-136">hello character used tooquote a string value.</span></span> <span data-ttu-id="07397-137">hello oszlop- és sorfejlécek határolójelek belül hello idézőjel karakterek hello karakterláncértéket részeként kezelik.</span><span class="sxs-lookup"><span data-stu-id="07397-137">hello column and row delimiters inside hello quote characters would be treated as part of hello string value.</span></span> <span data-ttu-id="07397-138">Ez a tulajdonság akkor alkalmazható tooboth bemeneti és kimeneti adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="07397-138">This property is applicable tooboth input and output datasets.</span></span><br/><br/><span data-ttu-id="07397-139">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="07397-139">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="07397-140">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="07397-140">Only one character is allowed.</span></span> <span data-ttu-id="07397-141">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="07397-141">No default value.</span></span> <br/><br/><span data-ttu-id="07397-142">Például, ha vesszővel (', '), hello oszlop elválasztó, de szeretné toohave vesszővel karaktert hello szöveges (Példa: < Hello, world >), definiálhat "(idézőjelek nélkül), hello idézőjel, és használja a hello karakterlánc"Hello, world"hello forrás.</span><span class="sxs-lookup"><span data-stu-id="07397-142">For example, if you have comma (',') as hello column delimiter but you want toohave comma character in hello text (example: <Hello, world>), you can define " (double quote) as hello quote character and use hello string "Hello, world" in hello source.</span></span> |<span data-ttu-id="07397-143">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-143">No</span></span> |
| <span data-ttu-id="07397-144">nullValue</span><span class="sxs-lookup"><span data-stu-id="07397-144">nullValue</span></span> |<span data-ttu-id="07397-145">Egy vagy több karaktert használt toorepresent null értékű.</span><span class="sxs-lookup"><span data-stu-id="07397-145">One or more characters used toorepresent a null value.</span></span> |<span data-ttu-id="07397-146">Egy vagy több karakter.</span><span class="sxs-lookup"><span data-stu-id="07397-146">One or more characters.</span></span> <span data-ttu-id="07397-147">Hello **alapértelmezett** értékei **"\N" és "NULL"** olvasáskor és **"\N"** írás.</span><span class="sxs-lookup"><span data-stu-id="07397-147">hello **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="07397-148">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-148">No</span></span> |
| <span data-ttu-id="07397-149">encodingName</span><span class="sxs-lookup"><span data-stu-id="07397-149">encodingName</span></span> |<span data-ttu-id="07397-150">Adja meg a hello kódolási név.</span><span class="sxs-lookup"><span data-stu-id="07397-150">Specify hello encoding name.</span></span> |<span data-ttu-id="07397-151">Egy érvényes kódolási név.</span><span class="sxs-lookup"><span data-stu-id="07397-151">A valid encoding name.</span></span> <span data-ttu-id="07397-152">Lásd az [Encoding.EncodingName tulajdonságot](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="07397-152">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="07397-153">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="07397-153">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="07397-154">Hello **alapértelmezett** értéke **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="07397-154">hello **default** value is **UTF-8**.</span></span> |<span data-ttu-id="07397-155">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-155">No</span></span> |
| <span data-ttu-id="07397-156">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="07397-156">firstRowAsHeader</span></span> |<span data-ttu-id="07397-157">Meghatározza, hogy tooconsider hello első sor adatai alkossák fejléc.</span><span class="sxs-lookup"><span data-stu-id="07397-157">Specifies whether tooconsider hello first row as a header.</span></span> <span data-ttu-id="07397-158">A bemeneti adatkészletek első sorát a Data Factory fejlécként olvassa be.</span><span class="sxs-lookup"><span data-stu-id="07397-158">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="07397-159">A kimeneti adatkészletek első sorát a Data Factory fejlécként írja ki.</span><span class="sxs-lookup"><span data-stu-id="07397-159">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="07397-160">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="07397-160">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="07397-161">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="07397-161">True</span></span><br/><span data-ttu-id="07397-162">**False (alapértelmezett)**</span><span class="sxs-lookup"><span data-stu-id="07397-162">**False (default)**</span></span> |<span data-ttu-id="07397-163">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-163">No</span></span> |
| <span data-ttu-id="07397-164">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="07397-164">skipLineCount</span></span> |<span data-ttu-id="07397-165">Meghatározza a sorok tooskip hello számát, bemeneti fájlok adatainak olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="07397-165">Indicates hello number of rows tooskip when reading data from input files.</span></span> <span data-ttu-id="07397-166">Ha skipLineCount és firstRowAsHeader is meg van adva, hello sorok először kimarad, és majd hello fejléc-információ a hello bemeneti fájl olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="07397-166">If both skipLineCount and firstRowAsHeader are specified, hello lines are skipped first and then hello header information is read from hello input file.</span></span> <br/><br/><span data-ttu-id="07397-167">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="07397-167">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="07397-168">Egész szám</span><span class="sxs-lookup"><span data-stu-id="07397-168">Integer</span></span> |<span data-ttu-id="07397-169">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-169">No</span></span> |
| <span data-ttu-id="07397-170">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="07397-170">treatEmptyAsNull</span></span> |<span data-ttu-id="07397-171">Megadja, hogy a tootreat null vagy üres karakterlánc, a rendszer null értéket, amikor a bemeneti fájl adatainak olvasása.</span><span class="sxs-lookup"><span data-stu-id="07397-171">Specifies whether tootreat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="07397-172">**True (alapértelmezett)**</span><span class="sxs-lookup"><span data-stu-id="07397-172">**True (default)**</span></span><br/><span data-ttu-id="07397-173">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="07397-173">False</span></span> |<span data-ttu-id="07397-174">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-174">No</span></span> |

#### <a name="textformat-example"></a><span data-ttu-id="07397-175">A TextFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="07397-175">TextFormat example</span></span>
<span data-ttu-id="07397-176">hello következő példa bemutatja hello formátum tulajdonságok a szöveges.</span><span class="sxs-lookup"><span data-stu-id="07397-176">hello following sample shows some of hello format properties for TextFormat.</span></span>

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

<span data-ttu-id="07397-177">toouse egy `escapeChar` helyett `quoteChar`, cserélje le a hello sor `quoteChar` a következő escapeChar hello:</span><span class="sxs-lookup"><span data-stu-id="07397-177">toouse an `escapeChar` instead of `quoteChar`, replace hello line with `quoteChar` with hello following escapeChar:</span></span>

```json
"escapeChar": "$",
```

#### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="07397-178">A firstRowAsHeader és a skipLineCount használatára vonatkozó forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="07397-178">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="07397-179">Nem fájl forrás tooa szöveges fájlból másolja át, és szeretné tooadd hello séma metaadatokat tartalmazó fejléc sor (pl.: SQL-séma).</span><span class="sxs-lookup"><span data-stu-id="07397-179">You are copying from a non-file source tooa text file and would like tooadd a header line containing hello schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="07397-180">Adja meg `firstRowAsHeader` hello kimeneti adatkészlet ebben a forgatókönyvben a true.</span><span class="sxs-lookup"><span data-stu-id="07397-180">Specify `firstRowAsHeader` as true in hello output dataset for this scenario.</span></span>
* <span data-ttu-id="07397-181">A fejléc sor tooa nem fájl fogadó tartalmazó szöveges fájlból másolja át, és szeretné, hogy a sor toodrop.</span><span class="sxs-lookup"><span data-stu-id="07397-181">You are copying from a text file containing a header line tooa non-file sink and would like toodrop that line.</span></span> <span data-ttu-id="07397-182">Adja meg `firstRowAsHeader` hello bemeneti adatkészletet, igaz.</span><span class="sxs-lookup"><span data-stu-id="07397-182">Specify `firstRowAsHeader` as true in hello input dataset.</span></span>
* <span data-ttu-id="07397-183">Másolja át egy szövegfájlból, és szeretné, hogy tooskip néhány sor hello elején, adatok vagy a fejléc adatokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="07397-183">You are copying from a text file and want tooskip a few lines at hello beginning that contain no data or header information.</span></span> <span data-ttu-id="07397-184">Adja meg `skipLineCount` tooindicate hello száma sorok toobe kihagyva.</span><span class="sxs-lookup"><span data-stu-id="07397-184">Specify `skipLineCount` tooindicate hello number of lines toobe skipped.</span></span> <span data-ttu-id="07397-185">Ha hello rest hello fájl tartalmazza a fejléc, azt is megadhatja, `firstRowAsHeader`.</span><span class="sxs-lookup"><span data-stu-id="07397-185">If hello rest of hello file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="07397-186">Ha mindkét `skipLineCount` és `firstRowAsHeader` megadott, hello sorok először kimarad, és ezután hello fejléc-információ a hello bemeneti fájl olvasásának</span><span class="sxs-lookup"><span data-stu-id="07397-186">If both `skipLineCount` and `firstRowAsHeader` are specified, hello lines are skipped first and then hello header information is read from hello input file</span></span>

### <a name="specifying-jsonformat"></a><span data-ttu-id="07397-187">JsonFormat megadása</span><span class="sxs-lookup"><span data-stu-id="07397-187">Specifying JsonFormat</span></span>
<span data-ttu-id="07397-188">túl**importálási/exportálási JSON-fájlok használata-be/Azure Cosmos DB van**, lásd: [importálási/exportálási JSON-dokumentumok](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) szakasz hello Azure Cosmos DB Connector adatokkal.</span><span class="sxs-lookup"><span data-stu-id="07397-188">too**import/export JSON files as-is into/from Azure Cosmos DB**, see [Import/export JSON documents](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) section in hello Azure Cosmos DB connector with details.</span></span>

<span data-ttu-id="07397-189">Ha szeretné, hogy tooparse hello JSON-fájlokat vagy hello adatírás JSON formátumban, állítsa be a hello `format` `type` tulajdonság túl**JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="07397-189">If you want tooparse hello JSON files or write hello data in JSON format, set hello `format` `type` property too**JsonFormat**.</span></span> <span data-ttu-id="07397-190">Azt is megadhatja, hello következő **választható** hello tulajdonságok `format` szakasz.</span><span class="sxs-lookup"><span data-stu-id="07397-190">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="07397-191">Lásd: [JsonFormat példa](#jsonformat-example) hogyan szakasz tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="07397-191">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="07397-192">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="07397-192">Property</span></span> | <span data-ttu-id="07397-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="07397-193">Description</span></span> | <span data-ttu-id="07397-194">Kötelező</span><span class="sxs-lookup"><span data-stu-id="07397-194">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="07397-195">filePattern</span><span class="sxs-lookup"><span data-stu-id="07397-195">filePattern</span></span> |<span data-ttu-id="07397-196">Minden JSON-fájlban tárolt adatok hello mintát jelzi.</span><span class="sxs-lookup"><span data-stu-id="07397-196">Indicate hello pattern of data stored in each JSON file.</span></span> <span data-ttu-id="07397-197">Az engedélyezett értékek a következők: **setOfObjects** és **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="07397-197">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="07397-198">Hello **alapértelmezett** értéke **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="07397-198">hello **default** value is **setOfObjects**.</span></span> <span data-ttu-id="07397-199">A mintákkal kapcsolatban lásd a [JSON-fájlminták](#json-file-patterns) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="07397-199">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="07397-200">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-200">No</span></span> |
| <span data-ttu-id="07397-201">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="07397-201">jsonNodeReference</span></span> | <span data-ttu-id="07397-202">Ha tooiterate és az adatok kinyerése egy tömb belül hello objektumok mezőben a hello azonos mintát, adja meg, hogy tömb hello JSON elérési útját.</span><span class="sxs-lookup"><span data-stu-id="07397-202">If you want tooiterate and extract data from hello objects inside an array field with hello same pattern, specify hello JSON path of that array.</span></span> <span data-ttu-id="07397-203">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="07397-203">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="07397-204">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-204">No</span></span> |
| <span data-ttu-id="07397-205">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="07397-205">jsonPathDefinition</span></span> | <span data-ttu-id="07397-206">Adja meg a hello JSON elérési út minden oszlop leképezése egyéni oszlopnévvel (kisbetűvel kezdődik).</span><span class="sxs-lookup"><span data-stu-id="07397-206">Specify hello JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="07397-207">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat, és ki tud nyerni adatokat objektumokból vagy tömbökből.</span><span class="sxs-lookup"><span data-stu-id="07397-207">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="07397-208">A mezők a gyökérszintű objektum esetében indítsa el a legfelső szintű $; belül hello tömb által kiválasztott mező `jsonNodeReference` tulajdonság, hello tömbelem elindítását.</span><span class="sxs-lookup"><span data-stu-id="07397-208">For fields under root object, start with root $; for fields inside hello array chosen by `jsonNodeReference` property, start from hello array element.</span></span> <span data-ttu-id="07397-209">Lásd: [JsonFormat példa](#jsonformat-example) hogyan szakasz tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="07397-209">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span> | <span data-ttu-id="07397-210">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-210">No</span></span> |
| <span data-ttu-id="07397-211">encodingName</span><span class="sxs-lookup"><span data-stu-id="07397-211">encodingName</span></span> |<span data-ttu-id="07397-212">Adja meg a hello kódolási név.</span><span class="sxs-lookup"><span data-stu-id="07397-212">Specify hello encoding name.</span></span> <span data-ttu-id="07397-213">Érvényes kódolási nevek hello listájáért lásd: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="07397-213">For hello list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="07397-214">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="07397-214">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="07397-215">Hello **alapértelmezett** értéke: **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="07397-215">hello **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="07397-216">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-216">No</span></span> |
| <span data-ttu-id="07397-217">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="07397-217">nestingSeparator</span></span> |<span data-ttu-id="07397-218">Az karakter, amely használt tooseparate beágyazási szinttel.</span><span class="sxs-lookup"><span data-stu-id="07397-218">Character that is used tooseparate nesting levels.</span></span> <span data-ttu-id="07397-219">hello alapértelmezett érték "." (pont).</span><span class="sxs-lookup"><span data-stu-id="07397-219">hello default value is '.' (dot).</span></span> |<span data-ttu-id="07397-220">Nem</span><span class="sxs-lookup"><span data-stu-id="07397-220">No</span></span> |

#### <a name="json-file-patterns"></a><span data-ttu-id="07397-221">JSON-fájlminták</span><span class="sxs-lookup"><span data-stu-id="07397-221">JSON file patterns</span></span>

<span data-ttu-id="07397-222">A másolási tevékenység a JSON-fájlok következő mintáit tudja elemezni:</span><span class="sxs-lookup"><span data-stu-id="07397-222">Copy activity can parse below patterns of JSON files:</span></span>

- <span data-ttu-id="07397-223">**I. típus: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="07397-223">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="07397-224">Minden fájl egyetlen objektumot, illetve több, sorokkal határolt/összefűzött objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="07397-224">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="07397-225">Ha ezt a lehetőséget választja egy kimeneti adatkészletben, a másolási tevékenység egyetlen JSON-fájlt állít elő, soronként egy objektummal (sorokkal határolt).</span><span class="sxs-lookup"><span data-stu-id="07397-225">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="07397-226">**példa egy objektumot tartalmazó JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="07397-226">**single object JSON example**</span></span>

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

    * <span data-ttu-id="07397-227">**példa sorokkal határolt JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="07397-227">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="07397-228">**példa összefűzött JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="07397-228">**concatenated JSON example**</span></span>

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

- <span data-ttu-id="07397-229">**II. típus: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="07397-229">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="07397-230">Minden fájl objektumok egy tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="07397-230">Each file contains an array of objects.</span></span>

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

#### <a name="jsonformat-example"></a><span data-ttu-id="07397-231">A JsonFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="07397-231">JsonFormat example</span></span>

<span data-ttu-id="07397-232">**1. eset: Adatok másolása JSON-fájlokból**</span><span class="sxs-lookup"><span data-stu-id="07397-232">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="07397-233">Lásd az alábbi minták két típusú adatok másolásakor a JSON-fájlokat, és általános pontok toonote hello:</span><span class="sxs-lookup"><span data-stu-id="07397-233">See below two types of samples when copying data from JSON files, and hello generic points toonote:</span></span>

<span data-ttu-id="07397-234">**1. példa: adatok kigyűjtése objektumból és tömbből**</span><span class="sxs-lookup"><span data-stu-id="07397-234">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="07397-235">Ez a minta elvárt egy legfelső szintű JSON-objektum leképezi a táblázatos eredmény toosingle rekord.</span><span class="sxs-lookup"><span data-stu-id="07397-235">In this sample, you expect one root JSON object maps toosingle record in tabular result.</span></span> <span data-ttu-id="07397-236">Ha egy JSON-fájl a következő tartalmat hello:</span><span class="sxs-lookup"><span data-stu-id="07397-236">If you have a JSON file with hello following content:</span></span>  

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
<span data-ttu-id="07397-237">és azt szeretné, hogy azt egy Azure SQL táblázatba hello alábbi formázása, kinyeri az adatokat a objektumok és a tömb toocopy:</span><span class="sxs-lookup"><span data-stu-id="07397-237">and you want toocopy it into an Azure SQL table in hello following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="07397-238">id</span><span class="sxs-lookup"><span data-stu-id="07397-238">id</span></span> | <span data-ttu-id="07397-239">deviceType</span><span class="sxs-lookup"><span data-stu-id="07397-239">deviceType</span></span> | <span data-ttu-id="07397-240">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="07397-240">targetResourceType</span></span> | <span data-ttu-id="07397-241">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="07397-241">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="07397-242">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="07397-242">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="07397-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="07397-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="07397-244">PC</span><span class="sxs-lookup"><span data-stu-id="07397-244">PC</span></span> | <span data-ttu-id="07397-245">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="07397-245">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="07397-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="07397-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="07397-247">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="07397-247">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="07397-248">bemeneti adatkészlet hello **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban).</span><span class="sxs-lookup"><span data-stu-id="07397-248">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="07397-249">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="07397-249">More specifically:</span></span>

- <span data-ttu-id="07397-250">`structure`szakasz határozza meg a testreszabott hello oszlopneveket és a megfelelő adattípus hello tootabular adatok konvertálásakor.</span><span class="sxs-lookup"><span data-stu-id="07397-250">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="07397-251">Ez a szakasz **választható** kivéve toodo oszlopleképezés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="07397-251">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="07397-252">A további részletekkel kapcsolatban lásd a [Struktúrameghatározások négyszögletes adatkészletek esetén](#specifying-structure-definition-for-rectangular-datasets) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="07397-252">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="07397-253">`jsonPathDefinition`az egyes oszlopok, ahol tooextract hello adatait jelző hello JSON elérési út megadása</span><span class="sxs-lookup"><span data-stu-id="07397-253">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="07397-254">toocopy adatokat egy olyan tömbből, használhat **tömb [x] .property** hello hello x-edik objektumot, vagy a tulajdonság megadott értéke tooextract használható  **tömb [*] .property** toofind hello értéket az összes ilyen tulajdonságot tartalmazó objektumtól.</span><span class="sxs-lookup"><span data-stu-id="07397-254">toocopy data from array, you can use **array[x].property** tooextract value of hello given property from hello xth object, or you can use **array[*].property** toofind hello value from any object containing such property.</span></span>

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

<span data-ttu-id="07397-255">**2. példa: közötti több objektumnak azonos mintát egy olyan tömbből, hello alkalmazása**</span><span class="sxs-lookup"><span data-stu-id="07397-255">**Sample 2: cross apply multiple objects with hello same pattern from array**</span></span>

<span data-ttu-id="07397-256">Ez a minta elvárt tootransform egy legfelső szintű JSON-objektum táblázatos eredményben több bejegyzésekbe.</span><span class="sxs-lookup"><span data-stu-id="07397-256">In this sample, you expect tootransform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="07397-257">Ha egy JSON-fájl a következő tartalmat hello:</span><span class="sxs-lookup"><span data-stu-id="07397-257">If you have a JSON file with hello following content:</span></span>  

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
<span data-ttu-id="07397-258">és szeretné, hogy azt egy Azure SQL táblázatba hello alábbi formázása, hello hello tömbben található adatok közül egybesimítását toocopy Keresztillesztés információval hello közös legfelső szintű:</span><span class="sxs-lookup"><span data-stu-id="07397-258">and you want toocopy it into an Azure SQL table in hello following format, by flattening hello data inside hello array and cross join with hello common root info:</span></span>

| <span data-ttu-id="07397-259">ordernumber</span><span class="sxs-lookup"><span data-stu-id="07397-259">ordernumber</span></span> | <span data-ttu-id="07397-260">orderdate</span><span class="sxs-lookup"><span data-stu-id="07397-260">orderdate</span></span> | <span data-ttu-id="07397-261">order_pd</span><span class="sxs-lookup"><span data-stu-id="07397-261">order_pd</span></span> | <span data-ttu-id="07397-262">order_price</span><span class="sxs-lookup"><span data-stu-id="07397-262">order_price</span></span> | <span data-ttu-id="07397-263">city</span><span class="sxs-lookup"><span data-stu-id="07397-263">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="07397-264">01</span><span class="sxs-lookup"><span data-stu-id="07397-264">01</span></span> | <span data-ttu-id="07397-265">20170122</span><span class="sxs-lookup"><span data-stu-id="07397-265">20170122</span></span> | <span data-ttu-id="07397-266">P1</span><span class="sxs-lookup"><span data-stu-id="07397-266">P1</span></span> | <span data-ttu-id="07397-267">23</span><span class="sxs-lookup"><span data-stu-id="07397-267">23</span></span> | <span data-ttu-id="07397-268">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="07397-268">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="07397-269">01</span><span class="sxs-lookup"><span data-stu-id="07397-269">01</span></span> | <span data-ttu-id="07397-270">20170122</span><span class="sxs-lookup"><span data-stu-id="07397-270">20170122</span></span> | <span data-ttu-id="07397-271">P2</span><span class="sxs-lookup"><span data-stu-id="07397-271">P2</span></span> | <span data-ttu-id="07397-272">13</span><span class="sxs-lookup"><span data-stu-id="07397-272">13</span></span> | <span data-ttu-id="07397-273">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="07397-273">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="07397-274">01</span><span class="sxs-lookup"><span data-stu-id="07397-274">01</span></span> | <span data-ttu-id="07397-275">20170122</span><span class="sxs-lookup"><span data-stu-id="07397-275">20170122</span></span> | <span data-ttu-id="07397-276">P3</span><span class="sxs-lookup"><span data-stu-id="07397-276">P3</span></span> | <span data-ttu-id="07397-277">231</span><span class="sxs-lookup"><span data-stu-id="07397-277">231</span></span> | <span data-ttu-id="07397-278">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="07397-278">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="07397-279">bemeneti adatkészlet hello **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban).</span><span class="sxs-lookup"><span data-stu-id="07397-279">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="07397-280">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="07397-280">More specifically:</span></span>

- <span data-ttu-id="07397-281">`structure`szakasz határozza meg a testreszabott hello oszlopneveket és a megfelelő adattípus hello tootabular adatok konvertálásakor.</span><span class="sxs-lookup"><span data-stu-id="07397-281">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="07397-282">Ez a szakasz **választható** kivéve toodo oszlopleképezés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="07397-282">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="07397-283">A további részletekkel kapcsolatban lásd a [Struktúrameghatározások négyszögletes adatkészletek esetén](#specifying-structure-definition-for-rectangular-datasets) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="07397-283">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="07397-284">`jsonNodeReference`azt jelzi, ugyanaz a mintát hello hello objektumok tooiterate és hogyan nyerhet ki adatokat **tömb** orderlines.</span><span class="sxs-lookup"><span data-stu-id="07397-284">`jsonNodeReference` indicates tooiterate and extract data from hello objects with hello same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="07397-285">`jsonPathDefinition`az egyes oszlopok, ahol tooextract hello adatait jelző hello JSON elérési út megadása</span><span class="sxs-lookup"><span data-stu-id="07397-285">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="07397-286">Ebben a példában a "ordernumber", "orderdate oszlopra" és "város" vannak a legfelső szintű objektum kezdve "$.", amíg a "order_pd" és "order_price" meg van határozva, származó hello tömbelem nélkül "$." elérési úttal rendelkező JSON-útvonalhoz.</span><span class="sxs-lookup"><span data-stu-id="07397-286">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from hello array element without "$.".</span></span>

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

<span data-ttu-id="07397-287">**Vegye figyelembe a következő pontok hello:**</span><span class="sxs-lookup"><span data-stu-id="07397-287">**Note hello following points:**</span></span>

* <span data-ttu-id="07397-288">Ha hello `structure` és `jsonPathDefinition` nincsenek megadva itt: hello adat-előállító adatkészlet hello másolási tevékenység észleli hello hello első objektum sémáját, és egybesimítására hello teljes objektum.</span><span class="sxs-lookup"><span data-stu-id="07397-288">If hello `structure` and `jsonPathDefinition` are not defined in hello Data Factory dataset, hello Copy Activity detects hello schema from hello first object and flatten hello whole object.</span></span>
* <span data-ttu-id="07397-289">Ha hello JSON-bevitelben tömb, alapértelmezés szerint hello másolási tevékenység alakítja hello teljes tömb érték karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="07397-289">If hello JSON input has an array, by default hello Copy Activity converts hello entire array value into a string.</span></span> <span data-ttu-id="07397-290">Választhat tooextract adatok használatával `jsonNodeReference` és/vagy `jsonPathDefinition`, vagy hagyja ki a nem ad meg a legyen `jsonPathDefinition`.</span><span class="sxs-lookup"><span data-stu-id="07397-290">You can choose tooextract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="07397-291">Ha duplikált nevek: hello ugyanazon a szinten, szerzi hello legutóbb hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="07397-291">If there are duplicate names at hello same level, hello Copy Activity picks hello last one.</span></span>
* <span data-ttu-id="07397-292">A tulajdonságnevek megkülönböztetik a kis- és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="07397-292">Property names are case-sensitive.</span></span> <span data-ttu-id="07397-293">A rendszer két azonos nevű, de eltérő kis- és nagybetűket tartalmazó tulajdonságot két különálló tulajdonságként kezel.</span><span class="sxs-lookup"><span data-stu-id="07397-293">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="07397-294">**2. eset: Adatok tooJSON fájl írása**</span><span class="sxs-lookup"><span data-stu-id="07397-294">**Case 2: Writing data tooJSON file**</span></span>

<span data-ttu-id="07397-295">Ha a következő táblával rendelkezik az SQL Database-ben:</span><span class="sxs-lookup"><span data-stu-id="07397-295">If you have below table in SQL Database:</span></span>

| <span data-ttu-id="07397-296">id</span><span class="sxs-lookup"><span data-stu-id="07397-296">id</span></span> | <span data-ttu-id="07397-297">order_date</span><span class="sxs-lookup"><span data-stu-id="07397-297">order_date</span></span> | <span data-ttu-id="07397-298">order_price</span><span class="sxs-lookup"><span data-stu-id="07397-298">order_price</span></span> | <span data-ttu-id="07397-299">order_by</span><span class="sxs-lookup"><span data-stu-id="07397-299">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="07397-300">1</span><span class="sxs-lookup"><span data-stu-id="07397-300">1</span></span> | <span data-ttu-id="07397-301">20170119</span><span class="sxs-lookup"><span data-stu-id="07397-301">20170119</span></span> | <span data-ttu-id="07397-302">2000</span><span class="sxs-lookup"><span data-stu-id="07397-302">2000</span></span> | <span data-ttu-id="07397-303">David</span><span class="sxs-lookup"><span data-stu-id="07397-303">David</span></span> |
| <span data-ttu-id="07397-304">2</span><span class="sxs-lookup"><span data-stu-id="07397-304">2</span></span> | <span data-ttu-id="07397-305">20170120</span><span class="sxs-lookup"><span data-stu-id="07397-305">20170120</span></span> | <span data-ttu-id="07397-306">3500</span><span class="sxs-lookup"><span data-stu-id="07397-306">3500</span></span> | <span data-ttu-id="07397-307">Patrick</span><span class="sxs-lookup"><span data-stu-id="07397-307">Patrick</span></span> |
| <span data-ttu-id="07397-308">3</span><span class="sxs-lookup"><span data-stu-id="07397-308">3</span></span> | <span data-ttu-id="07397-309">20170121</span><span class="sxs-lookup"><span data-stu-id="07397-309">20170121</span></span> | <span data-ttu-id="07397-310">4000</span><span class="sxs-lookup"><span data-stu-id="07397-310">4000</span></span> | <span data-ttu-id="07397-311">Jason</span><span class="sxs-lookup"><span data-stu-id="07397-311">Jason</span></span> |

<span data-ttu-id="07397-312">és az egyes rekordokhoz, várt toowrite tooa JSON-objektumból az alábbi formátumban:</span><span class="sxs-lookup"><span data-stu-id="07397-312">and for each record, you expect toowrite tooa JSON object in below format:</span></span>
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

<span data-ttu-id="07397-313">hello kimeneti adatkészlet **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban).</span><span class="sxs-lookup"><span data-stu-id="07397-313">hello output dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="07397-314">Pontosabban `structure` szakaszban definiálja a testre szabott hello tulajdonságnevek célfájlt, `nestingSeparator` (alapértelmezett érték ".") használt tooidentify hello nest réteg hello neve lesz.</span><span class="sxs-lookup"><span data-stu-id="07397-314">More specifically, `structure` section defines hello customized property names in destination file, `nestingSeparator` (default is ".") will be used tooidentify hello nest layer from hello name.</span></span> <span data-ttu-id="07397-315">Ez a szakasz **választható** kivéve, ha szeretné, hogy toochange hello tulajdonságnév összehasonlítva az forrásoszlop neve, vagy beágyazni hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="07397-315">This section is **optional** unless you want toochange hello property name comparing with source column name, or nest some of hello properties.</span></span>

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

### <a name="specifying-avroformat"></a><span data-ttu-id="07397-316">Az AvroFormat megadása</span><span class="sxs-lookup"><span data-stu-id="07397-316">Specifying AvroFormat</span></span>
<span data-ttu-id="07397-317">Ha szeretné, hogy tooparse hello az Avro-fájlok vagy az Avro formátum hello adatírás, állítsa be a hello `format` `type` tulajdonság túl**AvroFormat**.</span><span class="sxs-lookup"><span data-stu-id="07397-317">If you want tooparse hello Avro files or write hello data in Avro format, set hello `format` `type` property too**AvroFormat**.</span></span> <span data-ttu-id="07397-318">Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell.</span><span class="sxs-lookup"><span data-stu-id="07397-318">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="07397-319">Példa:</span><span class="sxs-lookup"><span data-stu-id="07397-319">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="07397-320">az Avro-formátumban toouse Hive táblákat, olvassa el a túl[Apache Hive oktatóanyag](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="07397-320">toouse Avro format in a Hive table, you can refer too[Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="07397-321">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="07397-321">Note hello following points:</span></span>  

* <span data-ttu-id="07397-322">Az [összetett adattípusok](http://avro.apache.org/docs/current/spec.html#schema_complex) nem támogatottak (rekordok, felsorolt típusok, tömbök, leképezések, egyesítések és rögzített típusok).</span><span class="sxs-lookup"><span data-stu-id="07397-322">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions and fixed).</span></span>

### <a name="specifying-orcformat"></a><span data-ttu-id="07397-323">OrcFormat megadása</span><span class="sxs-lookup"><span data-stu-id="07397-323">Specifying OrcFormat</span></span>
<span data-ttu-id="07397-324">Ha szeretné, hogy tooparse hello ORC fájlokat vagy hello adatírás ORC formátumban, állítsa be a hello `format` `type` tulajdonság túl**OrcFormat**.</span><span class="sxs-lookup"><span data-stu-id="07397-324">If you want tooparse hello ORC files or write hello data in ORC format, set hello `format` `type` property too**OrcFormat**.</span></span> <span data-ttu-id="07397-325">Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell.</span><span class="sxs-lookup"><span data-stu-id="07397-325">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="07397-326">Példa:</span><span class="sxs-lookup"><span data-stu-id="07397-326">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="07397-327">Nem másolása ORC fájlokat **,-van** között a helyszíni és felhőalapú adattároló, az átjáró gépen tooinstall hello JRE 8 (Java Runtime Environment) van szüksége.</span><span class="sxs-lookup"><span data-stu-id="07397-327">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="07397-328">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="07397-328">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="07397-329">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="07397-329">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="07397-330">Válassza ki a hello megfelelő egy.</span><span class="sxs-lookup"><span data-stu-id="07397-330">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="07397-331">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="07397-331">Note hello following points:</span></span>

* <span data-ttu-id="07397-332">Az összetett adattípusok nem támogatottak (STRUCT, MAP, LIST, UNION)</span><span class="sxs-lookup"><span data-stu-id="07397-332">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="07397-333">Az ORC-fájlok három, [tömörítéshez kapcsolódó beállítással](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/) rendelkeznek: NONE, ZLIB, SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="07397-333">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="07397-334">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="07397-334">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="07397-335">Használja hello tömörítési kodek hello metaadatok tooread hello adatok van.</span><span class="sxs-lookup"><span data-stu-id="07397-335">It uses hello compression codec is in hello metadata tooread hello data.</span></span> <span data-ttu-id="07397-336">Azonban tooan ORC fájl írásakor, adat-előállító úgy dönt, ZLIB, ez utóbbi érték az ORC hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="07397-336">However, when writing tooan ORC file, Data Factory chooses ZLIB, which is hello default for ORC.</span></span> <span data-ttu-id="07397-337">Jelenleg nincs lehetőség toooverride ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="07397-337">Currently, there is no option toooverride this behavior.</span></span>

### <a name="specifying-parquetformat"></a><span data-ttu-id="07397-338">ParquetFormat megadása</span><span class="sxs-lookup"><span data-stu-id="07397-338">Specifying ParquetFormat</span></span>
<span data-ttu-id="07397-339">Ha szeretné, hogy tooparse hello Parquet fájlok vagy hello adatírás Parquet formátumban, állítsa be a hello `format` `type` tulajdonság túl**ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="07397-339">If you want tooparse hello Parquet files or write hello data in Parquet format, set hello `format` `type` property too**ParquetFormat**.</span></span> <span data-ttu-id="07397-340">Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell.</span><span class="sxs-lookup"><span data-stu-id="07397-340">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="07397-341">Példa:</span><span class="sxs-lookup"><span data-stu-id="07397-341">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="07397-342">Nem Parquet fájlok másolása **,-van** között a helyszíni és felhőalapú adattároló, tooinstall hello JRE 8 (Java Runtime Environment) a átjáró számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="07397-342">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="07397-343">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="07397-343">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="07397-344">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="07397-344">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="07397-345">Válassza ki a hello megfelelő egy.</span><span class="sxs-lookup"><span data-stu-id="07397-345">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="07397-346">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="07397-346">Note hello following points:</span></span>

* <span data-ttu-id="07397-347">Az összetett adattípusok nem támogatottak (MAP, LIST)</span><span class="sxs-lookup"><span data-stu-id="07397-347">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="07397-348">Parquet fájl rendelkezik-e hello alábbi tömörítési kapcsolatos beállítások: NONE, SNAPPY GZIP és LZO.</span><span class="sxs-lookup"><span data-stu-id="07397-348">Parquet file has hello following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="07397-349">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="07397-349">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="07397-350">Hello tömörítési kodek hello metaadatok tooread hello adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="07397-350">It uses hello compression codec in hello metadata tooread hello data.</span></span> <span data-ttu-id="07397-351">Azonban tooa Parquet fájl írásakor, adat-előállító úgy dönt, SNAPPY, hello alapértelmezett Parquet formátum.</span><span class="sxs-lookup"><span data-stu-id="07397-351">However, when writing tooa Parquet file, Data Factory chooses SNAPPY, which is hello default for Parquet format.</span></span> <span data-ttu-id="07397-352">Jelenleg nincs lehetőség toooverride ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="07397-352">Currently, there is no option toooverride this behavior.</span></span>
