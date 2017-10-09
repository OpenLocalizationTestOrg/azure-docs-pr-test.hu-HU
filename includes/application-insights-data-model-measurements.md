<span data-ttu-id="95fbb-101">Egyéni mérési adatok gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="95fbb-101">Collection of custom measurements.</span></span> <span data-ttu-id="95fbb-102">A társított hello telemetriai elemet mértékegység nevű gyűjtemény tooreport használja.</span><span class="sxs-lookup"><span data-stu-id="95fbb-102">Use this collection tooreport named measurement associated with hello telemetry item.</span></span> <span data-ttu-id="95fbb-103">A tipikus használati esetek a következők:</span><span class="sxs-lookup"><span data-stu-id="95fbb-103">Typical use cases are:</span></span>
- <span data-ttu-id="95fbb-104">– Függőségi Telemetria hasznos hello mérete</span><span class="sxs-lookup"><span data-stu-id="95fbb-104">hello size of Dependency Telemetry payload</span></span>
- <span data-ttu-id="95fbb-105">hello Telemetriai kérelem által feldolgozott várólista elemek száma</span><span class="sxs-lookup"><span data-stu-id="95fbb-105">hello number of queue items processed by Request Telemetry</span></span>
- <span data-ttu-id="95fbb-106">időt, hogy az ügyfél az toocomplete hello lépés varázsló lépés befejezése esemény Telemetriai vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="95fbb-106">time that customer took toocomplete hello step in wizard step completion Event Telemetry.</span></span>

<span data-ttu-id="95fbb-107">Lekérheti [egyéni mértékek](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) alkalmazás Analytics:</span><span class="sxs-lookup"><span data-stu-id="95fbb-107">You can query [custom measurements](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in Application Analytics:</span></span>

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > <span data-ttu-id="95fbb-108">Egyéni mértékek társított hello telemetriai elem tartoznak.</span><span class="sxs-lookup"><span data-stu-id="95fbb-108">Custom measurements are associated with hello telemetry item they belong to.</span></span> <span data-ttu-id="95fbb-109">A fenti mérések tartalmazó hello telemetriai elemhez tulajdonos toosampling.</span><span class="sxs-lookup"><span data-stu-id="95fbb-109">They are subject toosampling with hello telemetry item containing those measurements.</span></span> <span data-ttu-id="95fbb-110">egy mérték, amely független a más telemetriai, használjon értéke tootrack [metrika telemetriai](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="95fbb-110">tootrack a measurement that has a value independent from other telemetry types, use [Metric telemetry](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="95fbb-111">Maximális kulcshossz: 150</span><span class="sxs-lookup"><span data-stu-id="95fbb-111">Max key length: 150</span></span>
