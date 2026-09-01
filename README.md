# GEPEFF Gestão

Sistema de gestão do GEPEFF — Grupo de Extensão e Pesquisa em Enzimologia, Fermentação
e Farmacologia (Faculdade Pio Décimo, Campus Jabotiana, Aracaju/SE).

Arquivo único, sem build. Conecta ao Supabase quando há login; sem login, funciona sozinho
no navegador.

---

## Ligar o Supabase — faça isto primeiro

**1. Rode o SQL.** No painel do projeto → SQL Editor → cole `supabase-setup.sql` inteiro e
execute. Ele cria as doze tabelas, liga o RLS com acesso só para quem tem login, cria o
bucket `gepeff` e as políticas de arquivo.

**2. Crie os logins.** Authentication → Users → *Add user* → *Create new user*, com e-mail e
senha, marcando **Auto Confirm User**. Crie um por diretoria (ou um por pessoa, se preferir
rastrear quem alterou o quê depois). Sem usuário criado, ninguém entra — é isso que protege
o financeiro e os dados dos integrantes.

**3. Abra o sistema.** A tela de login aparece sozinha. No primeiro acesso, como a base está
vazia, ele envia as nove diretorias com objetivos e necessidades para a nuvem. A partir daí
todo mundo que entrar vê a mesma base.

A URL e a chave `anon` do projeto já estão no arquivo (`const NUVEM`, no primeiro `<script>`).
A chave `anon` é pública por design e só serve para chegar até o Supabase — quem manda é o
RLS, e ele exige sessão autenticada.

---

## Os dois modos

**Nuvem** — logado. Cada alteração grava a linha correspondente na tabela certa (não o
documento inteiro), então duas diretorias trabalhando ao mesmo tempo não se sobrescrevem.
Arquivos vão para o Storage. A tarja verde na barra lateral mostra o e-mail da sessão.

**Local** — sem login, sem internet, ou com o servidor fora de alcance. Tudo continua
funcionando no IndexedDB do navegador, e a tarja fica cinza dizendo por quê. Nada se perde:
quando você entrar, use **Configurações → Enviar tudo desta máquina** para subir o que fez
offline.

Ao entrar na nuvem, os dados do servidor substituem os do navegador. Se havia registros só
locais, o sistema guarda uma cópia antes e oferece **Restaurar cópia local anterior** em
Configurações.

---

## As nove diretorias

Diretoria Executiva · Secretaria · Tesouraria · Marketing e Comunicação · Eventos ·
Científica · Extensão · Ensino · Membros e Seleção.

Cada uma tem aba própria com responsável e contato, objetivos do semestre em checklist com
prazo e barra de progresso, necessidades da área com prioridade, as tarefas daquela
diretoria, upload de arquivos próprio e um bloco de anotações.

As nove vêm pré-preenchidas com objetivos e necessidades. **São sugestões de partida** —
a ideia é cada diretoria abrir a sua aba na primeira reunião e ajustar.

## Módulos

**Tarefas** — quadro em quatro colunas com arrastar e soltar. Diretoria, responsável, prazo e
prioridade; borda colorida marca a prioridade e prazo vencido aparece em vermelho. Filtro por
diretoria, busca e "só atrasadas".

**Financeiro** — entradas e saídas com categoria, valor, data e evento vinculado. Saldo no
topo, comprovante anexável a cada lançamento e exportação em CSV.

**Agenda** — calendário mensal navegável e lista de próximos compromissos com contagem
regressiva.

**Integrantes** — cadastro com período, cargo e diretoria; sub-aba de **presença** por
encontro; sub-aba de **relatório individual** com frequência, trabalhos apresentados e
tarefas concluídas por pessoa, exportável em CSV.

**Apresentações** — quem apresentou, título, tipo (journal club, caso clínico, seminário,
relato, projeto, tema livre), referência, duração, observações e material anexado. Alimenta
o relatório individual — a prova de participação que vale em edital, estágio e residência.

**Arquivos** — biblioteca com filtro por diretoria, categoria e busca. Pré-visualização de
imagem, PDF e texto; download em qualquer formato; até 25 MB por arquivo.

**Configurações** — identificação do grupo, backup em JSON, painel de conexão.

---

## Publicar

- **GitHub Pages**: suba `index.html` na raiz do repositório e ative Pages.
- **Vercel / Netlify**: arraste a pasta.
- Adicione a URL publicada em **Authentication → URL Configuration** no Supabase.

A pré-visualização publicada como artifact da Claude **não** alcança o Supabase (o sandbox
bloqueia requisições externas), então lá o sistema roda sempre em modo local. A versão
conectada é a que você hospedar.

## Download de arquivos

Na versão hospedada por você o navegador baixa direto. Na pré-visualização da Claude o
download passa por uma confirmação e aceita `csv`, `json`, `pdf`, `png`, `txt`, `md`, `docx`,
`pptx`, `html` e `svg`; outros formatos só na versão hospedada.

## Estrutura

- `index.html` — o sistema inteiro
- `supabase-setup.sql` — schema, RLS e bucket

Dentro do HTML, na ordem: tokens de cor e componentes, depois `seed.js` (as nove diretorias,
ícones e listas de referência) e `app.js` (armazenamento, sincronização, navegação e telas).

## Aviso

Os objetivos e necessidades pré-preenchidos são sugestões redigidas a partir do funcionamento
típico de um grupo acadêmico. Revise com a Diretoria Executiva antes de tratá-los como o
plano oficial do semestre.
