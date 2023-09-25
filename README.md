# Svelte kit

#### TODO: Passos inicias (criando o projeto e explicando as opções dadas)

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