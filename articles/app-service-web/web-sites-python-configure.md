---
title: "Python konfigurálása az Azure App Service Web Apps alkalmazások"
description: "Ez az oktatóanyag leírja a szerzői műveletekhez és az alapszintű webes kiszolgáló átjáró felület (WSGI) kompatibilis Python alkalmazást az Azure App Service Web Apps beállításait."
services: app-service
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: fd00dc91-9935-4331-b955-4bd71e66d518
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/26/2016
ms.author: huvalo
ms.openlocfilehash: 9683a1af13eeff364d3c4714f0b791324fd82659
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-python-with-azure-app-service-web-apps"></a><span data-ttu-id="dbf9f-103">Python konfigurálása az Azure App Service Web Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="dbf9f-103">Configuring Python with Azure App Service Web Apps</span></span>
<span data-ttu-id="dbf9f-104">Ez az oktatóanyag ismerteti, szerzői és egy alapszintű Web Server átjáró felület (WSGI) kompatibilis Python alkalmazást beállításának beállítások [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="dbf9f-104">This tutorial describes options for authoring and configuring a basic Web Server Gateway Interface (WSGI) compliant Python application on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="dbf9f-105">Git üzemelő példányt, például a virtuális környezet és a csomag telepítése a requirements.txt használatával további szolgáltatásait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-105">It describes additional features of Git deployment, such as virtual environment and package installation using requirements.txt.</span></span>

## <a name="bottle-django-or-flask"></a><span data-ttu-id="dbf9f-106">Bottle, a Django vagy Flask?</span><span class="sxs-lookup"><span data-stu-id="dbf9f-106">Bottle, Django or Flask?</span></span>
<span data-ttu-id="dbf9f-107">Az Azure piactéren a Bottle, a Django és a Flask keretrendszerek sablonjait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-107">The Azure Marketplace contains templates for the Bottle, Django and Flask frameworks.</span></span> <span data-ttu-id="dbf9f-108">Ha az első webalkalmazás az Azure App Service fejleszt, vagy még nem ismeri a Gitet, javasoljuk, hogy ezek az oktatóanyagok, amely részletes utasításokat tartalmaz egy működő alkalmazást a katalógusból Git-telepítés a Windows vagy Mac használatával felépítése egyik kövesse:</span><span class="sxs-lookup"><span data-stu-id="dbf9f-108">If you are developing your first web app in Azure App Service, or you are not familiar with Git, we recommend that you follow one of these tutorials, which include step-by-step instructions for building a working application from the gallery using Git deployment from Windows or Mac:</span></span>

* [<span data-ttu-id="dbf9f-109">Webalkalmazások létrehozása a Bottle</span><span class="sxs-lookup"><span data-stu-id="dbf9f-109">Creating web apps with Bottle</span></span>](web-sites-python-create-deploy-bottle-app.md)
* [<span data-ttu-id="dbf9f-110">Webalkalmazások létrehozása a djangóval</span><span class="sxs-lookup"><span data-stu-id="dbf9f-110">Creating web apps with Django</span></span>](web-sites-python-create-deploy-django-app.md)
* [<span data-ttu-id="dbf9f-111">Webalkalmazások létrehozása a Flask</span><span class="sxs-lookup"><span data-stu-id="dbf9f-111">Creating web apps with Flask</span></span>](web-sites-python-create-deploy-flask-app.md)

## <a name="web-app-creation-on-azure-portal"></a><span data-ttu-id="dbf9f-112">Webalkalmazás létrehozása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="dbf9f-112">Web app creation on Azure Portal</span></span>
<span data-ttu-id="dbf9f-113">Ez az oktatóanyag azt feltételezi, hogy egy meglévő Azure-előfizetés és az Azure portál eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-113">This tutorial assumes an existing Azure subscription and access to the Azure Portal.</span></span>

<span data-ttu-id="dbf9f-114">Ha még nem rendelkezik egy létező webalkalmazása, létrehozhat egyet a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dbf9f-114">If you do not have an existing web app, you can create one from the [Azure Portal](https://portal.azure.com).</span></span>  <span data-ttu-id="dbf9f-115">Kattintson az új gombra a bal felső sarokban, majd **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-115">Click the NEW button in the top left corner, then click **Web + Mobile** > **Web app**.</span></span>

## <a name="git-publishing"></a><span data-ttu-id="dbf9f-116">Git-közzététel</span><span class="sxs-lookup"><span data-stu-id="dbf9f-116">Git Publishing</span></span>
<span data-ttu-id="dbf9f-117">Konfigurálja az újonnan létrehozott webalkalmazáshoz tartozó Git-közzétételt a [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben) részben megadott utasítások szerint.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-117">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="dbf9f-118">Ez az oktatóanyag a Git segítségével létrehozására, kezelésére és a Python webalkalmazás közzététele az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-118">This tutorial uses Git to create, manage, and publish our Python web app to Azure App Service.</span></span>

<span data-ttu-id="dbf9f-119">Git-közzététel beállítása után a Git-tárház létrehozása, és a webalkalmazás társított.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-119">Once Git publishing is set up, a Git repository will be created and associated with your web app.</span></span> <span data-ttu-id="dbf9f-120">A tárház URL-cím fog megjelenni, és ezentúl segítségével küldje le adatokat a helyi fejlesztési környezetet a felhőben.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-120">The repository's URL will be displayed and can henceforth be used to push data from the local development environment to the cloud.</span></span> <span data-ttu-id="dbf9f-121">Git keresztül alkalmazások közzétételét, győződjön meg arról, hogy a Git-ügyfél telepítése, és kövesse az utasításokat, majd a webtartalom-alkalmazás az Azure App Service megadott.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-121">To publish applications via Git, make sure a Git client is also installed and use the instructions provided to push your web app content to Azure App Service.</span></span>

## <a name="application-overview"></a><span data-ttu-id="dbf9f-122">Az alkalmazás áttekintése</span><span class="sxs-lookup"><span data-stu-id="dbf9f-122">Application Overview</span></span>
<span data-ttu-id="dbf9f-123">A következő szakaszok a következő fájlok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-123">In the next sections, the following files are created.</span></span> <span data-ttu-id="dbf9f-124">A Git-tárház gyökérkönyvtárában található kerüljön.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-124">They should be placed in the root of the Git repository.</span></span>

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a><span data-ttu-id="dbf9f-125">WSGI kezelő</span><span class="sxs-lookup"><span data-stu-id="dbf9f-125">WSGI Handler</span></span>
<span data-ttu-id="dbf9f-126">WSGI egy Python szabvány szerint [EGP 3333](http://www.python.org/dev/peps/pep-3333/) meghatározása a webkiszolgáló és a Python közötti illesztőfelületet szolgáltasson.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-126">WSGI is a Python standard described by [PEP 3333](http://www.python.org/dev/peps/pep-3333/) defining an interface between the web server and Python.</span></span> <span data-ttu-id="dbf9f-127">Egy egységes felületet biztosít írása a webalkalmazások és -keretrendszerek pythonos környezetekben.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-127">It provides a standardized interface for writing various web applications and frameworks using Python.</span></span> <span data-ttu-id="dbf9f-128">Python webes népszerű keretrendszerekre ma használható WSGI.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-128">Popular Python web frameworks today use WSGI.</span></span> <span data-ttu-id="dbf9f-129">Támogatja az ilyen keretrendszerek; az Azure App Service Web Apps által biztosított Ezenkívül advanced-felhasználók is létrehozható a saját mindaddig, amíg a egyéni kezelő követi az WSGI specification útmutatást.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-129">Azure App Service Web Apps gives you support for any such frameworks; in addition, advanced users can even author their own as long as the custom handler follows the WSGI specification guidelines.</span></span>

<span data-ttu-id="dbf9f-130">Íme egy példa egy `app.py` , amely meghatározza, hogy egy egyéni kezelő:</span><span class="sxs-lookup"><span data-stu-id="dbf9f-130">Here's an example of an `app.py` that defines a custom handler:</span></span>

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

<span data-ttu-id="dbf9f-131">Az alkalmazást helyileg is futtathatja `python app.py`, majd tallózással `http://localhost:5555` a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-131">You can run this application locally with `python app.py`, then browse to `http://localhost:5555` in your web browser.</span></span>

## <a name="virtual-environment"></a><span data-ttu-id="dbf9f-132">Virtuális környezet</span><span class="sxs-lookup"><span data-stu-id="dbf9f-132">Virtual Environment</span></span>
<span data-ttu-id="dbf9f-133">Bár a fenti példa alkalmazás nincs szükség külső csomagok, valószínű, hogy az alkalmazás néhány szükséges.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-133">Although the example app above doesn't require any external packages, it is likely that your application will require some.</span></span>

<span data-ttu-id="dbf9f-134">Külső csomagfüggőségek kezelése érdekében a Azure Git-telepítés a virtuális környezetek létrehozását támogatja.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-134">To help manage external package dependencies, Azure Git deployment supports the creation of virtual environments.</span></span>

<span data-ttu-id="dbf9f-135">Ha az Azure a requirements.txt észlel a tárház gyökérkönyvtárában található, automatikusan létrehoz egy nevű virtuális környezet `env`.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-135">When Azure detects a requirements.txt in the root of the repository, it automatically creates a virtual environment named `env`.</span></span> <span data-ttu-id="dbf9f-136">Csak az első központi telepítése történik, vagy a telepítést követően a kijelölt Python során futásidejű megváltozott.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-136">This only occurs on the first deployment, or during any deployment after the selected Python runtime has changed.</span></span>

<span data-ttu-id="dbf9f-137">Valószínűleg érdemes lehet létrehozni egy helyi fejlesztési virtuális környezetről, de nem adja hozzá a Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-137">You will probably want to create a virtual environment locally for development, but don't include it in your Git repository.</span></span>

## <a name="package-management"></a><span data-ttu-id="dbf9f-138">Csomagkezelés</span><span class="sxs-lookup"><span data-stu-id="dbf9f-138">Package Management</span></span>
<span data-ttu-id="dbf9f-139">A requirements.txt fájlban felsorolt csomagok automatikusan települnek a pip használatával a virtuális környezetben.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-139">Packages listed in requirements.txt will be installed automatically in the virtual environment using pip.</span></span> <span data-ttu-id="dbf9f-140">Ez történik, minden üzembe helyezés, de a pip kihagyja a telepítést, ha a csomag már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-140">This happens on every deployment, but pip will skip installation if a package is already installed.</span></span>

<span data-ttu-id="dbf9f-141">Példa `requirements.txt`:</span><span class="sxs-lookup"><span data-stu-id="dbf9f-141">Example `requirements.txt`:</span></span>

    azure==0.8.4


## <a name="python-version"></a><span data-ttu-id="dbf9f-142">Python-verzió</span><span class="sxs-lookup"><span data-stu-id="dbf9f-142">Python Version</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

<span data-ttu-id="dbf9f-143">Példa `runtime.txt`:</span><span class="sxs-lookup"><span data-stu-id="dbf9f-143">Example `runtime.txt`:</span></span>

    python-2.7


## <a name="webconfig"></a><span data-ttu-id="dbf9f-144">Web.config</span><span class="sxs-lookup"><span data-stu-id="dbf9f-144">Web.config</span></span>
<span data-ttu-id="dbf9f-145">Hozzon létre egy web.config fájlt adhatja meg, hogy a kiszolgáló hogyan kezelje kérelmek lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-145">You'll need to create a web.config file to specify how the server should handle requests.</span></span>

<span data-ttu-id="dbf9f-146">Vegye figyelembe, hogy a beállítást, ha egy web.x.y.config fájlt a tárházban, ahol x megfelel a kiválasztott Python-futtatókörnyezet, majd az Azure automatikusan másolatot készít a megfelelő web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-146">Note that if you have a web.x.y.config file in your repository, where x.y matches the selected Python runtime, then Azure will automatically copy the appropriate file as web.config.</span></span>

<span data-ttu-id="dbf9f-147">A következő web.config példák virtuális környezet proxyparancsfájl, amely a következő szakaszban ismertetett támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-147">The following web.config examples rely on a virtual environment proxy script, which is described in the next section.</span></span>  <span data-ttu-id="dbf9f-148">Ezek a példában használt WSGI kezelő működnek `app.py` felett.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-148">They work with the WSGI handler used in the example `app.py` above.</span></span>

<span data-ttu-id="dbf9f-149">Példa `web.config` Python 2.7-hez:</span><span class="sxs-lookup"><span data-stu-id="dbf9f-149">Example `web.config` for Python 2.7:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


<span data-ttu-id="dbf9f-150">Példa `web.config` Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="dbf9f-150">Example `web.config` for Python 3.4:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


<span data-ttu-id="dbf9f-151">Statikus fájlok kezelik a webkiszolgáló által közvetlenül, Python kódját, javítja a teljesítményt áthaladás nélkül.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-151">Static files will be handled by the web server directly, without going through Python code, for improved performance.</span></span>

<span data-ttu-id="dbf9f-152">A fenti példákban a lemezen a statikus fájlokat meg kell felelnie a hely URL-címét.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-152">In the above examples, the location of the static files on disk should match the location in the URL.</span></span> <span data-ttu-id="dbf9f-153">Ez azt jelenti, hogy kérelmet `http://pythonapp.azurewebsites.net/static/site.css` szolgálja ki a fájl a lemezen `\static\site.css`.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-153">This means that a request for `http://pythonapp.azurewebsites.net/static/site.css` will serve the file on disk at `\static\site.css`.</span></span>

<span data-ttu-id="dbf9f-154">`WSGI_ALT_VIRTUALENV_HANDLER`van, amelyben meg kell határoznia a WSGI kezelő.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-154">`WSGI_ALT_VIRTUALENV_HANDLER` is where you specify the WSGI handler.</span></span> <span data-ttu-id="dbf9f-155">A fenti példákban rendelkezik `app.wsgi_app` , mert a kezelő nevű függvény `wsgi_app` a `app.py` a gyökérmappában.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-155">In the above examples, it's `app.wsgi_app` because the handler is a function named `wsgi_app` in `app.py` in the root folder.</span></span>

<span data-ttu-id="dbf9f-156">`PYTHONPATH`testre szabható, de ha a requirements.txt fájlban megadásával a virtuális környezetben telepíti a függőségek, nem szabad módosítani szeretné azt.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-156">`PYTHONPATH` can be customized, but if you install all your dependencies in the virtual environment by specifying them in requirements.txt, you shouldn't need to change it.</span></span>

## <a name="virtual-environment-proxy"></a><span data-ttu-id="dbf9f-157">Virtuális környezet Proxy</span><span class="sxs-lookup"><span data-stu-id="dbf9f-157">Virtual Environment Proxy</span></span>
<span data-ttu-id="dbf9f-158">A következő parancsfájl beolvasni a WSGI kezelő, illetve aktiválja a virtuális környezet és a naplófájlok hibák szolgál.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-158">The following script is used to retrieve the WSGI handler, activate the virtual environment and log errors.</span></span> <span data-ttu-id="dbf9f-159">Általános és módosítások nélkül használható tervezték.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-159">It is designed to be generic and used without modifications.</span></span>

<span data-ttu-id="dbf9f-160">Tartalmát `ptvs_virtualenv_proxy.py`:</span><span class="sxs-lookup"><span data-stu-id="dbf9f-160">Contents of `ptvs_virtualenv_proxy.py`:</span></span>

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n')

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')

        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)

        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()

        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))

        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []

        site.main()

        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a><span data-ttu-id="dbf9f-161">Git-telepítés testreszabása</span><span class="sxs-lookup"><span data-stu-id="dbf9f-161">Customize Git deployment</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="dbf9f-162">Hibaelhárítás – Csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="dbf9f-162">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="dbf9f-163">Hibaelhárítás – Virtuális környezet</span><span class="sxs-lookup"><span data-stu-id="dbf9f-163">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="dbf9f-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dbf9f-164">Next steps</span></span>
<span data-ttu-id="dbf9f-165">További információ: [Python fejlesztői központban](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="dbf9f-165">For more information, see the [Python Developer Center](/develop/python/).</span></span>

> [!NOTE]
> <span data-ttu-id="dbf9f-166">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-166">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="dbf9f-167">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="dbf9f-167">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="dbf9f-168">A változások</span><span class="sxs-lookup"><span data-stu-id="dbf9f-168">What's changed</span></span>
* <span data-ttu-id="dbf9f-169">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="dbf9f-169">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

