
# Kun: Answer Polishment for Chinese Self-Alignment with Instruction Back-Translation

## Introduction

Inspired by Meta's mid-year paper "Self-Alignment with Instruction Backtranslation," our project embarks on an innovative approach to enhance language model training using a novel data augmentation paradigm. This method, rooted in the principles of self-alignment, involves a meticulous process of selecting, refining, and employing high-quality [instructional data] to fine-tune language models.

## Methodology

Our approach closely follows the self-alignment method described by Meta, with adaptations to optimize the process:

1. **Seed Data Selection and Model Training**: Initially, appropriate seed data are selected and inverted to train a Label Model on a base model(Yi Base). Concurrently, using the same seed data, a Primary Chat model is trained following the Supervised Fine-Tuning (SFT) method typical of chat models.

3. **Labeling Unlabeled Data**: The Label Model is then used to annotate preliminarily cleansed Primary data. Cleansing involves filtering based on perplexity (ppl) and length, discarding data exceeding 512 tokens.

4. **Instruction Data Generation**: Post-annotation, we obtain our first version of Labeled data. Unlike the original project where both instruction and output data pairs are fed into Primary Chat Model for scoring, our replication revealed limitations in Primary Chat's ability to discern high-quality instructions. We innovated by scoring only the instruction component, effectively filtering out noise and selecting high-quality instructions.

5. **Output Data Refinement**: Upon manual inspection, we identified a mismatch between the Primary Data (used as output) and the standard requirements for output in instruction data. To address this, we introduced an additional step: refining the output data. Using Primary Chat's capabilities, the output (originally unlabeled data) is adjusted according to the instructions, making it more suitable as output for the instruction data.

6. **Framework Completion**: Our methodology concludes with the acquisition of a substantial volume of instructional data, achieved with minimal resource expenditure.


## Usage Instructions

### Usage
After cloning the repository, follow these steps to use the framework:

1. **Navigate to the Project Directory**:
   Change to the directory where the repository has been cloned:
   ```
   cd COIG-Kun
   ```

2. **Move to the Scripts Directory**:
   Change to the scripts directory within the project folder:
   ```
   cd scripts
   ```

3. **Execute the Pipeline Script**:
   Run the pipeline script with the required parameters:
   ```
   sh pipline.sh label_model_path point_model_path answer_model_path data_path output_path
   ```

   - `label_model_path`: Path to the instruction generation model.
   - `point_model_path`: Path to the scoring model.
   - `answer_model_path`: Path to the response correction model.
   - `data_path`: Directory where the raw, unlabeled data is stored.
   - `output_path`: Directory where the generated data will be stored.

Make sure that each path is correctly specified and points to the respective model or data directory in your file system.

## Training Process

This section provides a detailed explanation of the training process for the Label Model, Answer Model, and Point Model from a base model, using high-quality seed data.

### Label Model Training
- **Huggingface URL** : Anonymity requirement, temporarily not released
- **Data Preparation**: Utilize high-quality seed instructions. The responses to these instructions are used as outputs, and the instructions themselves as inputs.
- **Training Parameters**:
  - Learning Rate: 1e-5

### Answer Model / Point Model Training
- **Data Preparation**: Employ the same set of ten thousand seed instruction data as used for the Label Model.
- **Model Configuration**: Positive fine-tuning of the Yi-34B base model is performed with the seed instruction data.
- **Training Parameters**:
  - Epochs: Four
  - Learning Rate: 1e-5

**Note**: The seed data comprises high-quality instructional data, meticulously curated to ensure the effectiveness of the training process.

## Implementation in Scripts
The training process for each model is embedded within specific scripts in the server's repository. Users are encouraged to adjust the training parameters based on their specific requirements and computational resources.

## Results

Our project has successfully generated high-quality instructional data through a novel, cost-effective approach that eschews traditional distillation methods. By refining our data selection and processing techniques, we have produced a large volume of valuable instructional data. This approach focuses on enhancing the quality and applicability of the data, which is essential for improving the performance of language models. Our method demonstrates the potential to significantly advance the field of language model training by providing superior instructional data with minimal resource expenditure.

## Contributions

This project contributes to the field of language model training by:

- Proposing a novel approach to generate and refine instructional data.
- Demonstrating the effectiveness of selective scoring on instruction data for quality enhancement.
- Offering a resource-efficient methodology for data augmentation.
- Provision of a directly usable Chinese instruction generation model.

## Future Work

We plan to explore further refinements in data selection and scoring methods, aiming to enhance the quality of the generated instructional data even more. Additionally, adapting this methodology to other language models and contexts remains an area of interest.
