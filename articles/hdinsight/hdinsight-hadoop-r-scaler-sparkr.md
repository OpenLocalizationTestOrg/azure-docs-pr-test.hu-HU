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
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a><span data-ttu-id="30de4-103">ScaleR és SparkR a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="30de4-103">Combine ScaleR and SparkR in HDInsight</span></span>

<span data-ttu-id="30de4-104">Ez a cikk bemutatja, hogyan toopredict repülési érkezési késések használatával egy **ScaleR** logisztikai regresszió modell repülési késést és időjárási adatokból csatlakoztatni a **SparkR**.</span><span class="sxs-lookup"><span data-stu-id="30de4-104">This article shows how toopredict flight arrival delays using a **ScaleR** logistic regression model from data on flight delays and weather joined with **SparkR**.</span></span> <span data-ttu-id="30de4-105">A forgatókönyv segít a Spark használt Microsoft R Server analytics adatkezelési hello ScaleR képességeket.</span><span class="sxs-lookup"><span data-stu-id="30de4-105">This scenario demonstrates hello capabilities of ScaleR for data manipulation on Spark used with Microsoft R Server for analytics.</span></span> <span data-ttu-id="30de4-106">Ezek a technológiák hello kombinációja lehetővé teszi tooapply hello legújabb funkciókat az elosztott feldolgozási.</span><span class="sxs-lookup"><span data-stu-id="30de4-106">hello combination of these technologies enables you tooapply hello latest capabilities in distributed processing.</span></span>

<span data-ttu-id="30de4-107">Bár mindkét csomagot Hadoop a Spark végrehajtási motorján futnak, akkor sem, minden egyes van szükségük a saját megfelelő Spark-munkamenetek megosztása a memóriában levő.</span><span class="sxs-lookup"><span data-stu-id="30de4-107">Although both packages run on Hadoop’s Spark execution engine, they are blocked from in-memory data sharing as they each require their own respective Spark sessions.</span></span> <span data-ttu-id="30de4-108">Ezzel a problémával az R Server egy jövőbeli verziójában, amíg a hello megoldás, toomaintain mozaikként, átfedés nélkül Spark munkamenetek és tooexchange adatok keresztül közbülső fájlokat.</span><span class="sxs-lookup"><span data-stu-id="30de4-108">Until this issue is addressed in an upcoming version of R Server, hello workaround is toomaintain non-overlapping Spark sessions, and tooexchange data through intermediate files.</span></span> <span data-ttu-id="30de4-109">hello utasításokat itt mutatja, hogy ezek a követelmények egyszerű tooachieve.</span><span class="sxs-lookup"><span data-stu-id="30de4-109">hello instructions here show that these requirements are straightforward tooachieve.</span></span>

<span data-ttu-id="30de4-110">Itt először egy előadás rétegek 2016, az által megosztott Mario Inchiosa és Roni Burd hello webinar keresztül érhetők el, például használjuk [egy méretezhető tudományos Adatplatform r felépítése](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). hello példában SparkR toojoin hello jól ismert légitársaság érkezési késleltetés adatkészlet indulási és érkezési repülőtereken időjárási adatokkal.</span><span class="sxs-lookup"><span data-stu-id="30de4-110">We use an example here initially shared in a talk at Strata 2016 by Mario Inchiosa and Roni Burd that is also available through hello webinar [Building a Scalable Data Science Platform with R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). hello example uses SparkR toojoin hello well-known airlines arrival delay data set with weather data at departure and arrival airports.</span></span> <span data-ttu-id="30de4-111">csatlakoztatott hello adatokat ezután történik bemeneti tooa ScaleR logisztikai regresszió modell repülési érkezési késleltetés előrejelzéséhez.</span><span class="sxs-lookup"><span data-stu-id="30de4-111">hello data joined is then used as input tooa ScaleR logistic regression model for predicting flight arrival delay.</span></span>

<span data-ttu-id="30de4-112">hello kód azt bemutatása eredetileg készült futó a Spark on Azure HDInsight-fürtök R kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="30de4-112">hello code we walkthrough was originally written for R Server running on Spark in an HDInsight cluster on Azure.</span></span> <span data-ttu-id="30de4-113">De hello hello használata SparkR és ScaleR egy parancsfájlban keverése fogalma is a helyszíni környezetek hello környezetben érvényes.</span><span class="sxs-lookup"><span data-stu-id="30de4-113">But hello concept of mixing hello use of SparkR and ScaleR in one script is also valid in hello context of on-premises environments.</span></span> <span data-ttu-id="30de4-114">Hello követően, az azt feltételezik, a közbenső szintű R és R hello ismerete [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) az R Server könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="30de4-114">In hello following, we presume an intermediate level of knowledge of R and R hello [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) library of R Server.</span></span> <span data-ttu-id="30de4-115">Azt is vezethet használatát [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) ebben a forgatókönyvben közben.</span><span class="sxs-lookup"><span data-stu-id="30de4-115">We also introduce use of [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) while walking through this scenario.</span></span>

## <a name="hello-airline-and-weather-datasets"></a><span data-ttu-id="30de4-116">hello légitársaság és időjárási adatkészletek</span><span class="sxs-lookup"><span data-stu-id="30de4-116">hello airline and weather datasets</span></span>

<span data-ttu-id="30de4-117">Hello **AirOnTime08to12CSV** légitársaság nyilvános dataset repülési érkezési és hello USA, belül minden kereskedelmi repülőútra indító részletei információkat tartalmaz a 2012. októberi 1987 tooDecember.</span><span class="sxs-lookup"><span data-stu-id="30de4-117">hello **AirOnTime08to12CSV** airlines public dataset contains information on flight arrival and departure details for all commercial flights within hello USA, from October 1987 tooDecember 2012.</span></span> <span data-ttu-id="30de4-118">Ez a nagy adatbázisból: nincsenek majdnem 150 millió rekordot összesen.</span><span class="sxs-lookup"><span data-stu-id="30de4-118">This is a large dataset: there are nearly 150 million records in total.</span></span> <span data-ttu-id="30de4-119">Csomagolva csak a 4 GB.</span><span class="sxs-lookup"><span data-stu-id="30de4-119">It is just under 4 GB unpacked.</span></span> <span data-ttu-id="30de4-120">Elérhető a hello [Egyesült államokbeli kormányzati archívumokat](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span><span class="sxs-lookup"><span data-stu-id="30de4-120">It is available from hello [U.S. government archives](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span></span> <span data-ttu-id="30de4-121">Több kényelmesen érhető el egy zip-fájlba (AirOnTimeCSV.zip) tartalmazó 303 külön havi CSV-fájlok hello [fordulat Analytics dataset tárház](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span><span class="sxs-lookup"><span data-stu-id="30de4-121">More conveniently, it is available as a zip file (AirOnTimeCSV.zip) containing a set of 303 separate monthly CSV files from hello [Revolution Analytics dataset repository](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span></span>

<span data-ttu-id="30de4-122">toosee hello hatása időjárási repülési késések, is kell mindegyik hello repülőtéren hello időjárási adatokat.</span><span class="sxs-lookup"><span data-stu-id="30de4-122">toosee hello effects of weather on flight delays, we also need hello weather data at each of hello airports.</span></span> <span data-ttu-id="30de4-123">Ezek az adatok letölthetik zip fájlt nyers formátumban, hónap, a hello [nemzeti óceáni és légköri felügyeleti tárház](http://www.ncdc.noaa.gov/orders/qclcd/).</span><span class="sxs-lookup"><span data-stu-id="30de4-123">This data can be downloaded as zip files in raw form, by month, from hello [National Oceanic and Atmospheric Administration repository](http://www.ncdc.noaa.gov/orders/qclcd/).</span></span> <span data-ttu-id="30de4-124">A jelen példában hello célokra azt olvasnak be adatokat időjárási, előfordulhat, hogy 2007 – December 2012 és hello óránkénti az adatfájlok minden hello 68 havi zips belül használt.</span><span class="sxs-lookup"><span data-stu-id="30de4-124">For hello purposes of this example, we pull weather data from May 2007 – December 2012 and used hello hourly data files within each of hello 68 monthly zips.</span></span> <span data-ttu-id="30de4-125">hello havi zip-fájloknak is hello időjárási állomás azonosítója (WBAN) társítva (hívójel) hello repülőtéri közötti leképezéseket (YYYYMMstation.txt) tartalmaz, és hello repülőtéri tartozó időzóna-eltolódás az UTC (időzóna).</span><span class="sxs-lookup"><span data-stu-id="30de4-125">hello monthly zip files also contain a mapping (YYYYMMstation.txt) between hello weather station ID (WBAN), hello airport that it is associated with (CallSign), and hello airport’s time zone offset from UTC (TimeZone).</span></span> <span data-ttu-id="30de4-126">Ezen információk van szükség, ha való csatlakozás hello légitársaság késleltetés és időjárási adatokat.</span><span class="sxs-lookup"><span data-stu-id="30de4-126">All of this information is needed when joining with hello airline delay and weather data.</span></span>

## <a name="setting-up-hello-spark-environment"></a><span data-ttu-id="30de4-127">Hello Spark-környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="30de4-127">Setting up hello Spark environment</span></span>

<span data-ttu-id="30de4-128">hello első lépéseként tooset hello Spark-környezet.</span><span class="sxs-lookup"><span data-stu-id="30de4-128">hello first step is tooset up hello Spark environment.</span></span> <span data-ttu-id="30de4-129">Az első lépések mutat, amely tartalmazza a bemeneti adatok könyvtárak toohello directory, a Spark számítási környezet létrehozásakor, és tájékoztató naplózási toohello konzol egy naplózási függvény létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="30de4-129">We begin by pointing toohello directory that contains our input data directories, creating a Spark compute context, and creating a logging function for informational logging toohello console:</span></span>

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

<span data-ttu-id="30de4-130">Ezután azt "Spark_Home" toohello keresési elérési út hozzáadása az R csomagokat, hogy azt SparkR használja, és egy SparkR munkamenet inicializálása:</span><span class="sxs-lookup"><span data-stu-id="30de4-130">Next we add “Spark_Home” toohello search path for R packages so that we can use SparkR, and initialize a SparkR session:</span></span>

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

## <a name="preparing-hello-weather-data"></a><span data-ttu-id="30de4-131">Hello időjárási adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="30de4-131">Preparing hello weather data</span></span>

<span data-ttu-id="30de4-132">tooprepare hello időjárási adatok azt részhalmaza azt toohello oszlopok modellezési szükséges:</span><span class="sxs-lookup"><span data-stu-id="30de4-132">tooprepare hello weather data, we subset it toohello columns needed for modeling:</span></span> 

- <span data-ttu-id="30de4-133">"Látható"</span><span class="sxs-lookup"><span data-stu-id="30de4-133">"Visibility"</span></span>
- <span data-ttu-id="30de4-134">"DryBulbCelsius"</span><span class="sxs-lookup"><span data-stu-id="30de4-134">"DryBulbCelsius"</span></span>
- <span data-ttu-id="30de4-135">"DewPointCelsius"</span><span class="sxs-lookup"><span data-stu-id="30de4-135">"DewPointCelsius"</span></span>
- <span data-ttu-id="30de4-136">"RelativeHumidity"</span><span class="sxs-lookup"><span data-stu-id="30de4-136">"RelativeHumidity"</span></span>
- <span data-ttu-id="30de4-137">"Szélsebesség"</span><span class="sxs-lookup"><span data-stu-id="30de4-137">"WindSpeed"</span></span>
- <span data-ttu-id="30de4-138">"Magasságmérő"</span><span class="sxs-lookup"><span data-stu-id="30de4-138">"Altimeter"</span></span>

<span data-ttu-id="30de4-139">Majd azt egy hello időjárási állomás társított repülőtéri kódot, és helyi idő tooUTC hello mértékek konvertálása.</span><span class="sxs-lookup"><span data-stu-id="30de4-139">Then we add an airport code associated with hello weather station and convert hello measurements from local time tooUTC.</span></span>

<span data-ttu-id="30de4-140">Azt először hozzon létre egy fájl toomap hello időjárási állomáson (WBAN) adatai tooan repülőtéri kódot.</span><span class="sxs-lookup"><span data-stu-id="30de4-140">We begin by creating a file toomap hello weather station (WBAN) info tooan airport code.</span></span> <span data-ttu-id="30de4-141">A korrelációs hello időjárási adatok mellékelt hello leképezési fájlból sikerült azt.</span><span class="sxs-lookup"><span data-stu-id="30de4-141">We could get this correlation from hello mapping file included with hello weather data.</span></span> <span data-ttu-id="30de4-142">Leképezési hello által *hívójel* (például LAX) mezőben hello időjárási adatfájlban túl*származási* hello légitársaság adataiban.</span><span class="sxs-lookup"><span data-stu-id="30de4-142">By mapping hello *CallSign* (for example, LAX) field in hello weather data file too*Origin* in hello airline data.</span></span> <span data-ttu-id="30de4-143">Azonban azt csak történt toohave egy másik leképezési aktuális, amely leképezhető *WBAN* túl*AirportID* (például 12892 LAX) és is *időzóna* , amely tooa mentése CSV-fájl neve "wban-az-repülőtéri-azonosító-tz. Fürt megosztott kötetei szolgáltatás", amely is használhatók.</span><span class="sxs-lookup"><span data-stu-id="30de4-143">However, we just happened toohave another mapping on hand that maps *WBAN* too*AirportID* (for example, 12892 for LAX) and includes *TimeZone* that has been saved tooa CSV file called “wban-to-airport-id-tz.CSV” that we can use.</span></span> <span data-ttu-id="30de4-144">Példa:</span><span class="sxs-lookup"><span data-stu-id="30de4-144">For example:</span></span>

| <span data-ttu-id="30de4-145">AirportID</span><span class="sxs-lookup"><span data-stu-id="30de4-145">AirportID</span></span> | <span data-ttu-id="30de4-146">WBAN</span><span class="sxs-lookup"><span data-stu-id="30de4-146">WBAN</span></span> | <span data-ttu-id="30de4-147">Időzóna</span><span class="sxs-lookup"><span data-stu-id="30de4-147">TimeZone</span></span>
|-----------|------|---------
| <span data-ttu-id="30de4-148">10685</span><span class="sxs-lookup"><span data-stu-id="30de4-148">10685</span></span> | <span data-ttu-id="30de4-149">54831</span><span class="sxs-lookup"><span data-stu-id="30de4-149">54831</span></span> | <span data-ttu-id="30de4-150">-6</span><span class="sxs-lookup"><span data-stu-id="30de4-150">-6</span></span>
| <span data-ttu-id="30de4-151">14871</span><span class="sxs-lookup"><span data-stu-id="30de4-151">14871</span></span> | <span data-ttu-id="30de4-152">24232</span><span class="sxs-lookup"><span data-stu-id="30de4-152">24232</span></span> | <span data-ttu-id="30de4-153">-8</span><span class="sxs-lookup"><span data-stu-id="30de4-153">-8</span></span>
| <span data-ttu-id="30de4-154">..</span><span class="sxs-lookup"><span data-stu-id="30de4-154">..</span></span> | <span data-ttu-id="30de4-155">..</span><span class="sxs-lookup"><span data-stu-id="30de4-155">..</span></span> | <span data-ttu-id="30de4-156">..</span><span class="sxs-lookup"><span data-stu-id="30de4-156">..</span></span>

<span data-ttu-id="30de4-157">a következő kód olvasási minden hello óránkénti nyers időjárási adatok hello fájlokhoz, részhalmazának toohello oszlopok igazolnia kell, hello időjárási állomás leképezési fájlban egyesíti, hello mérések tooUTC dátum időpontjairól beállítja és hello fájl egy új verzióját, majd írja:</span><span class="sxs-lookup"><span data-stu-id="30de4-157">hello following code reads each of hello hourly raw weather data files, subsets toohello columns we need, merges hello weather station mapping file, adjusts hello date times of measurements tooUTC, and then writes out a new version of hello file:</span></span>

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

## <a name="importing-hello-airline-and-weather-data-toospark-dataframes"></a><span data-ttu-id="30de4-158">Hello légitársaság és időjárási adatok tooSpark DataFrames importálása</span><span class="sxs-lookup"><span data-stu-id="30de4-158">Importing hello airline and weather data tooSpark DataFrames</span></span>

<span data-ttu-id="30de4-159">Most hello SparkR használjuk [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) tooimport hello időjárása és a légitársaság adatok tooSpark DataFrames működik.</span><span class="sxs-lookup"><span data-stu-id="30de4-159">Now we use hello SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) function tooimport hello weather and airline data tooSpark DataFrames.</span></span> <span data-ttu-id="30de4-160">Ez a funkció számos más Spark módszerek, például végrehajtása lazily, ami azt jelenti, hogy a végrehajtási sorba állított, de nem hajtotta végre a kötelező.</span><span class="sxs-lookup"><span data-stu-id="30de4-160">This function, like many other Spark methods, are executed lazily, meaning that they are queued for execution but not executed until required.</span></span>

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

## <a name="data-cleansing-and-transformation"></a><span data-ttu-id="30de4-161">Adatok tisztítására és átalakítás</span><span class="sxs-lookup"><span data-stu-id="30de4-161">Data cleansing and transformation</span></span>

<span data-ttu-id="30de4-162">Ezután néhány karbantartási műveleteket végezzük hello légitársaság adatokon toorename oszlopok importált azt.</span><span class="sxs-lookup"><span data-stu-id="30de4-162">Next we do some cleanup on hello airline data we’ve imported toorename columns.</span></span> <span data-ttu-id="30de4-163">Jelenleg csak megőrzése hello változók szükséges, és le toohello óra tooenable hello legfrissebb adatokkal időjárási induláskor egyesítése a legközelebbi ütemezett indító alkalommal kerekíteni:</span><span class="sxs-lookup"><span data-stu-id="30de4-163">We only keep hello variables needed, and round scheduled departure times down toohello nearest hour tooenable merging with hello latest weather data at departure:</span></span>

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

<span data-ttu-id="30de4-164">Most azt műveleteket hasonló hello időjárási adatokat:</span><span class="sxs-lookup"><span data-stu-id="30de4-164">Now we perform similar operations on hello weather data:</span></span>

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

## <a name="joining-hello-weather-and-airline-data"></a><span data-ttu-id="30de4-165">Hello időjárása és a légitársaság adatok csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="30de4-165">Joining hello weather and airline data</span></span>

<span data-ttu-id="30de4-166">Hello SparkR használja [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) toodo a bal oldali külső illesztések indító AirportID hello légitársaság és időjárási adatait és a dátum és idő működik.</span><span class="sxs-lookup"><span data-stu-id="30de4-166">We now use hello SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) function toodo a left outer join of hello airline and weather data by departure AirportID and datetime.</span></span> <span data-ttu-id="30de4-167">hello külső illesztés lehetővé teszi minden hello légitársaság adatokat rögzíti, még akkor is, ha nincsenek egyező időjárási adatok tooretain.</span><span class="sxs-lookup"><span data-stu-id="30de4-167">hello outer join allows us tooretain all hello airline data records even if there is no matching weather data.</span></span> <span data-ttu-id="30de4-168">Következő hello illesztési azt néhány felesleges oszlopok eltávolítása, és nevezze át a tartani hello oszlopok tooremove hello bejövő DataFrame előtag hello illesztési által bevezetett.</span><span class="sxs-lookup"><span data-stu-id="30de4-168">Following hello join, we remove some redundant columns, and rename hello kept columns tooremove hello incoming DataFrame prefix introduced by hello join.</span></span>

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

<span data-ttu-id="30de4-169">Hasonló módon csatlakoztassa azt hello időjárása és a légitársaság adatok érkezési AirportID és a dátum és idő alapján:</span><span class="sxs-lookup"><span data-stu-id="30de4-169">In a similar fashion, we join hello weather and airline data based on arrival AirportID and datetime:</span></span>

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

## <a name="save-results-toocsv-for-exchange-with-scaler"></a><span data-ttu-id="30de4-170">Mentse ScaleR eredmények tooCSV exchange-hez</span><span class="sxs-lookup"><span data-stu-id="30de4-170">Save results tooCSV for exchange with ScaleR</span></span>

<span data-ttu-id="30de4-171">Ezzel befejezte igazolnia kell a SparkR toodo hello illesztéseket.</span><span class="sxs-lookup"><span data-stu-id="30de4-171">That completes hello joins we need toodo with SparkR.</span></span> <span data-ttu-id="30de4-172">A Microsoft hello végső Spark DataFrame "joinedDF5" tooa CSV a bemeneti tooScaleR hello adatainak mentése, és zárja be a kimenő hello SparkR munkamenet.</span><span class="sxs-lookup"><span data-stu-id="30de4-172">We save hello data from hello final Spark DataFrame “joinedDF5” tooa CSV for input tooScaleR and then close out hello SparkR session.</span></span> <span data-ttu-id="30de4-173">Explicit módon biztosítunk SparkR toosave eredő CSV hello a 80 külön partíciók tooenable elegendő párhuzamos ScaleR feldolgozási:</span><span class="sxs-lookup"><span data-stu-id="30de4-173">We explicitly tell SparkR toosave hello resultant CSV in 80 separate partitions tooenable sufficient parallelism in ScaleR processing:</span></span>

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

## <a name="import-tooxdf-for-use-by-scaler"></a><span data-ttu-id="30de4-174">Importálás tooXDF ScaleR általi használatra</span><span class="sxs-lookup"><span data-stu-id="30de4-174">Import tooXDF for use by ScaleR</span></span>

<span data-ttu-id="30de4-175">CSV-fájl hello illesztett légitársaság és időjárási adataikat sikerült használjuk-a modellezési ScaleR szöveg adatforrás keresztül.</span><span class="sxs-lookup"><span data-stu-id="30de4-175">We could use hello CSV file of joined airline and weather data as-is for modeling via a ScaleR text data source.</span></span> <span data-ttu-id="30de4-176">De azt importálnia tooXDF először, mert hatékonyabb hello adatkészlethez több művelet futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="30de4-176">But we import it tooXDF first, since it is more efficient when running multiple operations on hello dataset:</span></span>

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

## <a name="splitting-data-for-training-and-test"></a><span data-ttu-id="30de4-177">Felosztása tanítási és tesztelési adatokat</span><span class="sxs-lookup"><span data-stu-id="30de4-177">Splitting data for training and test</span></span>

<span data-ttu-id="30de4-178">Azt a rxDataStep toosplit hello 2012 adatai a teszteléshez használni, és tartsa hello rest képzési:</span><span class="sxs-lookup"><span data-stu-id="30de4-178">We use rxDataStep toosplit out hello 2012 data for testing and keep hello rest for training:</span></span>

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

## <a name="train-and-test-a-logistic-regression-model"></a><span data-ttu-id="30de4-179">Betanítása és logisztikai regresszió modell teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="30de4-179">Train and test a logistic regression model</span></span>

<span data-ttu-id="30de4-180">Most már készen áll a toobuild modell folyamatban.</span><span class="sxs-lookup"><span data-stu-id="30de4-180">Now we are ready toobuild a model.</span></span> <span data-ttu-id="30de4-181">toosee hello hatással időjárási adatok késleltetése hello érkezésének ideje, az ScaleR tartozó logisztikai regresszió rutin használjuk.</span><span class="sxs-lookup"><span data-stu-id="30de4-181">toosee hello influence of weather data on delay in hello arrival time, we use ScaleR’s logistic regression routine.</span></span> <span data-ttu-id="30de4-182">Használjuk, toomodel e egy érkezési késés nagyobb, mint 15 percig hello időjárási hello indulási és érkezési repülőtereken befolyásolja:</span><span class="sxs-lookup"><span data-stu-id="30de4-182">We use it toomodel whether an arrival delay of greater than 15 minutes is influenced by hello weather at hello departure and arrival airports:</span></span>

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

<span data-ttu-id="30de4-183">Most nézzük meg, hogyan azt a hello Tesztadatok néhány előrejelzéseket és ROC és AUC megnézi.</span><span class="sxs-lookup"><span data-stu-id="30de4-183">Now let’s see how it does on hello test data by making some predictions and looking at ROC and AUC.</span></span>

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

## <a name="scoring-elsewhere"></a><span data-ttu-id="30de4-184">A pontozási máshol</span><span class="sxs-lookup"><span data-stu-id="30de4-184">Scoring elsewhere</span></span>

<span data-ttu-id="30de4-185">Azt is használható hello modell pontozási adatok más platformon.</span><span class="sxs-lookup"><span data-stu-id="30de4-185">We can also use hello model for scoring data on another platform.</span></span> <span data-ttu-id="30de4-186">Mentés tooan távoli asztali szolgáltatások fájl és átvitele, és a távoli asztali szolgáltatások importál egy cél, például az SQL Server R Services környezet pontozási.</span><span class="sxs-lookup"><span data-stu-id="30de4-186">By saving it tooan RDS file and then transferring and importing that RDS into a destination scoring environment such as SQL Server R Services.</span></span> <span data-ttu-id="30de4-187">Fontos fontos tooensure, amelyek hello tényező szintű hello adatok toobe program pontozza a mennyiségeket megegyeznek a mely hello modell lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="30de4-187">It is important tooensure that hello factor levels of hello data toobe scored match those on which hello model was built.</span></span> <span data-ttu-id="30de4-188">Hogy egyeznek megvalósítható beolvasásával, és hello modellezési keresztül ScaleR tartozó adatok mentése hello oszlopra vonatkozó információ társított `rxCreateColInfo()` függvény és majd alkalmazása az adott oszlop toohello bemeneti adatok forrása előrejelzés.</span><span class="sxs-lookup"><span data-stu-id="30de4-188">That match can be achieved by extracting and saving hello column infomation associated with hello modeling data via ScaleR’s `rxCreateColInfo()` function and then applying that column information toohello input data source for prediction.</span></span> <span data-ttu-id="30de4-189">Hello alábbi azt hello tesztelési adatkészletnél néhány sornyi mentése, bontsa ki és hello oszlopra vonatkozó információ ettől a példától hello előrejelzés parancsfájl használata:</span><span class="sxs-lookup"><span data-stu-id="30de4-189">In hello following we save a few rows of hello test dataset and extract and use hello column information from this sample in hello prediction script:</span></span>

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

## <a name="summary"></a><span data-ttu-id="30de4-190">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="30de4-190">Summary</span></span>

<span data-ttu-id="30de4-191">Ebben a cikkben azt korábban jelenik meg, hogyan azt a Hadoop Spark modell fejlesztési ScaleR adatok módosítását a lehetséges toocombine SparkR használható.</span><span class="sxs-lookup"><span data-stu-id="30de4-191">In this article, we’ve shown how it’s possible toocombine use of SparkR for data manipulation with ScaleR for model development in Hadoop Spark.</span></span> <span data-ttu-id="30de4-192">Ebben a forgatókönyvben csak fut egyszerre csak egy munkamenet külön Spark munkamenetek, karbantartása és exchange-adatok CSV-fájlok keresztül igényel.</span><span class="sxs-lookup"><span data-stu-id="30de4-192">This scenario requires that you maintain separate Spark sessions, only running one session at a time, and exchange data via CSV files.</span></span> <span data-ttu-id="30de4-193">Egyszerű, bár ez a folyamat lehet egy jövőbeli R Server a kiadásban még egyszerűbbé SparkR és ScaleR is megosztani egy Spark-munkamenetet, és így megosztani a Spark DataFrames.</span><span class="sxs-lookup"><span data-stu-id="30de4-193">Although straightforward, this process should be even easier in an upcoming R Server release, when SparkR and ScaleR can share a Spark session and so share Spark DataFrames.</span></span>

## <a name="next-steps-and-more-information"></a><span data-ttu-id="30de4-194">Következő lépések és további információ</span><span class="sxs-lookup"><span data-stu-id="30de4-194">Next steps and more information</span></span>

- <span data-ttu-id="30de4-195">R Server, a Spark használatára vonatkozó további információkért lásd: hello [kezdeti lépések útmutatóban az MSDN webhelyen](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span><span class="sxs-lookup"><span data-stu-id="30de4-195">For more information on use of R Server on Spark, see hello [Getting started guide on MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span></span>

- <span data-ttu-id="30de4-196">R Server általános információkért lásd: hello [Ismerkedés az R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) cikk.</span><span class="sxs-lookup"><span data-stu-id="30de4-196">For general information on R Server, see hello [Get started with R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) article.</span></span>

- <span data-ttu-id="30de4-197">Az R Server on HDInsight információkért lásd: [az R Server on Azure HDInsight áttekintése](hdinsight-hadoop-r-server-overview.md) és [az R Server on Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="30de4-197">For information on R Server on HDInsight, see [R Server on Azure HDInsight overview](hdinsight-hadoop-r-server-overview.md) and [R Server on Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span>

<span data-ttu-id="30de4-198">SparkR használatára vonatkozó további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="30de4-198">For more information on use of SparkR, see:</span></span>

- [<span data-ttu-id="30de4-199">Apache SparkR dokumentum</span><span class="sxs-lookup"><span data-stu-id="30de4-199">Apache SparkR document</span></span>](https://spark.apache.org/docs/2.1.0/sparkr.html)

- <span data-ttu-id="30de4-200">[SparkR áttekintése](https://docs.databricks.com/spark/latest/sparkr/overview.html) a Databricks</span><span class="sxs-lookup"><span data-stu-id="30de4-200">[SparkR Overview](https://docs.databricks.com/spark/latest/sparkr/overview.html) from Databricks</span></span>
