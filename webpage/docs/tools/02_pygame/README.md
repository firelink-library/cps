---
sidebar_position: 2
slug: /pygame
---

# PyGame

Nesta seção, vamos abordar o que é o PyGame e como ele pode ser utilizado como uma forma de realizar as visualizações nas nossas interações.

## O que é Pygame?

**Pygame** é uma biblioteca multiplataforma de código aberto para Python, projetada para criar jogos e aplicações multimídia interativas. Ela fornece funcionalidades para manipulação de gráficos, áudio, entrada de dispositivos (teclado, mouse, joystick) e muito mais. Legal, até aqui texto bem padrão né? Falando bem do projeto e tudo mais. Mas vamos por partes, primeiro o site do projeto: [https://www.pygame.org/](https://www.pygame.org/).


<img src="https://www.pygame.org/docs/_images/pygame_logo.png" style={{ display: 'block', marginLeft: 'auto', maxHeight: '60vh', marginRight: 'auto' }}/>
<br/>


### Principais características:

- **Gráficos 2D**: Desenho de formas, sprites, imagens e animações
- **Áudio**: Reprodução de sons e música
- **Input**: Captura de eventos de teclado, mouse e joystick
- **Colisão**: Detecção de colisões entre objetos
- **Simplicidade**: API fácil de usar e aprender
- **Performance**: Otimizado para jogos e simulações em tempo real
- **Escrito em Python**: Ok, para um projeto que se chama *PyGame* essa é bem direta, mas não custa avisar 🐍

### Por que usar Pygame?

Pygame é uma excelente ferramenta para:
- **Visualizações interativas**: Criar representações visuais de algoritmos e simulações (esse é o ponto que nos interessa mais aqui!!)
- **Prototipagem rápida**: Desenvolver interfaces gráficas simples e eficientes
- **Aprendizado**: Entender conceitos de programação gráfica e game development
- **Simulações**: Modelar sistemas físicos, biológicos ou outros fenômenos
- **Projetos educacionais**: Demonstrar conceitos de forma visual e interativa

:::tip[Introdução Padrão]

Pessoal até aqui uma introdução padrão da ferramenta. Nesse momento, minha sugestão: olhar a documentação.

> *Caramba Murilão! Mais obvío impossível não é mesmo?*

Bem colocado, mas esse pode ser uma prática que ainda não está muito clara. Quando vendo um projeto novo, muitas vezes, a documentação do projeto é tudo que teremos! Muitas vezes, vamos seguir pelo que está na documentação do projeto e na sequencia e temos que verificar o GitHub do projeto.

> *Caramba Murilão, mas você recomenda fazer isso em todo projeto?*

Aqui a resposta é depende. Porque sugiro um depende aqui. Se você já tem prática como pessoa desenvolvedora, beleza! Bora! Contudo, muitas vezes você pode estar iniciando no desenvolvimento. E algumas documentações são muito **duras**, no sentindo que não são simples de se compreender. 
Então, minha sugestão, tentar iniciar pela documentação. Se tiver exemplo, tentar seguir eles. Reproduzir mesmo. Depois, mas só depois de ter colocado os exemplos e outros projetos para rodar, colocar suas modificações.

> *Isso sempre funciona Murilo?*

Esse é um ponto complexo, eu recomendo sempre testar diferentes abordagens. Minha sugestão, contudo, é escrever qual o seu objetivo com a seção de estudo atual. Dessa forma, isso evita ficar perdido em um monte de frameworks e sem um objetivo final se aproximando daquela seção.

Esse ponto já ficou bem longo, vou colocando mais observações ao longo do texto!

<img src="https://i.pinimg.com/originals/6e/53/b4/6e53b476eb8595e5e2229a818fd9d95a.jpg" style={{ display: 'block', marginLeft: 'auto', maxHeight: '60vh', marginRight: 'auto' }}/>
<br/>

:::

## Instalação e Configuração

Quando trabalhando com um pacote em Python, que é o nosso caso aqui, recomendo sempre separar ele da sua instalação principal de Python. Existem diversas formas de fazer isso (vou sugerir olhar esse projeto aqui (uv)[https://github.com/astral-sh/uv]). 

Mas vamos seguir com o mais básico e utilizar o `venv`. Ele é um módulo padrão com a instalação do Python, desde a versão 3.3, que nos permite criar um ambiente virtual separado para utilizar no nosso projeto.

:::tip[venv]

Sugestão de vídeo:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Y21OR1OPC9A?si=9KN9ZhIi6lJ-NoXl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen style={{ display: 'block', marginLeft: 'auto', maxHeight: '60vh', marginRight: 'auto' }}></iframe>

:::

### 1. Criando um ambiente virtual (venv)

Primeiro, vamos criar um ambiente virtual isolado para o projeto:

```bash
# Criar o ambiente virtual
# O nome do diretório do venv fica a sua escolha, aqui foi utilizado o pygame_env
python3 -m venv pygame_env

# Ativar o ambiente virtual
# No macOS/Linux:
source pygame_env/bin/activate

# No Windows:
# pygame_env\Scripts\activate
```

### 2. Instalando Pygame

Com o ambiente virtual ativado, instale o Pygame:

```bash
pip install pygame
```

Para verificar a instalação:

```bash
python -c "import pygame; print(pygame.version.ver)"
```

Você deve ver no terminal a seguinte saída:

```sh
pygame 2.6.1 (SDL 2.28.4, Python 3.13.1)
Hello from the pygame community. https://www.pygame.org/contribute.html
2.6.1
```
### 3. Estrutura básica de um projeto

Aqui gente, vamos analisar como pode ser a estrutura básica para um projeto utilizando o PyGame.

> `Calma lá Murilão, precisamos mesmo de tudo isso para iniciar nosso projeto com o PyGame?`

Na verdade não, podemos iniciar apenas com um arquivo. Por sinal, vamos fazer isso então, antes de iniciar com nossa estrutura de diretórios e o restante, vamos primeiro só com um arquivo `00_ola_pygame.py`:

```python
import pygame
import sys

# Inicializar Pygame
pygame.init()

# Configurações da janela
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Meu Primeiro Pygame")

# Cores (RGB)
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Loop principal
clock = pygame.time.Clock()
running = True

while running:
    # Processar eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                running = False
    
    # Limpar a tela
    screen.fill(BLACK)
    
    # Desenhar um círculo
    pygame.draw.circle(screen, RED, (500, 500), 50)
    
    # Desenhar um retângulo
    pygame.draw.rect(screen, BLUE, (350, 250, 100, 100))
    
    # Atualizar a tela
    pygame.display.flip()
    
    # Controlar FPS (60 frames por segundo)
    clock.tick(60)

# Encerrar Pygame
pygame.quit()
sys.exit()
```

Vamos lá! Uma coisa de cada vez que aconteceu um montão de coisa agora nesse bloco. Primeiro, podemos executar ele com o comando:

```bash
python main.py
```

Vai aparecer uma tela com o nosso sistema em execução, fundo da cor preta, um retangulo azul e um circulo vermelho!

> `Caramba Murilão, você ainda vai explicar, mas não deveria ser divertido?`

Bem pontuado, isso não está la nada divertido, mas vai servir para conseguirmos analisar algumas coisas da ferramenta e depois vamos melhorando ela.

Bom, primeiro nos importamos tanto o **pygame** quando o **sys**. O **pygame** para utilizarmos a biblioteca e o **sys** para terminarmos nossa aplicação. Por sinal, se você pressionar a tecla `esc`, o programa fecha (ahhh olha ai a interatividade!!! Eu sei, ainda está ruim).

O `pygame.init()` inicializa o módulo do PyGame e já prepara os drivers para que som, vídeo e eventos possam ser utilizados por nossa aplicação. Na sequencia, vamos criar a janela da aplicação. Vamos ver o trecho do código:

```python
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Meu Primeiro Pygame")
```

A resolução da tela é definida para 800x600. Um objeto `screen` é criado. Ele vai receber toda manipulação gráfica, elementos serão desenhados nele. Por fim, definimos o título da nossa aplicação.

As cores no PyGame são definidas como Tuplas de Python, contendo o valor em 0 e 255 das componentes RGB da cor. As constantes são definidas para ficar mais simples de estabelecer cada uma das cores.

Agora, dois controles do jogo são criados: **clock** e **running**. A variável **clock** controla o FPS da aplicação, ela vai dizer de quanto em quanto tempo o game loop do nosso jogo vai ser atualizado. Já a variável **running** é definida pelo usuário como uma flag para sinalizar o estado atual do jogo.

Os eventos analizados dentro do Game Loop são:

```python
for event in pygame.event.get():
    if event.type == pygame.QUIT:
        running = False
    elif event.type == pygame.KEYDOWN:
        if event.key == pygame.K_ESCAPE:
            running = False
```

Aqui o jogo verifica:
- Se a janela foi fechada → finaliza (pygame.QUIT)
- Se uma tecla foi pressionada (KEYDOWN)
- Se essa tecla é ESC → também encerra o programa

Isso impede que a janela trave ou fique congelada.

Agora vamos para a lógica de desenhar os elementos na tela. Primeiro precisamos apagar a tela anterior, para que o que já estava desenhado na tela não atrapalhe o que está sendo renderizado. Vou colocar alguns comentários no trecho de código só para ficar mais fácil de acompanhar:

```python
screen.fill(BLACK) # Limpa o frame anterior
pygame.draw.circle(screen, RED, (500, 500), 50) #screen é onde o desenho é realizado, com a cor RED, na posição (500,500), com raio de 50 px
pygame.draw.rect(screen, BLUE, (350, 250, 100, 100)) #screen e BLUE, idem anterior. (350,250) coordenada do canto superior esquerdo, (100,100) - largura e altura do retangulo
```

Agora para desenhar tudo isso na tela, temos o `pygame.display.flip()`. Ele é responsável por atualizar o que estamos fazendo com a interface que está sendo desenhada na tela.

Antes de fecharmos, temos um ponto muito importante quanto ao seu funcionamento:

```python
clock.tick(60)
```

O que essa linha está fazendo é muito interessante e importante. 

> `Caramba Murilão, aqui está bem de boas, o programa roda 60 vezes por segundo! Simples!`

Então, esse ponto é um pouco mais complexo que parece. Sim, o objetivo desta linha é fazer o programa rodar até 60 vezes por segundo. O que isso significa? Dentro do nosso loop principal, o PyGame vai manter os eventos sendo observados, as animações acontecendo e vai verificar quanto tempo as interações estão levanto. Se ela levar mais que o tempo para garantir que o FPS determinado aconteça, ele simplesmente vai para o próximo passo do loop, caso contrário ele segura sua execução para limitar a sua velocidade de atualização.

:::tip[Frames demorados]

> `Mas Murilão, o que acontece se alguma ação dentro do game loop demorar demais?`

Ótima pergunta! Vou colocar aqui uma descrição mais simples da resposta, depois vou colocar mais detalhes dela!

Exemplo:

Você quer 60 FPS → cada frame tem ~16,67 ms disponíveis.

Mas, em um frame específico…
- você acessa disco,
- e/ou faz uma requisição,
- e/ou roda uma lógica muito pesada,

💥 E esse frame leva 100 ms para terminar.

⛓️ O que acontece com o clock.tick(60)?

✔️ Ele NÃO trava esperando

✔️ Ele NÃO tenta recuperar o tempo perdido

✔️ Ele NÃO acelera frames futuros

O que ele faz é muito simples:

👉 Ele tenta limitar para no máximo 60 FPS

👉 MAS se um frame já passou desse tempo, ele não retroage.

🧠 Em resumo:

✔️ Se o frame atrasou, o atraso acontece.

✔️ O loop NÃO é pausado depois disso.

✔️ O jogo não "compensa" o tempo perdido.

✔️ O próximo frame tenta manter 60 FPS normalmente (se possível).

:::

Caramba! Já falamos um monte e nosso projeto ainda está vem simples. Agora que já entendemos como criar e jogar nosso primeiro jogo, que ainda é bem básico, vamos avançar com o projeto. Aqui vai uma estrutura de diretórios para organizar melhor ele. Denovo, é uma recomendação de estrutura, ajustar de acordo com o projeto que estiver sendo desenvolvido.

Crie uma estrutura de diretórios organizada:

```
projeto_pygame/
├── pygame_env/          # Ambiente virtual (não versionar)
├── src/
│   ├── main.py         # Arquivo principal
│   └── utils/          # Funções auxiliares
├── assets/
│   ├── images/         # Imagens do projeto
│   └── sounds/         # Sons e músicas
└── requirements.txt    # Dependências do projeto
```

Crie um arquivo `requirements.txt`:

```txt
pygame>=2.5.0
```

Esse arquivo é o que permite que outras pessoas possam clonar seu repositório e instalar as dependências para executar ele.

## Exemplos Práticos

### Exemplo 1: Janela Básica

Crie um arquivo `main.py` com o seguinte código:

```python
import pygame
import sys

# Inicializar Pygame
pygame.init()

# Configurações da janela
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Meu Primeiro Pygame")

# Cores (RGB)
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Loop principal
clock = pygame.time.Clock()
running = True

while running:
    # Processar eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                running = False
    
    # Limpar a tela
    screen.fill(BLACK)
    
    # Desenhar um círculo
    pygame.draw.circle(screen, RED, (400, 300), 50)
    
    # Desenhar um retângulo
    pygame.draw.rect(screen, BLUE, (350, 250, 100, 100))
    
    # Atualizar a tela
    pygame.display.flip()
    
    # Controlar FPS (60 frames por segundo)
    clock.tick(60)

# Encerrar Pygame
pygame.quit()
sys.exit()
```

Execute o programa:

```bash
python main.py
```

### Exemplo 2: Objeto Móvel com Controle

```python
import pygame
import sys

pygame.init()

WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Objeto Móvel")

# Cores
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)

# Posição e velocidade do objeto
x, y = WIDTH // 2, HEIGHT // 2
speed = 5
radius = 30

clock = pygame.time.Clock()
running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    
    # Capturar teclas pressionadas
    keys = pygame.key.get_pressed()
    
    # Movimento com setas ou WASD
    if keys[pygame.K_LEFT] or keys[pygame.K_a]:
        x -= speed
    if keys[pygame.K_RIGHT] or keys[pygame.K_d]:
        x += speed
    if keys[pygame.K_UP] or keys[pygame.K_w]:
        y -= speed
    if keys[pygame.K_DOWN] or keys[pygame.K_s]:
        y += speed
    
    # Limitar dentro da tela
    x = max(radius, min(WIDTH - radius, x))
    y = max(radius, min(HEIGHT - radius, y))
    
    # Desenhar
    screen.fill(BLACK)
    pygame.draw.circle(screen, GREEN, (int(x), int(y)), radius)
    
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
```
Nesse segundo exemplo, a bolinha verde é movimentada com as teclas *WASD*. Repare que a tecla `esc` não está mais fechando o jogo, tente fazer essa implementação.

### Exemplo 3: Classe de Sprite

Agora, vamos carregar uma imagem para nosso projeto. Para isso, vai ser necessário criar uma classe dentro do `utils` que servirá para resolver o nome do nosso arquivo. Por que isso é necessário? Para que o nome dos arquivos possa ser resolvido de forma independente do sistema operacional.

Primeiro vamos para o nosso `src/utils/loader.py`:

```python
# src/utils/loader.py
import os
import pygame

def load_image(name: str) -> pygame.Surface:
    """Carrega uma imagem da pasta assets/images."""
    # Pasta onde está este arquivo: src/utils
    current_dir = os.path.dirname(__file__)
    # Sobe duas pastas: src/utils -> src -> projeto raiz
    project_root = os.path.join(current_dir, "..", "..")
    # Monta o caminho até assets/images/name
    image_path = os.path.join(project_root, "assets", "images", name)
    # Normaliza o caminho (resolve .. etc.)
    image_path = os.path.normpath(image_path)
    return pygame.image.load(image_path).convert_alpha()

```

Agora vamos ajustar nosso programa principal para utilizar o loader:

```python
import pygame
import sys
from utils.loader import load_image

pygame.init()

WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Sprites com Classes")

BLACK = (0, 0, 0)
RED = (255, 0, 0)

class Player(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = load_image("python.png")
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.speed = 5
    
    def update(self, keys):
        if keys[pygame.K_LEFT] or keys[pygame.K_a]:
            self.rect.x -= self.speed
        if keys[pygame.K_RIGHT] or keys[pygame.K_d]:
            self.rect.x += self.speed
        if keys[pygame.K_UP] or keys[pygame.K_w]:
            self.rect.y -= self.speed
        if keys[pygame.K_DOWN] or keys[pygame.K_s]:
            self.rect.y += self.speed
        
        # Manter dentro da tela
        self.rect.x = max(0, min(WIDTH - self.rect.width, self.rect.x))
        self.rect.y = max(0, min(HEIGHT - self.rect.height, self.rect.y))

# Criar jogador
player = Player(WIDTH // 2, HEIGHT // 2)
all_sprites = pygame.sprite.Group()
all_sprites.add(player)

clock = pygame.time.Clock()
running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    
    keys = pygame.key.get_pressed()
    
    # Atualizar sprites
    player.update(keys)
    
    # Desenhar
    screen.fill(BLACK)
    all_sprites.draw(screen)
    
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
```

### Exemplo 4: Detecção de Colisão

```python
import pygame
import sys
import random

pygame.init()

WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Detecção de Colisão")

BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((40, 40))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH // 2, HEIGHT // 2)
        self.speed = 5
    
    def update(self, keys):
        if keys[pygame.K_LEFT] or keys[pygame.K_a]:
            self.rect.x -= self.speed
        if keys[pygame.K_RIGHT] or keys[pygame.K_d]:
            self.rect.x += self.speed
        if keys[pygame.K_UP] or keys[pygame.K_w]:
            self.rect.y -= self.speed
        if keys[pygame.K_DOWN] or keys[pygame.K_s]:
            self.rect.y += self.speed
        
        self.rect.x = max(0, min(WIDTH - self.rect.width, self.rect.x))
        self.rect.y = max(0, min(HEIGHT - self.rect.height, self.rect.y))

class Obstacle(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.Surface((30, 30))
        self.image.fill(BLUE)
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

# Criar objetos
player = Player()
obstacles = pygame.sprite.Group()

# Criar obstáculos aleatórios
for _ in range(10):
    x = random.randint(0, WIDTH - 30)
    y = random.randint(0, HEIGHT - 30)
    obstacles.add(Obstacle(x, y))

all_sprites = pygame.sprite.Group()
all_sprites.add(player)
all_sprites.add(obstacles)

clock = pygame.time.Clock()
running = True
score = 0

# Fonte para texto
font = pygame.font.Font(None, 36)

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    
    keys = pygame.key.get_pressed()
    player.update(keys)
    
    # Detectar colisões
    hits = pygame.sprite.spritecollide(player, obstacles, True)
    score += len(hits)
    
    # Desenhar
    screen.fill(BLACK)
    all_sprites.draw(screen)
    
    # Mostrar pontuação
    text = font.render(f"Pontuação: {score}", True, RED)
    screen.blit(text, (10, 10))
    
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
```

## Sugestões de Uso

### 1. Visualizações de Algoritmos
- **Algoritmos de ordenação**: Visualizar como diferentes algoritmos ordenam dados
- **Árvores e grafos**: Representar estruturas de dados de forma interativa
- **Pathfinding**: Demonstrar algoritmos como A* ou Dijkstra

### 2. Simulações Físicas
- **Gravidade e movimento**: Simular física básica com objetos caindo
- **Partículas**: Criar sistemas de partículas para efeitos visuais
- **Colisões elásticas**: Modelar colisões entre objetos

### 3. Jogos Educacionais
- **Quiz interativo**: Criar jogos de perguntas e respostas
- **Puzzles**: Desenvolver quebra-cabeças educativos
- **Simuladores**: Modelar sistemas complexos de forma simplificada

### 4. Prototipagem de Interfaces
- **Menus interativos**: Criar interfaces de usuário simples
- **Dashboards**: Visualizar dados em tempo real
- **Ferramentas de desenho**: Aplicações de desenho básicas

### 5. Projetos de Pesquisa
- **Visualização de dados**: Representar dados científicos
- **Modelos matemáticos**: Visualizar funções e equações
- **Animações**: Criar animações para apresentações

## Dicas e Boas Práticas

1. **Organize seu código**: Use classes e funções para manter o código limpo
2. **Use sprites**: A classe `Sprite` do Pygame facilita o gerenciamento de objetos
3. **Controle de FPS**: Use `clock.tick()` para manter performance consistente
4. **Gerenciamento de recursos**: Carregue imagens e sons uma vez, reutilize depois
5. **Tratamento de eventos**: Sempre verifique eventos de saída (QUIT)
6. **Documentação**: Comente seu código, especialmente lógica complexa

## Recursos Adicionais

- **Documentação oficial**: [pygame.org/docs](https://www.pygame.org/docs/)
- **Tutoriais**: [pygame.org/tags/tutorial](https://www.pygame.org/tags/tutorial)
- **Exemplos**: [pygame.org/tags/example](https://www.pygame.org/tags/example)
- **Comunidade**: [Reddit r/pygame](https://www.reddit.com/r/pygame/)

## Conclusão

Pygame é uma ferramenta poderosa e acessível para criar visualizações interativas, simulações e jogos. Com sua API simples e documentação extensa, é ideal tanto para iniciantes quanto para projetos mais complexos. Experimente os exemplos acima e adapte-os às suas necessidades!
