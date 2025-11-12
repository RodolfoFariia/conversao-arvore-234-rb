# 🌳 Árvores 2-3-4 & Rubro-Negra

![CMake](https://img.shields.io/badge/CMake-3.10%2B-blue) ![Language](https://img.shields.io/badge/Linguagem-C-brightgreen)

> Implementação e estudo de Árvore 2-3-4, Árvore Rubro-Negra e conversão entre elas, além de benchmarks de desempenho.

---

## 🎯 Objetivos e Conceitos-Chave

Este não é apenas um repositório de implementações, mas um estudo prático sobre a equivalência e o desempenho de árvores de busca balanceadas. Os principais conceitos demonstrados são:

1.  **Implementação do Zero:** As árvores **2-3-4** e **Rubro-Negra (RBT)** foram implementadas do zero em C, com gerenciamento manual de memória (`malloc`/`free`) e manipulação de ponteiros.
2.  **Lógica de Auto-Balanceamento:**
    * **Árvore 2-3-4:** Implementação da lógica de `split` (divisão) na inserção e `merge` (fusão) na remoção para manter a árvore perfeitamente balanceada.
    * **Árvore Rubro-Negra:** Implementação das 5 propriedades da RBT, com lógica de **recoloração** e **rotações** (simples e duplas) para corrigir violações.
3.  **Isomorfia (A Conversão):** A prova prática de que Árvores 2-3-4 e Árvores Rubro-Negras são isomórficas (estruturalmente equivalentes). Este projeto demonstra como uma RBT é, na essência, uma representação binária de uma Árvore 2-3-4.
4.  **Análise de Desempenho:** Comparação de performance (benchmarks) entre as duas estruturas para analisar o custo computacional de suas diferentes estratégias de balanceamento.

---

## 📂 Estrutura do Projeto

```
TRABALHO02/
│
├── arv234/             # Implementação da Árvore 2-3-4
│   ├── arv234.c        # Código-fonte
│   ├── arv234.h        # Cabeçalhos de funções e structs
│   └── CMakeLists.txt  # Configuração CMake
│
├── arvrb/              # Implementação da Árvore Rubro-Negra
│   ├── arvrb.c
│   ├── arvrb.h
│   └── CMakeLists.txt
│
├── conversao/          # Conversão 2-3-4 → Rubro-Negra
│   ├── conversao.c
│   ├── conversao.h
│   └── CMakeLists.txt
│
├── benchmark/          # Testes de desempenho (benchmark)
│   ├── experimentos.c
│   ├── experimentos.h
│   └── CMakeLists.txt
│
├── main.c              # Programa principal (menu, interface)
├── testes.c            # Casos de teste
├── CMakeLists.txt      # CMake principal
└── .gitignore          # Arquivos/pastas ignorados pelo Git
```

---

## 🛠️ Pré-requisitos

* CMake ≥ 3.10
* Compilador C (GCC, Clang, MSVC)
* Sistema operacional: Linux, macOS ou Windows

---

## 🚀 Como Compilar

```bash
# 1. Criar pasta de build e entrar nela
e:
mkdir build && cd build

# 2. Gerar arquivos de projeto via CMake
e:
cmake ..

# 3. Compilar tudo
e:
make
```

> Todos os módulos serão compilados como bibliotecas estáticas e ligados ao executável principal.

---

## 🎯 Como Executar

Após `make`, você terá os executáveis:

| Executável   | Descrição                                  |
| ------------ | ------------------------------------------ |
| `trabalho02` | Interface principal com menu de operações  |

```bash
# Exemplo de execução:
./trabalho02

```

---

## 🔗 A Mágica da Conversão: 2-3-4 ➔ Rubro-Negra

A parte central deste trabalho é a função de conversão, que prova a **isomorfia (equivalência estrutural)** entre essas duas árvores. Uma Árvore Rubro-Negra pode ser vista como uma forma diferente de representar uma Árvore 2-3-4 no formato binário.



A lógica de conversão (`conversao/conversao.c`) segue o mapeamento direto entre os nós:

| Nó da Árvore 2-3-4 | Representação na Árvore Rubro-Negra (RBT) |
| :--- | :--- |
| **Nó-2** (1 chave) | **1 Nó Preto** (com 2 filhos pretos/NIL) |
| **Nó-3** (2 chaves) | **1 Nó Preto** (Pai) com **1 Filho Vermelho** |
| **Nó-4** (3 chaves) | **1 Nó Preto** (Pai) com **2 Filhos Vermelhos** |

A função `converte234ParaRB` percorre a Árvore 2-3-4 e aplica essa transformação nó a nó, gerando uma Árvore Rubro-Negra perfeitamente válida e balanceada como resultado.

## 📊 Benchmarks

O módulo `benchmark` mede algumas métricas de inserção e remoção em diferentes cenários.
Ele grava resultados em arquivos CSV para posterior análise.

```bash
# Exemplo de uso:
./trabalho02 --benchmark
```

---

## 📝 Documentação das APIs

### Árvores 2-3-4 (`arv234.h`)

```c
// Estrutura principal
typedef struct arvore234 arv234;

// Cria e inicializa a árvore
arv234 *alocaArvore();

// Insere uma chave
void inserir(arv234 *arv, int chave);

// Remove uma chave
void removeChave(arv234 *arv, int chave);

// Percorre em pré-ordem\ void percorrePreOrdem(no *n, int nivel);
// ... e outras funções de consulta (getAltura, getQtdSplits, etc.)
```

### Árvore Rubro-Negra (`arvrb.h`)

```c
// Cria e inicializa a árvore
arvRb *rb_alocaArvore();

// Insere e remove nó
void rb_insereNo(arvRb *arv, noRB *novoNo);
int rb_removeNo(arvRb *arv, int chave);

// Percorre e obtém raiz
void rb_percorrePreOrdem(arvRb *arv, noRB *n);
noRB *rb_getRaiz(arvRb *arv);
```

### Conversão (`conversao.h`)

```c
// Converte árvore 2-3-4 inteira para RB
arvRb *converte234ParaRB(arv234 *arv234Orig);

// Converte nó a nó
noRB *converteNo(arvRb *arvoreRB, no *no234);
```

---

## 📝 .gitignore

Este projeto já inclui um `.gitignore` para ignorar:

* Diretório de build (`/build/`)
* Artefatos CMake (`CMakeFiles/`, `CMakeCache.txt`, etc.)
* Binários e objetos (`*.o`, `*.exe`, etc.)
* Configurações do VSCode (`.vscode/`)

---

## 🤝 Contribuições

1. Fork deste repositório
2. Crie uma feature branch: `git checkout -b feature/nova-funcionalidade`
3. Faça commits claros: `git commit -m "Adiciona ..."`
4. Abra pull request

---


<p align="center">Feito por Rodolfo Henrique Faria e Rafael S. P. B. Leite</p>
