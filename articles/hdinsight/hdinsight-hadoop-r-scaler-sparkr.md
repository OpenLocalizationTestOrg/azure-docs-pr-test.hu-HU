---
title: "aaaUse ScaleR és az Azure HDInsight SparkR |} Microsoft Docs"
description: "R Server és a HDInsight ScaleR és SparkR használata"
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: da732ff0235cf465a1452b81750c7cdd0351eed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a>ScaleR és SparkR a Hdinsightban

Ez a cikk bemutatja, hogyan toopredict repülési érkezési késések használatával egy **ScaleR** logisztikai regresszió modell repülési késést és időjárási adatokból csatlakoztatni a **SparkR**. A forgatókönyv segít a Spark használt Microsoft R Server analytics adatkezelési hello ScaleR képességeket. Ezek a technológiák hello kombinációja lehetővé teszi tooapply hello legújabb funkciókat az elosztott feldolgozási.

Bár mindkét csomagot Hadoop a Spark végrehajtási motorján futnak, akkor sem, minden egyes van szükségük a saját megfelelő Spark-munkamenetek megosztása a memóriában levő. Ezzel a problémával az R Server egy jövőbeli verziójában, amíg a hello megoldás, toomaintain mozaikként, átfedés nélkül Spark munkamenetek és tooexchange adatok keresztül közbülső fájlokat. hello utasításokat itt mutatja, hogy ezek a követelmények egyszerű tooachieve.

Itt először egy előadás rétegek 2016, az által megosztott Mario Inchiosa és Roni Burd hello webinar keresztül érhetők el, például használjuk [egy méretezhető tudományos Adatplatform r felépítése](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). hello példában SparkR toojoin hello jól ismert légitársaság érkezési késleltetés adatkészlet indulási és érkezési repülőtereken időjárási adatokkal. csatlakoztatott hello adatokat ezután történik bemeneti tooa ScaleR logisztikai regresszió modell repülési érkezési késleltetés előrejelzéséhez.

hello kód azt bemutatása eredetileg készült futó a Spark on Azure HDInsight-fürtök R kiszolgálón. De hello hello használata SparkR és ScaleR egy parancsfájlban keverése fogalma is a helyszíni környezetek hello környezetben érvényes. Hello követően, az azt feltételezik, a közbenső szintű R és R hello ismerete [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) az R Server könyvtárban. Azt is vezethet használatát [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) ebben a forgatókönyvben közben.

## <a name="hello-airline-and-weather-datasets"></a>hello légitársaság és időjárási adatkészletek

Hello **AirOnTime08to12CSV** légitársaság nyilvános dataset repülési érkezési és hello USA, belül minden kereskedelmi repülőútra indító részletei információkat tartalmaz a 2012. októberi 1987 tooDecember. Ez a nagy adatbázisból: nincsenek majdnem 150 millió rekordot összesen. Csomagolva csak a 4 GB. Elérhető a hello [Egyesült államokbeli kormányzati archívumokat](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236). Több kényelmesen érhető el egy zip-fájlba (AirOnTimeCSV.zip) tartalmazó 303 külön havi CSV-fájlok hello [fordulat Analytics dataset tárház](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)

toosee hello hatása időjárási repülési késések, is kell mindegyik hello repülőtéren hello időjárási adatokat. Ezek az adatok letölthetik zip fájlt nyers formátumban, hónap, a hello [nemzeti óceáni és légköri felügyeleti tárház](http://www.ncdc.noaa.gov/orders/qclcd/). A jelen példában hello célokra azt olvasnak be adatokat időjárási, előfordulhat, hogy 2007 – December 2012 és hello óránkénti az adatfájlok minden hello 68 havi zips belül használt. hello havi zip-fájloknak is hello időjárási állomás azonosítója (WBAN) társítva (hívójel) hello repülőtéri közötti leképezéseket (YYYYMMstation.txt) tartalmaz, és hello repülőtéri tartozó időzóna-eltolódás az UTC (időzóna). Ezen információk van szükség, ha való csatlakozás hello légitársaság késleltetés és időjárási adatokat.

## <a name="setting-up-hello-spark-environment"></a>Hello Spark-környezet létrehozása

hello első lépéseként tooset hello Spark-környezet. Az első lépések mutat, amely tartalmazza a bemeneti adatok könyvtárak toohello directory, a Spark számítási környezet létrehozásakor, és tájékoztató naplózási toohello konzol egy naplózási függvény létrehozásához:

```
workDir        <- '~'  
myNameNode     <- 'default' 
myPort         <- 0
inputDataDir   <- 'wasb://hdfs@myAzureAcccount.blob.core.windows.net'
hdfsFS         <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# create a persistent Spark session tooreduce startup times 
#   (remember toostop it later!)
 
sparkCC        <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort, persistentRun=TRUE)

# create working directories 

rxHadoopMakeDir('/user')
rxHadoopMakeDir('user/RevoShare')
rxHadoopMakeDir('user/RevoShare/remoteuser')

(dataDir <- '/share')
rxHadoopMakeDir(dataDir)
rxHadoopListFiles(dataDir) 

setwd(workDir)
getwd()

# version of rxRoc that runs in a local CC 
rxRoc <- function(...){
  rxSetComputeContext(RxLocalSeq())
  roc <- RevoScaleR::rxRoc(...)
  rxSetComputeContext(sparkCC)
  return(roc)
}

logmsg <- function(msg) { cat(format(Sys.time(), "%Y-%m-%d %H:%M:%S"),':',msg,'\n') } 
t0 <- proc.time() 

#..start 

logmsg('Start') 
(trackers <- system("mapred job -list-active-trackers", intern = TRUE))
logmsg(paste('Number of task nodes=',length(trackers)))
```

Ezután azt "Spark_Home" toohello keresési elérési út hozzáadása az R csomagokat, hogy azt SparkR használja, és egy SparkR munkamenet inicializálása:

```
#..setup for use of SparkR  

logmsg('Initialize SparkR') 

.libPaths(c(file.path(Sys.getenv("SPARK_HOME"), "R", "lib"), .libPaths()))
library(SparkR)

sparkEnvir <- list(spark.executor.instances = '10',
                   spark.yarn.executor.memoryOverhead = '8000')

sc <- sparkR.init(
  sparkEnvir = sparkEnvir,
  sparkPackages = "com.databricks:spark-csv_2.10:1.3.0"
)

sqlContext <- sparkRSQL.init(sc)
```

## <a name="preparing-hello-weather-data"></a>Hello időjárási adatok előkészítése

tooprepare hello időjárási adatok azt részhalmaza azt toohello oszlopok modellezési szükséges: 

- "Látható"
- "DryBulbCelsius"
- "DewPointCelsius"
- "RelativeHumidity"
- "Szélsebesség"
- "Magasságmérő"

Majd azt egy hello időjárási állomás társított repülőtéri kódot, és helyi idő tooUTC hello mértékek konvertálása.

Azt először hozzon létre egy fájl toomap hello időjárási állomáson (WBAN) adatai tooan repülőtéri kódot. A korrelációs hello időjárási adatok mellékelt hello leképezési fájlból sikerült azt. Leképezési hello által *hívójel* (például LAX) mezőben hello időjárási adatfájlban túl*származási* hello légitársaság adataiban. Azonban azt csak történt toohave egy másik leképezési aktuális, amely leképezhető *WBAN* túl*AirportID* (például 12892 LAX) és is *időzóna* , amely tooa mentése CSV-fájl neve "wban-az-repülőtéri-azonosító-tz. Fürt megosztott kötetei szolgáltatás", amely is használhatók. Példa:

| AirportID | WBAN | Időzóna
|-----------|------|---------
| 10685 | 54831 | -6
| 14871 | 24232 | -8
| .. | .. | ..

a következő kód olvasási minden hello óránkénti nyers időjárási adatok hello fájlokhoz, részhalmazának toohello oszlopok igazolnia kell, hello időjárási állomás leképezési fájlban egyesíti, hello mérések tooUTC dátum időpontjairól beállítja és hello fájl egy új verzióját, majd írja:

```
# Look up AirportID and Timezone for WBAN (weather station ID) and adjust time

adjustTime <- function(dataList)
{
  dataset0 <- as.data.frame(dataList)
  
  dataset1 <- base::merge(dataset0, wbanToAirIDAndTZDF1, by = "WBAN")

  if(nrow(dataset1) == 0) {
    dataset1 <- data.frame(
      Visibility = numeric(0),
      DryBulbCelsius = numeric(0),
      DewPointCelsius = numeric(0),
      RelativeHumidity = numeric(0),
      WindSpeed = numeric(0),
      Altimeter = numeric(0),
      AdjustedYear = numeric(0),
      AdjustedMonth = numeric(0),
      AdjustedDay = integer(0),
      AdjustedHour = integer(0),
      AirportID = integer(0)
    )
    
    return(dataset1)
  }
  
  Year <- as.integer(substr(dataset1$Date, 1, 4))
  Month <- as.integer(substr(dataset1$Date, 5, 6))
  Day <- as.integer(substr(dataset1$Date, 7, 8))
  
  Time <- dataset1$Time
  Hour <- ceiling(Time/100)
  
  Timezone <- as.integer(dataset1$TimeZone)
  
  adjustdate = as.POSIXlt(sprintf("%4d-%02d-%02d %02d:00:00", Year, Month, Day, Hour), tz = "UTC") + Timezone * 3600

  AdjustedYear = as.POSIXlt(adjustdate)$year + 1900
  AdjustedMonth = as.POSIXlt(adjustdate)$mon + 1
  AdjustedDay   = as.POSIXlt(adjustdate)$mday
  AdjustedHour  = as.POSIXlt(adjustdate)$hour
  
  AirportID = dataset1$AirportID
  Weather = dataset1[,c("Visibility", "DryBulbCelsius", "DewPointCelsius", "RelativeHumidity", "WindSpeed", "Altimeter")]
  
  data.set = data.frame(cbind(AdjustedYear, AdjustedMonth, AdjustedDay, AdjustedHour, AirportID, Weather))
  
  return(data.set)
}

wbanToAirIDAndTZDF <- read.csv("wban-to-airport-id-tz.csv")

colInfo <- list(
  WBAN = list(type="integer"),
  Date = list(type="character"),
  Time = list(type="integer"),
  Visibility = list(type="numeric"),
  DryBulbCelsius = list(type="numeric"),
  DewPointCelsius = list(type="numeric"),
  RelativeHumidity = list(type="numeric"),
  WindSpeed = list(type="numeric"),
  Altimeter = list(type="numeric")
)

weatherDF <- RxTextData(file.path(inputDataDir, "WeatherRaw"), colInfo = colInfo)

weatherDF1 <- RxTextData(file.path(inputDataDir, "Weather"), colInfo = colInfo,
                filesystem=hdfsFS)

rxSetComputeContext("localpar")
rxDataStep(weatherDF, outFile = weatherDF1, rowsPerRead = 50000, overwrite = T,
           transformFunc = adjustTime,
           transformObjects = list(wbanToAirIDAndTZDF1 = wbanToAirIDAndTZDF))
```

## <a name="importing-hello-airline-and-weather-data-toospark-dataframes"></a>Hello légitársaság és időjárási adatok tooSpark DataFrames importálása

Most hello SparkR használjuk [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) tooimport hello időjárása és a légitársaság adatok tooSpark DataFrames működik. Ez a funkció számos más Spark módszerek, például végrehajtása lazily, ami azt jelenti, hogy a végrehajtási sorba állított, de nem hajtotta végre a kötelező.

```
airPath     <- file.path(inputDataDir, "AirOnTime08to12CSV")
weatherPath <- file.path(inputDataDir, "Weather") # pre-processed weather data
rxHadoopListFiles(airPath) 
rxHadoopListFiles(weatherPath) 

# create a SparkR DataFrame for hello airline data

logmsg('create a SparkR DataFrame for hello airline data') 
# use inferSchema = "false" for more robust parsing
airDF <- read.df(sqlContext, airPath, source = "com.databricks.spark.csv", 
                 header = "true", inferSchema = "false")

# Create a SparkR DataFrame for hello weather data

logmsg('create a SparkR DataFrame for hello weather data') 
weatherDF <- read.df(sqlContext, weatherPath, source = "com.databricks.spark.csv", 
                     header = "true", inferSchema = "true")
```

## <a name="data-cleansing-and-transformation"></a>Adatok tisztítására és átalakítás

Ezután néhány karbantartási műveleteket végezzük hello légitársaság adatokon toorename oszlopok importált azt. Jelenleg csak megőrzése hello változók szükséges, és le toohello óra tooenable hello legfrissebb adatokkal időjárási induláskor egyesítése a legközelebbi ütemezett indító alkalommal kerekíteni:

```
logmsg('clean hello airline data') 
airDF <- rename(airDF,
                ArrDel15 = airDF$ARR_DEL15,
                Year = airDF$YEAR,
                Month = airDF$MONTH,
                DayofMonth = airDF$DAY_OF_MONTH,
                DayOfWeek = airDF$DAY_OF_WEEK,
                Carrier = airDF$UNIQUE_CARRIER,
                OriginAirportID = airDF$ORIGIN_AIRPORT_ID,
                DestAirportID = airDF$DEST_AIRPORT_ID,
                CRSDepTime = airDF$CRS_DEP_TIME,
                CRSArrTime =  airDF$CRS_ARR_TIME
)

# Select desired columns from hello flight data. 
varsToKeep <- c("ArrDel15", "Year", "Month", "DayofMonth", "DayOfWeek", "Carrier", "OriginAirportID", "DestAirportID", "CRSDepTime", "CRSArrTime")
airDF <- select(airDF, varsToKeep)

# Apply schema
coltypes(airDF) <- c("character", "integer", "integer", "integer", "integer", "character", "integer", "integer", "integer", "integer")

# Round down scheduled departure time toofull hour.
airDF$CRSDepTime <- floor(airDF$CRSDepTime / 100)
```

Most azt műveleteket hasonló hello időjárási adatokat:

```
# Average weather readings by hour
logmsg('clean hello weather data') 
weatherDF <- agg(groupBy(weatherDF, "AdjustedYear", "AdjustedMonth", "AdjustedDay", "AdjustedHour", "AirportID"), Visibility="avg",
                  DryBulbCelsius="avg", DewPointCelsius="avg", RelativeHumidity="avg", WindSpeed="avg", Altimeter="avg"
                  )

weatherDF <- rename(weatherDF,
                    Visibility = weatherDF$'avg(Visibility)',
                    DryBulbCelsius = weatherDF$'avg(DryBulbCelsius)',
                    DewPointCelsius = weatherDF$'avg(DewPointCelsius)',
                    RelativeHumidity = weatherDF$'avg(RelativeHumidity)',
                    WindSpeed = weatherDF$'avg(WindSpeed)',
                    Altimeter = weatherDF$'avg(Altimeter)'
)
```

## <a name="joining-hello-weather-and-airline-data"></a>Hello időjárása és a légitársaság adatok csatlakoztatása

Hello SparkR használja [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) toodo a bal oldali külső illesztések indító AirportID hello légitársaság és időjárási adatait és a dátum és idő működik. hello külső illesztés lehetővé teszi minden hello légitársaság adatokat rögzíti, még akkor is, ha nincsenek egyező időjárási adatok tooretain. Következő hello illesztési azt néhány felesleges oszlopok eltávolítása, és nevezze át a tartani hello oszlopok tooremove hello bejövő DataFrame előtag hello illesztési által bevezetett.

```
logmsg('Join airline data with weather at Origin Airport')
joinedDF <- SparkR::join(
  airDF,
  weatherDF,
  airDF$OriginAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF1 <- select(joinedDF, varsToKeep)

joinedDF2 <- rename(joinedDF1,
                    VisibilityOrigin = joinedDF1$Visibility,
                    DryBulbCelsiusOrigin = joinedDF1$DryBulbCelsius,
                    DewPointCelsiusOrigin = joinedDF1$DewPointCelsius,
                    RelativeHumidityOrigin = joinedDF1$RelativeHumidity,
                    WindSpeedOrigin = joinedDF1$WindSpeed,
                    AltimeterOrigin = joinedDF1$Altimeter
)
```

Hasonló módon csatlakoztassa azt hello időjárása és a légitársaság adatok érkezési AirportID és a dátum és idő alapján:

```
logmsg('Join airline data with weather at Destination Airport')
joinedDF3 <- SparkR::join(
  joinedDF2,
  weatherDF,
  airDF$DestAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF3)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF4 <- select(joinedDF3, varsToKeep)

joinedDF5 <- rename(joinedDF4,
                    VisibilityDest = joinedDF4$Visibility,
                    DryBulbCelsiusDest = joinedDF4$DryBulbCelsius,
                    DewPointCelsiusDest = joinedDF4$DewPointCelsius,
                    RelativeHumidityDest = joinedDF4$RelativeHumidity,
                    WindSpeedDest = joinedDF4$WindSpeed,
                    AltimeterDest = joinedDF4$Altimeter
                    )
```

## <a name="save-results-toocsv-for-exchange-with-scaler"></a>Mentse ScaleR eredmények tooCSV exchange-hez

Ezzel befejezte igazolnia kell a SparkR toodo hello illesztéseket. A Microsoft hello végső Spark DataFrame "joinedDF5" tooa CSV a bemeneti tooScaleR hello adatainak mentése, és zárja be a kimenő hello SparkR munkamenet. Explicit módon biztosítunk SparkR toosave eredő CSV hello a 80 külön partíciók tooenable elegendő párhuzamos ScaleR feldolgozási:

```
logmsg('output hello joined data from Spark tooCSV') 
joinedDF5 <- repartition(joinedDF5, 80) # write.df below will produce this many CSVs

# write result toodirectory of CSVs
write.df(joinedDF5, file.path(dataDir, "joined5Csv"), "com.databricks.spark.csv", "overwrite", header = "true")

# We can shut down hello SparkR Spark context now
sparkR.stop()

# remove non-data files
rxHadoopRemove(file.path(dataDir, "joined5Csv/_SUCCESS"))
```

## <a name="import-tooxdf-for-use-by-scaler"></a>Importálás tooXDF ScaleR általi használatra

CSV-fájl hello illesztett légitársaság és időjárási adataikat sikerült használjuk-a modellezési ScaleR szöveg adatforrás keresztül. De azt importálnia tooXDF először, mert hatékonyabb hello adatkészlethez több művelet futtatásakor:

```
logmsg('Import hello CSV toocompressed, binary XDF format') 

# set hello Spark compute context for R Server 
rxSetComputeContext(sparkCC)
rxGetComputeContext()

colInfo <- list(
  ArrDel15 = list(type="numeric"),
  Year = list(type="factor"),
  Month = list(type="factor"),
  DayofMonth = list(type="factor"),
  DayOfWeek = list(type="factor"),
  Carrier = list(type="factor"),
  OriginAirportID = list(type="factor"),
  DestAirportID = list(type="factor"),
  RelativeHumidityOrigin = list(type="numeric"),
  AltimeterOrigin = list(type="numeric"),
  DryBulbCelsiusOrigin = list(type="numeric"),
  WindSpeedOrigin = list(type="numeric"),
  VisibilityOrigin = list(type="numeric"),
  DewPointCelsiusOrigin = list(type="numeric"),
  RelativeHumidityDest = list(type="numeric"),
  AltimeterDest = list(type="numeric"),
  DryBulbCelsiusDest = list(type="numeric"),
  WindSpeedDest = list(type="numeric"),
  VisibilityDest = list(type="numeric"),
  DewPointCelsiusDest = list(type="numeric")
)

joinedDF5Txt <- RxTextData(file.path(dataDir, "joined5Csv"),
                           colInfo = colInfo, fileSystem = hdfsFS)
rxGetInfo(joinedDF5Txt) 

destData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

rxImport(inData = joinedDF5Txt, destData, overwrite = TRUE)

rxGetInfo(destData, getVarInfo = T)

# File name: /user/RevoShare/dev/delayDataLarge/joined5XDF 
# Number of composite data files: 80 
# Number of observations: 148619655 
# Number of variables: 22 
# Number of blocks: 320 
# Compression type: zlib 
# Variable information: 
#   Var 1: ArrDel15, Type: numeric, Low/High: (0.0000, 1.0000)
# Var 2: Year
# 26 factor levels: 1987 1988 1989 1990 1991 ... 2008 2009 2010 2011 2012
# Var 3: Month
# 12 factor levels: 10 11 12 1 2 ... 5 6 7 8 9
# Var 4: DayofMonth
# 31 factor levels: 1 3 4 5 7 ... 29 30 2 18 31
# Var 5: DayOfWeek
# 7 factor levels: 4 6 7 1 3 2 5
# Var 6: Carrier
# 30 factor levels: PI UA US AA DL ... HA F9 YV 9E VX
# Var 7: OriginAirportID
# 374 factor levels: 15249 12264 11042 15412 13930 ... 13341 10559 14314 11711 10558
# Var 8: DestAirportID
# 378 factor levels: 13303 14492 10721 11057 13198 ... 14802 11711 11931 12899 10559
# Var 9: CRSDepTime, Type: integer, Low/High: (0, 24)
# Var 10: CRSArrTime, Type: integer, Low/High: (0, 2400)
# Var 11: RelativeHumidityOrigin, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 12: AltimeterOrigin, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 13: DryBulbCelsiusOrigin, Type: numeric, Low/High: (-46.1000, 47.8000)
# Var 14: WindSpeedOrigin, Type: numeric, Low/High: (0.0000, 81.0000)
# Var 15: VisibilityOrigin, Type: numeric, Low/High: (0.0000, 90.0000)
# Var 16: DewPointCelsiusOrigin, Type: numeric, Low/High: (-41.7000, 29.0000)
# Var 17: RelativeHumidityDest, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 18: AltimeterDest, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 19: DryBulbCelsiusDest, Type: numeric, Low/High: (-46.1000, 53.9000)
# Var 20: WindSpeedDest, Type: numeric, Low/High: (0.0000, 136.0000)
# Var 21: VisibilityDest, Type: numeric, Low/High: (0.0000, 88.0000)
# Var 22: DewPointCelsiusDest, Type: numeric, Low/High: (-43.0000, 29.0000)

finalData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

```

## <a name="splitting-data-for-training-and-test"></a>Felosztása tanítási és tesztelési adatokat

Azt a rxDataStep toosplit hello 2012 adatai a teszteléshez használni, és tartsa hello rest képzési:

```
# split out hello training data

logmsg('split out training data as all data except year 2012')
trainDS <- RxXdfData( file.path(dataDir, "finalDataTrain" ),fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = trainDS,
            rowSelection = ( Year != 2012 ), overwrite = T )

# split out hello testing data

logmsg('split out hello test data for year 2012') 
testDS <- RxXdfData( file.path(dataDir, "finalDataTest" ), fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = testDS,
            rowSelection = ( Year == 2012 ), overwrite = T )

rxGetInfo(trainDS)
rxGetInfo(testDS)
```

## <a name="train-and-test-a-logistic-regression-model"></a>Betanítása és logisztikai regresszió modell teszteléséhez

Most már készen áll a toobuild modell folyamatban. toosee hello hatással időjárási adatok késleltetése hello érkezésének ideje, az ScaleR tartozó logisztikai regresszió rutin használjuk. Használjuk, toomodel e egy érkezési késés nagyobb, mint 15 percig hello időjárási hello indulási és érkezési repülőtereken befolyásolja:

```
logmsg('train a logistic regression model for Arrival Delay > 15 minutes') 
formula <- as.formula(ArrDel15 ~ Year + Month + DayofMonth + DayOfWeek + Carrier +
                     OriginAirportID + DestAirportID + CRSDepTime + CRSArrTime + 
                     RelativeHumidityOrigin + AltimeterOrigin + DryBulbCelsiusOrigin +
                     WindSpeedOrigin + VisibilityOrigin + DewPointCelsiusOrigin + 
                     RelativeHumidityDest + AltimeterDest + DryBulbCelsiusDest +
                     WindSpeedDest + VisibilityDest + DewPointCelsiusDest
                   )

# Use hello scalable rxLogit() function but set max iterations too3 for hello purposes of 
# this exercise 

logitModel <- rxLogit(formula, data = trainDS, maxIterations = 3)

base::summary(logitModel)
```

Most nézzük meg, hogyan azt a hello Tesztadatok néhány előrejelzéseket és ROC és AUC megnézi.

```
# Predict over test data (Logistic Regression).

logmsg('predict over hello test data') 
logitPredict <- RxXdfData(file.path(dataDir, "logitPredict"), fileSystem = hdfsFS)

# Use hello scalable rxPredict() function

rxPredict(logitModel, data = testDS, outData = logitPredict,
          extraVarsToWrite = c("ArrDel15"), 
          type = 'response', overwrite = TRUE)

# Calculate ROC and Area Under hello Curve (AUC).

logmsg('calculate hello roc and auc') 
logitRoc <- rxRoc("ArrDel15", "ArrDel15_Pred", logitPredict)
logitAuc <- rxAuc(logitRoc)
head(logitAuc)
logitAuc

plot(logitRoc)
```

## <a name="scoring-elsewhere"></a>A pontozási máshol

Azt is használható hello modell pontozási adatok más platformon. Mentés tooan távoli asztali szolgáltatások fájl és átvitele, és a távoli asztali szolgáltatások importál egy cél, például az SQL Server R Services környezet pontozási. Fontos fontos tooensure, amelyek hello tényező szintű hello adatok toobe program pontozza a mennyiségeket megegyeznek a mely hello modell lett létrehozva. Hogy egyeznek megvalósítható beolvasásával, és hello modellezési keresztül ScaleR tartozó adatok mentése hello oszlopra vonatkozó információ társított `rxCreateColInfo()` függvény és majd alkalmazása az adott oszlop toohello bemeneti adatok forrása előrejelzés. Hello alábbi azt hello tesztelési adatkészletnél néhány sornyi mentése, bontsa ki és hello oszlopra vonatkozó információ ettől a példától hello előrejelzés parancsfájl használata:

```
# save hello model and a sample of hello test dataset 

logmsg('save serialized version of hello model and a sample of hello test data')
rxSetComputeContext('localpar') 
saveRDS(logitModel, file = "logitModel.rds")
testDF <- head(testDS, 1000)  
saveRDS(testDF    , file = "testDF.rds"    )
list.files()

rxHadoopListFiles(file.path(inputDataDir,''))
rxHadoopListFiles(dataDir)

# stop hello spark engine 
rxStopEngine(sparkCC) 

logmsg('Done.')
elapsed <- (proc.time() - t0)[3]
logmsg(paste('Elapsed time=',sprintf('%6.2f',elapsed),'(sec)\n\n'))
```

## <a name="summary"></a>Összefoglalás

Ebben a cikkben azt korábban jelenik meg, hogyan azt a Hadoop Spark modell fejlesztési ScaleR adatok módosítását a lehetséges toocombine SparkR használható. Ebben a forgatókönyvben csak fut egyszerre csak egy munkamenet külön Spark munkamenetek, karbantartása és exchange-adatok CSV-fájlok keresztül igényel. Egyszerű, bár ez a folyamat lehet egy jövőbeli R Server a kiadásban még egyszerűbbé SparkR és ScaleR is megosztani egy Spark-munkamenetet, és így megosztani a Spark DataFrames.

## <a name="next-steps-and-more-information"></a>Következő lépések és további információ

- R Server, a Spark használatára vonatkozó további információkért lásd: hello [kezdeti lépések útmutatóban az MSDN webhelyen](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)

- R Server általános információkért lásd: hello [Ismerkedés az R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) cikk.

- Az R Server on HDInsight információkért lásd: [az R Server on Azure HDInsight áttekintése](hdinsight-hadoop-r-server-overview.md) és [az R Server on Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).

SparkR használatára vonatkozó további információkért lásd:

- [Apache SparkR dokumentum](https://spark.apache.org/docs/2.1.0/sparkr.html)

- [SparkR áttekintése](https://docs.databricks.com/spark/latest/sparkr/overview.html) a Databricks
