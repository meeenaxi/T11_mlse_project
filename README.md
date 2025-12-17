# Aircraft-Centric Multimodal Retrieval-Augmented Generation System

Project Members
202418050 Shahswat Sharma
202418031 Meenakshi Iyer

### 1. Project Overview

This project implements an end-to-end multimodal Retrieval-Augmented Generation (RAG) pipeline tailored specifically for military aircraft recognition and description. The system integrates and retrieval-based grounding to support two tightly coupled generative tasks:

Image-to-Text Generation
Automatically generating detailed, technically accurate textual descriptions from aircraft images.

Text-to-Image Generation
Generating aircraft imagery conditioned on structured textual prompts describing aircraft characteristics.

The core motivation is to move beyond generic vision–language models and build a domain-constrained system that understands aircraft morphology, configuration, and distinguishing visual features.

### 2. Dataset Description
2.1 Aircraft Image Dataset

The project is built around a military aircraft image dataset, which contains labeled images of multiple military aircraft classes.

Key characteristics of the image dataset:

Multiple aircraft categories (fighters, bombers, transports, UAVs). Variability in viewpoints, lighting, and backgrounds. Strong inter-class visual similarity, making the task non-trivial. These images form the visual modality of the system and are used for both retrieval and generation tasks.

2.2 Aircraft Knowledge Corpus

The file aircraft_descriptions.json serves as the authoritative textual knowledge base for the RAG pipeline aircraft_descriptions
Each entry:
Maps an aircraft identifier (e.g., F-16, B-2, SR-71)
To a highly structured, technical description covering:
Engine configuration and placement
Wing geometry and sweep
Tail and stabilizer design
Fuselage shape and distinctive features
Landing gear configuration
This dataset is not merely descriptive; it functions as the retrieval corpus that grounds all generated outputs.

### 3. Why Retrieval-Augmented Generation (RAG)

Pure generative models often:

Hallucinate aircraft features, Confuse visually similar aircraft types and Produce vague or non-technical descriptions

RAG is used to:
Retrieve relevant aircraft-specific knowledge. Inject it into the generation process and ensure outputs remain factually consistent and domain-aligned


### 4. Detailed Code and Pipeline Explanation
4.1 Data Loading and Preparation

Aircraft images are loaded from the image dataset directory.
Aircraft descriptions are loaded from aircraft_descriptions.json.
Each aircraft class is treated as a semantic unit linking images and textual descriptions.
This alignment enables cross-modal retrieval.

4.2 Embedding Generation

Two embedding pathways are established: 

Text Embeddings

Aircraft descriptions are converted into dense vector representations.

These embeddings capture semantic and technical aircraft features, not just surface-level text similarity.

Image Embeddings

Aircraft images are encoded into visual feature vectors.

The embedding space is designed to be compatible with text embeddings for retrieval.

4.3 Vector Store Construction

All embeddings (image and text) are stored in a vector database.

Each vector entry contains:

Aircraft identifier

Modality type (image or text)

Associated metadata

This enables:

Image → Text retrieval

Text → Image retrieval

Image → Image similarity search

### 5. Retrieval Phase

When a query is issued:

Image query
Retrieves the most relevant aircraft descriptions based on visual similarity.

Text query
Retrieves aircraft images and descriptions matching the semantic content of the prompt.

Retrieval acts as a filtering and grounding step, ensuring only relevant aircraft knowledge reaches the generation stage.

### 6. Generation Phase
6.1 Image-to-Text Generation

Retrieved aircraft descriptions are injected as context.

The model generates structured, technical captions describing:

Aircraft configuration

Visual attributes

Distinguishing features

This avoids generic captions and enforces domain specificity.

6.2 Text-to-Image Generation

Text prompts describing aircraft features are enriched using retrieved knowledge.

Generation is conditioned on both the prompt and retrieved aircraft context.

This reduces visual drift and improves aircraft-class fidelity.

### 7. Role of updated_rag.py

The Python file:

Implements the complete RAG workflow

Mirrors the logic of the original notebook

Encapsulates:

Data loading

Embedding computation

Retrieval logic

Context injection

Generation execution

This file enables reproducibility and deployment readiness beyond a notebook environment 

updated_rag



### 9. Limitations

Performance depends on dataset coverage and class balance

Extremely rare aircraft types may be underrepresented

Fine-grained subtype differentiation can be further improved

### 10. Future Work

Aircraft subtype and variant classification

Attribute-level grounding (engine count, wing sweep angle)

API deployment for real-time inference

Quantitative evaluation using multimodal alignment metrics

This project demonstrates how Retrieval-Augmented Generation can be effectively applied to a highly specialized multimodal domain such as military aviation. By grounding both image and text generation in a curated aircraft knowledge base, the system achieves higher accuracy, interpretability, and reliability than generic generative approaches.
