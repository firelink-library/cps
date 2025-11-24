---
sidebar_position: 1
slug: /pico
---

# Raspberry Pi Pico

Pessoal aqui vamos discutir um pouco sobre o que é a Raspberry Pi Pico e como ela pode ser utilizada como um poderoso microcontrolador.

## O que é Raspberry Pi Pico?

**Raspberry Pi Pico** é um microcontrolador de baixo custo desenvolvido pela Raspberry Pi Foundation. Diferente dos modelos anteriores da Raspberry Pi (que são computadores completos), o Pico é um microcontrolador baseado no chip **RP2040**, projetado especificamente para projetos de eletrônica e automação.

Site oficial: [https://www.raspberrypi.com/documentation/microcontrollers/](https://www.raspberrypi.com/documentation/microcontrollers/)

<img src="https://www.raspberrypi.com/app/uploads/2022/09/picow-pinout.png" style={{ display: 'block', marginLeft: 'auto', maxHeight: '60vh', marginRight: 'auto' }}/>
<br/>

### Principais características:

- **Chip RP2040**: Dual-core ARM Cortex-M0+ a 133 MHz
- **Memória**: 264 KB de RAM SRAM
- **GPIO**: 26 pinos GPIO multifuncionais
- **ADC**: 3 canais analógicos de 12 bits
- **PWM**: 16 canais PWM independentes
- **UART, SPI, I2C**: Múltiplos protocolos de comunicação
- **MicroPython**: Suporte nativo para programação em Python
- **Baixo custo**: Ideal para projetos educacionais e prototipagem

### Por que usar Raspberry Pi Pico?

O Pico é excelente para:
- **Projetos IoT**: Conectar sensores e atuadores à internet
- **Automação**: Controlar dispositivos e sistemas
- **Aprendizado**: Entender microcontroladores e eletrônica embarcada
- **Prototipagem rápida**: Desenvolver projetos sem necessidade de placas complexas
- **Projetos educacionais**: Ensinar programação embarcada e eletrônica

## Instalação e Configuração

Agora vamos para alguns pontos de configuração e instalação das ferramentas para trabalharmos com nosso microcontrolador.

:::danger[Dispositivo Físico]

Pessoal, existem alguns simuladores que podem ser utilizados aqui para simular o comportamento da Raspberry Pi Pico e testar suas interações com hardware. Contudo, recomendo fortemente testar com um dispositivo físico.

> `Murilão é mais fácil de ver funcionando em um dispositivo físico?`

Na verdade não, estaremos trazendo mais complexidade para nosso sistema. Isso pois, não teremos só o software que pode não funcionar, mas também alguma coisa com o hardware pode não estar corretamente configurado ou montado.

> `...`

Calma, eu sei que parece estranho. Mas a sensação de ver um hardware que você montou funcionando é diferente quando o hardware é real e está bem na sua frente. Se por alguma questão isso não possível, procurar os simuladores é uma excelente alternativa. 

Mas, se for possível, tente utilizar o hardware real, é uma experiência ótima de aprendizado e jornada.

Sugestão: se for a primeira vez que você estiver montando componentes eletrônicos, vale dar uma olhada com calma em cada um deles, ver manuais de operação e, sempre que possível, tentar verificar um ou dois vídeos de como eles são ligados e funcionam.

Isso não só vai dar mais fundamento de como você pode utilizar aquele dispositivo, como também vai ajudar você a não queimar o dispositivo por ter ligado ele incorretamente 🔥!

:::

### 1. Instalando MicroPython no Pico

O primeiro passo é instalar o MicroPython no seu Raspberry Pi Pico. Existem duas formas principais:

#### Método 1: Usando Thonny (Recomendado para iniciantes)

1. **Baixe o Thonny**: [https://thonny.org/](https://thonny.org/)
2. **Instale o Thonny** no seu computador
3. **Conecte o Pico** ao computador segurando o botão **BOOTSEL** enquanto conecta o cabo USB
4. **Abra o Thonny** e vá em: `Tools > Options > Interpreter`
5. **Selecione "MicroPython (Raspberry Pi Pico)"**
6. O Thonny irá instalar o MicroPython automaticamente

#### Método 2: Instalação Manual

1. **Baixe o firmware MicroPython**: [https://micropython.org/download/rp2-pico/](https://micropython.org/download/rp2-pico/)
2. **Conecte o Pico** segurando o botão **BOOTSEL**
3. **Copie o arquivo `.uf2`** para a unidade USB que aparece (RPI-RP2)
4. O Pico reiniciará automaticamente com MicroPython instalado

### 2. Verificando a Instalação

Abra o Thonny ou qualquer terminal serial e execute:

```python
import machine
import sys
print(sys.version)
print(machine.freq())
```

Você deve ver algo como:

```
MicroPython v1.23.0 on 2024-01-01; Raspberry Pi Pico with RP2040
133000000
```

### 3. Estrutura de um Projeto

Para organizar seus projetos, crie uma estrutura como esta:

```
projeto_pico/
├── main.py              # Arquivo principal (executado ao iniciar)
├── boot.py              # Executado antes do main.py (opcional)
├── config.py            # Configurações do projeto
├── lib/                 # Bibliotecas externas
│   ├── wifi.py
│   └── sensors.py
└── examples/            # Exemplos de código
    ├── digital_io.py
    ├── analog_io.py
    └── pwm.py
```

:::tip[Arquivo main.py]

O arquivo `main.py` é executado automaticamente quando o Pico inicia. É aqui que você coloca o código principal do seu projeto. Se você quiser que o código rode apenas uma vez, coloque tudo no `main.py`. Se quiser que rode continuamente, use um loop `while True`.

:::

## Exemplos Práticos

### Exemplo 1: I/O Digital - LED Piscando

O exemplo mais básico: fazer um LED piscar. O Pico tem um LED integrado no pino GPIO 25.

```python
# main.py
from machine import Pin
import time

# Configurar o LED integrado (GPIO 25)
led = Pin(25, Pin.OUT)

# Loop infinito
while True:
    led.on()      # Liga o LED
    time.sleep(1) # Espera 1 segundo
    led.off()     # Desliga o LED
    time.sleep(1) # Espera 1 segundo
```

**Explicação:**
- `Pin(25, Pin.OUT)` cria um objeto para controlar o pino 25 como saída
- `led.on()` coloca o pino em nível alto (3.3V)
- `led.off()` coloca o pino em nível baixo (0V)
- `time.sleep(1)` pausa a execução por 1 segundo

### Exemplo 2: I/O Digital - LED Externo

Para usar um LED externo, você precisa de:
- 1 LED
- 1 Resistor de 220Ω a 1kΩ
- Fios jumper

**Conexão:**
- Anodo do LED → GPIO 16 (através do resistor)
- Catodo do LED → GND

```python
from machine import Pin
import time

# Configurar LED externo no GPIO 16
led = Pin(16, Pin.OUT)

while True:
    led.value(1)  # Liga (equivalente a led.on())
    time.sleep(0.5)
    led.value(0)  # Desliga (equivalente a led.off())
    time.sleep(0.5)
```

### Exemplo 3: I/O Digital - Leitura de Botão

Ler o estado de um botão e controlar um LED.

**Conexão:**
- Um terminal do botão → GPIO 15
- Outro terminal do botão → GND
- LED → GPIO 16 (com resistor)

```python
from machine import Pin
import time

# Configurar botão (entrada com pull-up interno)
button = Pin(15, Pin.IN, Pin.PULL_UP)
led = Pin(16, Pin.OUT)

while True:
    # Ler o estado do botão
    # PULL_UP: quando não pressionado = 1, quando pressionado = 0
    if button.value() == 0:
        led.on()
    else:
        led.off()
    
    time.sleep(0.1)  # Pequeno delay para evitar leituras muito rápidas
```

**Explicação:**
- `Pin.IN` configura o pino como entrada
- `Pin.PULL_UP` ativa o resistor pull-up interno (evita leituras flutuantes)
- Quando o botão é pressionado, conecta o pino ao GND (valor = 0)

### Exemplo 4: I/O Digital - Múltiplos LEDs (Chase Effect)

Criar um efeito de "perseguição" com múltiplos LEDs.

```python
from machine import Pin
import time

# Lista de pinos dos LEDs
leds = [Pin(16, Pin.OUT), Pin(17, Pin.OUT), Pin(18, Pin.OUT), Pin(19, Pin.OUT)]

# Inicializar todos os LEDs desligados
for led in leds:
    led.off()

while True:
    # Acender cada LED sequencialmente
    for led in leds:
        led.on()
        time.sleep(0.2)
        led.off()
    
    # Voltar na ordem inversa
    for led in reversed(leds):
        led.on()
        time.sleep(0.2)
        led.off()
```

### Exemplo 5: I/O Analógica - Leitura de Potenciômetro

O Pico tem 3 canais ADC (Analog-to-Digital Converter) nos pinos GP26, GP27 e GP28.

**Conexão:**
- Terminal 1 do potenciômetro → 3.3V
- Terminal 2 (wiper) → GP26
- Terminal 3 → GND

```python
from machine import Pin, ADC
import time

# Configurar ADC no GP26
adc = ADC(Pin(26))

while True:
    # Ler valor analógico (0 a 65535 para 12 bits)
    value = adc.read_u16()
    
    # Converter para voltagem (0 a 3.3V)
    voltage = (value / 65535) * 3.3
    
    print(f"Valor ADC: {value}, Voltagem: {voltage:.2f}V")
    time.sleep(0.5)
```

**Explicação:**
- `ADC(Pin(26))` cria um objeto ADC no pino 26
- `read_u16()` retorna um valor de 0 a 65535 (16 bits, mas o ADC é 12 bits)
- O valor pode ser convertido para voltagem multiplicando por 3.3V

### Exemplo 6: I/O Analógica - Sensor de Temperatura (LM35)

Usar um sensor de temperatura analógico.

**Conexão:**
- VCC do LM35 → 3.3V
- GND do LM35 → GND
- OUT do LM35 → GP26

```python
from machine import Pin, ADC
import time

adc = ADC(Pin(26))

while True:
    # Ler valor do ADC
    value = adc.read_u16()
    
    # Converter para voltagem
    voltage = (value / 65535) * 3.3
    
    # LM35: 10mV por grau Celsius
    temperature = voltage * 100
    
    print(f"Temperatura: {temperature:.2f}°C")
    time.sleep(1)
```

### Exemplo 7: I/O Analógica - Controlar LED com Potenciômetro

Controlar o brilho de um LED baseado na posição de um potenciômetro.

```python
from machine import Pin, ADC, PWM
import time

# ADC para ler o potenciômetro
adc = ADC(Pin(26))

# PWM para controlar o LED
led = PWM(Pin(16))
led.freq(1000)  # Frequência de 1kHz

while True:
    # Ler valor do potenciômetro (0 a 65535)
    adc_value = adc.read_u16()
    
    # Converter para duty cycle (0 a 65535)
    # duty_u16() aceita valores de 0 a 65535
    led.duty_u16(adc_value)
    
    time.sleep(0.01)  # Atualização rápida para resposta suave
```

### Exemplo 8: PWM - Controle de Brilho de LED

PWM (Pulse Width Modulation) permite controlar a intensidade de um LED.

```python
from machine import Pin, PWM
import time

# Criar objeto PWM no pino 16
led = PWM(Pin(16))

# Configurar frequência (Hz)
led.freq(1000)  # 1kHz é uma boa frequência para LEDs

# Variar o brilho de 0 a 100%
while True:
    # Aumentar brilho
    for duty in range(0, 65535, 1000):
        led.duty_u16(duty)
        time.sleep(0.01)
    
    # Diminuir brilho
    for duty in range(65535, 0, -1000):
        led.duty_u16(duty)
        time.sleep(0.01)
```

**Explicação:**
- `PWM(Pin(16))` cria um objeto PWM no pino 16
- `freq(1000)` define a frequência do PWM em Hz
- `duty_u16()` define o duty cycle (0 a 65535, onde 65535 = 100%)

### Exemplo 9: PWM - Servo Motor

Controlar um servo motor com PWM.

**Conexão:**
- Fio vermelho (VCC) → 5V ou 3.3V (depende do servo)
- Fio preto/marrom (GND) → GND
- Fio amarelo/laranja (sinal) → GP16

```python
from machine import Pin, PWM
import time

# Configurar PWM para servo (50Hz é padrão para servos)
servo = PWM(Pin(16))
servo.freq(50)  # 50Hz = 20ms de período

def set_servo_angle(angle):
    """
    Define o ângulo do servo (0 a 180 graus)
    Servos geralmente usam: 1ms (0°) a 2ms (180°) de pulso
    Com 50Hz: período = 20ms
    Duty cycle: 1ms/20ms = 5% (0°) a 2ms/20ms = 10% (180°)
    """
    # Converter ângulo para duty cycle
    # 0° = 1638 (5% de 32767), 180° = 8191 (25% de 32767)
    duty = int(1638 + (angle / 180) * 6553)
    servo.duty_u16(duty)

# Mover servo de 0 a 180 graus
while True:
    for angle in range(0, 181, 10):
        set_servo_angle(angle)
        time.sleep(0.5)
    
    for angle in range(180, -1, -10):
        set_servo_angle(angle)
        time.sleep(0.5)
```

### Exemplo 10: PWM - Múltiplos Canais

Controlar múltiplos LEDs com PWM independente.

```python
from machine import Pin, PWM
import time

# Criar múltiplos canais PWM
leds = [
    PWM(Pin(16)),
    PWM(Pin(17)),
    PWM(Pin(18)),
    PWM(Pin(19))
]

# Configurar frequência para todos
for led in leds:
    led.freq(1000)

# Efeito de onda senoidal em cada LED
import math

while True:
    for i in range(360):
        for j, led in enumerate(leds):
            # Offset de fase para cada LED
            phase = (i + j * 90) % 360
            # Calcular duty cycle baseado em seno
            duty = int(32767 + 32767 * math.sin(math.radians(phase)))
            led.duty_u16(duty)
        time.sleep(0.01)
```

### Exemplo 11: Temporizador - Timer Simples

Usar um timer para executar código periodicamente.

```python
from machine import Pin, Timer
import time

led = Pin(25, Pin.OUT)
led_state = False

def toggle_led(timer):
    """Função chamada pelo timer"""
    global led_state
    led_state = not led_state
    led.value(led_state)

# Criar timer (modo PERIODIC = repetitivo)
# Frequência: 2Hz = a cada 500ms
timer = Timer(period=500, mode=Timer.PERIODIC, callback=toggle_led)

# Loop principal (pode fazer outras coisas aqui)
try:
    while True:
        time.sleep(1)
        print("Timer rodando em background...")
except KeyboardInterrupt:
    timer.deinit()  # Parar o timer
    print("Timer parado")
```

### Exemplo 12: Temporizador - Múltiplos Timers

Usar múltiplos timers para diferentes tarefas.

```python
from machine import Pin, Timer

led1 = Pin(16, Pin.OUT)
led2 = Pin(17, Pin.OUT)
led3 = Pin(18, Pin.OUT)

led1_state = False
led2_state = False
led3_state = False

def blink_led1(timer):
    global led1_state
    led1_state = not led1_state
    led1.value(led1_state)

def blink_led2(timer):
    global led2_state
    led2_state = not led2_state
    led2.value(led2_state)

def blink_led3(timer):
    global led3_state
    led3_state = not led3_state
    led3.value(led3_state)

# Criar 3 timers com frequências diferentes
timer1 = Timer(period=500, mode=Timer.PERIODIC, callback=blink_led1)   # 2Hz
timer2 = Timer(period=1000, mode=Timer.PERIODIC, callback=blink_led2)  # 1Hz
timer3 = Timer(period=2000, mode=Timer.PERIODIC, callback=blink_led3)  # 0.5Hz

# Manter o programa rodando
import time
try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    timer1.deinit()
    timer2.deinit()
    timer3.deinit()
    print("Todos os timers parados")
```

### Exemplo 13: Interrupção - Botão com Interrupção

Usar interrupções para responder imediatamente a eventos.

```python
from machine import Pin
import time

led = Pin(25, Pin.OUT)
button = Pin(15, Pin.IN, Pin.PULL_UP)

# Contador de pressionamentos
press_count = 0

def button_handler(pin):
    """Função chamada quando o botão é pressionado"""
    global press_count
    press_count += 1
    led.toggle()  # Alternar estado do LED
    print(f"Botão pressionado {press_count} vezes!")

# Configurar interrupção no pino do botão
# IRQ_FALLING = quando o sinal vai de alto para baixo (botão pressionado)
button.irq(trigger=Pin.IRQ_FALLING, handler=button_handler)

print("Pressione o botão...")
print("(O programa continua rodando normalmente)")

# Loop principal pode fazer outras coisas
try:
    while True:
        time.sleep(1)
        print(f"Contador: {press_count}")
except KeyboardInterrupt:
    print("Programa encerrado")
```

**Explicação:**
- `irq()` configura uma interrupção no pino
- `trigger=Pin.IRQ_FALLING` ativa quando o sinal vai de 1 para 0
- `handler=button_handler` é a função chamada quando a interrupção ocorre
- A interrupção funciona mesmo enquanto o código principal está executando

### Exemplo 14: Interrupção - Debounce de Botão

Implementar debounce para evitar múltiplas leituras do botão.

```python
from machine import Pin, Timer
import time

led = Pin(25, Pin.OUT)
button = Pin(15, Pin.IN, Pin.PULL_UP)

# Variáveis para debounce
last_press_time = 0
debounce_time = 200  # 200ms

def button_handler(pin):
    """Handler da interrupção com debounce"""
    global last_press_time
    
    current_time = time.ticks_ms()
    
    # Verificar se passou tempo suficiente desde a última pressão
    if time.ticks_diff(current_time, last_press_time) > debounce_time:
        last_press_time = current_time
        led.toggle()
        print("Botão pressionado (com debounce)")

# Configurar interrupção
button.irq(trigger=Pin.IRQ_FALLING, handler=button_handler)

print("Pressione o botão (com proteção contra bounce)...")

try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    print("Programa encerrado")
```

### Exemplo 15: Interrupção - Múltiplos Botões

Gerenciar múltiplos botões com interrupções.

```python
from machine import Pin

led1 = Pin(16, Pin.OUT)
led2 = Pin(17, Pin.OUT)
led3 = Pin(18, Pin.OUT)

button1 = Pin(15, Pin.IN, Pin.PULL_UP)
button2 = Pin(14, Pin.IN, Pin.PULL_UP)
button3 = Pin(13, Pin.IN, Pin.PULL_UP)

def button1_handler(pin):
    led1.toggle()
    print("Botão 1 pressionado")

def button2_handler(pin):
    led2.toggle()
    print("Botão 2 pressionado")

def button3_handler(pin):
    led3.toggle()
    print("Botão 3 pressionado")

# Configurar interrupções para cada botão
button1.irq(trigger=Pin.IRQ_FALLING, handler=button1_handler)
button2.irq(trigger=Pin.IRQ_FALLING, handler=button2_handler)
button3.irq(trigger=Pin.IRQ_FALLING, handler=button3_handler)

print("Pressione qualquer botão...")

import time
try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    print("Programa encerrado")
```

### Exemplo 16: Comunicação Serial - UART Básico

Comunicação serial usando UART (Universal Asynchronous Receiver-Transmitter).

**Conexão entre dois Picos:**
- Pico 1 TX (GP0) → Pico 2 RX (GP1)
- Pico 1 RX (GP1) → Pico 2 TX (GP0)
- GND de ambos → GND comum

**Código para Pico 1 (Transmissor):**

```python
from machine import UART, Pin
import time

# Configurar UART
# UART0: TX=GP0, RX=GP1
uart = UART(0, baudrate=9600)

led = Pin(25, Pin.OUT)

counter = 0
while True:
    # Enviar mensagem
    message = f"Contador: {counter}\n"
    uart.write(message.encode())
    print(f"Enviado: {message.strip()}")
    
    led.toggle()
    counter += 1
    time.sleep(1)
```

**Código para Pico 2 (Receptor):**

```python
from machine import UART, Pin
import time

# Configurar UART
uart = UART(0, baudrate=9600)

led = Pin(25, Pin.OUT)

while True:
    # Verificar se há dados disponíveis
    if uart.any():
        # Ler dados recebidos
        data = uart.read()
        if data:
            message = data.decode()
            print(f"Recebido: {message.strip()}")
            led.toggle()
    
    time.sleep(0.1)
```

### Exemplo 17: Comunicação Serial - Bidirecional

Comunicação bidirecional entre dois dispositivos.

```python
from machine import UART, Pin
import time

# Configurar UART
uart = UART(0, baudrate=9600, tx=Pin(0), rx=Pin(1))

led = Pin(25, Pin.OUT)
button = Pin(15, Pin.IN, Pin.PULL_UP)

last_button_state = 1
message_counter = 0

while True:
    # Ler botão
    button_state = button.value()
    
    # Se botão foi pressionado, enviar mensagem
    if button_state == 0 and last_button_state == 1:
        message = f"Mensagem {message_counter}\n"
        uart.write(message.encode())
        print(f"Enviado: {message.strip()}")
        message_counter += 1
        led.toggle()
    
    last_button_state = button_state
    
    # Verificar se há dados recebidos
    if uart.any():
        data = uart.read()
        if data:
            message = data.decode()
            print(f"Recebido: {message.strip()}")
            led.toggle()
    
    time.sleep(0.05)
```

### Exemplo 18: Comunicação Serial - Protocolo Simples

Implementar um protocolo simples de comunicação.

```python
from machine import UART, Pin
import time

uart = UART(0, baudrate=9600)

def send_command(command, value=None):
    """Enviar comando formatado"""
    if value is not None:
        message = f"{command}:{value}\n"
    else:
        message = f"{command}\n"
    uart.write(message.encode())
    print(f"Enviado: {message.strip()}")

def receive_command():
    """Receber e processar comando"""
    if uart.any():
        data = uart.read()
        if data:
            message = data.decode().strip()
            parts = message.split(':')
            command = parts[0]
            value = parts[1] if len(parts) > 1 else None
            return command, value
    return None, None

# Exemplo de uso
led = Pin(25, Pin.OUT)
counter = 0

while True:
    # Enviar comando a cada 2 segundos
    if counter % 20 == 0:
        send_command("STATUS", counter)
    
    # Processar comandos recebidos
    cmd, val = receive_command()
    if cmd == "LED_ON":
        led.on()
        send_command("ACK", "LED_ON")
    elif cmd == "LED_OFF":
        led.off()
        send_command("ACK", "LED_OFF")
    elif cmd == "GET_COUNTER":
        send_command("COUNTER", counter)
    
    counter += 1
    time.sleep(0.1)
```

### Exemplo 19: WiFi - Conectar à Rede

Conectar o Pico W (com WiFi) a uma rede sem fio.

**Nota:** Este exemplo requer Raspberry Pi Pico W (com WiFi). O Pico padrão não tem WiFi.

```python
from machine import Pin
import network
import time

# Configurar LED
led = Pin("LED", Pin.OUT)

# Configurar WiFi
wlan = network.WLAN(network.STA_IF)
wlan.active(True)

# Credenciais da rede
SSID = "sua_rede_wifi"
PASSWORD = "sua_senha"

print("Conectando à rede WiFi...")
wlan.connect(SSID, PASSWORD)

# Aguardar conexão
max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    print("Aguardando conexão...")
    time.sleep(1)

# Verificar conexão
if wlan.status() != 3:
    print("Falha na conexão WiFi")
    led.off()
else:
    print("Conectado!")
    led.on()
    status = wlan.ifconfig()
    print(f"IP: {status[0]}")
    print(f"Subnet: {status[1]}")
    print(f"Gateway: {status[2]}")
    print(f"DNS: {status[3]}")
```

### Exemplo 20: WiFi - Cliente HTTP Simples

Fazer uma requisição HTTP GET simples.

```python
from machine import Pin
import network
import socket
import time

# Configurar WiFi (mesmo código do exemplo anterior)
wlan = network.WLAN(network.STA_IF)
wlan.active(True)

SSID = "sua_rede_wifi"
PASSWORD = "sua_senha"

print("Conectando...")
wlan.connect(SSID, PASSWORD)

max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    time.sleep(1)

if wlan.status() != 3:
    raise RuntimeError("Falha na conexão WiFi")

print("Conectado!")
status = wlan.ifconfig()
print(f"IP: {status[0]}")

# Função para fazer requisição HTTP GET
def http_get(url):
    """Faz uma requisição HTTP GET simples"""
    # Parse da URL
    _, _, host, path = url.split('/', 3)
    
    # Criar socket
    addr = socket.getaddrinfo(host, 80)[0][-1]
    s = socket.socket()
    s.connect(addr)
    
    # Enviar requisição HTTP
    request = f"GET /{path} HTTP/1.1\r\nHost: {host}\r\n\r\n"
    s.send(request.encode())
    
    # Receber resposta
    response = s.recv(4096)
    s.close()
    
    return response.decode()

# Exemplo: buscar dados de uma API
try:
    # Exemplo com httpbin.org (serviço de teste)
    response = http_get("http://httpbin.org/get")
    print("\nResposta HTTP:")
    print(response)
except Exception as e:
    print(f"Erro: {e}")
```

### Exemplo 21: WiFi - Cliente HTTP com JSON

Fazer requisição HTTP e processar resposta JSON.

```python
from machine import Pin
import network
import socket
import json
import time

# Configurar WiFi (código anterior)
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("sua_rede_wifi", "sua_senha")

max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    time.sleep(1)

if wlan.status() != 3:
    raise RuntimeError("Falha na conexão")

print(f"Conectado! IP: {wlan.ifconfig()[0]}")

def http_get_json(url):
    """Faz requisição HTTP GET e retorna JSON"""
    _, _, host, path = url.split('/', 3)
    
    addr = socket.getaddrinfo(host, 80)[0][-1]
    s = socket.socket()
    s.connect(addr)
    
    request = f"GET /{path} HTTP/1.1\r\nHost: {host}\r\n\r\n"
    s.send(request.encode())
    
    # Receber resposta completa
    response = b""
    while True:
        data = s.recv(1024)
        if not data:
            break
        response += data
    
    s.close()
    
    # Separar header do body
    header, body = response.split(b"\r\n\r\n", 1)
    
    # Parse JSON
    try:
        data = json.loads(body.decode())
        return data
    except:
        return None

# Exemplo: buscar dados JSON
try:
    data = http_get_json("http://httpbin.org/json")
    if data:
        print("\nDados JSON recebidos:")
        print(json.dumps(data, indent=2))
except Exception as e:
    print(f"Erro: {e}")
```

### Exemplo 22: WiFi - POST Request

Enviar dados via HTTP POST.

```python
from machine import Pin
import network
import socket
import json
import time

# Configurar WiFi (mesmo código)
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("sua_rede_wifi", "sua_senha")

max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    time.sleep(1)

if wlan.status() != 3:
    raise RuntimeError("Falha na conexão")

print(f"Conectado! IP: {wlan.ifconfig()[0]}")

def http_post(url, data):
    """Faz requisição HTTP POST com JSON"""
    _, _, host, path = url.split('/', 3)
    
    # Converter dados para JSON
    json_data = json.dumps(data)
    
    addr = socket.getaddrinfo(host, 80)[0][-1]
    s = socket.socket()
    s.connect(addr)
    
    # Montar requisição POST
    request = f"POST /{path} HTTP/1.1\r\n"
    request += f"Host: {host}\r\n"
    request += "Content-Type: application/json\r\n"
    request += f"Content-Length: {len(json_data)}\r\n"
    request += "\r\n"
    request += json_data
    
    s.send(request.encode())
    
    # Receber resposta
    response = s.recv(4096)
    s.close()
    
    return response.decode()

# Exemplo: enviar dados de sensor
led = Pin(25, Pin.OUT)
counter = 0

while True:
    # Simular dados de sensor
    sensor_data = {
        "sensor_id": "pico_001",
        "temperature": 25.5 + (counter % 10),
        "humidity": 60 + (counter % 20),
        "timestamp": counter
    }
    
    try:
        response = http_post("http://httpbin.org/post", sensor_data)
        print(f"Dados enviados: {sensor_data}")
        print(f"Resposta: {response[:200]}...")  # Primeiros 200 caracteres
        led.toggle()
    except Exception as e:
        print(f"Erro: {e}")
    
    counter += 1
    time.sleep(5)  # Enviar a cada 5 segundos
```

### Exemplo 23: WiFi - Servidor HTTP Simples

Criar um servidor HTTP básico no Pico.

```python
from machine import Pin
import network
import socket
import time

# Configurar WiFi
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("sua_rede_wifi", "sua_senha")

max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    time.sleep(1)

if wlan.status() != 3:
    raise RuntimeError("Falha na conexão")

ip = wlan.ifconfig()[0]
print(f"Conectado! IP: {ip}")

# Configurar LED
led = Pin(25, Pin.OUT)
led_state = False

# Criar socket servidor
addr = socket.getaddrinfo('0.0.0.0', 80)[0][-1]
s = socket.socket()
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind(addr)
s.listen(1)

print(f"Servidor HTTP rodando em http://{ip}")

def handle_request(conn):
    """Processa requisição HTTP"""
    global led_state
    
    request = conn.recv(1024).decode()
    
    # Parse simples da requisição
    if "GET /led/on" in request:
        led.on()
        led_state = True
        response = "LED ligado"
    elif "GET /led/off" in request:
        led.off()
        led_state = False
        response = "LED desligado"
    elif "GET /led/toggle" in request:
        led.toggle()
        led_state = not led_state
        response = f"LED {'ligado' if led_state else 'desligado'}"
    elif "GET /status" in request:
        response = f"LED: {'ligado' if led_state else 'desligado'}"
    else:
        response = """
        <html>
        <body>
            <h1>Raspberry Pi Pico Web Server</h1>
            <p><a href="/led/on">Ligar LED</a></p>
            <p><a href="/led/off">Desligar LED</a></p>
            <p><a href="/led/toggle">Alternar LED</a></p>
            <p><a href="/status">Status</a></p>
        </body>
        </html>
        """
    
    # Enviar resposta HTTP
    conn.send("HTTP/1.1 200 OK\r\n")
    conn.send("Content-Type: text/html\r\n")
    conn.send("\r\n")
    conn.send(response)
    conn.close()

# Loop principal do servidor
while True:
    try:
        conn, addr = s.accept()
        print(f"Conexão de {addr}")
        handle_request(conn)
    except Exception as e:
        print(f"Erro: {e}")
        conn.close()
```

### Exemplo 24: WebSocket - Cliente WebSocket Simples

Conectar a um servidor WebSocket.

```python
from machine import Pin
import network
import socket
import time
import ubinascii

# Configurar WiFi
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("sua_rede_wifi", "sua_senha")

max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    time.sleep(1)

if wlan.status() != 3:
    raise RuntimeError("Falha na conexão")

print(f"Conectado! IP: {wlan.ifconfig()[0]}")

def websocket_connect(host, path, port=80):
    """Conecta a um servidor WebSocket"""
    # Criar socket
    addr = socket.getaddrinfo(host, port)[0][-1]
    s = socket.socket()
    s.connect(addr)
    
    # Gerar chave WebSocket (simplificado)
    import urandom
    key = ubinascii.b2a_base64(bytes([urandom.getrandbits(8) for _ in range(16)]))[:-1]
    
    # Enviar handshake HTTP
    handshake = f"GET /{path} HTTP/1.1\r\n"
    handshake += f"Host: {host}\r\n"
    handshake += "Upgrade: websocket\r\n"
    handshake += "Connection: Upgrade\r\n"
    handshake += f"Sec-WebSocket-Key: {key.decode()}\r\n"
    handshake += "Sec-WebSocket-Version: 13\r\n"
    handshake += "\r\n"
    
    s.send(handshake.encode())
    
    # Receber resposta do handshake
    response = s.recv(4096)
    if b"101 Switching Protocols" in response:
        print("WebSocket conectado!")
        return s
    else:
        s.close()
        return None

def websocket_send(s, message):
    """Envia mensagem via WebSocket"""
    msg_bytes = message.encode()
    msg_len = len(msg_bytes)
    
    # Frame WebSocket (simplificado)
    frame = bytearray([0x81])  # FIN + opcode text
    
    if msg_len < 126:
        frame.append(msg_len)
    elif msg_len < 65536:
        frame.append(126)
        frame.extend(msg_len.to_bytes(2, 'big'))
    else:
        frame.append(127)
        frame.extend(msg_len.to_bytes(8, 'big'))
    
    frame.extend(msg_bytes)
    s.send(frame)

def websocket_receive(s):
    """Recebe mensagem via WebSocket"""
    if s.recv(1, socket.MSG_PEEK):
        data = s.recv(1024)
        if len(data) > 2:
            # Parse simplificado do frame WebSocket
            payload_len = data[1] & 0x7F
            if payload_len < 126:
                mask_start = 2
            elif payload_len == 126:
                mask_start = 4
            else:
                mask_start = 10
            
            mask = data[mask_start:mask_start+4]
            payload = data[mask_start+4:]
            
            # Decodificar payload
            decoded = bytearray([payload[i] ^ mask[i % 4] for i in range(len(payload))])
            return decoded.decode()
    return None

# Exemplo de uso
try:
    # Conectar a um servidor WebSocket de teste
    # Nota: Este é um exemplo simplificado. Para produção, use uma biblioteca completa
    ws = websocket_connect("echo.websocket.org", "echo", 80)
    
    if ws:
        # Enviar mensagem
        websocket_send(ws, "Hello WebSocket!")
        time.sleep(1)
        
        # Receber resposta
        response = websocket_receive(ws)
        if response:
            print(f"Recebido: {response}")
        
        ws.close()
except Exception as e:
    print(f"Erro: {e}")
```

### Exemplo 25: WebSocket - Cliente com Reconexão

Cliente WebSocket com sistema de reconexão automática.

```python
from machine import Pin
import network
import socket
import time
import ubinascii

# Configurar WiFi
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("sua_rede_wifi", "sua_senha")

max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    time.sleep(1)

if wlan.status() != 3:
    raise RuntimeError("Falha na conexão")

print(f"Conectado! IP: {wlan.ifconfig()[0]}")

led = Pin(25, Pin.OUT)
ws_connected = False

def connect_websocket(host, path, port=80):
    """Conecta a um servidor WebSocket (implementação simplificada)"""
    global ws_connected
    try:
        addr = socket.getaddrinfo(host, port)[0][-1]
        s = socket.socket()
        s.settimeout(5)
        s.connect(addr)
        
        # Handshake WebSocket (simplificado)
        import urandom
        key = ubinascii.b2a_base64(bytes([urandom.getrandbits(8) for _ in range(16)]))[:-1]
        
        handshake = f"GET /{path} HTTP/1.1\r\n"
        handshake += f"Host: {host}\r\n"
        handshake += "Upgrade: websocket\r\n"
        handshake += "Connection: Upgrade\r\n"
        handshake += f"Sec-WebSocket-Key: {key.decode()}\r\n"
        handshake += "Sec-WebSocket-Version: 13\r\n"
        handshake += "\r\n"
        
        s.send(handshake.encode())
        response = s.recv(4096)
        
        if b"101" in response:
            ws_connected = True
            led.on()
            print("WebSocket conectado!")
            return s
        else:
            s.close()
            return None
    except Exception as e:
        print(f"Erro ao conectar: {e}")
        return None

# Loop principal com reconexão
host = "echo.websocket.org"
path = "echo"
ws = None
message_counter = 0

while True:
    # Tentar conectar se não estiver conectado
    if not ws_connected or ws is None:
        print("Tentando conectar WebSocket...")
        ws = connect_websocket(host, path)
        time.sleep(2)
    
    if ws_connected and ws:
        try:
            # Enviar mensagem
            message = f"Mensagem {message_counter}"
            # Implementação simplificada de envio WebSocket
            ws.send(message.encode())
            print(f"Enviado: {message}")
            message_counter += 1
            
            # Tentar receber resposta
            try:
                data = ws.recv(1024)
                if data:
                    print(f"Recebido: {data.decode()}")
            except:
                pass  # Timeout ou sem dados
            
            time.sleep(2)
        except Exception as e:
            print(f"Erro na comunicação: {e}")
            ws_connected = False
            ws.close()
            ws = None
            led.off()
            time.sleep(5)  # Aguardar antes de reconectar
```

### Exemplo 26: Projeto Completo - Estação Meteorológica IoT

Projeto completo combinando múltiplos conceitos: sensores, WiFi, HTTP e WebSocket.

```python
from machine import Pin, ADC, PWM
import network
import socket
import json
import time

# ========== CONFIGURAÇÃO WIFI ==========
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("sua_rede_wifi", "sua_senha")

max_wait = 10
while max_wait > 0:
    if wlan.status() < 0 or wlan.status() >= 3:
        break
    max_wait -= 1
    time.sleep(1)

if wlan.status() != 3:
    raise RuntimeError("Falha na conexão WiFi")

ip = wlan.ifconfig()[0]
print(f"Conectado! IP: {ip}")

# ========== CONFIGURAÇÃO DE HARDWARE ==========
# LED de status
status_led = Pin(25, Pin.OUT)

# Sensores (simulados com ADC)
temp_sensor = ADC(Pin(26))
humidity_sensor = ADC(Pin(27))

# LED indicador de alerta
alert_led = PWM(Pin(16))
alert_led.freq(1000)

# Botão de reset
reset_button = Pin(15, Pin.IN, Pin.PULL_UP)

# ========== VARIÁVEIS GLOBAIS ==========
sensor_data = {
    "temperature": 0.0,
    "humidity": 0.0,
    "timestamp": 0
}

alert_active = False

# ========== FUNÇÕES ==========
def read_sensors():
    """Lê valores dos sensores"""
    global sensor_data
    
    # Simular leitura de temperatura (0-50°C)
    temp_raw = temp_sensor.read_u16()
    sensor_data["temperature"] = (temp_raw / 65535) * 50
    
    # Simular leitura de umidade (0-100%)
    hum_raw = humidity_sensor.read_u16()
    sensor_data["humidity"] = (hum_raw / 65535) * 100
    
    sensor_data["timestamp"] = time.time()
    
    return sensor_data

def check_alerts():
    """Verifica condições de alerta"""
    global alert_active
    
    if sensor_data["temperature"] > 40 or sensor_data["humidity"] > 80:
        alert_active = True
        alert_led.duty_u16(32767)  # 50% de brilho
    else:
        alert_active = False
        alert_led.duty_u16(0)

def send_data_to_server(data):
    """Envia dados para servidor via HTTP POST"""
    try:
        host = "seu-servidor.com"
        path = "api/sensor-data"
        
        addr = socket.getaddrinfo(host, 80)[0][-1]
        s = socket.socket()
        s.settimeout(5)
        s.connect(addr)
        
        json_data = json.dumps(data)
        request = f"POST /{path} HTTP/1.1\r\n"
        request += f"Host: {host}\r\n"
        request += "Content-Type: application/json\r\n"
        request += f"Content-Length: {len(json_data)}\r\n"
        request += "\r\n"
        request += json_data
        
        s.send(request.encode())
        response = s.recv(1024)
        s.close()
        
        return True
    except Exception as e:
        print(f"Erro ao enviar dados: {e}")
        return False

def reset_button_handler(pin):
    """Handler de interrupção do botão reset"""
    global alert_active
    alert_active = False
    alert_led.duty_u16(0)
    print("Alerta resetado!")

# Configurar interrupção do botão
reset_button.irq(trigger=Pin.IRQ_FALLING, handler=reset_button_handler)

# ========== LOOP PRINCIPAL ==========
print("Estação Meteorológica iniciada!")
status_led.on()

counter = 0

while True:
    try:
        # Ler sensores a cada iteração
        read_sensors()
        
        # Verificar alertas
        check_alerts()
        
        # Enviar dados a cada 10 iterações (10 segundos)
        if counter % 10 == 0:
            if send_data_to_server(sensor_data):
                print(f"Dados enviados: {sensor_data}")
                status_led.toggle()
        
        # Mostrar dados no console
        if counter % 5 == 0:
            print(f"Temp: {sensor_data['temperature']:.1f}°C, "
                  f"Umidade: {sensor_data['humidity']:.1f}%, "
                  f"Alerta: {'SIM' if alert_active else 'NÃO'}")
        
        counter += 1
        time.sleep(1)
        
    except KeyboardInterrupt:
        print("Programa encerrado")
        break
    except Exception as e:
        print(f"Erro: {e}")
        time.sleep(5)
```

## Dicas e Boas Práticas

1. **Gerenciamento de Memória**: MicroPython tem memória limitada. Evite criar listas muito grandes ou strings muito longas.

2. **Tratamento de Erros**: Sempre use `try/except` para operações de rede e I/O que podem falhar.

3. **Debounce de Botões**: Sempre implemente debounce para evitar leituras múltiplas de botões.

4. **WiFi e Energia**: Desconecte o WiFi quando não estiver em uso para economizar energia.

5. **Uso de Interrupções**: Use interrupções para eventos que precisam de resposta imediata, mas mantenha os handlers curtos.

6. **Documentação**: Comente seu código, especialmente configurações de hardware e pinos.

7. **Testes Incrementais**: Teste cada funcionalidade separadamente antes de combinar tudo.

## Recursos Adicionais

- **Documentação oficial**: [https://www.raspberrypi.com/documentation/microcontrollers/](https://www.raspberrypi.com/documentation/microcontrollers/)
- **MicroPython**: [https://docs.micropython.org/](https://docs.micropython.org/)
- **RP2040 Datasheet**: [https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf](https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf)
- **Fórum Raspberry Pi**: [https://forums.raspberrypi.com/](https://forums.raspberrypi.com/)
- **GitHub MicroPython**: [https://github.com/micropython/micropython](https://github.com/micropython/micropython)

## Conclusão

O Raspberry Pi Pico com MicroPython é uma plataforma poderosa e acessível para projetos de eletrônica e IoT. Com seus 26 pinos GPIO, suporte a múltiplos protocolos de comunicação e a facilidade de programação em Python, é ideal tanto para iniciantes quanto para projetos mais complexos. Experimente os exemplos acima e adapte-os às suas necessidades!
