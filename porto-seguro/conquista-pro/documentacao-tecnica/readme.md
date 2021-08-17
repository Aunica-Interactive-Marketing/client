![aunica](https://implementacaoaunica.github.io/client/aunica.jpg?raw=true)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Guia de estruturação HTML para tagueamento - PPW Corretor Online Unificado

> Última atualização: 23/04/2021 <br />

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
Neste documento são apresentados os requisitos necessários para que seja possível a implementação de uma estrutura otimizada e consistente de tagueamento no ambiente Corretor Online, diminuindo esforços com manutenção e evitando a perda de dados nas bases de Analytics.
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
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-PV5P22J');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-PV5P22J" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
    tipo_usuario: '',
    saldo: '',
    susep: '',
    logado: '',
    origem: '',
  },
  page: {
    cliente: ''.
    step: '',
    fluxo: ''
  },
  cotacao: {
    id_cotacao: '',
    versao_cotacao: '',
    seguro_corretor: '',
    coberturas: ['RCFV - Danos Materiais', 'RCFV - Danos Corporais'],
    coberturas_acessorio: [{
      tipo_de_cobertura: 'equipamento',
      tipo_de_acessorio: '', //nome do equipamento
    },{
      tipo_de_cobertura: 'acessorios',
      tipo_de_acessorio: '', //nome do acessorio
    }],
    vidros: 'tipo_de_atendimento:<<referenciada ou livre-escolha>>',
    veiculo: '',
    ano_veiculo: '',
    tipo_de_uso: '',
    cep: '',
    id_cliente: '',
    assitencia: '',
    carro_reserva: '',
    despesa_extraodinaria: '',
    franquias: '',
    demais_assistencias: [''],
    programa_de_relacionamento: [''],
    vantagens_para_voce: [''],
    quantidade_condutores_adc: '',
    avaliacao_risco: [''],
    bonus_classe: '',
    bonus_origem: ''
    seguro_residencial:{
      tipo_moradia: '',
      id_oferta: '',
      valor_premio: ''
    },
  }
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
| customData.user.id_usuario  | CPF Criptografado  AESCryptography (Utilizada nos ambientes porto e azul) | Texto | ""| “a750c220a060fcf487f9519d3203035b”|
| customData.user.tipo_usuario  | Deve indicar a classificação do usuário em sua conta PortoSeguro | Texto| ""| “corretor”, “prestador-de-servico”,“cliente"|
| customData.user.saldo  | Deve indicar o saldo total disponível do usuário | Texto | ""| “31.257”, “12.345”|
| customData.user.susep  | Deve indicar o ID da Susep (quando se aplica). | Texto | ""| "COL10J"|
| customData.user.logado  | Deve indicar se o usuário está já realizou o Login | Boolean | ""| "true", "false"|
| customData.user.origem  | Deve indicar de onde veio o usuário | Texto | ""| "auto 2.0" ou "antigo"|
| customData.user.cliente  | Deve indicar o tipo de cliente | Texto | ""| "novo-cliente", "segurado"|
| customData.user.step  | Deve indicar o nome do step no fluxo | Texto | ""| "pagamento", "cadastro", "veiculo" e etc |
| customData.user.fluxo  | Deve indicar os fluxos macros | Texto | ""| "orcamento", "proposta" e etc |
| customData.cotacao.id_cotacao  |  Deve retornar o id da cotação| Texto | "" | "956565656"|
| customData.cotacao.versao_cotacao  | Trazer a versão da cotação | Texto  | ""| "versao1" |
| customData.cotacao.seguro_corretor  |  Trazer se é seguro do corretor| Texto | "" | “sim”,”nao”|
| customData.coberturas  | Trazer as coberturas selecionadas | Array | []| 'RCFV - Danos Materiais', 'RCFV - Danos Corporais'|
| customData.coberturas.coberturas_acessorio | Array de objetos destinado a guardar informações  sobre a cobertura de acessórios|Array de Objeto |[{}]| -|
| customData.coberturas.tipo_de_cobertura   | Trazer o tipo cobertura selecionada  | Texto| ""| “equipamentos”, “acessorios”|
| customData.coberturas.tipo_de_acessorio  | Trazer o tipo de acessório selecionado | Texto| ""| -|
| customData.vidros  | Trazer o tipo de atendimento selecionado na cobertura de vidros | Texto | "" | “Tipo_de_atendimento:livre-escolha”, “tipo_de_atendimento:referenciada”|
| customData.veiculo  | Deve trazer o veículo a ser cotado | Texto | ""| “Renault Sandero RS 2.0. Flex”|
| customData.ano_veiculo  | Deve trazer o ano do veículo a ser cotado | Texto| "" | "2020"|
| customData.tipo_de_uso  | Deve trazer o tipo de uso do veículo a ser cotado | Texto | "" |“particular” |
| customData.cep  | Retornar o cep | Texto | ""|"051452-001" |
| customData.id_cliente  | CPF do cliente criptografado utilizando o mesmo padrão de outros ambientes, (cpf preenchido no formulario) | Texto |"" | "aishdu6265"|
| customData.assitencia  | Trazer as coberturas de assistência selecionadas |  Texto| ""| - |
| customData.carro_reserva  | Trazer o carro reserva selecionado |  Texto |"" |“carro-reserva-porte-basico-15-dias” |
| customData.despesa_extraodinaria  | Trazer as coberturas de despesas selecionadas  | Texto | "" | -|
| customData.franquias  | Trazer as coberturas de franquia selecionadas  | Texto | ""|  -|
| customData.demais_assistencias  | Trazer as coberturas de demais assistências selecionadas | Array | []| -|
| customData.programa_de_relacionamento  | Trazer as coberturas de programas de relacionamento selecionadas | Array | []| -|
| customData.vantagens_para_voce  | Trazer as coberturas de vantagens para você selecionadas |  Array | []| -|
| customData.avaliacao_risco  |  Trazer os valores selecionado na avaliação de risco| Array | [] |[“transporte-da-porto-seguro”,”outros”] |
| customData.bonus_classe  | Trazer a classe de bônus selecionada |  Texto| ""| -|
| customData.bonus_origem  | Trazer a origem do bônus selecionado | Texto| ""| -|
| customData.seguro_residencial  | Objeto destinado a armazenar os dados do seguro residencial | Objeto | {}| - |
| customData.tipo_moradia  | Deve trazer o tipo de moradia selecionada (trazer quando pertinente) | Texto | ""| - |
| customData.id_oferta  |  Deve trazer o ID da oferta (trazer quando pertinente)| Texto| "" | "s84554544"|
| customData.valor_premio  | Deve trazer o valor do prêmio do seguro residencial (trazer quando pertinente) | Texto | ""| "125.62"|


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

#### Atributo data-gtm-formtype

<p style='text-align: justify;'>
Os valores input e submit do tipo form deverão ser preenchidos no atributo data-gtm-form.
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

#### Visualização de modal

<p style='text-align: justify;'>Em aplicações onde existe a interação dentro modais na página, há a necessidade de um push no objeto dataLayer para sabermos o momento exato em que o usuário trocou de passo:</p>

```html
<script>
dataLayer = window.dataLayer || [];
dataLayer.push({
  event: 'modal',
  nome: '',
  action: ''
})
</script>
```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| nome      | Deve retornar o nome do modal | "atencao" e etc |
| action      | Deve retornar a ação do usuário com o modal | "open" ou "close" |

<br />


#### Solicitação de serviços

<p style='text-align: justify;'>Chamada criada para ser disparada sempre que algum dos serviços for solicitado pelo usuário.</p>

```html
<script>
dataLayer.push({
    event: 'solicitacao_servico',
    nome_servico: '',
    tipo_servico: '',
    cliente: '',
    susep: '',
    doc: '',
    tipo_pessoa: '',
    tipo_uso: '',
    modelo_veiculo: '',
    ano_fabricacao: '',
    paises: '',
    cartao_porto: '',
    id_oferta: '',
    valor_premio,
    valor_franquia,
    plano: '',
    marca: '',
    seguro_coletivo: '',
    worksite: '',
    lmi: '',
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
| nome_servico | Deve indicar o nome do serviço que será realizado | "proposta", "orcamento" e etc |
| tipo_servico | Deve indicar qual o tipo de serviço que está sendo solicitado | "salvar-orcamento" e etc |
| susep        | Deve indicar o susep preenchida nos formularios | "CJO1" e etc |
| cliente      | Deve retornar o nome do cliente | "segurado", "novo-cliente" e etc |
| doc          | Deve retornar o numero da doc | "56565656" e etc |
| tipo_pessoa          | Deve retornar o tipo da pessoa | "fisica" ou "juridica"|
| tipo_uso          | Deve retornar o tipo de uso com Veiculo| "particular", "taxi" e etc|
| modelo_veiculo      | Deve retornar o nome do veiculo | "novo-focus-hath" e etc |
| ano_fabricacao      | Deve retornar o ano de fabricação | "2015" e etc |
| paises      | Paises selecionados (separados por ,)| "argentina", "paraguai" e etc |
| cartao_porto      | Retornar true ou false| "true", "false"  |
| id_oferta      | Deve trazer o id da oferta (trazer quando pertinente)| "6465656jjj" e etc|
| valor_premio      | Deve trazer o valor do prêmio (trazer quando pertinente) | "196.98" e etc  |
| valor_franquia      | Deve trazer o valor da franquia (trazer quando pertinente) | "170.98" e etc |
| plano      | Deve trazer o plano selecionado (trazer quando pertinente) | “coberturas-01-auto-premium-casa” |
| marca      | Deve trazer a marca do plano selecionado (trazer quando pertinente)| “azul”,”itau”,”porto-seguro” |
| seguro_coletivo      | Deve retornar se possui seguro coletivo |  "sim" ou "nao"|
| worksite      | Deve retornar sim ou não| "sim" ou "nao" |
| lmi          | Deve retornar o valor do lmi | "48.773,00" e etc |
| retorno      | Deve indicar o sucesso ou erro da tentativa da solicitação de serviço | "sucesso" ou "erro" |
| descricao    | Deve trazer a descrição do retorno | "proposta-realizada-com-sucesso" e etc |
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

### Localizar Documentos:

![Localizar-doc](https://implementacaoaunica.github.io/client/prints/localizar-doc.png?raw=true)


- **Em Localizar Documentos**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "localizar-documentos"
 >
</form>
```

<br />

- **Ao clicar no botão "Buscar" em Localizar Documentos**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input 
  data-gtm-type= "submit"
  data-gtm-name= "buscar"
  data-gtm-subname= "localizar-documentos"
 >
</input>
```

<br />

- **Na interação com os itens do formulário em Localizar Documentos**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "numero", "nome-do-proponente" e etc |


<br />

- **No clique dos links em Localizar Documentos**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="link"
  data-gtm-name="1500-protocolos-atualizados"
  data-gtm-subname="localizar-documentos"
 >
</div>
```


<br />

- **Na interação com os campos para selecionar uma opção em Localizar Documentos**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[campo-selecionado]]"
  data-gtm-subname="localizar-documentos"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo-selecionado]]  | Deve retornar o nome do campo que selecionou alguma opção | "tipo:n-cia", "tipo:endosso", "situacao:calculado" e etc |



<br />

- Resultado Busca

![Resultado-busca](https://implementacaoaunica.github.io/client/prints/resultado-busca.png?raw=true)

- **No clique dos botões**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-button]]"
  data-gtm-subname="resultado-busca"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-button]]  | Deve retornar o nome do botão clicado | "orcamento", "proposta", "historico" e etc |



<br />

- **Na interação para abrir ou fechar algum resultado de busca**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="link"
  data-gtm-name="[[acao]]"
  data-gtm-subname="resultado-busca"
  data-gtm-doc="[[doc]]"

 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[acao]]  | Deve retornar a ação do usuário | "abrir" ou "fechar" |
| [[doc]]  | Deve retornar o numero do documento que se teve a ação | "95863254" e etc |



<br />

- **Ao selecionar algum resultado de busca**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="resultado-busca"
  data-gtm-doc="[[doc]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[doc]]  | Deve retornar o numero do documento que se teve a ação | "95863254" e etc |



<br />

- **No clique do botão do chat do resultado de busca**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="chat"
  data-gtm-subname="resultado-busca"
  data-gtm-doc="[[doc]]"
 >
</div>
```



| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[doc]]  | Deve retornar o numero do documento que se teve a ação | "95863254" e etc |



<br />

---

### Orçamento

- Orçamento

![Orcamento](https://implementacaoaunica.github.io/client/prints/orcamento.png?raw=true)

- **No clique dos botões ou links de cada etapa do formulário**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="[[button ou link]]"
  data-gtm-name="[[nome-button ou link]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[button ou link]]  | Deve retornar o tipo de elemento clicado | "button", "link" |
| [[nome-button ou link]]  | Deve retornar o nome do botão ou link clicado | "salvar", "continuar", "distribuicao-de-corretagem", "pesquisar-cep" e etc |



<br />

- **Ao selecionar alguma opção no formulario em cada etapa**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[nome-filtro]]"
  data-gtm-subname="[[opcao]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [nome-filtro]]  | Deve retornar o nome do filtro | "documento", "susep", "seguro-coletivo" e etc |
| [[opcao]]  | Deve retornar a opção selecionada | "seguro-novo", "renovacao", "sim" e etc |


<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "orcamento"
 >
</form>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 


<br />


- **Na interação com os campos do formulário em cada etapa**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "nome", "data-nascimento" e etc |


<br />

- Modal

![Modal](https://implementacaoaunica.github.io/client/prints/distribuicao.png?raw=true)

- **No clique dos botões dos modais**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-button]]"
  data-gtm-subname="[[nome-modal]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-button]]  | Deve retornar o nome do botão clicado | "salvar-e-sair", "ok", "fechar" e etc|
| [[nome-modal]]  | Deve retornar o nome do modal | "modal-distribuicao-de-corretagem", "modal-depreciacao" e etc |



<br />

![selecao](https://implementacaoaunica.github.io/client/prints/selecao.png?raw=true)

- **Ao selecionar alguma opção nos modais de cada step**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[selecao]]",
  data-gtm-subname="[[nome-modal]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[selecao]]  | Deve retornar a opçao selecionada | "orcamento/azul-seguros" e etc |
| [[nome-modal]]  | Deve retornar o nome do modal | "modal-distribuicao-de-corretagem", "modal-depreciacao", "modal-opcoes-de-impressao" e etc |



<br />

![checkbox](https://implementacaoaunica.github.io/client/prints/checkbox.png?raw=true)

- **Ao selecionar alguma opção de checkbox nos steps**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "porto-seguro", "itau-seguro-auto" e etc |
| [[titulo]]  | Deve retornar o nome do titulo | "seguradoras-para-calcular" e etc |
| [[step]]  | Deve retornar o nome do step | "orcamento-cliente", "orcamento-veiculo" e etc |

<br />

---

### Proposta

![proposta](https://implementacaoaunica.github.io/client/prints/proposta.png?raw=true)


- **No clique dos botões ou links**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="[[button ou link]]"
  data-gtm-name="[[nome-item]]"
  data-gtm-subname="proposta"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[[button ou link]]  | Deve retornar o tipo de elemento clicado | "botao" ou "link" |
| [[nome-item]]  | Deve retornar o nome do botão ou link clicado | "voltar-para-orcamento", "salvar", "continuar" e etc |


<br />

- **Ao selecionar alguma opção de checkbox nos steps**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "1x-todas-cartao-de-credito-porto-seguro", "o-cliente-deseja-adquirir-cartao-porto" e etc |
| [[titulo]]  | Deve retornar o nome do titulo | "formas-de-pagamento", "dados-de-cobrança" e etc |

<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "proposta"
 >
</form>
```



<br />


- **Na interação com os campos do formulário em cada etapa**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "numero-cartao-credito", "validade-cartao" e etc |


<br />

![modal-atencao](https://implementacaoaunica.github.io/client/prints/modal-atencao.png?raw=true)


- **No clique dos botões dos modais de cada step**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-button]]"
  data-gtm-subname="[[nome-modal]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-button]]  | Deve retornar o nome do botão clicado | "sim", "nao" e etc|
| [[nome-modal]]  | Deve retornar o nome do modal | "atencao" e etc |



<br />

---

### Seguro Carta Verde

![seguro-carta-verde](https://implementacaoaunica.github.io/client/prints/seguro-carta-verde.png?raw=true)


- **No clique dos botões ou links**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="[[button ou link]]"
  data-gtm-name="[[nome-item]]"
  data-gtm-subname="seguro-carta-verde"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[[button ou link]]  | Deve retornar o tipo de elemento clicado | "botao" ou "link" |
| [[nome-item]]  | Deve retornar o nome do botão ou link clicado | "buscar", "pesquisar-cep" e etc |


<br />



- **Ao selecionar alguma opção nos steps**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "segurado", "cliente", "porto" e etc|
| [[titulo]]  | Deve retornar o nome do titulo | "cliente", "empresa" e etc |

<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "seguro-carta-verde"
 >
</form>
```


<br />


- **Na interação com os campos do formulário em cada etapa**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "placa", "chassi" e etc |


<br />

- Modal

![modal-resultado](https://implementacaoaunica.github.io/client/prints/modal-resultado.png?raw=true)

- **No clique dos botões dos modais**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-button]]"
  data-gtm-subname="resultado-da-busca-de-apolice"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-button]]  | Deve retornar o nome do botão clicado | "salvar-e-sair", "ok", "fechar" e etc|

<br />

- Historico Transmissão de Orçamentos

![modal-resultado](https://implementacaoaunica.github.io/client/prints/modal-resultado.png?raw=true)

- **No clique dos botões dos modais**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-button]]"
  data-gtm-subname="historico-de-transmissao-de-orcamentos"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-button]]  | Deve retornar o nome do botão clicado | "imprirmir", "fechar"|

<br />

---

### Renovação

![renovacao](https://implementacaoaunica.github.io/client/prints/renovacao.png?raw=true)


- **No clique dos botões**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-item]]"
  data-gtm-subname="renovacao"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-item]]  | Deve retornar o nome do botão clicado | "solicitar-renovacao", "consultar-solicitacoes" e etc |


<br />



- **Ao selecionar alguma opção nos steps**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "COL10J", "todos"|
| [[titulo]]  | Deve retornar o nome do titulo | "susep", "marca" e etc |

<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "renovacao"
 >
</form>
```


<br />


- **Na interação com os campos do formulário em cada etapa**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "sucursal", "apolice" e etc |


<br />

---

### Endoso

- Inclusão de item

![inclusao](https://implementacaoaunica.github.io/client/prints/inclusao.png?raw=true)


- **No clique doo botão buscar**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="buscar"
  data-gtm-subname="endosso-inclusao-de-item"
 >
</div>
```

<br />



- **Ao selecionar alguma opção no formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "COL10J", "1" e etc|
| [[titulo]]  | Deve retornar o nome do titulo | "susep", "quantidade-de-itens" e etc |

<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "endosso-inclusao-de-item"
 >
</form>
```

<br />


- **Na interação com os campos do formulário em cada etapa**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
  data-gtm-subname= "endosso-inclusao-de-item"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "sucursal", "apolice" e etc |


<br />




- Substituição de Veiculo ou alteração de Apólice

![substituicao](https://implementacaoaunica.github.io/client/prints/substituicao.png?raw=true)


- **No clique doo botão buscar**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="buscar"
  data-gtm-subname="endosso-substituicao-de-veiculo-ou-alteracao-de-apolice"
 >
</div>
```


<br />



- **Ao selecionar alguma opção no formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "COL10J", "1" e etc|
| [[titulo]]  | Deve retornar o nome do titulo | "susep", "n-item" e etc |

<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "endosso-substituicao-de-veiculo-ou-alteracao-de-apolice"
 >
</form>
```

<br />


- **Na interação com os campos do formulário**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
  data-gtm-subname= "endosso-substituicao-de-veiculo-ou-alteracao-de-apolice"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "sucursal", "apolice" e etc |


<br />


- Cancelamento de apolice ou item

![cancelamento](https://implementacaoaunica.github.io/client/prints/cancelamento.png?raw=true)


- **No clique doo botão buscar**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="buscar"
  data-gtm-subname="endosso-cancelamento-de-apolice-ou-item"
 >
</div>
```

<br />



- **Ao selecionar alguma opção no formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "COL10J", "1" e etc|
| [[titulo]]  | Deve retornar o nome do titulo | "susep", "n-item" e etc |

<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "endosso-cancelamento-de-apolice-ou-item"
 >
</form>
```

<br />


- **Na interação com os campos do formulário**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
  data-gtm-subname= "endosso-cancelamento-de-apolice-ou-item"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "sucursal", "apolice" e etc |


<br />

---

### Oportunidade de Vendas

![oportunidade-de-vendas](https://implementacaoaunica.github.io/client/prints/oportunidade-de-vendas.png?raw=true)


- **No clique doo botão buscar**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="buscar"
  data-gtm-subname="oportunidade-de-vendas"
 >
</div>
```

<br />



- **Ao selecionar alguma opção no formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="select"
  data-gtm-name="[[item-selecionado]]"
  data-gtm-subname="[[titulo]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[item-selecionado]]  | Deve retornar o nome do item selecionado | "ex-cliente-porto", "n-documento" e etc|
| [[titulo]]  | Deve retornar o nome do titulo | "tipo-de-cliente", "tipo" e etc |

<br />

- **Em todas as etapas do formulario**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<form 
  data-gtm-type="form"
  data-gtm-name= "oportunidade-de-vendas"
 >
</form>
```

<br />


- **Na interação com os campos do formulário**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-form="input"
  data-gtm-name="[[campo]]"
  data-gtm-subname= "oportunidade-de-vendas"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "nome-do-proponente", "numero" e etc |


<br />

---

### Novidades

![novidades](https://implementacaoaunica.github.io/client/prints/novidades.png?raw=true)


- **No clique do botão buscar**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-botao]]"
  data-gtm-subname="novidades"

 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-botao]]  | Deve retornar o nome do botao clicado | "proximo", "anterior", "comecar-as-mudancas" |


<br />

---



### Parametrização


- Proposta Veículo


As URL’s devem conter o parâmetro origem e seu valor deve ser proposta-cadastro


![veiculo](https://implementacaoaunica.github.io/client/prints/veiculo.PNG?raw=true)

https://aplwebhml.portoseguro.brasil/portoprintweb/index.jsp?menuid=COL-02XV6&orig=menu_auto&portal=1&novoportal=1&portal=1&corsus=COL10J&webusrcod=2299925&usrtip=S&sesnum=4669456&cpf=11144477735?origem=proposta-cadastro


- Faq

As URL’s devem conter o parâmetro origem e seu valor deve ser portoprintweb


![faq](https://implementacaoaunica.github.io/client/prints/faq.PNG?raw=true)


https://aplwebhml.portoseguro.brasil/portoprintweb/file/faq.pdf?origem=portoprintweb


- Manual


As URL’s devem conter o parâmetro origem e seu valor deve ser portoprintweb


![faq](https://implementacaoaunica.github.io/client/prints/manual.PNG?raw=true)


https://aplwebhml.portoseguro.brasil/portoprintweb/file/manual.pdf?origem=portoprintweb



---

> Em caso de dúvidas, estamos a disposição.

<br />

<script> document.querySelector('h1').style.display = 'none' </script>

