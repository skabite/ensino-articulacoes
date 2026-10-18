# Instruções para agentes — Semiologia ortopédica na APS

Este arquivo vale para o DeepSeek Harness e para qualquer agente que trabalhe neste repositório. Leia antes de editar.

## O que é o projeto

Site estático, em português do Brasil, de **semiologia ortopédica para a atenção primária**. O foco atual é:

- consulta guiada por queixa (ombro e joelho);
- fichas completas por região, para estudo;
- testes e manobras com demonstrações 3D interativas;
- página consolidada de evidência (Sn/Sp, fontes e limitações);
- página sobre utilidade da imagem e achados em assintomáticos.

O público é médicos e enfermeiros da APS/AB; fisioterapeutas podem usar conforme o escopo.

## Estrutura

- `index.html` — home do repositório, com os cartões dos materiais.
- `joelho-atj/index.html` — material de ensino sobre artroplastia total do joelho. **Não alterar o conteúdo sem pedido explícito.**
- `semiologia/index.html` — o app principal. É um **arquivo único** com CSS, JS e dados JSON embutidos.
- `semiologia/REVISAO.md` — checklist de revisão clínica.
- `README.md` — visão geral e publicação.
- `AGENTS.md` — este arquivo.

## Regras clínicas e de conteúdo

1. **Revisão clínica pendente.** O material está em `v0.1 — em revisão clínica`. Não marcar como “revisado”, não remover o banner de revisão, os badges “Em revisão clínica” nem o aviso de que não substitui julgamento clínico, sem o OK explícito do Luis.
2. **Sem medicações específicas por ora.** Não incluir nomes de medicamentos, doses ou esquemas. Usar formulações genéricas como “controle da dor conforme protocolo local” e medidas não farmacológicas.
3. **Sem seção de encaminhamento por ora.** O tema será retomado no futuro com o nome **“referenciamento”**. Não reintroduzir a página, o item de menu, a seção das fichas, a linha dos diagnósticos nem o bloco “Encaminhar se” do questionário. Exceção: manter alertas de urgência nas bandeiras vermelhas (“encaminhe à urgência” em fratura, infecção, déficit neurovascular etc.).
4. **Conteúdo em pt-BR**, direto e prático, para profissionais de saúde — não para leigos. Evitar tom de aula longa; priorizar o que muda a conduta.
5. **Evidência:** nenhum valor de Sn/Sp sem fonte verificada. Registrar IC quando disponível; se não houver, escrever “IC não reportado”. Quando a literatura for heterogênea, escrever “evidência limitada” ou “variável”. Nunca inventar números.
6. **Fontes preferidas:** Ministério da Saúde/TelessaúdeRS (Ortopedia Adulto, 2022), CONITEC, SBOT, AAOS, NICE, ACR, regras de Ottawa e revisões sistemáticas no PubMed. UpToDate pode ser citado quando o material local existir; não copiar texto ou imagens.
7. **Segurança:** incluir contraindicações e limitações; não prometer diagnóstico por um teste isolado; não usar imagem como substituto do exame.

## Regras de produto e UX

- Mobile-first; a consulta rápida deve funcionar bem a 360 px.
- Manter o padrão visual existente: fontes Archivo + Atkinson Hyperlegible, tokens `--paper`, `--surface`, `--ink`, `--drape`, dark mode automático e manual, foco visível, alvos de toque e bom contraste.
- O modo **Consulta** é o padrão: questionário curto e resultado focado (não perder, exame agora, conduta inicial, testes). A ficha completa fica no modo **Ficha completa**.
- O resultado da consulta guiada não deve voltar a ter o bloco de encaminhamento.
- O “Em 30 segundos” tem três cartões: não perder, exame-chave e conduta inicial.
- Textos de teste devem explicar: como fazer, o que é positivo, interpretação, evidência, limitações e fonte.
- Não adicionar dependências de frontend, build step, framework ou `fetch` para o app. O `semiologia/index.html` deve continuar abrindo por `file://` e no GitHub Pages.

## Arquitetura do app

- Dados no bloco `<script type="application/json" id="dados">`.
- CSS e JS inline em `semiologia/index.html`.
- Rotas por hash: `#/`, `#/ombro`, `#/joelho`, `#/testes`, `#/testes/<id>`, `#/evidencia`, `#/imagem`, `#/regras`, `#/conduta`, `#/sn-sp`, `#/referencias`, `#/sobre`.
- Regiões em `DATA.regioes`. Cada região tem `status`, `revisao`, `em30s`, `bandeiras`, `anamnese`, `exame`, `testes`, `diagnosticos`, `fluxo`, `imagem`, `perolas`, `armadilhas`, `refs` e, quando aplicável, `guia`.
- Testes têm `id`, `nome`, `como`, `passos`, `positivo`, `interpretacao`, `sn`, `sp`, `ic`, `lr`, `fonte`, `limitacao`.
- Demonstrações 3D:
  - Ombro: `GENERIC_DEMOS` + `buildShoulderScene`; Jobe/Neer/Hawkins têm builders próprios.
  - Joelho: `KNEE_DEMOS` + `buildKneeScene`.
  - Para registrar uma demo, adicionar o id em `DEMOS`, na lista de inicialização em `bindPage` e no config correspondente.
  - Three.js é carregado de CDN fixa (`three@0.128.0`) apenas quando a demo abre; manter fallback em SVG/placeholder se falhar.
- A página de testes e a de evidência se atualizam sozinhas a partir de `DATA.regioes`; não duplicar cartões à mão.

## Como adicionar uma região

1. Criar o objeto da região em `DATA.regioes`, seguindo ombro/joelho.
2. Preencher os testes com fontes verificadas.
3. Criar o `guia` com perguntas condicionais e resultados.
4. Se houver demo 3D, criar o config e o builder/modo correspondente.
5. Adicionar o id em `DEMOS` e na lista de inicialização de `bindPage` quando houver 3D.
6. Testar as rotas e as demos; atualizar `README.md` e `REVISAO.md`.

## Git, publicação e segurança

- Repositório público: `skabite/ensino-articulacoes`, branch `main`, GitHub Pages em `https://skabite.github.io/ensino-articulacoes/semiologia/`.
- Não fazer commit, push ou publicação sem pedido do Luis. Quando ele pedir, usar a identidade local já configurada (`Luis Fernando <skabite@users.noreply.github.com>`).
- Nunca apagar arquivos do Luis. Se algo precisar sair, sinalizar e deixar a decisão com ele.
- Não executar `rm -rf`, `git reset --hard`, `git push --force` ou mudanças destrutivas sem autorização explícita.
- Não ler nem imprimir chaves de API (`~/.dsh/dsh.env`, `~/.bashrc`, variáveis de ambiente). A chave do DeepSeek já está configurada no harness.
- O repositório é público: não adicionar dados de pacientes, nomes, prontuários, fotos clínicas identificáveis ou informações pessoais sensíveis.

## Verificação antes de concluir

- Servir localmente:
  `python3 -m http.server 8080 --directory ~/Desktop/ensino-articulacoes`
- Abrir `http://127.0.0.1:8080/semiologia/` e testar:
  - home, `#/ombro`, `#/joelho`, `#/testes`, uma demo 3D, `#/evidencia`, `#/imagem`;
  - busca com e sem acento;
  - modo Consulta e Ficha completa;
  - 360 px sem rolagem horizontal;
  - dark mode, teclado, impressão.
- Validar o JSON embutido (parse) e procurar erros no console.
- Se houver Playwright disponível em `~/internato-husm/node_modules`, usar `NODE_PATH` para um smoke test.
- Revisar o diff antes de commitar; conteúdo clínico só é “revisado” com o OK do Luis.

## Estilo de comunicação com o Luis

- Responder em português do Brasil.
- Explicar em linguagem simples; ele não é programador.
- Em mudanças de interface, guiar uma tela por vez, com print quando ajudar.
- Dizer claramente o que foi alterado, o que foi testado e o que ainda precisa de revisão clínica.
