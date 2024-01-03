# Svelte kit

## Passos Iniciais

Para criar nosso projeto Svelte, temos alguns pré-requisitos.

1 - Precisamos ter o node e o npm instalados na máquina. Estarei utilizando o `node v20.5.1` e o `npm v9.8.1` para este projeto.

2 - Executar o comando `npm create svelte@latest <nome-do-app>`, ao rodar este comando, o projeto será inicializado e algumas configurações podem ser selecionadas via linha de comando mesmo. Navegue usando as setas do teclado e selecione uma opção com a tecla enter

```
- Template da aplicação. Temos projeto demo, somente o esqueleto e template de biblioteca. Estarei utilizando Skeleton project.

Welcome to SvelteKit!

◆  Which Svelte app template?
│  ● SvelteKit demo app (A demo app showcasing some of the features of SvelteKit - play a word guessing game that works without JavaScript!)
│  ○ Skeleton project
│  ○ Library project

- Checagem de tipos. Usando JS com comentários JSDoc, usando sintaxe TypeScript ou sem checagem de tipos. Estarei utilizando a opção TypeScript.

◆  Add type checking with TypeScript?
│  ● Yes, using JavaScript with JSDoc comments
│  ○ Yes, using TypeScript syntax
│  ○ No

- Opções adicionais, marque com a tecla espaço o que preferir. Aqui não utilizarei nenhuma das oções.

◆  Select additional options (use arrow keys/space bar)
│  ◻ Add ESLint for code linting
│  ◻ Add Prettier for code formatting
│  ◻ Add Playwright for browser testing
│  ◻ Add Vitest for unit testing
```

Depois de configurado via linha de comando, o diretório será criado e alguns passos adicionais são necessários. Precisamos acessar o diretório e rodar o comando `npm install`, depois disso basta rodar `npm run dev -- --open` e o projeto será aberto no seu navegador padrão.

## Estrutura do Projeto Criado

- .svelte-kit & node-modules: Contém arquivos gerados pelo svelte. Podemos ignorar por enquanto.

- src: Onde ficará o código fonte da aplicação.
    - diretório `routes`: Aqui é onde colocaremos todas as rotas da aplicação, todas as telas da aplicação serão arquivos dentro da pasta route.
    - arquivo `app.d.ts`: ?
    - arquvio `app.html`: Onde tem o HTML padrão da aplicação. Vamos notar que ele utiliza % para algumas coisas. Dentro de arquivos html, posso utilizar alias. Um exemplo seria utilizar `%sveltekit.assets%`, que seria um alias para a o diretório static. `%sveltekit.head%` é um alias para o cabeçalho da aplicação. O mesmo para `%sveltekit.body%` mas para o corpo da apicação. Muito provavelmente nem será mexido, todas as alterações serão feitas em outros lugares.

- diretorio `static`: Onde iremos colocar nossos assets. Arquivos de fonte, imagens e etc, para serem utilizados pela aplicação.

- Arquivo `svelte.config.js`: Este é o arquivo de configuração do Svelte (criado automaticamente).

## Criando uma nova rota

Sempre que formos criar uma nova rota, criamos uma nova pasta dentro de routes e criamos um arquivo chamado `+page.svelte`. Ao acessar a aplicação, basta acessar `<url-app>/<nome-da-pasta-criada>` e estaremos navegando no arquivo criado. O nome do arquivo deve ser sempre `+page.svelte`, caso contrário não irá funcionar. Importante entender que é um nome reservado.

Vamos criar uma rota chamada `sobre` por exemplo. Dentro de routes criamos a pasta `sobre` e dentro dela um arquivo chamado `+page.svelte`. Ficando com a seguinte estrutura:

```
routes
├── +page.svelte
└── sobre
    └── +page.svelte
```

Agora utilizando nosso terminal, rodamos o comando `npm run dev` para iniciarmos a aplicação e acessamos a URL `http://localhost:5173/` para vermos a pagina inicial do svelte (o arquivo +page.svelte criado pelo framework). Ao acessarmos `/sobre` nessa URL, seremos direcionados para a nova rota criada por nós.

## Linkando arquivos

Na página inicial da aplicação, criaremos uma lista que nos levará para 3 rotas direntes: Sobre, Portfólio e Contato. Mas como linkamos o item da lista "Sobre" para a rota `/sobre` que criamos? No Svelte basta colocar a tag de links do html normalmente com `<a href=""></a>` e dentro do href adicionar o nome da rota desejada.

```
<h1>Aprendendo Svelte</h1>

<ul>
    <li><a href="/sobre">Sobre</a></li>
    <li><a href="/portfolio">Portfólio</a></li>
    <li><a href="/contato">Contato</a></li>
</ul>
```

Como utilizamos links relativos, ele irá pegar a raiz e colocar o conteúdo de href dentro dele. Fazendo isso já poderemos clicar em Sobre e sermos levados para a rota criada por nós. O mesmo vale para adicionar um botão que nos leva para a Home page. Basta criar a tag de link e adicionar uma `/` no href.

```
<h1>Sobre</h1>

<ul>
    <li><a href="/">Home</a></li>
    <li><a href="/sobre">Sobre</a></li>
    <li><a href="/portfolio">Portfólio</a></li>
    <li><a href="/contato">Contato</a></li>
</ul>
```

## Removendo códigos repetidos

Note que estamos repetindo o mesmo código para adicionar a lista criada em todas as rotas, isto não é uma boa prática. O ideal seria utilizar um componente para contornar este problema, falaremos sobre isso mais tarde. No momento vamos criar um `Layout` para a página.

Um layout no svelte, é mais um nome reservado e um arquivo, assim como o `+page.svelte`. Dentro do diretório routes, iremos criar o arquivo `+layout.svelte` e dentro dele iremos adicionar todo o conteúdo que será compartilhado entre as páginas.

Dentro do arquivo layout, adicionaremos a tag `<slot />` que pertence ao svelte. Com ela, dizemos para o svelte renderizar todo o conteúdo que queremos dentro das páginas, criando um ponto de inserção.

```
<!-- Arquivo -->
<ul>
    <li><a href="/">Home</a></li>
    <li><a href="/sobre">Sobre</a></li>
    <li><a href="/portfolio">Portfólio</a></li>
    <li><a href="/contato">Contato</a></li>
</ul>

<!-- Estrutura -->
routes
├── contato
│   └── +page.svelte
├── +layout.svelte
├── +page.svelte
├── portfolio
│   └── +page.svelte
└── sobre
    └── +page.svelte
```

Agora, se acessar o localhost, irá notar que o conteúdo do Layout sempre permanece nas páginas e que o conteúdo único das rotas é renderezido através da tag `<slot />`

## Layouts específicos para rotas

O arquivo `+layout` da raiz, aplica o mesmo layout para todas as rotas que temos no nosso projeto. Mas e se quisermos utilizar um layout específico para alguma rota? Basta criar um arquivo `+layout.svelte` dentro da pasta da rota desejada, tendo o arquivo dentro de um diretório específico de rota o layout será aplicado somente àquela rota. 

Vamos criar um arquivo +layout.svelte dentro da pasta portfolio e adicionarmos um conteúdo dele para testarmos. Lembrando que é necessário a tag `<slot />` para que o conteúdo do arquivo `+page.svelte` seja carregado neste arquivo.

```
// Arquivo

<h1>Layout do portfolio</h1>
<slot />

// Estrutura de arquivos dentro de /src/routes

 routes
├── contato
│   └── +page.svelte
├── +layout.svelte
├── +page.svelte
├── portfolio
│   ├── +layout.svelte
│   └── +page.svelte
└── sobre
    └── +page.svelte
```

Agora se navegarmos entre as páginas, veremos que o conteúdo adicionado neste novo arquivo só se aplica na página portfólio.

## Páginas de erro

Vamos adicionar um novo item na lista do arquivo `+layout.svelte` geral do projeto. Neste caso, como não criamos a rota para este novo item, ao acessarmos o item teremos o erro 404. No diretório de rotas, vamos criar um arquivo chamada `+error.svelte`. Quando uma página que não existe for acessada, ao invés de um erro 404 ser exibido, o conteúdo do arquivo `+error.svelte` será carregado. Isto permite personalizar as mensagens de erro que serão exibidas no projeto.

```
routes
├── contato
│   └── +page.svelte
├── +error.svelte
├── +layout.svelte
├── +page.svelte
├── portfolio
│   ├── +layout.svelte
│   └── +page.svelte
└── sobre
    └── +page.svelte
```

## Adicionado bibliotecas com svelte-add

O svelte-add serve para adicionarmos novas bibliotecas no nosso projeto svelte utilizando terminal. Ele integra tailwind, bootstrap e muitos outros. Cheque o [repositório oficial](https://github.com/svelte-add/svelte-add) para mais informações. Aqui vamos adicionar o TailwindCSS para estilizarmos nossa aplicação com o comando `npx svelte-add@latest tailwindcss  --forms --typography`.

## Componentes

Os componentes em svelte, sempre serão um arquivo svelte e, diferente do Angular por exemplo, não precisamos importar ele no módulo global, só de criarmos o arquivo e importando ele onde iremos utilizar já funciona. Os componentes devem ser nomeados com PascalCase. Um componente pode ter as tags script e style também.

## Alias 

Note que dentro da pasta src, temos uma pasta chamada `lib`. Em projetos Svelte, costumamos colocar nossos componentes dentro desta pasta. Vamos supor que temos um componente chamado `Logo.svelte` lá dentro e iremos utilizar ele dentro do `+page.svelte` de `/src`. O arquivo ficaria assim:

```svelte
<script>
  // Import com o caminho completo
  import Logo from '../lib/Logo.svelte';
</script>

<h1>Aprendendo Svelte</h1>

<Logo />
```

O svelte oferece um recurso de alias para estes casos e, para a pasta lib, ele já vem configurado por padrão, basta utilizar o símbolo $ + pasta ficando assim:

```svelte
<script>
  // Import com o alias
  import Logo from '$lib/Logo.svelte';
</script>

<h1>Aprendendo Svelte</h1>

<Logo />
```

Isso evita imports errados no código utilizando navegação de diretórios com '..' e também facilita uma possível refatoração, caso o arquivo em questão mude de diretório, o svelte sempre saberá chegar no diretório lib através do alias. Também é possível criar seu próprio alias, vamos por exemplo criar uma pasta chamada 'utils' dentro de /src. Para criarmos o alias desta pasta, basta acessar o arquivo `svelte.config.js` e adicionar o parâmetro alias dentro de kit, ficando assim:

```js 
kit: {
  alias: {
    $utils: "/src/utils",
  },
  // adapter-auto only supports some environments, see https://kit.svelte.dev/docs/adapter-auto for a list.
  // If your environment is not supported or you settled on a specific environment, switch out the adapter.
  // See https://kit.svelte.dev/docs/adapters for more information about adapters.
  adapter: adapter(),
},
```


