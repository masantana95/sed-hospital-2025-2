# sed-hospital-2025-2
# Simulação de Eventos Discretos - Processo de Exames Hospitalares

## 📋 Descrição do Projeto

Este projeto implementa uma simulação de eventos discretos utilizando a biblioteca **SimPy** para modelar o fluxo completo de processamento de exames de imagem  em um ambiente hospitalar, no Hospital Risoleta Tolentino Neves. A simulação acompanha cada exame desde sua entrada no sistema até a análise final dos resultados, considerando múltiplos recursos, filas e decisões baseadas em probabilidades.

## 🎯 Objetivo

O objetivo principal é analisar e otimizar o fluxo de exames hospitalares, identificando gargalos, tempos de espera e utilização de recursos. A simulação permite:

- Avaliar o desempenho do sistema sob diferentes condições
- Identificar recursos sobrecarregados ou subutilizados
- Quantificar tempos de fila e duração total de processamento
- Comparar o fluxo de exames urgentes vs. não urgentes
- Simular cenários de capacidade de recursos variável

## 🔄 Fluxo do Processo

O processo modelado segue a seguinte estrutura:

1. **Organizar pedidos** (Auxiliar Administrativo)
2. **Avaliar prioridade** (Médico) → Define se o exame é urgente ou não
3. **Agendar exame** (Auxiliar Administrativo) - apenas para exames não urgentes
4. **Informar pré-requisitos** (Equipe de Enfermagem)
5. **Preparar paciente** (Equipe de Enfermagem)
6. **Transportar paciente** (Técnico de Enfermagem)
7. **Avaliação de realização de exame** (Sem recurso)
8. **Loop de reagendamento** - se paciente não estiver preparado (máximo 3 iterações)
9. **Avaliar se exame pode ser feito no hospital** (Sem recurso)
10. **Realizar exame** - fora do hospital (Sem recurso) OU no hospital (Equipe de Enfermagem)
11. **Retorno do paciente ao PS** (Equipe de Enfermagem) - apenas para exames no hospital (processo paralelo)
12. **Atualizar MV** (Sem recurso)
13. **Analisar exame** - urgente (Sem recurso) OU não urgente (Sem recurso)

## 📊 Recursos Modelados

- **Auxiliar Administrativo** (capacity = 1)
- **Médico** (capacity = 1)
- **Equipe de Enfermagem** (capacity = 1)
- **Técnico de Enfermagem** (capacity = 1)

> **Nota**: A capacidade dos recursos ainda não traduz a realidade do hospital e pode ser ajustada para simular diferentes cenários operacionais.

## 📈 Relatórios Gerados

A simulação gera três relatórios detalhados:

### 1. Relatório por Exame
- Identificação do exame
- Classificação (urgente/não urgente)
- Tempo total em fila
- Tempo total no fluxo
- Instante de chegada
- Instante de saída

### 2. Relatório por Atividade
- Nome da atividade
- Tempo médio em fila
- Tempo total em fila
- Percentual do tempo de fila sobre o sistema
- Duração média do processo
- Duração total do processo
- Percentual da duração sobre o sistema

### 3. Relatório por Recurso
- Identificação do recurso
- Capacidade
- Tempo em uso
- Tempo total disponível
- Percentual de utilização
- Percentual de ociosidade

## ⚙️ Parâmetros de Configuração

### Distribuições de Tempo
Todos os tempos de processo seguem uma **distribuição triangular** com parâmetros (min=1, max=5, mode=3), exceto:
- **Análise de exame urgente**: reduzida em 70% (fator 0.3)

### Parâmetros de Decisão
- **x = 0.3** (30%) - Probabilidade de um exame ser urgente
- **y = 0.3** (30%) - Probabilidade de paciente não estar preparado (reagendamento)
- **z = 0.3** (30%) - Probabilidade de exame ser realizado fora do hospital
- **Taxa de chegada** = 10 minutos (distribuição exponencial)
- **Tempo de simulação** = 100 unidades de tempo
- **Limite de reagendamentos** = 3 tentativas

> ⚠️ **Versão Atual**: Os parâmetros estão hard-coded no código. Em versões futuras, serão implementadas interfaces para input dinâmico de parâmetros, permitindo maior flexibilidade na configuração de cenários de simulação.

## 🛠️ Tecnologias Utilizadas

- **Python 3.12+**
- **SimPy** - Framework de simulação de eventos discretos
- **Pandas** - Manipulação e análise de dados
- **Random** - Geração de números aleatórios

## 📦 Instalação

```bash
pip install simpy pandas
```

## 🚀 Como Executar

```python
# Execute o notebook Jupyter ou o script Python
python Trabalho_1_Código.py
```

## 📝 Estrutura do Código

- `geraChegadasExames()` - Gera chegadas de exames no sistema
- `procExames()` - Processo principal de cada exame
- `procPreRealizacaoExames()` - Subprocesso de preparação para exames
- `procRetornoPaciente()` - Processo paralelo de retorno do paciente
- `AtividadeCom1RecursoExame()` - Handler para atividades que consomem recursos
- `AtividadeCom1RecursoPaciente()` - Handler específico para atividades do paciente
- `AtividadeSemRecurso()` - Handler para atividades sem uso de recursos
- `distribuicoes()` - Define os tempos de processo

## 🔮 Próximos Passos

- [ ] Interface para configuração dinâmica de parâmetros
- [ ] Análise estatística dos resultados
- [ ] Visualizações gráficas dos relatórios
- [ ] Exportação de relatórios para CSV/Excel

## 📄 Licença

Este projeto foi desenvolvido para fins acadêmicos e de pesquisa.

## 👤 Autor

Desenvolvido como parte de trabalho acadêmico da disciplina EPD 899 - TÓPICOS EM OTIMIZAÇÃO: Simulação de Sistemas Logísticos, do Programa de Pós-Graduação em Engenharia de Produção, da Universidade Federal de Minas Gerais (UFMG), pelo Prof. Dr. João Flávio de Freitas Almeida (http://lattes.cnpq.br/9513742728448307 ; github.com/joaoflavioufmg)
