2. Descrição de Funcionamento
O sistema está estruturado em três frentes principais para garantir o fornecimento de internet, a comunicação local e o acesso ao CPE externo:

A. Entrada de Internet Principal (wlan1)
Como funciona: A interface wlan1 opera em modo estação (station), conectando-se diretamente ao Hotspot Wi-Fi do seu celular.

Atribuição: Um cliente DHCP solicita e recebe um endereço IP do celular.

Saída (NAT): O tráfego de saída passa por NAT (masquerade) vinculado à interface wlan1, compartilhando a conexão de internet com toda a rede local.

B. Rede Local e Wi-Fi de Acesso (bridge-local e wlan2)
Interfaces integradas: As portas físicas ether2, ether3 e a interface Wi-Fi virtual wlan2 (com o SSID teste) fazem parte da bridge-local.

Endereçamento: O MikroTik atua como gateway local através do IP 192.168.88.1/24.

Servidor DHCP: O dhcp-lan distribui endereços IP automaticamente na faixa de 192.168.88.10 a 192.168.88.250 para os dispositivos conectados via cabo ou pelo Wi-Fi virtual teste.

C. Gerenciamento do CPE na Porta 4 (ether4)
Isolamento: A porta ether4 foi removida da bridge para operar de forma independente.

Cliente DHCP: Um cliente DHCP configurado em ether4 (com add-default-route=no) solicita e recebe um endereço IP diretamente do CPE conectado nela.

Objetivo: Como o MikroTik assume um IP na mesma faixa do CPE, você consegue abrir o navegador em qualquer dispositivo conectado à rede local do MikroTik e acessar diretamente a página de gerenciamento do CPE utilizando o IP que foi atribuído a essa porta.


[ Hotspot do Celular ] (Internet Principal)
          │
          ▼ (Wi-Fi)
    ┌───────────┐
    │  wlan1    │ (Cliente DHCP - Recebe Internet)
    └─────┬─────┘
          │
          ▼
   ┌──────────────┐       ┌────────────────────────┐
   │ MikroTik hAP │◄─────►│    bridge-local        │
   │    lite      │       │ (ether2, ether3, wlan2)│
   └──────┬───────┘       └───────────┬────────────┘
          │                           │
          │ (Isolada)                 ▼ (Wi-Fi SSID: "teste" / LAN)
   ┌──────┴───────┐             [ Celular de Acesso / Dispositivos LAN ]
   │    ether4    │             (Rede: 192.168.88.0/24 | GW: 192.168.88.1)
   └──────┬───────┘
          │
          ▼ (Cliente DHCP sem rota padrão)
   [ CPE / Roteador Externo ]
   (Recebe IP dinâmico do CPE para gerenciamento)
