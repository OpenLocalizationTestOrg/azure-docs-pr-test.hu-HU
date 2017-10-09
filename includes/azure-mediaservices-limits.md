>[!NOTE]
>A nem javított erőforrások kérheti a hello kvóták toobe következik be, a támogatási jegy megnyitásával. Tegye **nem** további Azure Media Services-fiókok létrehozása kísérlet tooobtain magasabb korlátok.

| Erőforrás | Alapértelmezett korlát | 
| --- | --- | 
| Egy előfizetéshez tartozó Azure Media Services- (AMS-) fiókok | 25 (rögzített) |
| Media szolgáltatás számára fenntartott egységek AMS-fiókonként |25 (S1, S2)<br/>10 (S3) <sup>(1)</sup> | 
| Feladatok AMS-fiókonként | 50,000<sup>(2)</sup> |
| Kapcsolt műveletek feladatonként | 30 (rögzített) |
| Objektumok AMS-fiókonként | 1,000,000|
| Objektumok műveletenként | 50 |
| Objektumok feladatonként | 100 |
| Egy objektumhoz egyszerre társított egyedi keresők | 5<sup>(4)</sup> |
| Élő csatornák AMS-fiókonként |5|
| Leállított állapotú programok csatornánként |50|
| Futó állapotú programok csatornánként |3|
| Futó állapotú streamvégpontok AMS-fiókonként|2|
| Streamelési egységek streamvégpontonként |10 |
| Tárfiókok | 1000<sup>5</sup> (rögzített) |
| Házirendek | 1,000,000<sup>(6)</sup> |
| Fájlméret| Bizonyos esetekben nincs maximális hello maximális támogatott fájlméret a Media Services feldolgozásra. <sup>7</sup> |
  
<sup>1</sup> Az S3 fenntartott egységek Nyugat-Indiában nem érhetők el. hello maximális RU korlátok alaphelyzetbe hello ügyfél változásakor hello típusának (például S2 tooS1). 

<sup>2</sup> Ez a szám a várólistás, a befejeződött, az aktív, illetve a megszakított feladatokat is magában foglalja. A törölt feladatokat nem tartalmazza. Hello használó régi feladatok törlése **IJob.Delete** vagy hello **törlése** HTTP-kérelem.

2017. április 1., kezdési bármely feladat rekord 90 napnál régebbi fiókja automatikusan törlődik, a társított feladat bejegyzéseket, valamint akkor is, ha hello rekordok teljes száma nem éri el hello kvóta felső határát. Ha tooarchive hello/feladat tájékoztatásra van szüksége, használhatja a leírt hello kód [Itt](../articles/media-services/media-services-dotnet-manage-entities.md).

<sup>3</sup> amikor kérést toolist feladat entitások, legfeljebb 1000 kérelmenként adja vissza. Ha tookeep nyomon minden elküldött, feladatok, a felső/skip használható [OData rendszer lekérdezési lehetőségek](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> A keresőket nem a felhasználónkénti hozzáférés-vezérlés kezelésére tervezték. toogive eltérő hozzáférési jogok tooindividual felhasználók, a digitális tartalomvédelmi (DRM) megoldásokat. További információ [itt](../articles/media-services/media-services-content-protection-overview.md) érhető el.

<sup>5</sup> hello tárfiókok hello között kell lennie ugyanazon Azure-előfizetésből.

<sup>6</sup> A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. 

>[!NOTE]
> Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek / stb. További információkért és példákért lásd [ezt](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) a szakaszt.

<sup>7</sup>Ha tartalom tooan eszköz az Azure Media Services az hello leképezési tooprocess hello media processzorok szolgáltatás egyikével (pl. kódolók, például Media Encoder Standard és a Media Encoder prémium munkafolyamat vagy az analysis motorok például Arcfelismerési érzékelő) majd kell ügyelnie hello korlátozás a maximális méret hello. 

2017. május 15. frissítésétől hello egyetlen BLOB támogatott maximális méretnél 195 TB - fájl largers ennél a, a feladat sikertelen lesz. Dolgozunk a javítás tooaddress ezt a határt. Ezenkívül hello hello maximális mérete hello eszköz pedig az alábbiak szerint.

| Media szolgáltatás számára fenntartott egység típusa | Maximális bemeneti méret (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
