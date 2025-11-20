# Projeto: Malwares Simulados (Ransomware & Keylogger)

> **Atenção (IMPORTANTE)**: Este repositório contém **exercícios educacionais** que simulam o comportamento de malwares (ransomware e keylogger) *apenas* em ambiente controlado. **Não** execute os scripts em máquinas de produção, redes corporativas ou computadores com dados reais. Use **máquinas virtuais isoladas, sem acesso à internet** e com snapshots (restore points) para garantir que você possa reverter qualquer alteração.

---

## Sumário

1. Descrição do projeto
2. Objetivos de aprendizagem
3. Requisitos e ambiente seguro
4. Estrutura do repositório
5. Instruções de uso (executar os testes) — **apenas em ambiente controlado**
6. Arquitetura e funcionamento (resumo técnico)
7. Reflexão sobre defesa e mitigação (recomendações práticas)
8. Resultados esperados e validação
9. Capturas de tela (opcional)
10. Contribuições e como entregar o projeto
11. Licença

---

## 1. Descrição do projeto

Este projeto tem a finalidade de demonstrar, de forma controlada e educativa, o funcionamento básico de dois tipos de ameaças:

- **Ransomware Simulado**: scripts que geram arquivos de teste, aplicam criptografia (simulada) em um conjunto restrito de arquivos e geram uma mensagem de resgate. Também inclui um script de descriptografia para demonstrar o fluxo reverso.
- **Keylogger Simulado**: código que registra eventos de teclado em um arquivo `.txt` local, com uma variação opcional que simula o envio automático por e-mail (o envio real deve ser substituído por simulação ou validado em ambiente seguro).

Você pode optar por implementar ambos os simuladores ou documentar detalhadamente os estudos e experimentos realizados.

---

## 2. Objetivos de aprendizagem

Ao finalizar este projeto você estará apto a:

- Entender o fluxo básico de um ransomware: seleção de arquivos, criptografia, geração de chave, mensagem de resgate e descriptografia.
- Entender como um keylogger captura teclas e métodos comuns de persistência/obfuscação leves (apenas para fins didáticos).
- Executar experimentos em ambiente controlado e documentar resultados.
- Identificar indicadores de comprometimento (IoCs) e medidas imediatas de resposta.
- Recomendar controles preventivos e de mitigação.

---

## 3. Requisitos e ambiente seguro

**Requisitos mínimos**:

- Máquina virtual (VM) dedicada, isolada da rede de produção;
- Sistema operacional: Windows (ex.: Windows 10/Server) ou Linux (ex.: Ubuntu) — conforme o que for estudado;
- Python 3.8+ instalado na VM;
- Snapshots/restore points antes de qualquer execução;
- Ambiente sem acesso a arquivos sensíveis ou serviços de rede (desconectar da rede/usar NAT isolado);

**Boas práticas de segurança antes de executar**:

1. Crie um snapshot da VM e teste a restauração.
2. Use um diretório de testes (ex.: `C:\sandbox_test` ou `/home/user/sandbox_test`) contendo apenas arquivos fictícios.
3. Não execute scripts como administrador/root quando não for necessário.
4. Se simular envio de e-mail, substitua pelas rotinas de *stubbing* ou use um servidor SMTP de testes dentro da rede isolada.

---

## 4. Estrutura do repositório (exemplo)

```
/ (raiz do repositório)
├─ README.md
├─ /ransomware
│  ├─ sandbox_files/            # arquivos de teste (ex.: .txt, .jpg gerados automaticamente)
│  ├─ encrypt_sim.py            # script de criptografia (simulado) - **não executar em produção**
│  ├─ decrypt_sim.py            # script de descriptografia (simulado)
│  └─ ransom_note.txt           # modelo de nota de resgate gerado
├─ /keylogger
│  ├─ keylog_output.txt        # arquivo de saída (exemplo)
│  ├─ keylogger_sim.py         # script que captura teclas (para estudo)
│  └─ send_simulation.py       # rotina que simula envio (p.ex. grava um log ao invés de enviar)
├─ /images                    # capturas de tela (opcional)
└─ .gitignore
```

> Observação: os nomes dos arquivos sugeridos são ilustrativos. Caso você opte por **apenas documentar** (sem entregar scripts), inclua na raiz arquivos `.md` com explicações detalhadas e prints dos testes.

---

## 5. Instruções de uso (executar os testes) — **LEIA O AVISO DE SEGURANÇA**

> **Não execute** estes scripts fora de uma VM isolada.

### Ransomware (simulação)

1. Coloque arquivos de teste dentro de `ransomware/sandbox_files/`.
2. Crie snapshot da VM.
3. Execute `encrypt_sim.py` **somente** na VM de testes: o script deve criptografar apenas os arquivos do diretório sandbox e gerar `ransom_note.txt` informando instruções fictícias.
4. Para reverter, execute `decrypt_sim.py` usando a chave salva (o repositório deve descrever como a chave é gerada/armazenada — p.ex. chave em formato base64 no arquivo `key.bin`).

> Observação prática: em vez de usar criptografia real em arquivos do sistema, prefira criptografar apenas arquivos de teste no diretório sandbox. Documente quais bibliotecas foram usadas (ex.: `cryptography`, `Fernet`) e explique o gerenciamento de chaves.

### Keylogger (simulação)

1. Configure o diretório de saída em `keylogger/keylog_output.txt`.
2. Execute `keylogger_sim.py` na VM de testes; o script deve registrar teclas pressionadas e gravar em `keylog_output.txt`.
3. Para a parte de envio automático por e-mail, **não configure** credenciais reais: implemente a função `send_simulation.py` para gravar o que seria enviado em um arquivo de log (ex.: `send_log.txt`) ou utilize um servidor SMTP de teste.

> Observação: muitos ambientes detectam keyloggers como maliciosos. Execute apenas em ambiente controlado e prefira técnicas de *mock* / *stubbing* para simular envio.

---

## 6. Arquitetura e funcionamento (resumo técnico)

### Ransomware (simulado)

- **Geração de arquivos de teste**: scripts podem criar arquivos de texto/imagem aleatórios para validação.
- **Criptografia**: uso de algoritmo simétrico (ex.: AES via biblioteca `cryptography.Fernet`) para criptografar conteúdo de arquivos.
- **Gerenciamento de chave**: chave gerada localmente e armazenada em `key.bin` para permitir descriptografia durante os testes.
- **Nota de resgate**: arquivo `ransom_note.txt` gerado contendo texto explicativo (simulado) e instruções fictícias.

### Keylogger (simulado)

- **Captura de eventos de teclado**: bibliotecas como `pynput` (em Windows/Linux) permitem captura de teclas; registre apenas no arquivo de saída do sandbox.
- **Persistência/obfuscação (apenas para estudo)**: descrever mecanismos comuns (ex.: criação de serviço, execução em background) — **não** implementá-los em ambiente real.
- **Exfiltração (simulada)**: gravar o que seria enviado em um arquivo ou enviar para um servidor de teste isolado.

---

## 7. Reflexão sobre defesa e mitigação (recomendações práticas)

**Medidas preventivas**:

- Backups regulares, offline e testados (essenciais contra ransomware).
- Atualizações e patching de sistemas e aplicações.
- Privilégios mínimos: evitar execução de código com conta administrativa.
- Segmentação de rede e controles de acesso para limitar o alcance de ameaças.
- Políticas de e-mail e filtros para reduzir phishing (vetor comum de ransomware e keyloggers).

**Detecção e resposta**:

- Monitoramento de criação/alteração massiva de arquivos (padrões de criptografia em massa).
- EDR/AV atualizados com regras para detectar comportamentos suspeitos (ex.: processos que leem muitos arquivos e escrevem em sequência).
- Captura de logs e análise de indicadores de comprometimento (novos processos, conexões de rede divergentes, chaves registradas).
- Sandboxing de arquivos suspeitos para análise segura.

**Controles técnicos recomendados**:

- Uso de EDR com integração de resposta automática.
- Políticas de execução de macros e bloqueio de PowerShell remoting quando não necessário.
- Controle de dispositivos removíveis (USB) e criptografia de disco.

**Conscientização**:

- Treinamentos regulares sobre phishing e engenharia social.
- Procedimentos de recuperação e comunicação em caso de incidente.

---

## 8. Resultados esperados e validação

**Ransomware**:

- Arquivos de teste no diretório `sandbox_files` devem ficar com extensão alterada e conteúdo criptografado após a execução do `encrypt_sim.py`.
- `ransom_note.txt` deve ser gerado com instruções fictícias.
- `decrypt_sim.py` deve restaurar os arquivos ao estado original quando executado com a chave correta.

**Keylogger**:

- `keylog_output.txt` deve conter as teclas registradas durante a execução do script.
- O arquivo de envio simulado (`send_log.txt`) deve registrar quando a rotina de exfiltração seria acionada.

Inclua testes práticos no README (passo-a-passo) e os resultados obtidos (prints, logs) na pasta `/images`.

---

## 9. Capturas de tela (opcional)

Adicione prints demonstrando:

- Estrutura do diretório sandbox antes e depois da criptografia;
- Conteúdo do `ransom_note.txt` gerado;
- Saída do `keylog_output.txt` com eventos registrados;
- Logs de envio simulado.

Coloque as imagens em `/images` e referencie-as no README.

---

## 10. Contribuições e entrega

Para entregar o projeto (conforme solicitado no desafio):

1. Suba o repositório no GitHub (público).
2. Garanta que o `README.md` esteja completo e que os scripts estejam comentados.
3. Inclua instruções claras de execução e o aviso de segurança.
4. Clique em **Entregar Projeto** e cole o link do repositório junto com uma breve descrição do que foi feito.

Se você preferir **não incluir códigos executáveis**, entregue uma documentação detalhada com pseudocódigo, prints e explicações do que faria/fez em cada etapa.

---

## 11. Licença

Este projeto, por padrão, usa a licença **MIT** (sinta-se à vontade para ajustar). Ao incluir scripts que simulam malwares, lembre-se de colocar claramente no README a restrição de uso apenas para fins educacionais.

---

### Observações finais

Se quiser, eu posso:

- Gerar o `README.md` adaptado ao seu estilo (já está pronto neste arquivo);
- Sugerir um `.gitignore` apropriado para projetos Python;
- Gerar o `ransom_note.txt` modelo e exemplos de mensagens;
- Criar um checklist de segurança detalhado para anexar ao repositório.

Diga qual dessas opções você quer que eu gere agora e eu crio no repositório (ou atualizo o README) para você.

