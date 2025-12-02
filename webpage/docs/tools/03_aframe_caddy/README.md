---
sidebar_position: 2
slug: /aframe
---

# AFrame e Caddy

Repositório criado para criar projetos com a biblioteca AFrame. Sua documentação pode ser encontrada [aqui](https://aframe.io/). Mais detalhes sobre o projeto podem ser vistos no site do projeto. Aqui vamos colocar nossas energias em utlizar essa ferramenta e como podemos utilizar ela com projetos de AR (Realidade Aumentada) e VR (Realidade Virtual).

## Por que aplicações WebVR precisam de HTTPS?

Aplicações WebVR exigem o uso de HTTPS devido a políticas de segurança implementadas pelos navegadores modernos. Essas políticas restringem o acesso a recursos sensíveis, como sensores de movimento, APIs de realidade virtual e dispositivos de entrada (como controles VR), apenas a contextos considerados **seguros** (secure contexts), ou seja, aqueles servidos por HTTPS.

### Requisitos de Segurança

1. **APIs Sensíveis**: As APIs WebVR e WebXR acessam recursos do sistema como:
   - Sensores de movimento e orientação do dispositivo
   - Câmeras e microfones
   - Dispositivos de entrada VR/AR
   - Dados de posicionamento espacial

2. **Políticas dos Navegadores**: Navegadores modernos (Chrome, Firefox, Safari, Edge) implementam a política de **Secure Contexts**, que exige HTTPS para:
   - Proteger dados do usuário contra interceptação
   - Prevenir ataques man-in-the-middle
   - Garantir integridade da comunicação entre cliente e servidor

3. **Experiência do Usuário**: Sem HTTPS, as aplicações WebVR simplesmente não funcionarão corretamente, pois as APIs necessárias estarão bloqueadas ou indisponíveis.

### Caddy: Uma Solução 

O [**Caddy**](https://caddyserver.com/) é um servidor web moderno que facilita significativamente a implementação de HTTPS para aplicações WebVR, oferecendo:

#### HTTPS Automático
- Obtém e renova certificados SSL/TLS automaticamente via **Let's Encrypt**
- Elimina a necessidade de configurações manuais complexas
- Funciona tanto em produção quanto em desenvolvimento local

#### Configuração Simplificada
- Sintaxe intuitiva no arquivo `Caddyfile`
- Configuração mínima necessária para começar
- Ideal para desenvolvimento rápido de aplicações WebVR

#### Performance e Modernidade
- Suporte nativo a **HTTP/2** e **HTTP/3** (QUIC)
- Melhor desempenho e tempos de carregamento mais rápidos
- Otimizado para aplicações web modernas

#### Proxy Reverso
- Facilita o roteamento de múltiplas aplicações
- Suporte a múltiplos domínios e subdomínios
- Configuração flexível para ambientes de desenvolvimento e produção

Ao utilizar o Caddy para servir aplicações WebVR, garantimos que elas operem em um ambiente seguro, atendendo às políticas de segurança dos navegadores e proporcionando uma experiência de usuário otimizada. A configuração automática de HTTPS elimina barreiras técnicas, permitindo que desenvolvedores se concentrem na criação de experiências imersivas em vez de configurações de servidor.

## Criando um servidor com Caddy

Ok Ok, vai parecer estranho, mas vamos primeiro criar um servidor com Caddy e depois vamos chegar no AFrame.

### Instalação do Caddy

Existem várias formas de instalar o Caddy, dependendo do seu sistema operacional e preferências:

#### macOS

**Usando Homebrew:**
```bash
brew install caddy
```

**Ou usando o instalador oficial:**
```bash
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
```

#### Linux

**Ubuntu/Debian:**
```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

**Ou usando o binário direto:**
```bash
wget https://caddyserver.com/api/download?os=linux -O caddy
chmod +x caddy
sudo mv caddy /usr/local/bin/
```

#### Windows

Baixe o binário diretamente do site oficial: [https://caddyserver.com/download](https://caddyserver.com/download)

Ou use o Chocolatey:
```powershell
choco install caddy
```

#### Verificando a instalação

Após instalar, verifique se o Caddy está funcionando:
```bash
caddy version
```

### Executando o Caddy em um Container Docker

Executar o Caddy em um container Docker oferece isolamento, portabilidade e facilidade de gerenciamento. Esta é uma abordagem recomendada para desenvolvimento e produção.

#### Pré-requisitos

Certifique-se de que o Docker está instalado e em execução no seu sistema:
```bash
docker --version
```

#### Executando o Caddy com Docker Run

A forma mais simples de executar o Caddy em um container:

```bash
docker run -d \
  --name caddy-server \
  -p 8080:80 \
  -p 4430:443 \
  caddy:latest
```

**Explicação dos parâmetros:**
- `-d`: Executa o container em segundo plano (modo detached)
- `--name caddy-server`: Define o nome do container
- `-p 8080:80 -p 4430:443`: Mapeia as portas HTTP (8080) e HTTPS (4430) do host para o container
- `caddy:latest`: Utiliza a imagem oficial mais recente do Caddy

Beleza! E agora devemos ter o que na tela? Nada!
Mas por que isso? Porque não estamos servindo nenhuma página. Vamos fazer o seguinte, vamos criar o arquivo `index.html` e vamos popular ele com o conteúdo:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World</title>
</head>
<body>
    <h1>Hello World!</h1>
    <p>Servido pelo Caddy</p>
</body>
</html>
```

Agora, vamos colocar essa arquivo como o arquivo. Vamos parar e remover o nosso container:

```sh
docker stop caddy-server
docker container rm caddy-server
```

A vamos rodar ele:

```sh
docker run -d --rm\
  --name caddy-server \
  -p 8080:80 \
  -p 4430:443 \
  -v $(pwd):/usr/share/caddy \
  caddy:latest
```

Boa! Agora temos nosso servidor funcionando na porta 8080 (mapeada internamente para porta 80), mas apenas no protocolo HTTP. O que será que precisamos fazer para conseguirmos servir pelo protocolo HTTPS, na porta 4430 (mapeado para 443)? Nós precisamos de um `Caddyfile` para que o Caddy possa servir na porta. Vamos criar esse arquivo e salvar ele como *Caddyfile*:

```yml
:80 {
    root * /usr/share/caddy
    file_server
}

:443 {
    root * /usr/share/caddy
    file_server
    tls internal {
        on_demand
    }
}
```

O que aconteceu aqui:
- **:80 e :443**: configura as portas HTTP e HTTPS
- **root * /usr/share/caddy**: define o diretório raiz
- **file_server**: habilita o servidor de arquivos estáticos
- **tls internal**: gera certificados autoassinados para desenvolvimento local

Atualizando nosso comando de execução para o Docker, aproveitando para parar a execução anterior:

```sh
# Executar se o container ainda estiver rodando
docker stop caddy-server
# Esse é o comando que vai rodar nosso container
docker run -d --rm \
  --name caddy-server \
  -p 8080:80 \
  -p 4430:443 \
  -v $(pwd)/Caddyfile:/etc/caddy/Caddyfile \
  -v $(pwd):/usr/share/caddy \
  -v caddy_data:/data \
  -v caddy_config:/config \
  caddy:latest
```

Legal agora temos nossa página sendo servida tanto localmente por http quanto por https! Agora vamos voltar para o AFrame!

Observação Importante: O Caddy pode fazer muito mais que isso ainda! Minha recomendação, invistam um tempo avaliando o que a solução pode fazer!

## WebVR e AFrame: Uma Introdução

### O que é WebVR?

**WebVR** (Web Virtual Reality) é uma especificação de API que permite criar experiências de realidade virtual diretamente no navegador web, sem necessidade de instalar aplicativos nativos. Com WebVR, é possível:

- Criar experiências imersivas acessíveis através de qualquer navegador moderno
- Utilizar dispositivos VR como Oculus Rift, HTC Vive, Google Cardboard, Daydream e outros
- Desenvolver aplicações que funcionam em desktop, mobile e dispositivos VR dedicados
- Compartilhar experiências VR facilmente através de URLs

A WebVR foi sucedida pela **WebXR**, que expande as capacidades para incluir também realidade aumentada (AR) e realidade mista (MR), mas os conceitos fundamentais permanecem os mesmos.

### O que é AFrame?

**[AFrame](https://aframe.io/)** é um framework web de código aberto desenvolvido pela Mozilla para criar experiências de realidade virtual usando HTML. Ele simplifica drasticamente o desenvolvimento de aplicações WebVR/WebXR através de:

- **Sintaxe baseada em HTML**: Utiliza tags HTML declarativas, tornando o desenvolvimento acessível mesmo para iniciantes
- **Sistema de entidades**: Baseado no padrão Entity-Component-System (ECS), permitindo composição flexível de objetos 3D
- **Componentes reutilizáveis**: Biblioteca rica de componentes prontos (geometria, materiais, animações, física, etc.)
- **Performance otimizada**: Construído sobre Three.js, oferecendo renderização 3D de alta performance
- **Cross-platform**: Funciona em desktop, mobile e dispositivos VR sem modificações

Com AFrame, você pode criar cenas 3D complexas escrevendo apenas HTML, sem necessidade de conhecimento profundo em WebGL ou JavaScript avançado.

## Exemplos Básicos para Sala de Aula

A seguir, apresentamos exemplos progressivos de VR básica usando AFrame, ideais para introduzir os conceitos em sala de aula.

### Exemplo 1: Cena VR Básica - Hello World 3D

Este é o exemplo mais simples: uma cena VR com um cubo colorido no centro.

**Arquivo: `exemplos/01-hello-world.html`**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AFrame - Hello World</title>
    <script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
</head>
<body>
    <a-scene>
        <!-- Câmera VR -->
        <a-camera position="0 1.6 0"></a-camera>
        
        <!-- Cubo vermelho no centro -->
        <a-box 
            position="0 1 -3" 
            rotation="0 45 0" 
            color="#FF5733"
            shadow>
        </a-box>
        
        <!-- Chão -->
        <a-plane 
            position="0 0 -4" 
            rotation="-90 0 0" 
            width="10" 
            height="10" 
            color="#7BC8A4"
            shadow>
        </a-plane>
        
        <!-- Céu -->
        <a-sky color="#87CEEB"></a-sky>
        
        <!-- Iluminação -->
        <a-light type="ambient" color="#404040"></a-light>
        <a-light type="directional" position="0 5 5" intensity="0.5"></a-light>
    </a-scene>
</body>
</html>
```

**Conceitos apresentados:**
- Estrutura básica de uma cena AFrame (`<a-scene>`)
- Câmera VR (`<a-camera>`)
- Geometria básica (`<a-box>`)
- Planos e ambiente (`<a-plane>`, `<a-sky>`)
- Iluminação básica (`<a-light>`)

#### Entendendo o Posicionamento no AFrame

O AFrame utiliza um **sistema de coordenadas 3D** baseado em metros, similar ao mundo real. É muito imporante entender como funciona esse sistema para posicionar objetos corretamente na cena.

##### Sistema de Eixos (X, Y, Z)

O sistema segue a **regra da mão direita**:

```
     Y (cima/baixo)
     |
     |
     |____ X (esquerda/direita)
    /
   /
  Z (frente/trás - profundidade)
```

- **Eixo X**: Esquerda (-) ↔ Direita (+)
- **Eixo Y**: Baixo (-) ↔ Cima (+)
- **Eixo Z**: Atrás (-) ↔ Frente (+)

##### Origem (0, 0, 0)

A **origem** `(0, 0, 0)` é o centro da cena. Todos os objetos são posicionados em relação a esse ponto.

##### Formato de Posicionamento

O atributo `position` sempre usa três valores separados por espaço:

```html
position="X Y Z"
```

**Exemplos do código acima:**

```html
<!-- Câmera na origem, 1.6m acima do chão -->
<a-camera position="0 1.6 0"></a-camera>
<!-- X=0 (centro), Y=1.6 (altura de olhos), Z=0 (na origem) -->

<!-- Cubo: centro horizontal, 1m acima, 3m à frente -->
<a-box position="0 1 -3"></a-box>
<!-- X=0 (centro), Y=1 (1 metro acima), Z=-3 (3 metros à frente) -->

<!-- Chão: centro horizontal, no nível do chão, 4m à frente -->
<a-plane position="0 0 -4"></a-plane>
<!-- X=0 (centro), Y=0 (no chão), Z=-4 (4 metros à frente) -->
```

##### Por que a câmera está em `(0, 1.6, 0)`?

- `Y = 1.6`: Representa aproximadamente a altura dos olhos de uma pessoa (1,6 metros)
- `X = 0` e `Z = 0`: Posiciona a câmera na origem, olhando para frente

##### Por que objetos ficam em `Z = -3` ou `Z = -4`?

Valores **negativos de Z** colocam objetos **à frente** da câmera. Se estivessem em `Z = 0`, estariam na mesma posição da câmera e não seriam visíveis. Valores negativos maiores (como `-5`) colocam objetos mais distantes.

##### Unidades

- As unidades são sempre em **metros**
- `position="1 2 3"` significa: 1 metro à direita, 2 metros acima, 3 metros à frente
- Um cubo com `width="1"` tem exatamente 1 metro de largura

##### Dicas Práticas

1. **Altura do chão**: Objetos no chão geralmente têm `Y = 0` ou um pouco acima (ex: `Y = 0.5` para esferas)
2. **Distância da câmera**: Use valores negativos de `Z` para colocar objetos à frente (ex: `-3` a `-5` metros)
3. **Organização horizontal**: Use valores negativos de `X` para esquerda e positivos para direita
4. **Altura de visualização**: `Y = 1.6` é uma boa altura padrão para a câmera (altura dos olhos)

##### Experimente Modificar

Tente alterar os valores de posição no exemplo acima para ver as diferenças:

```html
<!-- Original -->
<a-box position="0 1 -3"></a-box>

<!-- Experimente: -->
<a-box position="2 1 -3"></a-box>    <!-- Move para direita -->
<a-box position="0 3 -3"></a-box>    <!-- Move para cima -->
<a-box position="0 1 -1"></a-box>    <!-- Move mais perto -->
<a-box position="0 1 -5"></a-box>    <!-- Move mais longe -->
```

### Exemplo 2: Múltiplas Formas Geométricas

Demonstra diferentes formas geométricas 3D disponíveis no AFrame.

**Arquivo: `exemplos/02-formas-geometricas.html`**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AFrame - Formas Geométricas</title>
    <script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
</head>
<body>
    <a-scene>
        <a-camera position="0 1.6 0"></a-camera>
        
        <!-- Cubo -->
        <a-box 
            position="-3 1 -3" 
            width="1" 
            height="1" 
            depth="1"
            color="#FF5733"
            shadow>
        </a-box>
        
        <!-- Esfera -->
        <a-sphere 
            position="-1 1 -3" 
            radius="0.5"
            color="#33FF57"
            shadow>
        </a-sphere>
        
        <!-- Cilindro -->
        <a-cylinder 
            position="1 1 -3" 
            radius="0.5" 
            height="1"
            color="#3357FF"
            shadow>
        </a-cylinder>
        
        <!-- Cone -->
        <a-cone 
            position="3 1 -3" 
            radius-bottom="0.5" 
            height="1"
            color="#FF33F5"
            shadow>
        </a-cone>
        
        <!-- Chão -->
        <a-plane 
            position="0 0 -4" 
            rotation="-90 0 0" 
            width="10" 
            height="10" 
            color="#7BC8A4"
            shadow>
        </a-plane>
        
        <a-sky color="#87CEEB"></a-sky>
        <a-light type="ambient" color="#404040"></a-light>
        <a-light type="directional" position="0 5 5" intensity="0.5"></a-light>
    </a-scene>
</body>
</html>
```

**Conceitos apresentados:**
- Diferentes primitivas 3D (box, sphere, cylinder, cone)
- Posicionamento de objetos no espaço 3D
- Propriedades de geometria (radius, width, height, depth)

### Exemplo 3: Interatividade Básica - Clique e Rotação

Adiciona interatividade através de eventos de clique e animações.

**Arquivo: `exemplos/03-interatividade.html`**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AFrame - Interatividade</title>
    <script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
</head>
<body>
    <a-scene>
        <a-camera position="0 1.6 0">
            <a-cursor></a-cursor>
        </a-camera>
        
        <!-- Cubo que muda de cor ao clicar -->
        <a-box 
            id="cuboInterativo"
            position="0 1 -3" 
            rotation="0 45 0" 
            color="#FF5733"
            animation="property: rotation; to: 0 405 0; loop: true; dur: 2000"
            shadow>
        </a-box>
        
        <!-- Esfera que pula ao clicar -->
        <a-sphere 
            position="-2 1 -3" 
            radius="0.5"
            color="#33FF57"
            animation__jump="property: position; to: -2 2 -3; startEvents: click; dur: 500; easing: easeOutQuad"
            animation__fall="property: position; to: -2 1 -3; startEvents: jumpend; dur: 500; easing: easeInQuad"
            shadow>
        </a-sphere>
        
        <!-- Cilindro que gira continuamente -->
        <a-cylinder 
            position="2 1 -3" 
            radius="0.5" 
            height="1"
            color="#3357FF"
            animation="property: rotation; to: 0 360 0; loop: true; dur: 3000"
            shadow>
        </a-cylinder>
        
        <a-plane 
            position="0 0 -4" 
            rotation="-90 0 0" 
            width="10" 
            height="10" 
            color="#7BC8A4"
            shadow>
        </a-plane>
        
        <a-sky color="#87CEEB"></a-sky>
        <a-light type="ambient" color="#404040"></a-light>
        <a-light type="directional" position="0 5 5" intensity="0.5"></a-light>
    </a-scene>
    
    <script>
        // JavaScript para mudar cor do cubo ao clicar
        document.querySelector('#cuboInterativo').addEventListener('click', function() {
            const cores = ['#FF5733', '#33FF57', '#3357FF', '#FF33F5', '#F5FF33'];
            const corAleatoria = cores[Math.floor(Math.random() * cores.length)];
            this.setAttribute('color', corAleatoria);
        });
    </script>
</body>
</html>
```

**Conceitos apresentados:**
- Cursor para interação (`<a-cursor>`)
- Eventos de clique
- Animações com o componente `animation`
- Integração de JavaScript com AFrame

### Exemplo 4: Texto 3D e Labels

Demonstra como adicionar texto em cenas VR.

**Arquivo: `exemplos/04-texto-3d.html`**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AFrame - Texto 3D</title>
    <script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
</head>
<body>
    <a-scene>
        <a-camera position="0 1.6 0"></a-camera>
        
        <!-- Texto 3D principal -->
        <a-text 
            value="Bem-vindo ao VR!" 
            position="0 2.5 -3" 
            align="center"
            color="#FF5733"
            scale="2 2 2">
        </a-text>
        
        <!-- Label para o cubo -->
        <a-text 
            value="Cubo" 
            position="-2 1.5 -3" 
            align="center"
            color="#FFFFFF"
            background-color="#000000"
            padding="0.2">
        </a-text>
        <a-box 
            position="-2 1 -3" 
            color="#FF5733"
            shadow>
        </a-box>
        
        <!-- Label para a esfera -->
        <a-text 
            value="Esfera" 
            position="0 1.5 -3" 
            align="center"
            color="#FFFFFF"
            background-color="#000000"
            padding="0.2">
        </a-text>
        <a-sphere 
            position="0 1 -3" 
            radius="0.5"
            color="#33FF57"
            shadow>
        </a-sphere>
        
        <!-- Label para o cilindro -->
        <a-text 
            value="Cilindro" 
            position="2 1.5 -3" 
            align="center"
            color="#FFFFFF"
            background-color="#000000"
            padding="0.2">
        </a-text>
        <a-cylinder 
            position="2 1 -3" 
            radius="0.5" 
            height="1"
            color="#3357FF"
            shadow>
        </a-cylinder>
        
        <a-plane 
            position="0 0 -4" 
            rotation="-90 0 0" 
            width="10" 
            height="10" 
            color="#7BC8A4"
            shadow>
        </a-plane>
        
        <a-sky color="#87CEEB"></a-sky>
        <a-light type="ambient" color="#404040"></a-light>
        <a-light type="directional" position="0 5 5" intensity="0.5"></a-light>
    </a-scene>
</body>
</html>
```

**Conceitos apresentados:**
- Componente de texto 3D (`<a-text>`)
- Alinhamento e formatação de texto
- Labels e identificação de objetos

### Exemplo 5: Troca de Cenas e Sky com Imagens

Este exemplo demonstra como criar interatividade avançada, permitindo que o usuário clique em objetos para trocar entre diferentes cenas. Além disso, mostra como usar imagens de fundo no componente `<a-sky>`.

**Arquivo: `exemplos/05-troca-cenas-sky.html`**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AFrame - Troca de Cenas e Sky com Imagens</title>
    <script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
</head>
<body>
    <!-- Cena 1: Cena Inicial com Sky Colorido -->
    <a-scene id="cena1">
        <a-camera position="0 1.6 0">
            <a-cursor></a-cursor>
        </a-camera>
        
        <!-- Texto de boas-vindas -->
        <a-text 
            value="Cena 1: Clique no cubo para mudar!" 
            position="0 2.5 -3" 
            align="center"
            color="#FFFFFF"
            scale="1.5 1.5 1.5">
        </a-text>
        
        <!-- Cubo clicável que muda de cena -->
        <a-box 
            id="botaoTroca"
            position="0 1 -3" 
            rotation="0 45 0" 
            color="#FF5733"
            animation="property: rotation; to: 0 405 0; loop: true; dur: 2000"
            shadow>
        </a-box>
        
        <!-- Label explicativa -->
        <a-text 
            value="Clique aqui" 
            position="0 0.3 -3" 
            align="center"
            color="#FFFFFF"
            background-color="#000000"
            padding="0.2">
        </a-text>
        
        <!-- Chão -->
        <a-plane 
            position="0 0 -4" 
            rotation="-90 0 0" 
            width="10" 
            height="10" 
            color="#7BC8A4"
            shadow>
        </a-plane>
        
        <!-- Sky com cor sólida (azul claro) -->
        <a-sky color="#87CEEB"></a-sky>
        
        <!-- Iluminação -->
        <a-light type="ambient" color="#404040"></a-light>
        <a-light type="directional" position="0 5 5" intensity="0.5"></a-light>
    </a-scene>
    
    <!-- Cena 2: Cena com Sky usando Imagem -->
    <a-scene id="cena2" style="display: none;">
        <a-camera position="0 1.6 0">
            <a-cursor></a-cursor>
        </a-camera>
        
        <!-- Texto de boas-vindas -->
        <a-text 
            value="Cena 2: Clique no cubo para voltar!" 
            position="0 2.5 -3" 
            align="center"
            color="#FFFFFF"
            scale="1.5 1.5 1.5">
        </a-text>
        
        <!-- Cubo clicável que volta para cena 1 -->
        <a-box 
            id="botaoVolta"
            position="0 1 -3" 
            rotation="0 45 0" 
            color="#33FF57"
            animation="property: rotation; to: 0 405 0; loop: true; dur: 2000"
            shadow>
        </a-box>
        
        <!-- Label explicativa -->
        <a-text 
            value="Clique para voltar" 
            position="0 0.3 -3" 
            align="center"
            color="#FFFFFF"
            background-color="#000000"
            padding="0.2">
        </a-text>
        
        <!-- Chão -->
        <a-plane 
            position="0 0 -4" 
            rotation="-90 0 0" 
            width="10" 
            height="10" 
            color="#8B4513"
            shadow>
        </a-plane>
        
        <!-- Sky com imagem de fundo -->
        <!-- Usando uma imagem de exemplo (você pode substituir por qualquer URL de imagem) -->
        <a-sky src="https://cdn.aframe.io/360-image-guide/panorama/puydesancy.jpg"></a-sky>
        
        <!-- Iluminação -->
        <a-light type="ambient" color="#404040"></a-light>
        <a-light type="directional" position="0 5 5" intensity="0.5"></a-light>
    </a-scene>
    
    <script>
        // Variável para controlar qual cena está ativa
        let cenaAtual = 1;
        
        // Função para trocar de cena
        function trocarCena() {
            const cena1 = document.getElementById('cena1');
            const cena2 = document.getElementById('cena2');
            
            if (cenaAtual === 1) {
                // Esconde cena 1 e mostra cena 2
                cena1.style.display = 'none';
                cena2.style.display = 'block';
                cenaAtual = 2;
            } else {
                // Esconde cena 2 e mostra cena 1
                cena2.style.display = 'none';
                cena1.style.display = 'block';
                cenaAtual = 1;
            }
        }
        
        // Adiciona evento de clique no cubo da cena 1
        document.querySelector('#botaoTroca').addEventListener('click', function() {
            trocarCena();
        });
        
        // Adiciona evento de clique no cubo da cena 2
        document.querySelector('#botaoVolta').addEventListener('click', function() {
            trocarCena();
        });
    </script>
</body>
</html>
```

**Conceitos apresentados:**
- Troca de cenas através de eventos de clique
- Múltiplas cenas AFrame na mesma página
- Componente `<a-sky>` com cor sólida
- Componente `<a-sky>` com imagens de fundo (360°)
- Navegação entre cenas usando JavaScript

#### Explicação Detalhada: Troca de Cenas

##### 1. Múltiplas Cenas na Mesma Página

No AFrame, você pode ter múltiplas tags `<a-scene>` na mesma página HTML. Para controlar qual cena está visível, usamos CSS para mostrar/esconder:

```html
<!-- Cena 1: visível por padrão -->
<a-scene id="cena1">
    <!-- conteúdo da cena 1 -->
</a-scene>

<!-- Cena 2: escondida inicialmente -->
<a-scene id="cena2" style="display: none;">
    <!-- conteúdo da cena 2 -->
</a-scene>
```

**Por que usar `display: none`?**
- Quando uma cena está com `display: none`, ela não é renderizada
- Isso economiza recursos do navegador
- Permite alternar entre cenas sem recarregar a página

##### 2. Eventos de Clique para Navegação

O JavaScript controla a troca de cenas através de eventos de clique:

```javascript
// Função que alterna entre as cenas
function trocarCena() {
    const cena1 = document.getElementById('cena1');
    const cena2 = document.getElementById('cena2');
    
    if (cenaAtual === 1) {
        cena1.style.display = 'none';  // Esconde cena 1
        cena2.style.display = 'block'; // Mostra cena 2
        cenaAtual = 2;
    } else {
        cena2.style.display = 'none';  // Esconde cena 2
        cena1.style.display = 'block'; // Mostra cena 1
        cenaAtual = 1;
    }
}

// Adiciona o evento de clique ao objeto
document.querySelector('#botaoTroca').addEventListener('click', function() {
    trocarCena();
});
```

**Como funciona:**
1. O usuário clica no objeto com `id="botaoTroca"`
2. O evento `click` é disparado
3. A função `trocarCena()` é executada
4. A cena atual é escondida e a outra é mostrada

##### 3. IDs para Identificação

Cada objeto clicável precisa de um `id` único para ser identificado pelo JavaScript:

```html
<a-box id="botaoTroca" ...></a-box>  <!-- Cena 1 -->
<a-box id="botaoVolta" ...></a-box>  <!-- Cena 2 -->
```

#### Explicação Detalhada: Componente `<a-sky>`

O componente `<a-sky>` cria um ambiente ao redor de toda a cena VR. Ele pode ser usado de duas formas principais:

##### 1. Sky com Cor Sólida

A forma mais simples é usar uma cor sólida:

```html
<a-sky color="#87CEEB"></a-sky>
```

**Características:**
- `color`: Define a cor de fundo usando código hexadecimal
- Cria um ambiente uniforme e simples
- Ideal para cenas que não precisam de contexto visual complexo
- Performance excelente (sem carregamento de imagens)

**Exemplos de cores:**
- `#87CEEB` - Azul céu claro
- `#000000` - Preto (espaço)
- `#FFA500` - Laranja (pôr do sol)
- `#2C3E50` - Azul escuro (noite)

##### 2. Sky com Imagem 360° (Panorama)

Para criar ambientes mais imersivos, você pode usar imagens panorâmicas 360°:

```html
<a-sky src="https://cdn.aframe.io/360-image-guide/panorama/puydesancy.jpg"></a-sky>
```

**Características:**
- `src`: URL da imagem panorâmica 360°
- A imagem é projetada como uma esfera ao redor da cena
- Cria uma experiência imersiva de estar em um local real
- O usuário pode olhar ao redor e ver a imagem completa

**Tipos de Imagens Suportadas:**
- **Imagens Equirretangulares (Equirectangular)**: Formato mais comum para panoramas 360°
- Formatos: JPG, PNG, WebP
- Resolução recomendada: 2048x1024 ou maior (quanto maior, melhor a qualidade)

**Onde encontrar imagens 360°:**
- [Unsplash 360°](https://unsplash.com/s/photos/360)
- [Flickr 360°](https://www.flickr.com/groups/equirectangular/)
- [Poly Haven](https://polyhaven.com/hdris) - HDRI (High Dynamic Range Images)
- Criar suas próprias com câmeras 360° ou apps de smartphone

##### 3. Comparação: Cor vs Imagem

| Característica | Cor Sólida | Imagem 360° |
|----------------|------------|-------------|
| **Performance** | Excelente | Depende do tamanho da imagem |
| **Imersão** | Baixa | Alta |
| **Carregamento** | Instantâneo | Requer download |
| **Uso de memória** | Mínimo | Maior |
| **Ideal para** | Cenas simples, testes | Ambientes realistas, experiências imersivas |

##### 4. Dicas para Usar Imagens no Sky

1. **Otimização de Imagens:**
   - Comprima imagens antes de usar (ferramentas como TinyPNG)
   - Use formatos modernos como WebP quando possível
   - Resoluções muito altas podem causar lentidão

2. **URLs de Imagens:**
   - Use URLs absolutas (com `https://`)
   - Imagens locais: coloque na pasta do projeto e use caminho relativo
   - Exemplo local: `<a-sky src="imagens/panorama.jpg"></a-sky>`

3. **Problemas Comuns:**
   - **Imagem não aparece**: Verifique se a URL está correta e acessível
   - **Imagem distorcida**: Certifique-se de que é uma imagem equirretangular 360°
   - **Carregamento lento**: Reduza a resolução ou comprima a imagem

##### 5. Exemplo Prático: Usando Imagem Local

Se você tiver uma imagem local na pasta do projeto:

```html
<!-- Estrutura de pastas -->
projetos-aframe/
  ├── exemplos/
  │   └── 05-troca-cenas-sky.html
  └── imagens/
      └── meu-panorama.jpg

<!-- No HTML -->
<a-sky src="../imagens/meu-panorama.jpg"></a-sky>
```


## Criando um HelloWorld com o AFrame

Pessoal vou iniciar colocando algumas referências que acredito que valem a pena vocês darem uma olhada:
- https://hacks.mozilla.org/2017/05/having-fun-with-physics-and-a-frame/
- https://hacks.mozilla.org/2018/03/immersive-aframe-low-poly/
- https://www.youtube.com/watch?app=desktop&v=cS8uGfd_oG8
- https://poly.pizza/m/fojR5i3h_nh



