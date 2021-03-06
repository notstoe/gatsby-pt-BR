---
title: Usando a Pasta de Arquivos Estáticos
---

No geral, todo _website_ precisa de _assets_: imagens, folhas de estilo, _scripts_, etc. Quando se está usando Gatsby, nós recomendamos
[Importar _Assets_ diretamente](/docs/importing-assets-into-files/) em arquivos JavaScript, por causa dos benefícios que são adquiridos:

- _Scripts_ e folhas de estilo são minificados e empacotados juntos para evitar chamadas adicionais de rede.
- Arquivos não encontrados causam erros de compilação ao invés de erros 404 para seus usuários.
- Nomes de arquivos resultantes incluem _hashes_ de conteúdo para que você não precise se preocupar sobre o cacheamento de versões antigas pelo browser.

Entretanto, existe um **saída** para que você consiga utilizar um _asset_ fora do sistema de módulos.

## Adicionando _assets_ fora do sistema de módulos

Você consegue criar uma pasta chamada `static` na raiz de seu projeto. Cada arquivo colocado nessa pasta será copiado para a pasta `public`. Por exemplo, se você adicionar um arquivo chamado `sun.jpg` dentro da pasta `static`, ele será copiado para `public/sun.jpg`.

### Referenciando seu _asset_ estático

Você pode referenciar _assets_ da pasta `static` no seu código sem precisar de nada especial:

```jsx
render() {
  // Nota: essa é uma saída que deve ser usada com moderação!
  // Normalmente nós recomendamos usar `import` para pegar URLs de assets
  // como descrito na página "Importando _Assets_ diretamente".
  return <img src={'/logo.png'} alt="Logo" />;
}
```

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

### Desvantagens

Tenha em mente as desvantagens dessa implementação:

- Nenhum desses arquivos na pasta `static` será pós-processado ou minificado.
- Arquivos não encontrados não serão invocados em tempo de compilação, e irão causar um erro 404 para seus usuários
- Nomes de arquivos resultantes não terão _hashes_ de conteúdo, então você precisará adicionar argumentos na _query_ ou renomeá-los cada vez que eles mudam.

## Quando usar a pasta `static`

Normalmente, nós recomendamos importar [folhas de estilo, imagens, e assets de fonte](/docs/importing-assets-into-files/) pelo JavaScript. A pasta `static` é útil como um contorno para um pequeno número de casos comuns, como:

- Você precisa de um arquivo com um nome específico na saída da compilação, como [`manifest.webmanifest`](https://developer.mozilla.org/en-US/docs/Web/Manifest).
- Você tem centenas de imagens e precisa referenciar seus caminhos dinamicamente.
- Você quer incluir um pequeno script externo como [`pace.js`](http://github.hubspot.com/pace/docs/welcome/).
- Algumas bibliotecas podem ser incompatíveis com o `Webpack` e você não possui outra opção que não seja incluí-la com uma tag `<script>`.
- Você precisa importar um arquivo JSON que não possui um esquema consistente, como [arquivos TopoJSON](https://en.wikipedia.org/wiki/GeoJSON#TopoJSON), que é trabalhoso de lidar usando GraphQL. Observe que, importar um arquivo JSON diretamente dentro de uma página, template ou componente usando a sintaxe `import` resultará em adicionar este arquivo ao pacote do aplicativo e aumentar o tamanho de todas as páginas do site. Para não ocorrer isso, é melhor colocar seu arquivo JSON dentro da pasta `static` e usar a sintaxe de importação dinâmica (`import('/static/myjson.json')`) dentro do ciclo de vida do `componentDidMount` ou o hook `useEffect`.
