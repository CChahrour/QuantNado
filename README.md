# QuantNado

`quantnado-call-peaks` is a quantile-based peak caller for CUT&Tag or ChIP-seq **RPKM-scaled bigWig** files. It tiles the genome, extracts log1p-transformed signal, and outputs BED files of high-signal regions using quantile thresholding. 

`quantnado-make-dataset` generates an `.h5ad` (AnnData) file representing signal matrices across defined regions, using signal from multiple bigWigs.

---

## 📦 Installation

Install from PyPI:

```bash
pip install quantnado
```

Requires Python ≥ 3.8

---

## 🚀 Usage

### 🔹 Call Peaks

```bash
quantnado-call-peaks \
  --bigwig-dir path/to/bigwigs/ \
  --output-dir path/to/output/ \
  --chromsizes path/to/hg38.chrom.sizes \
  --tilesize 128 \
  --quantile 0.98 \
  --merge \
  --blacklist path/to/hg38-blacklist.bed \
  --tmp-dir tmp/
```

This will:
- Tile the genome in 128bp bins
- Extract log1p(RPKM) signal from each bigWig file
- Threshold each track at the 98th percentile (default)
- Output BED files of high-signal bins per track

---

### 🔹 Make Dataset

```bash
quantnado-make-dataset \
  --bigwig-dir path/to/bigwigs/ \
  --output-file dataset.h5ad \
  --chromsizes path/to/hg38.chrom.sizes \
  --regions promoters_1024bp.bed \
  --blacklist hg38-blacklist.bed
```

If `--regions` is not provided, the genome will be tiled using `--binsize` (default: 128 bp).

The resulting `.h5ad` will contain:
- `.X`: matrix of mean RPKM per region per bigwig
- `.obs`: region metadata (chrom, start, end)
- `.var`: track names (from bigWig filenames)

---

## ⚙️ Options

| Option          | Description                                               |
|-----------------|-----------------------------------------------------------|
| `--bigwig-dir`  | Folder with `.bw` or `.bigWig` files                      |
| `--chromsizes`  | Tab-separated chrom.sizes file (`chr\tlength`)            |
| `--blacklist`   | BED file of regions to exclude                            |
| `--tilesize`    | Genomic bin size for peak calling (default: 128 bp)       |
| `--quantile`    | Quantile threshold for peaks (default: 0.98)              |
| `--merge`       | Merge adjacent bins into broader peaks                    |
| `--regions`     | BED file of regions to extract signal over                |
| `--binsize`     | Bin size used if `--regions` is not specified (default: 128) |
| `--output-file` | Path to save `.h5ad` or `.h5mu`                           |

---

## 📁 Output Files

**Peak caller (`quantnado-call-peaks`)**
- `*.bed`: BED files of peaks for each bigwig

**Dataset maker (`quantnado-make-dataset`)**
- `.h5ad`: AnnData file with signal matrix
