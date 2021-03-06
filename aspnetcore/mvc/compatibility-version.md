---
title: Versão de compatibilidade do ASP.NET Core MVC
author: rick-anderson
description: Saiba como a classe Startup no ASP.NET Core configura serviços e o pipeline de solicitação do aplicativo.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/20/2018
uid: mvc/compatibility-version
ms.openlocfilehash: bedeeba07dcca19176e779f3541c445e94efcd07
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910099"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Versão de compatibilidade do ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

O método <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> permite que um aplicativo aceite ou recuse as possíveis alterações da falha de comportamento introduzidas no ASP.NET Core MVC 2.1 ou posteriores. Essas possíveis alterações da falha de comportamento geralmente afetam como o subsistema MVC se comporta e como **o código do usuário** é chamado pelo tempo de execução. Ao aceitar, você obtém o comportamento mais recente e o comportamento de longo prazo do ASP.NET Core.

O código a seguir define o modo de compatibilidade para o ASP.NET Core 2.1:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

É recomendável que você teste seu aplicativo usando a versão mais recente (`CompatibilityVersion.Version_2_1`). Estimamos que a maioria dos aplicativos não terão alterações da falha de comportamento usando a versão mais recente.

Os aplicativos que chamam `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` são protegidos contra as possíveis alterações da falha de comportamento apresentadas no ASP.NET Core 2.1 MVC e nas versões 2.x posteriores. Essa proteção:

* Não se aplica a todas as alterações da 2.1 e posteriores, ela é direcionada às possíveis alterações da falha de comportamento do tempo de execução do ASP.NET Core no subsistema de MVC.
* Não se estende à próxima versão principal.

A compatibilidade padrão para aplicativos ASP.NET Core 2.1 e 2.x posteriores que **não** é chamada é a `SetCompatibilityVersion` compatibilidade 2.0. Ou seja, não chamar `SetCompatibilityVersion` é o mesmo que chamar `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

O código a seguir define o modo de compatibilidade para o ASP.NET Core 2.1, exceto para os seguintes comportamentos:

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Para aplicativos que encontram alterações da falha de comportamento, o uso das opções de compatibilidade apropriadas:

* Permite que você use a versão mais recente e recuse alterações da falha de comportamento específicas.
* Tenha tempo para atualizar seu aplicativo para que ele funcione com as alterações mais recentes.

Os comentários do código-fonte da classe [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) têm uma boa explicação sobre o que mudou e por que as alterações são uma melhoria para a maioria dos usuários.

Futuramente, haverá uma [versão 3.0 do ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap). Os comportamentos antigos compatíveis por meio de opções de compatibilidade serão removidos na versão 3.0. Consideramos que essas são alterações positivas que beneficiam quase todos os usuários. Com a introdução dessas alterações, a maioria dos aplicativos poderá se beneficiar agora e os demais terão tempo para serem atualizados.
