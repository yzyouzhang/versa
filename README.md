# VERSA
VERSA (Versatile Evaluation of Speech and Audio) is a toolkit dedicating a collection of evaluation metrics in speech and audio quality. Our goal is to provide comprehensive connection to the cutting-edge techniques developped for evaluation. The toolkit is also tightly integrated to [ESPnet](https://github.com/espnet/espnet.git).


## Install

The base-installation is as easy as follows:
```
git clone https://github.com/shinjiwlab/versa.git
cd versa
pip install .
```
or
```
pip install git+https://github.com/shinjiwlab/versa.git
```

As for collection purpose, VERSA instead of re-distribu the model, we try to align as much to the original API provided by the algorithm developer. Therefore, we are having many dependencies. We try to include as many as default, but there are cases where the toolkit needs specific installtion requirements. Please refer to our [list-of-metric section](https://github.com/shinjiwlab/versa?tab=readme-ov-file#list-of-metrics) for more details on whether the metrics are automatically included or not.  If not, we provide installation guide or installers in `tools`.




You need to manually install visqol. We prepared a detailed installation guide at https://github.com/ftshijt/speech_evaluation/blob/main/speech_evaluation/utterance_metrics/INSTALL_visqol.md if you met issue with original repo https://github.com/google/visqol


## Quick test
```
python speech_evaluation/bin/test.py
```

## Usage

Simple usage case for a few samples.
```
# direct usage
python speech_evaluation/bin/scorer.py \
    --score_config egs/codec_16k.yaml \
    --gt egs/test/test1 \
    --output_file egs/test/test_result.txt
```

Use launcher with slurm job submissions
```
# use the launcher
# Option1: with gt speech
./launcher.sh \
  <pred_speech_scp> \
  <gt_speech_scp> \
  <score_dir> \
  <split_job_num> 

# Option2: without gt speech
./launcher.sh \
  <pred_speech_scp> None 
  <score_dir> \
  <split_job_num>

# aggregate the results
cat <score_dir>/result/*.result.cpu.txt > <score_dir>/utt_result.cpu.txt
cat <score_dir>/result/*.result.gpu.txt > <score_dir>/utt_result.gpu.txt

# show result
python scripts/show_result.py <score_dir>/utt_result.cpu.txt
python scripts/show_result.py <score_dir>/utt_result.gpu.txt 

```

Access `egs/*.yaml` for different config for differnt setups.

## List of Metrics

We include [ ] and [x] to mark if the metirc is auto-installed in versa. 

| Metric Name  (Auto-Install)  | Key in config | Key in report  | Details | Code Source                                                                                                     | References                                                                                       |
|------------------|---------------|---------------|---------|-----------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| Mel Cepstral Distortion (MCD) [x]  | mcd_f0 | mcd | | [espnet](https://github.com/espnet/espnet) and [s3prl-vc](https://github.com/unilight/s3prl-vc) | https://ieeexplore.ieee.org/iel2/3220/9154/00407206.pdf |
| F0 Correlation [x]  | mcd_f0 | f0_corr | | [espnet](https://github.com/espnet/espnet) and [s3prl-vc](https://github.com/unilight/s3prl-vc) | https://ieeexplore.ieee.org/iel7/9040208/9052899/09053512.pdf |
| F0 Root Mean Square Error  [x] | mcd_f0 | f0_rmse | | [espnet](https://github.com/espnet/espnet) and [s3prl-vc](https://github.com/unilight/s3prl-vc) | https://ieeexplore.ieee.org/iel7/9040208/9052899/09053512.pdf |
| Signal-to-infererence Ratio (SIR) [x]  | signal_metric | sir | | [espnet](https://github.com/espnet/espnet) | - |
| Signal-to-artifact Ratio (SAR) [x]  | signal_metric | sar | | [espnet](https://github.com/espnet/espnet) | - |
| Signal-to-distortion Ratio (SDR) [x]  | signal_metric | sdr | | [espnet](https://github.com/espnet/espnet) | - |
| Convolutional scale-invariant signal-to-distortion ratio (CI-SDR) [x]  | signal_metric | ci-sdr | | [ci_sdr](https://github.com/fgnt/ci_sdr) | https://arxiv.org/abs/2011.15003 |
| Scale-invariant signal-to-noise ratio (SI-SNR) [x]  | signal_metric | si-snr | | [espnet](https://github.com/espnet/espnet) | https://arxiv.org/abs/1711.00541 |
| Perceptual Evaluation of Speech Quality (PESQ) [x]  | pesq | pesq | | [pesq](https://pypi.org/project/pesq/) | https://ieeexplore.ieee.org/document/941023 |
| Short-Time Objective Intelligibility (STOI) [x]  | stoi | stoi | | [pystoi](https://github.com/mpariente/pystoi) | https://ieeexplore.ieee.org/document/5495701 |
| Speech BERT Score [x]  | discrete_speech | speech_bert | | [discrete speech metric](https://github.com/Takaaki-Saeki/DiscreteSpeechMetrics) | https://arxiv.org/abs/2401.16812 |
| Discrete Speech BLEU Score [x]  | discrete_speech | speech_belu | | [discrete speech metric](https://github.com/Takaaki-Saeki/DiscreteSpeechMetrics) | https://arxiv.org/abs/2401.16812 |
| Discrete Speech Token Edit Distance [x]  | discrete_speech | speech_token_distance | | [discrete speech metric](https://github.com/Takaaki-Saeki/DiscreteSpeechMetrics) | https://arxiv.org/abs/2401.16812 |
| UTokyo-SaruLab System for VoiceMOS Challenge 2022 (UTMOS) [x]  | pseudo_mos | utmos | | [speechmos](https://github.com/tarepan/SpeechMOS) | https://arxiv.org/abs/2204.02152 |
| Deep Noise Suppression MOS Score of P.835 (DNSMOS) [x]  | pseudo_mos | dnsmos_overall | | [speechmos (MS)](https://pypi.org/project/speechmos/) | https://arxiv.org/abs/2110.01763 |
| Deep Noise Suppression MOS Score of P.808 (DNSMOS) [x]  | pseudo_mos | dnsmos_p808 | | [speechmos (MS)](https://pypi.org/project/speechmos/) | https://arxiv.org/abs/2005.08138 |
| Packet Loss Concealment-related MOS Score (PLCMOS) [x]  | pseudo_mos | plcmos | | [speechmos (MS)](https://pypi.org/project/speechmos/) | https://arxiv.org/abs/2305.15127|
| Virtual Speech Quality Objective Listener (VISQOL) [ ]  | visqol | visqol | | [google-visqol](https://github.com/google/visqol) | https://arxiv.org/abs/2004.09584 |
| Speaker Embedding Similarity [x]  | speaker | spk_similarity | | [espnet](https://github.com/espnet/espnet) | https://arxiv.org/abs/2401.17230 |
| PESQ in TorchAudio-Squim [x]  | squim | torch_squim_pesq | | [torch_squim](https://pytorch.org/audio/main/tutorials/squim_tutorial.html) | https://arxiv.org/abs/2304.01448 |
| STOI in TorchAudio-Squim [x]  | squim | torch_squim_stoi | | [torch_squim](https://pytorch.org/audio/main/tutorials/squim_tutorial.html) | https://arxiv.org/abs/2304.01448 |
| SI-SDR in TorchAudio-Squim [x]  | squim | torch_squim_si_sdr | | [torch_squim](https://pytorch.org/audio/main/tutorials/squim_tutorial.html) | https://arxiv.org/abs/2304.01448 |
| MOS in TorchAudio-Squim [x]  | squim | torch_squim_mos |  | [torch_squim](https://pytorch.org/audio/main/tutorials/squim_tutorial.html) | https://arxiv.org/abs/2304.01448 |
