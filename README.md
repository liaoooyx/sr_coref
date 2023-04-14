# Coreference Resolution

## Usage

### Model

Our coref model is based on [fast-coref](https://github.com/shtoshni/fast-coref) with minor modification

### Python requirements

python >= 3.9

```bash
pip install -r requirements.txt
```

### Python paths

If the scripts fail to import modules, please make sure the following paths are added to the PYTHONPATH environment variable.

```bash
export PYTHONPATH=/path_to/fast-coref/src
export PYTHONPATH=/path_to/str_rep_coref/src:$PYTHONPATH
```

### Pre-process the MIMIC-CXR data

```bash
cd ../str_rep_coref/src/data_preprocessing
python preprocess_mimic_cxr.py
```

The script output is: /output/mimic_cxr/mimic_cxr_sections.jsonlines

Check the src/data_preprocessing/README.md file for more configuation details.

### Linguistic pre-processing

#### Install CoreNLP

- Download CoreNLP from: <https://stanfordnlp.github.io/CoreNLP/>
- Install CoreNLP following the instruction: <https://stanfordnlp.github.io/CoreNLP/download.html#getting-a-copy>
  - Install Java
  - Setup CLASSPATH
- Make sure you can start the CoreNLP server. But shutdown the server before running our script, as the script will start a new server automatically.

#### Run script

```bash
cd ../str_rep_coref/src/nlp_ensemble
python process_mimic_cxr.py
```

### Coreference Resolution

Jupyter Notebook is required.

Check the src/coreference_resolution/README.md file for details.

## Cautions

The column of CSV files might not follow the same order. When the reports are being processed by CoreNLP with multiple coref annotators, some of the reports may not be successfully processed in the first round. We will re-run the coref annotators on `unfinished records` in the second round. This will lead to a different order of the columns for those second-round-processed reports. For those disorder reports' sid, you can find them from `/output/nlp_ensamble/run.log or corenlp_unfinished_records.log`

## Config

Please read the [Hydra Docs](https://hydra.cc/docs/intro/) for more details.

## Others

If you are using VSCode, add the following configs to get build-in supports:

.vscode/settings.json:

```json
{
    "python.analysis.extraPaths": [
        "/path_to/git_clone_repos/fast-coref/src",
        "/path_to/str_rep_coref/src",
    ],
    "terminal.integrated.env.linux": {
        "PYTHONPATH": "${workspaceFolder}/src:/path_to/fast-coref/src"
    },
}
```