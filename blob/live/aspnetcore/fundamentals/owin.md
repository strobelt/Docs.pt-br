---
title: OWIN (Open Web Interface para .NET)
author: ardalis
description: "Descobrir como ASP.NET Core dá suporte à Interface da Web aberto para .NET (OWIN), que permite que aplicativos da web para ser separada dos servidores web."
keywords: "Núcleo do ASP.NET, Interface Web aberta para .NET, OWIN"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2ee970a1c9cd05ebee76b92c3e2c7c6c6cc6ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="ea892-104">Introdução ao abrir a Interface da Web para .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="ea892-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="ea892-105">Por [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ea892-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ea892-106">O ASP.NET Core dá suporte para OWIN (Open Web Interface para .NET).</span><span class="sxs-lookup"><span data-stu-id="ea892-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="ea892-107">O OWIN permite que os aplicativos Web sejam separados dos servidores Web.</span><span class="sxs-lookup"><span data-stu-id="ea892-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="ea892-108">Define uma maneira padronizada de middleware para ser usada em um pipeline para manipular solicitações e respostas associadas.</span><span class="sxs-lookup"><span data-stu-id="ea892-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="ea892-109">Middleware e aplicativos do ASP.NET Core podem interoperar com middleware, servidores e aplicativos baseados no OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea892-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="ea892-110">OWIN fornece uma camada de dissociação que permite duas estruturas com modelos de objeto diferentes para ser usados juntos.</span><span class="sxs-lookup"><span data-stu-id="ea892-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="ea892-111">O `Microsoft.AspNetCore.Owin` pacote fornece duas implementações de adaptador:</span><span class="sxs-lookup"><span data-stu-id="ea892-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="ea892-112">ASP.NET Core para OWIN</span><span class="sxs-lookup"><span data-stu-id="ea892-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="ea892-113">OWIN para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea892-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="ea892-114">Isso permite que o ASP.NET Core ser hospedado em um servidor compatível OWIN/host ou para outros componentes compatíveis OWIN a ser executado sobre o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea892-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="ea892-115">Observação: Usar esses adaptadores vem com um custo de desempenho.</span><span class="sxs-lookup"><span data-stu-id="ea892-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="ea892-116">Aplicativos que usam somente os componentes principais do ASP.NET não devem usar o pacote de Owin ou adaptadores.</span><span class="sxs-lookup"><span data-stu-id="ea892-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="ea892-117">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ea892-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="ea892-118">Executando OWIN middleware no pipeline do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea892-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="ea892-119">O suporte do ASP.NET Core OWIN é implantado como parte do `Microsoft.AspNetCore.Owin` pacote.</span><span class="sxs-lookup"><span data-stu-id="ea892-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="ea892-120">Você pode importar suporte OWIN para seu projeto ao instalar este pacote.</span><span class="sxs-lookup"><span data-stu-id="ea892-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="ea892-121">Middleware OWIN está em conformidade com a [especificação OWIN](http://owin.org/spec/spec/owin-1.0.0.html), que requer um `Func<IDictionary<string, object>, Task>` interface e as chaves específicas ser definido (como `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="ea892-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="ea892-122">Middleware OWIN simple seguinte exibe "Hello World":</span><span class="sxs-lookup"><span data-stu-id="ea892-122">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="ea892-123">A assinatura de exemplo retorna um `Task` e aceita um `IDictionary<string, object>` conforme solicitado pelo OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea892-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="ea892-124">O código a seguir mostra como adicionar o `OwinHello` middleware (mostrado acima) para o pipeline do ASP.NET com o `UseOwin` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="ea892-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="ea892-125">Você pode configurar outras ações para ocorrer no pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea892-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="ea892-126">Cabeçalhos de resposta só devem ser modificados antes da primeira gravação no fluxo de resposta.</span><span class="sxs-lookup"><span data-stu-id="ea892-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="ea892-127">Diversas chamadas para `UseOwin` não é recomendado por motivos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="ea892-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="ea892-128">Componentes OWIN funcionarão melhor se agrupados juntos.</span><span class="sxs-lookup"><span data-stu-id="ea892-128">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="ea892-129">Usando o ASP.NET de hospedagem em um servidor baseado em OWIN</span><span class="sxs-lookup"><span data-stu-id="ea892-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="ea892-130">Servidores baseados no OWIN podem hospedar aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ea892-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="ea892-131">Um desses servidores é [Nowin](https://github.com/Bobris/Nowin), um servidor da web .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea892-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="ea892-132">O exemplo deste artigo, incluí um projeto que faz referência a Nowin e usa-o para criar um `IServer` capaz de hospedar o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea892-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="ea892-133">`IServer`é uma interface que requer um `Features` propriedade e um `Start` método.</span><span class="sxs-lookup"><span data-stu-id="ea892-133">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="ea892-134">`Start`é responsável por configurar e iniciar o servidor, que nesse caso é feito por meio de uma série de chamadas de API fluentes que definir endereços analisados a partir de IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="ea892-134">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="ea892-135">Observe que a configuração fluente do `_builder` variável Especifica que as solicitações serão manipuladas pelo `appFunc` definido anteriormente no método.</span><span class="sxs-lookup"><span data-stu-id="ea892-135">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="ea892-136">Isso `Func` é chamado em cada solicitação para processar solicitações de entrada.</span><span class="sxs-lookup"><span data-stu-id="ea892-136">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="ea892-137">Nós adicionaremos um `IWebHostBuilder` extensão para tornar mais fácil adicionar e configurar o servidor Nowin.</span><span class="sxs-lookup"><span data-stu-id="ea892-137">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="ea892-138">Com isso em vigor, tudo o que é necessário para executar um aplicativo ASP.NET usando o servidor personalizado para chamar a extensão na *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea892-138">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="ea892-139">Saiba mais sobre o ASP.NET [servidores](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ea892-139">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="ea892-140">Executar o ASP.NET Core em um servidor baseado em OWIN e usar seu suporte de WebSockets</span><span class="sxs-lookup"><span data-stu-id="ea892-140">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="ea892-141">Outro exemplo de recursos de servidores como baseados no OWIN pode ser aproveitado por ASP.NET Core é o acesso a recursos como o WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ea892-141">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="ea892-142">O servidor de web OWIN .NET usado no exemplo anterior tem suporte para soquetes da Web interna, que podem ser aproveitadas por um aplicativo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea892-142">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="ea892-143">O exemplo a seguir mostra um aplicativo simples da web que suporta soquetes da Web e exibe novamente tudo enviados ao servidor por meio de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ea892-143">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="ea892-144">Isso [exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) é configurado usando o mesmo `NowinServer` que o anterior - a única diferença está em como o aplicativo está configurado no seu `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="ea892-144">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="ea892-145">Um teste usando [um cliente do websocket simples](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstra o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="ea892-145">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Cliente de teste de soquete da Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="ea892-147">Ambiente OWIN</span><span class="sxs-lookup"><span data-stu-id="ea892-147">OWIN environment</span></span>

<span data-ttu-id="ea892-148">Você pode construir um ambiente OWIN usando o `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="ea892-148">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="ea892-149">Chaves OWIN</span><span class="sxs-lookup"><span data-stu-id="ea892-149">OWIN keys</span></span>

<span data-ttu-id="ea892-150">Depende do OWIN um `IDictionary<string,object>` objeto para comunicar informações ao longo de uma troca de solicitação/resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ea892-150">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="ea892-151">ASP.NET Core implementa as chaves listadas abaixo.</span><span class="sxs-lookup"><span data-stu-id="ea892-151">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="ea892-152">Consulte o [especificação primária, extensões](http://owin.org/#spec), e [OWIN chave diretrizes e chaves comuns](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="ea892-152">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="ea892-153">Dados de solicitação (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="ea892-153">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="ea892-154">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-154">Key</span></span>               | <span data-ttu-id="ea892-155">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-155">Value (type)</span></span> | <span data-ttu-id="ea892-156">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-156">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-157">owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="ea892-157">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="ea892-158">owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="ea892-158">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="ea892-159">owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="ea892-159">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="ea892-160">owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="ea892-160">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="ea892-161">owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="ea892-161">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="ea892-162">owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="ea892-162">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="ea892-163">owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="ea892-163">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="ea892-164">owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="ea892-164">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="ea892-165">Dados de solicitação (OWIN v 1.1.0)</span><span class="sxs-lookup"><span data-stu-id="ea892-165">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="ea892-166">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-166">Key</span></span>               | <span data-ttu-id="ea892-167">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-167">Value (type)</span></span> | <span data-ttu-id="ea892-168">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-168">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-169">owin. RequestId</span><span class="sxs-lookup"><span data-stu-id="ea892-169">owin.RequestId</span></span> | `String` | <span data-ttu-id="ea892-170">Opcional</span><span class="sxs-lookup"><span data-stu-id="ea892-170">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="ea892-171">Dados de resposta (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="ea892-171">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="ea892-172">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-172">Key</span></span>               | <span data-ttu-id="ea892-173">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-173">Value (type)</span></span> | <span data-ttu-id="ea892-174">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-174">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-175">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="ea892-175">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="ea892-176">Opcional</span><span class="sxs-lookup"><span data-stu-id="ea892-176">Optional</span></span> |
| <span data-ttu-id="ea892-177">owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="ea892-177">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="ea892-178">Opcional</span><span class="sxs-lookup"><span data-stu-id="ea892-178">Optional</span></span> |
| <span data-ttu-id="ea892-179">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="ea892-179">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="ea892-180">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="ea892-180">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="ea892-181">Outros dados (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="ea892-181">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="ea892-182">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-182">Key</span></span>               | <span data-ttu-id="ea892-183">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-183">Value (type)</span></span> | <span data-ttu-id="ea892-184">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-184">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-185">owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="ea892-185">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="ea892-186">owin. Versão</span><span class="sxs-lookup"><span data-stu-id="ea892-186">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="ea892-187">Chaves comuns</span><span class="sxs-lookup"><span data-stu-id="ea892-187">Common Keys</span></span>

| <span data-ttu-id="ea892-188">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-188">Key</span></span>               | <span data-ttu-id="ea892-189">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-189">Value (type)</span></span> | <span data-ttu-id="ea892-190">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-190">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-191">SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="ea892-191">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="ea892-192">SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="ea892-192">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="ea892-193">servidor. RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="ea892-193">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="ea892-194">servidor. Porta_remota</span><span class="sxs-lookup"><span data-stu-id="ea892-194">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="ea892-195">servidor. LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="ea892-195">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="ea892-196">servidor. LocalPort</span><span class="sxs-lookup"><span data-stu-id="ea892-196">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="ea892-197">servidor. IsLocal</span><span class="sxs-lookup"><span data-stu-id="ea892-197">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="ea892-198">servidor. OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="ea892-198">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="ea892-199">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="ea892-199">SendFiles v0.3.0</span></span>

| <span data-ttu-id="ea892-200">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-200">Key</span></span>               | <span data-ttu-id="ea892-201">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-201">Value (type)</span></span> | <span data-ttu-id="ea892-202">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-202">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-203">sendfile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="ea892-203">sendfile.SendAsync</span></span> | <span data-ttu-id="ea892-204">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea892-204">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="ea892-205">Por solicitação</span><span class="sxs-lookup"><span data-stu-id="ea892-205">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="ea892-206">V0.3.0 opaco</span><span class="sxs-lookup"><span data-stu-id="ea892-206">Opaque v0.3.0</span></span>

| <span data-ttu-id="ea892-207">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-207">Key</span></span>               | <span data-ttu-id="ea892-208">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-208">Value (type)</span></span> | <span data-ttu-id="ea892-209">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-209">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-210">opaco. Versão</span><span class="sxs-lookup"><span data-stu-id="ea892-210">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="ea892-211">opaco. Atualizar</span><span class="sxs-lookup"><span data-stu-id="ea892-211">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="ea892-212">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea892-212">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="ea892-213">opaco. Fluxo</span><span class="sxs-lookup"><span data-stu-id="ea892-213">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="ea892-214">opaco. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="ea892-214">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="ea892-215">V0.3.0 WebSocket</span><span class="sxs-lookup"><span data-stu-id="ea892-215">WebSocket v0.3.0</span></span>

| <span data-ttu-id="ea892-216">Chave</span><span class="sxs-lookup"><span data-stu-id="ea892-216">Key</span></span>               | <span data-ttu-id="ea892-217">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="ea892-217">Value (type)</span></span> | <span data-ttu-id="ea892-218">Descrição</span><span class="sxs-lookup"><span data-stu-id="ea892-218">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea892-219">WebSocket. Versão</span><span class="sxs-lookup"><span data-stu-id="ea892-219">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="ea892-220">WebSocket. Aceitar</span><span class="sxs-lookup"><span data-stu-id="ea892-220">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="ea892-221">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea892-221">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="ea892-222">WebSocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="ea892-222">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="ea892-223">Não-spec</span><span class="sxs-lookup"><span data-stu-id="ea892-223">Non-spec</span></span> |
| <span data-ttu-id="ea892-224">WebSocket. Subprotocolo</span><span class="sxs-lookup"><span data-stu-id="ea892-224">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="ea892-225">Consulte [RFC6455 seção 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) etapa 5.5</span><span class="sxs-lookup"><span data-stu-id="ea892-225">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="ea892-226">WebSocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="ea892-226">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="ea892-227">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea892-227">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="ea892-228">WebSocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="ea892-228">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="ea892-229">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea892-229">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="ea892-230">WebSocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="ea892-230">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="ea892-231">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea892-231">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="ea892-232">WebSocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="ea892-232">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="ea892-233">WebSocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="ea892-233">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="ea892-234">Opcional</span><span class="sxs-lookup"><span data-stu-id="ea892-234">Optional</span></span> |
| <span data-ttu-id="ea892-235">WebSocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="ea892-235">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="ea892-236">Opcional</span><span class="sxs-lookup"><span data-stu-id="ea892-236">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="ea892-237">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ea892-237">Additional Resources</span></span>

* [<span data-ttu-id="ea892-238">Middleware</span><span class="sxs-lookup"><span data-stu-id="ea892-238">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="ea892-239">Servidores</span><span class="sxs-lookup"><span data-stu-id="ea892-239">Servers</span></span>](servers/index.md)