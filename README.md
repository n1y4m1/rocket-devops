# rocket-devops

## Plano de Ação DevOps

### 1. Diagnóstico Cultural
1. **Workshop Interfuncional**  
   - Conduzir uma sessão com Desenvolvedores e Operações para apresentar objetivos de CI/CD, IaC e ganhos esperados.  
   - Mapear processos atuais de entrega de código e deploy manual.  
   - Documentar principais pontos de atrito (tempo de espera, falhas, responsabilidades).

2. **Mapeamento de Ambiente e Ferramentas**  
   - Inventariar repositórios Git, linguagens (Java, C#, JavaScript, Delphi), frameworks e dependências.  
   - Levantar infraestrutura de produção, homologação e desenvolvimento (servidores, banco de dados, sistemas legados).  
   - Identificar gaps de automação e padronização (scripts ausentes, playbooks informais).

3. **Definição de Métricas-Alvo**  
   - Estabelecer metas iniciais para Lead Time (≤ 8 horas), Deployment Frequency (3–5 por semana), MTTR (< 1 hora), Change Failure Rate (< 10 %), cobertura de testes (> 70 %), tempo de provisionamento de infraestrutura (< 30 min).  
   - Documentar métricas atuais como referência (Lead Time: 48 h, Taxa de Sucesso de Deploy: 80 %, Incidentes: 2/semana, MTTR: 4 h).

4. **Engajamento das Equipes**  
   - Apresentar roadmap de implementação para GPs de Dev e Ops.  
   - Coletar feedback inicial e identificar possíveis resistências.  
   - Alocar “champions” em Dev e Ops para facilitar comunicação e adesão.

---

### 2. Automação de Build, Testes e Deploy
1. **Configurar Repositório Git Centralizado**  
   - Migrar todos os projetos (Java, C#, JavaScript, Delphi) para Git (GitLab ou GitHub Enterprise).  
   - Definir estratégia de branching (Trunk-Based ou GitFlow leve).

2. **Prova de Conceito (PoC) de CI/CD**  
   - Escolher um módulo do Sistema de Gestão de Vendas (Java).  
   - Instalar e configurar servidor de CI (Jenkins, GitLab CI ou similar).  
   - Criar pipeline mínimo: build → testes unitários/integrados → análise estática (SonarQube) → geração de artefato.  
   - Validar sucesso do PoC e documentar aprendizados.

3. **Criação de Pipelines por Projeto**  
   - **Java**:  
     - Build com Maven/Gradle; testes JUnit; análise SonarQube; empacotamento de WAR/Docker.  
   - **C#/.NET**:  
     - Build com MSBuild; testes NUnit/xUnit; análise estática (SonarQube); empacotamento de contêiner ou deploy Win.  
   - **JavaScript (Plataforma de E-commerce)**:  
     - Build com npm/Yarn; testes Jest/Cypress; bundles otimizados; Docker.  
   - **Delphi (Legado)**:  
     - Configurar compilação via linha de comando; gerar artefato instalável ou contêiner Windows; incluir scripts de migração de banco.

4. **Automação de Infraestrutura (IaC)**  
   - Definir repositório de infraestrutura em Terraform (AWS/Azure/on-prem).  
   - Escrever playbooks Ansible para configuração de servidores (Linux e Windows), banco de dados e variáveis de ambiente.  
   - Criar módulos reutilizáveis para provisionamento de VMs, balanceadores e clusters de contêineres.

5. **Pipeline de Entrega Contínua (CD)**  
   - **Ambiente de Homologação (Staging)**:  
     - Deploy automático de cada build bem-sucedido; execução de testes de aceitação (Selenium, Cypress).  
     - Scripts automáticos de migração de banco e criação de dados de teste.  
   - **Promoção para Produção**:  
     - Aprovação manual ou automática após validação em staging.  
     - Deploy em produção via IaC/Ansible: provisionar recursos, executar scripts de migração, atualizar contêineres bLue-Green ou canary.  
     - Health checks automáticos (endpoints críticos, métricas de performance).

6. **Segurança de Credenciais e Variáveis**  
   - Integrar secrets manager (HashiCorp Vault, AWS Secrets Manager).  
   - Configurar variáveis de pipeline com roles e permissões restritas.  
   - Auditoria de acessos e logs de pipeline.

7. **Treinamento e Onboarding**  
   - Sessões hands-on de criação e manutenção de pipelines para Dev e Ops.  
   - Workshops específicos de Terraform e Ansible.  
   - Documentar templates de pipelines e playbooks básicos em repositório compartilhado.

---

### 3. Métricas, Monitoramento e Compartilhamento de Conhecimento
1. **Coleta e Visualização de Métricas**  
   - Integrar ferramentas de monitoramento (Prometheus + Grafana ou Datadog) para métricas de aplicação (CPU, memória, latência, erros).  
   - Configurar dashboards para acompanhar:  
     - Lead Time (commit → produção)  
     - Deployment Frequency  
     - MTTR (tempo de restauração)  
     - Change Failure Rate  
     - Cobertura de testes  
     - Tempo de provisionamento de infraestrutura  

2. **Relatórios Automáticos**  
   - Gerar relatórios semanais de pipeline (builds bem-sucedidos, falhas e motivos) e compartilhar em canal Slack/Teams.  
   - Notificações via Slack/Teams quando métricas-chave estiverem fora do esperado (por ex., MTTR > 2 h, erro 5xx > 1 % do tráfego).

3. **Postmortems e Lições Aprendidas**  
   - Implementar processo de postmortem sem culpa para cada incidente ou falha de deploy.  
   - Documentar causas, ações corretivas e plano de prevenção em repositório “postmortems” centralizado.  
   - Compartilhar resumos mensais de lições aprendidas em sessão “Lunch & Learn”.

4. **Repositório de Conhecimento**  
   - Criar Wiki interna (Confluence ou GitHub Pages) com:  
     - Diagramas de arquitetura e fluxo de deploy  
     - Modelos de pipeline YAML e playbooks Ansible/Terraform  
     - Checklist de deploy e validações automatizadas  
     - Glossário de práticas DevOps (CI/CD, IaC, Canary, Blue-Green)  

5. **Sessões de Disseminação de Conhecimento**  
   - **“Lunch & Learn” Mensais** com temas rotativos (Ansible, Terraform, testes automatizados, monitoramento).  
   - **“DevOps Days” Trimestrais** para apresentar novos scripts, ferramentas ou casos de sucesso internos.  
   - **Canal Slack/Teams #devops-tech** para dúvidas, compartilhamento de scripts e alertas em tempo real.

---

### 4. Aplicação das Três Maneiras do DevOps

#### 4.1 Primeira Maneira: Acelerar o Fluxo (Flow)
- **Padronizar Branching e Pipeline**  
  - Mínimo de branches long-lived; merges diários em `main` para feedback rápido via CI.  
  - Caching de dependências (Maven, npm/Yarn) e paralelização de testes.

- **Deploy Imutável**  
  - Construir imagens Docker/VM imutáveis para cada versão; substituir instâncias em vez de patch manual.  
  - Implementar Blue-Green ou Canary Releases para reduzir riscos de rollback.

- **Playbooks Idempotentes**  
  - Todos os scripts de deploy, migração de banco e configuração de servidores devem ser idempotentes e versionados.  
  - Eliminar procedimentos manuais (login SSH, cópias manuais) substituindo por automação.

#### 4.2 Segunda Maneira: Ampliar o Feedback (Feedback)
- **Monitoramento Proativo**  
  - Coletar métricas de serviço (latência, taxa de erro) e logs em Elasticsearch/Prometheus.  
  - Alertas configurados para thresholds críticos (ex.: erro 5xx > 1 % do tráfego, latência > 500 ms).

- **Feedback no Pipeline**  
  - Testes de performance e ponta-a-ponta (end-to-end) em staging; falhas acionam notificações diretas aos autores do último commit.  
  - Relatório diário de pipeline enviado automaticamente via Slack/Teams.

- **Revisão Contínua de Código**  
  - Pull Requests com ao menos dois reviewers (Dev + QA).  
  - Linters e análise estática bloqueando merges se cobertura de testes cair abaixo do mínimo.

- **Feedback de Clientes via Feature Toggles**  
  - Habilitar funcionalidades para um grupo restrito de usuários antes de rollout completo.  
  - Coletar feedback em tempo real e ajustar backlog conforme necessidades relatadas.

#### 4.3 Terceira Maneira: Experimentar e Aprender (Continuous Learning)
- **Postmortems sem Culpa**  
  - Documentar cada incidente, causas e correções em repositório público interno.  
  - Realizar reunião mensal para revisar lições aprendidas e atualizar playbooks.

- **Experimentação Controlada**  
  - Planejar “Hack Days” trimestrais para prototipar novas automações (scripts de rollback, chatbots de deploy).  
  - Definir hipóteses mensuráveis (ex.: reduzir tempo de provisionamento em X %) e validar com métricas.

- **Retrospectives Ágeis**  
  - Time de DevOps trabalha em sprints de 2 semanas, com retrospectives para revisar sucessos e falhas do pipeline.  
  - Manter backlog de melhorias contínuas em ferramentas e processos.

- **Capacitação e Certificações**  
  - Incentivar treinamentos em Docker, Kubernetes, AWS DevOps Engineer ou Azure DevOps.  
  - Criar grupo de estudo interno (“Guilda DevOps”) para troca de experiências e melhores práticas.

---

## Cronograma Resumido (Exemplo)
| Fase                      | Atividades Principais                                      | Prazo Estimado     |
|---------------------------|------------------------------------------------------------|--------------------|
| **Diagnóstico (Semana 1–2)**      | Workshop, mapeamento de processos, definição de métricas     | 2 semanas          |
| **PoC de CI/CD (Semana 3–4)**      | Construção de pipeline para módulo Java, testes e validação   | 2 semanas          |
| **Escalonamento (Mês 2–3)**      | Pipelines para C#, JS, Delphi; IaC com Terraform/Ansible       | 2 meses            |
| **Deploy em Homologação (Mês 3)** | Automação de deploy em staging, testes de aceitação            | 1 mês              |
| **Produção Automática (Mês 4)**   | Deploy automatizado em produção, Blue-Green/Canary             | 1 mês              |
| **Otimização Contínua (Mês 5–6)** | Monitoramento, postmortems, treinamentos e hackathons          | 2 meses            |

---

### Observações Finais
- Ajustar prazos conforme disponibilidade de recursos e prioridades de negócio.  
- Revisar métricas mensalmente e ajustar objetivos conforme evolução.  
- Garantir documentação sempre atualizada no repositório compartilhado.  
- Fomentar cultura DevOps por meio de incentivos, reconhecimento e comunicação transparente.
