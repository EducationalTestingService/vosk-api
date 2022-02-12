# Vosk Speech Recognition Toolkit

Vosk is an offline open source speech recognition toolkit. It enables
speech recognition for 20+ languages and dialects - English, Indian
English, German, French, Spanish, Portuguese, Chinese, Russian, Turkish,
Vietnamese, Italian, Dutch, Catalan, Arabic, Greek, Farsi, Filipino,
Ukrainian, Kazakh, Swedish, Japanese, Esperanto. More to come.

Vosk models are small (50 Mb) but provide continuous large vocabulary
transcription, zero-latency response with streaming API, reconfigurable
vocabulary and speaker identification.

Speech recognition bindings implemented for various programming languages
like Python, Java, Node.JS, C#, C++ and others.

Vosk supplies speech recognition for chatbots, smart home appliances,
virtual assistants. It can also create subtitles for movies,
transcription for lectures and interviews.

Vosk scales from small devices like Raspberry Pi or Android smartphone to
big clusters.

# Documentation

For installation instructions, examples and documentation visit [Vosk
Website](https://alphacephei.com/vosk).

# Test the functionality implemented in this repository using C scripts

1. Clone the `ETS fork` of the `Vosk API` repository [https://github.com/EducationalTestingService/vosk-api](https://github.com/EducationalTestingService/vosk-api).

2. If you are using your own custom ASR model, copy the model into the `model` directory in the structure shared in the section `Model structure` [on VOSK-API website](https://alphacephei.com/vosk/models). Alternatively, you can download one of the [open-source ASR models available on Vosk-API website](https://alphacephei.com/vosk/models),  uncompress the downloaded zip file and, rename the containing directory to `model`.
**NOTE**: To be able to extract phoneme labels and timestamps, you need to include the `phones.txt` file that was used during ASR model buildiong in the `model/graph` directory, so Vosk's open source models may not work out of the box.

3. Add the audio file to be used to test the C test script under `c` and rename the file to `test.wav` to use the script as is. 

4. The docker image for building Kaldi and the Python wheels is located in the cloned repository under `travis`.

   Build and run the docker image as follows:

   ```
   docker build --file Dockerfile.manylinux --tag alphacep/kaldi-manylinux:latest .
   docker run -ti -v $(pwd)/..:/io alphacep/kaldi-manylinux /bin/bash
   ```

5. Inside the docker container under the `io/src` folder run `KALDI_ROOT=/opt/kaldi make all` to compile your changes.

6. Go to the `io/c` folder `ln -s ../model .` to symlink the model directory you created in step 2 inside this folder. Next, run `KALDI_ROOT=/opt/kaldi make all` to compile the C scripts. This should create an executable for the C script. To test the custom functionalities built by ETS team, run the command `./test_phone_results`. This should print out the expected results for the script.