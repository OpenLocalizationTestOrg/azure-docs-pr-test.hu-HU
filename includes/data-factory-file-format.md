## <a name="specifying-formats"></a><span data-ttu-id="9af51-101">Formátumok meghatározása</span><span class="sxs-lookup"><span data-stu-id="9af51-101">Specifying formats</span></span>
<span data-ttu-id="9af51-102">Az Azure Data Factory a következő formátumtípusokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="9af51-102">Azure Data Factory supports the following format types:</span></span>

* [<span data-ttu-id="9af51-103">Szöveges formátum</span><span class="sxs-lookup"><span data-stu-id="9af51-103">Text Format</span></span>](#specifying-textformat)
* [<span data-ttu-id="9af51-104">JSON formátum</span><span class="sxs-lookup"><span data-stu-id="9af51-104">JSON Format</span></span>](#specifying-jsonformat)
* [<span data-ttu-id="9af51-105">Avro formátum</span><span class="sxs-lookup"><span data-stu-id="9af51-105">Avro Format</span></span>](#specifying-avroformat)
* [<span data-ttu-id="9af51-106">ORC formátum</span><span class="sxs-lookup"><span data-stu-id="9af51-106">ORC Format</span></span>](#specifying-orcformat)
* [<span data-ttu-id="9af51-107">Parquet formátum</span><span class="sxs-lookup"><span data-stu-id="9af51-107">Parquet Format</span></span>](#specifying-parquetformat)

### <a name="specifying-textformat"></a><span data-ttu-id="9af51-108">TextFormat megadása</span><span class="sxs-lookup"><span data-stu-id="9af51-108">Specifying TextFormat</span></span>
<span data-ttu-id="9af51-109">Ha elemezni szeretné a szöveges fájlokat, vagy szöveges formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **TextFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="9af51-109">If you want to parse the text files or write the data in text format, set the `format` `type` property to **TextFormat**.</span></span> <span data-ttu-id="9af51-110">Emellett megadhatja a következő **választható** tulajdonságokat a `format` szakaszban.</span><span class="sxs-lookup"><span data-stu-id="9af51-110">You can also specify the following **optional** properties in the `format` section.</span></span> <span data-ttu-id="9af51-111">A konfigurálással kapcsolatban lásd [A TextFormat használatát bemutató példa](#textformat-example) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="9af51-111">See [TextFormat example](#textformat-example) section on how to configure.</span></span>

| <span data-ttu-id="9af51-112">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9af51-112">Property</span></span> | <span data-ttu-id="9af51-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="9af51-113">Description</span></span> | <span data-ttu-id="9af51-114">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="9af51-114">Allowed values</span></span> | <span data-ttu-id="9af51-115">Kötelező</span><span class="sxs-lookup"><span data-stu-id="9af51-115">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9af51-116">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="9af51-116">columnDelimiter</span></span> |<span data-ttu-id="9af51-117">A fájlokban az oszlopok elválasztására használt karakter.</span><span class="sxs-lookup"><span data-stu-id="9af51-117">The character used to separate columns in a file.</span></span> <span data-ttu-id="9af51-118">Érdemes egy ritka, nem nyomtatható karaktert választania, amely valószínűleg nem szerepel az adatokban, például adja meg a „\u0001” értéket, amely a fejléc kezdetét jelzi.</span><span class="sxs-lookup"><span data-stu-id="9af51-118">You can consider to use a rare unprintable char which not likely exists in your data: e.g. specify "\u0001" which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="9af51-119">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="9af51-119">Only one character is allowed.</span></span> <span data-ttu-id="9af51-120">Az **alapértelmezett** érték a **vessző (,)**.</span><span class="sxs-lookup"><span data-stu-id="9af51-120">The **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="9af51-121">Unicode-karakterek használatához tekintse meg a megfelelő kódot az [Unicode-karakterek listájában](https://en.wikipedia.org/wiki/List_of_Unicode_characters).</span><span class="sxs-lookup"><span data-stu-id="9af51-121">To use an Unicode character, refer to [Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) to get the corresponding code for it.</span></span> |<span data-ttu-id="9af51-122">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-122">No</span></span> |
| <span data-ttu-id="9af51-123">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="9af51-123">rowDelimiter</span></span> |<span data-ttu-id="9af51-124">A fájlokban a sorok elválasztására használt karakter.</span><span class="sxs-lookup"><span data-stu-id="9af51-124">The character used to separate rows in a file.</span></span> |<span data-ttu-id="9af51-125">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="9af51-125">Only one character is allowed.</span></span> <span data-ttu-id="9af51-126">Az **alapértelmezett** érték olvasáskor a következő értékek bármelyike: **[„\r\n”, „\r”, „\n”]**, illetve **„\r\n”** írás esetén.</span><span class="sxs-lookup"><span data-stu-id="9af51-126">The **default** value is any of the following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="9af51-127">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-127">No</span></span> |
| <span data-ttu-id="9af51-128">escapeChar</span><span class="sxs-lookup"><span data-stu-id="9af51-128">escapeChar</span></span> |<span data-ttu-id="9af51-129">Az oszlophatároló feloldására szolgáló speciális karakter a bemeneti fájl tartalmában.</span><span class="sxs-lookup"><span data-stu-id="9af51-129">The special character used to escape a column delimiter in the content of input file.</span></span> <br/><br/><span data-ttu-id="9af51-130">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="9af51-130">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="9af51-131">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="9af51-131">Only one character is allowed.</span></span> <span data-ttu-id="9af51-132">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="9af51-132">No default value.</span></span> <br/><br/><span data-ttu-id="9af51-133">Ha például vessző (,) az oszlophatároló, de a vessző karaktert szeretné megjeleníteni a szövegben (például: „Helló, világ”), megadhatja a „$” karakter feloldójelként, és a „Helló$, világ” karakterláncot használhatja a forrásban.</span><span class="sxs-lookup"><span data-stu-id="9af51-133">Example: if you have comma (',') as the column delimiter but you want to have the comma character in the text (example: "Hello, world"), you can define ‘$’ as the escape character and use string "Hello$, world" in the source.</span></span> |<span data-ttu-id="9af51-134">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-134">No</span></span> |
| <span data-ttu-id="9af51-135">quoteChar</span><span class="sxs-lookup"><span data-stu-id="9af51-135">quoteChar</span></span> |<span data-ttu-id="9af51-136">Egy karakterláncérték idézéséhez használt karakter.</span><span class="sxs-lookup"><span data-stu-id="9af51-136">The character used to quote a string value.</span></span> <span data-ttu-id="9af51-137">Ekkor az idézőjel-karakterek közötti oszlop- és sorhatárolókat a rendszer a karakterláncérték részeként kezeli.</span><span class="sxs-lookup"><span data-stu-id="9af51-137">The column and row delimiters inside the quote characters would be treated as part of the string value.</span></span> <span data-ttu-id="9af51-138">Ez a tulajdonság a bemeneti és a kimeneti adatkészleteken is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="9af51-138">This property is applicable to both input and output datasets.</span></span><br/><br/><span data-ttu-id="9af51-139">Egy táblához nem határozható meg az escapeChar és a quoteChar is.</span><span class="sxs-lookup"><span data-stu-id="9af51-139">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="9af51-140">Csak egy karakter használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="9af51-140">Only one character is allowed.</span></span> <span data-ttu-id="9af51-141">Nincs alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="9af51-141">No default value.</span></span> <br/><br/><span data-ttu-id="9af51-142">Ha például vessző (,) az oszlophatároló, de a vessző karaktert szeretné megjeleníteni a szövegben (például: <Helló, világ>), megadhatja a " (angol dupla idézőjel) értéket idézőjel-karakterként, és a "Helló$, világ" karakterláncot használhatja a forrásban.</span><span class="sxs-lookup"><span data-stu-id="9af51-142">For example, if you have comma (',') as the column delimiter but you want to have comma character in the text (example: <Hello, world>), you can define " (double quote) as the quote character and use the string "Hello, world" in the source.</span></span> |<span data-ttu-id="9af51-143">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-143">No</span></span> |
| <span data-ttu-id="9af51-144">nullValue</span><span class="sxs-lookup"><span data-stu-id="9af51-144">nullValue</span></span> |<span data-ttu-id="9af51-145">A null értéket jelölő egy vagy több karakter.</span><span class="sxs-lookup"><span data-stu-id="9af51-145">One or more characters used to represent a null value.</span></span> |<span data-ttu-id="9af51-146">Egy vagy több karakter.</span><span class="sxs-lookup"><span data-stu-id="9af51-146">One or more characters.</span></span> <span data-ttu-id="9af51-147">Az **alapértelmezett** értékek az **„\N” és „NULL”** olvasás, illetve **„\N”** írás esetén.</span><span class="sxs-lookup"><span data-stu-id="9af51-147">The **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="9af51-148">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-148">No</span></span> |
| <span data-ttu-id="9af51-149">encodingName</span><span class="sxs-lookup"><span data-stu-id="9af51-149">encodingName</span></span> |<span data-ttu-id="9af51-150">A kódolási név megadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="9af51-150">Specify the encoding name.</span></span> |<span data-ttu-id="9af51-151">Egy érvényes kódolási név.</span><span class="sxs-lookup"><span data-stu-id="9af51-151">A valid encoding name.</span></span> <span data-ttu-id="9af51-152">Lásd az [Encoding.EncodingName tulajdonságot](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="9af51-152">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="9af51-153">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="9af51-153">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="9af51-154">Az **alapértelmezett** érték az **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="9af51-154">The **default** value is **UTF-8**.</span></span> |<span data-ttu-id="9af51-155">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-155">No</span></span> |
| <span data-ttu-id="9af51-156">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="9af51-156">firstRowAsHeader</span></span> |<span data-ttu-id="9af51-157">Megadja, hogy az első sort fejlécnek kell-e tekinteni.</span><span class="sxs-lookup"><span data-stu-id="9af51-157">Specifies whether to consider the first row as a header.</span></span> <span data-ttu-id="9af51-158">A bemeneti adatkészletek első sorát a Data Factory fejlécként olvassa be.</span><span class="sxs-lookup"><span data-stu-id="9af51-158">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="9af51-159">A kimeneti adatkészletek első sorát a Data Factory fejlécként írja ki.</span><span class="sxs-lookup"><span data-stu-id="9af51-159">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="9af51-160">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="9af51-160">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="9af51-161">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="9af51-161">True</span></span><br/><span data-ttu-id="9af51-162">**False (alapértelmezett)**</span><span class="sxs-lookup"><span data-stu-id="9af51-162">**False (default)**</span></span> |<span data-ttu-id="9af51-163">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-163">No</span></span> |
| <span data-ttu-id="9af51-164">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="9af51-164">skipLineCount</span></span> |<span data-ttu-id="9af51-165">Az adatok bemeneti fájlokból való olvasásakor kihagyandó sorok számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="9af51-165">Indicates the number of rows to skip when reading data from input files.</span></span> <span data-ttu-id="9af51-166">Ha a skipLineCount és a firstRowAsHeader tulajdonság is meg van adva, a rendszer először kihagyja a sorokat, majd beolvassa a fejléc-információkat a bemeneti fájlból.</span><span class="sxs-lookup"><span data-stu-id="9af51-166">If both skipLineCount and firstRowAsHeader are specified, the lines are skipped first and then the header information is read from the input file.</span></span> <br/><br/><span data-ttu-id="9af51-167">[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="9af51-167">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="9af51-168">Egész szám</span><span class="sxs-lookup"><span data-stu-id="9af51-168">Integer</span></span> |<span data-ttu-id="9af51-169">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-169">No</span></span> |
| <span data-ttu-id="9af51-170">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="9af51-170">treatEmptyAsNull</span></span> |<span data-ttu-id="9af51-171">Meghatározza, hogy az adatok bemeneti fájlból történő olvasásakor a null vagy üres értékeket null értékként kell-e kezelni.</span><span class="sxs-lookup"><span data-stu-id="9af51-171">Specifies whether to treat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="9af51-172">**True (alapértelmezett)**</span><span class="sxs-lookup"><span data-stu-id="9af51-172">**True (default)**</span></span><br/><span data-ttu-id="9af51-173">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="9af51-173">False</span></span> |<span data-ttu-id="9af51-174">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-174">No</span></span> |

#### <a name="textformat-example"></a><span data-ttu-id="9af51-175">A TextFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="9af51-175">TextFormat example</span></span>
<span data-ttu-id="9af51-176">A következő minta bemutatja a TextFormat néhány formázási tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="9af51-176">The following sample shows some of the format properties for TextFormat.</span></span>

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

<span data-ttu-id="9af51-177">`quoteChar` helyett `quoteChar` használatához cserélje le a sort `escapeChar` értékre a következő escapeChar kifejezéssel:</span><span class="sxs-lookup"><span data-stu-id="9af51-177">To use an `escapeChar` instead of `quoteChar`, replace the line with `quoteChar` with the following escapeChar:</span></span>

```json
"escapeChar": "$",
```

#### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="9af51-178">A firstRowAsHeader és a skipLineCount használatára vonatkozó forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="9af51-178">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="9af51-179">Egy nem fájlalapú forrásból másol egy szöveges fájlba, és szeretne hozzáadni egy fejlécsort, amely tartalmazza a séma (például SQL-séma) metaadatait.</span><span class="sxs-lookup"><span data-stu-id="9af51-179">You are copying from a non-file source to a text file and would like to add a header line containing the schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="9af51-180">Ebben a forgatókönyvben adja meg a `firstRowAsHeader` értékét igazként a kimeneti adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="9af51-180">Specify `firstRowAsHeader` as true in the output dataset for this scenario.</span></span>
* <span data-ttu-id="9af51-181">Egy fejlécsort tartalmazó szöveges fájlból másol egy nem fájlalapú fogadóba, és el szeretné hagyni azt a sort.</span><span class="sxs-lookup"><span data-stu-id="9af51-181">You are copying from a text file containing a header line to a non-file sink and would like to drop that line.</span></span> <span data-ttu-id="9af51-182">Adja meg a `firstRowAsHeader` értékét igazként a bemeneti adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="9af51-182">Specify `firstRowAsHeader` as true in the input dataset.</span></span>
* <span data-ttu-id="9af51-183">Egy szöveges fájlból másol, és szeretne kihagyni néhány sort az elejéről, amelyek nem tartalmaznak adatokat vagy fejléc-információkat.</span><span class="sxs-lookup"><span data-stu-id="9af51-183">You are copying from a text file and want to skip a few lines at the beginning that contain no data or header information.</span></span> <span data-ttu-id="9af51-184">Adja meg a `skipLineCount` értékét a kihagyni kívánt sorok számának jelzéséhez.</span><span class="sxs-lookup"><span data-stu-id="9af51-184">Specify `skipLineCount` to indicate the number of lines to be skipped.</span></span> <span data-ttu-id="9af51-185">Ha a fájl hátralévő része fejlécsort tartalmaz, a `firstRowAsHeader` is megadható.</span><span class="sxs-lookup"><span data-stu-id="9af51-185">If the rest of the file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="9af51-186">Ha a `skipLineCount` és a `firstRowAsHeader` is meg van adva, a rendszer először kihagyja a sorokat, majd beolvassa a fejléc-információkat a bemeneti fájlból</span><span class="sxs-lookup"><span data-stu-id="9af51-186">If both `skipLineCount` and `firstRowAsHeader` are specified, the lines are skipped first and then the header information is read from the input file</span></span>

### <a name="specifying-jsonformat"></a><span data-ttu-id="9af51-187">JsonFormat megadása</span><span class="sxs-lookup"><span data-stu-id="9af51-187">Specifying JsonFormat</span></span>
<span data-ttu-id="9af51-188">A **importálási/exportálási JSON-fájlok használata-a/Azure Cosmos DB van**, lásd: [importálási/exportálási JSON-dokumentumok](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) című szakaszában találhat az Azure Cosmos DB összekötő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="9af51-188">To **import/export JSON files as-is into/from Azure Cosmos DB**, see [Import/export JSON documents](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) section in the Azure Cosmos DB connector with details.</span></span>

<span data-ttu-id="9af51-189">Ha elemezni szeretné a JSON-fájlokat, vagy JSON formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **JsonFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="9af51-189">If you want to parse the JSON files or write the data in JSON format, set the `format` `type` property to **JsonFormat**.</span></span> <span data-ttu-id="9af51-190">Emellett megadhatja a következő **választható** tulajdonságokat a `format` szakaszban.</span><span class="sxs-lookup"><span data-stu-id="9af51-190">You can also specify the following **optional** properties in the `format` section.</span></span> <span data-ttu-id="9af51-191">A konfigurálással kapcsolatban lásd [A JsonFormat használatát bemutató példa](#jsonformat-example) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="9af51-191">See [JsonFormat example](#jsonformat-example) section on how to configure.</span></span>

| <span data-ttu-id="9af51-192">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9af51-192">Property</span></span> | <span data-ttu-id="9af51-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="9af51-193">Description</span></span> | <span data-ttu-id="9af51-194">Kötelező</span><span class="sxs-lookup"><span data-stu-id="9af51-194">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9af51-195">filePattern</span><span class="sxs-lookup"><span data-stu-id="9af51-195">filePattern</span></span> |<span data-ttu-id="9af51-196">Az egyes JSON-fájlokban tárolt adatok mintáját jelzi.</span><span class="sxs-lookup"><span data-stu-id="9af51-196">Indicate the pattern of data stored in each JSON file.</span></span> <span data-ttu-id="9af51-197">Az engedélyezett értékek a következők: **setOfObjects** és **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="9af51-197">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="9af51-198">Az **alapértelmezett** érték a **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="9af51-198">The **default** value is **setOfObjects**.</span></span> <span data-ttu-id="9af51-199">A mintákkal kapcsolatban lásd a [JSON-fájlminták](#json-file-patterns) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="9af51-199">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="9af51-200">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-200">No</span></span> |
| <span data-ttu-id="9af51-201">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="9af51-201">jsonNodeReference</span></span> | <span data-ttu-id="9af51-202">Ha egy azonos mintával rendelkező tömbmezőben található objektumokat szeretne iterálni, vagy azokból adatokat kinyerni, adja meg a tömb JSON-útvonalát.</span><span class="sxs-lookup"><span data-stu-id="9af51-202">If you want to iterate and extract data from the objects inside an array field with the same pattern, specify the JSON path of that array.</span></span> <span data-ttu-id="9af51-203">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="9af51-203">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="9af51-204">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-204">No</span></span> |
| <span data-ttu-id="9af51-205">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="9af51-205">jsonPathDefinition</span></span> | <span data-ttu-id="9af51-206">Megadja az egyes oszlopmegfeleltetések JSON-útvonalának kifejezését testre szabott oszlopnevekkel (kezdje kisbetűvel).</span><span class="sxs-lookup"><span data-stu-id="9af51-206">Specify the JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="9af51-207">Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat, és ki tud nyerni adatokat objektumokból vagy tömbökből.</span><span class="sxs-lookup"><span data-stu-id="9af51-207">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="9af51-208">A gyökérobjektum alatti mezők esetében kezdjen a gyökér $ értékkel. A `jsonNodeReference` tulajdonság által kiválasztott tömbben lévő mezők esetében kezdjen a tömbelemmel.</span><span class="sxs-lookup"><span data-stu-id="9af51-208">For fields under root object, start with root $; for fields inside the array chosen by `jsonNodeReference` property, start from the array element.</span></span> <span data-ttu-id="9af51-209">A konfigurálással kapcsolatban lásd [A JsonFormat használatát bemutató példa](#jsonformat-example) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="9af51-209">See [JsonFormat example](#jsonformat-example) section on how to configure.</span></span> | <span data-ttu-id="9af51-210">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-210">No</span></span> |
| <span data-ttu-id="9af51-211">encodingName</span><span class="sxs-lookup"><span data-stu-id="9af51-211">encodingName</span></span> |<span data-ttu-id="9af51-212">A kódolási név megadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="9af51-212">Specify the encoding name.</span></span> <span data-ttu-id="9af51-213">Az érvényes kódolási nevekkel kapcsolatban lásd az [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="9af51-213">For the list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="9af51-214">Például: windows-1250 vagy shift_jis.</span><span class="sxs-lookup"><span data-stu-id="9af51-214">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="9af51-215">Az **alapértelmezett** érték az **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="9af51-215">The **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="9af51-216">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-216">No</span></span> |
| <span data-ttu-id="9af51-217">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="9af51-217">nestingSeparator</span></span> |<span data-ttu-id="9af51-218">A beágyazási szinteket elválasztó karakter.</span><span class="sxs-lookup"><span data-stu-id="9af51-218">Character that is used to separate nesting levels.</span></span> <span data-ttu-id="9af51-219">Az alapértelmezett érték a „.” (pont).</span><span class="sxs-lookup"><span data-stu-id="9af51-219">The default value is '.' (dot).</span></span> |<span data-ttu-id="9af51-220">Nem</span><span class="sxs-lookup"><span data-stu-id="9af51-220">No</span></span> |

#### <a name="json-file-patterns"></a><span data-ttu-id="9af51-221">JSON-fájlminták</span><span class="sxs-lookup"><span data-stu-id="9af51-221">JSON file patterns</span></span>

<span data-ttu-id="9af51-222">A másolási tevékenység a JSON-fájlok következő mintáit tudja elemezni:</span><span class="sxs-lookup"><span data-stu-id="9af51-222">Copy activity can parse below patterns of JSON files:</span></span>

- <span data-ttu-id="9af51-223">**I. típus: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="9af51-223">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="9af51-224">Minden fájl egyetlen objektumot, illetve több, sorokkal határolt/összefűzött objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9af51-224">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="9af51-225">Ha ezt a lehetőséget választja egy kimeneti adatkészletben, a másolási tevékenység egyetlen JSON-fájlt állít elő, soronként egy objektummal (sorokkal határolt).</span><span class="sxs-lookup"><span data-stu-id="9af51-225">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="9af51-226">**példa egy objektumot tartalmazó JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="9af51-226">**single object JSON example**</span></span>

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

    * <span data-ttu-id="9af51-227">**példa sorokkal határolt JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="9af51-227">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="9af51-228">**példa összefűzött JSON-fájlra**</span><span class="sxs-lookup"><span data-stu-id="9af51-228">**concatenated JSON example**</span></span>

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

- <span data-ttu-id="9af51-229">**II. típus: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="9af51-229">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="9af51-230">Minden fájl objektumok egy tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9af51-230">Each file contains an array of objects.</span></span>

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

#### <a name="jsonformat-example"></a><span data-ttu-id="9af51-231">A JsonFormat használatát bemutató példa</span><span class="sxs-lookup"><span data-stu-id="9af51-231">JsonFormat example</span></span>

<span data-ttu-id="9af51-232">**1. eset: Adatok másolása JSON-fájlokból**</span><span class="sxs-lookup"><span data-stu-id="9af51-232">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="9af51-233">Alább láthatja a figyelembe veendő általános pontokat, valamint két példatípust az adatok JSON-fájlokból való másolásáról:</span><span class="sxs-lookup"><span data-stu-id="9af51-233">See below two types of samples when copying data from JSON files, and the generic points to note:</span></span>

<span data-ttu-id="9af51-234">**1. példa: adatok kigyűjtése objektumból és tömbből**</span><span class="sxs-lookup"><span data-stu-id="9af51-234">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="9af51-235">Ebben a példában egy JSON-gyökérobjektum képződik le egyetlen rekordba táblázatos nézetben.</span><span class="sxs-lookup"><span data-stu-id="9af51-235">In this sample, you expect one root JSON object maps to single record in tabular result.</span></span> <span data-ttu-id="9af51-236">Ha a JSON-fájl a következőt tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9af51-236">If you have a JSON file with the following content:</span></span>  

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
<span data-ttu-id="9af51-237">és az adatok objektumokból és tömbökből való kigyűjtésével szeretné átmásolni egy Azure SQL-táblába az alábbi formátumban:</span><span class="sxs-lookup"><span data-stu-id="9af51-237">and you want to copy it into an Azure SQL table in the following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="9af51-238">id</span><span class="sxs-lookup"><span data-stu-id="9af51-238">id</span></span> | <span data-ttu-id="9af51-239">deviceType</span><span class="sxs-lookup"><span data-stu-id="9af51-239">deviceType</span></span> | <span data-ttu-id="9af51-240">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="9af51-240">targetResourceType</span></span> | <span data-ttu-id="9af51-241">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="9af51-241">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="9af51-242">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="9af51-242">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="9af51-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="9af51-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="9af51-244">PC</span><span class="sxs-lookup"><span data-stu-id="9af51-244">PC</span></span> | <span data-ttu-id="9af51-245">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="9af51-245">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="9af51-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="9af51-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="9af51-247">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="9af51-247">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="9af51-248">A **JsonFormat** típusú bemeneti adatkészlet a következőképpen van meghatározva (részleges meghatározás, csak a fontos részekkel).</span><span class="sxs-lookup"><span data-stu-id="9af51-248">The input dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="9af51-249">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="9af51-249">More specifically:</span></span>

- <span data-ttu-id="9af51-250">A `structure` szakasz határozza meg a testre szabott oszlopneveket és a megfelelő adattípusokat, miközben átalakítja őket táblázatos adatokká.</span><span class="sxs-lookup"><span data-stu-id="9af51-250">`structure` section defines the customized column names and the corresponding data type while converting to tabular data.</span></span> <span data-ttu-id="9af51-251">Ez a szakasz **nem kötelező**, kivéve, ha oszlopleképezést kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="9af51-251">This section is **optional** unless you need to do column mapping.</span></span> <span data-ttu-id="9af51-252">A további részletekkel kapcsolatban lásd a [Struktúrameghatározások négyszögletes adatkészletek esetén](#specifying-structure-definition-for-rectangular-datasets) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="9af51-252">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="9af51-253">A `jsonPathDefinition` határozza meg az egyes oszlopok JSON-útvonalát, amely jelzi, hogy honnan történjen az adatok kinyerése.</span><span class="sxs-lookup"><span data-stu-id="9af51-253">`jsonPathDefinition` specifies the JSON path for each column indicating where to extract the data from.</span></span> <span data-ttu-id="9af51-254">Ha adatokat szeretne másolni egy tömbből, az **array[x].property** használatával kinyerheti az adott tulajdonság értékét az x. objektumból, vagy az **array[*].property** használatával rákereshet az értékre az ilyen tulajdonságot tartalmazó összes objektumban.</span><span class="sxs-lookup"><span data-stu-id="9af51-254">To copy data from array, you can use **array[x].property** to extract value of the given property from the xth object, or you can use **array[*].property** to find the value from any object containing such property.</span></span>

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

<span data-ttu-id="9af51-255">**2. példa: a tömbből származó ugyanazon minta keresztalkalmazása több objektumra**</span><span class="sxs-lookup"><span data-stu-id="9af51-255">**Sample 2: cross apply multiple objects with the same pattern from array**</span></span>

<span data-ttu-id="9af51-256">Ebben a példában egy JSON-gyökérobjektumot alakít át több rekorddá táblázatos nézetben.</span><span class="sxs-lookup"><span data-stu-id="9af51-256">In this sample, you expect to transform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="9af51-257">Ha a JSON-fájl a következőt tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9af51-257">If you have a JSON file with the following content:</span></span>  

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
<span data-ttu-id="9af51-258">és szeretné átmásolni egy Azure SQL-táblába az alábbi formátumban, a tömbben lévő adatok egybesimításával, valamint keresztillesztést létrehozni a közös gyökérinformációval:</span><span class="sxs-lookup"><span data-stu-id="9af51-258">and you want to copy it into an Azure SQL table in the following format, by flattening the data inside the array and cross join with the common root info:</span></span>

| <span data-ttu-id="9af51-259">ordernumber</span><span class="sxs-lookup"><span data-stu-id="9af51-259">ordernumber</span></span> | <span data-ttu-id="9af51-260">orderdate</span><span class="sxs-lookup"><span data-stu-id="9af51-260">orderdate</span></span> | <span data-ttu-id="9af51-261">order_pd</span><span class="sxs-lookup"><span data-stu-id="9af51-261">order_pd</span></span> | <span data-ttu-id="9af51-262">order_price</span><span class="sxs-lookup"><span data-stu-id="9af51-262">order_price</span></span> | <span data-ttu-id="9af51-263">city</span><span class="sxs-lookup"><span data-stu-id="9af51-263">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="9af51-264">01</span><span class="sxs-lookup"><span data-stu-id="9af51-264">01</span></span> | <span data-ttu-id="9af51-265">20170122</span><span class="sxs-lookup"><span data-stu-id="9af51-265">20170122</span></span> | <span data-ttu-id="9af51-266">P1</span><span class="sxs-lookup"><span data-stu-id="9af51-266">P1</span></span> | <span data-ttu-id="9af51-267">23</span><span class="sxs-lookup"><span data-stu-id="9af51-267">23</span></span> | <span data-ttu-id="9af51-268">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="9af51-268">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="9af51-269">01</span><span class="sxs-lookup"><span data-stu-id="9af51-269">01</span></span> | <span data-ttu-id="9af51-270">20170122</span><span class="sxs-lookup"><span data-stu-id="9af51-270">20170122</span></span> | <span data-ttu-id="9af51-271">P2</span><span class="sxs-lookup"><span data-stu-id="9af51-271">P2</span></span> | <span data-ttu-id="9af51-272">13</span><span class="sxs-lookup"><span data-stu-id="9af51-272">13</span></span> | <span data-ttu-id="9af51-273">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="9af51-273">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="9af51-274">01</span><span class="sxs-lookup"><span data-stu-id="9af51-274">01</span></span> | <span data-ttu-id="9af51-275">20170122</span><span class="sxs-lookup"><span data-stu-id="9af51-275">20170122</span></span> | <span data-ttu-id="9af51-276">P3</span><span class="sxs-lookup"><span data-stu-id="9af51-276">P3</span></span> | <span data-ttu-id="9af51-277">231</span><span class="sxs-lookup"><span data-stu-id="9af51-277">231</span></span> | <span data-ttu-id="9af51-278">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="9af51-278">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="9af51-279">A **JsonFormat** típusú bemeneti adatkészlet a következőképpen van meghatározva (részleges meghatározás, csak a fontos részekkel).</span><span class="sxs-lookup"><span data-stu-id="9af51-279">The input dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="9af51-280">Pontosabban:</span><span class="sxs-lookup"><span data-stu-id="9af51-280">More specifically:</span></span>

- <span data-ttu-id="9af51-281">A `structure` szakasz határozza meg a testre szabott oszlopneveket és a megfelelő adattípusokat, miközben átalakítja őket táblázatos adatokká.</span><span class="sxs-lookup"><span data-stu-id="9af51-281">`structure` section defines the customized column names and the corresponding data type while converting to tabular data.</span></span> <span data-ttu-id="9af51-282">Ez a szakasz **nem kötelező**, kivéve, ha oszlopleképezést kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="9af51-282">This section is **optional** unless you need to do column mapping.</span></span> <span data-ttu-id="9af51-283">A további részletekkel kapcsolatban lásd a [Struktúrameghatározások négyszögletes adatkészletek esetén](#specifying-structure-definition-for-rectangular-datasets) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="9af51-283">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="9af51-284">A `jsonNodeReference` jelzi az orderlines **tömb** alatti, azonos mintával rendelkező objektumok iterálását, illetve az adatok azokból való kinyerését.</span><span class="sxs-lookup"><span data-stu-id="9af51-284">`jsonNodeReference` indicates to iterate and extract data from the objects with the same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="9af51-285">A `jsonPathDefinition` határozza meg az egyes oszlopok JSON-útvonalát, amely jelzi, hogy honnan történjen az adatok kinyerése.</span><span class="sxs-lookup"><span data-stu-id="9af51-285">`jsonPathDefinition` specifies the JSON path for each column indicating where to extract the data from.</span></span> <span data-ttu-id="9af51-286">Ebben a példában az „ordernumber”, az „orderdate” és a „city” a „$.” értékkel kezdődő JSON-útvonallal jelzett gyökérobjektum alatt találhatók, míg az „order_pd” és az „order_price” a „$.” értéket nem tartalmazó tömbelemből származtatott útvonallal vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="9af51-286">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from the array element without "$.".</span></span>

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

<span data-ttu-id="9af51-287">**Vegye figyelembe a következő szempontokat:**</span><span class="sxs-lookup"><span data-stu-id="9af51-287">**Note the following points:**</span></span>

* <span data-ttu-id="9af51-288">Ha a `structure` és a `jsonPathDefinition` nincs meghatározva a Data Factory-adatkészletben, a másolási tevékenység az első objektumból észleli a sémát, és a teljes objektumot egybesimítja.</span><span class="sxs-lookup"><span data-stu-id="9af51-288">If the `structure` and `jsonPathDefinition` are not defined in the Data Factory dataset, the Copy Activity detects the schema from the first object and flatten the whole object.</span></span>
* <span data-ttu-id="9af51-289">Ha a JSON-bemenet egy tömböt tartalmaz, a másolási tevékenység alapértelmezés szerint a tömb teljes értékét egy karakterlánccá alakítja át.</span><span class="sxs-lookup"><span data-stu-id="9af51-289">If the JSON input has an array, by default the Copy Activity converts the entire array value into a string.</span></span> <span data-ttu-id="9af51-290">Választhatja, hogy a `jsonNodeReference` és/vagy a `jsonPathDefinition` használatával kívánja kinyerni belőle az adatokat, vagy ki is hagyhatja ezt a műveletet, ha nem adja meg a `jsonPathDefinition` értékét.</span><span class="sxs-lookup"><span data-stu-id="9af51-290">You can choose to extract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="9af51-291">Ha ismétlődő nevek szerepelnek ugyanazon a szinten, a másolási tevékenység az utolsót választja ki.</span><span class="sxs-lookup"><span data-stu-id="9af51-291">If there are duplicate names at the same level, the Copy Activity picks the last one.</span></span>
* <span data-ttu-id="9af51-292">A tulajdonságnevek megkülönböztetik a kis- és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="9af51-292">Property names are case-sensitive.</span></span> <span data-ttu-id="9af51-293">A rendszer két azonos nevű, de eltérő kis- és nagybetűket tartalmazó tulajdonságot két különálló tulajdonságként kezel.</span><span class="sxs-lookup"><span data-stu-id="9af51-293">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="9af51-294">**2. eset: Adatok írása JSON-fájlba**</span><span class="sxs-lookup"><span data-stu-id="9af51-294">**Case 2: Writing data to JSON file**</span></span>

<span data-ttu-id="9af51-295">Ha a következő táblával rendelkezik az SQL Database-ben:</span><span class="sxs-lookup"><span data-stu-id="9af51-295">If you have below table in SQL Database:</span></span>

| <span data-ttu-id="9af51-296">id</span><span class="sxs-lookup"><span data-stu-id="9af51-296">id</span></span> | <span data-ttu-id="9af51-297">order_date</span><span class="sxs-lookup"><span data-stu-id="9af51-297">order_date</span></span> | <span data-ttu-id="9af51-298">order_price</span><span class="sxs-lookup"><span data-stu-id="9af51-298">order_price</span></span> | <span data-ttu-id="9af51-299">order_by</span><span class="sxs-lookup"><span data-stu-id="9af51-299">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9af51-300">1</span><span class="sxs-lookup"><span data-stu-id="9af51-300">1</span></span> | <span data-ttu-id="9af51-301">20170119</span><span class="sxs-lookup"><span data-stu-id="9af51-301">20170119</span></span> | <span data-ttu-id="9af51-302">2000</span><span class="sxs-lookup"><span data-stu-id="9af51-302">2000</span></span> | <span data-ttu-id="9af51-303">David</span><span class="sxs-lookup"><span data-stu-id="9af51-303">David</span></span> |
| <span data-ttu-id="9af51-304">2</span><span class="sxs-lookup"><span data-stu-id="9af51-304">2</span></span> | <span data-ttu-id="9af51-305">20170120</span><span class="sxs-lookup"><span data-stu-id="9af51-305">20170120</span></span> | <span data-ttu-id="9af51-306">3500</span><span class="sxs-lookup"><span data-stu-id="9af51-306">3500</span></span> | <span data-ttu-id="9af51-307">Patrick</span><span class="sxs-lookup"><span data-stu-id="9af51-307">Patrick</span></span> |
| <span data-ttu-id="9af51-308">3</span><span class="sxs-lookup"><span data-stu-id="9af51-308">3</span></span> | <span data-ttu-id="9af51-309">20170121</span><span class="sxs-lookup"><span data-stu-id="9af51-309">20170121</span></span> | <span data-ttu-id="9af51-310">4000</span><span class="sxs-lookup"><span data-stu-id="9af51-310">4000</span></span> | <span data-ttu-id="9af51-311">Jason</span><span class="sxs-lookup"><span data-stu-id="9af51-311">Jason</span></span> |

<span data-ttu-id="9af51-312">és minden egyes objektum esetében egy JSON-objektum írását várja az alábbi formátumban:</span><span class="sxs-lookup"><span data-stu-id="9af51-312">and for each record, you expect to write to a JSON object in below format:</span></span>
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

<span data-ttu-id="9af51-313">A **JsonFormat** típusú kimeneti adatkészlet a következőképpen van meghatározva (részleges meghatározás, csak a fontos részekkel).</span><span class="sxs-lookup"><span data-stu-id="9af51-313">The output dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="9af51-314">Pontosabban, a `structure` szakasz definiálja a testre szabott tulajdonságneveket a célfájlban, a `nestingSeparator` (az alapértelmezett: „.”) pedig a beágyazási szintet különbözteti meg a névtől.</span><span class="sxs-lookup"><span data-stu-id="9af51-314">More specifically, `structure` section defines the customized property names in destination file, `nestingSeparator` (default is ".") will be used to identify the nest layer from the name.</span></span> <span data-ttu-id="9af51-315">Ez a szakasz **nem kötelező**, kivéve, ha módosítani szeretné a tulajdonság nevét a forrásoszlop nevéhez képest, vagy egyes tulajdonságokat egymásba szeretne ágyazni.</span><span class="sxs-lookup"><span data-stu-id="9af51-315">This section is **optional** unless you want to change the property name comparing with source column name, or nest some of the properties.</span></span>

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

### <a name="specifying-avroformat"></a><span data-ttu-id="9af51-316">Az AvroFormat megadása</span><span class="sxs-lookup"><span data-stu-id="9af51-316">Specifying AvroFormat</span></span>
<span data-ttu-id="9af51-317">Ha elemezni szeretné a Avro-fájlokat, vagy Avro formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **AvroFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="9af51-317">If you want to parse the Avro files or write the data in Avro format, set the `format` `type` property to **AvroFormat**.</span></span> <span data-ttu-id="9af51-318">Nem kell meghatároznia semmilyen tulajdonságot a Format szakaszban a typeProperties szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="9af51-318">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="9af51-319">Példa:</span><span class="sxs-lookup"><span data-stu-id="9af51-319">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="9af51-320">Az Avro formátum Hive-táblákban való használatával kapcsolatban lásd az [Apache Hive oktatóanyagát](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="9af51-320">To use Avro format in a Hive table, you can refer to [Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="9af51-321">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="9af51-321">Note the following points:</span></span>  

* <span data-ttu-id="9af51-322">Az [összetett adattípusok](http://avro.apache.org/docs/current/spec.html#schema_complex) nem támogatottak (rekordok, felsorolt típusok, tömbök, leképezések, egyesítések és rögzített típusok).</span><span class="sxs-lookup"><span data-stu-id="9af51-322">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions and fixed).</span></span>

### <a name="specifying-orcformat"></a><span data-ttu-id="9af51-323">OrcFormat megadása</span><span class="sxs-lookup"><span data-stu-id="9af51-323">Specifying OrcFormat</span></span>
<span data-ttu-id="9af51-324">Ha elemezni szeretné a ORC-fájlokat, vagy ORC formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **OrcFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="9af51-324">If you want to parse the ORC files or write the data in ORC format, set the `format` `type` property to **OrcFormat**.</span></span> <span data-ttu-id="9af51-325">Nem kell meghatároznia semmilyen tulajdonságot a Format szakaszban a typeProperties szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="9af51-325">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="9af51-326">Példa:</span><span class="sxs-lookup"><span data-stu-id="9af51-326">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="9af51-327">Ha nem **adott állapotban** másol ORC-fájlokat a helyszíni és a felhőbeli adattárolók között, telepítenie kell a JRE (Java-futtatókörnyezet) 8-as verzióját az átjáró számítógépre.</span><span class="sxs-lookup"><span data-stu-id="9af51-327">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need to install the JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="9af51-328">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="9af51-328">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="9af51-329">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="9af51-329">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="9af51-330">Válassza ki a megfelelő verziót.</span><span class="sxs-lookup"><span data-stu-id="9af51-330">Choose the appropriate one.</span></span>
>
>

<span data-ttu-id="9af51-331">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="9af51-331">Note the following points:</span></span>

* <span data-ttu-id="9af51-332">Az összetett adattípusok nem támogatottak (STRUCT, MAP, LIST, UNION)</span><span class="sxs-lookup"><span data-stu-id="9af51-332">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="9af51-333">Az ORC-fájlok három, [tömörítéshez kapcsolódó beállítással](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/) rendelkeznek: NONE, ZLIB, SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="9af51-333">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="9af51-334">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="9af51-334">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="9af51-335">Az adatok olvasásához a metaadatokban szereplő tömörítési kodeket használja.</span><span class="sxs-lookup"><span data-stu-id="9af51-335">It uses the compression codec is in the metadata to read the data.</span></span> <span data-ttu-id="9af51-336">Az ORC-fájlokba való írás esetén azonban a Data Factory a ZLIB tömörítést választja, amely az alapértelmezett az ORC-fájlok esetében.</span><span class="sxs-lookup"><span data-stu-id="9af51-336">However, when writing to an ORC file, Data Factory chooses ZLIB, which is the default for ORC.</span></span> <span data-ttu-id="9af51-337">Jelenleg nincs lehetőség ennek a viselkedésnek a felülírására.</span><span class="sxs-lookup"><span data-stu-id="9af51-337">Currently, there is no option to override this behavior.</span></span>

### <a name="specifying-parquetformat"></a><span data-ttu-id="9af51-338">ParquetFormat megadása</span><span class="sxs-lookup"><span data-stu-id="9af51-338">Specifying ParquetFormat</span></span>
<span data-ttu-id="9af51-339">Ha elemezni szeretné a Parquet-fájlokat, vagy Parquet formátumban szeretne adatokat írni, állítsa a `format` `type` tulajdonságot **ParquetFormat** értékre.</span><span class="sxs-lookup"><span data-stu-id="9af51-339">If you want to parse the Parquet files or write the data in Parquet format, set the `format` `type` property to **ParquetFormat**.</span></span> <span data-ttu-id="9af51-340">Nem kell meghatároznia semmilyen tulajdonságot a Format szakaszban a typeProperties szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="9af51-340">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="9af51-341">Példa:</span><span class="sxs-lookup"><span data-stu-id="9af51-341">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="9af51-342">Ha nem **adott állapotban** másol Parquet-fájlokat a helyszíni és a felhőbeli adattárolók között, telepítenie kell a JRE (Java-futtatókörnyezet) 8-as verzióját az átjáró számítógépre.</span><span class="sxs-lookup"><span data-stu-id="9af51-342">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need to install the JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="9af51-343">A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges.</span><span class="sxs-lookup"><span data-stu-id="9af51-343">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="9af51-344">Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="9af51-344">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="9af51-345">Válassza ki a megfelelő verziót.</span><span class="sxs-lookup"><span data-stu-id="9af51-345">Choose the appropriate one.</span></span>
>
>

<span data-ttu-id="9af51-346">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="9af51-346">Note the following points:</span></span>

* <span data-ttu-id="9af51-347">Az összetett adattípusok nem támogatottak (MAP, LIST)</span><span class="sxs-lookup"><span data-stu-id="9af51-347">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="9af51-348">A Parquet-fájlok a következő tömörítéshez kapcsolódó beállításokat használják: NONE, SNAPPY, GZIP és LZO.</span><span class="sxs-lookup"><span data-stu-id="9af51-348">Parquet file has the following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="9af51-349">A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="9af51-349">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="9af51-350">Az adatok olvasásához a metaadatokban szereplő tömörítési kodeket használja.</span><span class="sxs-lookup"><span data-stu-id="9af51-350">It uses the compression codec in the metadata to read the data.</span></span> <span data-ttu-id="9af51-351">A Parquet-fájlokba való írás esetén azonban a Data Factory a SNAPPY tömörítést választja, amely az alapértelmezett a Parquet-fájlok esetében.</span><span class="sxs-lookup"><span data-stu-id="9af51-351">However, when writing to a Parquet file, Data Factory chooses SNAPPY, which is the default for Parquet format.</span></span> <span data-ttu-id="9af51-352">Jelenleg nincs lehetőség ennek a viselkedésnek a felülírására.</span><span class="sxs-lookup"><span data-stu-id="9af51-352">Currently, there is no option to override this behavior.</span></span>
