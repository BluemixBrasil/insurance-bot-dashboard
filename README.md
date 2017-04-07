# Cloud Insurance Co. - Painel do Administrador

<!-- No tests are set up currently
| **master** | [![Build Status](https://travis-ci.org/IBM-Bluemix/insurance-bot-dashboard.svg?branch=master)](https://travis-ci.org/IBM-Bluemix/insurance-bot-dashboard) |
| ----- | ----- |
| **dev** | [![Build Status](https://travis-ci.org/IBM-Bluemix/insurance-bot-dashboard.svg?branch=dev)](https://travis-ci.org/IBM-Bluemix/insurance-bot-dashboard) |
 -->

Esse repositório é parte do projeto maior [Cloud Insurance Co.](https://github.com/IBM-Bluemix/cloudco-insurance).

# Visão Geral

O painel de administrador provê aos administradores do [Cloud Insurance Co.](https://github.com/IBM-Bluemix/cloudco-insurance) uma visão geral das atividades em progresso no website. Começando por uma view em tempo real das conversas com o chatbot provendo aos admins visões sobre as interações entre o bot e os visitantes.

Esse projeto foi criado como diversas novas tecnologias front-end incríveis, tudo em cima de um sistema de compilação webpack rico em recusos o qual já foi configurado para prover resultados rápidos às alterações no código, módulos CSS com suporte a Sass, teste de unidade, relatórios de cobertura de código, separação de pacote e muito mais, enquanto provê ferramentas incríveis de desenvolvedor como Redux CLI (um gerador), Redux devtools (extenção do Chrome), e um Storybook para desenvolver de forma visual visual e testar os componentes.

Para enviar todo o conjunto de microsserviços envolvidos confira o [repositório da toolchain do seguro][toolchain_url].
Ou se preferir, pode enviar apenas a aplicação seguindo os passos abaixo

## Requisitos da aplicação
* node `^6.7.0`
* npm `^3.10.10`

## Rodando a aplicação no Bluemix

1. Caso não tenha uma conta no Bluemix [inscreva-se][bluemix_reg_url]

1. Baixe e Instale a ferramenta [Cloud Foundry CLI][cloud_foundry_url].

1. O app depende dos microsserviços de [Catálogo](https://github.com/IBM-Bluemix/insurance-catalog) e [Demandas](https://github.com/IBM-Bluemix/insurance-orders). Certifique-se de enviá-los ao Bluemix primeiro.

1. Clone a aplicação para seu ambiente de trabalho pelo terminal de comandos usando o seguinte comando:

  ```
  git clone https://github.com/IBM-Bluemix/insurance-bot.git
  ```

1. `cd` seu-diretório

1. Abra o arquivo `manifest.yml` e mude o valor `host` para um nome único.

  Seu host criado definirá o subdomínio da URL de sua aplicação:  `<host>.mybluemix.net`

1. Conecte a aplicação no terminal de comandos e siga os comandos a seguir para logar:

  ```
  cf login -a https://api.ng.bluemix.net
  ```

1.Crie um serviço de Tone Analyzer do Watson.

  ```
  cf create-service tone_analyzer standard insurance-tone_analyzer
  ```

1. Construa a página do app

  ```
  npm install
  npm run deploy:prod
  ```

1. Publique o app no Bluemix

  ```
  cf push --no-start
  ```

1. Defina uma variável apontando para o envio da página inicial.

  ```
  cf set-env insurance-bot-dashboard SOCKET_URL https://your-insurance-bot.mybluemix.net
  ```

1. Execute seu app.

  ```
  cf start insurance-bot-dashboard
  ```

  E voila! Você tem sua própria versão do app rodando no Bluemix.

## Executando o app em host local

1. Caso não tenha uma conta no Bluemix [inscreva-se][bluemix_reg_url]

1. Baixe e Instale a ferramenta [Cloud Foundry CLI][cloud_foundry_url].

1. O app dependo do [aplicativo da página inicial](https://github.com/IBM-Bluemix/insurance-bot). Certifique-se de enviá-los ao Bluemix primeiro.

1. Clone a aplicação para seu ambiente de trabalho pelo terminal de comandos usando o seguinte comando:

  ```
  git clone https://github.com/IBM-Bluemix/insurance-bot.git
  ```

1. `cd` insira-seu-diretório

1. Crie um novo serviço do Watson Tone Analyzer chamado `insurance-tone_analyzer` usando sua conta Bluemix

1. Substitua as credenciais correspondentes pelos serviços `insurance-tone_analyzer` e `insurance-bot-db` em seu arquivo `vcap-local.json` usando `vcap-local.template.json` como template.

1. Defina uma variável de ambiente apontando para a página inicial (que pode ser executada em local host ou no Bluemix)

  ```
  export SOCKET_URL=https://localhost:6040
  ```

1. Instale os pacotes npm com o comando a seguir

  ```
  npm install
  ```

1. Execute seu app em local com o comando a seguir

  ```
  npm start
  ```

Esse comando dará início a seu servidor node.js e irá retornar o endereço do port no console: `server starting on http://localhost:3000`.

<img src="http://i.imgur.com/zR7VRG6.png?2" />

## Desenvolvimento

### Resolução da raíz
O Webpack está configurado para o uso do [resolve.root](http://webpack.github.io/docs/configuration.html#resolve-root), que permite a importação de pacotes locais como se você estivesse atravessando da raíz de seu diretório `~/src` como no exemplo a seguir:

```js
// current file: ~/src/views/some/nested/View.js
// What used to be this:
import SomeComponent from '../../../components/SomeComponent'

// Can now be this, HORRAY!:
import SomeComponent from 'components/SomeComponent'
```

### Variáveis Globais

Essas são as variáveis globais disponíveis para uso em qualquer parte do seu código. Caso deseje modificá-las, elas podem ser encontradas com a chave `globals` no arquivo `~/config/index.js`. Ao adicionar novas variáveis globais, certifique-se de adicioná-las a `~/.eslintrc` também.

|Variable|Description|
|---|---|
|`process.env.NODE_ENV`|the active `NODE_ENV` when the build started|
|`__DEV__`|True when `process.env.NODE_ENV` is `development`|
|`__PROD__`|True when `process.env.NODE_ENV` is `production`|
|`__TEST__`|True when `process.env.NODE_ENV` is `test`|
|`__DEBUG__`|True when `process.env.NODE_ENV` is `development` and cli arg `--no_debug` is not set (`npm run dev:no-debug`)|
|`__BASENAME__`|[history basename option](https://github.com/rackt/history/blob/master/docs/BasenameSupport.md)|
|`__SOCKET_URL__`|The websocket endpoint for the main website. It is initialized from `process.env.SOCKET_URL` variable specified as *https://host:port*.|

## Licença

Veja o [arquivo de licença](License.txt) para informação sobre a licença.

# Notificação de privacidade

Essa aplicação é configurada para rastrear seus envios para o [IBM Bluemix](http://www.ibm.com/cloud-computing/bluemix/) e outras plataformas cloud foundry. A informação a seguir é enviada para um [Rastreador de envio](https://github.com/IBM-Bluemix/cf-deployment-tracker-service) a cada envio:

* Versão do pacote Node.js
* URL do repositório Node.js
* Nome do app (`application_name`)
* ID do espaço (`space_id`)
* Versão do app (`application_version`)
* URIs do app (`application_uris`)
* Rótulos de serviços associados
* Número de instância para cada serviço associado e informações do plano associadas

Os dados coletados do arquivo `package.json` na aplicação e as variáveis do ambiente `VCAP_APPLICATION` e `VCAP_SERVICES` no Bluemix e outras plataformas Cloud Foundry. Esses dados são utilizados pela IBM para traçar medidos ao redor dos envios de aplicações modelo para o Bluemix para medir a utilidade dos exemplos, para que possamos sempre melhor o conteúdo que lhe oferecemos. Somente envios de aplicações modelo incluem código que importa o rastreador de envio serão rastreados.

## Desabilitando o rastreador de envio

O rastreador de envio pode ser removido deletando a linha `require("cf-deployment-tracker-client").track();` do início do arquivo `tracker.js`.

[bluemix_reg_url]: http://ibm.biz/insurance-store-registration
[cloud_foundry_url]: https://github.com/cloudfoundry/cli
