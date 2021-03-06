#Audio e Video

O HTML5 introduz o suporte de mídia embutido por meio dos elementos ```<audio>``` e ```<video>```,
oferecendo a possibilidade de incorporar facilmente mídia em documentos HTML.

#Incorporando mídia:

Adicionando elemento **vídeo** com HTML
```php
<video src="http://v2v.cc/~j/theora_testsuite/320x240.ogg" controls>
  Seu navegador não suporta o elemento <code>video</code>.
</video>
```
Esse exemplo reproduz uma amostra de vídeo, com controles de reprodução, a partir do site Theora.


<br />

Adicionando elemento **audio** com HTML

```php
<audio src="/test/audio.ogg">
<p>Seu nevegador não suporta o elemento audio.</p>
</audio>
```

O atributo src pode ser a URL do arquivo de áudio ou o caminho do arquivo no sistema local.

```php
<audio src="audio.ogg" controls autoplay loop>
<p>Seu navegador não suporta o elemento audio </p>
</audio>
```

<br />

Esse exemplo de código usa atributos do elemento <audio>:

* **controls:** Mostra os controles padrão para o áudio na página.
* **autoplay:** Faz com que o áudio reproduza automaticamente.
* **loop:** Faz com que o áudio repita automaticamente.

<br />

**Exemplo**
```php
<audio src="audio.mp3" preload="auto" controls></audio>
```

O atributo preload é usado em elementos audio para carregar arquivos grandes. Ele pode assumir 3 valores:

* **none:** não carrega o arquivo
* **auto:** carrega o arquivo
* **metadata:** carrega apenas os meta dados do arquivo

Vários arquivos podem ser especificados utilizando o elemento 
[\<source\>](https://developer.mozilla.org/pt-BR/docs/Web/HTML/Element/source) para disponibilizar 
vídeo ou áudio codificados em formatos diferentes para navegadores diferentes. Por exemplo:

```php
<video controls>
  <source src="foo.ogg" type="video/ogg">
  <source src="foo.mp4" type="video/mp4">
  Seu navegador não suporta o elemento <code>video</code>.
</video>
```

Isso reproduz o arquivo Ogg em navegadores que suportam o formato Ogg. Se o navegador 
não suportar Ogg, o navegador reproduz o arquivo  MPEG-4. 
Veja também a lista [media formats supported by the audio and video elements](https://developer.mozilla.org/En/Media_formats_supported_by_the_audio_and_video_elements) 
para mais detalhes.

Também é possível especificar os codecs que o arquivo de mídia requer, isso permite 
que o navegador faça escolhas mais inteligentes:


```php
<video controls>
  <source src="foo.ogg" type="video/ogg; codecs=dirac, speex">
  Seu navegador não suporta o elemento <code>video</code>.
</video>
```

Aqui é especificado que o vídeo usa os codecs Dirac e Speex. Se o navegador suportar 
Ogg, mas não suportar os codecs especificados, o vídeo não será reproduzido.

Se o atributo type não estiver especificado, o tipo de mídia é obtido no servidor e 
é verificado se o navegador consegue reproduzi-lo. Se ele não pode ser renderizado, 
o próximo source é verificado. 

Se nenhum dos elementos source pode ser utilizado, um evento error é enviado para 
o elemento video. 

Se o atributo type estiver especificado, ele é comparado aos 
tipos que o navegador consegue reproduzir, e se ele não for reconhecido, 
o servidor não é verificado, ao invés disso, o próximo source é verificado.

Veja [Media events](https://developer.mozilla.org/en/DOM/Media_events) para uma lista 
completa de eventos relacionados a reprodução de mídia. Para detalhes sobre os 
formatos de mídia suportados por vários navegadores, veja 
[Media formats supported by the audio and video elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats).


# Controlando a reprodução de mídia

Após a mídia ser incorporada é possível controla-los com código de JavaScript. 
Por exemplo, para começar (ou repetir) a reprodução, você pode fazer isto:

```php
var v = document.getElementsByTagName("video")[0];
v.play();
```
A primeira linha pega o primeiro elemento video, e a segunda chama o método ```play()```
do elemento, como definido na interface 
[nsIDOMHTMLMediaElement](https://developer.mozilla.org/pt-BR/docs/Mozilla/Tech/XPCOM/Reference/Interface/nsIDOMHTMLMediaElement), 
que é utilizada para implementar elementos de mídia.

Controlando um reprodutor de áudio para reproduzir, pausar, aumentar e diminuir o 
volume usando JavaScript é simples.

```php
<audio id="demo" src="audio.mp3"></audio>
<div>
  <button onclick="document.getElementById('demo').play()">Reproduzir o áudio</button>
  <button onclick="document.getElementById('demo').pause()">Pausar o áudio</button>
  <button onclick="document.getElementById('demo').volume+=0.1">Aumentar o volume</button>
  <button onclick="document.getElementById('demo').volume-=0.1">Diminuir o volume</button>
</div>
```

# Parando o download de mídia

Embora parar a reprodução de mídia seja fácil usando o método ```pause()``` do elemento, o 
navegador continua baixando a mídia até que o elemento de mídia seja excluído por 
meio da coleção de lixo.

Esta é um modo para parar o download imediatamente:

```php
var mediaElement = document.getElementById("myMediaElementID");
mediaElement.pause();
mediaElement.src = "";
```

Ao definir o atributo src do elemento de mídia para uma string vazia, o decodificador 
interno do elemento é destruído, o que para o download.

# Navegando pela mídia

Elementos de mídia provem suporte para mover a posição atual para pontos específicos 
do conteúdo da mídia. Iso é feito ao definir o valor da propriedade currentTime no 
elemento; veja [HTMLMediaElement](https://developer.mozilla.org/pt-BR/docs/Web/API/HTMLMediaElement) 
para mais detalhes sobre as propriedades do elemento. 

Simplesmente defina o valor para o tempo em segundos em que você quer que a 
reprodução do vídeo continue.

Você pode usar a propriedade ```seekable``` para determinar os valores em que é possível 
ir no momento. Essa propriedade retorna o objeto TimeRanges listando os 
intervalos de tempo em que você pode navegar.

```php
var mediaElement = document.getElementById('mediaElementID');
mediaElement.seekable.start();  // Retorna o tempo em que o arquivo começa (em segundos)
mediaElement.seekable.end();    // Retorna o tempo em que o arquivo termina (em segundos)
mediaElement.currentTime = 122; // Ir para 122 segundos
mediaElement.played.end();      // Retorna o numero de segundos que o navegador reproduziu
```

# Especificando o intervalo de reprodução

Quando é especificado a URI de um elemento ```<audio>``` ou ```<video>```, você pode incluir 
opcionalmente informações adicionais para especificar a parte da mídia a ser reproduzida. 
Para fazer isso use uma hashtag ("#") seguida pela descrição do fragmento da mídia.

O intervalo é especificado usando a sintaxe:

```php
#t=[tempoinicial],[tempofinal]
```

O tempo pode ser especificado como um número de segundos (como um valor de ponto flutuante) 
ou no formato horas:minutos:segundos (como 2:05:01 para 2 horas, 5 minutos, e 1 segundo).

**Alguns exemplos:**

**http://foo.com/video.ogg#t=10,20**
<br />
Especifica que o intervalo entre 10 e 20 segundos deve ser reproduzido.


**http://foo.com/video.ogg#t=,10.5**
<br />
Especifica que o vídeo deve ser reproduzido do início até 10,5 segundos.


**http://foo.com/video.ogg#t=,02:00:00**
<br />
Especifica que o vídeo deve ser reproduzido do início até 2 horas.


**http://foo.com/video.ogg#t=60,**
<br />
Especifica que o vídeo deve começar aos 60 segundos e ser reproduzido até o final. 

# Opções alternativas

O HTML inclui elementos que podem ser colocados entre as tags iniciais e finais 
de codigo que é processado por navegadores que não suportam mídia em HTML5. 
É possível aproveitar esse fato para prover alternativas para esses navegadores.

Essa seção mostra duas alternativas possíveis para vídeos. Em cada caso, se o navegador 
suportar HTML5, ele é usado, se não for posível a alternativa é utilizada.

**Utilizando Flash**

Você pode utilizar Flash para reproduzir um vídeo no formato Flash caso o 
elemento ```<video>``` não seja suportado:

```php
<video src="video.ogv" controls>
    <object data="flvplayer.swf" type="application/x-shockwave-flash">
      <param value="flvplayer.swf" name="movie"/>
    </object>
</video>
```

Note que você não deve incluir ```classid``` na tag object para que outros navegadores 
além do Internet Explorer sejam compatíveis.

<br />

**Reproduzindo vídeos em Ogg usando uma applet Java**

Existe uma applet Java chamada [Cortado](http://maikmerten.livejournal.com/2256.html) que você pode utilizar como alternativa 
para reproduzir vídeos em Ogg em navegadores que possuem suporte a Java, 
mas não suportam vídeos em HTML5:

```php
<video src="my_ogg_video.ogg" controls width="320" height="240">
  <object type="application/x-java-applet"
          width="320" height="240">
     <param name="archive" value="cortado.jar">
     <param name="code" value="com.fluendo.player.Cortado.class">
     <param name="url" value="my_ogg_video.ogg">
     <p>You need to install Java to play this file.</p>
  </object>
</video>
```

Se você não criar um elemento filho alternativo do elemento objeto cortado, como o 
elemento ```<p>``` mostrado acima, o Firefox 3.5 que conseguem reproduzir o vídeo 
mas não tem Java instalado vão informar incorretamente ao usuário que ele precisa 
instalar um plugin para visualizar o conteúdo da página.

# Error handling
(Firefox 4 / Thunderbird 3.3 / SeaMonkey 2.1)

A partir do **Gecko 2.0 (Firefox 4 / Thunderbird 3.3 / SeaMonkey 2.1)**, o gerenciamento de 
erros é revisada para corresponder à última versão da especificação do HTML5. 
Ao invés do evento error ser enviado ao elemento media, ele é enviado ao elemento 
filho [\<source\>](https://developer.mozilla.org/pt-BR/docs/Web/HTML/Element/source) correspondente às fontes em que ocorreram o erro.

Isso permite que você detecte que fonte falhou, o que pode ser útil. Considere esse código HTML:

```php
<video>
  <source id="mp4_src"
        src="video.mp4"
        type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'>
  </source>
  <source id="3gp_src"
        src="video.3gp"
        type='video/3gpp; codecs="mp4v.20.8, samr"'>
  </source>
  <source id="ogg_src"
        src="video.ogv"
        type='video/ogg; codecs="theora, vorbis"'>
  </source>
</video>
```

Como o Firefox não suporta MP4 e 3GP por serem patenteados, os elementos [\<source\>](https://developer.mozilla.org/pt-BR/docs/Web/HTML/Element/source) 
com os IDs **"mp4_src"** e **"3gp_src"** vão receber eventos error antes que o 
rescurso Ogg seja carregado. As fontes são testadas na ordem em que aparecem, 
e assim que uma é carregada de maneira correta, o resto das fontes não são testadas.

# Detectando quando nenhuma fonte foi carregada

Para detectar que todos os elementos filhos <source> falharam, confira os valores do 
atributo networkState do elemento media. Se esse valor for HTMLMediaElement.NETWORK_NO_SOURCE, 
você saberá que todas as fontes falharam o carregamento.

Se nesse ponto você inserir uma outra fonte ao inserir um novo elemento [\<source\>](https://developer.mozilla.org/pt-BR/docs/Web/HTML/Element/source) 
como filho do elemento media, o Gecko tenta carregar o recurso especificado.


<br />


**Conteúdo replicado de:**

* https://developer.mozilla.org/pt-BR/docs/Web/HTML/Using_HTML5_audio_and_video

<br />

**Referências:**

* http://www.w3schools.com/html/html5_audio.asp
* http://www.w3schools.com/html/html5_video.asp