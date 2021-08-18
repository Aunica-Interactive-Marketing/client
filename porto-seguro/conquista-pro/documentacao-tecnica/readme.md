![aunica](https://implementacaoaunica.github.io/client/aunica.jpg?raw=true)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Guia de estruturação HTML para tagueamento - Conquista PRO

> Última atualização: 18/08/2021 <br />

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
- [Parametrização](#parametriza&ccedil;&atilde;o)



## Objetivo

<p style='text-align: justify;'>  
Neste documento são apresentados os requisitos necessários para que seja possível a implementação de uma estrutura otimizada e consistente de tagueamento no ambiente Conquista PRO, diminuindo esforços com manutenção e evitando a perda de dados nas bases de Analytics.
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
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-P8LMS8H');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-P8LMS8H" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
    id_usuario: '',
    id_conquista: '',
    tipo_usuario: '',
  },
  produto: {
    nome_produto: ''.
  }
}
</script>
```

| Atributo  | Descrição de preenchimento  | Tipo de dado | Valor padrão | Exemplo |
| :-------- | :-------------------------- | :----------- | :----------- | :------ |
| customData.site  | Objeto destinado a armazenar informações sobre a página/site que está sendo acessada. | Objeto | ""| |
| customData.site.brand  | Deve indicar qual a marca do site. | Texto | ""| “portoseguro”|
| customData.site.versao  | Deve indicar qual variação/versão do site o usuário está acessando. | Texto | ""| “1.2”|
| customData.site.portal  | Deve indicar qual o tipo de produto. | Texto | ""| “conquista pro”|
| customData.user  | Objeto destinado a descrever as informações do usuário. O objeto deve ser trazido quando o usuário estiver identificado.| Objeto | ""| |
| customData.user.id_usuario  | CPF Criptografado  AESCryptography [Utilizada nos ambientes porto e azul](https://docs.google.com/document/d/1OXUxPWIHjis-nY0GdUgxjT_Zh8uSrZRHVo6atV86b9Y/edit) | Texto | ""| “a750c220a060fcf487f9519d3203035b”|
| customData.user.id_conquista  | Deve indicar o id na plataforma do conquista | Texto| ""| “123" e etc|
| customData.user.tipo_usuario  | Deve indicar a classificação do usuário em sua conta PortoSeguro | Texto| ""| “corretor”, “agente-autonomo-de-investimento"|
| customData.produto  | Objeto destinado a descrever as informações do produto| Objeto | ""| |
| customData.produto.nome_produto  | Deve indicar o nome do produto | Texto| ""| “previdencia", "consorcio-de-veiculo" e etc|

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
| etapa     | Deve retornar a etapa correspondente | "/conquista-pro/home-deslogada" e etc |
| titulo     | Deve retornar o nome da pagina | "Conquista Pro - Area deslogada home" e etc |

<br />

#### Visualização de modal

<p style='text-align: justify;'>Em aplicações onde existe a interação dentro modais na página, há a necessidade de um push no objeto dataLayer para sabermos o momento exato em que o usuário trocou de passo:</p>

```html
<script>
dataLayer = window.dataLayer || [];
dataLayer.push({
  event: 'modal',
  nome: '',
  acao: ''
})
</script>
```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| nome      | Deve retornar o nome do modal | /conquista-pro/login/cpf e etc |
| acao      | Deve retornar a ação do usuário com o modal | "abrir" ou "fechar" |

<br />


#### Solicitação de serviços

<p style='text-align: justify;'>Chamada criada para ser disparada sempre que algum dos serviços for solicitado pelo usuário.</p>

```html
<script>
dataLayer.push({
    event: 'solicitacao_servico',
    nome_servico: '',
    tipo_servico: '',
    produto: '',
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
| nome_servico | Deve indicar o nome do serviço que será realizado | "login  e etc |
| tipo_servico | Deve indicar qual o tipo de serviço que está sendo solicitado | "entrar" e etc |
| produto        | Deve indicar o nome do produto | "previdencia" e etc |
| retorno      | Deve indicar o sucesso ou erro da tentativa da solicitação de serviço | "sucesso" ou "erro" |
| descricao    | Deve trazer a descrição do retorno | "login-efetuado-com-sucesso" e etc |
| codigo       | Deve trazer o código do erro | "124 e etc |
| servico      | Deve trazer qual serviço foi acionado  | "login-api" e etc |
| mensagem    | Deve trazer a descrição do erro | "dados-invalidos" e etc |

<br />


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
| servico  | Deve trazer qual serviço foi acionado  | "produtos" e etc |
| mensagem  | Deve trazer a descrição do erro | "dados-invalidos" e etc |

<br />

### Parametrização

- Retornar a URL quando o usuário entra no site do Conquista Pro através do site Corretor Online. 

https://wwws.portoseguro.com.br/parceiroportoparfront/#/aai?utm_source=corretor-online&utm_medium=site&utm_campaign=corretor-online




![Col](https://aunica-interactive-marketing.github.io/client/prints/parametrizacao-col.png?raw=true)






- Trazer a URL parametrizada no “Corretor Online”.


https://corretor.portoseguro.com.br/corretoronline/?utm_source=conquista-pro&utm_medium=site&utm_campaign=conquista-pro



![conquista](https://aunica-interactive-marketing.github.io/client/prints/conquista-pro.png?raw=true)





---

> Em caso de dúvidas, estamos a disposição.

<br />

<script> document.querySelector('h1').style.display = 'none' </script>

