![aunica](https://implementacaoaunica.github.io/client/aunica.jpg?raw=true)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Guia de estruturação para tagueamento - Chatbot PortoNet

> Última atualização: 25/06/2021 <br />

<br />

## Sumário

- [Objetivo](#objetivo)
- [Descrição Código Google Tag Manager](#descri&ccedil;&atilde;o-c&oacute;digo-google-tag-manager)
- [Objeto customData](#objeto-customdata)
- [Eventos Padrões](#eventos-padr&otilde;es)




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
    user_id: '',
    matricula: '',
  },
  flux: {
    id: '',
    name: ''
  }
}
</script>
```

| Atributo  | Descrição de preenchimento  | Tipo de dado | Valor padrão | Exemplo |
| :-------- | :-------------------------- | :----------- | :----------- | :------ |
| customData.site  | Objeto destinado a armazenar informações sobre a página/site que está sendo acessada. | Objeto | ""| |
| customData.site.brand  | Deve indicar qual a marca do site. | Texto | ""| “portos-eguro”, "azul-seguros|
| customData.site.versao  | Deve indicar qual variação/versão do site o usuário está acessando. | Texto | ""| “1.2”|
| customData.site.portal  | Deve indicar qual o tipo de produto. | Texto | ""| “ChatBot PortoNet”|
| customData.user  | Objeto destinado a descrever as informações do usuário. O objeto deve ser trazido quando o usuário estiver identificado.| Objeto | ""| |
| customData.user.id  | Deve indicar o ID de usuário que a plataforma atribui. | Texto | ""| "ABC123"|
| customData.user.user_id  | CPF do cliente criptografado utilizando o padrão já aplicado em outros ambientes. (Após o usuário ser identificado) [https://docs.google.com/document/d/1OXUxPWIHjis-nY0GdUgxjT_Zh8uSrZRHVo6atV86b9Y/edit](https://docs.google.com/document/d/1OXUxPWIHjis-nY0GdUgxjT_Zh8uSrZRHVo6atV86b9Y/edit) | Texto | ""| "kjdad54656564"|
| customData.user.matricula | Deve indicar o numero da matricula do funcionario | Texto | ""| "065666669898"|
| customData.flux.id  | Deve indicar o ID do fluxo  | Texto | ""| "1", "2" e etc|
| customData.flux.name  | Deve indicar o nome do fluxo| Texto | ""| "cliente", "funcionario"|


<br />


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
| etapa     | Deve retornar a etapa correspondente | "/pos-venda/chat/cliente" e etc |
| titulo     | Deve retornar o nome da pagina | "Cliente - Chatbot PortoNet" e etc |

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
| nome_servico | Deve indicar o nome do serviço que será realizado | "pessoa-fisica:prorrogacao-da-fatura" |
| tipo_servico | Deve indicar qual o tipo de serviço que está sendo solicitado | "prorrogar" e etc |
| retorno      | Deve indicar o sucesso ou erro da tentativa da solicitação de serviço | "sucesso" ou "erro" |
| descricao    | Deve trazer a descrição do retorno | "parcela-prorrogada" e etc |
| codigo       | Deve trazer o código do erro | "124 e etc |
| servico      | Deve trazer qual serviço foi acionado  | "cobranca" e etc |
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
| servico  | Deve trazer qual serviço foi acionado  | "cobranca" e etc |
| mensagem  | Deve trazer a descrição do erro | "dados-invalidos" e etc |

<br />

#### Prenchimento dos campos e cliques

```html
<script>
dataLayer.push({
      event:'auto.event',
      au_category: '',
      au_acao: '',
      au_label: ''
});
</script>
```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| au_category  | Deve trazer a categoria do evento | "chatbot-portonet:pos-venda:contratado:pessoa-fisica" e etc |
| au_acao  | Deve trazer a ação do evento  | "preencheu:campo:cpf" ,"click:opcao"|

<br />

---


> Em caso de dúvidas, estamos a disposição.

<br />

<script> document.querySelector('h1').style.display = 'none' </script>
