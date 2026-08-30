# DA3408 AI Operations — Module 1 Assignment

Prabhav Gupta (DA24B018)

This repo has my work for the Module 1 assignment. All four questions are here.

The 1 page report covering all four questions is in the main branch of this repo.

## Where each answer is

- **Q1** — in the report only. It is a written question so there is no code for it.
- **Q2** — in `Question 2/` (notebook and the run comparison screenshot). The analysis is in the report.
- **Q3** — in `Question 3/` (terminal screenshots).
- **Q4** — in `Question 4/` (screenshots). The actual work is in Partner A's repo, linked below.

---

## Question 2 — MLflow

Folder: `Question 2/`

- `Assignment1_Q2.ipynb` — the notebook. It loads MNIST, trains an MLP six times with different learning rates and batch sizes, and logs everything to MLflow. The `mlflow.log_param` and `mlflow.log_metric` lines I added are in here.
- `q2_run_table.png` — the comparison table of all six runs from the MLflow UI.

I used learning rates 0.001 and 0.01 with batch sizes 32, 64 and 128. The best run was `mlp-lr-0.001-batch-32` with validation accuracy 0.9775. The written analysis is in the report.

The runs are in a local MLflow database on my VM, so they are not committed here. The comparison screenshot is the evidence for them.

## Question 3 — DVC

Folder: `Question 3/`

What I did:

1. Made a CSV of the file names from `data.zip`. It came to 1800 rows plus a header.
2. Ran `dvc init` and `dvc add filenames.csv`.
3. Set up an SSH remote at `ssh://pg@localhost/home/pg/dvc_storage` and ran `dvc push`.
4. Committed the `.dvc` file and tagged it v1.
5. Added the rows from `new-labels.zip`, which made it 2801 lines, ran `dvc add` and `dvc push` again, and committed that as v2.
6. Rolled back with `git checkout v1` and then `dvc checkout`. After that `wc -l` gave 1801 lines, which is the v1 count.

The commands:

```bash
wc -l filenames.csv   # 1801
dvc init
dvc add filenames.csv
dvc remote add --default ssh_remote ssh://pg@localhost/home/pg/dvc_storage
dvc push
git add filenames.csv.dvc .dvc/config
git commit -m "Add v1 dataset"
git tag v1

# after adding the new rows
wc -l filenames.csv   # 2801
dvc add filenames.csv
dvc push
git commit -am "Update dataset to v2"
git tag v2

# rollback
git checkout v1
dvc checkout
wc -l filenames.csv   # 1801
```

The screenshots:

- `q3_1.png` — the remote config and the git log showing the v1 and v2 tags
- `q3_2.png` — at v2, 2801 lines, dvc status clean
- `q3_3.png` — the rollback, going from 2801 down to 1801. This is the proof for part 3.
- `q3_4.png` — the push to the SSH remote

The DVC work was done in its own Git repo on my VM, so only the screenshots are here. The dataset is not committed because keeping big data out of Git is the point of using DVC in the first place.

## Question 4 — Reproducibility drill

Folder: `Question 4/`

I was Partner B. My partner is Rinkesh (Partner A). Since both partners have to commit to the same repo, all the Q4 work is in his repo on my own branch:

**https://github.com/Rinkesh-1612/DA3408-Assignment1-Q4**

My branch is `partner-b-reproduction` and my commits are under my account (shuriken17). `REPRODUCTION.md` is there too.

I cloned his repo, checked out commit `ff5e88c`, built the environment with `mamba env create -f environment.yml`, ran `dvc checkout` to get the dataset back, and reran `src/q4_train_and_register.py` without changing anything.

I got `val_accuracy = 0.9639` and `val_f1_macro = 0.9635`. Partner A got exactly the same, so the difference was 0.0000 and it matched well within the 0.001 tolerance I was checking against. This makes sense as all the settings were same.
The run is on the shared MLflow server as `q4-partner-b-run` and is registered as `q4-digits-classifier` version 2 in Staging. I also logged the reproduction note on the run itself.

- `q4_1.png` — the training output with the metrics, the git_commit tag, and the model being registered and moved to Staging
- `q4_2.png` — the MLflow run page showing the note, metrics and registered model

---

## AI usage

Tool used: Claude

- **Q1** — no AI use.
- **Q2** — used it for writing the code and getting the pipeline running.
- **Q3** — used it to set up the report section for this question, to fix errors I hit while unzipping the data files, and to work out the commands for taking the terminal screenshots after I had finished the question.
- **Q4** — used it for errors while cloning the repo, for package installation problems, and for Linux commands during setup.
- **Report** — used it to make the writing more concise.

Overall I mostly used it to debug errors and troubleshoot things that were not working. I also used it for this README.md file.

---

The repo was private until the deadline and made public after, as asked.
