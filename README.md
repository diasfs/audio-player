# Componente de Reprodução de Áudio Vue

Este componente Vue é uma solução simples e fácil de usar para a reprodução de arquivos de áudio em seu projeto. Ele utiliza a biblioteca WaveSurfer.js para exibir a forma de onda do áudio e controlar a reprodução.

## Características

- Exibição da forma de onda do áudio durante a reprodução
- Botões de reprodução e pausa para controlar a reprodução
- Exibição do tempo atual da reprodução e da duração total do áudio

## Como usar

Para usar este componente, basta importá-lo em seu projeto Vue e passar o arquivo de áudio como propriedade. O componente se encarregará de carregar e exibir a forma de onda do áudio e controlar a reprodução.

Exemplo de uso:

```vue
<template>
    <div>
        <audio-player :file="audioFile"/>
    </div>
</template>

<script>
import AudioPlayer from './AudioPlayer.vue';

export default {
    components: {
        AudioPlayer
    },
    data() {
        return {
            audioFile: require('path/to/audio.mp3')
        };
    }
};
</script>
```
