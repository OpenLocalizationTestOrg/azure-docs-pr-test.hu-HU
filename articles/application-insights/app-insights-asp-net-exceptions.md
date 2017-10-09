---
title: "aaaDiagnose hibákat és kivételeket a webes alkalmazásokat az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "ASP.NET alkalmazások együtt – kéréstelemetria kivételei rögzíti."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 8930e6d2b29f83ea635c4ecb7afd11fc1d97d085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Kivételek az Application insights szolgáltatással a webalkalmazások diagnosztizálásához
Az élő webalkalmazását kivételek által jelentett [Application Insights](app-insights-overview.md). Sikertelen kérelmek hozhatók kivételeket és hello ügyfél és a kiszolgáló, az eseményeket, így gyorsan diagnosztizálhatja a hello okok.

## <a name="set-up-exception-reporting"></a>Kivétel jelentéskészítés beállítása
* a kiszolgáló alkalmazás jelentett toohave kivételek:
  * Telepítés [Application Insights SDK](app-insights-asp-net.md) az alkalmazás kódjában, vagy
  * Az IIS webkiszolgálón: futtatása [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); vagy
  * Azure-webalkalmazásokban: hello hozzáadása [Application Insights-bővítmény](app-insights-azure-web-apps.md)
  * Java-webalkalmazások: telepítés hello [Java-ügynök](app-insights-java-agent.md)
* Telepítse a hello [JavaScript részlet](app-insights-javascript.md) a weblapok toocatch böngésző kivételek a.
* Az egyes alkalmazás-keretrendszerbeli vagy néhány beállításokkal, szükség tootake néhány további lépést toocatch további kivételek:
  * [Web Forms keretrendszerre](#web-forms)
  * [MVC](#mvc)
  * [Webes API-1.*](#web-api-1)
  * [Webes API-k 2.*](#web-api-2)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Visual Studio használatával kivételek diagnosztizálásáról
Nyissa meg a hello app megoldást a Visual Studio toohelp a hibakeresés.

A kiszolgálón vagy a fejlesztői gépen F5 használatával hello alkalmazás fut.

A Visual Studio hello Application Insights keresési ablak megnyitásához, majd állítsa be toodisplay események az alkalmazásból. Hibakeresése, közben ehhez csak hello Application Insights gombra kattintva.

![Kattintson a jobb gombbal a projekt hello, és válassza ki az Application Insights részére, nyissa meg.](./media/app-insights-asp-net-exceptions/34.png)

Figyelje meg, hogy hello jelentés tooshow csak kivételek szűrheti.

*Nincsenek kivételek megjelenítő? Lásd: [kivételek rögzítése](#exceptions).*

Kattintson egy kivétel jelentés tooshow a Veremkivonat.
Kattintson egy sor mutató hivatkozás megadásával hello Veremkivonat, tooopen hello vonatkozó forráskód fájlja.  

Hello kódban figyelje meg, hogy a CodeLens hello kivételek adatait jeleníti meg:

![Kivételek CodeLens értesítés.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-hello-azure-portal"></a>Hello Azure-portál használatával hibák diagnosztizálása
Hello Application Insights az alkalmazás áttekintésében hello hibák csempe elsajátíthatja, hogy a kivételek diagramok és sikertelen HTTP-kérelmekre együtt hello listája kérelem URL-címek, amelyek hello leggyakoribb hibákhoz vezethet.

![Válassza ki a beállítások, hibák](./media/app-insights-asp-net-exceptions/012-start.png)

Kattintson valamelyik hello keresztül nem sikerült a kivétel típusú hello lista tooget tooindividual eseményeket a hello kivétel, ahol hello részleteinek és Veremkivonat:

![Válasszon ki egy példányt a sikertelen kérelmek, valamint a kivétel részletes adatai alapján, majd a tooinstances hello kivétel.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

**Másik lehetőségként** hello lista-tól kezdődnek, és kivételeket kapcsolódó tooit található.

*Nincsenek kivételek megjelenítő? Lásd: [kivételek rögzítése](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Egyéni nyomkövetés és naplóadatok
diagnosztikai adatok adott tooyour app tooget, kód toosend beszúrásához saját telemetriai adatokat. Ez a diagnosztikai keresési hello kérelem, nézet és egyéb automatikusan gyűjtött adatok mellett jelenik meg.

Több lehetőség közül választhat:

* [A trackevent() függvény](app-insights-api-custom-events-metrics.md#trackevent) tipikus felhasználási területe a használati szokásokat, de is küld jelenik meg adat az egyéni események diagnosztikai keresési hello figyelése. Események megnevezett, és képes továbbítani az kapcsolatikarakterlánc-tulajdonságokat és amelyen is numerikus metrikák [szűrheti a diagnosztikai keresések](app-insights-diagnostic-search.md).
* [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lehetővé teszi, hogy hosszabb adatküldés például a POST-adatokat.
* [TrackException()](#exceptions) küld kivételeseményekhez megadhat veremkiíratásokat. [További tudnivalók a kivételek](#exceptions).
* Ha már használ egy naplózási keretrendszert, például a Log4Net vagy NLog, akkor [lesznek a naplók rögzítése](app-insights-asp-net-trace-logs.md) és lássák a diagnosztikai keresési kérelem és a kivétel adatai mellett.

toosee ezeket az eseményeket, nyissa meg a [keresési](app-insights-diagnostic-search.md), nyissa meg a szűrő, és válassza a egyéni esemény, nyomkövetésre, vagy kivétel.

![Részletezés](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Az alkalmazás nagy mennyiségű telemetriai adatokat állít elő, ha hello adaptív mintavételi modul automatikusan toohello portal reprezentatív része események küldése által küldött hello kötet csökkenti. Események eszköz részét képező hello kell kiválasztott vagy nincs kijelölve csoportosan ugyanazt a műveletet úgy, hogy a kapcsolódó események közti léphet. [További információk a mintavétel.](app-insights-sampling.md)
>
>

### <a name="how-toosee-request-post-data"></a>Hogyan toosee POST-adatokat kér
Szolgáltatáskérés részleteinek nem tartalmazhat tooyour app elküldve a POST híváson hello adatokat. Ezek az adatok jelentett toohave:

* [Hello SDK telepítése](app-insights-asp-net.md) a alkalmazás projektben.
* Helyezze be a kódot az alkalmazás toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace). Hello POST-adatokat küldenek hello üzenet paraméter. Nincs mérete engedélyezett toohello, t érdemes kipróbálni! toosend csak hello lényeges adat.
* A sikertelen kérelmek vizsgálatánál található hello tartozó nyomkövetési adatokat.  

![Részletezés](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a>Kivételeket és a kapcsolódó diagnosztikai adatokat rögzítése
Először nem jelenik meg a portál hello hello hibákhoz vezethet az alkalmazás összes kivétel. Látni fogja, a böngésző kivételek (hello használata [JavaScript SDK](app-insights-javascript.md) a weblapok). A legtöbb kivételek az IIS által észlelt és toowrite kód toosee bit van, de azokat.

A következőket teheti:

* **Kivételek explicit módon jelentkezzen** úgy kód kivétel kezelők tooreport hello kivételek.
* **Kivételek automatikusan rögzítése** úgy konfigurálja az ASP.NET keretrendszer. hello szükséges kiegészítés eltérőek a keretrendszer különböző típusú.

## <a name="reporting-exceptions-explicitly"></a>Explicit módon Reporting kivételek
hello legegyszerűbb módja a tooinsert egy hívás tooTrackException() a egy kivételkezelőbe.

JavaScript

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

C#

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send hello exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

VISUAL BASIC

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send hello exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

hello tulajdonságok és mérések paraméterek megadása nem kötelező, de hasznos a [szűrést és a hozzáadása](app-insights-diagnostic-search.md) további információt. Például ha egy alkalmazás futtatható több játékok, találta az összes hello kivétel jelentések kapcsolódó tooa játék. Hozzáadáshoz annyi, ha Ön például tooeach szótárban.

## <a name="browser-exceptions"></a>Böngészőkivételek
A legtöbb webböngésző kivételek jelenti.

Ha a weblap tartalmazza a parancsfájlok tartalomkézbesítési hálózat vagy a más tartományokban, győződjön meg arról, a script kód hello attribútummal rendelkezik ```crossorigin="anonymous"```, és adott hello a kiszolgáló elküldi [CORS fejlécek](http://enable-cors.org/). Ez lehetővé teszi egy Veremkivonat tooget és ezeket az erőforrásokat a nem kezelt JavaScript-kivételek adatai.

## <a name="web-forms"></a>Web Forms keretrendszerre
Web Forms keretrendszerre hello HTTP-modul is képes toocollect hello kivételek nincs konfigurálva a CustomErrors átirányítások esetén.

De ha aktív átirányításokat, adja hozzá a következő sorokat toohello Application_Error függvényt Global.asax.cs hello. (Ha adja hozzá a Global.asax fájl még nem rendelkezik.)

*C#*

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a>MVC
Ha hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfiguráció `Off`, majd kivételeket lesz elérhető a hello [HTTP-modulja](https://msdn.microsoft.com/library/ms178468.aspx) toocollect. Azonban ha `RemoteOnly` (alapértelmezett), vagy `On`, majd hello kivétel törlődik, és nem érhető el, az Application Insights tooautomatically gyűjtése. Ezt úgy javíthatja ki, amely hello felülbírálásával [System.Web.Mvc.HandleErrorAttribute osztály](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), és felül hello osztály alkalmazása hello különböző verziójához MVC alább látható módon ([github forrás](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report hello exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a>MVC 2
Hello HandleError attribútum cserélje le a tartományvezérlőket az új attribútumot.

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[Minta](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Regisztrálni `AiHandleErrorAttribute` Global.asax.cs globális szűrőként:

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[Minta](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
FilterConfig.cs globális szűrőként regisztrálja a AiHandleErrorAttribute:

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with hello override tootrack unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[Minta](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a>Webes API-t 1.x
Bírálja felül System.Web.Http.Filters.ExceptionFilterAttribute:

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

Adja hozzá a felülbírált attribútum toospecific tartományvezérlők, vagy vegye fel globális szűrőkonfigurációt toohello hello register osztályban:

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[Minta](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

Nincsenek olyan esetek számát, amelyet hello kivételszűrők nem tudja kezelni. Példa:

* A vezérlő konstruktorok kivételek.
* Az üzenet kezelők kivételek.
* Útválasztás során okozott kivételeket.
* Válasz a tartalom szerializálás során okozott kivételeket.

## <a name="web-api-2x"></a>Web API 2.x
Iexceptionlogger felület egy megvalósításának hozzáadása:

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

Adja hozzá a toohello szolgáltatások register:

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  }

[Minta](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Alternatív megoldásként sikerült:

1. Cserélje le csak egy egyéni végrehajtásának IExceptionHandler ExceptionHandler hello. Csak ennek meghívása az hello keretrendszer esetén is képes toochoose mely válasz üzenet toosend (amikor például hello kapcsolat megszakadt. nem)
2. Kivétel szűrők (leírtak hello szakasz a webes API 1.x tartományvezérlőkön a fenti) - hívása nem minden esetben.

## <a name="wcf"></a>WCF
Adjon hozzá egy osztály, amely kibővíti az attribútumot, és megvalósítja az IErrorHandler és az IServiceBehavior.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

Hello attribútum toohello szolgáltatás megvalósítások hozzáadása:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[Minta](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Kivétel teljesítményszámlálói
Ha rendelkezik [hello Application Insights-ügynök telepítve](app-insights-monitor-performance-live-website-now.md) a kiszolgálón, és hello kivételek, .NET mérhető diagram kaphat. Ez magában foglalja a kezelt, mind a nem kezelt .NET-kivételek.

Nyissa meg a metrika Explorer panelt, új diagram hozzáadása, és válassza ki **kivétel arány**, a teljesítményszámlálók felsorolt.

hello .NET-keretrendszer hello arány kivételek száma hello leltár időközönkénti és hello időköz hello hosszát elosztjuk számítja ki.

Vegye figyelembe, hogy az eltérő lesz hello "kivétel" count hello Application Insights portál TrackException jelentések alapján számítja ki. hello mintavételi időköze különböző, és hello SDK nem küldje minden kezelt és kezeletlen kivételek TrackException a jelentésekre.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a>Következő lépések
* [REST, SQL és egyéb hívások toodependencies figyelése](app-insights-asp-net-dependencies.md)
* [Lapbetöltési idők, a böngésző kivételeket és a AJAX-hívások figyelése](app-insights-javascript.md)
* [A figyelő teljesítményszámlálói](app-insights-performance-counters.md)
