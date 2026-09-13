# 🛡️ Guia de Proteção de Dados em Angola para Devs

### Como não seres apanhado de surpresa pela APD

> Guia prático (não é aconselhamento jurídico) sobre a Lei de Proteção de Dados Pessoais de Angola, escrito a pensar em quem tem de tomar decisões de arquitetura, schema e infraestrutura — não em advogados.
> Última atualização: setembro de 2026.

---

## Índice

1. [TL;DR — o essencial em 90 segundos](#1-tldr--o-essencial-em-90-segundos)
2. [O panorama legal](#2-o-panorama-legal)
3. [A quem se aplica isto](#3-a-quem-se-aplica-isto)
4. [Dados pessoais vs. dados sensíveis — o mapa para o teu schema](#4-dados-pessoais-vs-dados-sensíveis--o-mapa-para-o-teu-schema)
5. [Bases legais: quando precisas mesmo de consentimento](#5-bases-legais-quando-precisas-mesmo-de-consentimento)
6. [Notificar vs. pedir autorização](#6-notificar-vs-pedir-autorização--a-distinção-que-custa-caro)
7. [Situações do dia a dia (com o que fazer em cada uma)](#7-situações-do-dia-a-dia)
8. [Segurança técnica: o checklist dos artigos 30.º/31.º](#8-segurança-técnica-o-checklist-dos-artigos-30º31º)
9. [Violação de dados: runbook de resposta a incidentes](#9-violação-de-dados-runbook-de-resposta-a-incidentes)
10. [Direitos dos titulares: o que o teu produto precisa de suportar](#10-direitos-dos-titulares-o-que-o-teu-produto-precisa-de-suportar)
11. [Como notificar/pedir autorização à APD, na prática](#11-como-notificarpedir-autorização-à-apd-na-prática)
12. [Sala de troféus — multas reais para aprenderes com os erros dos outros](#12-sala-de-troféus--multas-reais)
13. [Precisas de um DPO?](#13-precisas-de-um-dpo)
14. [Checklist de pré-lançamento](#14-checklist-de-pré-lançamento)
15. [Outras leis a vigiar](#15-outras-leis-a-vigiar)
16. [Recursos e contactos](#16-recursos-e-contactos)

---

## 1. TL;DR — o essencial em 90 segundos

- A lei chama-se **Lei n.º 22/11, de 17 de junho** (LPDP — Lei de Proteção de Dados Pessoais). É de 2011, mas só ganhou dentes a sério a partir de 2019, quando o regulador entrou em funcionamento.
- O regulador é a **APD — Agência de Proteção de Dados**. Tem poder para multar, e usa-o: desde 2022 já aplicou mais de **2,5 milhões de dólares** em coimas a cerca de 15 empresas, e **2026 é o ano recorde**, com mais de 1,1 milhão de dólares aplicados até agosto.
- A tua obrigação central **não é pedir consentimento e pronto** — é **notificar (ou pedir autorização) à APD** antes de tratares dados pessoais. Isto é diferente do RGPD/LGPD e é o erro nº 1 que apanha empresas angolanas de surpresa.
- **Guardar dados de utilizadores angolanos numa cloud fora de Angola sem autorização da APD é, de longe, a forma mais cara de levar uma multa.** A maior coima de 2026 (505 mil USD) foi exatamente por isso.
- Não existe obrigação legal explícita de teres um DPO/Encarregado de Proteção de Dados — mas é fortemente recomendado ter alguém responsável.
- As multas para pessoas coletivas (empresas) são **triplicadas** em relação aos valores base da lei, e podem ir até **450 mil USD por infração** só pelo texto da lei — na prática a APD já foi além disso.

Se só vais ler uma secção antes de lançares o teu produto, lê a [secção 6](#6-notificar-vs-pedir-autorização--a-distinção-que-custa-caro) e a [secção 14](#14-checklist-de-pré-lançamento).

---

## 2. O panorama legal

Angola não tem "uma lei", tem um pequeno ecossistema. Para efeitos de dev, o que interessa:

| Diploma | O que regula | Porque importa para ti |
|---|---|---|
| **Lei n.º 22/11** (LPDP) | Regras gerais de tratamento de dados pessoais | É a lei principal deste guia |
| **Lei n.º 23/11** (LCE — Lei das Comunicações Eletrónicas e dos Serviços da Sociedade da Informação) | Comunicações eletrónicas, serviços da sociedade da informação | Marketing eletrónico, operadores de telecom, notificação de incidentes (art. 55.º) |
| **Lei n.º 7/17** (LRSI — Lei de Proteção das Redes e Sistemas Informáticos) | Segurança de redes e sistemas | Medidas de segurança e notificação de incidentes (art. 35.º) |
| **Decretos Presidenciais 256/25, 258/25 e 263/25** (dez. 2025) | Estratégia Nacional de Cibersegurança, Conselho Nacional de Cibersegurança, Centro Nacional de Cibersegurança | Novo aparato institucional de cibersegurança, ainda em fase de arranque em 2026 |
| **Lei n.º 6/26** (Lei Contra Informações Falsas na Internet, em vigor desde 4 ago. 2026) | Moderação de conteúdo, desinformação, plataformas digitais | Relevante se o teu produto tem reviews, comentários ou conteúdo gerado por utilizadores |

A **APD** (Agência de Proteção de Dados) é uma pessoa coletiva de direito público, criada pelo **Decreto Presidencial n.º 214/16**, que só entrou em funcionamento efetivo em **outubro de 2019** — por isso a maioria da jurisprudência/multas só aparece a partir de 2022. A APD tem sete membros (três nomeados pelo Presidente, três eleitos pela Assembleia Nacional, um magistrado judicial) e as suas competências incluem fiscalizar, registar tratamentos, emitir autorizações, receber queixas e **aplicar multas diretamente** (sem precisar de tribunal, embora a decisão possa ser recorrida).

> 💡 Nota mental: a APD publica regularmente "**Deliberações sobre Processos Contravencionais**" no seu site — é literalmente uma lista pública de quem foi multado e porquê. Vale a pena seguir.

---

## 3. A quem se aplica isto

Curto: **quase certamente a ti.**

A lei aplica-se ao tratamento de dados pessoais feito:
- por qualquer responsável sediado em Angola;
- no âmbito de atividades de um responsável estabelecido em Angola, mesmo que a sede seja noutro país;
- por quem **use meios situados em Angola** para recolher, registar ou fazer transitar dados — isto inclui, por exemplo, ter servidores, CDNs ou pontos de recolha em território angolano;
- fora de Angola, se o direito internacional determinar que a lei angolana se aplica mesmo assim.

Ou seja: **não precisas de ter sede em Angola para estares sujeito à LPDP.** Se o teu SaaS tem utilizadores angolanos e trata os dados deles a partir de servidores em Angola (ou mesmo só recolhidos a partir daí), a lei aplica-se. Se não tens presença em Angola mas usas "meios" lá (ex: um formulário acedido a partir de Angola, um app publicado nas lojas angolanas), a lei pede que nomeies um **representante estabelecido em Angola** perante a APD.

**Exceções relevantes:** tratamento por pessoa singular em atividades exclusivamente pessoais/domésticas, segredo de Estado/justiça, e dados de militares das Forças Armadas geridos internamente.

---

## 4. Dados pessoais vs. dados sensíveis — o mapa para o teu schema

A lei distingue dois níveis. Isto devia mapear diretamente para como classificas colunas na tua base de dados.

**Dado pessoal** = qualquer informação, de qualquer natureza ou suporte (incluindo imagem e som), que identifique ou torne identificável uma pessoa singular — direta ou indiretamente.

**Dado sensível** = dados sobre convicções filosóficas ou políticas, filiação partidária ou sindical, fé religiosa, vida privada, origem racial ou étnica, saúde e vida sexual (incluindo dados genéticos). Também são tratados como sensíveis, com regras próprias, os dados sobre **suspeita de atividades ilícitas/crimes** e dados de **crédito e solvabilidade**.

| Campo típico no teu schema | Classificação | Nota prática |
|---|---|---|
| `nome`, `email`, `telefone`, `endereço IP` | Dado pessoal comum | Precisa de base legal, mas não de autorização especial |
| `foto de perfil`, `geolocalização` | Dado pessoal comum | Imagem conta como dado pessoal por definição legal |
| `salário`, `histórico de crédito`, `score de crédito` | **Sensível — regime de crédito/solvabilidade** | Exige consentimento expresso OU autorização da APD (art. 16.º) |
| `religião`, `orientação política`, `filiação sindical` | **Sensível** | Exige autorização prévia da APD na quase totalidade dos casos |
| `dados de saúde`, `dados biométricos usados para identificar` | **Sensível** | Regras reforçadas (art. 14.º) + separação lógica obrigatória de outros dados |
| `BI/passaporte/documentos de verificação de identidade` | Dado pessoal comum, mas de alto risco | Não é "sensível" pela definição legal, mas exige medidas de segurança reforçadas por ser identificador direto |
| `histórico de compras`, `comportamento de navegação` | Dado pessoal comum | Cuidado com perfis/scoring automatizado (ver secção 7.6) |
| `avaliações/reviews sobre terceiros (empresas, pessoas)` | Dado pessoal se identificar alguém | Se a review nomear uma pessoa singular (ex: um gestor), esses dados dela também entram na lei |

> ⚠️ Um detalhe que apanha muita gente: **dados de crédito e salário são tratados como categoria especial**, não como dado pessoal comum. Se o teu produto lida com salários, remunerações ou histórico financeiro — pensa duas vezes antes de tratares isto como "só mais uma coluna".

---

## 5. Bases legais: quando precisas mesmo de consentimento

A regra geral (art. 12.º) é que só podes tratar dados pessoais se:
- (a) tiveres **consentimento inequívoco e expresso** do titular, **ou**
- (b) tiveres feito a **notificação à APD**.

Ou seja — ao contrário do que muita gente assume vindo do RGPD — **a notificação à APD é, em si, uma via legítima**, não um extra burocrático por cima do consentimento. Mas o consentimento continua a ser dispensável quando o tratamento for necessário para:

- executar um contrato em que o titular é parte (ex: processar uma encomenda);
- cumprir uma obrigação legal do responsável (ex: reter faturas para efeitos fiscais);
- proteger interesses vitais do titular (ex: emergência médica);
- executar uma missão de interesse público;
- prosseguir um interesse legítimo do responsável, desde que não prevaleçam os direitos do titular.

Para **dados sensíveis**, a fasquia sobe: precisas de consentimento **inequívoco, expresso e escrito**, ou de autorização específica da APD, salvo situações muito delimitadas (dados manifestamente tornados públicos pelo próprio, defesa de um direito em processo judicial, etc.).

---

## 6. Notificar vs. pedir autorização — a distinção que custa caro

Isto é provavelmente a parte mais mal compreendida da lei angolana por quem vem de fora. Nem tudo precisa da mesma coisa:

| Precisas de... | Situações |
|---|---|
| **Apenas notificação simples** (a APD tem 30 dias para se pronunciar; se não responder, considera-se notificado) | Tratamento geral de dados pessoais comuns; comunicação de dados a terceiros/subcontratados; marketing por email/SMS com consentimento; transferência para país com "nível de proteção adequado" |
| **Autorização prévia obrigatória** (não podes lançar antes de a teres) | Dados sensíveis (art. 13.º/14.º); dados de crédito/solvabilidade sem consentimento expresso (art. 16.º); videovigilância e outros meios de controlo eletrónico (art. 17.º); gravação de chamadas (art. 20.º); interconexão de ficheiros de dados de responsáveis diferentes (art. 24.º); **transferência internacional para país sem nível de proteção adequado** (art. 34.º); decisões individuais totalmente automatizadas com efeitos jurídicos (art. 29.º, em certos casos) |

O erro clássico (e o mais caro, como vais ver na secção 12): tratar a "autorização prévia" como notificação, lançar em produção, e só duas ou três multas depois perceber a diferença.

> 💡 A APD pode isentar certas categorias de tratamento da obrigação de notificação, por deliberação própria, quando o risco para os titulares for baixo (critérios de celeridade, economia e eficiência). Não presumas isenção — confirma junto da APD.

---

## 7. Situações do dia a dia

### 7.1 Registo e autenticação de utilizadores

- Nome, email, password (hash), telefone → dado pessoal comum, base legal = execução de contrato/consentimento.
- **Boa prática de arquitetura (e não só de conformidade):** nunca uses email, nome ou telefone como identificador público ou chave estrangeira exposta noutras tabelas (ex: numa review, numa avaliação pública). Usa um ID interno opaco e resolve a identidade só onde for estritamente necessário. Isto reduz superfície de exposição em caso de fuga de dados e facilita cumprir pedidos de eliminação/retificação sem teres de repensar todo o schema.
- Se recolhes dados de menores, a lei exige informação "clara, precisa e objetiva" dirigida a eles — e provavelmente vais precisar do consentimento de um responsável legal, dependendo do tratamento.

### 7.2 Emails, notificações e marketing

- Enviar email de marketing sem consentimento **é, segundo a própria APD, uma violação clara da lei** (é uma das perguntas frequentes explicitamente respondidas no site deles).
- Publicidade eletrónica (art. 19.º) precisa de consentimento inequívoco **e** notificação à APD, com exceções para: comunicações a representantes/colaboradores de pessoas coletivas, comunicações do Estado via governação eletrónica, e clientes existentes que já tiveram oportunidade de recusar.
- Tens de dar sempre uma forma gratuita e fácil de opt-out, e manter uma lista atualizada de quem se opôs.

### 7.3 Analytics, cookies e tracking

A LPDP não tem um artigo "cookies" dedicado (ao contrário do RGPD/ePrivacy), mas os princípios gerais aplicam-se na mesma: se recolhes um identificador que, direta ou indiretamente, permite reconhecer um dispositivo/pessoa, isso é dado pessoal. Na prática:
- Trata analytics/tracking como tratamento de dados pessoais comum — precisa de base legal e, tipicamente, notificação.
- Documenta isto na tua política de privacidade (a lei exige, no mínimo, identidade do responsável, finalidades, destinatários, e como exercer direitos de acesso/retificação).
- Se recolheres dados via rede aberta (o teu site/app), a lei aceita que o "dever de informação" seja cumprido através de uma política de privacidade acessível — mas ela tem de avisar explicitamente que os dados podem circular sem garantias totais de segurança numa rede aberta.

### 7.4 Upload de documentos de identidade / KYC / verificação

Documentos como BI, passaporte ou comprovativos são dados pessoais de alto risco (mesmo não sendo tecnicamente "sensíveis" pela definição legal). Pontos de atenção:
- Aplica medidas de segurança reforçadas (encriptação em repouso, controlo de acesso granular, logs de quem acedeu a quê).
- Define um período de retenção claro — a lei exige que dados só sejam conservados enquanto forem necessários à finalidade, com eliminação ou anonimização depois.
- Se subcontratas a verificação a um serviço de terceiros (ex: um provedor de KYC), precisas de contrato escrito com esse subcontratado, nos termos do art. 23.º (ver 7.9).

### 7.5 Dados salariais, financeiros e de crédito

Como já viste na secção 4, isto tem regime próprio (art. 16.º):
- Precisa de consentimento expresso do titular **ou** autorização da APD.
- Se estiveres a manter um "ficheiro de devedores" ou histórico de incumprimento, tens de notificar o titular no prazo de 60 dias após a inserção dos dados nesse ficheiro, além de precisares de autorização da APD.
- Isto aplica-se tanto a fintechs e produtos de crédito como a qualquer plataforma que trate salários, remunerações ou avaliações de solvabilidade — por exemplo, uma plataforma que publique faixas salariais por empresa/cargo deve tratar essa componente com o mesmo cuidado que um histórico de crédito, mesmo que os dados sejam agregados ou parcialmente anonimizados a jusante.

### 7.6 Scoring automático, "trust score" e decisões algorítmicas

O art. 29.º dá a qualquer pessoa o direito de **não ficar sujeita a uma decisão que a afete significativamente, tomada exclusivamente por meios automatizados** e destinada a avaliar aspetos da sua personalidade — capacidade profissional, crédito, confiança ou comportamento. Isto é relevante para qualquer:
- sistema de scoring de crédito;
- "trust score" ou pontuação de reputação/confiança calculada automaticamente;
- sistema de recomendação/matching com impacto relevante (ex: decidir quem vê ou não determinada oportunidade).

Na prática, para não caíres na proibição, o teu produto normalmente precisa de pelo menos uma destas salvaguardas:
- o titular pediu ativamente o serviço que depende dessa decisão (ex: pediu para ser avaliado);
- existem medidas que garantam o direito de representação e expressão do titular (ex: possibilidade de contestar, pedir revisão humana, ou perceber os critérios gerais usados);
- a APD autorizou especificamente esse tipo de decisão automatizada.

> 💡 Se o teu produto calcula qualquer tipo de pontuação de confiança/reputação a partir de sinais agregados, vale a pena desenhar desde o início: (1) um mecanismo de recurso/contestação, (2) versionamento auditável da metodologia (para poderes explicar "porque é que este número mudou"), e (3) nunca basear decisões de alto impacto **só** no algoritmo sem via de revisão humana.

### 7.7 Conteúdo gerado por utilizadores (reviews, comentários) — e a Lei das Fake News

Se o teu produto tem avaliações, comentários ou qualquer conteúdo gerado por utilizadores sobre terceiros (empresas, produtos, pessoas):
- Uma review que nomeie ou identifique uma pessoa singular (ex: um gestor específico) torna essa pessoa também titular de dados pessoais nesse conteúdo — pensa em como ela pode exercer direito de acesso/oposição sobre o que é dito sobre ela.
- Desde agosto de 2026 está em vigor a **Lei n.º 6/26 — Lei Contra Informações Falsas na Internet**, que impõe deveres de transparência e de reporte a plataformas digitais (incluindo publicar dados sobre remoções de conteúdo, identificar publicidade paga/patrocinada, e disponibilizar mecanismos de denúncia e recurso). Esta lei é distinta da LPDP e gerou debate político em Angola — o governo apresenta-a como resposta à desinformação, enquanto oposição, jornalistas e organizações da sociedade civil manifestaram preocupação com o impacto na liberdade de expressão. Independentemente da tua posição sobre o mérito da lei, se o teu produto modera ou publica conteúdo gerado por utilizadores dirigido ao público angolano, vale a pena teres consciência de que este é um segundo diploma (além da LPDP) a acompanhar.

### 7.8 Cloud, hosting e transferências internacionais — o erro mais caro

Isto merece destaque próprio porque é, segundo os dados públicos da APD, **a categoria de infração mais cara** vista até agora (multa de 505 mil USD à Crescer Tech em 2026, por guardar dados de clientes numa cloud fora de Angola sem notificação nem autorização).

Regras (arts. 33.º e 34.º):
- Transferir dados para um país que a APD considere ter **nível de proteção adequado** → precisa apenas de **notificação**.
- Transferir para um país **sem nível de proteção adequado** → precisa de **autorização prévia**, só concedida em casos específicos (consentimento expresso e escrito do titular, execução de contrato, proteção de interesses vitais, cláusulas contratuais que garantam proteção adequada, regras internas de grupo empresarial, entre outros).
- A APD é quem decide, caso a caso, se um país tem "nível de proteção adequado" — não presumas que um provedor cloud popular (AWS, GCP, Azure, etc.) está automaticamente coberto só porque é uma empresa internacional conhecida.
- **Importante para arquitetura:** usar uma região de cloud fora de Angola conta como transferência internacional de dados — mesmo que seja "só" para backups, logs, ou processamento intermédio. Isto já foi a base direta de multas reais.

O que isto significa na prática para uma stack típica:
- Se usas AWS/GCP/Azure/DigitalOcean fora de Angola para guardar dados pessoais de utilizadores angolanos, isto é uma transferência internacional sujeita, no mínimo, a notificação — e possivelmente a autorização prévia.
- Documenta desde o início: que dados saem de Angola, para onde, e com que fundamento (consentimento? cláusula contratual? grupo empresarial?). Isto é literalmente um dos campos obrigatórios do formulário de notificação da APD.
- Contratos com fornecedores cloud deveriam idealmente incluir cláusulas de proteção de dados equivalentes às exigidas pela LPDP — a lei permite que seja essa garantia contratual a servir de fundamento para a transferência.

### 7.9 Fornecedores e APIs de terceiros (subcontratados)

Sempre que passas dados pessoais a um fornecedor externo que os trata **em teu nome** (processador de pagamentos, serviço de email transacional, provedor de KYC, serviço de analytics) — a lei chama a isso um "subcontratado" (art. 23.º) e exige:
- **contrato escrito** que obrigue o subcontratado a cumprir a LPDP e a agir só segundo as tuas instruções;
- o subcontratado não pode comunicar os dados a mais ninguém, tem de aplicar as medidas de segurança da lei, e tem de destruir ou devolver os dados no fim da relação contratual;
- se o subcontratado sair destas regras, passa ele próprio a ser considerado responsável pelo tratamento (com toda a responsabilidade que isso implica) — o que é do interesse dele evitar, mas o contrato tem de deixar isto explícito.

### 7.10 Videovigilância e gravação de chamadas

Se o teu produto/empresa usa câmaras (ex: escritório, loja física integrada com o produto) ou grava chamadas (ex: para "efeitos de qualidade" num call center):
- Ambos exigem, regra geral, **autorização prévia da APD**, além de informação visível sobre a existência do sistema, quem é o responsável e como contactá-lo.
- Gravação de chamadas para prova de transação comercial precisa de consentimento prévio explícito **e** autorização da APD — exceto chamadas de emergência, que só precisam de notificação.

---

## 8. Segurança técnica: o checklist dos artigos 30.º/31.º

A lei não dita tecnologias específicas, mas exige medidas "técnicas e organizativas" adequadas ao risco. Na prática, e olhando para os motivos mais citados nas multas reais da APD, isto costuma incluir:

- [ ] Controlo de acesso: só pessoas autorizadas acedem aos dados, com autenticação forte e permissões por função (não toda a gente tem acesso a tudo).
- [ ] Encriptação de dados sensíveis em trânsito e em repouso — especialmente para os que a lei já trata como sensíveis (saúde, crédito, biométricos).
- [ ] Separação lógica entre dados de saúde/vida sexual e o resto dos dados pessoais (exigência explícita do art. 31.º).
- [ ] Logs de auditoria: capacidade de verificar a posteriori que dados foram introduzidos, quando e por quem.
- [ ] Documento interno de políticas e procedimentos de segurança (a lei exige explicitamente que exista este documento, não só que as medidas existam na prática).
- [ ] Plano de resposta a incidentes (ver secção 9) — não é opcional dado que a notificação de incidentes a jusante é obrigatória.
- [ ] Cifra de dados em rede sempre que a APD determine que a circulação em rede de dados sensíveis/de risco elevado o exige.
- [ ] Medidas contra destruição, perda ou alteração acidental — ou seja, backups testados, não só "termos backups".

> Repara que quase todas as multas de 2025-2026 (Standard Bank, TAAG, BDA, Hotel Diamante, Fast Digital Center) citam explicitamente falta de "medidas técnicas e organizativas adequadas" como fundamento — não é um detalhe secundário da lei, é o artigo mais invocado nas sanções reais.

---

## 9. Violação de dados: runbook de resposta a incidentes

Este é um dos pontos onde a lei "de papel" (2011) e a orientação prática mais recente da APD divergem — e onde muita gente ainda não sabe que a obrigação existe.

Em **outubro de 2024**, a APD emitiu a **Circular n.º 2/2024**, clarificando a obrigação de notificar acidentes e incidentes informáticos, cruzando três diplomas: a LPDP (arts. 30.º/31.º), a LCE (art. 55.º — que também obriga a notificar o INACOM em paralelo, se fores operador de telecomunicações) e a LRSI (art. 35.º).

**O que conta como incidente a notificar:** destruição, perda, alteração, indisponibilidade, divulgação, acesso não autorizado, ou qualquer outro incidente de segurança que afete dados pessoais.

**Quando notificar:** "assim que tomar conhecimento" — a circular não fixa um prazo em horas (ao contrário das 72 horas do RGPD), mas o critério é imediatez, não conveniência.

**O que a notificação tem de incluir, no mínimo:**
1. Descrição da natureza da violação, incluindo tipo de dados afetados e número aproximado de titulares afetados (se possível).
2. Descrição das consequências prováveis.
3. Descrição das medidas já tomadas ou propostas para mitigar/reparar a violação.

Se não conseguires reunir toda a informação de imediato, a orientação permite notificar mesmo assim, explicando o que falta e porquê, e complementar depois.

### Runbook mínimo sugerido

```
1. Deteção        → confirma o incidente, contém o impacto (isola sistemas/credenciais comprometidas)
2. Avaliação       → que dados, quantos titulares, que risco real para eles
3. Notificação APD → email para geral@apd.ao (ou canal formal aplicável), "assim que tomar conhecimento"
                     — não esperes ter a investigação 100% fechada
4. Se fores operador de telecom/LCE → notifica também o INACOM em paralelo
5. Mitigação       → aplica correções, reforça controlos que falharam
6. Registo interno → guarda tudo: quando soubeste, quando notificaste, o que fizeste
                     (isto é o que separa uma "atenuação extraordinária" de uma multa cheia —
                     ver a secção 12, caso Hotel Diamante)
```

---

## 10. Direitos dos titulares: o que o teu produto precisa de suportar

A lei dá aos titulares de dados os seguintes direitos, que idealmente devem existir como funcionalidades reais no teu produto — não só como uma cláusula na política de privacidade:

| Direito | O que implica na prática |
|---|---|
| **Informação** | O utilizador sabe, no momento da recolha (ou até 30 dias depois, se os dados não vierem diretamente dele), quem é o responsável, para quê os dados servem, e a quem podem ser comunicados |
| **Acesso** | O utilizador pode pedir, sem custos nem demoras excessivas, que finalidades e categorias de dados existem sobre ele, e obter cópia |
| **Retificação, atualização e eliminação** | Prazo legal de **60 dias úteis** para o responsável cumprir. Se já tiveres comunicado esses dados a terceiros, tens de os notificar da retificação/eliminação também |
| **Oposição** | O utilizador pode opor-se ao tratamento em certas circunstâncias (ex: marketing direto) |
| **Não sujeição a decisões automatizadas** | Ver secção 7.6 |

Notas de implementação:
- Um pedido de eliminação nem sempre significa apagar tudo de imediato — há situações em que a lei obriga a **bloquear e conservar** (ex: obrigação legal de retenção, disputa de crédito em curso, dados sob investigação). Desenha o teu sistema de eliminação com um estado de "soft delete + bloqueio" em mente, não só um `DELETE` puro.
- Se usas arquitetura de eventos/logs imutáveis (ex: um `audit_events` append-only), pensa desde já em como cumprir "retificação/eliminação" sem quebrar a integridade do log — normalmente via desidentificação/tombstoning do registo, não apagando o histórico todo.

---

## 11. Como notificar/pedir autorização à APD, na prática

1. **Identifica que tipo de notificação/pedido precisas.** A APD tem formulários específicos no site (apd.ao) para, entre outros: tratamento geral de dados pessoais, dados biométricos, videovigilância, gestão de meios de comunicação (telefone/email/internet), legalização de ficheiros já existentes, e reclamações de cidadãos.
2. **Preenche o formulário.** Os campos tipicamente pedidos incluem: identidade e morada do responsável (e representante, se aplicável); finalidades do tratamento; categorias de titulares e de dados; destinatários; se há subcontratados envolvidos (com o contrato de proteção de dados anexo); se há transferência para fora de Angola (país, entidade, tipo de dados, fundamento legal); tempo de conservação; e medidas de segurança aplicadas.
3. **Submete por um dos três canais:** email para `geral@apd.ao`, entrega presencial na sede da APD, ou através do balcão online em apd.ao (registo prévio necessário).
4. **Aguarda.** Para notificação simples, a APD tem 30 dias para se pronunciar — decorrido esse prazo sem resposta, considera-se notificado. Para autorização (dados sensíveis, transferências para países sem nível adequado, etc.), não há prazo fixo equivalente na lei — na prática, deves tratar isto como um item de bloqueio de lançamento, não uma formalidade em paralelo.
5. **Guarda o comprovativo.** O registo de tratamentos sujeitos a notificação/autorização é público e consultável na APD — isto é bom para ti: é também como consegues verificar mais tarde que o teu processo ficou mesmo registado.

**Contacto oficial:**
Rua do MAT, Complexo Administrativo Clássicos de Talatona, 3.º Edifício, 7.º andar, Luanda
Telefone: +244 937 930 788 · Email: geral@apd.ao · Site: apd.ao

> A APD tem uma **tabela de taxas** própria para notificações/autorizações (o valor varia consoante o tipo de tratamento) — consulta diretamente no site ou por contacto direto antes de orçamentares o processo, porque os valores publicados variam por categoria e não são um número único.

---

## 12. Sala de troféus — multas reais

Nada ensina conformidade mais depressa do que ver quem já pagou a fatura. Todos os valores são os equivalentes em USD divulgados pela própria APD/imprensa angolana; as multas são pagas em Kwanzas.

| Ano | Entidade | Multa (equiv. USD) | Motivo principal |
|---|---|---:|---|
| 2022 | BPC — Banco de Poupança e Crédito | $525.000 | Não pediu autorização à APD para tratar dados pessoais de trabalhadores (art. 35.º, n.º 1) |
| 2024 | Maxam — Companhia de Pólvoras e Explosivos | $150.000 | Transferência internacional ilegal de dados de um cidadão angolano para o Reino Unido, sem autorização, além de falta de notificação sobre dados de trabalhadores |
| 2024 | Unitel, ENDE, Cosal, Banco Comercial do Huambo | $75.000–$225.000 cada | Conjunto de 4 sanções adicionais em 2024 (total do ano ≈ $600 mil) |
| 2025 | TAAG — Linhas Aéreas de Angola (2 multas) | $175.000 (total) | Falta de medidas técnicas/organizativas para proteger dados de passageiros e familiares + incumprimento de notificação |
| 2025 | BDA — Banco de Desenvolvimento de Angola | $75.000 | Falta de medidas de segurança adequadas — sofreu um ataque de ransomware em fevereiro de 2023 |
| 2026 | **Crescer Tech** — Comércio e Prestação de Serviços | **$505.000** | **Maior multa do ano.** Transferência internacional de dados sem notificação nem autorização — dados armazenados numa infraestrutura de cloud fora de Angola |
| 2026 | Standard Bank Angola | $150.000 | Acesso não autorizado a dados de 51 clientes (nome, email, morada, telefone, saldo bancário) |
| 2026 | EPAL — Empresa Pública de Águas | $150.000 | Falhas na proteção de dados de clientes + falta de notificação/autorização |
| 2026 | Fast Digital Center | $112.000 | Medidas técnicas/organizativas inadequadas para proteger dados de um cliente + falta de notificação/autorização |
| 2026 | Hotel Diamante (Lizíria, Lda.) | $115.000 | Incumprimento dos arts. 30.º/31.º (segurança) + falta de notificação/autorização — coima atenuada por cooperação ativa |
| 2026 | TAKE NOW | $75.000 | Tratamento de dados de clientes sem notificação nem autorização |
| 2026 | Unitel (caso adicional) | não divulgado | Falha de segurança permitiu transferência fraudulenta da titularidade de um número e dos dados pessoais de uma cliente |

**Totais que valem a pena reter:**
- Desde 2022: **mais de 2,5 milhões de USD** aplicados a cerca de 15 empresas.
- Só em 2026 (até meados de agosto): **mais de 1,1 milhão de USD** — o ano mais pesado desde sempre, mais do dobro do recorde anterior (o próprio caso BPC de 2022).
- O padrão mais comum, de longe: **falta de notificação/autorização prévia** combinada com **medidas técnicas/organizativas insuficientes**. A transferência internacional sem autorização é o motivo isolado mais caro.
- Cooperar ativamente com a investigação (caso Hotel Diamante) resultou em atenuação explícita da multa — vale a pena ter isto em mente ao desenhar o teu runbook de incidentes.

---

## 13. Precisas de um DPO?

Curto: **a lei angolana não exige, por si só, a nomeação de um Encarregado de Proteção de Dados** (ao contrário do RGPD europeu ou da LGPD brasileira, que têm essa figura formalizada). A LPDP de 2011 não tem nenhum artigo equivalente ao "Data Protection Officer".

Isto não significa que não precises de ninguém responsável. Na prática:
- Alguém tem de ser o ponto de contacto para a APD, para pedidos de titulares, e para coordenar a resposta a incidentes.
- Para startups pequenas, isto costuma ser uma responsabilidade acumulada (fundador técnico, head of engineering, ou legal/compliance externo), não um cargo dedicado.
- Se a tua empresa cresce, lida com dados sensíveis em volume, ou opera em setores regulados (banca, seguros, telecom), formalizar a função ajuda — mas não é um requisito estatutário angolano como é noutras jurisdições.

---

## 14. Checklist de pré-lançamento

Antes de dares deploy em produção com dados pessoais reais de utilizadores angolanos:

- [ ] Sabes exatamente que dados pessoais e sensíveis o teu produto recolhe (mapeamento/inventário de dados).
- [ ] Já classificaste os teus dados de crédito/salariais como categoria especial, não como campo comum (secção 4 e 7.5).
- [ ] Tens uma política de privacidade publicada, com identidade do responsável, finalidades, destinatários e como exercer direitos.
- [ ] Sabes se o teu tratamento precisa só de **notificação** ou de **autorização prévia** — e, se for autorização, já a pediste (não é algo para tratar "depois do lançamento").
- [ ] Se usas cloud/hosting fora de Angola, já identificaste isso como transferência internacional e trataste da notificação/autorização correspondente (secção 7.8) — este é o item que mais caro sai a ignorar.
- [ ] Tens contratos escritos com subcontratados (processadores de pagamento, serviços de email, KYC, analytics) com cláusulas de proteção de dados (secção 7.9).
- [ ] Tens medidas técnicas mínimas implementadas: controlo de acesso, encriptação, logs de auditoria, backups testados (secção 8).
- [ ] Tens um runbook de resposta a incidentes, com o contacto da APD já identificado (secção 9).
- [ ] O teu produto permite, na prática, exercer os direitos de acesso, retificação e eliminação — não só "escreve para este email" sem processo por trás.
- [ ] Se calculas algum tipo de scoring/trust score/decisão automatizada relevante, tens mecanismo de contestação/revisão (secção 7.6).
- [ ] Se o teu produto tem conteúdo gerado por utilizadores sobre terceiros, sabes como responder a pedidos de acesso/oposição sobre esse conteúdo (secção 7.7).
- [ ] Sabes quem, na tua equipa, é o ponto de contacto responsável por isto tudo (mesmo sem ser um "DPO" formal).

---

## 15. Outras leis a vigiar

Fora do âmbito central da LPDP, mas relevantes para quem constrói produto digital em Angola em 2026:

- **LCE (Lei n.º 23/11)** — regula comunicações eletrónicas e serviços da sociedade da informação; relevante para marketing eletrónico e para operadores de telecom (obrigação paralela de notificar o INACOM em caso de incidente).
- **LRSI (Lei n.º 7/17)** — proteção de redes e sistemas informáticos; existe uma estratégia nacional que já sinaliza necessidade de revisão desta lei para a adequar ao contexto digital atual.
- **Novo aparato de cibersegurança (dez. 2025)** — a Estratégia Nacional de Cibersegurança 2024–2028, o Conselho Nacional de Cibersegurança e o Centro Nacional de Cibersegurança foram criados por decreto presidencial em dezembro de 2025 e ainda estão em fase de implementação ao longo de 2026. Vale a pena acompanhar, especialmente se operas infraestrutura crítica (saúde, energia, telecom, finanças).
- **Lei n.º 6/26 (Lei Contra Informações Falsas na Internet)** — em vigor desde agosto de 2026, impõe deveres de transparência e moderação a plataformas digitais. É um diploma distinto da LPDP e politicamente debatido em Angola (ver secção 7.7).

---

## 16. Recursos e contactos

- **APD — Agência de Proteção de Dados:** [apd.ao](https://apd.ao/ao/) · geral@apd.ao · +244 937 930 788
- **Texto da Lei n.º 22/11 (LPDP):** disponível em apd.ao e em repositórios de legislação como o AngoLEX
- **Deliberações e processos contravencionais da APD:** publicados na secção de notícias/deliberações do site da APD — a melhor forma de acompanhar precedentes reais
- **Programa SensiDados:** iniciativa de sensibilização da própria APD, com ações em escolas e institutos técnicos

---

## Aviso legal

Este guia foi escrito para orientar decisões técnicas e de produto, com base em informação pública disponível até setembro de 2026 (texto da lei, orientações e circulares publicadas pela APD, e cobertura jornalística de deliberações públicas). **Não substitui aconselhamento jurídico.** A lei angolana de proteção de dados está a ser ativamente fiscalizada e interpretada pela APD, e a prática regulatória pode evoluir mais depressa do que este documento. Antes de submeteres uma notificação/pedido de autorização formal à APD, ou antes de tomares decisões de arquitetura com implicações legais relevantes (ex: onde alojar dados, que categorias tratar como sensíveis), confirma com um advogado angolano especializado em proteção de dados ou diretamente com a APD.

Encontraste um erro ou uma atualização legal que falta aqui? Abre uma *issue* ou um *pull request* — este guia é para a comunidade dev angolana manter atualizado.
