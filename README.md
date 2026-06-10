# Laboratório de Performance: Otimização de Mídias para a Web
> Trabalho prático desenvolvido para a disciplina de **Introdução a Web** no **IFSP** (Instituto Federal de São Paulo).

Este repositório tem como objetivo comparar de forma prática os impactos de performance, consumo de banda e experiência do usuário (UX) ao utilizar elementos de mídia bruto (sem otimização) em contraste com mídias otimizadas utilizando práticas e formatos modernos de desenvolvimento web.

---

## Estrutura do Projeto

O projeto está dividido em duas abordagens principais:

* **[`nao_otimizado/`](./nao_otimizado)**: Contém a página HTML original e mídias pesadas de formatos tradicionais (WAV, GIF pesado, JPG não comprimido).
* **[`otimizado/`](./otimizado)**: Contém a versão otimizada com mídias em formatos modernos (WebP, WebM, MP3/OGG) e boas práticas de codificação HTML (Lazy loading, Picture tag, preload de áudio).

---

## Tabela de Comparação de Mídias

Abaixo está o levantamento comparativo de tamanho e formato dos arquivos utilizados em ambas as versões:

| Mídia | Tipo / Função | Formato Original | Tamanho Original | Formato Otimizado | Tamanho Otimizado | Redução de Tamanho (%) | Benefício Técnico |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Imagem** | Foto Principal | `.jpg` | `1.4 MB` | `.webp` | `169.9 KB` | **~88%** | WebP possui melhor compressão que o JPG mantendo a fidelidade visual. |
| **Animação** | Banner Promocional | `.gif` | `22.3 MB` | `.webm` / `.mp4` | `4.5 MB` (WebM)<br>`17.1 MB` (MP4) | **~80%** (WebM) | GIFs de alta resolução exigem alto poder de processamento da CPU/GPU. Vídeos em WebM/MP4 rodam muito mais fluidos. |
| **Áudio** | Podcast Informativo | `.wav` | `10.5 MB` | `.ogg` / `.mp3` | `595 KB` (Ogg)<br>`1.9 MB` (MP3) | **~94%** (Ogg) | WAV é um formato sem perdas e excessivamente pesado para streaming Web. OGG/MP3 oferecem excelente qualidade com compressão com perda. |

### Impacto no Carregamento Total da Página:
* **Página Não Otimizada:** **~34.2 MB**
* **Página Otimizada:** **~5.2 MB** (Considerando o carregamento do vídeo WebM e do áudio OGG)
* **Economia de Banda:** **~85% de redução** no peso total da página carregada.

---

## Boas Práticas Implementadas no HTML Otimizado

A otimização de mídias vai além do formato do arquivo. Na versão otimizada [`com_otimizacao.html`](./otimizado/com_otimizacao.html), implementamos técnicas essenciais para a melhoria de métricas do **Core Web Vitals**:

### 1. Tag `<picture>` com Imagens Responsivas
O uso de `<picture>` permite que o navegador decida o formato ideal baseado na compatibilidade de suporte, utilizando o WebP por padrão e o JPG tradicional como fallback:
```html
<picture>
    <source srcset="fundo.webp" type="image/webp">
    <img src="paisagem.jpg" alt="Foto do Produto Otimizada" loading="lazy">
</picture>
```

### 2. Atributo `loading="lazy"`
O atributo `loading="lazy"` indica ao navegador para carregar as imagens apenas quando elas estiverem próximas de entrar na viewport do usuário. Isso melhora drasticamente o tempo até a interatividade inicial da página (**LCP** e **TBT**).

### 3. Substituição de GIF por Elementos de `<video>`
GIFs animados consomem muita memória e processamento, além de não possuírem suporte a pausa ou controle de carregamento. Substituímos por um vídeo com autoplay em loop e com som silenciado:
```html
<video autoplay loop muted playsinline preload="metadata">
    <source src="animacao.webm" type="video/webm">
    <source src="video.mp4" type="video/mp4">
    Seu navegador não suporta vídeos.
</video>
```
* `preload="metadata"`: Evita o download do vídeo completo caso o navegador ainda não tenha iniciado a reprodução, salvando dados do usuário.

### 4. Otimização de Áudio com Múltiplas Fontes e `preload="none"`
O áudio está configurado com `preload="none"`. O arquivo só será requisitado no servidor quando o usuário clicar ativamente no botão de play, poupando largura de banda de quem apenas visita a página.
```html
<audio controls preload="none">
    <source src="audio.ogg" type="audio/ogg">
    <source src="audio.mp3" type="audio/mpeg">
    Seu navegador não suporta este áudio.
</audio>
```

---

## Conclusão do Estudo

A otimização de mídias é um dos pilares mais cruciais para a experiência do usuário moderno e performance web. O carregamento de mais de 34MB de mídias em conexões móveis limitadas (como 3G ou 4G instáveis) causaria uma péssima experiência de navegação e possível desistência do usuário. 

Utilizando formatos modernos (WebP, WebM e Ogg) acoplados a tags HTML semânticas e inteligentes, garantimos uma entrega **85% mais leve**, mantendo a fidelidade sonora/visual e reduzindo o consumo de processamento nos dispositivos móveis.
