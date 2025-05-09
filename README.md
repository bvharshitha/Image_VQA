# 🖼️ Image_VQA

## 🎯 Project Overview

**Assistive Visual Question Answering for Visually Impaired Users**

This project develops an Assistive Visual Question Answering (VQA) system aimed at empowering visually impaired individuals. By answering natural language questions about images, the system helps users gain contextual understanding of their environment. 

We explore two core approaches using the **VizWiz** dataset:
- **Classification-based VQA** (PaliGemma-2 inspired by "Less Is More")
- **Generation-based VQA** (CLIP-ViT-GPT2, SigLIP-GPT2, ViT-GPT2)

The ultimate goal is to enhance **accessibility** and **independence** through robust and efficient AI-driven image understanding.

---

## 📦 Dataset: VizWiz

- Contains ~31,000 image-question pairs collected from **visually impaired users**.
- Real-world images are often **noisy** (blurry, under/over-exposed).
- **Diverse questions** with an average length of **7.14 words**.
- Large answer space with **6503 unique answers**, including **4843 marked "unanswerable"**.

### Preprocessing

- Combined official `train` and `val` sets (~25k samples).
- Performed an **80-10-10 split**: train / validation / test.
- Selected **top-1 answer** for each question (from 10 annotator responses).
- Final format includes:  



## 🧠 Visual Question Answering (VQA) Approaches

### 1. Classification-Based VQA (PaliGemma-2 + Classifier)

Inspired by the "Less Is More" paper, this approach focuses on **efficiency**:

- **Feature Extraction**: Extracted features using PaliGemma-2’s last hidden state on the VizWiz dataset to capture robust features from image-question pairs.
- **Classifier Training**: Trained a lightweight classifier (VQAClassifier: `512 → 256 → 6503`) on the extracted features.
- **Inference**:  
  `New image-question pair → Extract features → Predict answer with classifier`
- **Advantages**:  
  ✅ Efficient and reliable predictions within a fixed vocabulary.  
- **Challenges**:  
  ⚠️ Limited to 6503 predefined answers; cannot handle open-ended responses.

---

### 2. Generation-Based VQA (CLIP-ViT-GPT2, SigLIP-GPT2, ViT-GPT2)

This approach explores **generative** VQA for greater flexibility:

- **Models**:
  - `CLIP-ViT-GPT2`: CLIP’s Vision Transformer + GPT-2
  - `SigLIP-GPT2`: SigLIP (improved CLIP) + GPT-2
  - `ViT-GPT2`: Vision Transformer + GPT-2
- **Pipeline**:  
  `Image → Vision Encoder → Embedding → Concatenate with question → GPT-2 generates answer`
- **Training**:  
  Fine-tuned each model end-to-end on VizWiz dataset.
- **Advantages**:  
  ✅ Can generate open-ended answers beyond a fixed vocabulary.
- **Challenges**:  
  ⚠️ Resource-intensive and struggles with “unanswerable” questions.

## 📊 Results

### Classification-Based (PaliGemma-2 + Classifier)
- Test accuracy: ~71% (VizWiz is challenging due to noisy data).
- Reliable for fixed answers, especially "unanswerable" cases.

### Generation-Based (CLIP-ViT/SigLIP/ViT + GPT-2)
- Test (semantic) accuracy: ~56% (struggles with precise answers on noisy data).
- More flexible but less accurate, especially for "unanswerable" cases.

### Comparison
- Classification is more efficient and reliable for VizWiz.
- Generation offers flexibility but requires more resources and fine-tuning.

---

## 🚀 Future Work

- Develop a hybrid approach: Classify "unanswerable" first, then generate if answerable.
- Expand vocabulary: Use larger generative models (e.g., GPT-3) for broader answer coverage.
