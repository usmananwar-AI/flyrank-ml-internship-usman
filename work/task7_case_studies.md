**Voice Card:** Direct, analytical, data-backed, honest, zero fluff.

---

## Before / After

**Generic AI voice:**
> "Our team leveraged advanced machine learning techniques to significantly improve retention outcomes for our valued users."

**Edited, direct voice:**
> "A static rule — '30 seconds equals drop-off' — ignores search intent entirely. The fix required machine learning: pull historical search telemetry and weigh it dynamically to predict actual churn, not just measure session length."

---

## The Problem

A static rule — "30 seconds equals drop-off" — ignores search intent entirely. A 30-second session is a *success* for a quick-definition query and a *failure* for a complex setup guide, but a fixed threshold scores both the same, generating false alarms. The fix required machine learning: pull historical search telemetry — query frequency, interaction depth, past session behavior — and weigh those variables dynamically to predict actual churn, not just measure session length.

## What I Did

I framed the problem as binary classification with a ranking objective: score each session, then rank by churn risk. The unit of analysis was a single user search session. From the historical search telemetry I engineered features — query frequency, interaction depth — that fed the model. The critical technical decision was the success metric: I optimized for PR-AUC over accuracy, because in proactive retention a false negative — missing a session that's about to churn — is the costliest failure. The pipeline is built to flag high-risk sessions before drop-off, not after.

## The Outcome

The result is a pipeline that shifts content retention from reactive guesswork to proactive data strategy. Optimized for PR-AUC, the model outputs a prioritized ranking of at-risk sessions, built entirely on historical search telemetry. Engineering teams no longer depend on an arbitrary time threshold — they get a ranked intervention list, triggered exactly when drop-off is predicted.

The pipeline is currently architected to optimize for PR-AUC, establishing a data-backed baseline to catch high-risk sessions before full model deployment.

---

## About

I am Usman Anwar Awan, an AI undergraduate at NUML specializing in machine learning, NLP, and computer vision.

**Review this FlyRank case study and reach out: usmananwar7861234@gmail.com**
