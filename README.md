
## 1. Navigate to Your Ocean Folder

```
myocean
```

## 2. Clone Your Final Project Repository

```bash
git clone <repo-url>
```

## 3. Enter the Cloned Repository

```bash
cd <cloned-repository>
```

---

## 4. Create `srr_accessions.txt`

> You may already have this file under another name.

```bash
vi srr_accessions.txt
```

- Press `I` to enter **Insert mode**
- Paste your SRR accession list (one per line):

```
SRR26406912
SRR8861588
SRR8861589
SRR8861590
SRR8861591
SRR26406885
SRR26406898
SRR26406903
SRR26406904
SRR26406890
SRR26406892
```

- Press `ESC`, then type `:wq` and press Enter to save and quit

### If your file has a different name:

```
mv AccESSIOns_list_update.txt srr_accessions.txt
```

> Replace `AccESSIOns_list_update.txt` with your file name.

---

## 5. Install SRA Tools via Conda

```bash
module load anaconda3
conda create -n sra-tools -c bioconda -c conda-forge sra-tools
```

---

## 6. Exit and Reconnect to HPC

```bash
exit
```

- Type `exit` again as needed to fully log out
- Reconnect and `cd` back into your project folder

---

## 7. Create Batch Download Script

```bash
vi download.sh
```

- Press `I` and paste:

```bash
#!/bin/bash
module load anaconda3
conda activate sra-tools

while read -r SRR; do
  echo "Downloading $SRR..."
  prefetch --max-size 100G $SRR
  fasterq-dump $SRR --split-files -O fastq_files/
done < srr_accessions.txt

conda deactivate
```

- Press `ESC`, then `:wq` to save and quit

---

## 8. Run the Download Script

```bash
bash download.sh
```

> Output FASTQ files will be saved to the `fastq_files/` folder.

- If working with paired-end reads, ensure you see two files per SRR ID.

---
