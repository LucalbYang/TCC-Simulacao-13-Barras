# Simulador SEP - 13 Barras (TCC)

Este é um aplicativo desktop interativo e focado na simulação e análise de Sistemas Elétricos de Potência (SEP), utilizando como base o sistema teste IEEE de 13 barras. O simulador permite manipulação direta de parâmetros, visualização interativa do diagrama unifilar, e simulação rápida baseada no PyPower/PandaPower.

## Características:
- **Fluxo de Potência**: Utiliza a biblioteca `pandapower` para calcular tensões nodais, fluxo de potência e perdas nas linhas.
- **Curva PV (Curva do Nariz)**: Geração interativa da curva de tensão por potência, permitindo identificar o ponto exato de colapso de tensão.
- **Interface Gráfica Interativa**:
    - Desenvolvida em `PyQt6` com um tema escuro (*Dark Mode*) de visualização amigável.
    - Diagrama unifilar interativo feito com `QGraphicsView` onde o estado (tensão) das barras muda de cor, alertando problemas.
    - Edição rápida de parâmetros da rede (Cargas, Geração, Impedâncias) com duplo clique sobre barras ou linhas.
    - Gráficos integrados gerados e atualizados usando `matplotlib`.
- **Multithreading**: O fluxo de simulação roda assincronamente através de um `QThread`, garantindo que a interface se mantenha perfeitamente fluida.
- **Gerenciador de Cenários**: Salve modificações temporárias ou específicas em cenários e navegue facilmente entre eles, carregando os parâmetros automaticamente (o salvamento de cenários salva as modificações diretamente das tabelas antes).
- **Relatórios**: Geração e exportação de resultados direto para planilhas em Excel (`.xlsx`).

## Requisitos de Sistema e Dependências
O software foi desenvolvido e testado usando Python 3.8+.
As seguintes bibliotecas Python são necessárias (presentes no arquivo `requirements.txt`):

- PyQt6
- pandapower
- matplotlib
- scipy
- openpyxl
- numpy
- pyinstaller (para compilar)

## Como Instalar

Instale todas as dependências recomendadas utilizando o pip:

```bash
pip install -r requirements.txt
```
*(ou instale manualmente: `pip install PyQt6 pandapower matplotlib scipy openpyxl numpy`)*

## Arquitetura (Organização dos Arquivos)
A base do projeto foi modularizada em vários arquivos distintos para separação de responsabilidades (Model-View-Controller).

1. `data_models.py`: Gerenciamento do estado e armazenamento de dados baseados em DataClasses. Manipula os dados de barra, linha e cabos de maneira centralizada.
2. `engine_sep.py`: O "Motor" por trás do simulador. Monta a rede internamente, encapsula as integrações do `pandapower` e roda cálculos pesados de Fluxo de Potência e Geração da Curva PV.
3. `diagram_view.py`: A visualização interativa da rede (`QGraphicsScene/View`). Renderiza o diagrama unifilar, captura os cliques dos usuários, realiza zoom dinâmico e desenha o sistema usando geometrias dinâmicas.
4. `plot_utils.py`: Funções utilitárias e integração do `matplotlib` com o `PyQt6` para plotar a Curva PV e gerenciar outros gráficos.
5. `ui_main.py`: A estrutura principal da Janela e de todas as abas. Guarda definições diretas de Interface do Usuário (layouts, tabelas, botões, barras e as configurações do QSS/Tema escuro).
6. `main.py`: O Controlador Central e *Entry Point* do aplicativo. Conecta os cliques, preenche as tabelas, carrega a configuração/cenários e gerencia as threads de execução para sincronizar a simulação elétrica com a tela.
7. `build.py`: Script autônomo para compilação via `pyinstaller`. Apaga caches anteriores e cria o pacote final na pasta `/dist/`.

## Como Executar

Execute o script principal a partir da raiz do projeto:

```bash
python main.py
```

## Como Gerar o Executável

Caso deseje transformar a aplicação em um arquivo standalone (um único executável sem necessidade de Python nativo):

```bash
python build.py
```
O executável gerado será salvo na pasta `dist`.