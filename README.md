# IE7500 Group Project
<p><b>Group 2: Jie Lian, Jiajin Zhou, Peter Mink, Curtis Neiderer</b></p>

<hr>

# Milestone 1: Initial Project Proposal

## SkyComm-AIDE: An Automated NLP Agent for Air Traffic Communications

### <u>Description</u>
<p>This project builds an AI agent that listens to pilot transmissions and generates appropriate air traffic control (ATC) responses. To achieve this, we first classify each utterance as either “pilot” or “controller.” We then focus on pilot inputs—detecting their intent and critical flight parameters—and automatically propose safe, standardized ATC replies.</p>
<p>The project will be implemented as a minimum viable Python pipeline within a Jupyter Notebook. We will preprocess communication transcripts, extract relevant flight management information, and classify utterances according to their function within instrument control procedures, aiming to support automated monitoring or decision assistance tools for air traffic management.</p>

### <u>Problem Statement</u>
<p>Clear, timely responses from ATC are vital for safe flights—especially under instrument flight rules (IFR). Pilots transmit requests (e.g., altitude change, heading queries) and expect precise instructions. Human delays or misinterpretation can cause inefficiencies or safety risks.</p>

<p>We aim to automate the loop:</p>

1. <b>Detect:</b> Pilot speech in mixed transcripts.
2. <b>Extract:</b> Intent and flight parameters.
3. <b>Generate:</b> Compliant ATC responses.

<p>Automating this process will support controller decision-making and reduce workload peaks.</p>

### <u>Background</u>
<p>Air traffic control communications have unique linguistic characteristics, including use of standardized phraseology, abbreviations, and procedural commands. Previous research has explored speech recognition for ATC communications, intent detection, and anomaly detection in air traffic exchanges.</p>

<p>Relevant Papers:</p>

* "Automatic Speech Recognition for Air Traffic Control" by Balakrishnan et al. 
  * Techniques to transform utterances into instructions and acknowledgments.
* "Contextualizing Air Traffic Management Conversations using Natural Language Understanding" by Rohani et al.
  * Methods for Intent Classification (IC) and Slot Filling (SF) to identify and extract Traffic Management Initiatives (TMIs) from aviation-specific dialogues. 
* "Proof-of-concept study of a small language model chatbot for breast cancer decision support – a transparent, source-controlled, explainable and data-secure approach" by Griewing et al.
  * Shows a domain-specific small-scale chatbot proof-of-concept.

### <u>Methodology</u>
1. <b>Transcript Segmentation & Speaker Classification</b>
   * Input: mixed transcripts.
   * Model: fine-tuned transformer (e.g. BERT) to tag each line “pilot” vs. “controller.”
2. <b>Pilot Intent & Parameter Extraction</b>
   * Within “pilot” segments, classify intent: Altitude change, heading change, clearance request, position report, etc.
   * Use sequence-labeling (BiLSTM-CRF or fine-tuned BERT-NER) to pull out parameters (flight levels, headings, waypoints).
3. <b>Context Tracking</b>
   * Maintain a simple state dictionary: current altitude, heading, clearance status.
   * Update state after each pilot and generated ATC turn.
4. <b>ATC Response Generation</b>
   * Fine-tune a seq-to-seq model (e.g. T5 or GPT-2) on paired pilot-controller text.
   * Input: pilot utterance + current state.
   * Output: ATC-compliant sentence (“Climb and maintain FL200,” “Turn right heading 090.”).
5. <b>Evaluation</b>
   * <b>Automatic:</b> BLEU/ROUGE, intent-parameter accuracy.
   * <b>Domain:</b> Phraseology compliance checks.
   * <b>Human:</b> Subject matter expert evaluation with respect to clarity and safety.

### <u>Relevant Datasets</u>

* ATCOSIM Dataset ([https://www.spsc.tugraz.at/databases-and-tools/atcosim-air-traffic-control-simulation-speech-corpus.html](https://www.spsc.tugraz.at/databases-and-tools/atcosim-air-traffic-control-simulation-speech-corpus.html),  [https://huggingface.co/datasets/Jzuluaga/atcosim_corpus](https://huggingface.co/datasets/Jzuluaga/atcosim_corpus))
  * A speech database of air traffic control operator speech.
  * Consists of 10hr of speech data. 
  * Includes orthographic transcriptions and additional information on speakers and recording sessions.
* FAA Communications ATCO2 Corpus ([https://arxiv.org/abs/2211.04054](https://arxiv.org/abs/2211.04054), [https://www.atco2.org/data](https://www.atco2.org/data), [https://github.com/idiap/atco2-corpus](https://github.com/idiap/atco2-corpus))
  * A collection of real-world ATC-pilot voice transcripts annotated for procedural commands and interactions.
  * A large-scale dataset for research on automatic speech recognition and natural language understanding of air traffic control communications.
  * Comprises approximately 5000hr of ATC communications data.
* OpenSky Network Data ([https://opensky-network.org/data](https://opensky-network.org/data))
  * Flight trajectory data that can complement the textual data for validation and correlation.  

### <u>Expected Outcomes</u>
* End-to-end NLP pipeline:
    1. Speaker separation
    2. Intent and parameter extraction
    3. ATC reply generation
 * Comparative study of classical vs. transformer-based approaches
 * GitHub Repository

### <u>Team Roles</u>

| Role             | Responsibilities                                          |
| :-----------     | :---------------------------------------------------------|
| <b> Member 1</b> | Speaker classification and dataset preparation            |
| <b> Member 2</b> | Pilot intent categorization and slot extraction           |
| <b> Member 3</b> | Context management & response-generation model training   |
| <b> Member 4</b> | Evaluation, expert coordination, visualization, reporting |

<p><b>Note:</b> These roles are flexible, designed to encompass the major task areas for project completion. Team members are not locked into any one role and may take on different responsibilities throughout the project to foster collaboration.</p>

<hr>

# Milestone 2: Model Development

## Research and Methods

### <u>Objectives</u>
1. <b>Transcript Segmentation & Speaker Classification</b>
   * Tag each line within mixed transcripts as either “pilot” or “controller.”
2. <b>Pilot Intent & Parameter Extraction</b>
   * Within “pilot” segments, classify intent: Altitude change, heading change, clearance request, position report, etc.
   * Use sequence-labeling to pull out parameters (flight levels, headings, waypoints).
3. <b>Context Tracking</b>
   * Maintain a simple state dictionary: current altitude, heading, clearance status.
   * Update state after each pilot and generated ATC turn.
4. <b>ATC Response Generation</b>
   * Fine-tune a seq-to-seq model on paired pilot-controller text to generate an ATC-compliant sentence (“Climb and maintain FL200,” “Turn right heading 090.”).

### <u>Literature Review</u>
* "Automatic Speech Recognition for Air Traffic Control" by Balakrishnan et al. 
  * Techniques to transform utterances into instructions and acknowledgments.
* "Contextualizing Air Traffic Management Conversations using Natural Language Understanding" by Rohani et al.
  * Methods for Intent Classification (IC) and Slot Filling (SF) to identify and extract Traffic Management Initiatives (TMIs) from aviation-specific dialogues. 

### <u>Benchmarking</u>
<span style="color:red">
<p>TBD</p>
<p>Proposal had something about a comparative study of classical vs. transformer-based approaches. We need to be flush this out with a description of at least one model of each type with performance results against some common dataset for comparison.</p></span>

### <u>Preliminary Experiments</u>
<span style="color:red">
<p>TBD</p>
<p>We need to identify and execute some simple experiments to satsify this requirement.</p> 
</span>

## Model Implementation

### <u>Framework Selection</u>
<span style="color:red">
<p>TBD</p>
<p>We will need an explanation as to why we selected whichever framework here.</p>
</span>

### <u>Dataset Preparation</u>
<span style="color:red">
<p>TBD</p>
<p>We need a description of the data preprocessing steps for each dataset we use here.</p>
</span>

### <u>Model Development</u>
<span style="color:red">
<p>TBD</p>
<p>We need a diagram and description of the model architecture here.</p>
</span>

### <u>Training & Fine-Tuning</u>
<span style="color:red">
<p>TBD</p>
<p>We will need a description of the training strategy and model hyperparameters here.</p>
</span>

### <u>Evaluation & Metrics</u>
<p>Model performance will be evaluated using the following:</p>

* BLEU Score
* ROUGE Score
* Intent parameter accuracy
* ATC phraseology compliance

## GitHub Repository Setup & Code Management

https://github.com/cneiderer/ie7500_group_project

### Repository Structure
<span style="color:red">
<p>We need to update this structure prior to submission.</p>
</span>

```
ie7500_group_project<br>
├──  .git<br>
├──  .gitignore<br>
├──  LICENSE<br>
├──  README.md<br>
├──  \_\_init\_\_.py<br>
├──  data<br>
│   ├── \_\_init\_\_.py<br>
│   ├── data_loader.py<br>
│   └── data_preprocessor.py<br>
├── documentation<br>
│   ├── documentation.md<br>
├── evaluation<br>
│   ├── \_\_init\_\_.py<br>
│   └── evaluation_metrics.py<br>
├── experiments<br>
│   ├── \_\_init\_\_.py<br>
│   ├── experiment_1.ipynb<br>
│   ├── experiment_2.ipynb<br>
│   └── experiment_3.ipynb<br>
├── models<br>
│   ├── \_\_init\_\_.py<br>
│   ├── model_1.py<br>
│   ├── model_2.py<br>
│   └── model_3.py<br>
├── requirements.txt<br>
└── setup.py<br>
```
