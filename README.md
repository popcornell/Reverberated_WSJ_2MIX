# Kinect-WSJ
Code to simulate a reverberated, noisy version of the WSJ0-2MIX dataset. Microphones are placed on a linear array with spacing between the devices resembling that of Microsoft Kinect &trade;, the device used to record the CHiME-5 dataset. This was done so that we could use the real ambient noise captured as part of CHiME-5 dataset. The room impulse responses (RIR) were simulated for a sampling rate of 16,000 Hz.


## Requirements
* [CHIME5 data set](http://spandh.dcs.shef.ac.uk/chime_challenge/CHiME5)
* [Dihard 2 labels](https://coml.lscp.ens.fr/dihard/index.html)
* [WSJ 2 mix](http://www.merl.com/demos/deep-clustering/create-speaker-mixtures.zip)

## Instructions

Run ``` ./create_corrupted_speech.sh --stage 0 --wsj_data_path  wsj_path --chime5_wav_base chime_path --dihard_sad_label_path dihard_path --dest save_path```

### Paths
* wsj_path :  Path to precomputed wsj-2mix dataset. Should contain the folder 2speakers/wav16k/
* chime_path : Path to chime-5 dataset. Should contain the folders train, dev and eval
* dihard_path : Path to dihard labels. Should contain ```*.lab``` files for the train and dev set


### Stages
* stage 0:  Download RIR files (~ 7 GB) to save_path, deflate the compressed files and create a softlink to the RIRs in the current folder
* stage 1:  Extract chime 5 noise using dihard labels
* stage 2:  Create the mixture. See the code to allow parallelization

## Output Data
Creates the following sub-folders in each of tr, tt and cv folders:

* s1 : spatial image of s1 (Reverberated version of s1 speaker)
* s2 : spatial image of s2 (Reverberated version of s2 speaker)
* s1_direct :  direct component of s1 at each of the microphones
* s2_direct : direct component of s2 at each of the microphones
* s1_early : Contains only the early reflections (first 50 ms of RIR. See config.py to change the value) of s1 at each of the microphones
* s2_early : Contains only the early reflections (first 50 ms of RIR. See config.py to change the value) of s2 at each of the microphones
* noise : Contains the noise imposed for each mixture
* mix : s1 + s2 + noise
* list.yaml : A yaml file containing the positions and direction of arrival (DOA) for each utterance and speaker

###### Hard disk usage


| Dataset type   | Per sub-folder   | Total *  |
|-----------|------------ |-------- |
| Train (tr)| 21G         | 168G    |
| Validation (cv)| 5.2G    | 41.6G   |
| Test (tt) | 3.2G        |25.6G    |

[*] Combination of mix, s1_early, s2_early, s1_direct, s2_direct and noise.


## References

[Analyzing the impact of speaker localization errors on speech separation for automatic speech recognition](https://arxiv.org/abs/1910.11114)


If you are using this code please cite the following paper:

```
@article{sivasankaran2019analyzing,
    title={Analyzing the impact of speaker localization errors on speech separation for automatic speech recognition},
    author={Sunit Sivasankaran and Emmanuel Vincent and Dominique Fohr},
    year={2019},
}
```
