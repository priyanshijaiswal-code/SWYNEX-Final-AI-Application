# Scam Message Classifier — Final AI Application

**Internship:** SWYNEX Technologies — Artificial Intelligence Internship
**Task:** Task 4 — Final AI Application (Problem, Method, Demo, Limitations, Ethics)

This repository packages the full scam-message classification project built across all four internship tasks into one beginner-friendly application.

---

## 1. Problem

Classify incoming SMS/text messages as **scam** or **legitimate**, to help users identify phishing and fraud attempts before they act on them.

- **User:** A general smartphone user receiving unsolicited SMS messages — particularly ones with links, urgency cues ("your account will be blocked"), or requests for personal/financial info.
- **Use case type:** Binary classification (scam vs. legitimate)
- **Why it matters:** Scam/phishing messages are a widespread, low-effort attack vector; a lightweight, explainable classifier can act as a first line of defense before a user clicks a malicious link or shares sensitive information.

## 2. Method

- **Model:** TF-IDF vectorization (unigrams + bigrams) + Logistic Regression (scikit-learn).
- **Why this approach:** With a small training dataset, a simple, interpretable model generalizes better than a deep learning approach, and its coefficients make explainability straightforward — each prediction can show exactly which words drove the result.
- **Explainability:** For every prediction, the system surfaces the specific words in the message that most influenced the model's decision (based on trained coefficient weights), rather than returning a black-box score.
- **Error handling:** Input is validated before classification — empty messages, non-string input, overly short messages, and messages with no recognized vocabulary are all handled explicitly rather than producing a silent or falsely confident result.

## 3. Demo

An interactive command-line interface lets you classify messages live:
```bash
python intelligent_feature.py
```
```
Enter a message: Your account will be suspended, click here to verify now
  -> Prediction: SCAM (risk: Medium, confidence: 0.58)
  -> Flagged words: ['click', 'verify', 'suspended', 'account']
```
Type `quit` to exit the demo.

## 4. Limitations

This is a prototype trained on a small (~20-example) labeled dataset, so it has real, verified limitations:

- **False positives:** Legitimate messages containing words like "congratulations" or "important update" can be misclassified as scam, since the small training set over-associates those words with fraud.
- **False negatives:** Scam messages that avoid specific trigger words (e.g. "click", "verify", "account") — such as softer-toned promotional scams — can slip through as legitimate. This is the more concerning failure mode for a real-world scam detector.
- **Keyword dependence:** With limited training data, the model leans on surface-level keyword patterns rather than deeper semantic or contextual understanding of a message.
- **Not production-ready:** This is a learning prototype. A real deployment would need a much larger, more diverse, and regularly updated training dataset, plus ongoing evaluation against new scam tactics as they emerge.

## 5. Ethics Notes

- **False positives have a real cost.** Wrongly flagging a legitimate message (e.g. from a bank, employer, or family member) as a scam can cause users to ignore or distrust important communications. Precision on the "legitimate" class matters as much as scam recall.
- **False negatives have a real cost too.** A missed scam can lead to direct financial or privacy harm. Neither failure mode is "safe" — tradeoffs between them should be made deliberately, not by accident of training data.
- **No personal data is stored or transmitted.** This prototype classifies text locally; it does not log, store, or send user messages anywhere, and uses no external API that would expose message content to a third party.
- **Bias risk from a small, non-diverse dataset.** The training examples used here are illustrative, not representative of the full range of real-world scam tactics (which vary by language, region, and target demographic). A model trained this way could underperform — or systematically miss certain scam styles — for populations not reflected in its training data.
- **Not a substitute for user judgment.** This tool is meant to support, not replace, a user's own caution. It should be presented as a risk signal, not an infallible verdict, especially given the documented false-negative risk above.

## Files
- `scam_classifier.py` — base model: data, training, evaluation (Task 2)
- `intelligent_feature.py` — error handling, failure case report, CLI demo (Task 3)
- `README.md` — this file: full project write-up (Task 4)

**Note:** `intelligent_feature.py` imports directly from `scam_classifier.py`, so both files must be downloaded and kept in the same folder — running `intelligent_feature.py` on its own, without `scam_classifier.py` alongside it, will raise an import error.

## How to Run
```bash
pip install scikit-learn
python intelligent_feature.py
```
