<div align="center">

# REXA

### Reverse Engineering eXtensible Analyzer

*Framework moderno de engenharia reversa de alta performance construído em Rust*

[![Licença](https://img.shields.io/badge/licença-MIT%2FApache--2.0-blue.svg)](#licença)
[![Versão Rust](https://img.shields.io/badge/rust-1.75+-orange.svg)](https://www.rust-lang.org)
[![Status Build](https://img.shields.io/badge/build-passing-brightgreen.svg)](#)
[![PRs Bem-vindos](https://img.shields.io/badge/PRs-bem--vindos-brightgreen.svg)](#contribuindo)

[Recursos](#-recursos) • [Início Rápido](#-início-rápido) • [Arquitetura](#-arquitetura) • [Documentação](#-documentação) • [Contribuindo](#-contribuindo)

</div>

---

## 📖 Visão Geral

**REXA** é um framework de engenharia reversa de próxima geração projetado para velocidade, extensibilidade e análise avançada. Construído inteiramente em Rust, o REXA combina práticas modernas de engenharia de software com técnicas de análise binária de ponta para fornecer uma plataforma poderosa para pesquisadores de segurança, analistas de malware e engenheiros reversos.

### Por que REXA?

- **🚀 Extremamente Rápido**: Performance impulsionada pelo Rust com abstrações de custo zero
- **🧠 Potencializado por IA**: Integração nativa com LLMs para análise inteligente de código
- **🔌 Altamente Extensível**: Sistema rico de plugins suportando Rust, Python e Lua
- **🔬 Descompilação Avançada**: Descompilador baseado em SSA com otimizações agressivas
- **🛡️ Foco em Segurança**: Detecção de vulnerabilidades e execução simbólica integradas
- **🎯 Arquitetura Modular**: 19 crates especializados para máxima flexibilidade

---

## ✨ Recursos

### Capacidades Principais

#### 🔬 Descompilador Avançado

- **Pipeline Completo**: Assembly → IR → SSA → Otimizações → Estruturação → Código C
- **140+ Instruções x86/x64**: MOV, aritmética, SIMD (SSE/AVX), FPU, operações de string
- **6 Passes de Otimização**: 
  - Eliminação de Código Morto (DCE)
  - Dobramento e Propagação de Constantes
  - Propagação de Cópia
  - Eliminação de Subexpressões Comuns (CSE)
  - Simplificação Algébrica
  - Redução de Força
- **Recuperação de Fluxo de Controle**: Detecção automática de if/else, loops e switch statements usando o algoritmo de Cifuentes
- **Geração de Código Limpo**: Produz código C legível com precedência adequada, casts mínimos e comentários úteis

#### 🧠 Integração com IA

Aproveite o poder da IA para análise inteligente de binários:

- **Explicação de Funções**: Resumo automático do que as funções fazem
- **Renomeação Inteligente**: Sugestões inteligentes de nomes de variáveis e funções
- **Detecção de Vulnerabilidades**: Identificação de possíveis problemas de segurança com IA
- **Similaridade de Código**: Busca semântica em bases de código
- **Inferência de Tipos**: Recuperação de tipos assistida por ML
- **Suporte Multi-Provedor**: OpenAI, Anthropic e modelos locais

#### 🛡️ Análise de Segurança

Capacidades abrangentes de análise de segurança:

- **Scanner de Vulnerabilidades**: 
  - Buffer overflows e underflows
  - Use-after-free e double-free
  - Vulnerabilidades de format string
  - Integer overflow/underflow
  - Desreferências de ponteiro nulo
  - Race conditions
- **Banco de Dados CVE**: Pattern matching contra vulnerabilidades conhecidas
- **Execução Simbólica**: Resolução de restrições com Z3 para descoberta automática de vulnerabilidades
- **Análise de Taint**: Rastreamento de entrada não confiável através de caminhos de execução
- **Exploração de Caminhos**: Estratégias BFS/DFS com exploração guiada por cobertura

#### 🔌 Sistema de Plugins

Arquitetura de plugins poderosa e multi-linguagem:

**Plugins Rust** - Performance máxima:
```rust
use rexa_plugin::prelude::*;

#[rexa_plugin]
pub struct CryptoScanner;

impl Plugin for CryptoScanner {
    fn on_function_analyzed(&mut self, func: &Function) {
        if func.contains_constant(0x67452301) {
            self.report("Constante MD5 detectada");
        }
    }
}
```

**Plugins Python** - Prototipagem rápida:
```python
from rexa import Plugin

class MeuAnalisador(Plugin):
    def on_function_analyzed(self, func):
        if "crypto" in func.name.lower():
            self.log(f"Função cripto encontrada: {func.name}")
```

**Plugins Lua** - Scripts leves:
```lua
function on_function_analyzed(func)
    if string.match(func.name, "^crypt") then
        log("Função de criptografia encontrada")
    end
end
```

Recursos:
- **Hot-Reload**: Desenvolva plugins sem reiniciar
- **API Rica**: 200+ funções para análise, GUI e emulação
- **Orientado a Eventos**: 9 tipos de eventos com dados estruturados
- **Thread-Safe**: Execução concorrente de plugins

#### 📦 Suporte a Formatos Binários

- **PE** (executáveis Windows, DLLs, drivers)
- **ELF** (binários Linux, bibliotecas compartilhadas)
- **Mach-O** (executáveis macOS, frameworks)
- **Binário Raw** (firmware, bootloaders)

#### 🖥️ Suporte a Arquiteturas

- **x86** (Intel/AMD 32-bit)
- **x86-64** (Intel/AMD 64-bit)
- **ARM** / **ARM64** (AArch64)
- **MIPS** (planejado)
- **RISC-V** (planejado)

#### 🔍 Ferramentas de Análise Adicionais

- **Diff Binário**: Comparação em nível de função, detecção de patches e avaliação de impacto de segurança
- **Inferência de Tipos**: Propagação avançada de tipos e recuperação de estruturas
- **Sistema de Assinaturas**: Assinaturas compatíveis com FLIRT, regras YARA, identificação de bibliotecas
- **Emulador**: Análise dinâmica com emulação de CPU e interceptação de syscalls

---

## 🚀 Início Rápido

### Pré-requisitos

- **Rust** 1.75 ou superior ([Instalar Rust](https://rustup.rs/))
- **Git**

### Instalação

```bash
# Clone o repositório
git clone https://github.com/rexa-re/rexa.git
cd rexa

# Compile o projeto (modo release para melhor performance)
cargo build --release

# Execute os testes para verificar a instalação
cargo test --all

# Instale a ferramenta CLI globalmente
cargo install --path crates/rexa-cli
```

### Uso Básico

```bash
# Analise um binário
rexa analyze binary.exe

# Descompile uma função específica
rexa decompile --address 0x401000 binary.exe

# Inicie a GUI
rexa gui binary.exe

# Execute com um plugin
rexa analyze --plugin crypto-scanner binary.exe

# Exporte resultados da análise
rexa export --format json binary.exe > analysis.json
```

### Modo GUI

A GUI fornece uma interface intuitiva para análise de binários:

```bash
rexa gui /caminho/para/binario
```

Recursos:
- Visualização interativa de disassembly
- Lista de funções e grafos de chamadas
- Editor hexadecimal
- Saída do descompilador
- Gerenciamento de plugins

---

## 🏗️ Arquitetura

O REXA é organizado em 19 crates especializados, cada um lidando com um aspecto específico da análise binária:

```
rexa/
├── rexa-core          # Estruturas de dados, traits e interfaces principais
├── rexa-loader        # Parsing de formatos de arquivo binário (PE, ELF, Mach-O)
├── rexa-disasm        # Motor de disassembly (wrapper do Capstone)
├── rexa-ir            # Representação intermediária (IR)
├── rexa-decompiler    # Pipeline de descompilação de alto nível
├── rexa-analysis      # Passes de análise estática e algoritmos
├── rexa-ai            # Camada de integração com IA/LLM
├── rexa-plugins       # Sistema de plugins e API
├── rexa-cli           # Interface de linha de comando
├── rexa-gui           # Interface gráfica (egui)
├── rexa-api           # Servidor API REST para acesso remoto
├── rexa-emulator      # Emulação de CPU para análise dinâmica
├── rexa-vuln          # Motor de detecção de vulnerabilidades
├── rexa-diff          # Algoritmos de diff binário
├── rexa-types         # Sistema de tipos e inferência
├── rexa-signatures    # Pattern matching e assinaturas
├── rexa-symbolic      # Motor de execução simbólica
├── rexa-utils         # Utilitários e helpers comuns
└── rexa-filesystem    # Abstração de sistema de arquivos virtual
```

Cada crate é:
- **Testável independentemente**
- **Utilizável como biblioteca standalone**
- **Bem documentado com exemplos**
- **Seguindo as melhores práticas do Rust**

### Fluxo de Dados

```
Arquivo Binário → Loader → Disassembler → IR → Decompiler → Código de Alto Nível
                    ↓          ↓           ↓        ↓
                Analysis ← Plugins ← AI ← Types
```

---

## 📚 Documentação

- **[Roadmap](docs/ROADMAP_VISUAL.md)** - Roadmap do projeto e marcos
- **[Referência da API](https://docs.rs/rexa)** - Documentação completa da API
- **[Guia de Desenvolvimento de Plugins](docs/plugins.md)** - Crie seus próprios plugins
- **[Arquitetura Detalhada](docs/architecture.md)** - Design interno do sistema
- **[Guia de Contribuição](CONTRIBUTING.md)** - Como contribuir

---

## 🤝 Contribuindo

Agradecemos contribuições da comunidade! Seja corrigindo bugs, adicionando recursos, melhorando documentação ou criando plugins, sua ajuda é apreciada.

### Formas de Contribuir

- 🐛 **Reportar Bugs**: Abra uma issue com passos detalhados de reprodução
- ✨ **Solicitar Recursos**: Proponha novos recursos através de issues no GitHub
- 📖 **Melhorar Documentação**: Ajude-nos a tornar a documentação mais clara e abrangente
- 🧪 **Escrever Testes**: Aumente a cobertura de código e confiabilidade
- 🔌 **Criar Plugins**: Compartilhe suas ferramentas de análise com a comunidade
- 💻 **Submeter Código**: Corrija bugs ou implemente novos recursos

### Configuração de Desenvolvimento

```bash
# Faça fork do repositório no GitHub, então:
git clone https://github.com/SEU_USUARIO/rexa.git
cd rexa

# Crie um branch de feature
git checkout -b feature/meu-recurso-incrivel

# Faça suas mudanças e teste
cargo test --all
cargo clippy --all-targets --all-features
cargo fmt --all

# Commit e push
git commit -am "Adiciona meu recurso incrível"
git push origin feature/meu-recurso-incrivel
```

Então abra um Pull Request no GitHub com uma descrição clara de suas mudanças.

### Padrões de Código

- Siga os idiomas e melhores práticas do Rust
- Escreva testes para novas funcionalidades
- Documente APIs públicas com doc comments
- Execute `cargo fmt` e `cargo clippy` antes de commitar
- Mantenha commits focados e atômicos

Veja [CONTRIBUTING.md](CONTRIBUTING.md) para diretrizes detalhadas.

---

## 🛣️ Roadmap

### Curto Prazo (Q1 2026)

- [ ] Sistema completo de inferência de tipos
- [ ] Expandir suporte a arquiteturas (melhorias ARM64)
- [ ] Recursos aprimorados da GUI
- [ ] Cobertura de testes >70%
- [ ] Documentação abrangente

### Médio Prazo (Q2-Q3 2026)

- [ ] Recursos de IA prontos para produção
- [ ] Scanner avançado de vulnerabilidades
- [ ] Melhorias no diff binário
- [ ] Otimizações de performance
- [ ] Marketplace de plugins

### Longo Prazo (Q4 2026+)

- [ ] Análise distribuída baseada em nuvem
- [ ] Recursos de colaboração em tempo real
- [ ] Recursos empresariais (SSO, logs de auditoria)
- [ ] Suporte a arquiteturas mobile
- [ ] Capacidades avançadas de emulação

Veja [docs/ROADMAP_VISUAL.md](docs/ROADMAP_VISUAL.md) para o roadmap completo.

---

## 📊 Status do Projeto

REXA está em desenvolvimento ativo. Componentes principais estão funcionais, com melhorias contínuas sendo feitas.

**Foco Atual**: Estabilidade, testes e documentação

**Status de Release**: Alpha (v0.1.0)

---

## 📄 Licença

REXA é licenciado sob dupla licença à sua escolha:

- **Licença MIT** ([LICENSE-MIT](LICENSE-MIT))
- **Licença Apache 2.0** ([LICENSE-APACHE](LICENSE-APACHE))

Isso significa que você pode usar o REXA sob os termos de qualquer uma das licenças.

---

## 🙏 Agradecimentos

REXA é construído sobre excelentes projetos open-source:

- **[Capstone](http://www.capstone-engine.org/)** - Framework de disassembly multi-arquitetura
- **[Goblin](https://github.com/m4b/goblin)** - Biblioteca de parsing de binários para Rust
- **[egui](https://github.com/emilk/egui)** - Framework de GUI em modo imediato
- **[Z3](https://github.com/Z3Prover/z3)** - Solver SMT para execução simbólica
- **Comunidade Rust** - Por criar um ecossistema incrível

Agradecimentos especiais a todos os contribuidores e apoiadores do projeto.

---

## ⚠️ Aviso Legal

REXA é projetado para pesquisa de segurança legítima, educação e análise de software. Os usuários são responsáveis por garantir que seu uso esteja em conformidade com todas as leis e regulamentos aplicáveis. Os desenvolvedores não assumem responsabilidade pelo uso indevido desta ferramenta.

---

<div align="center">

**Construído com ❤️ em Rust**

[GitHub](https://github.com/rexa-re/rexa) • [Documentação](https://docs.rs/rexa) • [Reportar Bug](https://github.com/rexa-re/rexa/issues) • [Solicitar Recurso](https://github.com/rexa-re/rexa/issues)

⭐ **Dê uma estrela no GitHub** se você achar o REXA útil!

</div>
