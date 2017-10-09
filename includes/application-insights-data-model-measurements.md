Egyéni mérési adatok gyűjtése. A társított hello telemetriai elemet mértékegység nevű gyűjtemény tooreport használja. A tipikus használati esetek a következők:
- – Függőségi Telemetria hasznos hello mérete
- hello Telemetriai kérelem által feldolgozott várólista elemek száma
- időt, hogy az ügyfél az toocomplete hello lépés varázsló lépés befejezése esemény Telemetriai vett igénybe.

Lekérheti [egyéni mértékek](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) alkalmazás Analytics:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Egyéni mértékek társított hello telemetriai elem tartoznak. A fenti mérések tartalmazó hello telemetriai elemhez tulajdonos toosampling. egy mérték, amely független a más telemetriai, használjon értéke tootrack [metrika telemetriai](../articles/application-insights/app-insights-api-custom-events-metrics.md).

Maximális kulcshossz: 150
