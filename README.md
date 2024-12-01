# dynamic-batch-TTS-pipeline

Dynamic batching for Speech Enhancement and diffusion based TTS.

1. Dynamic batching for SOTA Speech Enhancement and diffusion based TTS, suitable to serve better concurrency.
2. Can serve user defined max concurrency.

## Available models

### Speech Enhancement

1. https://github.com/resemble-ai/resemble-enhance

### TTS

1. https://github.com/SWivid/F5-TTS
2. https://github.com/jaywalnut310/vits

## how to install

Using PIP with git,

```bash
pip3 install git+https://github.com/mesolitica/dynamic-batch-TTS-pipeline
```

Or you can git clone,

```bash
git clone https://github.com/mesolitica/dynamic-batch-TTS-pipeline && cd dynamic-batch-TTS-pipeline
```

## how to

### Supported parameters

```bash
python3 -m dynamicbatch_ttspipeline.main --help
```

```
usage: main.py [-h] [--host HOST] [--port PORT] [--loglevel LOGLEVEL] [--reload RELOAD] [--torch-dtype TORCH_DTYPE]
               [--enable-speech-enhancement ENABLE_SPEECH_ENHANCEMENT]
               [--model-speech-enhancement MODEL_SPEECH_ENHANCEMENT] [--enable-tts ENABLE_TTS] [--model-tts MODEL_TTS]
               [--dynamic-batching-microsleep DYNAMIC_BATCHING_MICROSLEEP]
               [--dynamic-batching-speech-enhancement-batch-size DYNAMIC_BATCHING_SPEECH_ENHANCEMENT_BATCH_SIZE]
               [--dynamic-batching-ts-batch-size DYNAMIC_BATCHING_TS_BATCH_SIZE] [--accelerator-type ACCELERATOR_TYPE]
               [--max-concurrent MAX_CONCURRENT] [--torch-compile TORCH_COMPILE]

Configuration parser

options:
  -h, --help            show this help message and exit
  --host HOST           host name to host the app (default: 0.0.0.0, env: HOSTNAME)
  --port PORT           port to host the app (default: 7088, env: PORT)
  --loglevel LOGLEVEL   Logging level (default: INFO, env: LOGLEVEL)
  --reload RELOAD       Enable hot loading (default: False, env: RELOAD)
  --torch-dtype TORCH_DTYPE
                        Torch data type (default: bfloat16, env: TORCH_DTYPE)
  --enable-speech-enhancement ENABLE_SPEECH_ENHANCEMENT
                        Enable document layout detection (default: True, env: ENABLE_SPEECH_ENHANCEMENT)
  --model-speech-enhancement MODEL_SPEECH_ENHANCEMENT
                        Model type (default: resemble-enhance, env: MODEL_SPEECH_ENHANCEMENT)
  --enable-tts ENABLE_TTS
                        Enable OCR (default: True, env: ENABLE_TTS)
  --model-tts MODEL_TTS
                        Model type (default: f5-tts, env: MODEL_TTS)
  --dynamic-batching-microsleep DYNAMIC_BATCHING_MICROSLEEP
                        microsleep to group dynamic batching, 1 / 1e-4 = 10k steps for second (default: 0.0001, env:
                        DYNAMIC_BATCHING_MICROSLEEP)
  --dynamic-batching-speech-enhancement-batch-size DYNAMIC_BATCHING_SPEECH_ENHANCEMENT_BATCH_SIZE
                        maximum of batch size for speech enhancement during dynamic batching (default: 5, env:
                        DYNAMIC_BATCHING_SPEECH_ENHANCEMENT_BATCH_SIZE)
  --dynamic-batching-ts-batch-size DYNAMIC_BATCHING_TS_BATCH_SIZE
                        maximum of batch size for TTS during dynamic batching (default: 5, env:
                        DYNAMIC_BATCHING_TTS_BATCH_SIZE)
  --accelerator-type ACCELERATOR_TYPE
                        Accelerator type (default: cuda, env: ACCELERATOR_TYPE)
  --max-concurrent MAX_CONCURRENT
                        Maximum concurrent requests (default: 100, env: MAX_CONCURRENT)
  --torch-compile TORCH_COMPILE
                        Torch compile necessary forwards, can speed up at least 1.5X (default: False, env: TORCH_COMPILE)
```

**We support both args and OS environment**.

### Run

```bash
python3 -m dynamicbatch_ttspipeline.main \
--host 0.0.0.0 --port 7088
```

### Run with Torch Compile

Want speed up at least 1.5X? Use torch compile!

```bash
python3 -m dynamicbatch_ttspipeline.main \
--host 0.0.0.0 --port 7088 \
--torch-compile true --dynamic-batching-speech-enhancement-batch-size 2
```

**Compiling use a lot of GPU memory, make sure set low batch size**.

#### Example speech enhancement

```bash
curl -X 'POST' \
  'http://localhost:7088/speech_enhancement' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'file=@stress-test/test.mp3;type=audio/mpeg'
```

Checkout [notebook/speech-enhancement.ipynb](notebook/speech-enhancement.ipynb).

Checkout the speed of torch compile [notebook/speech-enhancement-torch-compile.ipynb](notebook/speech-enhancement-torch-compile.ipynb).