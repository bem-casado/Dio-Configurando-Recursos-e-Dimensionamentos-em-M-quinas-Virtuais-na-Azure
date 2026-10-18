# Configurando Recursos e Dimensionamentos em Máquinas Virtuais na Azure

Este repositório foi criado como parte de um desafio da **DIO (Digital Innovation One)** para documentar os principais conceitos estudados sobre **configuração de recursos e dimensionamento de Máquinas Virtuais no Microsoft Azure**.

O objetivo é registrar uma visão prática sobre como escolher, configurar e administrar máquinas virtuais de acordo com requisitos de **desempenho, disponibilidade, segurança e custo**.

## Objetivo do desafio

Durante o laboratório, o foco é compreender como uma Máquina Virtual é criada na Azure e quais decisões precisam ser tomadas para dimensionar corretamente seus recursos.

Entre os principais pontos estudados estão:

- criação de Máquinas Virtuais;
- escolha de região e zona de disponibilidade;
- seleção da imagem do sistema operacional;
- famílias e tamanhos de VMs;
- quantidade de vCPUs e memória RAM;
- configuração de discos;
- configuração de rede;
- regras de acesso;
- escalabilidade;
- disponibilidade;
- monitoramento;
- otimização de custos.

## Máquinas Virtuais na Azure

O **Azure Virtual Machines** é um serviço de computação que permite executar máquinas virtuais na nuvem.

Com uma VM é possível escolher e controlar recursos como:

- sistema operacional;
- processador virtual;
- memória;
- armazenamento;
- rede;
- regras de segurança;
- aplicações instaladas.

Esse modelo oferece bastante flexibilidade, mas também exige maior responsabilidade na administração do ambiente.

## Estrutura básica de uma VM

Uma Máquina Virtual não funciona de forma isolada. Ela depende de outros recursos da Azure.

Um exemplo simplificado é:

```text
Resource Group
├── Virtual Machine
├── Network Interface
├── Virtual Network
│   └── Subnet
├── Network Security Group
├── Public IP
└── Managed Disks
```

Esses recursos trabalham em conjunto para fornecer computação, armazenamento e conectividade para a máquina.

## Região e zona de disponibilidade

Antes de criar uma VM é necessário escolher onde ela será executada.

A **região** representa a localização geográfica dos datacenters da Azure.

A escolha pode influenciar:

- latência;
- preço;
- disponibilidade de serviços;
- requisitos de residência de dados;
- recuperação de desastres.

Quando disponíveis, as **Availability Zones** permitem distribuir recursos entre datacenters fisicamente separados dentro da mesma região.

Isso ajuda a reduzir o impacto de falhas em um único datacenter.

## Imagem do sistema operacional

Durante a criação da VM é necessário escolher uma imagem.

Alguns exemplos são:

- Windows Server;
- Ubuntu Server;
- Red Hat Enterprise Linux;
- SUSE Linux Enterprise;
- imagens personalizadas;
- imagens disponíveis no Azure Marketplace.

A escolha depende do sistema que será executado, das ferramentas utilizadas e dos requisitos da aplicação.

## Dimensionamento da VM

Uma das decisões mais importantes é selecionar o tamanho adequado da máquina.

O tamanho define principalmente:

```text
vCPUs
+
Memória RAM
+
Desempenho de disco
+
Capacidade de rede
```

Escolher uma VM muito pequena pode causar problemas de desempenho.

Escolher uma VM muito grande pode gerar desperdício de recursos e aumentar o custo sem necessidade.

## Famílias de Máquinas Virtuais

A Azure possui diferentes famílias de VMs, cada uma voltada para um tipo de carga.

### Uso geral

Oferecem equilíbrio entre CPU e memória.

Indicadas para:

- servidores web;
- ambientes de desenvolvimento;
- aplicações corporativas;
- pequenos bancos de dados.

### Otimizadas para computação

Possuem maior proporção de CPU em relação à memória.

Indicadas para:

- processamento intensivo;
- servidores de aplicação;
- cálculos;
- workloads com uso elevado de CPU.

### Otimizadas para memória

Possuem maior quantidade de memória RAM.

Indicadas para:

- bancos de dados;
- análise de dados;
- aplicações que mantêm grandes volumes de dados em memória.

### Otimizadas para armazenamento

São voltadas para cargas que precisam de alto desempenho de disco.

Indicadas para:

- processamento de grandes volumes de dados;
- bancos de dados com uso intenso de armazenamento;
- aplicações com muitas operações de leitura e escrita.

### GPU

Incluem placas gráficas dedicadas.

Podem ser utilizadas em:

- inteligência artificial;
- machine learning;
- renderização;
- processamento gráfico;
- simulações.

## Escolha do tamanho correto

O dimensionamento deve considerar a necessidade real da aplicação.

Antes de escolher uma VM, é importante avaliar:

- utilização de CPU;
- memória necessária;
- espaço em disco;
- quantidade de operações de entrada e saída;
- tráfego de rede;
- quantidade de usuários;
- crescimento esperado;
- custo mensal.

Uma abordagem comum é começar com um tamanho compatível com a carga prevista e ajustar após observar métricas reais.

## Escalabilidade vertical e horizontal

Existem duas formas principais de aumentar a capacidade de processamento.

### Escalabilidade vertical

Consiste em aumentar os recursos da própria VM.

Exemplo:

```text
2 vCPUs / 8 GB RAM
        ↓
4 vCPUs / 16 GB RAM
```

É simples, mas existe um limite máximo para o tamanho da máquina.

### Escalabilidade horizontal

Consiste em adicionar mais instâncias.

Exemplo:

```text
VM
↓
VM + VM + VM
```

Essa estratégia pode ser utilizada junto com balanceadores de carga e conjuntos de dimensionamento.

## Virtual Machine Scale Sets

O **Virtual Machine Scale Sets (VMSS)** permite criar e administrar várias instâncias de máquinas virtuais de forma centralizada.

Ele pode ajudar em cenários que precisam de:

- alta disponibilidade;
- distribuição de carga;
- aumento automático de capacidade;
- redução automática de instâncias quando a demanda diminui.

Exemplo:

```text
Usuários
   ↓
Load Balancer
   ↓
VM1   VM2   VM3
```

## Discos

As VMs utilizam discos para armazenar o sistema operacional e os dados.

Normalmente existem:

- **OS Disk:** contém o sistema operacional;
- **Data Disks:** utilizados para dados adicionais;
- **Temporary Disk:** armazenamento temporário disponível em alguns tipos de VM.

A Azure oferece diferentes opções de desempenho de disco, e a escolha deve considerar:

- capacidade;
- IOPS;
- throughput;
- latência;
- custo.

## Rede

A máquina virtual normalmente é conectada a uma **Virtual Network (VNet)**.

Uma configuração básica pode incluir:

```text
VNet
└── Subnet
     └── Network Interface
          └── Virtual Machine
```

Dependendo do cenário também podem existir:

- IP público;
- IP privado;
- Network Security Group;
- Load Balancer;
- VPN;
- Azure Bastion.

## Network Security Group

O **Network Security Group (NSG)** controla o tráfego de entrada e saída utilizando regras.

Por exemplo:

```text
Internet
   ↓
NSG
   ├── Porta 80  → HTTP
   ├── Porta 443 → HTTPS
   └── Porta 22  → SSH, somente quando necessário
```

Uma boa prática é liberar somente as portas realmente necessárias.

## Segurança

Algumas boas práticas importantes ao trabalhar com VMs são:

1. evitar exposição pública desnecessária;
2. aplicar o princípio do menor privilégio;
3. utilizar autenticação segura;
4. restringir portas no NSG;
5. manter o sistema operacional atualizado;
6. utilizar Azure RBAC para controle de acesso;
7. proteger credenciais e segredos;
8. habilitar logs e monitoramento.

## Monitoramento

Depois de criar uma VM, o dimensionamento deve ser acompanhado por métricas reais.

O **Azure Monitor** pode ajudar a observar informações como:

- uso de CPU;
- memória;
- utilização de disco;
- tráfego de rede;
- disponibilidade;
- alertas;
- logs.

Essas métricas ajudam a identificar se a VM está superdimensionada ou subdimensionada.

## Custos

O custo de uma VM depende de diferentes fatores.

Entre eles:

- tamanho da máquina;
- região;
- sistema operacional;
- quantidade e tipo de discos;
- tráfego de rede;
- tempo em execução;
- serviços adicionais.

Em ambientes de estudo, é importante lembrar que uma VM pode continuar gerando custos enquanto estiver provisionada.

Quando o recurso não for mais necessário, é recomendado verificar se ele pode ser:

- desligado;
- desalocado;
- redimensionado;
- excluído.

Também é importante analisar recursos associados, como discos e IPs públicos.

## Boas práticas de dimensionamento

Algumas práticas importantes são:

1. escolher a VM de acordo com a carga real;
2. evitar superdimensionamento;
3. acompanhar métricas de utilização;
4. redimensionar quando necessário;
5. separar dados importantes em discos apropriados;
6. restringir acessos de rede;
7. utilizar alta disponibilidade quando o sistema exigir;
8. avaliar escalabilidade horizontal para cargas variáveis;
9. acompanhar custos continuamente;
10. remover recursos de laboratório que não serão mais utilizados.

## Exemplo de decisão de dimensionamento

Imagine uma aplicação web pequena em ambiente de desenvolvimento.

Inicialmente, pode ser suficiente utilizar:

```text
Aplicação pequena
      ↓
VM de uso geral
      ↓
Poucas vCPUs
      +
Memória moderada
      +
Disco gerenciado
```

Após a aplicação entrar em produção, as métricas podem mostrar aumento de utilização.

Nesse caso, algumas opções seriam:

```text
Aumentar o tamanho da VM
        ou
Adicionar novas instâncias
        ou
Otimizar a aplicação
```

A melhor escolha depende do tipo de gargalo identificado.

## Resumo

Configurar uma Máquina Virtual na Azure envolve mais do que simplesmente selecionar CPU e memória.

É necessário equilibrar:

```text
Desempenho
    +
Disponibilidade
    +
Segurança
    +
Escalabilidade
    +
Custo
```

O dimensionamento correto deve considerar a necessidade atual da aplicação, monitorar o comportamento real da carga e permitir ajustes ao longo do tempo.

## Referências

- [Documentação do Azure Virtual Machines](https://learn.microsoft.com/pt-br/azure/virtual-machines/)
- [Tamanhos de Máquinas Virtuais no Azure](https://learn.microsoft.com/pt-br/azure/virtual-machines/sizes/)
- [Azure Virtual Machine Scale Sets](https://learn.microsoft.com/pt-br/azure/virtual-machine-scale-sets/)
- [Azure Monitor](https://learn.microsoft.com/pt-br/azure/azure-monitor/)
- [Documentação do GitHub](https://docs.github.com/)
- [Markdown no GitHub](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

---

**Desafio de Projeto — DIO**
