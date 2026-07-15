# README

## 1. Setup

To ensure that the `pytest -q` command successfully discovered the `src` module without raising `ModuleNotFoundError` or `ImportError`, I created an empty `conftest.py` file in the project root. This automatically added the project root to the Python path during test collection.

---

## 2. Test Run

See **`runtestscreen.png`** for the successful test execution.

---

## 3. Output of Toy Experiment

```text
(.venv) (anlp3) neferpitou@MacBookAir anlp_hw3_q3_dpo % python run_experiment.py --data data/toy_preferences.jsonl --beta 0.5
Loaded 12 preference pairs
beta: 0.5
mean DPO loss: 0.6371
preference accuracy from DPO logits: 0.583

Per-example diagnostics:
ex01: logit= 0.350, loss= 0.533, prompt=Explain why RLHF can be unstable.
ex02: logit= 0.250, loss= 0.576, prompt=What is Direct Preference Optimization?
ex03: logit=-0.050, loss= 0.718, prompt=Summarize the main advantage of pairwise preference training.
ex04: logit= 0.400, loss= 0.513, prompt=Why keep a reference policy in DPO?
ex05: logit= 0.300, loss= 0.554, prompt=What does beta control in DPO?
ex06: logit=-0.075, loss= 0.731, prompt=Define a chosen response in preference data.
ex07: logit= 0.300, loss= 0.554, prompt=Explain why scalar reward RL can be difficult for language models.
ex08: logit=-0.150, loss= 0.771, prompt=What is a rejected response?
ex09: logit= 0.350, loss= 0.533, prompt=Why might DPO be easier to implement than PPO-based RLHF?
ex10: logit=-0.125, loss= 0.758, prompt=What is the intuition behind the DPO objective?
ex11: logit=-0.250, loss= 0.826, prompt=What does a negative DPO margin suggest?
ex12: logit= 0.250, loss= 0.576, prompt=How does DPO use preference pairs?
(.venv) (anlp3) neferpitou@MacBookAir anlp_hw3_q3_dpo %
```

---

## 4. Answers

### What does a positive DPO logit mean?

A positive DPO logit means that the current policy prefers the chosen response over the rejected response more strongly than the reference policy does. This indicates that the model already agrees with the preference pair for that example.

### Why does DPO compare the current policy against a reference policy?

The reference policy provides a baseline so that the current policy is only rewarded for improving relative to the original model. This helps prevent the policy from drifting too far while still learning from preference data.

### What does the beta parameter control?

The beta parameter controls how strongly differences between the current policy and the reference policy affect the DPO objective. Larger beta values make the optimization more sensitive to preference differences, while smaller values produce more conservative updates.

### Pick one example with a low loss and one example with a high loss. Explain the difference.

- **Low loss:** **ex04** has a DPO logit of **0.400** and a loss of **0.513**. The positive logit indicates that the current policy already prefers the chosen response more than the reference policy, so the objective is largely satisfied.

- **High loss:** **ex11** has a DPO logit of **-0.250** and a loss of **0.826**. The negative logit indicates that the current policy still favors the rejected response too much relative to the chosen response. As a result, this example receives a higher loss because it would require a stronger update during training.
