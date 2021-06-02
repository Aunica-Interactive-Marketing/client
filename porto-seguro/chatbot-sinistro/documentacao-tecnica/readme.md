![aunica](https://implementacaoaunica.github.io/client/aunica.jpg?raw=true)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Guia de estruturação HTML para tagueamento - Chatbot Sinistro

> Última atualização: 02/06/2021 <br />

<br />

## Sumário

- [Objetivo](#objetivo)
- [Descrição Código Google Tag Manager](#descri&ccedil;&atilde;o-c&oacute;digo-google-tag-manager)
- [Objeto customData](#objeto-customdata)
- [Estrutura HTML](#estrutura-html)
- [Atributo data-gtm-type](#atributo-data-gtm-type)
- [Atributo data-gtm-clicktype](#atributo-data-gtm-clicktype)
- [Atributo data-gtm-formtype](#atributo-data-gtm-formtype)
- [Atributo data-gtm-name](#atributo-data-gtm-name)
- [Atributo data-gtm-subname](#atributo-data-gtm-subname)
- [Eventos Padrões](#eventos-padr&otilde;es)
- [Exemplos práticos](#exemplos-pr&aacute;ticos)
- [Parametrização](#parametriza&ccedil;&atilde;o)



## Objetivo

<p style='text-align: justify;'>  
Neste documento são apresentados os requisitos necessários para que seja possível a implementação de uma estrutura otimizada e consistente de tagueamento no Chabot de Sinistro, diminuindo esforços com manutenção e evitando a perda de dados nas bases de Analytics.
</p>

<br />

### **Descrição Código Google Tag Manager**

<p style='text-align: justify;'>  
O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.
</p>

### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-xxxxxxx');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-xxxxxxx" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
  <!-- End Google Tag Manager (noscript) -->
  //...
  </body>
</html>
```

Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)

---

## Overview e Descrições Técnicas

### Camada de dados (DataLayer)

<p style='text-align: justify;'>  
É um array de objetos javascript utilizado pelo Google Tag Manager para receber em seus atributos, dados importantes do site.
Para implementar o dataLayer no site, o desenvolvedor pode utilizar formas diferentes para preencher os dados. Essas formas são dependentes da ação estabelecida na documentação e também do nível da interação.
</p>

**Instalação**<br />
Inserir a camada de dados antes do snippet de instalação do Google Tag Manager. Exemplo:


```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'atributo1': '[[valor1]]',
    'atributo2': '[[valor2]]'
  });
</script>
```

---


<br />

### Objeto customData

<p style='text-align: justify;'>  
O objeto customData é a fonte de alimentação de informações pertinentes ao disparo de tags da estrutura que foi desenvolvida. Trata-se de um objeto global que deve estar carregado e preenchido no momento do carregamento de todas as páginas do site, e caso haja a necessidade o seu conteúdo deve ser atualizado. Preferencialmente o customData deve ser carregado antes do GTM. Abaixo temos a indicação da estrutura do objeto e um descritivo de todos os seus atributos.
</p>

```html
<script>
customData = {
  site: {
    brand: '',
    versao: '',
    portal: ''
  },
  user: {
    id: '',
    id_usuario: '',
  },
  page: {
    cliente: ''.
    step: '',
    fluxo: ''
  }
}
</script>
```

| Atributo  | Descrição de preenchimento  | Tipo de dado | Valor padrão | Exemplo |
| :-------- | :-------------------------- | :----------- | :----------- | :------ |
| customData.site  | Objeto destinado a armazenar informações sobre a página/site que está sendo acessada. | Objeto | ""| |
| customData.site.brand  | Deve indicar qual a marca do site. | Texto | ""| “portoseguro”|
| customData.site.versao  | Deve indicar qual variação/versão do site o usuário está acessando. | Texto | ""| “1.2”|
| customData.site.portal  | Deve indicar qual o tipo de produto. | Texto | ""| “PPW corretor online”|
| customData.user  | Objeto destinado a descrever as informações do usuário. O objeto deve ser trazido quando o usuário estiver identificado.| Objeto | ""| |
| customData.user.id  | Deve indicar o ID de usuário que a plataforma atribui. | Texto | ""| "ABC123"|


<br />


### Estrutura HTML

<p style='text-align: justify;'>  
Foi desenvolvida uma categorização dos elementos HTML para que quando houverem interações do usuário na página, possamos identificar exatamente onde e qual foi a interação. Esse processo tem o objetivo de possibilitar que a captura de informações ocorra de forma automatizada, diminuindo o esforço necessário na implementação do mapeamento de chamadas.
Portanto, o primeiro requisito para a estrutura de tagueamento automático funcionar é o preenchimento do atributo data-gtm-type para que seja feita a categorização dos elementos da página e partir disso definir o comportamento de cada um para o disparo de tags. 
São 5 tipos de elementos, sendo que 3 possuem sub-categorias devido suas características.
É necessária a implementação desses itens em quaisquer elementos que possuam a necessidade de serem mensurados, pois baseado no preenchimento dos atributos data que as interações serão mapeadas pelo script.
</p>

* Check
* Click
   - filtro
   - accordion
   - menu
   - sub-menu
   - rodape
   - button
   - download
   - link
   - card
* Form
   - input
   - submit
* Select

---

> Todos os elementos do html que serão clicados, deverão ser mapeados recebendo os atributos com sua estrutura no item.

```html
<div  
  data-gtm-type= "click, form, select"
  data-gtm-clicktype= "accordion, filtro, button, card, download, link, menu, sub-menu, rodape"
  data-gtm-formtype= "input, submit"
  data-gtm-name= "nome-acao"
  data-gtm-subname= "sub-nome-acao"
 >
</div>
```

#### Atributo data-gtm-type

<p style='text-align: justify;'>  
Os valores click, form e select deverão ser preenchidos no atributo data-gtm-type.
</p>

#### Atributo data-gtm-clicktype

<p style='text-align: justify;'>
Os valores accordion, filtro, button, card, download, link, menu, sub-menu e rodape da categoria de elementos do tipo click deverão ser preenchidos no atributo data-gtm-clicktype.
</p>


#### Atributo data-gtm-name

<p style='text-align: justify;'>
Além disso, é necessário o preenchimento do atributo data-gtm-name para que possamos identificar em qual elemento ocorreu a interação. O atributo deve carregar um nome descritivo que representa a ação do botão, o nome do card, o nome do form, o nome do campo, etc.
</p>

#### Atributo data-gtm-subname

<p style='text-align: justify;'>
Caso exista a necessidade de especificar ainda mais o elemento que foi clicado é necessário indicar um no preenchimento do atributo data-gtm-subname para que possamos identificar em qual elemento ocorreu a interação.
</p>

---

### Eventos Padrões

#### Visualizações de página

<p style='text-align: justify;'>Em aplicações Ajax, Angular, React, Vue e demais onde a página não é recarregada durante o passo a passo, há a necessidade de um push no objeto dataLayer com as seguintes informações preenchidas:</p>

```html
<script>
dataLayer = window.dataLayer || [];
dataLayer.push({
  event: 'step_change',
      etapa: '',
      titulo ''
})
</script>
```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| etapa     | Deve retornar a etapa correspondente | "/localizar-documentos" e etc |
| titulo     | Deve retornar o nome da pagina | "proposta", "orcamento" e etc |

<br />


#### Solicitação de serviços

<p style='text-align: justify;'>Chamada criada para ser disparada sempre que algum dos serviços for solicitado pelo usuário.</p>

```html
<script>
dataLayer.push({
    event: 'solicitacao_servico',
    nome_servico: '',
    tipo_servico: '',
    retorno: '',
    descricao: '',
    erro: {
        codigo: '',
        servico: ''
    }
});
</script>


```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| nome_servico | Deve indicar o nome do serviço que será realizado | "data-futura", "data-futura" |
| tipo_servico | Deve indicar qual o tipo de serviço que está sendo solicitado | "validar-data" e etc |
| retorno      | Deve indicar o sucesso ou erro da tentativa da solicitação de serviço | "sucesso" ou "erro" |
| descricao    | Deve trazer a descrição do retorno | "data-futura", "formato-invalido",  "data-correta", "hora-futura" e etc |
| codigo       | Deve trazer o código do erro | "124 e etc |
| servico      | Deve trazer qual serviço foi acionado  | "cobranca" e etc |
| mensagem    | Deve trazer a descrição do erro | "dados-invalidos" e etc |

<br />

#### Geração de proposta - Transação

<p style='text-align: justify;'>É necessário a implementação de um dataLayer.push com o objetivo de mensurar o número de serviços contratados. Importante que o método descrito seja disparado no retorno da solicitação:</p>

```html
<script>
dataLayer = window.dataLayer || [];
dataLayer.push({
    event: 'transmissao',
    doc: '',
    origem: '',
    numero_proposta: '',
    cartao_porto: '',
    versao_proposta: '',
    paymentMethod: '',
    id_oferta: '',
    retorno:  '',
    descricao: '',
    ecommerce: {
        purchase: {
            actionField: {
                id: '',             //id da transação
                revenue: 0.00,
            },
            products: [{
              id: '',
              name: '',
              brand: '',
              valor_premio,
              valor_franquia,
            }]
        }
    },
    erro: {
        codigo: '',
        servico: ''
    }
});
</script>



```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| doc          | Deve retornar o numero da doc | "56565656" e etc |
| origem          | Deve retornar a origem  | "auto 2.0" ou "antigo""|
| numero_proposta         | Deve retornar o numero da proposta| "35656456" e etc|
| cartao_proposta      | Deve retornar o cartao | "visa", "mastercard", "nao-contratado" |
| versao_proposta      | numero da versao da proposta | "5444646" e etc |
| paymentMethod      |  "metodo de pagamento" | "cartao-de-credito" e etc |
| id_oferta      | Deve retornar o id da oferta | "454646" e etc |
| retorno      | Deve indicar o sucesso ou erro da tentativa da solicitação de serviço | "sucesso" ou "erro" |
| descricao    | Deve trazer a descrição do retorno | "proposta-realizada-com-sucesso" e etc |
| ecommerce.purchase.actionField.id  |  TS+Protocolo | "dd/mm/yyyy-hh:mm:ss:protocolo de transmição" |
| ecommerce.purchase.actionField.revenue  | Elemento deve informar o valor total da transação | "236.00" e etc |
| ID da oferta  | Retornar o id da oferta | “321” e etc |
| ecommerce.purchase.products[x].name  | Deve trazer o nome do plano | “plano123" e etc |
| ecommerce.purchase.products[x].brand  | Marca do plano | “porto seguro" e etc |
| ecommerce.purchase.products[x].valor_premio  | Deve trazer o valor do premio | “0.00" e etc |
| ecommerce.purchase.products[x].valor_franquia  | Valor total da franquia | “0.00" e etc |
| codigo       | Deve trazer o código do erro | "124 e etc |
| servico      | Deve trazer qual serviço foi acionado  | "cobranca" e etc |
| mensagem    | Deve trazer a descrição do erro | "dados-invalidos" e etc |

<br />


#### Resultado consulta


<p style='text-align: justify;'>Chamada criada para ser disparada sempre que for resultado de alguma busca.</p>

```html
<script>
dataLayer.push({
    event: 'resultado_consulta',
    nome_servico: '',
    tipo_busca: '',
    susep: '',
    cliente: '',
    data_calculo: '', 
    data_inicio_vigencia: '',
    placa: '',
    num_item: '',
    sucursal: '',
    apolice: '',
    cancelamento: '',
    tipo_seguro: '',
    situacao: '',
    status_expiracao: '',
    retorno: '',
    descricao: '',
    erro: {
        codigo: '',
        servico: ''
    }
});
</script>


```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| nome_servico | Deve indicar o nome do serviço que será realizado | "localizar-documentos" e etc|
| tipo_busca | Deve retornar o tipo de busca | "inclusao-de-item", "substituicao-de-veiculo-ou-alteracao-apolice" e etc |
| susep        | Deve indicar o susep preenchida nos formularios | "CJO1" e etc |
| cliente      | Deve retornar o nome do cliente | "segurado", "novo-cliente" e etc |
| data_calculo          | Deve retornar a data do calculo | "22/04/21" e etc |
| data_inicio_vigencia          | Deve retornar a data de vigência | "28/04/21" e etc|
| placa          | "placa criptografada"| "jsdosodkoskd6565"|
| num_item      | Deve retornar o nome numero do item | "5454545" e etc |
| sucursal      | "Deve retornar a sucursal" | "sucursal" e etc |
| apolice      | Deve retornar o numero da apolice| "4545454545" e etc |
| cancelamento      | Deve retornar o cancelamento| "apolice" ou "item" ou "apolice-item"  |
| tipo_seguro      | Deve trazer o tipo de seguro selecionado no campo| "n-cia", "n-documento" e etc|
| situacao      | Deve retornar a situação | "calculado, "pendente", "recusado" e etc  |
| status_expiracao      | Deve retornar o status da expiração | "prazo-expirado", "tarifa-expirada" e etc |
| retorno      | Deve indicar o sucesso ou erro da tentativa da solicitação de serviço | "sucesso" ou "erro" |
| descricao    | Deve trazer a descrição do retorno | "proposta-realizada-com-sucesso" e etc |
| codigo       | Deve trazer o código do erro | "124 e etc |
| servico      | Deve trazer qual serviço foi acionado  | "cobranca" e etc |
| mensagem    | Deve trazer a descrição do erro | "dados-invalidos" e etc |

###


#### Erros

<p style='text-align: justify;'>Apesar de termos a indicação de erro nos eventos que representam as KPI’s, precisamos mapear todos os retornos de erro do sistema que são exibidos para o usuário. Por isso, caso o erro exibido não corresponda ao retorno dos eventos listados acima, é necessário a implementação do dataLayer.push descrito abaixo:</p>

```html
<script>
dataLayer.push({
      event:'erro',
      codigo: '',
      servico: '',
      mensagem: ''
});
</script>
```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| codigo  | Deve trazer o código do erro | "124 e etc |
| servico  | Deve trazer qual serviço foi acionado  | "cobranca" e etc |
| mensagem  | Deve trazer a descrição do erro | "dados-invalidos" e etc |

<br />

#### Evento para lista de ofertas

É necessário a implementação de um dataLayer.push com o objetivo de mensurar compra após a exibição dos produtos. Importante que o método descrito seja disparado após os produtos serem exibidos ao usuário

```html
<script>
dataLayer.push({
  event: 'lista_ofertas',
  produto: [{
    id: '',
    name: '',
    brand: '',
    list: '',
    valor_premio,
    valor_franquia,
    position: '',
   },
   {
    id: '',
    name: '',
    brand: '',
    list: '',
    valor_premio,
    valor_franquia,
    position: '',
  }]
});
</script>
```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| id  | Deve trazer o código SKU do produto | "123" e etc |
| name  | Deve trazer o nome do plano  | "plano123" e etc |
| brand  | Deve trazer marca do plano | “azul”,”itau”,”porto-seguro” |
| list  | Deve trazer a qual lista a oferta pertenc| "ofertas-principais" e etc |
| valor_premio  | Valor total do prêmio| "0.00" e etc |
| valor_franquia  | Valor total da franquia | "0.00" e etc |
| position  | Deve trazer a posição da oferta | "1" e etc |

<br />

---

### Exemplos práticos

### Menu

![Menu](https://aunica-interactive-marketing.github.io/client/prints/1.png?raw=true)


- **Ao escolher uma opção no menu**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "menu:[[opcao-selecionada]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[opcao-selecionada]]  | Deve retornar a opção selecionada | "1-avisar-um-sinistro", "2-acompanhar-um-sinistro" e etc |


<br />

### Data ocorrência - Avisar sinistro

![Data](https://aunica-interactive-marketing.github.io/client/prints/data-ocorrencia.png?raw=true)


- **No preenchimento do campo "data ocorrência"**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "data-ocorrencia"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```
<br />

![Data](https://aunica-interactive-marketing.github.io/client/prints/formato-invalido.png?raw=true)


- **Ao digitar sair**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "data-ocorrencia:formato-invalido:sair"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

- **Ao preencher o campo de data na mensagem de formnato inválido**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "formato-invalido:data-ocorrencia"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

### Data Futura - Avisar sinistro

![Data Futura](https://aunica-interactive-marketing.github.io/client/prints/data-futura.png?raw=true)


- **No preenchimento do campo de data futura**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "data-futura"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

- **Ao digitar sair**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "data-futura:sair"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

### Você ainda esta digitando data futura

![Hora](https://aunica-interactive-marketing.github.io/client/prints/voce-ainda-esta-digitando.png?raw=true)

- **No preenchimento do campo de data futura**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "ainda-esta-digitando-data-futura"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

- **Ao digitar sair**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "ainda-esta-digitando-data-futura:sair"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

### Horario ocorrência - Avisar sinistro

![Data](https://aunica-interactive-marketing.github.io/client/prints/horario-ocorrencia.png?raw=true)


- **No preenchimento do campo "horario ocorrência"**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "horario-ocorrencia"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```
<br />


- **Ao digitar sair**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "horario-ocorrencia:formato-invalido:sair"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

- **Ao preencher o campo de data na mensagem de formnato inválido**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "formato-invalido:horario-ocorrencia"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />


### Hora Futura - Avisar sinistro

![Hora Futura](https://aunica-interactive-marketing.github.io/client/prints/hora-futura.png?raw=true)


- **No preenchimento do campo de data futura**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "hora-futura"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

- **Ao digitar sair**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "hora-futura:sair"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

### Você ainda esta digitando hora futura

![Hora](https://aunica-interactive-marketing.github.io/client/prints/ainda-esta-digitando-hora.png?raw=true)

- **No preenchimento do campo de data futura**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "ainda-esta-digitando-hora-futura"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />

- **Ao digitar sair**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div 
  data-gtm-type="text"
  data-gtm-name= "ainda-esta-digitando-hora-futura:sair"
  data-gtm-subname= "avisar-sinistro"
 >
</div>
```

<br />



> Em caso de dúvidas, estamos a disposição.

<br />

<script> document.querySelector('h1').style.display = 'none' </script>

