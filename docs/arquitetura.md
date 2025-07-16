# Definição arquitetural - RAAI

> Apresentação do modelo da solução e padrões arquiteturais utilizados no projeto RAAI

## Visão Geral da Solução Proposta

### Eletrônica

Para tornar o robô funcional, é necessário projetar o módulo de controle dos motores. Para movimentar-se, são necessários 4 motores de corrente contínua de 25 Watts de potência e 4 kgf·cm de torque. Rodas omnidirecionais são acopladas nos motores para permitir movimentos em todas as direções. Para acionar os motores, são utilizados o motor driver BTS7960. Encoders de efeito Hall são utilizados para estimar a velocidade das rodas, essencial para os algoritmos de robótica. Para controlar todos esses módulos e garantir velocidade constante dos motores, o microcontrolador Raspberry Pi Pico RP2040 é utilizado e comunica-se com o módulo de Robótica. Um sistema de baterias será utilizado para fornecer energia aos motores e ligar todos os módulos de controle dos motores e do robô.

### Robótica e Navegação

Além de funcional, o robô precisa ser inteligente. O módulo de controle do robô é responsável por realizar a implementação dos algoritmos de navegação autônoma, processar as leituras dos sensores de temperatura e enviá-las para a IHM na central de operações da fábrica. O módulo de robótica conta com um LIDAR 3D, câmeras e sensores de distância, equipamentos necessários para a realização de navegação autônoma segura pelo ambiente.

### Software

A camada de software do robô é de extrema importância, pois ela coordena o fluxo de informações entre os módulos, integrando os sensores e a interface IHM. A Jetson irá operar com o sistema operacional Ubuntu 22.04 LTS, juntamente com o ROS 2 Humble para realizar a integração entre todos os subsistemas do robô. O robô deve processar as imagens capturadas pela câmera e as leituras dos sensores, a fim de detectar se há anomalias pré-determinadas no ambiente.

As leituras dos sensores, eventos detectados pelo robô, bem como os mapas 3D gerados da fábrica devem ser enviados a uma interface web. Um banco de dados deve armazenar as leituras dos sensores do robô, permitindo consultas futuras. Um serviço de LLM estará disponível aos operadores para consulta e insights sobre os dados capturados pelo robô, bem como para a geração de relatórios personalizados. Estas funcionalidades estarão disponíveis ao operador através de uma GUI na central de operações

### Comunicação

O subsistema de comunicação garante que os dados sejam transmitidos com baixa latência entre o robô, estação de comando e os demais sistemas da fábrica. No próprio robô, os subsistemas de controle se comunicam através do ROS 2. O servidor, que armazena as informações das missões, recebe os dados de telemetria e envia comandos ao robô através de comunicação via MQTT. Perceba que esta configuração permite que haja vários robôs operando ao mesmo tempo, comunicando-se com o servidor. As informações do servidor são mostradas ao operador através de uma Interface Web, que acessa o banco de dados do servidor através de Websockets

## Diagramas Arquiteturais