# Articulações: material de ensino

Páginas interativas para ensino de anatomia, semiologia e técnica cirúrgica das articulações, além de uma ferramenta de consulta rápida para a atenção primária.

## Conteúdo

- `index.html`: página inicial com a lista de materiais
- `joelho-atj/`: artroplastia total do joelho (eixos, cortes, gaps, partes moles, quiz)
- `semiologia/`: semiologia ortopédica na APS — consulta rápida com bandeiras vermelhas, exame dirigido, diagnósticos, conduta inicial, critérios de imagem e encaminhamento

## Status do material de semiologia

A ferramenta está em **v0.1 — em revisão clínica**. Ombro e joelho já estão disponíveis; as outras regiões entram em lotes. Nenhuma ficha deve ser publicada como revisada antes do OK clínico. O checklist está em `semiologia/REVISAO.md`.

## Publicar no GitHub Pages

1. Settings → Pages
2. Em "Build and deployment", escolha "Deploy from a branch"
3. Branch `main`, pasta `/ (root)`, salvar

O site fica em `https://<seu-usuario>.github.io/ensino-articulacoes/`.

## Adicionar uma articulação

Crie uma pasta (ex.: `ombro-semiologia/`) com um `index.html` e acrescente o link na lista do `index.html` da raiz.
