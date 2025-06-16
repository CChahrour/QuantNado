# QuantNado

**QuantNado** is a quantile-based peak caller for CUT&Tag RPKM-scaled bigWig files. It tiles the genome, extracts log1p-transformed signal, and outputs BED files of high-signal regions using quantile thresholding.

---

## 📦 Installation

Install from source:

```bash
git clone https://github.com/YOUR_USERNAME/QuantNado.git
cd QuantNado
pip install -e .
```
Or:

```bash
pip install quantnado

```

## 🚀 Usage

```bash
QuantNado \
  --bigwig path/to/file.bw \
  --output-dir output/ \
  --chromsizes path/to/hg38.chrom.sizes \
  # Optional parameters:
  --blacklist path/to/hg38-blacklist.bed \
  --tilesize 128 \
  --quantile 0.98 \
  --min-peak-length 128 \
  --tmp-dir $TMPDIR
```