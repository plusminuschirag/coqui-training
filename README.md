# COQUI TTS Training and Adding a New Language

## Implementation
* Check the google collab for the implementation [here](https://colab.research.google.com/drive/1g1QE8k5Ro882Y4AzMDPpS-x32f4_Ymx6?usp=sharing)
* Ran out of memory on T4 GPU.

## Finetuning VITS Model on Hindi

* Download the dataset from [here](https://www.iitm.ac.in/donlab/indictts/database) : Download `MonoMale` and within MonoMale we are using `hindi_male_mono`
* Upload the dataset to Google Drive (downloaded zip from the website) to a folder called `Indic-Dataset` Making your whole file path `Indic-Dataset/hindi_male_mono.zip`
* Following steps for making dataset ready are also provided in google collab:
  * mount your google drive, complete OAuth
  * Run following commands to extract the dataset in expected format
    ```
    !pwd
    !mkdir Indic_Dataset_Extracted
    !unzip /content/drive/MyDrive/Indic-Dataset/hindi_male_mono.zip -d Indic_Dataset_Extracted/
    ```
  * Create Directory on Google Session and Run the following:
    ```
    !mkdir tts_train_dir
    !unzip Indic_Dataset_Extracted/hindi_male_mono1.zip -d tts_train_dir/
    !mv tts_train_dir/speaker2 tts_train_dir/hindi_male_mono1
    !mv tts_train_dir/hindi_male_mono1/wav tts_train_dir/hindi_male_mono1/wavs
    ```
  * Fix the numpy 1.22 and 1.23 discrepancy
    ```
    !pip install numpy==1.23
    ```
  * Change the environment to GPU and set `CUDA_VISIBLE_DEVICES="0"` for training
  * `data_formatting.py` : Script present in collab to convert donwloaded data to standard `LJSpeech` Format.
  * Rest is the training steps for this model on the Collab.
 

### Why not XTTS V2 and VITS?
  * While doing XTTS V2, ran into configuration and tensor error, hence due to time crunch went along with VITS model, for whom configuration worked fine.

### How to add Heavy Tamil Accent?
  * Setting the language to hindi when text is in Tamil will do the job.


### How to train a model on a new language from scratch
  * Make sure you have installed TTS in pip editable mode.
  * Check `python TTS/tts/utils/text/phonemizers/__init__.py `
      * We can check if that language is supported, if yes then from there it is straightforward,
      * Set
         ```
         use_phonemes=True,
         phonemizer="espeak",
         phoneme_language="en",
        ```
      * Configure CharConfig in case it's new
        ```
        char_config = CharactersConfig(...)
        config = ModelConfig(...
           characters=char_config
           ...
        )
        ```
     * Use Example Recipes, to train the model.
         

### References
 * [Adding an Accent](https://www.reddit.com/r/Oobabooga/comments/18ajb00/any_luck_using_coqui_tts_with_language_accents/)
 * [Adding a New Language](https://docs.coqui.ai/en/latest/implementing_a_new_language_frontend.html)
 * [Ollama Implementation for XTTS V2](https://huggingface.co/coqui/XTTS-v2)
  
