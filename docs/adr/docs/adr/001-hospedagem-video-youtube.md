# ADR 001: Estratégia de Streaming de Videoaulas via YouTube (Não Listado)

* **Status:** Aceito
* **Data:** 2026-11-26

## Contexto
A plataforma precisa armazenar e distribuir videoaulas 24/7 com custo operacional zero. Serviços dedicados de streaming (como Cloudflare Stream ou AWS MediaConvert) cobram por minuto armazenado e transmitido. Armazenamento de objetos gratuito (como Cloudflare R2) limita o plano sem custos a 10 GB, o que seria esgotado rapidamente por arquivos `.mp4` em alta definição.

## Decisão
Delegar a hospedagem e o streaming adaptativo de vídeo para o YouTube utilizando a visibilidade **"Não Listado"**. O professor realiza o upload em seu próprio canal e registra apenas a URL do vídeo na plataforma. O backend extrai e valida o `video_id`, e o frontend renderiza o player embutido.

## Consequências
* **Positivas:** Custo zero de armazenamento e banda; transcodificação automática para múltiplas resoluções (360p a 1080p) e baixa latência via CDN global do Google.
* **Negativas:** Dependência da disponibilidade da API/Player do YouTube e impossibilidade de aplicar DRM proprietário contra compartilhamento externo do link direto.