# Sistema Especialista Baseado em Regras para Classificação Inteligente de E-mails

## Descrição do Domínio
Este projeto consiste em um Sistema Baseado em Regras (SBC) desenvolvido com a biblioteca `experta` em Python para automatizar a triagem, categorização e tomada de ações estruturadas em e-mails recebidos. O motor processa metadados textuais organizando os fluxos em três níveis de inferência progressiva (Forward Chaining).

## Listagem das Regras (Linguagem Natural)
1. **R1 (Identificação de Phishing):** SE o corpo do e-mail contiver termos como "ganhou", "loteria", "herança" ou "link suspeito", ENTÃO declare uma extração de propriedade maliciosa.
2. **R2 (Identificação Institucional):** SE o domínio do remetente terminar estritamente com "@ufpb.br", ENTÃO extraia o indicador de origem institucional confiável.
3. **R3 (Identificação de Urgência):** SE o assunto do e-mail contiver palavras-chave temporais como "urgente", "prazo" ou "imediato", ENTÃO marque o e-mail com a propriedade de urgência.
4. **R4 (Identificação Comercial):** SE o assunto possuir termos como "desconto", "oferta" ou "promoção", ENTÃO extraia a propriedade comercial de marketing.
5. **R5 (Categorização de Spam):** SE houver uma extração ativa de propriedade maliciosa, ENTÃO categorizar o e-mail como "Spam" sob prioridade crítica (Regra executada com máxima prioridade via `salience`).
6. **R6 (Categorização de Trabalho Urgente):** SE o e-mail for identificado como institucional AND contiver marcadores de urgência, ENTÃO categorizá-lo como "Trabalho Crítico".
7. **R7 (Categorização de Trabalho Comum):** SE o e-mail for de origem institucional AND NOT possuir marcadores de urgência, ENTÃO categorizá-lo como de rotina de trabalho.
8. **R8 (Categorização de Promoção Segura):** SE houver indicador comercial AND NOT for um e-mail de phishing, ENTÃO classifique-o na categoria de promoções legítimas.
9. **R9 (Ação de Quarentena):** SE a classificação final for "Spam", ENTÃO tome a ação de isolá-lo em ambiente de quarentena por segurança.
10. **R10 (Ação de Notificação Crítica):** SE a classificação for "Trabalho Crítico", ENTÃO despache um alerta prioritário com notificação push imediata ao usuário.
11. **R11 (Ação de Arquivamento de Rotina):** SE for classificado como rotina de trabalho, ENTÃO encaminhe a mensagem para a pasta padrão de trabalho corporativo.
12. **R12 (Ação de Triagem de Marketing):** SE for categorizado como promoções, ENTÃO mova-o para a aba lateral de ofertas para mitigar poluição visual da caixa de entrada.

## Casos de Teste Verificados
* **Caso 1 (Ataque Cibernético):** Entrada simulando e-mail fraudulento prometendo ganho financeiro. Resultado esperado: Ativação da barreira de segurança por salience e envio automático para Quarentena.
* **Caso 2 (Mensagem Acadêmica de Alta Prioridade):** Entrada originada do domínio oficial da faculdade com dados de prazo. Resultado esperado: Classificação como Trabalho Urgente e ativação do protocolo de alerta imediato.
* **Caso 3 (Publicidade de Varejo Comum):** Entrada promocional sem marcadores fraudulentos. Resultado esperado: Encaminhamento automático para a aba de Promoções sem perturbar a interface de foco do usuário.
