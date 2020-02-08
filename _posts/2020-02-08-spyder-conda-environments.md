---
layout: post
title: "Usando spyder com conda environments"
author: "Neuremberg de Matos"
categories: infraestrura
tags: [python, conda]
image: ./spyder-conda-environments/conda-blog.jpg
---

Até recentemente sabia muito pouco o porque usar o `conda`. Para quem não sabe `conda` é um gerenciador de pacotes para o python e ao mesmo tempo um gerenciador de de ambientes virtuais. Enquanto o `pip` é um gerenciador de pacote e `pyenv` é um gerenciador de ambientes virtuais, o `conda` faz os dois papeis.

A pergunta que surge é: porque usar ambientes virtuais? Vou dar duas razões: 1) isolar dependências, 2) reprodutibilidade.

Suponha que você esteja trabalhando em dois projeto diferentes, digamos A e B, o projeto A depende do pacote _x_ na versão 1.0 e projeto B depende do mesmo pacote na versão 0.9. Neste caso, não é possível trabalhar em um projeto sem quebrar o outro, se você estiver trabalhando no mesmo ambiente virtual. Com o `conda` você pode criar um ambiente para cada projeto, assim não haverá interações entre eles.

Considerando o segundo motivo, dado que você está trabalhando em um ambiente virtual, é possível exportar todas as dependências para um arquivo `.yml` e compartilhá-lo. Assim, outra pessoa poderá reproduzir o mesmo ambiente em sua própria máquina.

## Primeiros passos com conda


Partiremos do pressuposto de que você já tem o _conda_ instalado na sua máquina. Eu usei uma instalação do spyder e python da distribuição anaconda no Windows 7 para testar esses passos. Para instalar, a distribuição Anacoda do python use link [link](https://www.anaconda.com/distribution/) para baixar o instalador esse [tutorial](https://lamfo-unb.github.io/2017/06/10/Instalando-Python/) da passo a passo para a instalação.

Para iniciar, abra o anacoda prompt. O console aparecerá piscando com a seguinte linha de comando.

```
(base) C:\Users\nome_usuario>
```
O parte `(base) ` significa que o estamos no ambiente base ou raiz da sua instalação _conda_. Para criar um novo a ambiente usamos os seguinte comando:

```
conda create --name my_env python=3.7
```

Neste caso, estamos criando um ambiente cujo o nome é "my_env". A parte `python=3.7` significa que vamos usar o python 3.7 nesse ambiente. Essa comando é opcional, caso o omitimos o ambiente será iniciado com a mesma versão do python instalado no ambiente base.

Após criar o ambiente virtual, podemos ativá-lo usando:

```
conda activate my_env
```
Ou, se você estiver usando linux:

```
source activate my_env
```

Assim o console mudará para:

```
(my_env) C:\Users\nome_usuario>
```

Para  instalar pacotes no ambiente `my_env` usamos a conda normalmente, isto é:

```
(my_env) C:\Users\nome_usuario> conda install package_name
```

Para voltar para o ambiente base, rodamos o seguinte comando:

```
conda deactivate
```

Ou, para linux:

```
source deactivate
```

## Usando Spyder

O spyder é uma IDE para trabalhar com o python. Isto é, ela fornece um conjunto de facilidades como, auto completar, análise de sintaxe e etc que nos ajuda a ser mais produtivo programando.

Há duas formas de trabalhar com ambientes virtuais com spyder. A primeira é instalar o spider no ambiente que desejamos e depois iniciá-lo a partir deste ambiente. Isto é, seguir os seguintes passos:

```
conda activate my_env
conda install -c anaconda spyder=3
spyder
```

Entretanto, essa não é a melhor forma, uma vez que, para cada ambiente teríamos uma instalação para spyder diferente, a instalação pode demorar e quando formos compartilhar o nosso projeto a spyder estará lá desnecessariamente.

A segunda alternativa é mudar o caminho do interpretador usado pelo spyder. Neste caso, precisaríamos do spyder instalado em apenas um ambiente virtual, que pode ser o ambiente base.

Entretanto, ainda é necessário instalar o pacote `spyder-kernels` para que essa alternativa funcione.

Neste caso, os seguinte passos devem ser seguidos:

1 - Ativar o ambiente virtual que desejamos usar com o spyder:

```
conda activate my_env
```
2 - Installar `spyder-kernels`:

```
conda install spyder-kernels=0.*
```
3 - Rodar o seguinte comando para o obter o caminho do interpretador pyhton do ambiente:
```
python -c "import sys; print(sys.executable)"
```
copiar o caminho mostrado

4 - Rodar o spyder instalado na sua máquina. Caso haja uma instalação no ambiente base, desativamos a o ambiente `my_env` , voltamos para o `base` e abrimos o spyder como o comando `spyder`.

5 - Após isso, na barra superior do spyder, ir para `Ferramentas > Preferências > Interpretador python > Usar o seguinte interpretador` , colar o caminho obtido no passo 3 e dar um ok.

6 - Após isso reinicie o kernel do spyder. Isso pode ser feito com o atalho `Ctrl + .`

Agora é só aproveitar o seu novo ambiente.

Eu usei uma instalação do spyder e python da distribuição anaconda no Windows 7 para testar esses passos. Em todo caso, você pode encontrar a documentação do spyder sobre isso [aqui](https://github.com/spyder-ide/spyder/wiki/Working-with-packages-and-environments-in-Spyder).
