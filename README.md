# Data-Augmentation-to-help-with-Guitar-String-Separation

This repository contains the code for my dissertation on using data augmentation to improve guitar string separation with deep learning.

### Overview

Accurate detection and separation of individual guitar strings from an audio signal is a challenging task due to complex guitar acoustics and lack of diverse training data. This dissertation explores the application of data augmentation techniques to improve guitar string separation using a U-Net model.
The main techniques investigated are:
- Time Stretching
- Pitch Shifting
- Noise Injection

The aim is to enhance model robustness, improve generalization, and increase separation accuracy by augmenting the training data.

### Repository Files
The repository contains the following key components
- Model Train and Evaluation Notebook [Guitar String Separation](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/GSS.ipynb)
- Exploratory Analysis for each channel of the Ground Truth Library to understand the musical componets - [Exploratory Analysis](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Exploratory_Analysis.ipynb)
- Notebook to perform Data Augmentation Techinques and create Augmented Datasets - [Data Augmentation](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Data_Augmentation_Dataset_Creation.ipynb)

### Requirements
- Python 3.6+
- Pytorch 1.0+
- Other common packages lusted in [requirements.txt](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/requirements.txt)

## Details About the Augmentation Techniques.

### Time Stretching

The dissertation implements time stretching by first separating the audio into harmonic and percussive components using harmonic-percussive source separation (HPSS). The harmonic and percussive parts are then time stretched independently using librosa's time_stretch function. This applies a phase vocoder technique to stretch the signals between 0.9 and 1.1 times their original length, selected randomly per sample. The time-modified harmonic and percussive components are recombined to produce the final time-stretched audio.

Separating the components avoids undesirable artifacts and maintains transient quality. Stretching between 0.9-1.1x introduces realistic variations in tempo.


![Time Stretching](Images/Time_Stretching.png)


- Audio without Augmentation - [Base Audio](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Test%20Audio%20File/00_BN1-129-Eb_comp.wav)


- Audio with Time Stretching - [Time Stretching](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Test%20Audio%20File/BN1_TS.wav)


### Pitch Shifting

For pitch shifting, the entire signal is first shifted between -2 and 2 semitones using librosa's pitch_shift. Then, the harmonic part is isolated with HPSS and pitch-shifted separately by the same randomly selected interval. This second shift on just the harmonic introduces inharmonicity. The original shifted signal is added back to the inharmonic harmonic component to get the final pitch-shifted audio.

Shifting both the entire signal and its harmonic part exposes the model to realistic pitch changes and inharmonicity in guitar tones during training. The -2 to 2 semitone range covers interval fluctuations when playing.

![Pitch Shifting](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Images/Pitch%20Shifting.png)

- Audio with Pitch Shifting - [Pitch Shifting](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Test%20Audio%20File/BN1_PS.wav)

### Noise Injection 

Random white Gaussian noise is generated using NumPy's random.normal distribution. The power of the noise and original signal are calculated. The noise is then scaled by a ratio of the signal power to noise power before adding it to the signal. A noise_factor hyperparameter controls the amount of noise added.

This injects random noise proportional to the signal power, simulating real-world ambient and electronic noise. The model learns to separate strings even with noisy input.

![Noise Injection](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Images/Noise_Injection.png)

- Audio with Noise Injection - [Noise Injection](https://github.com/change0z/Data-Augmentation-to-help-with-Guitar-String-Separation/blob/main/Test%20Audio%20File/BN1_NI.wav)

### Results

The experiments compare the performance of the U-Net model on the guitar string separation task with different data augmentation techniques. The model is evaluated on the test set using signal-to-distortion ratio (SDR) as the primary metric.

The results demonstrate that certain augmentations like pitch shifting and time stretching, when combined with the original training data, can significantly improve the model's accuracy. These techniques introduce realistic variations in pitch and timing that enhance the diversity of the dataset. The model is able to learn more robust representations that translate to better separation quality on the unseen test data.

However, excessive augmentation through random noise injection is shown to degrade performance. While real guitar recordings contain inherent noise, injecting too much artificial noise obscures the underlying patterns for the model to learn effectively. Additionally, finding the right balance between augmentations and original training samples is important.

Overall, the experiments highlight the potential of data augmentation, when applied appropriately, to improve separation accuracy by exposing the model to useful transformations of the training data. However, augmentation should complement and not replace original diverse recordings.

### Conclusion

The experiments compare the performance of the U-Net model on the guitar string separation task with different data augmentation techniques. The model is evaluated on the test set using signal-to-distortion ratio (SDR) as the primary metric.

The results demonstrate that certain augmentations like pitch shifting and time stretching, when combined with the original training data, can significantly improve the model's accuracy. These techniques introduce realistic variations in pitch and timing that enhance the diversity of the dataset. The model is able to learn more robust representations that translate to better separation quality on the unseen test data.

However, excessive augmentation through random noise injection is shown to degrade performance. While real guitar recordings contain inherent noise, injecting too much artificial noise obscures the underlying patterns for the model to learn effectively. Additionally, finding the right balance between augmentations and original training samples is important.

Overall, the experiments highlight the potential of data augmentation, when applied appropriately, to improve separation accuracy by exposing the model to useful transformations of the training data. However, augmentation should complement and not replace original diverse recordings.
