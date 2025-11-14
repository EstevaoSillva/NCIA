# Instalação

Para executar estes notebooks, **não é necessário instalar nada**: você pode rodá-los direto no navegador usando o Google Colab:

* <a href="https://colab.research.google.com/github/ageron/handson-mlp/blob/main/" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Abrir no Colab"/></a> (recomendado)

Outras plataformas como Kaggle ou Binder também devem funcionar.

No entanto, se você realmente quiser rodar os notebooks na sua própria máquina, siga as instruções abaixo.

> **OBS:** Como alternativa, você pode [rodar estes notebooks em um contêiner Docker](https://github.com/ageron/handson-mlp/blob/main/docker/README.md).

## Passo 1 — garanta que você tem `uv` e `git`

Certifique-se de ter o [uv](https://docs.astral.sh/uv/getting-started/installation) e o [git](https://git-scm.com/downloads) instalados.

Você pode usar outro gerenciador de pacotes Python se souber o que está fazendo, mas o `uv` é muito rápido e simples, então é o que recomendamos.

## Passo 2 — instalar o projeto e as dependências

Vamos baixar o projeto `handson-mlp` para o seu diretório pessoal, instalar todas as bibliotecas Python exigidas em um novo ambiente virtual (venv) e tornar esse ambiente o padrão para os notebooks Jupyter. Abra um terminal e rode:

```shell
cd                      # vai para o diretório HOME do seu usuário (~)

git clone https://github.com/ageron/handson-mlp.git
# baixa o repositório do GitHub para uma pasta 'handson-mlp'

cd handson-mlp          # entra na pasta do projeto

uv sync --frozen        # cria .venv e instala dependências do projeto
                        # (--frozen garante versões exatamente como no lockfile)

# Em macOS ou Linux, ative o ambiente virtual:
source .venv/bin/activate
# 'source' carrega as variáveis do venv atual no shell

# No Windows usando o Prompt de Comando (cmd.exe):
# (execute este comando no cmd.exe, não no PowerShell)
.\.venv\Scripts\activate.bat
# ativa o venv no cmd clássico

# No Windows usando o PowerShell:
.\.venv\Scripts\Activate.ps1
# ativa o venv no PowerShell

python -m ipykernel install --user --name=python3
# registra este venv como kernel "python3" no Jupyter (padrão)
```

Se aparecer erro ao ativar o venv no PowerShell, talvez seja preciso ajustar a política de execução **apenas para essa janela**:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
# permite executar o script de ativação nesta sessão do PowerShell
```

## Passo 3 — se você tiver GPU, atualize as bibliotecas do PyTorch

Se você tem uma GPU, garanta que o driver está instalado corretamente (consulte a documentação do fabricante).

Em seguida, atualize as bibliotecas do PyTorch para habilitar suporte à GPU:

```shell
cd                      # volta ao HOME por conferência
cd handson-mlp          # entra no projeto
source .venv/bin/activate   # no Windows, use o comando de ativação do Passo 2

uv pip uninstall torch torchvision torchaudio
# remove os pacotes CPU padrão do PyTorch instalados pelo sync

# Para uma GPU Nvidia com CUDA 12.9 (ajuste 'cu129' conforme sua versão CUDA):
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu129
# instala as wheels do PyTorch compatíveis com sua CUDA a partir do repositório oficial
```

**OBS:** Se você tem MacBook com suporte a MPS ([Metal Performance Shaders](https://developer.apple.com/documentation/metalperformanceshaders)), não precisa atualizar o PyTorch: as versões padrão já suportam MPS.

## Como iniciar e parar o servidor Jupyter Notebook

Sempre que quiser iniciar o servidor Jupyter Notebook, abra um terminal e rode:

```shell
cd                      # garante que está no HOME
cd handson-mlp          # entra no projeto
source .venv/bin/activate   # no Windows, use o comando equivalente do Passo 2
jupyter notebook        # inicia o servidor Jupyter; abrirá no navegador
```

Para parar o servidor, pressione **Ctrl+C** na janela do terminal (salve suas mudanças antes!).

## Softwares e bibliotecas opcionais

Você pode opcionalmente instalar:

* [ffmpeg](https://www.ffmpeg.org/download.html): usado no tutorial de Matplotlib para exportar animações em `.mp4`.
* [ImageMagick](https://imagemagick.org/): usado no tutorial de Matplotlib para exportar animações em `.gif`.
* [Graphviz](https://graphviz.org/): usado no Capítulo 5 para gerar imagens a partir de arquivos `.dot`.

Também pode ser interessante instalar a biblioteca **Box2D**, necessária para alguns exercícios do Capítulo 19 (ambientes LunarLander-v2 e BipedalWalker-v3). Em geral, instale assim:

```shell
cd
cd handson-mlp
source .venv/bin/activate    # no Windows, use o comando equivalente
uv add box2d                 # instala a dependência 'box2d' no pyproject/venv
```

Isso deve funcionar na maioria dos sistemas. Se aparecer erro, provavelmente não há binário pronto para sua plataforma; nesse caso você terá que compilar a partir do código-fonte. Primeiro, garanta as ferramentas essenciais de C++ (exemplo em Debian/Ubuntu):

```shell
apt update && apt install -y build-essential cmake
# atualiza a lista de pacotes e instala compiladores e o CMake
```

Depois instale o pacote `box2d-py` (não `box2d`) a partir do código:

```shell
cd
cd handson-mlp
source .venv/bin/activate    # ative seu venv
uv add swig                  # SWIG é necessário para gerar bindings
uv add box2d-py              # instala a versão Python que compila localmente
```

## Atualizar o projeto e suas bibliotecas

Os notebooks são atualizados regularmente (correções e novos suportes). Atualize periodicamente.

Para isso, abra um terminal e rode:

```shell
cd
cd handson-mlp
git pull
# busca as últimas mudanças do repositório remoto
```

Se aparecer erro, provavelmente você alterou algum notebook. Antes de `git pull`, faça commit das suas mudanças — de preferência em um *branch* próprio para evitar conflitos:

```shell
git checkout -b my_branch          # cria e muda para um novo branch
git add -u                         # adiciona arquivos alterados/renomeados/removidos
git commit -m "descreva suas mudanças aqui"  # cria um commit com sua mensagem
git checkout main                  # volta para o branch principal
git pull                           # puxa as últimas alterações do remoto
```

Depois, atualize as bibliotecas:

```shell
cd
cd handson-mlp
source .venv/bin/activate  # no Windows, use o comando equivalente
uv sync --frozen           # sincroniza dependências exatamente como no lockfile
```

Reinicie o servidor Jupyter Notebook (pode não ser estritamente necessário, mas é mais seguro).

Pronto! Divirta-se.
