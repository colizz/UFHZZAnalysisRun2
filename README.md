Table of Contents
=================

* [HZZ Analyzer for CMS Run2 UL](#hzz-analyzer-for-cms-run2-ul)
   * [To install:](#to-install)
   * [Alternate installation](#alternate-installation)
   * [local run and tests](#local-run-and-tests)
* [pre-existing](#pre-existing)
   * [cfg of Data and MC to be used for each year](#cfg-of-data-and-mc-to-be-used-for-each-year)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

# HZZ Analyzer for CMS Run2 UL


## To install:

you can use `install_UL.sh` 

```bash
wget https://raw.githubusercontent.com/qyguo/UFHZZAnalysisRun2/UL_10_6_26/install_UL.sh
chmod +x install_UL.sh
./install_UL.sh
```

## Alternate installation

`install_UL.sh` is same as the following commands.

<details>
  <summary>Click to expand!</summary>

```bash
SCRAM_ARCH=slc7_amd64_gcc700; export SCRAM_ARCH
cmsrel CMSSW_10_6_26
cd CMSSW_10_6_26/src/
cmsenv
git cms-init

git clone -b UL_10_6_26 https://github.com/qyguo/UFHZZAnalysisRun2.git

##git cms-merge-topic asculac:Electron_XGBoost_MVA_16UL_17UL

git cms-addpkg GeneratorInterface/RivetInterface

git cms-addpkg SimDataFormats/HTXS

git cms-addpkg RecoEgamma/PhotonIdentification

git cms-addpkg RecoEgamma/ElectronIdentification

git cms-merge-topic cms-egamma:EgammaPostRecoTools

git cms-addpkg RecoEgamma/EgammaTools

git clone https://github.com/cms-egamma/EgammaPostRecoTools.git

mv EgammaPostRecoTools/python/EgammaPostRecoTools.py RecoEgamma/EgammaTools/python/.

#git clone https://github.com/cms-data/EgammaAnalysis-ElectronTools.git EgammaAnalysis/ElectronTools/data/

git clone -b ULSSfiles_correctScaleSysMC https://github.com/jainshilpi/EgammaAnalysis-ElectronTools.git EgammaAnalysis/ElectronTools/data/

git cms-addpkg EgammaAnalysis/ElectronTools

git cms-addpkg  RecoJets/JetProducers

git cms-addpkg PhysicsTools/PatAlgos/

git clone -b v2.3.5 https://github.com/JHUGen/JHUGenMELA

sh JHUGenMELA/MELA/setup.sh -j 8

git clone https://github.com/bachtis/Analysis.git -b KaMuCa_V4 KaMuCa

git clone -b tmp_Ferrico https://github.com/ferrico/KinZfitter.git

scramv1 b -j 8

voms-proxy-init --rfc --voms cms
```

</details>


## local run and tests

```bash
cmsRun UFHZZAnalysisRun2/UFHZZ4LAna/python/Sync_106X_2018UL_cfg.py
cmsRun UFHZZAnalysisRun2/UFHZZ4LAna/python/Sync_106X_2017UL_cfg.py
```

# pre-existing

```
cp UFHZZAnalysisRun2/Utilities/crab/* .

#voms-proxy-init --valid=168:00
#probably need "voms-proxy-init -voms cms -rfc"

voms-proxy-init -voms cms -rfc
source /cvmfs/cms.cern.ch/crab3/crab.sh
```

**NOTE:** before submitting jobs, please check the outLFNDirBase and storageSite in `crabConfig_TEMPLATE.py` 


```bash
python SubmitCrabJobs.py -t "DataUL18" -d SampleList_UL18_Data.txt -c UFHZZAnalysisRun2/UFHZZ4LAna/python/templateData_106X_2018UL_cfg.py
```

## cfg of Data and MC to be used for each year

- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateData_106X_2016ULAPV_cfg.py`
- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateData_106X_2016UL_cfg.py`
- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateData_106X_2017UL_cfg.py`
- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateData_106X_2018UL_cfg.py`
- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateMC_106X_2016ULAPV_cfg.py`
- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateMC_106X_2016UL_cfg.py`
- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateMC_106X_2017UL_cfg.py`
- `UFHZZAnalysisRun2/UFHZZ4LAna/python/templateMC_106X_2018UL_cfg.py`

or similary for MC:

```bash
python SubmitCrabJobs.py -t "MC_UL18" -d SampleList_UL18_MC.txt -c UFHZZAnalysisRun2/UFHZZ4LAna/python/templateMC_106X_2018UL_cfg.py
```

You can use `manageCrabTask.py` to check the status, resubmit, or kill your task. E.g. after submitting:

```bash
nohup python -u manageCrabTask.py -t resultsAna_Data_M17_Feb19 -r -l >& managedata.log &
```

This will start an infinite loop of running crab resubmit on all of your tasks, then sleep for 30min. You should kill the process once all of your tasks are done. Once all of your tasks are done, you should run the following command to purge your crab cache so that it doesn't fill up:

```bash
python manageCrabTask.py -t resultsAna_Data_M17_Feb19 -p
```

