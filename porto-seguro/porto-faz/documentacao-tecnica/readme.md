![aunica](https://aunica-interactive-marketing.github.io/client/aunica.jpg?raw=true)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Guia de estruturação HTML para tagueamento - Porto Faz

> Última atualização: 24/06/21

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



## Objetivo

<p style='text-align: justify;'>  
Neste documento são apresentados os requisitos necessários para que seja possível a implementação de uma estrutura otimizada e consistente de tagueamento no ambiente do Porto faz, diminuindo esforços com manutenção e evitando a perda de dados nas bases de Analytics.
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
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-N72SPK4');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-N72SPK4" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
  },
  page: {
    name: '',
 },
 produto: {
    origem: '',
  },
}
</script>
```


| Atributo  | Descrição de preenchimento  | Tipo de dado | Valor padrão | Exemplo |
| :-------- | :-------------------------- | :----------- | :----------- | :------ |
| customData.site | Objeto destinado a armazenar informações sobre o site que está sendo acessado. | Objeto | ""| |
| customData.site.brand | Deve indicar qual a marca do site. | Texto | ""| “portoseguro”|
| customData.site.versao | Deve indicar qual variação/versão do site o usuário está acessando. | Texto | ""| “1.2”|
| customData.site.portal | Deve indicar qual o tipo de produto. | Texto | ""| “Porto Faz”|
| customData.user | Objeto destinado a descrever as informações do usuário. O objeto deve ser trazido quando o usuário estiver identificado.| Objeto | ""| |
| customData.user.id | Deve indicar o ID de usuário que a plataforma atribui. | Texto | ""| "ABC123"|
| customData.page | Objeto destinado a armazenar informações sobre a página que está sendo acessada. | Objeto | ""| |
| customData.page.name  | Deve retornar o nome descritivo da pagina | Texto | ""| “home”, "conserto-e-manutencao" e etc |
| customData.produto.origem  | Deve retornar de onde o usuário veio | Texto | ""| "menu-lateral, "categoria-subcategoria", "promocao" etc e etc |



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


#### Visualização de modal

<p style='text-align: justify;'>Em aplicações onde existe a interação dentro modais na página, há a necessidade de um push no objeto dataLayer para sabermos o momento exato em que o usuário trocou de passo:</p>

```html
<script>
dataLayer = window.dataLayer || [];
dataLayer.push({
  event: 'modal',
  titulo: '',
  acao: ''
})
</script>
```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| titulo      | Deve retornar o nome do modal | "atencao" e etc |
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
    termo_buscado: '',
    retorno: '',
    descricao: '',
    erro: {
        codigo: '',
        servico: '',
        mensagem: ''
    }
});
</script>


```

| Atributo  | Descrição de preenchimento  | Exemplo |
| :-------- | :-------------------------- | :------ | 
| nome_servico | Deve indicar o nome do serviço que será realizado | "proposta", "orcamento" e etc |
| tipo_servico | Deve indicar qual o tipo de serviço que está sendo solicitado | "salvar-orcamento" e etc |
| termo_buscado  | Deve retornar o termo buscado | "limpeza" e etc |
| codigo  | Deve trazer o código do erro | "124 e etc |
| servico  | Deve trazer qual serviço foi acionado  | "busca" e etc |
| mensagem  | Deve trazer a descrição do erro | "dados-invalidos" e etc |



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

---

### Exemplos práticos

### Cookies

![cookies](https://aunica-interactive-marketing.github.io/client/prints/cookies.png?raw=true)


- **No clique dos links ou botões referente aos cookies**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="[[link ou button]]"
  data-gtm-name="[[nome-item]]"
  data-gtm-subname="cookies"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-item]]  | Deve retornar o nome do link ou botão clicado | "continuar-e-fechar", "politica-de-privacidade"|

<br />

---

### Home 

![home](https://aunica-interactive-marketing.github.io/client/prints/home.png?raw=true)


- **No clique dos itens do menu**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div  
  data-gtm-type="click"
  data-gtm-clicktype="menu"
  data-gtm-name="[[nome-menu]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-menu]]  | Deve retornar o nome do menu clicado | "todos-os-servicos", "reagendamento", "retorno" e etc |

<br />


- **No clique dos itens do header**<br />

```html
<div  
  data-gtm-type="click"
  data-gtm-clicktype="header"
  data-gtm-name="[[nome-item]]"
 >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-item]]  | Deve retornar o nome do header clicado | "guincho-24h", "reppara", "logo-porto-seguro", "logo-porto-faz", "preencher-proposta" e etc |

<br />

- **No clique dos serviços porto faz**<br />

```html
<div  
  data-gtm-type="click"
  data-gtm-clicktype="card"
  data-gtm-name="[[nome-card]]"
  data-gtm-subname="servicos-porto-faz"
  >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-card]]  | Deve retornar o nome do card clicado| "eletricista", "encanador", "limpeza" e etc |

<br />

- **No clique dos links da seção dos serviços porto faz**<br />

```html
<div  
  data-gtm-type="click"
  data-gtm-clicktype="link"
  data-gtm-name="[[nome-link]]"
  data-gtm-subname="servicos-porto-faz"
  >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-link]]  | Deve retornar o nome do link clicado| "ver-mais", "ver-menos"e etc |

<br />

![continuacao-home](https://aunica-interactive-marketing.github.io/client/prints/continuacao-home.png?raw=true)
![final](https://aunica-interactive-marketing.github.io/client/prints/final.png?raw=true)

- **No clique dos botões das seções**<br />

```html
<div  
  data-gtm-type="click"
  data-gtm-clicktype="button"
  data-gtm-name="[[nome-button]]"
  data-gtm-subname="[[nome-secao]]"
  >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-button]]  | Deve retornar o nome do botão clicado| "acesse", "saiba-mais", "acesse-o-chat" e etc |
| [[nome-secao]]  | Deve retornar o nome da seção do botão| "ja-e-cliente-porto-seguro", "ficou-alguma-duvida" e etc |

<br />

- **Na interação com os campos para receber promoções**<br />

```html
<!-- Use se os atributos no elemento a ser clicado -->
<input  
  data-gtm-type="input"
  data-gtm-name="[[campo]]"
 >
</input>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[campo]]  | Deve retornar o nome do campo que teve a interação | "nome", "email" e etc |


<br />

---

### Home - Menu lateral

![menu-lateral](https://aunica-interactive-marketing.github.io/client/prints/menu-lateral.png?raw=true)

- **No clique dos itens do menu lateral**<br />

```html
<div  
  data-gtm-type="click"
  data-gtm-clicktype="menu-lateral"
  data-gtm-name="[[nome-menu]]"
  data-gtm-subname="[[nome-submenu]]"
  >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-menu]]  | Deve retornar o nome do menu| "destaques", "eletricista", "encanador" e etc |
| [[nome-submenu]]  | Deve retornar o nome do submenu clicado| "servicos-em-promocao", "instalacao-de-chuveiro" e etc |

<br />

---

### Categorias

![limpeza](https://aunica-interactive-marketing.github.io/client/prints/limpeza.png?raw=true)
![conserto](https://aunica-interactive-marketing.github.io/client/prints/conserto.png?raw=true)
![instalacao](https://aunica-interactive-marketing.github.io/client/prints/instalacao.png?raw=true)
![eletricista](https://aunica-interactive-marketing.github.io/client/prints/eletricista.png?raw=true)
![encanador](https://aunica-interactive-marketing.github.io/client/prints/encanador.png?raw=true)
![automacao](https://aunica-interactive-marketing.github.io/client/prints/automacao.png?raw=true)
![manutencao](https://aunica-interactive-marketing.github.io/client/prints/manutencao.png?raw=true)
![carro](https://aunica-interactive-marketing.github.io/client/prints/carro.png?raw=true)
![chaveiro](https://aunica-interactive-marketing.github.io/client/prints/chaveiro.png?raw=true)


- **No clique dos links em seções nas páginas das categorias**<br />

```html
<div  
  data-gtm-type="click"
  data-gtm-clicktype="link"
  data-gtm-name="[[nome-link]]"
  data-gtm-subname="[[secao]]:categoria:[[nome-categoria]]"
  >
</div>
```

| Variavel  |  Descrição  | Exemplo |
| :-------- | :---------- | :------ | 
| [[nome-link]]  | Deve retornar o nome do link clicado | "sofa", "limpeza-ar-condicionado" e etc |
| [[secao]]  | Deve retornar o nome da secao| "limpeza-de-estofado", "ar-condicionado" e etc |
| [[nome-categoria]]  | Deve retornar o nome da categoria| "limpeza-e-higienizacao", "conserto-e-manutencao" e etc |

<br />


---

> Em caso de dúvidas, estamos a disposição.

<br />

<script> document.querySelector('h1').style.display = 'none' </script>

