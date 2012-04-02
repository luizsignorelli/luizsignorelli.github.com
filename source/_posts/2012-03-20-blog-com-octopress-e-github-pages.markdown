---
layout: post
title: "Blog com Octopress e GitHub Pages"
date: 2012-03-28 10:07
comments: true
published: true
categories: octopress
---

Esse é meu novo blog sobre desenvolvimento de software. A idéia é falar coisas que uso dia-a-dia e de projetos pessoais que estou desenvolvendo.

Nesse primeiro post gostaria de compartilhar as ferramentas que estou utilizando para servir e hospedar o blog: [GitHub Pages](http://pages.github.com) e [Octopress](http://octopress.org/).

##Github Pages
GitHub Pages é um serviço que o GitHub oferece aos seus usuários, onde é possível hospedar sua página/blog pessoal, e também páginas para seus projetos do GitHub, usando apenas Git. Para isso o GitHub utiliza o [Jekyll](http://jekyllrb.com/), um gerador de sites e blogs estático, escrito por um de seus fundadores.

Funciona mais ou menos assim:

1. Você cria suas páginas e posts usando a linguagem de marcação [Markdown](http://daringfireball.net/projects/markdown/), que é a mesma utilizada no Wiki do GiHub;
2. O **Jekyll** lê esses arquivos e os transforma em HTML já na estrutura correta para o GitHub Pages;
3. Você faz `push` para seu repositório no GitHub, e em alguns minutos seu site está no ar.

O único problema do **Jekyll** é que ele vem bem cru, cabendo a você fazer todo o layout e design das páginas. É aí que entra o **Octopress**.

##Octopress

O **Octopress** é um *framework* em cima do **Jekyll** que trás uma série de facilidadas e recursos pré-configurados, você só tem o trabalho de escrever o conteúdo.

Alguns recursos do octopress:

- Layout HTML 5 sermântico
- Layout responsivo para mobile
- Integração com Twitter, Google Plus One, Disqus, Pinboard, Delicious, e Google Analytics
- Deploy com Github pages ou Rsync
- Suporte a servidores Rack e [POW](http://pow.cx), para preview local
- Temas com Compass e Sass
- Syntax highlighting com [Solarized](http://ethanschoonover.com/solarized)

### Setup

O **Octopress** tem duas ependências: *git* e *Ruby 1.9.2*. O *git* você já deve ter instalado, caso contrário basta usar o gerenciador de pacotes do seu sistema operacional. Para Mac eu sugiro usar o [Homebrew](http://mxcl.github.com/homebrew).

Para instalar o *Ruby 1.9.2*, a melhor opção é usar um gerenciador de versões como o [RVM](https://rvm.beginrescueend.com/) ou [rbenv](https://github.com/sstephenson/rbenv).

Para conferir se as dependências estão instaladas faça:

```
git --version # deve ser 1.7.x
ruby -v       # deve ser 1.9.2
```

Se tudo estiver funcionando, o próximo passo é clonar o repositório do **Octopress**:

```
git clone git://github.com/imathis/octopress.git octopress
cd octopress    # Se você usa o RVM será perguntado se confia no script .rvmrc, responda sim (y)
```

O **Octopress** depende de algumas *Ruby gems*, para instalar essas dependências faça:

```
gem install bundler
rbenv rehash    # Só se você usa rbenv
bundle install
```

Agora gere o layout inicial com o tema *default*:

```
rake install
```

###Preview local

Para ver uma prévia do seu blog antes de fazer o *deploy* no Github faça:

```
rake generate   # Gera os arquivos estátivos do blog 
rake preview    # Inicia o servidor e observa qualquer alteração nos fontes
```
Acessando no seu browser `http://localhost:4000`, você deve ver seu blog vazio.

### Deploy com Github Pages

Para você hospedar seu blog no **Github Pages** o primeiro passo é criar um repositório no Github chamdo `<seu_usuario_github>.github.com`, no meu caso `luizsignorelli.github.com`.

O **Github Pages** usa a branch *master* do seu repositório para servir o contúdo estático do seu blog, por isso o **Octopress** cria uma branch *source* que é onde o fonte do seu blog (os arquivos Markdown) dexando a branch *master* só com o HTML gerado a partir do fonte.

Existe uma rake task para fazer tudo isso:

```
rake setup_github_pages
```

Ela vai:

1. Perguntar a url do seu repositório **Github Pages**
2. Renomear a fonte remota que aponta para o repositório git do octopress de *origin* para *octopress*
3. Adicionar seu repositório **Github Pages** como a fonte remota *origin*
4. Mudar para a branch local *source*
5. Configurar a url do seu blog de acordo com o seu repositório
6. Configrar a branch *master* no diretório `_deploy`


Para gerar o blog:

```
rake generate
rake deploy
```

Os arquivos gerados serão copiados para o diretório `_deploy`. Eles serão adicionados no git e evniados para seu repositório remoto na branch *master*. Assim que seu site estiver pronto o Github irá te enviar um email avisando, basta acessar `http://<seu_usuario_github>.github.com`.

Não se esqueça de sempre commitar os fontes:

```
git add .
git commit -m 'commit message'
git push origin source
```

É isso. Para mais informações sobre o **Octopress**, como configurar se blog e como escrever posts acesse [http://octopress.org/](http://octopress.org/).