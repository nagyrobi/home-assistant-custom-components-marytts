# Mary TTS - with extra prepending delay

This is a fork of Home Assistant's built-in Mary TTS Text-to-Speech component as a custom component, with an extra option added for prepending a silence delay to the audio stream.

Many people complain about network media players cutting down the first bits of the TTS audio because their devices need a little time to wake up from standby or idle state when they receive a network stream to play. The only good solution for this is to add a configurable amount of silence at the beginning of the audio stream. The proposed solution does this locally, and the generated files stay like that in the cache.

Adding this as a custom component to your Home Assistant instance will override the internal component with the same name so you can still use it as before, with the extended functionality as below. You can install it also with HACS if you add it as a [custom repository like this](https://hacs.xyz/docs/faq/custom_repositories).

## Configuration

To enable text-to-speech with Google, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
tts:
  - platform: marytts
    delay: 800
```
Configuration options:
```
delay:
  description: "Prepend the generated speech with this amount of silence (miliseconds)."
  required: false
  type: int
  default: 0
host:
  description: The MaryTTS server hostname or IP address.
  required: false
  type: string
  default: localhost
port:
  description: The MaryTTS server port.
  required: false
  type: integer
  default: 59125
codec:
  description: "The audio codec. Supported codecs are `AIFF_FILE`, `AU_FILE` and `WAVE_FILE`."
  required: false
  type: string
  default: "`WAVE_FILE`"
voice:
  description: The speaker voice.
  required: false
  type: string
  default: "`cmu-slt-hsmm`"
language:
  description: "The language to use. Supported languages are `de`, `en_GB`, `en_US`, `fr`, `it`, `lb`, `ru`, `sv`, `te` and `tr`."
  required: false
  type: string
  default: "`en_US`"
effect:
  description: "A dictionary of effects which should be applied to the speech output."
  required: false
  type: map
```
With the `delay` option you can add some silence to the beginning of the rendered speech, to cope with audio systems which need time to wake up their speakers when starting a network stream. Value is in miliseconds, maximum is 15000 (15s).

When `delay` is set, the audio stream is sent to the player in uncompressed `wav` format to preserve audio quality (avoid recompression). Without `delay` set, the stream is sent unmodified, in its original format.

See [documentation](http://mary.dfki.de/documentation/index.html) for other details.

## Speech effects

For more information about the different effects take a look at the demo page of your MaryTTS installation (`http://localhost:59125/`).

There you can read about each effect and also test them on the fly.

## Full configuration example

A full configuration sample including optional variables:

```yaml
# Example configuration.yaml entry
tts:
  - platform: marytts
    host: "localhost"
    port: 59125
    codec: "WAVE_FILE"
    voice: "cmu-slt-hsmm"
    language: "en_US"
    delay: 800
    effect:
      Volume: "amount:2.0;"
      TractScaler: "amount:1.5;"
      F0Scale: "f0Scale:2.0;"
      F0Add: "f0Add:50.0;"
      Rate: "durScale:1.5;"
      Robot: "amount:100.0;"
      Whisper: "amount:100.0;"
      Stadium: "amount:100.0"
      Chorus: "delay1:466;amp1:0.54;delay2:600;amp2:-0.10;delay3:250;amp3:0.30"
      FIRFilter: "type:3;fc1:500.0;fc2:2000.0"
      JetPilot: ""
```

